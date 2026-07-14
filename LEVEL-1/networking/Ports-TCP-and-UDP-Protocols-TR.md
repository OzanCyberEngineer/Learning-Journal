# Öğrenim Raporu: Port Kavramı, Taşıma Katmanı Protokolleri (TCP/UDP) ve El Sıkışma Güvenliği

Bu rapor; Katman 4 (Taşıma Katmanı) seviyesindeki mantıksal port mimarisini, TCP ve UDP protokollerinin yapısal farklarını, Üçlü El Sıkışma (3-Way Handshake) mekanizmasını ve bu mekanizmaya yönelik siber saldırıları incelemektedir.

---

## 1. Port (Liman) Kavramı Nedir?
Bir IP adresi, ağ üzerindeki belirli bir cihazı (hedefi) bulmamızı sağlar. Ancak bir cihaz aynı anda birden fazla ağ servisiyle (Web, Discord, Spotify vb.) iletişim kurabilir. Gelen veri paketlerinin hangi uygulamaya ait olduğunu ayırt etmek için mantıksal kapılar kullanılır; bunlara **Port (Liman)** denir.

Bir işletim sisteminde toplam **65.535** adet port bulunur. Bunlar siber güvenlik dünyasında üç ana gruba ayrılır:
* **0 - 1023:** Tanınmış Portlar (Well-Known Ports) - Standart sistem servisleri için rezerve edilmiştir.
* **1024 - 49151:** Kayıtlı Portlar (Registered Ports) - Belirli şirket veya uygulamalar için ayrılmıştır.
* **49152 - 65535:** Dinamik/Geçici Portlar (Dynamic/Private Ports) - İstemcilerin anlık bağlantıları için kullanılır.

### Siber Güvenlikte Kritik Popüler Portlar:
| Port Numarası | Protokol / Servis | Güvenlik Durumu ve Tanımı |
| :---: | :--- | :--- |
| **21** | FTP (File Transfer Protocol) | Kimlik bilgileri düz metin (plain-text) akar, zafiyetlere açıktır. |
| **22** | SSH (Secure Shell) | Güvenli, şifreli uzak yönetim protokolüdür. |
| **23** | Telnet | Şifresiz ve güvensizdir. Günümüz ağlarında doğrudan bir zafiyet kabul edilir. |
| **25** | SMTP (Simple Mail Transfer Protocol) | E-posta iletim servisidir. |
| **53** | DNS (Domain Name System) | İsim çözümleme servisidir (Genellikle UDP kullanır). |
| **80** | HTTP | Şifresiz web trafiğidir; MitM saldırılarına tamamen açıktır. |
| **443** | HTTPS (HTTP Secure) | SSL/TLS ile şifrelenmiş güvenli web trafiğidir. |

---

## 2. TCP (Transmission Control Protocol) ve Üçlü El Sıkışma
TCP; bağlantı tabanlı (connection-oriented), güvenilir ve veri iletimini garanti eden bir Taşıma Katmanı protokolüdür. Paket kaybı yaşandığında veriyi yeniden gönderir ve sırasını korur (Web trafiği, SSH, dosya aktarımı vb.).



### Üçlü El Sıkışma (3-Way Handshake) Mekanizması:
TCP, veri aktarımına başlamadan önce hedefle güvenli bir oturum başlatır:
1.  **SYN (Synchronize):** İstemci, sunucuya rastgele bir sıra numarası (Sequence Number) içeren ve bağlantı kurmak istediğini belirten bir `SYN` bayraklı (flag) paket gönderir.
2.  **SYN-ACK (Synchronize-Acknowledgment):** Sunucu isteği onaylar. İstemcinin sıra numarasını 1 artırarak onaylar (`ACK`) ve kendisi de senkronizasyon için kendi sıra numarasıyla bir `SYN` bayrağı ekleyerek `SYN-ACK` paketi döner.
3.  **ACK (Acknowledgment):** İstemci, sunucunun sıra numarasını 1 artırarak son onay paketini (`ACK`) gönderir. Oturum açılır (Established).

---

## 3. UDP (User Datagram Protocol) - Hızlı Taşımacılık
UDP; bağlantısız (connectionless) ve veri iletim garantisi sunmayan bir protokoldür. 
* El sıkışma mekanizması yoktur; veriyi doğrudan hedefe gönderir. Paketin yolda kaybolmasını veya sırasının bozulmasını denetlemez.
* **Kullanım Alanları:** Canlı yayınlar, VoIP sesli görüşmeler ve online oyunlar. Bu senaryolarda gecikme (latency) süresi, paket kaybından daha kritik olduğu için UDP tercih edilir.

---

## 4. Siber Güvenlik Perspektifi (Saldırı Vektörleri)

### Port Taraması (Port Scanning)
Saldırganlar, bir hedef sisteme sızmadan önce keşif (Reconnaissance) aşamasında **Nmap** gibi araçlarla tüm portları tarar. Amaç; açık kalmış güvensiz servisleri (Örn: Port 23 Telnet) veya güncellenmemiş, üzerinde zafiyet (exploit) barındıran servis sürümlerini tespit etmektir.

### SYN Flood (DDoS) Saldırısı
* **Saldırı Mantığı:** Bir TCP tasarım zafiyeti saldırısıdır. Saldırgan, sunucuya saniyede milyonlarca sahte kaynak IP'li `SYN` paketi gönderir. Sunucu, gelen her istek için hafızasında bir yer ayırır ve `SYN-ACK` yanıtı dönerek üçüncü adımı (`ACK`) beklemeye başlar. 
* **Sonuç:** Saldırgan son `ACK` paketini asla göndermez. Sunucunun bağlantı bekleyen havuzu (Backlog Queue) tamamen dolar. Sunucu kaynakları (CPU/RAM) tükenir ve meşru kullanıcılara hizmet veremez hale gelerek çöker (Denial of Service).
