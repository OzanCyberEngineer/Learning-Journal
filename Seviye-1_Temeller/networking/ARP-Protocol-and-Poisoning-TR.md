# Öğrenim Raporu: ARP Protokolü, Zehirlenme (Poisoning) Saldırıları ve Savunma Yaklaşımları

Bu rapor; Katman 2 (Veri Bağı) ve Katman 3 (Ağ) arasında bir köprü görevi gören ARP protokolünün mekanizmasını, doğasındaki güvenlik zafiyetlerini ve bu zafiyetlerin MitM saldırılarında nasıl kullanıldığını incelemektedir.

---

## ARP (Address Resolution Protocol) Nedir ve Nasıl Çalışır?
ARP, bir yerel ağdaki (LAN) mantıksal IP adreslerini, fiziksel MAC adreslerine eşleyen bir çözümleme protokolüdür. Verinin bir cihazdan diğerine yerel ağda iletilebilmesi için hedef cihazın MAC adresinin bilinmesi zorunludur.

### ARP İşleyiş Senaryosu:
Aynı yerel ağda bulunan üç cihazı (Bilgisayar A, Bilgisayar B/Modem ve Saldırgan C) ele alalım:

1.  **ARP Request (İstek - Broadcast):** Bilgisayar A, internete çıkmak için varsayılan ağ geçidinin (Modem) MAC adresine ihtiyaç duyar. Ağa bir yayın (Broadcast) paketi fırlatır: *"192.168.1.1 IP adresine sahip cihazın MAC adresi nedir?"* Bu mesaj ağdaki tüm cihazlara ulaşır.
2.  **Ağdaki Tepki:** Saldırgan (C) dahil olmak üzere ağdaki diğer tüm cihazlar bu isteği inceler. Kendi IP adresleri ile eşleşmediği için paketi düşürürler (drop).
3.  **ARP Reply (Cevap - Unicast):** Hedef IP'ye sahip olan Modem, doğrudan Bilgisayar A'ya tekli gönderim (Unicast) ile yanıt döner: *"192.168.1.1 IP adresi bana ait ve MAC adresim BB-BB-BB..."*
4.  **ARP Cache (Önbellek):** Bilgisayar A, her iletişimde ağda gereksiz trafik (gürültü) oluşturmamak için bu bilgiyi yerel hafızasına kaydeder. Bu alana **ARP Tablosu (ARP Cache)** denir. Windows sistemlerde bu tabloya `arp -a` komutuyla erişilebilir.

---

## Siber Güvenlik Perspektifi: ARP Zehirlenmesi (ARP Spoofing / MitM)
ARP protokolü tasarlandığı dönem itibariyle herhangi bir **kimlik doğrulama (authentication)** mekanizmasına sahip değildir. Cihazlar, kendilerine gelen ARP cevaplarının gerçekten o IP'ye sahip cihazdan gelip gelmediğini doğrulayamaz. Saldırganlar bu tasarım hatasını Ortadaki Adam (MitM) saldırıları için suistimal eder.

### Saldırı Mekanizması:
Saldırgan (C), ağdaki cihazların ARP tablolarını manipüle etmek için sürekli olarak sahte ARP cevapları (Gratuitous ARP) gönderir:
* **Bilgisayar A'ya:** *"Ben aslında 192.168.1.1 (Modem) cihazıyım ve yeni MAC adresim CC-CC-CC..."* der.
* **Modeme:** *"Ben aslında 192.168.1.5 (Bilgisayar A) cihazıyım ve yeni MAC adresim CC-CC-CC..."* der.

**Sonuç:** Bilgisayar A ve Modem arasındaki tüm trafik Saldırgan C'nin ağ kartı üzerinden akmaya başlar. Saldırgan bu trafiği şeffaf bir şekilde dinleyebilir (sniffing), şifresiz verileri okuyabilir veya trafiği manipüle edebilir.

---

## Mühendislik Çıkarımı ve Savunma Önerisi (Mavi Takım Yaklaşımı)
Bir siber güvenlik uzmanı veya sistem yöneticisi olarak, ARP tablosundaki anormal değişiklikleri izlemek kritik bir proaktif savunma adımıdır. 

* **Otomasyon Fikri:** Kritik sunucularda veya ağ geçitlerinde çalışan basit bir Python scripti veya otomasyon aracı ile `arp -a` çıktısı belirli periyotlarla (günlük/saatlik) taranabilir. 
* **Anomali Tespiti:** Eğer aynı MAC adresi birden fazla farklı IP adresiyle eşleşmeye başladıysa veya kritik bir IP'nin (örneğin Gateway) MAC adresi aniden değiştiyse, sistem otomatik olarak alarm üretebilir. 
* **Kurumsal Çözüm:** Büyük ölçekli ağlarda ise bu saldırıları engellemek için Switch seviyesinde **Dynamic ARP Inspection (DAI)** ve **DHCP Snooping** gibi güvenlik özellikleri aktif edilmelidir.
