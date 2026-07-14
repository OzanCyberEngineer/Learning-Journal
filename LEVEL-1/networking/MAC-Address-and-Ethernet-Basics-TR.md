# Öğrenim Raporu: MAC Adresi, Ethernet Temelleri ve Anahtarlayıcı (Switch) Güvenliği

Bu rapor; Katman 2 (Veri Bağı) seviyesindeki fiziksel adresleme mantığını, Ethernet protokolünün işleyişini ve bu yapıların siber güvenlik zafiyetleri üzerinden nasıl manipüle edilebileceğini incelemektedir.

---

## MAC Adresi Nedir?
MAC (Media Access Control) adresi, ağ kartına (NIC) üretici tarafından kalıcı olarak gömülen fiziksel bir adrestir. 48 bit uzunluğundadır ve 16'lık (Hexadecimal) sayı sisteminde, ikişerli gruplar halinde yazılır.

*   **Örnek Format:** `00:1A:2B:3C:4D:5E`

Bir MAC adresi mimari olarak tam ortadan ikiye bölünür (3 bayt + 3 bayt):
1.  **İlk 3 Blok (Örn: `00:1A:2B`):** **OUI (Organizationally Unique Identifier)** olarak adlandırılır. IEEE tarafından doğrudan üretici şirkete (Apple, Samsung, Intel vb.) tahsis edilir. Bu blok sayesinde cihazın markası tespit edilebilir. [MAC Vendors](https://macvendors.com/) gibi platformlar bu ilk 3 bloğu analiz ederek üretici bilgisini doğrular.
2.  **Son 3 Blok (Örn: `3C:4D:5E`):** Üreticinin o cihaza atadığı benzersiz bir seri numarasıdır.

---

## Ethernet ve Switch (Anahtarlayıcı) Mantığı

### Ethernet Protokolü
Ethernet, sadece fiziksel bir kablo değil; OSI modelinin **Katman 1 (Fiziksel)** ve **Katman 2 (Veri Bağı)** seviyelerinde çalışan bir kurallar bütünüdür (protokoldür). Yerel bir ağdaki (LAN) verilerin kablolu veya kablosuz ortamda **Çerçeve (Frame)** adı verilen paketçikler halinde nasıl taşınacağını belirler.

### Switch (Anahtarlayıcı) İşleyişi
Switch, yerel ağ trafiğini yöneten akıllı bir cihazdır. Bünyesinde bir **MAC Adres Tablosu (CAM Table)** barındırır. 
*   Bir cihaz yerel ağdaki başka bir cihaza veri göndermek istediğinde Switch bu tabloya bakar.
*   Veriyi ağdaki herkese yaymak yerine, yalnızca hedef MAC adresinin bağlı olduğu fiziksel porta (kabloya) iletir.
*   Hedef adres tabloda henüz yoksa, Switch trafiği tüm portlara gönderir; bu işleme **Broadcast (Yayın)** denir.

---

## Siber Güvenlik Perspektifi (Katman 2 Manipülasyonları)

Saldırganlar, Katman 2 protokollerinin doğasındaki "güven ilişkisini" suistimal etmek için çeşitli teknikler kullanırlar.

### 1. MAC Spoofing (MAC Sahteciliği)
*   **Senaryo:** Kurumsal ağlarda veya otellerde yetkisiz cihazların erişimini engellemek için "yalnızca belirli MAC adreslerine izin ver" kuralı (MAC Filtreleme) uygulanır.
*   **Saldırı Mantığı:** Saldırgan, Wi-Fi trafiğini koklayarak (sniffing) ağdaki yasal bir kullanıcının MAC adresini tespit eder. Ardından kendi işletim sistemi üzerinden ağ kartının dışarıya gösterdiği adresi bu yasal adresle değiştirir. 
*   **Sonuç:** Güvenlik mekanizmaları saldırganı yasal bir kullanıcı olarak algılar ve ağ erişimi sağlar.

### 2. MAC Flooding (CAM Tablosunu Şişirme)
*   **Senaryo:** Switch cihazlarının içindeki MAC Adres Tablosunun (CAM Table) bellek kapasitesi sınırlıdır.
*   **Saldırı Mantığı:** Saldırgan, ağa saniyede binlerce sahte ve rastgele üretilmiş MAC adresi gönderir. Switch'in CAM tablosu kısa sürede tamamen dolar.
*   **Sonuç:** Hafızası dolan Switch kararsızlaşır ve bir güvenlik hatası (fail-open) olarak **Hub** gibi davranmaya başlar. Yani gelen tüm özel trafiği ayırt etmeksizin ağdaki tüm portlara fırlatır. Saldırgan, ağdaki diğer tüm kullanıcıların şifrelerini ve hassas trafiklerini kendi bilgisayarından izleyebilir (sniffing) hale gelir.
