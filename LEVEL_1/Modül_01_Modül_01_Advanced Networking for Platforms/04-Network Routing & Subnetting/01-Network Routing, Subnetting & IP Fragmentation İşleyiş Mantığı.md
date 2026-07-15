# 🌐 Modül 01 / Konu 04: Network Routing, Subnetting & IP Fragmentation


### 🏢 Analoji: İki Farklı Mahalle ve Posta Sistemi



Büyük bir şehirde yaşadığını düşün. Bu şehirde iki farklı mahalle var: 
* **Aşağı Mahalle** (`192.168.1.0/24`)
* **Yukarı Mahalle** (`192.168.2.0/24`)

* **Switch (Yerel Postacı):** Sadece kendi mahallendeki evleri bilir. Sen Aşağı Mahalle'deki komşuna mektup yazıp posta kutusuna attığında, yerel postacı (**Switch**) mektubu alır ve doğrudan komşunun kapısına götürür. Mahalleden dışarı asla çıkmaz.
* **Subnet Mask (Sınır Çizgisi):** Senin mektup yazacağın kişinin seninle aynı mahallede olup olmadığını anlamanı sağlayan mantıksal bir çizgidir. Adrese bakarsın, sınırın içindeyse mektubu yerel postacıya verirsin. Sınırın dışındaysa, mektubu mahallenin çıkışındaki ana postaneye göndermen gerektiğini anlarsın.
* **Default Gateway (Mahalle Postanesi):** Eğer mektup Yukarı Mahalle'deki birine gidecekse, yerel postacının yetkisi yetmez. Mektubu mahallenin çıkışındaki ana postaneye (**Gateway / Geçit**) teslim edersin.
* **Router (Şehirler Arası Kargo):** Mahalle postanelerini birbirine bağlayan devasa nakliye ağıdır. Aşağı Mahalle postanesinden mektubu alır, kendi üzerindeki yol haritalarına (**Routing Table / Yönlendirme Tablosu**) bakar, *"Yukarı Mahalle'ye giden en kısa yol otobandan geçiyor"* der ve paketi doğru yöne yönlendirir.
---
# ⚙️ 2. IP Adresleme ve Subnetting (Alt Ağlar) Matematiği

İnternete bağlı her cihazın mantıksal bir adresi olmak zorundadır: **IP (Internet Protocol) Adresi**. Bugün dünyada ağırlıklı olarak 32-bit uzunluğundaki **IPv4** standardını kullanıyoruz.

32-bit'lik bir IP adresi (Örn: `192.168.1.50`) yan yana gelmiş 4 adet 8-bit'lik (Byte) sayıdan oluşur. Her bir bölüme **Oktet (Octet)** adı verilir.

---

### 🔍 IP Adresinin İki Bölümü

Bir IP adresi her zaman iki ana parçadan oluşur:
1. **Network ID (Ağ Kimliği):** Cihazın hangi ağda (mahallede) olduğunu belirtir.
2. **Host ID (Cihaz Kimliği):** O ağın içindeki hangi spesifik cihaz (bina numarası) olduğunu belirtir.

Bu iki parçayı birbirinden ayıran sihirli araç ise **Subnet Mask'tır (Alt Ağ Maskesi)**.



---

### 🔀 CIDR (Classless Inter-Domain Routing) Notasyonu

Eski yıllarda IP'ler A, B, C gibi katı sınıflara ayrılırdı ve bu durum büyük bir IP adresi israfına yol açardı. Bugün **CIDR** adı verilen esnek ve modern yönlendirme sistemini kullanıyoruz.

#### Örnek Analiz: `192.168.1.0/24`
Buradaki `/24` (Prefix), alt ağ maskesinin soldan sağa doğru kaç bitinin "1" (yani ağ adresine ayrılmış) olduğunu söyler.

* **Binary (İkili) Gösterim:** `11111111.11111111.11111111.00000000` (tam 24 adet 1 yan yana)
* **Decimal (Ondalık) Gösterim:** `255.255.255.0`
* **Cihaz (Host) Alanı:** Geriye kalan 8 bit ($32 - 24 = 8$) ise cihazlara dağıtabileceğimiz alandır.
* **Toplam Adres Sayısı:** Bu ağda matematiksel olarak toplam $2^8 = 256$ adet adres üretebiliriz.

---

### ⚠️ Hayati Kural: Rezerv Adresler

Bir alt ağda üretilen adreslerin ilk ve son adresi asla fiziksel cihazlara (bilgisayar, sunucu vb.) tanımlanamaz:

1. **İlk Adres (Network Address):** Ağın kendisini temsil eden adrestir (Örn: `192.168.1.0`).
2. **Son Adres (Broadcast Address):** O ağdaki tüm cihazlara aynı anda paket göndermek/seslenmek için rezerve edilmiştir (Örn: `192.168.1.255`).

Bu yüzden bir alt ağdaki **kullanılabilir cihaz (Host) sayısı** formülü şu şekildedir:

$$\text{Kullanılabilir Cihaz Sayısı} = 2^{\text{Host Bitleri}} - 2$$

*(Örnek ağımız için: $2^8 - 2 = 254$ cihaz)*
---

### 🗺️ 3. Routing (Yönlendirme) Dinamikleri
Router'lar, paketleri hedeflerine ulaştırmak için dünya haritalarına benzer Routing Table (Yönlendirme Tabloları) kullanırlar. Bir IP paketi router'a geldiğinde izlenen teknik senaryo şudur:

Paket Alınır: Router, gelen paketin L2 başlığını (Header) söker ve L3 başlığındaki Hedef IP adresini okur.

Tablo Taraması: Kendi yönlendirme tablosundaki kuralları yukarıdan aşağıya tarar.

En Uzun Eşleşme (Longest Prefix Match - LPM): Eğer hedef IP adresiyle eşleşen birden fazla kural (route) varsa, router her zaman en spesifik (yani alt ağ maskesi en dar, / prefix değeri en yüksek olan) kuralı seçer.

Örnek: Hedef IP adresi 10.0.1.5 olsun. Yönlendirme tablosunda hem 10.0.0.0/16 hem de 10.0.1.0/24 kuralları tanımlıysa; router /24 olan kuralı seçer. Çünkü /24 daha dar ve nokta atışı bir tariftir.

Default Route (0.0.0.0/0): Eğer router hedef IP'ye nasıl gideceğini tablodaki hiçbir özel kuralda bulamazsa, paketi çöpe atmak yerine "dış dünyaya açılan kapı" olan Varsayılan Geçit (Default Route) yönüne fırlatır.

### 💥 4. IP Fragmentation (Parçalanma) ve MTU Sınırları
Fiziksel katmanı işlerken MTU (Maximum Transmission Unit) kavramının standart ethernet ağlarında varsayılan olarak 1500 Byte olduğunu görmüştük. Peki, bir router kendisinden daha küçük MTU'ya (örneğin VPN tünelleri veya özel geniş alan ağları nedeniyle) sahip bir sonraki bağlantı yoluna devasa bir paket göndermek zorunda kalırsa ne olur?

İşte burada Katman 3'ün esneklik yeteneği olan IP Fragmentation (IP Parçalanması) devreye girer.

### ⚙️ Parçalanma Nasıl Gerçekleşir?
Diyelim ki elimizde 1500 byte boyutunda bir IP paketi var ve bu paketin 1400 byte MTU limitine sahip bir hattan/tünelden geçmesi gerekiyor. Router bu paketi şu şekilde dilimler:

1. Parça: 1400 byte boyutunda oluşturulur. IP başlığındaki (header) More Fragments (MF) bayrağı (flag) 1 yapılır. Bu, "ben son parça değilim, arkamdan gelen parçalar var" anlamına gelir. Fragment Offset değeri ise paketin en başı olduğu için 0 olarak ayarlanır.

2. Parça: Kalan 100 byte (artı IP başlığı) boyutunda oluşturulur. Bu son parça olduğu için MF bayrağı 0 yapılır (yani "ben son parçayım" mesajı verilir). Fragment Offset alanına ise bu parçanın orijinal paketin tam olarak hangi byte'ından itibaren başladığı bilgisi yazılır.

⚠️ Hayati Bilgi: Parçalara ayrılan paketler yol üstündeki router'lar tarafından tekrar birleştirilmez. Birleştirme işlemi (Reassembly), paketlerin ulaştığı en uçtaki hedef cihazın (Host) işletim sistemi seviyesinde gerçekleştirilir.
