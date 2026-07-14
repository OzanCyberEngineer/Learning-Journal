# 🌐 DHCP'nin Anatomisi ve DORA Süreci

Bir cihazın ağa bağlandığı andan itibaren IP adresi, alt ağ maskesi, varsayılan ağ geçidi (default gateway) ve DNS bilgilerini otomatik olarak elde etmesini sağlayan 4 adımlı DORA el sıkışma sürecinin teknik incelemesi.
---
## 🏢 1. DORA El Sıkışma Şeması

Cihazların otomatik IP adresi almasını sağlayan protokol mekanizması, istemci ile DHCP sunucusu arasında gerçekleşen 4 adımlı bir el sıkışma süreciyle yürütülür:
  * Note over İstemci: IP Adresi Yok (0.0.0.0)
  * İstemci->>Sunucu: DHCPDISCOVER (L2/L3 Broadcast)
  * Note over Sunucu: Havuzdan IP Seçilir
  * Sunucu-->>İstemci: DHCPOFFER (Unicast / Broadcast)
  * İstemci->>Sunucu: DHCPREQUEST (Broadcast)
  * Sunucu-->>İstemci: DHCPACK (Unicast / Broadcast)
  * Note over İstemci: IP Resmi Olarak Tanımlandı
---
##🔍 2. Adım Adım Çalışma Mekanizması
1. Discover (Keşif - Broadcast)
Ağa yeni giren, kablo takan veya Wi-Fi'a bağlanan cihazın henüz bir IP adresi yoktur. Ağda iletişim kurabilmek adına "Ağda hiç DHCP sunucusu var mı? Bana bir IP verin!" diyerek bir DHCPDISCOVER paketi gönderir. Cihazın kimliği henüz belirlenmediği için bu paket hem L2 hem de L3 seviyesinde tam bir Broadcast (Yayın) olarak gönderilir:

Katman 2 (L2): Source MAC: Kendi MAC'i ➔ Destination MAC: FF:FF:FF:FF:FF:FF

Katman 3 (L3): Source IP: 0.0.0.0 ➔ Destination IP: 255.255.255.255

Bu sayede söz konusu istek yerel ağdaki tüm cihazlara ve sunuculara ulaştırılmış olur.

2. Offer (Teklif)
Ağ üzerinde aktif olarak çalışan DHCP sunucusu bu isteği yakalar. Kendi bünyesindeki boş adres havuzundan (IP Pool) bir IP seçer ve istemci cihaz için bir teklif dökümanı hazırlar. Hazırlanan bu DHCPOFFER paketinin içerisinde; istemciye tahsis edilmesi planlanan IP adresi, alt ağ maskesi (subnet mask), kiralama süresi (Lease Time) ve varsayılan ağ geçidi (default gateway) bilgileri yer alır.

3. Request (İstek - Broadcast)
İstemci sunucudan gelen teklifi teslim alır. Eğer yerel ağda birden fazla aktif DHCP sunucusu mevcutsa ve cihaz birden fazla teklif aldıysa, genellikle kendisine ilk ulaşan teklifi baz alır. Ardından ağa yeniden Broadcast formatında bir DHCPREQUEST paketi yayınlar: "Ben X sunucusunun verdiği Y IP adresini kabul ettim, diğer sunucular tekliflerini geri çekebilir, teşekkürler!"

4. Ack (Onay)
İlgili teklifi sunan DHCP sunucusu bu istek paketini alır. Söz konusu IP adresini kendi yerel veritabanında, isteği gerçekleştiren istemcinin fiziksel MAC adresiyle eşleştirerek rezerve eder. Son adım olarak istemciye DHCPACK (Acknowledge) onay paketini gönderir. Bu paketin istemciye ulaşmasıyla birlikte cihaz resmi olarak ağın bir üyesi haline gelir.
