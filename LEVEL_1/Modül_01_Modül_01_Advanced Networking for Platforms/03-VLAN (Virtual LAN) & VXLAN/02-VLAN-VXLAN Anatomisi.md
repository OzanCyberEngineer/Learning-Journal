# ⚙️ 2. VLAN (Virtual LAN) ve 802.1Q Teknolojisinin Anatomisi

Fiziksel bir switch'i yazılımsal olarak birden fazla mantıksal switch'e bölme işlemine **VLAN** diyoruz. Bir switch'in üzerindeki portlar iki ana kategoriye ayrılır:

* **Access Port:** Sadece tek bir VLAN'a üye olan, bilgisayarların, sunucuların veya yazıcıların bağlandığı porttur. Bu porta bağlı cihazlar paketlerinde etiket (tag) taşımazlar, normal ethernet paketi gönderirler.
* **Trunk Port:** Birden fazla VLAN trafiğini üzerindeki tek bir fiziksel kablodan başka bir switch'e veya router'a taşıyan liman kapısıdır.



---

### 🏷️ IEEE 802.1Q (VLAN Tagging / Etiketleme)

Bir paket, bir Access porttan girip Trunk porttan çıkarken switch, orijinal ethernet çerçevesinin (Frame) ortasına **IEEE 802.1Q** standardına göre **4-byte'lık bir "Tag" (Etiket)** yerleştirir. 



Bu etiketin en önemli kısmı **12-bit'lik VLAN ID (VLAN Kimliği)** alanıdır. Karşı taraftaki switch bu etikete bakarak paketin hangi departmana/VLAN'a ait olduğunu anlar, etiketi söker ve paketi ilgili Access porttan hedefe ulaştırır.

---

### ⚠️ Klasik VLAN'ın Sınırları ve Bulut Dünyasındaki Çöküşü

VLAN geleneksel ağlar için harika bir teknolojidir; ancak AWS, Azure gibi devasa veri merkezlerinde ve modern Kubernetes cluster'larında iki büyük duvara toslamıştır:

1. **4096 Sınırı (Ölçekleme Problemi):** VLAN ID alanı 12-bit olduğu için matematiksel olarak en fazla $2^{12} = 4096$ farklı VLAN oluşturulabilir. Çok kiracılı (Multi-tenant) ve milyonlarca müşterisi olan bir genel bulut (Public Cloud) sağlayıcı için bu sınır komik derecede küçüktür.
2. **Fiziksel Switch Bağımlılığı (Hantallık):** Bir VLAN'ı genişletmek veya iki farklı sunucu arasında konuşturmak için, veri iletim yolundaki tüm fiziksel router ve switch'lerde o VLAN ID'sinin el ile (veya otomatik ağ araçlarıyla) tek tek tanımlanması gerekir. Bulut dünyasının esnekliğine, yani saniyede binlerce sanal makine (VM) ve pod açılıp kapanmasına bu hantal fiziksel yöntem ayak uyduramaz.
---
# 🚀 3. VXLAN (Virtual Extensible LAN) ve Overlay Ağlar

Bulut ve Kubernetes dünyasının ağ altyapısını kurtaran kahraman **VXLAN**'dir. VXLAN, Katman 2 (Ethernet) paketlerini Katman 3 (IP) paketlerinin içine sararak (**encapsulation**) taşıyan bir tünelleme (**Overlay**) teknolojisidir.

---

### 🌟 VXLAN'in Getirdiği Devrimler

* **16 Milyon Ağ İzolasyonu:** VXLAN, etiketleme için **24-bit'lik VNI (VXLAN Network Identifier)** alanını kullanır. Bu sayede $2^{24} = 16.777.216$ (yaklaşık 16 milyon) adet izole sanal ağ oluşturulabilir!
* **Fiziksel Altyapı Bağımsızlığı:** Fiziksel ağdaki switch'lerin üzerinde binlerce VLAN tanımlama zorunluluğunu ortadan kaldırır. Fiziksel switch'ler sadece standart IP paketlerini taşır.

---

### 🏗️ Underlay vs. Overlay Ayrımı



* **Underlay (Fiziksel Altyapı):** Yerlerdeki kablolar, fiziksel switch'ler ve router'lar. Tek görevleri IP paketlerini en hızlı şekilde A noktasından B noktasına taşımaktır. İçerideki sanal ağlardan veya hangi Pod'un hangi Pod ile konuştuğundan haberleri bile yoktur.
* **Overlay (Sanal Ağ):** Fiziksel altyapının üzerinde koşan, sanal tünellerle birbirine bağlanmış yazılımsal sanal ağlar. (Kubernetes'teki **Calico**, **Flannel** veya **Cilium** gibi CNI'lar tam olarak bu overlay katmanını inşa eder).

---

### 🔌 VTEP (VXLAN Tunnel End Point)

Tünelin giriş ve çıkış kapılarıdır. Paketleme ve paket açma (**encapsulation / decapsulation**) işlemlerini bu birim yapar. 



* Bir sanal makine veya Kubernetes Pod'u paket gönderdiğinde, yerel **VTEP** bu L2 paketini alır, standart bir **UDP (Port 4789)** paketi içine sarar (encapsulate eder) ve karşıdaki VTEP'e gönderir.
* Karşı taraftaki VTEP paketi alır, dışarıdaki UDP zarfını soyar (decapsulate eder) ve içerideki orijinal L2 ethernet paketini hedef pod/makineye teslim eder. VTEP bir yazılım (**Linux Kernel**) veya donanım (**akıllı switch**) olabilir.
