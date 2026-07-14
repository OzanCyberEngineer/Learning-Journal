# Öğrenim Raporu: DNS Mimarisi, Yönlendirme Saldırıları ve DNS Güvenlik Protokolleri

Bu rapor; Katman 7 (Uygulama Katmanı) seviyesinde çalışan DNS protokolünün işleyiş zincirini, siber güvenlik zafiyetlerini (Zehirlenme ve Hosts manipülasyonu) ve bu zafiyetlere karşı geliştirilen modern savunma mimarilerini incelemektedir.

---

## 1. DNS (Domain Name System) Nedir ve Nasıl Çalışır?
Bilgisayarlar ağ üzerinde yalnızca IP adresleri üzerinden haberleşebilirler. Ancak insanların karmaşık IP adreslerini ezberlemesi imkansız olduğundan, alan adlarını (Örn: `github.com`) bilgisayarların anlayacağı IP adreslerine çeviren küresel bir hiyerarşik telefon rehberine ihtiyaç duyulur. Bu sisteme **DNS** denir.



### Bir Web Sitesine Erişim Esnasındaki Sorgu Zinciri:
Kullanıcı tarayıcıya bir alan adı yazıp Enter'a bastığı an saliseler içinde şu sıra takip edilir:
1.  **DNS Cache (Yerel Önbellek):** İşletim sistemi önce kendi hafızasını kontrol eder: *"Bu siteye yakın zamanda gittim mi, IP adresi önbelleğimde var mı?"*
2.  **Hosts Dosyası Kontrolü:** İşletim sistemi yerel ağ eşlemelerinin yer aldığı özel bir metin dosyasına bakar. (Windows için: `C:\Windows\System32\drivers\etc\hosts`).
3.  **Çözümleyici (Recursive Resolver / DNS Sunucusu):** Yerelde bilgi bulunamazsa, istek modeme, oradan da internet servis sağlayıcısının veya özel olarak tanımlanmış (Google `8.8.8.8`, Cloudflare `1.1.1.1` vb.) DNS sunucularına iletilerek IP adresi talep edilir.
4.  **Bağlantı Kurulması:** DNS sunucusundan dönen IP adresi alınır, yerel önbelleğe yazılır ve hedef sunucuyla bağlantı başlatılır.

---

## 2. Siber Güvenlik Perspektifi (DNS Katmanı Saldırıları)

### DNS Spoofing / Poisoning (DNS Önbellek Zehirlenmesi)
* **Saldırı Mantığı:** Saldırgan yerel ağda ARP Zehirlenmesi ile araya girdiğinde (MitM) veya yetersiz korunan bir DNS sunucusunu manipüle ettiğinde bu saldırıyı gerçekleştirir. Kullanıcı `bankam.com` adresini sorguladığında, saldırgan gerçek DNS sunucusundan daha hızlı yanıt dönerek kurbana sahte bir IP adresi (`192.168.1.50` - saldırganın sunucusu) iletir.
* **Sonuç:** Kullanıcı adres çubuğunda doğru domaini görse de arkada hackerın hazırladığı sahte web sitesine (Phishing) yönlendirilir ve kimlik bilgileri çalınır.

### Yerel Hosts Dosyası Manipülasyonu (Trojan/Zararlı Yazılım Saldırısı)
* **Saldırı Mantığı:** Bilgisayara sızan bir zararlı yazılım (Trojan), sistemde yönetici (Administrator/Root) yetkisi elde ederse doğrudan `hosts` dosyasını hedef alır. Dosyanın en altına gizlice `192.168.1.50 instagram.com` satırını ekler.
* **Sonuç:** `hosts` dosyası dışarıdaki tüm DNS sunucularından daha öncelikli olduğu için, bilgisayar internete hiç sormadan doğrudan hackerın IP'sine gider. Güvenli DNS ayarları yapılmış olsa bile bu yerel dosya üzerinden sistem baypas edilir.

---

## 3. Mühendislik Çıkarımı ve Modern Savunma Katmanları (Mavi Takım)
DNS protokolünün ilk tasarımı şifresiz, doğrulanmamış ve UDP 53 portu üzerinden düz metin (plain-text) akacak şekilde kurgulanmıştır. Bu durum, araya giren herkesin trafiği manipüle etmesine izin verir. Bu açığı kapatmak için endüstri standardı olarak şu **güvenli protokoller ve kimlik doğrulama katmanları** geliştirilmiştir:

1.  **DNSSEC (Domain Name System Security Extensions):** DNS sorgularına dijital imza (cryptographic signing) katmanı getirir. Bu sayede bilgisayarımız, DNS sunucusundan gelen yanıtın yolda manipüle edilmediğini ve gerçekten yetkili sunucudan geldiğini **doğrular**.
2.  **DoH (DNS over HTTPS) & DoT (DNS over TLS):** DNS sorgularını düz metin olarak göndermek yerine şifreli bir tünel (HTTPS/TLS) içinden geçirir. Böylece yerel ağdaki bir saldırgan paketleri koklasa dahi hangi sitenin sorgulandığını göremez ve sahte IP yanıtları enjekte edemez.
3.  **Endpoint Güvenliği ve Hosts İzleme:** `hosts` dosyasının manipüle edilmesini önlemek için son kullanıcı cihazlarında EDR/Antivirüs yazılımları ile bu dosya sürekli izlenmeli, dosya üzerinde "Salt Okunur (Read-Only)" yetkileri sıkılaştırılmalıdır.
