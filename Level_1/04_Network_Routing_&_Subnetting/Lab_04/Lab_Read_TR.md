# Lab 04 — Network Layer

## Yönlendirme Tablosu, Hedef Rota ve Path MTU Analizi

Bu laboratuvar çalışmasında, Linux işletim sistemi üzerinde çekirdeğin (kernel) yönlendirme mekanizması, aktif yönlendirme tablolarının yapısı, belirli bir dış hedefe giden en uygun yolun (Next-Hop) belirlenmesi ve ağ yolları üzerindeki Path MTU (Maximum Transmission Unit) değişimleri incelenmiştir.

> Bu çalışma fiziksel donanımı doğrudan incelemekten ziyade, Linux ve bulut ortamında gözlemlenebilen ağ davranışlarını analiz etmektedir.

## Amaç

* Linux çekirdeğindeki aktif yönlendirme tablosunu satır satır analiz etmek ve varsayılan ağ geçidini belirlemek.
* Yerel ağ geçidine olan katman 3 erişilebilirliğini ICMP protokolü üzerinden doğrulamak.
* Çekirdeğin belirli bir hedef IP adresi için dinamik olarak seçtiği çıkış arayüzünü ve kaynak IP adresini sorgulamak.
* Dış ağlardaki bir hedefe giden yoldaki yönlendirici duraklarını izlemek.
* Bulut ortamındaki Jumbo Frame destekli yerel ağ ile internet hatları arasındaki MTU değişimlerini (Path MTU Discovery) gözlemlemek.

## Kullanılan Ortam ve Araçlar

| Bileşen | Kullanılan ortam veya araç |
|---|---|
| Bulut platformu | AWS EC2 |
| İşletim sistemi | Ubuntu Server |
| Ağ arayüzü | ens5 (Elastic Network Adapter) |
| Kullanılan araçlar | iproute2 (ip route), ping, tracepath |

> Public IP adresi ve kimlik doğrulama bilgileri güvenlik nedeniyle paylaşılmamıştır.

## Ağ Yapısı veya Mimari

```text
Ubuntu EC2 Instance (ens5)
İç IP: 172.31.43.154 (Yerel MTU: 9001)
        |
        | Yerel Alt Ağ: 172.31.32.0/20
        v
AWS VPC Default Gateway
Ağ Geçidi IP: 172.31.32.1 (Dış Hat MTU: 1500)
        |
        v
İnternet Yönlendiricileri
        |
        v
Hedef Sunucu (Google DNS: 8.8.8.8)
```

## Uygulama Adımları

### 1. Ağ Arayüzünün ve Yönlendirme Tablosunun İncelenmesi

Sistemin paketleri hangi ağ arayüzlerinden ve hangi ağ geçitleri üzerinden ileteceğini belirleyen aktif yönlendirme tablosu sorgulanmıştır.

Komut:

```bash
ip route show
```

Çıktı:

```text
default via 172.31.32.1 dev ens5 proto dhcp src 172.31.43.154 metric 100
172.31.0.2 via 172.31.32.1 dev ens5 proto dhcp src 172.31.43.154 metric 100
172.31.32.0/20 dev ens5 proto kernel scope link src 172.31.43.154 metric 100
172.31.32.1 dev ens5 proto dhcp scope link src 172.31.43.154 metric 100
```

Teknik Açıklama:
Çıktı incelendiğinde, sistemin `172.31.32.0/20` CIDR bloğuna sahip bir yerel ağda bulunduğu ve `scope link` ifadesinden anlaşılacağı üzere bu ağa doğrudan bağlı olduğu görülmektedir. Bilinmeyen tüm dış ağ trafikleri (`default`) `ens5` arayüzü kullanılarak `172.31.32.1` IP adresli varsayılan ağ geçidine yönlendirilmektedir.

### 2. Varsayılan Ağ Geçidine Erişilebilirliğin Test Edilmesi

Yönlendirme tablosunda belirlenen yerel ağ geçidinin aktif ve erişilebilir olup olmadığını doğrulamak amacıyla ICMP eko istekleri gönderilmiştir.

Komut:

```bash
ping -c 3 172.31.32.1
```

Çıktı:

```text
PING 172.31.32.1 (172.31.32.1) 56(84) bytes of data.
64 bytes from 172.31.32.1: icmp_seq=1 ttl=64 time=0.046 ms
64 bytes from 172.31.32.1: icmp_seq=2 ttl=64 time=0.061 ms
64 bytes from 172.31.32.1: icmp_seq=3 ttl=64 time=0.087 ms

--- 172.31.32.1 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2086ms
rtt min/avg/max/mdev = 0.046/0.064/0.087/0.016 ms
```

Teknik Açıklama:
Gönderilen 3 paketin tamamı başarıyla alınmış ve `%0` paket kaybı raporlanmıştır. Ortalama gecikme süresinin `0.064 ms` gibi çok düşük bir değerde çıkması, ağ geçidinin yerel sanallaştırma altyapısında çok yakın bir noktada konumlandığını göstermektedir.

### 3. Belirli Bir Hedef İçin Rota Sorgulaması

İnternet üzerindeki harici bir IP adresine (`8.8.8.8`) gönderilecek bir paket için çekirdeğin hangi kararı vereceği simüle edilerek sorgulanmıştır.

Komut:

```bash
ip route get 8.8.8.8
```

Çıktı:

```text
8.8.8.8 via 172.31.32.1 dev ens5 src 172.31.43.154 uid 1000
    cache
```

Teknik Açıklama:
Çekirdek, `8.8.8.8` hedefine ulaşmak için en spesifik eşleşme olarak varsayılan rotayı seçmiştir. Paketlerin `ens5` arayüzünden, `172.31.43.154` kaynak IP adresiyle çıkacağı ve sonraki durak (next-hop) olarak `172.31.32.1` ağ geçidine teslim edileceği doğrulanmıştır.

### 4. Yol Boyunca Rota ve MTU Değişiminin İzlenmesi

Hedefe giden katman 3 durakları listelenirken aynı zamanda ağ yolları üzerindeki MTU sınırları analiz edilmiştir.

Komut:

```bash
tracepath -n 8.8.8.8
```

Çıktı:

```text
1?: [LOCALHOST]                  pmtu 9001
1:  172.31.32.1                        0.147ms pmtu 1500
1:  242.14.245.131                     1.188ms asymm  6
2:  no reply
3:  74.125.49.104                      0.582ms asymm 11
4:  no reply
...
25:  no reply
```

Teknik Açıklama:
Analiz başlatıldığında yerel işletim sistemi arayüzünün Jumbo Frame destekli (`pmtu 9001`) olduğu görülmektedir. Ancak paket ilk durağa (`172.31.32.1`) ulaştığı anda Path MTU değeri `1500` bayta düşmüştür. Yolun devamındaki bazı intermediate yönlendiriciler güvenlik politikaları (ICMP kısıtlamaları) nedeniyle `no reply` yanıtı dönmüştür.

## Teknik Hesaplama veya Analiz

Bulut altyapılarında (AWS VPC içi) ağ içi yüksek veri aktarım hızları sağlamak amacıyla MTU boyutu varsayılan olarak `9001 bayt (Jumbo Frame)` seviyesine ayarlanmıştır. Yapılan `tracepath` analizinde elde edilen veriler şu şekildedir:

* **Yerel Ana Bilgisayar MTU Kapasitesi (EC2):** 9001 bayt
* **Ağ Geçidi Sonrası İnternet Standardı MTU Limiti:** 1500 bayt

Paket yerel ağ geçidini geçip internet omurgasına adım attığı an, yol üzerindeki maksimum paket boyutu sınırına (Link MTU) çarpmış ve Path MTU Discovery (PMTUD) mekanizması devreye girerek bu akışa ait maksimum paket boyutunu otomatik olarak 1500 bayta indirgemiştir. Bu durum, büyük boyutlu paketlerin internet hatlarında parçalanmasını (fragmentation) önlemek için optimize edilen standart bir katman 3 davranışıdır.

## Öğrendiklerim

* `ip route show` komutuyla yerel yönlendirme tablosundaki kuralları, metrik değerlerini ve `scope link` gibi doğrudan bağlı ağ ifadelerini okumayı öğrendim.
* `ping` aracıyla katman 3 seviyesinde yerel ağ geçidinin yanıt sürelerini ve kararlılığını analiz etmeyi deneyimledim.
* `ip route get` komutunu kullanarak, gerçekte paket göndermeden önce işletim sistemi çekirdeğinin rota seçim algoritmasının (Longest Prefix Match prensibi çıktısı) nasıl sonuçlanacağını sorgulamayı öğrendim.
* `tracepath` aracı ile bir paketin hedefe giderken uğradığı yönlendiricileri izlemeyi ve ağ geçitlerinin ardındaki asimetrik yol (`asymm`) durumlarını gözlemlemeyi öğrendim.
* Bulut ortamlarındaki dahili ağlarda kullanılan Jumbo Frame (9001 bayt) yapısının internet standartları gereği ağ geçidinde nasıl 1500 baytlık standart MTU değerine çekildiğini canlı çıktılar üzerinde inceledim.
* Ağ üzerindeki bazı kurumsal yönlendiricilerin veya güvenlik duvarlarının ICMP paketlerini engelleyebileceğini ve bu durumlarda trace çıktılarında neden `no reply` görüldüğünü teknik olarak kavradım.

## Sonuç

Bu laboratuvarda, Linux ağ yönetim araçları kullanılarak katman 3 yönlendirme adımları başarılı bir şekilde taklit edilmiş ve doğrulanmıştır. Sistem yönlendirme tablosunun tutarlı çalıştığı, yerel ağ geçidine kesintisiz erişim sağlanabildiği ve harici hedeflere giden rotaların doğru belirlendiği tespit edilmiştir. tracepath analizi sayesinde ağ altyapılarındaki MTU sınırlandırmalarının dinamik davranışı ve bulut ortamından dış dünyaya çıkıştaki paket boyutu dönüşümleri somut teknik verilerle doğrulanmıştır.
