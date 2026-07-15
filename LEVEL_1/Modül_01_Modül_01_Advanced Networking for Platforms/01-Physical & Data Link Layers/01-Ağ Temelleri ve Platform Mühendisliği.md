# Fiziksel ve Veri Bağı Katmanları (Physical & Data Link Layers)

Bu doküman, OSI (Open Systems Interconnection) referans modelinin en altında yer alan ve ağ iletişiminin temelini oluşturan Fiziksel Katman (Layer 1) ve Veri Bağı Katmanını (Layer 2) kapsamlı bir teknik çerçevede ele almaktadır. Ağ Altyapısı ve Temelleri modülünün bir parçası olan bu çalışma; verinin fiziksel ortamlardan elektrik, ışık veya radyo sinyalleriyle nasıl iletildiğini, yerel ağlarda cihazların donanımsal olarak nasıl adreslendiğini (MAC), anahtarlama (switching) mekanizmalarını, Linux çekirdeğindeki (kernel) sanal ağ yapılarını, karşılaşılan güvenlik tehditlerini ve üretim (production) ortamı pratiklerini incelemektedir. Bu dokümanı tamamlayan okuyucular, düşük seviyeli ağ trafiğini analiz etme, donanımsal ve mantıksal ağ hatalarını giderme (troubleshooting) ve sanal ağ topolojilerini doğrulama becerilerini kazanacaktır.

---

## 1. Genel Bakış

Bilgisayar ağları, bağımsız çalışan sistemlerin standart protokoller ve fiziksel bağlantılar aracılığıyla veri alışverişi yapmasını sağlayan karmaşık yapılardır. Ağ mimarisinin en alt iki katmanını oluşturan Fiziksel ve Veri Bağı katmanları, üst katmanlardaki mantıksal süreçlerin (örneğin IP yönlendirmesi veya uygulama verileri) donanımdan bağımsız bir şekilde çalışabilmesi için gerekli soyutlamayı sağlar.

* **Fiziksel Katman (Layer 1):** Verinin iletim ortamı (kablo, hava, fiber vb.) üzerinden ham bit dizileri (0 ve 1) halinde, fiziksel sinyallerle (voltaj, ışık dalgası, RF) taşınmasından sorumludur.
* **Veri Bağı Katmanı (Layer 2):** Fiziksel katmandaki ham bit iletimini anlamlı veri bloklarına (**çerçeve / frame**) dönüştürür. Yerel ağ (LAN) içindeki fiziksel adreslemeyi (MAC), hata denetimini (CRC) ve ortama erişim kurallarını yönetir.

Bu iki katman, ağ arabirim kartları (NIC) ve switchler (anahtarlayıcılar) aracılığıyla donanım ile yazılım arasındaki kritik köprüyü oluşturur.

---

## 2. Neden Önemlidir?

Alt katman dinamiklerini anlamak, modern altyapıların yönetilmesinde ve güvenliğinin sağlanmasında kritik bir rol oynar:

* **Sistem Yönetimi ve SRE (Site Reliability Engineering):** Sunucular arası paket kayıpları, yavaş veri aktarımları, yanlış MTU (Maximum Transmission Unit) yapılandırmasından kaynaklanan paket düşmeleri veya ağ kartı (NIC) seviyesindeki donanımsal tampon (buffer) dolulukları doğrudan bu katmanların metrikleriyle teşhis edilir.
* **Ağ Mühendisliği:** Fiziksel topolojilerin tasarımı, switch portlarının yönetimi, döngülerin (loop) engellenmesi (STP), VLAN segmentasyonu ve yüksek bant genişliği sağlayan hat birleştirme (LACP) işlemleri tamamen bu katmanların çalışma prensiplerine dayanır.
* **Siber Güvenlik:** Ağın sınır hatlarında gerçekleştirilen MAC Spoofing, CAM Tablosu Tüketme (CAM Flooding) ve ARP Poisoning gibi saldırılar doğrudan Katman 2 zayıflıklarını hedef alır. Bu saldırıların tespiti ve savunulması (Port Security, DAI, 802.1X) alt katman uzmanlığı gerektirir.
* **Platform ve DevOps Mühendisliği:** Docker container'ları veya Kubernetes Pod'larının aynı host üzerinde veya hostlar arasında nasıl haberleştiğini anlamak için Linux çekirdeğinde çalışan sanal switch (Linux Bridge) ve sanal ethernet arayüzü (veth pair) yapılarının yönetilmesi gerekir.

---

## 3. Temel Kavramlar ve Terminoloji

Ağ performansını ve altyapı limitlerini belirleyen temel kavramlar aşağıdaki tabloda açıklanmıştır:

| Kavram | Açıklama | Sistemdeki Rolü |
| :--- | :--- | :--- |
| **Bant Genişliği (Bandwidth)** | Bir iletişim kanalının bir saniyede taşıyabileceği teorik maksimum veri miktarıdır. | Hattın kapasite limitini belirler (Örn: 10 Gbps Ethernet). |
| **Aktarım Hızı (Throughput)** | Belirli bir anda, pratik koşullar altında hattan başarıyla geçen gerçek veri miktarıdır. | Protokol başlıkları (overhead) ve paket kayıpları düşüldükten sonra kalan net hızı temsil eder. |
| **Gecikme Süresi (Latency)** | Bir veri paketinin kaynaktan çıkıp hedefe ulaşması için geçen süredir (milisaniye - ms). | Sistemler arası senkronizasyon ve tepki süresini doğrudan etkiler. |
| **Gecikme Sapması (Jitter)** | Paketlerin hedefe ulaşma sürelerindeki tutarsızlık veya varyasyondur. | Gerçek zamanlı veri akışlarında (ses, video) paket sırasının bozulmasına yol açar. |
| **Paket Kaybı (Packet Loss)** | Gönderilen paketlerin hedefe ulaşamadan yolda düşmesi (drop) durumudur. | TCP gibi protokollerde yeniden iletimi (retransmission) tetikleyerek hızı düşürür. |
| **Çarpışma Alanı (Collision Domain)** | Aynı fiziksel hatta birden fazla cihazın veri göndermesi durumunda sinyallerin çakışabileceği ağ alanıdır. | Eski hub tabanlı ağlarda yaygındır; modern switch portlarında her port ayrı bir çarpışma alanıdır. |
| **Yayın Alanı (Broadcast Domain)** | Gönderilen bir broadcast (herkese yayın) çerçevesinin ulaştığı tüm cihazları kapsayan ağ sınırıdır. | Router (yönlendirici) cihazları veya sanal yerel ağlar (VLAN) ile sınırlandırılır. |

---

## 4. Çalışma Mantığı

Ağ sistemlerinde karmaşıklığı yönetmek ve standardizasyonu sağlamak amacıyla katmanlı referans modelleri kullanılır.

### OSI ve TCP/IP Modelleri

```text
       OSI Referans Modeli                   TCP/IP Modeli
   +---------------------------+       +---------------------------+
 7 | Uygulama (Application)    |       |                           |
   +---------------------------+       | Uygulama (Application)    |
 6 | Sunum (Presentation)      |       |                           |
   +---------------------------+       +---------------------------+
 5 | Oturum (Session)          |       | Taşıma (Transport)        |
   +---------------------------+       +---------------------------+
 4 | Taşıma (Transport)        |       | İnternet (Internet)       |
   +---------------------------+       +---------------------------+
 3 | Ağ (Network)              |       |                           |
   +---------------------------+       | Ağ Erişim (Network Access)|
 2 | Veri Bağı (Data Link)     |       |                           |
   +---------------------------+       +---------------------------+
 1 | Fiziksel (Physical)       |
   +---------------------------+
```

* **OSI Modeli (7 Katman):** ISO tarafından geliştirilen, teorik ve eğitsel standardı belirleyen referans modelidir.
* **TCP/IP Modeli (4 Katman):** Gerçek dünya ağ altyapısının (İnternet) üzerinde çalıştığı pratik modeldir. OSI'deki ilk üç katman (Uygulama, Sunum, Oturum) TCP/IP'de tek bir "Uygulama" katmanında birleştirilmiştir. OSI'deki Fiziksel ve Veri Bağı katmanları ise TCP/IP modelinde "Ağ Erişim (Network Access)" katmanı altında ele alınır.

### Kapsülleme (Encapsulation) ve Kapsül Açma (Decapsulation)

Veri ağ üzerinden gönderilirken üst katmandan alt katmana doğru iner. Her aşamada ilgili katmanın kontrol bilgilerini içeren bir başlık (header) ve bazı durumlarda kuyruk (trailer) eklenir. Alıcı tarafta ise bu işlem tersine işletilerek başlıklar tek tek soyulur.

* **Protocol Data Units (PDU - Protokol Veri Birimleri):**
    * **Veri (Data):** Uygulama katmanında üretilen ham bilgi.
    * **Segment / Datagram (Taşıma - L4):** Veriye kaynak/hedef portları ve TCP/UDP başlıkları eklenmiş hali.
    * **Paket (Ağ - L3):** Segment'e kaynak/hedef IP adresleri eklenmiş hali.
    * **Çerçeve / Frame (Veri Bağı - L2):** Pakete kaynak/hedef MAC adresleri ve hata kontrol kodu (FCS) eklenmiş hali.
    * **Bit (Fiziksel - L1):** Fiziksel iletim ortamından geçecek olan 0 ve 1 sinyalleri.

```text
[Veri] (Uygulama)
  |
  +--> [L4 Başlığı][Veri]  ==> Segment / Datagram (Taşıma)
         |
         +--> [L3 Başlığı][L4 Başlığı][Veri]  ==> Paket (Ağ)
                |
                +--> [L2 Başlığı][L3 Başlığı][L4 Başlığı][Veri][L2 Kuyruğu]  ==> Çerçeve (Veri Bağı)
                       |
                       v
                011010011101...  ==> Bit (Fiziksel)
```

---

## 5. Mimari ve Bileşenler

Fiziksel ve Veri Bağı katmanlarının kararlı çalışmasını sağlayan donanımsal ve yazılımsal bileşenler şunlardır:

### 1. Fiziksel Katman Bileşenleri ve Sorumlulukları

* **Analog ve Dijital Sinyaller:** Dijital sinyaller kesikli voltaj seviyeleriyle (0 ve 1) veri taşırken, analog sinyaller sürekli dalga boyu (frekans, genlik) değişimlerini kullanır. Ağ kartları bu iki dünya arasındaki dönüşümü gerçekleştirir.
* **İletim Ortamları (Transmission Media):**
    * *Bakır (Copper):* Elektrik sinyalleri kullanır (Örn: Cat6 UTP kablolar). Ucuzdur ancak elektromanyetik parazitlerden (EMI) etkilenir ve mesafe limiti (~100m) vardır.
    * *Fiber Optik:* Işık dalgalarını cam lifler içinden yansıtır. Çok yüksek bant genişliği sunar, EMI'dan etkilenmez, kilometrelerce mesafeye kayıpsız ulaşır ancak maliyetlidir.
    * *Kablosuz (Wireless):* Radyo dalgalarını (RF) kullanır. Mobilite sağlar ancak çevresel engellerden ve parazitlerden kolayca etkilenir.
* **Ağ Arabirim Kartı (NIC):** Bilgisayarı ağ ortamına fiziksel olarak bağlayan donanımdır. Dijital veriyi fiziksel sinyale çeviren PHY (Physical Layer) yongasını barındırır.
* **Duplex ve Hız Müzakeresi (Speed/Duplex Negotiation):**
    * *Half-Duplex (Yarı Çift Yönlü):* Aynı anda sadece veri gönderilebilir veya alınabilir (Telsiz mantığı).
    * *Full-Duplex (Tam Çift Yönlü):* Aynı anda hem veri gönderilebilir hem de alınabilir.
    * *Auto-Negotiation:* İki cihazın bağlanırken destekledikleri en yüksek hız ve duplex modunu (Örn: 1000 Mbps Full-Duplex) otomatik olarak kararlaştırması sürecidir.

### 2. Veri Bağı Katmanı Bileşenleri ve Sorumlulukları

* **Ethernet Standartları:** IEEE 802.3 ailesi altında tanımlanmıştır. Kablolu ağların çerçeve yapısını ve ortama erişim kurallarını belirler.
* **MAC Adresleme (Media Access Control):** Ağ kartı üreticisi tarafından donanıma kalıcı olarak yazılan (burned-in address), 48 bitlik (6 byte) benzersiz fiziksel adrestir. İlk 24 biti üreticiyi tanımlayan OUI (Organizationally Unique Identifier) bilgisini taşır. Onaltılık (hexadecimal) tabanda yazılır (Örn: `AA:BB:CC:DD:EE:FF`).
* **Ethernet II Çerçeve Yapısı:**

```text
+-------------------+------------------+------------------+---------------+-------------------------+-----------------------+
| Preamble (Öncü)   | Hedef MAC        | Kaynak MAC       | Tip (Type)    | Veri (Payload)          | Hata Kontrolü (FCS)   |
| 8 Byte            | 6 Byte           | 6 Byte           | 2 Byte        | 46 - 1500 Byte          | 4 Byte                |
+-------------------+------------------+------------------+---------------+-------------------------+-----------------------+
```

* **İletişim Türleri:**
    * *Unicast (Teke Tek):* Tek bir kaynaktan tek bir hedef MAC adresine giden trafik.
    * *Multicast (Gruba Yayın):* Belirli bir gruba üye cihazlara giden trafik (Örn: `01:00:5E` ile başlayan IPv4 multicast MAC adresleri).
    * *Broadcast (Herkese Yayın):* Yerel ağdaki tüm cihazlara giden trafik (Hedef MAC: `FF:FF:FF:FF:FF:FF`).

### 3. Anahtarlama (Switching) Altyapısı

Switch (anahtar) cihazları, Katman 2 seviyesinde çalışır. Hub'lar gibi gelen sinyali tüm portlara çoğaltmak yerine, hedef cihazın bağlı olduğu portu tespit ederek trafiği sadece o porta yönlendirir.
* **MAC Adres Tablosu (CAM Table):** Switch'in hangi fiziksel portunda hangi MAC adresine sahip cihazın bağlı olduğunu tuttuğu hızlı bellektir.
* **Sanal ve Modern Arayüzler (Linux Bridge & veth):**
    * *Linux Bridge:* Linux çekirdeğinde çalışan yazılımsal bir switch'tir. Container'ların sanal portlarını birbirine bağlar.
    * *veth (Virtual Ethernet) Pair:* Çift taraflı sanal ağ kablosudur. Bir ucuna yazılan paket, diğer ucundan aynen çıkar. Bir ucu container ağ alanında (network namespace) `eth0` olurken, diğer ucu host üzerindeki Linux Bridge'e bağlanır.

---

## 6. Adım Adım Teknik Akış

Bir yerel ağda, Sunucu A'nın (IP: `192.168.1.10`, MAC: `AA:AA:AA:AA:AA:AA`), Sunucu B'ye (IP: `192.168.1.20`, MAC: `BB:BB:BB:BB:BB:BB`) ilk kez veri göndermesi durumundaki teknik akış şu şekildedir:

```text
Sunucu A                       Switch                        Sunucu B
   |                             |                              |
   |--- 1. ARP Request --------->|                              |
   |    (Broadcast)              |--- 2. ARP Request ---------->|
   |                             |    (Broadcast)               |
   |                             |                              |
   |                             |<-- 3. ARP Reply -------------|
   |<-- 4. ARP Reply ------------|    (Unicast)                 |
   |    (Unicast)                |                              |
   |                             |                              |
   |--- 5. Unicast Data -------->|                              |
   |    (Hedef MAC: BB)          |--- 6. Unicast Data --------->|
   v                             v    (Sadece Port B'ye)        v
```

1. **Paket Hazırlığı ve ARP Kontrolü:** Sunucu A, IP katmanında hedef IP'yi `192.168.1.20` olarak belirler ancak veri bağı katmanında çerçeveyi kapsüllemek için Sunucu B'nin MAC adresini bilmemektedir. Kendi ARP tablosunu (ARP Cache) kontrol eder; eğer kayıt bulamazsa süreci başlatır.
2. **ARP İstek Yayını (ARP Request):** Sunucu A, hedef MAC alanına `FF:FF:FF:FF:FF:FF` (Broadcast) yazarak "Kimde `192.168.1.20` IP'si varsa bana MAC adresini söylesin" sorusunu içeren bir ARP çerçevesi gönderir.
3. **Switch'in MAC Öğrenmesi:** Broadcast çerçevesi switch'in 1. portundan girer. Switch, gelen çerçevenin kaynak MAC adresine bakar (`AA:AA:AA:AA:AA:AA`) ve CAM tablosuna yazar: `Port 1 -> AA:AA:AA:AA:AA:AA`.
4. **Broadcast Flooding:** Switch, hedef MAC adresi broadcast olduğu için çerçeveyi geldiği port (Port 1) hariç tüm aktif portlarına kopyalar (Flooding).
5. **ARP İşleme:** Yerel ağdaki tüm cihazlar bu çerçeveyi alır. IP adresi uyuşmayan cihazlar paketi çöpe atar (drop). Sunucu B ise kendi IP'si ile eşleştiği için isteği işler ve Sunucu A'nın MAC adresini kendi ARP tablosuna kaydeder.
6. **ARP Yanıtı (ARP Reply):** Sunucu B, doğrudan Sunucu A'yı hedefleyen (Unicast - Hedef MAC: `AA:AA:AA:AA:AA:AA`) bir ARP yanıt çerçevesi üretir ve switch'in 2. portundan gönderir.
7. **Switch'in İkinci Öğrenmesi ve Yönlendirmesi:** Switch, 2. porttan gelen çerçevenin kaynak MAC'ine bakar (`BB:BB:BB:BB:BB:BB`) ve CAM tablosuna ekler: `Port 2 -> BB:BB:BB:BB:BB:BB`. Ardından hedef MAC olan `AA:AA:AA:AA:AA:AA` adresini CAM tablosunda arar. Bu adresin 1. portta bağlı olduğunu bildiği için çerçeveyi **sadece** 1. porta iletir (Forwarding).
8. **Veri İletimi:** Sunucu A, Sunucu B'nin MAC adresini öğrendikten sonra asıl verisini (Örn: TCP/IP paketi) içeren çerçeveyi doğrudan hedef MAC `BB:BB:BB:BB:BB:BB` olarak hazırlar ve gönderir. Switch bu çerçeveyi unknown unicast flooding yapmadan, doğrudan Port 2'ye iletir.

---

## 7. Gerçek Dünya Kullanım Senaryoları

### Senaryo 1: Veri Merkezi Yedeklilik Hattı (Data Center Interconnect - DCI)
* **Problem:** İki farklı coğrafi veri merkezinde çalışan veritabanı replikasyon sunucularının (aktif-aktif küme) aynı Layer 2 yayın alanında (L2 Broadcast Domain) olması gerekmektedir. Ancak aradaki fiziksel mesafe büyüktür ve arada Layer 3 yönlendiriciler bulunmaktadır.
* **Yaklaşım:** WAN üzerinden VXLAN (Virtual Extensible LAN) gibi tünelleme teknolojileri kullanılarak Layer 2 ağ sanal olarak uzatılır. Sunucular aradaki mesafeyi ve L3 yönlendiricileri hissetmeden, sanki aynı fiziksel switch'e bağlıymış gibi doğrudan MAC adresleriyle haberleşir.
* **Elde Edilen Fayda:** IP adresi değişikliği yapmadan veritabanı cluster'larının coğrafi olarak yedekli çalışması sağlanır.
* **Yeni Oluşan Karmaşıklık / Güvenlik Gereksinimi:** L2 ağın uzatılması, bir veri merkezinde oluşabilecek bir broadcast fırtınasının (broadcast storm) diğer veri merkezine de sıçrayarak tüm altyapıyı çökertmesi riskini (failure domain genişlemesi) beraberinde getirir. Tünel uç noktalarında (VTEP) sıkı hız sınırlandırmaları (storm control) uygulanmalıdır.

### Senaryo 2: Yüksek Performanslı Depolama Ağları (SAN - Storage Area Network)
* **Problem:** Kubernetes cluster'ında koşan stateful podlar, host üzerindeki NFS veya iSCSI depolama alanlarına büyük miktarda veri yazmaktadır. Standart 1500 byte MTU kullanıldığında yüksek paket başlığı (overhead) ve CPU kesme (interrupt) yükü oluşmakta, bu da yazma performansını düşürmektedir.
* **Yaklaşım:** Depolama ağındaki tüm NIC, Switch ve Storage arayüzlerinde **Jumbo Frames** (9000 MTU) aktif edilir.
* **Elde Edilen Fayda:** Tek seferde taşınan veri boyutu 6 katına çıkar. Paket sayısı azaldığı için CPU'nun paket işleme yükü düşer ve disk yazma throughput değeri ciddi oranda artar.
* **Yeni Oluşan Karmaşıklık / Operasyonel Gereksinim:** Yol üzerindeki tek bir cihaz bile (örneğin bir ara switch) 9000 MTU desteklemiyorsa paketler bölünemediği için sessizce düşürülür (**MTU Mismatch**). Tüm ağ bileşenlerinin uçtan uca aynı MTU değerine sahip olması ve bunun otomatik testlerle doğrulanması gerekir.

---

## 8. Güvenlik Perspektifi

### MAC Spoofing (MAC Sahtekarlığı)

* **Sorun:** Saldırganın kendi ağ kartının MAC adresini, ağdaki yetkili başka bir cihazın (örneğin varsayılan ağ geçidinin) MAC adresiyle değiştirmesi.
* **Genel Çalışma Mantığı:** Linux üzerinde `ip link set` komutu gibi yazılımsal araçlarla NIC'in fiziksel adresi işletim sistemi düzeyinde kolayca manipüle edilir. Saldırgan bu sahte MAC ile ağa paket gönderdiğinde switch'in CAM tablosundaki kayıt güncellenir ve hedef cihaza gitmesi gereken paketler saldırgana yönlendirilir.
* **Olası Etki:** Saldırgan, hedef cihaza gitmesi gereken Katman 2 çerçevelerini kendi üzerine çekebilir, ağda kimlik taklidi yapabilir ve ağ erişim engellerini (MAC Filtering) aşabilir.
* **Tespit:** Ağ izleme araçlarında aynı MAC adresinin farklı switch portlarından veya IP adreslerinden görünmesi (MAC flapping logları) ile tespit edilir.
* **Savunma:** Switch portlarında **Port Security** konsepti uygulanmalı, port başına izin verilen maksimum MAC adresi sayısı sınırlandırılmalı ve statik/dinamik adres eşleşmeleri kısıtlanmalıdır.
* **Sınırlamalar:** Port Security dinamik ve çok fazla cihazın bağlandığı ofis ortamlarında yüksek yönetim yükü oluşturur.

### MAC Flooding & CAM Exhaustion (CAM Tablosu Tüketme)

* **Sorun:** Switch'in kısıtlı bellek alanı olan CAM tablosunun sahte verilerle doldurularak işlevsiz hale getirilmesi.
* **Genel Çalışma Mantığı:** Saldırgan, `macof` gibi sızma testi araçlarıyla ağa saniyede binlerce rastgele sahte kaynak MAC adresine sahip ethernet çerçeveleri gönderir.
* **Olası Etki:** Switch'in CAM tablosu tamamen dolar. Yeni ve gerçek cihazların MAC adreslerini kaydedecek yer kalmaz. Bu durumda switch, koruma mekanizması gereği bir hub gibi davranmaya başlar ve gelen tüm tekli (unicast) paketleri tüm portlarına kopyalar (**Unknown Unicast Flooding**). Saldırgan, ağdaki tüm trafiği (şifresiz parolaları, oturumları) sniffing (izleme) araçlarıyla kolayca ele geçirebilir.
* **Tespit:** Switch üzerinde `show mac address-table count` komutuyla tablo doluluğunun ve saniyede değişen adres sayısının anormal artışının izlenmesi.
* **Savunma:** Port Security ile her porttan öğrenilebilecek maksimum MAC adresi sayısı (Örn: Port başına maksimum 2 MAC) sınırlandırılmalı ve ihlal durumunda portun kapatılması (Shutdown modu) sağlanmalıdır.
* **Sınırlamalar:** Port Security tek başına IP seviyesindeki spoofing saldırılarını (Örn: ARP Poisoning) engellemez; bu durumlar için DAI (Dynamic ARP Inspection) gibi ek mekanizmalar gerekir.

---

## 9. Hardening ve Best Practices

Yerel ağ seviyesinde güvenliği ve kararlılığı artırmak için uygulanması önerilen yöntemler:

| Uygulama | Neden Önemli? | Uygulama Alanı | Sınırlama / Trade-off |
| :--- | :--- | :--- | :--- |
| **Port Security (Shutdown Modu)** | Yetkisiz bir MAC adresi bağlandığında veya CAM flooding denendiğinde ilgili switch portunu kapatarak (`err-disable` durumuna sokarak) ağa fiziksel sızmaları engeller. | Kullanıcı erişim portları (Edge/Access Ports). | Yanlışlıkla cihaz değiştiren personelin portunun kapanmasına ve yüksek destek taleplerine yol açar. |
| **IEEE 802.1X (NAC - Network Access Control)** | Cihazların ağa fiziksel olarak bağlandıklarında merkezi bir RADIUS/TACACS+ sunucusu üzerinden kimlik doğrulaması yapmasını zorunlu kılar. | Kurumsal ofis ağları, kablolu ve kablosuz erişim noktaları. | Yüksek lisans maliyeti, sertifika yönetimi ve istemci (supplicant) yapılandırma karmaşıklığı. |
| **VLAN Segmentasyonu** | Ağdaki yayın alanlarını (broadcast domain) mantıksal olarak bölerek bir segmentteki güvenlik açığının diğer segmentlere yayılmasını (lateral movement) engeller. | Tüm kurumsal ve veri merkezi ağları. | VLAN'ler arası iletişim için router veya L3 switch ihtiyacı doğurur, gecikmeyi minimal düzeyde artırabilir. |
| **Kullanılmayan Portların Kapatılması** | Boşta duran fiziksel switch portlarının saldırganlar tarafından doğrudan kullanılmasını engeller. | Fiziksel switch dolapları ve ofis içi aktif olmayan duvar prizleri. | Yeni bir cihaz bağlanmak istendiğinde ağ ekibinin manuel müdahalesini gerektirir (operasyonel yük). |

---

## 10. Avantajlar, Dezavantajlar ve Trade-off’lar

### Standart Frames (1500 MTU) vs Jumbo Frames (9000 MTU)

* **Standart Frames (1500 MTU):**
    * *Avantaj:* Evrensel uyumluluk. İnternetteki ve yerel ağlardaki hemen hemen her cihaz bu değeri destekler. MTU uyuşmazlığına bağlı paket düşmeleri yaşanmaz.
    * *Dezavantaj:* Büyük veri transferlerinde (yedekleme, depolama) saniyede işlenen paket sayısı çok yüksek olur, bu da sunucu CPU'sunda yüksek kesme (interrupt) yükü oluşturur.
* **Jumbo Frames (9000 MTU):**
    * *Avantaj:* Paket başlığı (overhead) oranı düşer. CPU'nun paket işleme yükü azalır ve veri aktarım hızı (throughput) ciddi oranda artar.
    * *Dezavantaj:* Yol üzerindeki tek bir cihaz bile 9000 MTU desteklemiyorsa paketler bölünemediği (Katman 2'de parçalama yoktur) için sessizce düşürülür (**MTU Mismatch**). Teşhis edilmesi son derece zordur.

---

## 11. Yaygın Yanlış Anlamalar

### Yanlış Anlama 1

**Yanlış düşünce:**
Bir ağ kartının MAC adresi fiziksel olarak değiştirilemez, donanıma kalıcı olarak gömülüdür ve taklit edilmesi imkansızdır.

**Doğru yaklaşım:**
MAC adresi fabrikada ağ kartının ROM belleğine yazılır (BIA - Burned-In Address). Ancak işletim sistemi ağ arayüzünü başlattığında bu adresi RAM'e kopyalar. İşletim sistemi seviyesinde RAM'deki bu değer saniyeler içinde yazılımsal olarak değiştirilebilir.

**Gerçek sistemdeki etkisi:**
Sadece MAC adresine güvenerek yapılan MAC Filtreleme (MAC Filtering) gibi erişim kontrolleri güvenlik açısından yetersizdir ve saldırganlar tarafından kolayca aşılabilir.

### Yanlış Anlama 2

**Yanlış düşünce:**
Switch'ler ağ trafiğini port bazlı izole ettiği için yerel (Katman 2) bir ağda paket dinleme (sniffing) saldırısı yapılamaz.

**Doğru yaklaşım:**
Saldırgan CAM tablosunu doldurarak (CAM Flooding) switch'i hub moduna geçmeye zorlayabilir veya ARP Poisoning (ARP zehirlenmesi) ile trafiği kendi üzerine yönlendirebilir.

**Gerçek sistemdeki etkisi:**
Yerel ağda şifresiz iletilen tüm trafik (HTTP, FTP, Telnet vb.) aynı ağdaki yetkisiz bir kullanıcı tarafından ele geçirilebilir. Bu nedenle yerel ağda bile uçtan uca şifreleme (TLS, IPsec) zorunludur.

---

## 12. Troubleshooting ve Validation

Katman 1 ve Katman 2 sorunlarını teşhis ederken izlenmesi gereken sistematik akış:

```text
Sorun Belirtisi (Paket Kaybı / Bağlantı Yok)
   |
   +--> 1. Fiziksel Katman Kontrolü (Kablo, Sinyal, Link Işığı)
   |       Komut: "ethtool <interface>" (Link detected: yes/no)
   |
   +--> 2. Arayüz Hata Sayaçlarının Analizi
   |       Komut: "ip -s link show <interface>" (RX/TX errors, dropped, collisions)
   |
   +--> 3. MTU ve Parçalanamayan Paket Testi
   |       Komut: "ping -M do -s 8972 <hedef_IP>" (MTU uyuşmazlığı tespiti)
   v
Çözümün Uygulanması ve Doğrulanması
```

### Linux Sistemlerinde Kullanılan Teşhis Komutları

#### 1. `ip link` ve `ip -s link`
Arayüzlerin donanımsal durumunu, MTU değerlerini ve paket istatistiklerini gösterir.

```bash
# Arayüz durumunu ve MTU değerini sorgulama
ip link show eth0
```

* **Beklenen Çıktı:**
    ```text
    2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
        link/ether 00:15:5d:01:ca:12 brd ff:ff:ff:ff:ff:ff
    ```
    * `UP`: Yazılımsal olarak arayüzün etkin olduğunu gösterir.
    * `LOWER_UP`: Fiziksel olarak kablonun takılı olduğunu ve karşı taraftan sinyal alındığını (Katman 1 çalışıyor) teyit eder.

```bash
# Hata sayaçlarını detaylı listeleme
ip -s link show eth0
```

* **Beklenen Çıktı:**
    ```text
    2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 ...
        RX:  bytes packets errors dropped overrun mcast
        104857600  1250000      0       0       0   120
        TX:  bytes packets errors dropped carrier collsns
         52428800   625000     12       0       0   450
    ```
    * `errors`: Donanımsal sinyal bozuklukları veya CRC hatalarını gösterir (Genellikle kablo/port arızası).
    * `dropped`: Çekirdek (kernel) veya ağ kartı tampon belleği (buffer) dolu olduğu için işlenemeden atılan paket sayısını belirtir. Sunucunun aşırı yüklendiğinin birincil göstergesidir.
    * `collsns`: Çarpışma sayısı. Eğer ağ Full-Duplex ise bu değer kesinlikle **0** olmalıdır. 0'dan büyükse fiziksel hatta bir **Duplex Mismatch** (Duplex Uyuşmazlığı) hatvası vardır.

#### 2. `ethtool`
Ağ arabirim kartının (NIC) donanımsal yeteneklerini, hızını ve bağlantı durumunu sorgular.

```bash
sudo ethtool eth0
```

* **Beklenen Çıktı:**
    ```text
    Settings for eth0:
        Supported ports: [ TP ]
        Speed: 1000Mb/s
        Duplex: Full
        Auto-negotiation: on
        Link detected: yes
    ```
    * `Link detected: yes`: Fiziksel olarak sinyalin ulaştığını gösterir. `no` ise kablo takılı değildir veya karşı taraftaki switch portu kapalıdır.

#### 3. MTU Sınırlarını Test Etmek (`ping` parametreleri)
Ağ yolundaki MTU uyuşmazlıklarını tespit etmek için paketin bölünmesini engelleyerek hedef cihaza büyük paketler gönderilir.

```bash
# Linux için: -M do (parçalama), -s (veri boyutu - 28 byte IP+ICMP başlığı düşülmelidir, 9000 - 28 = 8972)
ping -M do -s 8972 192.168.1.20
```

* **Sorun Durumunda Çıktı:**
    ```text
    ping: local error: Message too long, mtu=1500
    ```
    *Bu çıktı, yol üzerindeki bir cihazın veya yerel arayüzün bu boyuttaki paketi parçalamadan geçiremeyeceğini kanıtlar.*

---

## 13. Uygulamalı Doğrulama

### Amaç
Linux üzerinde sanal bir switch (bridge) oluşturmak, bu switch'e sanal ethernet kabloları (veth pair) bağlamak, trafik üreterek Katman 2 seviyesindeki MAC öğrenme sürecini `tcpdump` ve `bridge fdb` araçlarıyla doğrulamaktır.

### Gereksinimler
* Herhangi bir modern Linux dağıtımı (Ubuntu, Debian, RHEL vb.).
* `sudo` yetkileri.
* `iproute2` (varsayılan gelir) ve `tcpdump` araçları.

### Adım Adım Uygulama

#### Adım 1: Sanal Switch (Bridge) Oluşturulması ve Aktifleştirilmesi
```bash
sudo ip link add br-lab type bridge
sudo ip link set dev br-lab up
```

#### Adım 2: Sanal Cihaz Bağlantılarının (veth pairs) Oluşturulması
Sistem üzerinde iki farklı bağımsız cihazı simüle edecek sanal arayüz çiftleri tanımlanır.
```bash
# Cihaz A için sanal kablo (Host ucu: veth_brA, Cihaz ucu: veth_nsA)
sudo ip link add veth_nsA type veth peer name veth_brA

# Cihaz B için sanal kablo (Host ucu: veth_brB, Cihaz ucu: veth_nsB)
sudo ip link add veth_nsB type veth peer name veth_brB
```

#### Adım 3: Sanal Kabloların Switch Portlarına Bağlanması
```bash
sudo ip link set veth_brA master br-lab
sudo ip link set veth_brB master br-lab

# Tüm arayüzlerin up duruma getirilmesi
sudo ip link set veth_brA up
sudo ip link set veth_brB up
sudo ip link set veth_nsA up
sudo ip link set veth_nsB up
```

#### Adım 4: IP Adreslerinin Tanımlanması
```bash
sudo ip addr add 10.10.10.11/24 dev veth_nsA
sudo ip addr add 10.10.10.12/24 dev veth_nsB
```

#### Adım 5: MAC Adres Tablosunun (FDB) İlk Durumu
Sanal switch'in MAC tablosu sorgulanır:
```bash
bridge fdb show dev br-lab | grep -v self
```
*Henüz hiçbir trafik akmadığı için bu komut çıktı üretmeyecektir. Switch henüz hiçbir MAC adresi öğrenmemiştir.*

#### Adım 6: Trafik İzleme (tcpdump)
İkinci bir terminal ekranı açarak sanal switch üzerindeki Katman 2 (Ethernet) başlıklarını dinlemeye başlayın:
```bash
sudo tcpdump -e -i br-lab
```
* `-e`: Ethernet başlıklarını (Kaynak ve Hedef MAC adreslerini) görmemizi sağlar.

#### Adım 7: Trafik Üretilmesi ve MAC Öğreniminin Tetiklenmesi
İlk terminalden ping işlemini başlatın:
```bash
ping -I veth_nsA 10.10.10.12 -c 2
```

#### Adım 8: Sonuçların Doğrulanması
Yeniden MAC tablosunu sorgulayın:
```bash
bridge fdb show dev br-lab | grep -v self
```

* **Beklenen Çıktı:**
    ```text
    00:15:5d:01:ca:aa dev veth_brA dst 10.10.10.11
    00:15:5d:01:ca:bb dev veth_brB dst 10.10.10.12
    ```
* `tcpdump` ekranında ilk paketin (ARP Request) hedefinin `broadcast` olduğunu, ancak öğrenme gerçekleştikten sonra asıl ping (ICMP) paketlerinin doğrudan kaynak ve hedef MAC adresleri arasında aktığını (Unicast) görebilirsiniz.

#### Adım 9: Temizlik
Oluşturulan tüm sanal kaynaklar kaldırılarak sistem eski haline getirilir:
```bash
sudo ip link delete br-lab
sudo ip link delete veth_nsA
sudo ip link delete veth_nsB
```

#### Güvenlik Notu
Bu laboratuvar çalışması sadece yerel Linux çekirdeği üzerinde sanal kaynaklar üretmektedir. Üretim ortamında (production) çalışan canlı ağ kartları veya servisler üzerinde doğrudan uygulanmamalıdır.

---

## 14. Production Ortamı Perspektifi

* **Yedeklilik ve Yüksek Erişilebilirlik (High Availability):** Üretim ortamlarında tek bir fiziksel ağ kartı veya kablo arızası tüm sunucunun erişilmez olmasına neden olur. Bu riski azaltmak için **NIC Teaming / Bonding** (LACP - Link Aggregation Control Protocol) kullanılarak iki veya daha fazla fiziksel ağ kartı tek bir mantıksal hat olarak switch'e bağlanır.
* **Gözlemlenebilirlik (Observability):** SRE ekipleri, Prometheus ve Grafana entegrasyonu sağlayan `node_exporter` vasıtasıyla sunucuların ağ kartı hata sayaçlarını (`node_network_receive_errors_total`, `node_network_transmit_drop_total`) sürekli izler ve belirli bir eşik aşıldığında (Örn: dropped paketin toplam paketlere oranı %0.1'i geçtiğinde) otomatik alarm üretir.
* **Bulut Altyapısı Farkları:** AWS, Azure veya Google Cloud gibi genel bulut sağlayıcılarda klasik Katman 2 broadcast paketleri engellenmiştir. Bulut sağlayıcılar Katman 2'yi arka planda kendi SDN (Software Defined Networking) katmanlarında kapsülleyerek (genellikle VXLAN veya Geneve ile) taklit ederler. Bu nedenle bulut ortamlarında klasik ARP tabanlı saldırılar çalışmaz.

---

## 15. Diğer Teknolojilerle İlişkisi

* **ARP (Address Resolution Protocol):** Bu katmanda öğrenilen MAC adresleri ile bir sonraki katmanda (Network Layer) işlenecek olan IP adresleri arasındaki köprüdür.
* **VLAN (IEEE 802.1Q):** Katman 2 seviyesinde tek bir fiziksel switch'i mantıksal olarak birden fazla sanal switch'e bölerek güvenlik ve performans sınırları oluşturur.
* **Kubernetes CNI (Container Network Interface):** Kubernetes üzerindeki Pod'ların haberleşebilmesi için arka planda Linux bridge, veth pair ve MACVLAN gibi Katman 2 teknolojilerini otomatik olarak yönetir.

---

## 16. Junior Seviyede Bilinmesi Gerekenler

Junior bir mühendis adayının bu konuda bilmesi ve açıklayabilmesi gereken temel noktalar:

* [ ] OSI ve TCP/IP modelleri arasındaki temel farkları ve katman eşleşmelerini açıklayabilmeli.
* [ ] Kapsülleme (encapsulation) sürecini ve PDU (Segment, Paket, Çerçeve) kavramlarını bilmeli.
* [ ] Bir ağ kartının (NIC) MAC adresinin ne olduğunu, OUI'nin neyi temsil ettiğini söyleyebilmeli.
* [ ] Unicast, Broadcast ve Multicast iletişim türleri arasındaki farkı net bir şekilde çizebilmeli.
* [ ] Switch'lerin CAM tablosunu nasıl dinamik olarak öğrendiğini ve "unknown unicast flooding" mantığını açıklayabilmeli.
* [ ] Linux üzerinde `ip link` ve `ethtool` komutlarını kullanarak ağ arayüzünün fiziksel durumunu ve MTU değerini sorgulayabilmeli.
* [ ] MTU uyuşmazlığının (MTU Mismatch) ne olduğunu ve büyük paketlerin neden sessizce düşebileceğini bilmeli.

---

## 17. Teknik Mülakat Soruları

### Soru 1: Bir switch, hedef MAC adresini CAM tablosunda bulamadığında ne yapar? Bu durumun hub çalışma mantığından farkı nedir?
**Cevap:** Switch, gelen çerçeveyi (frame), girdiği port hariç tüm aktif portlarına kopyalar. Buna **Unknown Unicast Flooding** denir. Hedef cihaz bu çerçeveye yanıt verdiğinde, switch onun kaynak MAC adresini ve portunu tablosuna kaydeder ve sonraki iletişimleri doğrudan tek bir porta iletir. Hub'lar ise bunu geçici bir durum olarak değil, her zaman yaparlar; asla MAC adresi öğrenmezler.

### Soru 2: Katman Full-Duplex olarak ayarlanmış olmasına rağmen arayüz istatistiklerinde "collisions" (çarpışma) sayacının arttığını görüyorsanız neyden şüphelenirsiniz?
**Cevap:** Kesinlikle bir **Duplex Mismatch** (Duplex Uyuşmazlığı) hatasından şüphelenirim. Bağlantının bir ucu Full-Duplex çalışırken diğer ucu Half-Duplex olarak kalmıştır. Half-Duplex tarafı çarpışma algıladığı için paketi yeniden göndermeye çalışırken, Full-Duplex tarafı çarpışma kontrolü yapmadan sürekli veri göndermeye devam eder. Bu durum hat üzerinde yüksek paket kaybı ve performans düşüşüne sebep olur.

### Soru 3: Standard Ethernet MTU değeri 1500 byte iken, 9000 byte (Jumbo Frame) gönderen bir sunucunun paketi yol üzerindeki 1500 MTU destekli bir switch'e gelirse ne olur?
**Cevap:** Paket switch tarafından doğrudan drop edilir (çöpe atılır). Çünkü Katman 2 seviyesinde paket parçalama (fragmentation) mekanizması yoktur. Parçalama Katman 3'te (IP katmanında) router'lar tarafından yapılabilir. L2 switch bu devasa çerçeveyi (oversized frame) taşıyamayacağı için sessizce düşürür.

### Soru 4: Linux sistemlerde "dropped" ağ istatistiği ne anlama gelir ve hangi durumlarda artar?
**Cevap:** "Dropped" sayısı, ağ kartına veya işletim sistemi çekirdeğine (kernel) gelen paketlerin donanım/yazılım tampon belleklerinin (buffer) dolması sebebiyle işlenemeden silindiğini gösterir. Genellikle ani trafik yoğunluklarında (microbursts), CPU'nun ağ kesmelerini (interrupts) işlemekte yetersiz kaldığı durumlarda veya sistem kaynaklarının tükendiği anlarda artar.

### Soru 5: MAC Flooding saldırısı switch'in güvenlik duvarını (isolation) nasıl aşar? Saldırgan bundan nasıl faydalanır?
**Cevap:** Saldırgan, saniyede binlerce sahte kaynak MAC adresi üreterek switch'in kısıtlı CAM tablosunu doldurur. Boş yer kalmadığında switch koruma moduna geçerek tüm gelen tekli paketleri tüm portlara yayınlamaya (flood) başlar. Switch adeta bir hub gibi davranır. Böylece saldırgan, kendi portuna normalde gelmemesi gereken diğer sunucuların trafiğini pasif olarak dinleyebilir.

### Soru 6: AWS veya Google Cloud gibi cloud platformlarında neden klasik ARP Poisoning saldırıları çalışmaz?
**Cevap:** Bulut sağlayıcıların sanal ağ altyapıları (VPC) klasik fiziksel Katman 2 protokollerini doğrudan çalıştırmaz. Arka planda çalışan yazılım tanımlı ağlar (SDN), broadcast ve multicast paketlerini filtreler veya doğrudan engeller. Adres eşleşmeleri (IP-MAC) doğrudan bulut kontrol düzlemi (control plane) tarafından statik olarak yönetildiği için manipüle edilebilecek dinamik bir ARP süreci mevcut değildir.

### Soru 7: Bir Linux sunucusunda `ethtool` çıktısında `Link detected: no` görüyorsanız bu durum hangi katmandaki bir soruna işaret eder ve ilk kontrol etmeniz gereken şey nedir?
**Cevap:** Bu durum **Katman 1 (Fiziksel Katman)** seviyesinde bir soruna işaret eder. Ağ kartı fiziksel olarak bir sinyal/elektrik alamamaktadır. İlk olarak fiziksel kablonun takılı olup olmadığı, kablonun hasar görüp görmediği ve bağlı olduğu switch portunun aktif (up) durumda olup olmadığı kontrol edilmelidir.

### Soru 8: LACP (Link Aggregation Control Protocol) nedir ve hangi katmanda çalışır?
**Cevap:** LACP, birden fazla fiziksel ethernet bağlantısını birleştirerek tek bir mantıksal bağlantı oluşturmayı sağlayan bir protokoldür. **Katman 2 (Veri Bağı Katmanı)** seviyesinde çalışır. Hem bant genişliğini artırır hem de fiziksel hatlardan biri koptuğunda trafiğin kesintisiz devam etmesini sağlayarak yedeklilik sunar.

---

## 18. Kısa Özet

* OSI Modeli teorik standartları belirler; TCP/IP ise pratik olarak internetin üzerinde çalıştığı modeldir.
* Fiziksel katmanda (L1) dijital veri; bakır, fiber veya hava gibi ortamlarda taşınabilecek sinyallere dönüştürülür.
* Veri Bağı katmanında (L2) verinin birimi **Çerçeve (Frame)**'dir ve adresleme fiziksel **MAC Adresleri** ile yapılır.
* Ethernet çerçevesinin sonunda yer alan FCS alanı, çerçevenin yolda bozulup bozulmadığını kontrol eden CRC hata denetim kodunu taşır.
* Switch'ler, gelen paketlerin kaynak MAC adreslerini dinleyerek port-MAC eşleşmelerini dinamik olarak CAM tablosuna kaydeder.
* Hedef MAC adresi CAM tablosunda yoksa switch çerçeveyi tüm portlara kopyalar (Unknown Unicast Flooding).
* SRE ve sistem yöneticileri, ağ performans sorunlarını teşhis etmek için `ip -s link` ve `ethtool` hata sayaçlarını (errors, dropped, collisions) izlemelidir.
* Jumbo Frames (9000 MTU) veri merkezlerinde CPU yükünü düşürür ancak tüm ağda uçtan uca yapılandırılmadığında sessiz paket düşmelerine (MTU Mismatch) neden olur.
* Sanal ağlarda Linux Bridge yazılımsal switch görevini görürken, veth pair yapıları container'ları bu switch'e bağlayan çift taraflı sanal kablolardır.
* L2 ağlar varsayılan olarak doğrulanmamış güven esasına dayandığı için MAC Spoofing ve CAM Flooding gibi saldırılara açıktır. Port Security ve 802.1X ile sıkılaştırılmalıdır (hardening).

---

## 19. Kaynaklar

1. [RFC 826 - An Ethernet Address Resolution Protocol](https://datatracker.ietf.org/doc/html/rfc826) — Internet Engineering Task Force (IETF)
2. [IEEE 802.3 Ethernet Working Group](https://ieee802.org/3/) — Institute of Electrical and Electronics Engineers (IEEE)
3. [Linux Kernel Networking Documentation](https://www.kernel.org/doc/html/latest/networking/index.html) — kernel.org Official Docs
4. [ethtool Manual Page](https://man7.org/linux/man-pages/man8/ethtool.8.html) — Linux Man Pages
