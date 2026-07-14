# 🌐 Ağ Temelleri ve Platform Mühendisliği / Bilgi Deposu

Ağ dünyasında sadece ezberlemek yetmez; sistemlerin, donanımların ve protokollerin arkasındaki gerçek mühendislik felsefesini anlamak gerekir. Bu doküman, modern bulut sistemlerini yönetebilmek için bilinmesi gereken temel ağ konseptlerini derinlemesine açıklar.

---

## 🏛️ Bölüm 1: OSI vs. TCP/IP Modellerinin Gerçek Felsefesi

Ağ dünyasına adım atan herkes bu iki modeli duyar ama genellikle katman isimlerini ezberleyip geçer. İşin asıl mantığı **Soyutlama (Abstraction)** ve **Standardizasyon** konseptidir.

> 💡 **Bir Senaryo Düşünelim:**
> Elinde Apple bir bilgisayar var, bağlı olduğun switch Cisco, bağlandığın sunucu ise Linux işletim sistemli bir Dell. Bu cihazların işlemci mimarileri, işletim sistemleri ve anakartları tamamen farklıdır. Eğer ortak bir "anayasa" olmasaydı; Apple cihazının kabloya bastığı elektrik sinyalini Cisco switch anlayamaz, Linux sunucu ise bu veriyi açamazdı.

| Model | Geliştirilme Amacı & Durumu | Katman Sayısı |
| :--- | :--- | :---: |
| **OSI (Open Systems Interconnection)** | 1984 yılında ISO tarafından geliştirilmiş teorik bir referans modelidir. İşin "idealde nasıl olması gerektiğini" açıklar. | 7 Katman |
| **TCP/IP** | Bugün kullandığımız internetin üzerine kurulduğu pratik protokol kümesidir. Sahada savaşı kazanmıştır. | 4 Katman |

> **Altın Kural:** Biz mühendislik derinliği için anlatımı OSI üzerinden aşağıdan yukarıya doğru yapacağız; çünkü **donanımı anlamadan bulutu yönetemezsin.**

---

## ⚡ Bölüm 2: 1. Katman - Fiziksel Katman (Physical Layer)

Burası ağın "et ve kemik" olan kısmıdır. Bu katmanda yazılım, işletim sistemi, IP veya veri diye bir şey yoktur. Sadece **Bitler (0 ve 1)** ve bu bitlerin taşınmasını sağlayan **Sinyaller** vardır.

* **Görevi:** Üst katmandan gelen dijital 0 ve 1 dizilimini, fiziksel ortama uygun sinyallere dönüştürüp kabloya fırlatmak; karşı tarafta ise o sinyalleri yakalayıp yeniden 0 ve 1'e çevirmek.

### 🔌 Sinyal Türleri ve Fiziksel Ortamlar

* **Bakır Kablolar (UTP/STP - Ethernet):** Veriyi **Elektrik Voltajı** ile taşır. Örneğin: `+5 Volt` elektrik akımı varsa "1", `0 Volt` varsa "0" demektir.
* **Fiber Optik Kablolar (Single-Mode / Multi-Mode):** Veriyi **Işık Çakmaları (Lazer veya LED)** ile taşır. Işık var = 1, Işık yok = 0. Sinyal kaybı (attenuation) bakıra göre neredeyse sıfırdır, bu yüzden veri merkezlerinin omurgaları tamamen fiberdir.
* **Kablosuz Ağlar (Wi-Fi / Radyo):** Veriyi **Radyo Frekansları** ve elektromanyetik dalgaların yönünü/frekansını değiştirerek (Modülasyon) taşır.

### ⚙️ Platform Mühendisi İçin Kritik Donanım Dinamikleri

* **Duplex Modları (Half vs. Full Duplex):**
    * *Half Duplex:* Aynı anda sadece bir taraf konuşabilir (Telsiz mantığı). Biri veri gönderirken diğeri beklemek zorundadır. Aynı anda basarlarsa *Collision (Çatışma)* olur.
    * *Full Duplex:* Aynı anda hem veri gönderilebilir hem alınabilir (Telefon mantığı). Modern switch ve sunucular tamamen Full Duplex çalışır.
* **Donanımsal Hız Kilitlenmeleri (Auto-Negotiation):**
    Sunucu üzerindeki Ağ Kartı (NIC) ile bağlı olduğu switch portu, ağ ilk açıldığında otomatik olarak el sıkışarak hangi hızda konuşacaklarını belirler (Örn: `10Gbps Full Duplex`). Eğer sunucu tarafında bu ayar manuel olarak yanlış set edilirse veya kabloda bir ezilme varsa, sistem güvenli moda geçip hızı `100Mbps Half Duplex` seviyesine düşürebilir. 
    
    > ⚠️ **Unutma:** Yukarıda istediğin kadar optimize Kubernetes podu çalıştır, bu donanımsal dar boğazı (bottleneck) yazılımla çözemezsin.

---

## 🔗 Bölüm 3: 2. Katman - Veri Bağı Katmanı (Data Link Layer)

Fiziksel katmandan gelen o milyarlarca anlamsız 0 ve 1 yığınını alıp; anlamlı, sınırları belli paketçiklere dönüştürdüğümüz ilk mantıksal katmandır. Bu katmanda verinin adı artık **Frame (Çerçeve)** olur.

Data Link katmanı kendi içinde iki kritik alt katmana (Sublayer) ayrılır:
1.  **LLC (Logical Link Control):** Üst katmandaki Ağ Protokolü (çoğunlukla IPv4 veya IPv6) ile alt katmandaki donanım arasında bir hakemdir. Paketlerin akış kontrolünü (Flow Control) sağlar.
2.  **MAC (Media Access Control):** Fiziksel ortama (kabloya) verinin nasıl yazılacağını, kimin ne zaman konuşacağını ve paketlerin fiziksel adreslemesini yönetir.

### 🏷️ MAC Adresinin Anatomisi

MAC adresi, dünyadaki her ağ kartına (NIC) daha fabrikada üretilirken donanımsal olarak basılan, değiştirilemez, fiziksel ve benzersiz bir adrestir. **48-bit (6 Byte)** uzunluğundadır ve okunması kolay olsun diye **Onaltılık (Hexadecimal)** tabanda yazılır (Örn: `00:1A:2B:3C:4D:5E`).

* **İlk 3 Byte (24-bit) -> OUI (Organizationally Unique Identifier):** Bu kod IEEE tarafından üretici firmalara satılır. Paket analizinde ilk 3 byte'a bakarak cihazın Cisco, Apple veya Intel olduğunu kolayca anlarsın.
* **Son 3 Byte (24-bit):** Üreticinin o karta verdiği tamamen benzersiz cihaz seri numarasıdır.

> 🔔 **Altın Kural:** Aynı yerel ağda (aynı switch'e bağlı cihazlar arasında) iletişim sadece ve sadece MAC adresleri üzerinden yapılır. Switch'ler IP adresini okumaz, bilmez ve ilgilenmez.

---

## 📦 Bölüm 4: MTU (Maximum Transmission Unit) ve Büyük Paket Krizi

Platform mühendisliğinin en gizemli ve en çok can yakan konularından biri MTU'dur.

**MTU**, bir ağ arayüzünden (Network Interface) tek bir seferde gönderilebilecek en büyük Katman 3 (IP) paket boyutunu belirler. Küresel internet standardında varsayılan MTU değeri **1500 Byte**'tır. Yani bilgisayarın kabloya tek seferde en fazla 1500 byte'lık bir IP paketi koyabilir.

### 💥 IP Fragmentation (Parçalanma) Nedir?

Diyelim ki sunucun MTU değeri 1500 byte olan bir hattan 1500 byte'lık bir paket çıkardı. Ancak bu paket yoldaki bir router'dan geçerken, o router'ın bacağı **1400 byte** MTU ile yapılandırılmış olsun.

1.  Router pakete bakar: *"Bu paket benim kapımdan geçemez"* der.
2.  Eğer paket içerisinde "Beni parçalama" (`Don't Fragment - DF`) bayrağı set edilmediyse, router bu dev paketi ikiye böler (**Fragmentation**).
3.  İlk parça 1400 byte, ikinci parça ise kalan 100 byte şeklinde hedefe gönderilir.

### 🛑 Neden Tehlikeli?
* **Yüksek CPU Yükü:** Parçalanma (Fragmentation) router'ların CPU'suna inanılmaz büyük yük bindirir.
* **Bellek Tüketimi:** Alıcı sunucu tüm parçalar gelene kadar veriyi belleğinde bekletmek zorundadır.
* **Paket Kaybı Hassasiyeti:** Eğer parçalardan tek bir tanesi bile yolda düşerse, tüm paket çöp olur ve baştan gönderilmesi gerekir. Bu da ağda ciddi performans kaybı ve paket düşmesi (**Packet Drop**) demektir.
