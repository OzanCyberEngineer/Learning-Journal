# 🚨 VLAN & VXLAN Güvenlik Boyutu: Saldırılar ve Zafiyetler

Ağ bölümlendirme sınırlarının siber güvenlik boyutunu ele alan; saldırganların fiziksel ve sanal sınırları aşarak yan ağlara sızma (Lateral Movement) yöntemlerini inceleyen teknik döküman.

---

## 🎯 1. VLAN Hopping (VLAN Atlama) Saldırıları

Güvenlik duvarlarını ağ seviyesinde kurduğumuz için, bir siber saldırganın en büyük hayali bu VLAN veya VXLAN sınırlarını aşarak yan ağlara sızmaktır (**Lateral Movement**). Bu saldırı türü, saldırganın fiziksel olarak bağlı olmadığı başka bir departmanın VLAN trafiğine sızmasını hedefler ve iki popüler yöntemle gerçekleştirilir:

### A) Switch Spoofing (Switch Taklidi)
* **Saldırı Mantığı:** Normalde switch portları ya uç cihazlara bağlıdır (**Access Port**) ya da diğer switch'lere bağlıdır (**Trunk Port**). Switch'ler kendi aralarında hangi portun trunk olacağını **DTP (Dynamic Trunking Protocol)** ile otomatik olarak anlaşır. Saldırgan, switch portuna bağlı bilgisayarından sahte DTP paketleri göndererek switch'e *"Ben de bir switch'im, aramıza trunk hat kuralım"* der. Switch buna inanırsa ilgili portu trunk moduna alır. Artık saldırganın bilgisayarına ağdaki tüm VLAN'ların trafiği akmaya başlar.
* **Zayıflık:** Varsayılan ayarlarda switch portlarının dinamik anlaşmaya (DTP) açık bırakılması.
* **Çözüm (Hardening):** Kullanıcıların bağlanacağı tüm portlar manuel olarak `switchport mode access` olarak set edilmeli ve DTP protokolü `switchport nonegotiate` komutuyla kesinlikle kapatılmalıdır.

### B) Double Tagging (Çift Etiketleme)
* **Saldırı Mantığı:** Saldırgan, gönderdiği paketin üzerine üst üste iki tane VLAN etiketi koyar. Örneğin; **Dış Etiket:** `VLAN 10` (Saldırganın kendi VLAN'ı), **İç Etiket:** `VLAN 20` (Kurbanın VLAN'ı). Paket ilk switch'e ulaştığında, switch dıştaki `VLAN 10` etiketini söker ve paketi trunk porttan diğer switch'e iletir. Ancak paketin içinde hala `VLAN 20` etiketi durmaktadır! İkinci switch paketi aldığında içteki etikete bakar ve doğrudan `VLAN 20`'deki kurbana iletir. Böylece saldırgan tek yönlü olarak kurbanın VLAN'ına paket göndermeyi başarmış olur.
* **Zayıflık:** Switch'lerin "Native VLAN" (etiketsiz gelen paketleri kabul ettiği varsayılan VLAN) değerinin, aktif kullanıcıların bulunduğu VLAN (genellikle `VLAN 1`) ile aynı bırakılması.
* **Çözüm (Hardening):** Native VLAN değeri, kullanıcıların asla bulunmadığı, kullanılmayan bir dummy ID'ye (Örn: `VLAN 999`) atanmalıdır.

---

## 🦠 2. VXLAN Zafiyetleri ve Tünel Sömürüsü

VXLAN tamamen yazılımsal ve dinamik olduğu için geleneksel donanımsal switch güvenlik önlemleri burada yetersiz kalır.

* **Şifresiz Tünel Trafiği:** Standart VXLAN tünel trafiği (**UDP 4789**) tamamen şifresizdir. Eğer bir saldırgan fiziksel sunucuların bağlı olduğu omurga ağa (**Underlay**) sızıp paket dinleme (sniffing) yaparsa, o bulut altyapısında veya Kubernetes cluster'ında dönen tüm Pod trafiğini, şifreleri ve gizli verileri açıkça okuyabilir.
* **Rogue VTEP (Sahte Tünel Noktası):** Fiziksel ağa sızan bir saldırgan, kendi bilgisayarını sahte bir VTEP gibi yapılandırıp ağa sahte VXLAN paketleri enjekte edebilir. Bu sayede Kubernetes cluster'ının içine hiç yetkisi olmadığı halde dışarıdan paket sokmayı başarabilir.
