# 🛡️ Bölüm 5: Siber Güvenlik Boyutu ve Geliştirme/Savunma Teknikleri

Katman 1 ve Katman 2 seviyesindeki açıklar yazılımsal güvenlik duvarları (WAF'lar, kurumsal firewall'lar) tarafından **görülemez**. Çünkü o cihazlar Katman 3 ve yukarısını inceler. Bu yüzden buradaki saldırılar çok ölümcüldür.

---

### 1. MAC Flooding (Switch Felç Etme) Saldırısı
Kurumsal bir switch, hangi portuna hangi MAC adresli cihazın bağlı olduğunu hafızasındaki **CAM (Content Addressable Memory) Tablosunda** tutar. Bir paket geldiğinde bu tabloya bakar ve paketi sadece ilgili porta gönderir.

Saldırgan, switch'e bağlı bir porttan saniyeler içinde binlerce sahte kaynak MAC adresine sahip paket fırlatır (**MAC Flooding**). Switch'in CAM tablosunun hafıza kapasitesi sınırlıdır. Tablo sahte kayıtlarla tamamen dolduğunda switch akıl tutulması yaşar ve koruma moduna geçer.

* **Saldırı Sonucu:** Switch, bir **"Hub"** gibi davranmaya başlar. Yani gelen her paketi, hedefi neresi olursa olsun tüm portlara broadcast olarak fırlatır. Saldırgan artık ağdaki diğer tüm şirket çalışanlarının trafiğini hiçbir ek çaba sarf etmeden dinleyebilir (**Sniffing**).

> 🛠️ **Savunma / Hardening Teknikleri (Port Security):**
> Kurumsal switch'lerde port seviyesinde sıkılaştırma yapılır. Port başına öğrenilebilecek maksimum MAC adresi sayısı sınırlandırılır (Örn: `switchport port-security maximum 1`). Eğer o porttan birden fazla farklı MAC adresi görülürse switch portu otomatik olarak kapatır (shutdown) veya saldırganın paketlerini drop eder.

---

### 2. MAC Spoofing (Kimlik Hırsızlığı)
Şirket ağlarında genellikle "MAC tabanlı kimlik doğrulama" (MAB) veya statik IP atamaları kullanılır. Saldırgan, hedef aldığı bir yöneticinin bilgisayarının MAC adresini kendi ağ kartına kopyalar:

🛡️ Savunma / Geliştirme Teknikleri (802.1X Kimlik Doğrulama):
Ağa bağlanırken sadece MAC adresine güvenilmemelidir. 802.1X (RADIUS/TACACS+) protokolü altyapıya entegre edilerek, cihaz kabloyu taktığı an kullanıcı adı/şifre veya dijital sertifika ile kimlik doğrulaması yapmaya zorlanmalıdır. Kimlik doğrulamadan geçemeyen cihazın portuna switch elektrik vermez.

---
🚀 Bölüm 6: Modern Sistem Mimarisinde MTU ve "Karanlık" Ağ Sorunları
[ Standart Paket: 1500 Byte ] ---> (Geçiş Başarılı ✅)
[ Tünellenmiş Paket (VXLAN): 1550 Byte ] ---> [ MTU: 1500 Limit ] ---> (Paket Drop / Parçalanma Kriz ❌)

⚠️ MTU Black Hole (Kara Delik) Nedir?
Eğer senin fiziksel ağındaki (Underlay) switch'lerin MTU sınırı hala 1500 byte ise, bu tünel paketleri kapılardan geçemez. Router'lar paketleri parçalamaya çalışır, parçalanamayan paketler ise doğrudan drop edilir.

Kullanıcı sistemi açtığında web sitesinin yarısının yüklendiğini, büyük dosyaların (Örn: Docker imajları push edilirken) yarıda kilitlendiğini görür ama küçük paketler (Ping gibi) sorunsuz çalışır. Buna ağ dünyasında MTU Black Hole (Kara Delik) denir.

💡 Mimari Çözüm: Jumbo Frames
Modern veri merkezlerinde ve bulut altyapılarında bu sorunun önüne geçmek için fiziksel switch ve sunucuların MTU değeri varsayılan 1500'den 9000 Byte (Jumbo Frames) seviyesine yükseltilir. Böylece araya tünel başlıkları girse bile paketler asla parçalanmadan, tek parça halinde ve ışık hızında iletilir.
