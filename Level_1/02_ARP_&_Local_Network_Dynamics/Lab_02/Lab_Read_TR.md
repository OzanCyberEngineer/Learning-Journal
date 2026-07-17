# Lab 01 — Physical & Data Link Layers

## ARP ve Yerel Ağ Dinamikleri Analizi

Bu laboratuvar çalışmasında, Linux işletim sistemi çekirdek (kernel) seviyesinde Address Resolution Protocol (ARP) protokolünün işleyişi, komşu tablosunun (neighbor table) yönetimi ve yerel ağ segmentindeki canlı veri akışı analiz edilmiştir. Çalışma kapsamında ağ geçidi tespiti, Katman 2 seviyesinde erişim testleri, statik kayıt manipülasyonları ve paket yakalama işlemleri gerçekleştirilmiştir.

> Bu çalışma fiziksel donanımı doğrudan incelemekten ziyade, Linux ve bulut ortamında gözlemlenebilen ağ davranışlarını analiz etmektedir.

## Amaç

* Varsayılan ağ geçidi (default gateway) bilgilerinin ve aktif ağ arayüzünün doğrulanması.
* `arping` aracı kullanılarak Katman 2 (Data Link Layer) seviyesinde uçtan uca erişilebilirliğin test edilmesi.
* Linux çekirdeğindeki komşu tablosunun (`ip neigh`) incelenmesi ve durum kodlarının analizi.
* Çekirdek tablosuna statik ve kalıcı (`PERMANENT`) ARP kayıtlarının eklenmesi ve silinmesi adımlarının uygulanması.
* `tcpdump` paket analiz aracı yardımıyla ağ arayüzündeki ham ARP istek (Request) ve yanıt (Reply) paketlerinin yakalanarak incelenmesi.

## Kullanılan Ortam ve Araçlar

| Bileşen | Kullanılan ortam veya araç |
|---|---|
| Bulut platformu | AWS EC2 |
| İşletim sistemi | Ubuntu Server 24.04 LTS |
| Ağ arayüzü | ens5 |
| Kullanılan araçlar | iproute2 (ip route, ip neigh), arping, tcpdump |

> Public IP adresi ve kimlik doğrulama bilgileri güvenlik nedeniyle paylaşılmamıştır.

## Ağ Yapısı veya Mimari

```text
Ubuntu EC2 Instance
Private IP: 172.31.43.154
        |
        | Ağ arayüzü: ens5 (Ethernet)
        |
        v
AWS VPC Gateway / Router
Gateway IP: 172.31.32.1 [MAC: 06:9d:9b:ec:1c:eb]
```

## Uygulama Adımları

### 1. Varsayılan Ağ Geçidinin Belirlenmesi
Sistemdeki aktif ağ geçidini, yönlendirme tablosunu ve çıkış arayüzünü doğrulamak için ilgili filtreleme komutu çalıştırılmıştır.

Komut:
```bash
ip route show | grep default
```

Çıktı:
```text
default via 172.31.32.1 dev ens5 proto dhcp src 172.31.43.154 metric 100
```

Teknik Açıklama:
Sistemin varsayılan yönlendirme hedefinin `172.31.32.1` IP adresi olduğu, paketlerin `ens5` ağ arayüzü üzerinden gönderildiği ve sunucunun yerel IP adresinin `172.31.43.154` olduğu tespit edilmiştir.

### 2. Katman 2 Seviyesinde Canlı ARP Sorgusu (`arping`)
Belirlenen varsayılan ağ geçidine ICMP (IP katmanı) yerine doğrudan Katman 2 seviyesinde ARP istekleri gönderilerek donanım düzeyinde erişim testi gerçekleştirilmiştir.

Komut:
```bash
arping -I ens5 172.31.32.1
```

Çıktı:
```text
ARPING 172.31.32.1 from 172.31.43.154 ens5
Unicast reply from 172.31.32.1 [06:9D:9B:EC:1C:EB] 0.560ms
Unicast reply from 172.31.32.1 [06:9D:9B:EC:1C:EB] 0.589ms
Unicast reply from 172.31.32.1 [06:9D:9B:EC:1C:EB] 0.569ms
Unicast reply from 172.31.32.1 [06:9D:9B:EC:1C:EB] 0.574ms
Unicast reply from 172.31.32.1 [06:9D:9B:EC:1C:EB] 0.580ms
Unicast reply from 172.31.32.1 [06:9D:9B:EC:1C:EB] 0.577ms
Unicast reply from 172.31.32.1 [06:9D:9B:EC:1C:EB] 0.581ms
Unicast reply from 172.31.32.1 [06:9D:9B:EC:1C:EB] 0.577ms
Unicast reply from 172.31.32.1 [06:9D:9B:EC:1C:EB] 0.570ms
Unicast reply from 172.31.32.1 [06:9D:9B:EC:1C:EB] 0.584ms
```

Teknik Açıklama:
`ens5` arayüzü üzerinden ağ geçidine yapılan sorgularda kesintisiz tekli gönderim (unicast) yanıtları alınmıştır. Bu işlem sonucunda ağ geçidinin donanım (MAC) adresinin `06:9d:9b:ec:1c:eb` olduğu doğrulanmıştır.

### 3. Komşu Tablosunun (Neighbor Table) İncelenmesi
Linux çekirdeği tarafından RAM üzerinde önbelleğe alınan IP-MAC adresi eşleşme tablosu kontrol edilmiştir.

Komut:
```bash
ip neigh show
```

Çıktı:
```text
172.31.32.1 dev ens5 lladdr 06:9d:9b:ec:1c:eb REACHABLE
```

Teknik Açıklama:
Ağ geçidine ait eşleşme kaydının tabloda yer aldığı ve durum kodunun `REACHABLE` olduğu gözlemlenmiştir. Bu durum, söz konusu ağ nesnesinin erişilebilir olduğunu ve kaydın çekirdek geçerlilik süresi içinde aktif olarak doğrulandığını gösterir.

### 4. Statik ve Kalıcı ARP Kaydı Ekleme Simülasyonu
Ağ güvenliği testleri veya belirli mimari gereksinimleri simüle etmek amacıyla tabloya manuel olarak kalıcı bir ARP kaydı işlenmiştir.

Komut:
```bash
sudo ip neigh add 192.168.1.50 lladdr 00:11:22:33:44:55 dev ens5
ip neigh show
```

Çıktı:
```text
172.31.32.1 dev ens5 lladdr 06:9d:9b:ec:1c:eb REACHABLE
192.168.1.50 dev ens5 lladdr 00:11:22:33:44:55 PERMANENT
```

Teknik Açıklama:
`192.168.1.50` IP adresi için belirlenen örnek MAC adresi sisteme başarıyla tanımlanmıştır. Çıktıda görülen `PERMANENT` ifadesi, bu kaydın statik olduğunu, çekirdek zaman aşımı mekanizmalarından etkilenmeyeceğini ve dışarıdan gelen dinamik ARP paketleriyle ezilemeyeceğini belirtir.

### 5. Oluşturulan Statik ARP Kaydının Silinmesi
Tablonun bütünlüğünü korumak ve sistemi varsayılan durumuna döndürmek amacıyla eklenen statik kayıt temizlenmiştir.

Komut:
```bash
sudo ip neigh del 192.168.1.50 dev ens5
ip neigh show
```

Çıktı:
```text
172.31.32.1 dev ens5 lladdr 06:9d:9b:ec:1c:eb REACHABLE
```

Teknik Açıklama:
`del` parametresiyle hedef alınan IP adresi komşu tablosundan kaldırılmış ve çekirdek seviyesindeki önbellek temizlenerek ilk hâline getirilmiştir.

### 6. tcpdump ile Canlı ARP Trafiğinin İzlenmesi
Ağ arayüzü üzerinden geçen ham paketleri yakalamak ve protokol döngüsünü doğrulamak amacıyla bir paket analiz oturumu başlatılmıştır.

Komut:
```bash
sudo tcpdump -pni ens5 arp
```

Çıktı:
```text
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on ens5, link-type EN10MB (Ethernet), snapshot length 262144 bytes
19:03:09.064825 ARP, Request who-has 172.31.43.154 tell 172.31.32.1, length 28
19:03:09.064844 ARP, Reply 172.31.43.154 is-at 06:62:d3:8c:29:27, length 28
^C
2 packets captured
2 packets received by filter
0 packets dropped by kernel
```

Teknik Açıklama:
Ağ geçidinin (`172.31.32.1`), yerel sunucumuzun (`172.31.43.154`) MAC adresini öğrenmek amacıyla Katman 2 seviyesinde bir ARP Request (who-has) paketi gönderdiği yakalanmıştır. Hemen ardından sunucumuzun kendi fiziksel donanım adresini (`06:62:d3:8c:29:27`) beyan eden bir ARP Reply (is-at) paketiyle yanıt verdiği canlı trafik üzerinde doğrulanmıştır.

## Teknik Hesaplama veya Analiz

Yakaladığımız standart ARP paketlerinin veri alanı (payload) boyutu analiz edildiğinde RFC standartlarıyla tam uyum gösterdiği doğrulanmıştır:

```text
Donanım Türü (2B) + Protokol Türü (2B) + Donanım Uzunluğu (1B) + Protokol Uzunluğu (1B) + İşlem Kodu (2B) + Gönderici MAC (6B) + Gönderici IP (4B) + Hedef MAC (6B) + Hedef IP (4B)
2 + 2 + 1 + 1 + 2 + 6 + 4 + 6 + 4 = 28 bayt
```

`tcpdump` çıktısında yer alan `length 28` ifadesi, Ethernet çerçevesi (frame) içerisine gömülen ham ARP veri yükünün tam olarak bu hesaplamaya denk geldiğini kanıtlamaktadır.

## Öğrendiklerim

* Varsayılan ağ geçidi adresinin ve aktif rota önceliklerinin `ip route` komut çıktısı üzerinden nasıl analiz edildiğini öğrendim.
* IP katmanından bağımsız olarak, doğrudan Veri Bağı Katmanı (Katman 2) üzerinde `arping` aracıyla uç birim erişilebilirlik testleri gerçekleştirmeyi deneyimledim.
* Linux çekirdeğinin (kernel) IP-MAC haritalandırmalarını tuttuğu komşu tablosunun (`ip neigh show`) yapısını ve işleyiş mantığını kavradım.
* `REACHABLE` ve `PERMANENT` durum kodları arasındaki mimari farkları görerek, statik ARP kayıtlarının ağ güvenliğindeki (ARP Spoofing/Poisoning koruması) rolünü uyguladım.
* `tcpdump` aracıyla canlı ağ arayüzlerinde belirli bir protokole yönelik (`arp`) filtreleme yapmayı ve paket analiz süreçlerini yönetmeyi öğrendim.
* Canlı trafik verilerinde `Request (who-has)` ve `Reply (is-at)` döngüsünü inceleyerek teorik protokol adımlarını pratik çıktılarla eşleştirdim.
* Bir ARP veri yükünün (payload) yapısal olarak neden tam olarak 28 bayt yer kapladığını çıktı analizleriyle doğruladım.

## Sonuç

Bu laboratuvar çalışmasında, AWS bulut altyapısında çalışan bir Linux sunucusu üzerinde Katman 2 ve Katman 3 katmanları arasındaki adres çözümleme süreçleri incelenmiştir. `iproute2` alt araçları, `arping` ve `tcpdump` kullanılarak hem işletim sistemi çekirdek belleğindeki tablolar hem de ağ arayüzünden geçen ham trafik başarılı bir şekilde manipüle ve analiz edilmiştir. 

Gerçekleştirilen adımlarda ağ geçidi ile kurulan ARP ilişkisi doğrulanmış, statik kayıt süreçleri işletilmiş ve yakalanan paket boyutlarının teorik standartlarla uyuştuğu görülmüştür.
