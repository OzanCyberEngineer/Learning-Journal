# Öğrenim Raporu: Bir Web İsteğinin Anatomisi (Uçtan Uca Paket Yolculuğu)

Bu rapor; bir istemcinin tarayıcı üzerinden bir web sitesine (Örn: `github.com`) erişim isteği başlattığı andan, sayfanın ekrana yüklenmesine kadar geçen sürede Katman 2, 3, 4 ve 7 protokollerinin arka planda nasıl birlikte çalıştığını adım adım analiz etmektedir.

---

## 🏃‍♂️ Adım Adım Uçtan Uca Ağ Akışı

### 1. Adım: İsim Çözümleme Katmanı (DNS Sorgusu)
İstemci bilgisayar (`192.168.1.10`) hedef alan adının mantıksal IP adresini bilmeden dış ağa paket iletemez.
* **İşlem:** Tarayıcı, yerel önbellekte veri bulamazsa, ağ geçidi (Gateway) üzerinden DNS sunucusuna (UDP Port 53) bir istek fırlatır: *"github.com alan adının IP adresi nedir?"*
* **Sonuç:** Sunucu, `140.82.113.25` hedef IP adresini istemciye döner. Mantıksal hedef artık bellidir.

### 2. Adım: Yerel Adres Çözümleme (ARP ve Switch İşlemleri)
Bilgisayar, paketi oluştururken işletim sisteminin yönlendirme tablosuna (`netstat -r`) bakar ve hedef IP'nin yerel ağda (LAN) olmadığını, dış ağda (WAN) olduğunu anlar. Bu yüzden paketi varsayılan ağ geçidine (Modem/Router - `192.168.1.1`) teslim etmelidir.
* **İşlem:** Ağ geçidinin fiziksel adresini bulmak için yerel **ARP Önbelleği** (`arp -a`) kontrol edilir. Gerekirse bir ARP Request (Broadcast) atılarak modemin MAC adresi (`c0-51-5c...`) öğrenilir.
* **Sonuç:** Paket, Katman 2 seviyesinde modemin MAC adresine etiketlenir ve Switch üzerinden doğrudan ağ geçidinin bağlı olduğu fiziksel porta iletilir.

### 3. Adım: Oturum Katmanı Güvenliği (TCP Üçlü El Sıkışma)
Tarayıcı, HTTPS protokolü üzerinden güvenli bir veri akışı başlatmak amacıyla karşı sunucunun **Port 443** kapısını hedefler. Veri transferinden önce TCP güvenli tüneli kurulmalıdır.
* **İşlem (3-Way Handshake):**
    1.  İstemci -> Sunucu: `SYN` (Bağlantı isteği)
    2.  Sunucu -> İstemci: `SYN-ACK` (İsteği onaylama ve senkronizasyon)
    3.  İstemci -> Sunucu: `ACK` (Son onay ve el sıkışmanın tamamlanması)
* **Sonuç:** Oturum `ESTABLISHED` durumuna geçer ve şifreli veri akışı için zemin hazırlanır.

### 4. Adım: Sınır Koruması ve Sınırlar Arası Geçiş (NAT ve Routing)
Paket yerel ağı terk edip internet dünyasına açılırken ağ geçidi (Modem/Router) devreye girer.
* **NAT (Network Address Translation):** Yerel ağda kullanılan ve internette yönlendirilemeyen Private IP (`192.168.1.10`), modemin dış dünyaya bakan yasal **Public IP** adresi ile değiştirilir.
* **Routing:** Paket, İnternet Servis Sağlayıcının (ISP) omurga yönlendiricilerine aktarılır. Buradan okyanus altı fiber hatlar ve kıtalararası Router cihazları üzerinden zıplayarak (Hop) GitHub veri merkezinin sınır güvenlik duvarlarına (Firewall) ulaşır.

### 5. Adım: Yanıt ve Tarayıcı Tarafında İşleme (Rendering)
GitHub web sunucuları gelen HTTP GET isteğini işler. Web sayfasının kaynak kodlarını (HTML, CSS, JS) içeren TCP paketlerini aynı rotayı tersten takip ederek istemciye geri gönderir. İstemcinin tarayıcısı bu kodları işleyerek görsel arayüzü ekrana çizer.

---

## 🕵️‍♂️ Siber Güvenlik Bakış Açısı (Saldırı Yüzey Analizi)
Bu uçtan uca senaryoda, ağın her adımında siber saldırganlar için farklı bir manipülasyon alanı (Attack Surface) bulunur:
1.  **DNS Aşamasında:** Saldırgan DNS Spoofing ile araya girerek kurbanı sahte bir IP'ye yönlendirebilir.
2.  **ARP Aşamasında:** Yerel ağdaki saldırgan ARP Poisoning ile modemin MAC adresini taklit edip tüm bu trafiği kendi üzerinden akıtabilir (MitM).
3.  **TCP Aşamasında:** Saldırgan sahte kaynak IP'lerle sunucuya el sıkışma tamamlanmayan milyonlarca `SYN` paketi atarak sistemi çökertebilir (SYN Flood DDoS).
