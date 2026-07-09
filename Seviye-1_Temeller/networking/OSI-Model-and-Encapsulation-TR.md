# Öğrenim Raporu: OSI Modeli, TCP/IP Modeli ve Kapsülleme (Encapsulation)

Bu rapor, bilgisayar ağlarının temel işleyişini, veri iletim süreçlerini ve bu yapıların siber güvenlik perspektifindeki önemini anlamak amacıyla hazırlanmıştır.

---

## OSI Modeli Nedir?
OSI (Open Systems Interconnection) Modeli, farklı bilgisayar sistemlerinin bir ağ üzerinden birbiriyle nasıl iletişim kurduğunu açıklayan bir referans modelidir. 

Bu modelin temel amacı, karmaşık ağ iletişim süreçlerini standartlaştırarak anlaşılmasını kolaylaştırmaktır. OSI modeli, iletişim sürecini her birinin kendine özgü görev ve sorumlulukları olan **7 farklı katmana** ayırır.

### OSI Modelinin 7 Katmanı

1. **Uygulama Katmanı (Application Layer):** Kullanıcıya en yakın katmandır ve uygulamaların ağ hizmetlerinden yararlanmasını sağlar. Chrome veya Brave gibi web tarayıcıları, bu katmana ait olan **HTTP** ve **HTTPS** protokolleri ile çalışır.
2. **Sunum Katmanı (Presentation Layer):** Veriyi uygulama katmanı için hazırlar; verinin çevrilmesi, sıkıştırılması ve şifrelenmesinden sorumludur. Örneğin, **SSL/TLS** gibi şifreleme mantıkları bu katmanla ilişkilidir.
3. **Oturum Katmanı (Session Layer):** İki sistem arasındaki bağlantıyı yönetir. Cihazlar arasındaki iletişim oturumlarının başlatılması, sürdürülmesi ve sonlandırılmasından sorumludur.
4. **Taşıma Katmanı (Transport Layer):** Verinin nasıl taşınacağını belirler. Büyük verileri **segment** adı verilen küçük parçalara ayırır ve alıcı tarafta doğru şekilde birleştirilmesi için sıra numaraları ekler. En kritik protokolleri **TCP** (bağlantı tabanlı ve güvenilir) ve **UDP** (hızlı ama teslimat garantisi sunmayan) protokolleridir.
5. **Ağ Katmanı (Network Layer):** Mantıksal adresleme ve yönlendirmeden (routing) sorumludur. Veriye kaynak ve hedef IP adresleri bu katmanda eklenir ve veri artık **paket (packet)** olarak adlandırılır.
6. **Veri Bağı Katmanı (Data Link Layer):** Yerel ağ (LAN) içindeki fiziksel adreslemeyi (**MAC adresi**) kullanır. IP adresleri ağlar arası iletişimi sağlarken, MAC adresleri aynı yerel ağdaki cihazların birbirini bulmasını sağlar. Bu katmanda veri **çerçeve (frame)** adını alır.
7. **Fiziksel Katman (Physical Layer):** Verinin kablolar, sinyaller ve dijital veriler (0 ve 1'ler) üzerinden fiziksel olarak iletilmesini kapsar. Veriler **bit** formatında taşınır.

---

## Gerçek Dünyada TCP/IP Modeli
OSI modeli teorik öğrenim ve analiz için mükemmel bir araç olsa da, günümüz internet altyapısı pratik olarak **TCP/IP Modeli** üzerinde çalışır. TCP/IP, OSI katmanlarını birleştirerek daha sade bir yapı sunar:

*   **Uygulama Katmanı:** OSI'nin 7, 6 ve 5. katmanlarını birleştirir.
*   **Taşıma Katmanı:** OSI'nin 4. katmanı ile aynı kalır.
*   **İnternet Katmanı:** OSI'nin Ağ Katmanına (3. Katman) denk gelir.
*   **Ağ Erişimi / Bağlantı Katmanı:** OSI'nin 2 ve 1. katmanlarını kapsar.

---

## Kapsülleme (Encapsulation) Nedir?
Kapsülleme, verinin üst katmanlardan alt katmanlara doğru inerken her aşamada üzerine ek protokol bilgilerinin (header/üstbilgi) eklenmesi sürecidir. Bu süreç, alıcı cihazın veriyi nasıl işlemesi gerektiğini anlamasını sağlar.

*   **Uygulama Katmanı:** Veri (Data)
*   **Taşıma Katmanı:** Veri + TCP Başlığı = **Segment**
*   **Ağ Katmanı:** Segment + IP Başlığı = **Paket (Packet)**
*   **Veri Bağı Katmanı:** Paket + MAC Başlığı = **Çerçeve (Frame)**
*   **Fiziksel Katman:** Çerçeve -> **Bit (0-1)**

---

## Siber Güvenlik Perspektifi

Siber güvenlik alanında ağ trafiğinin kaynağını, hedefini ve yapısını analiz edebilmek için OSI modelini derinlemesine bilmek hayati önem taşır. Farklı saldırı ve savunma mekanizmaları, doğrudan bu katmanlarla ilişkilidir.

### Taşıma Katmanı (Layer 4) Güvenliği
Taşıma Katmanı; portları, trafik akışını ve bağlantı durumlarını yönettiği için trafik analizinde kritik bir role sahiptir. 
*   **Saldırı Analizi:** Saldırganlar, TCP el sıkışma (handshake) sürecini manipüle ederek **SYN Flood** gibi DoS/DDoS saldırıları gerçekleştirebilirler. Ayrıca port tarama (port scanning) teknikleri de bu katmanda çalışır.
*   **Araçlar:** **Wireshark** veya **tcpdump** gibi araçlarla kaynak/hedef portları incelenerek sistemler arası şifresiz veya şüpheli iletişimler tespit edilebilir.

### Oturum, Sunum ve Uygulama Katmanı Güvenliği
TCP/IP modelinde tek bir çatı altında toplanan üst katmanlar; kimlik doğrulama, oturum yönetimi, veri formatları ve şifreleme zafiyetlerini kapsar.
*   **Odak Alanları:** Oturum yönetimi, SSL/TLS zafiyetleri, veri manipülasyonu ve web uygulaması güvenliği gibi konular günümüzde **Uygulama Güvenliği (AppSec)** çatısı altında incelenir.

## Sonuç
OSI ve TCP/IP modellerini kavramak, siber güvenlik çalışmaları için sarsılmaz bir temel oluşturur. Verinin ağ üzerindeki yolculuğunu, paketlerin nasıl kapsüllendiğini ve üst katmanlarda nasıl işlendiğini bilmeden ağ trafiğini analiz etmek veya olası saldırıları engellemek mümkün değildir.
