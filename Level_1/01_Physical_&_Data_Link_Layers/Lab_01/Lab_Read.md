# Laboratuvar Dosya Bilgileri

## Lab 01 — Physical & Data Link Layers

## Amaç

* Bu laboratuvar çalışmasının amacı, fiziksel katman (Physical Layer) ve veri bağı katmanı (Data Link Layer) protokol ve parametrelerini uygulamalı olarak analiz etmektir. Çalışma kapsamında ağ arayüzü yapılandırması, yerel yönlendirme tablosu kuralları, ARP (Address Resolution Protocol) önbelleği ve veri paketlerinin geçiş sınırlarını belirleyen MTU (Maximum Transmission Unit) kısıtlamaları incelenmiştir.

## Kullanılan Ortam ve Araçlar

* Laboratuvar çalışmasında kullanılan altyapı ve yazılımlar aşağıdaki tabloda belirtilmiştir:

| Bileşen / Araç | Açıklama |
| :--- | :--- |
| **Ortam / OS** | AWS EC2 üzerinde Ubuntu Server 24.04 LTS (veya benzeri Linux dağıtımı) |
| **Ağ Yapılandırma Araçları** | `iproute2` paket bileşenleri (`ip a`, `ip route`, `ip neigh`) |
| **Ağ Analiz Aracı** | `tcpdump` paket analizörü |
| **Test Aracı** | `ping` (ICMP test aracı) |

---

## Uygulama Adımları

### 1. Aktif Ağ Arayüzü ve MTU Değerinin Tespiti

* Sistemdeki aktif ağ arayüzlerini ve bu arayüzlerin MTU (Maximum Transmission Unit) limitlerini incelemek amacıyla `ip a` komutu çalıştırılmıştır.


* |ip a|
* Yapılan incelemede, dış dünya ile iletişimi sağlayan ana arayüzün ens5 olduğu, bu arayüze 172.31.43.154/20 IP adresinin atandığı ve ağ kartının MTU değerinin standart Ethernet paket limitinin üzerinde, 9001 (Jumbo Frame) olarak yapılandırıldığı doğrulanmıştır.
---

## 2. Varsayılan Ağ Geçidinin Sorgulanması
* Sistemin kendi alt ağı dışındaki adreslerle iletişim kurabilmesi için kullandığı varsayılan ağ geçidini (default gateway) tespit etmek amacıyla yönlendirme tablosu sorgulanmıştır.


* ip route show default
* Sorgu sonucunda, tüm harici trafiğin ens5 arayüzü üzerinden 172.31.32.1 IP adresli ağ geçidine yönlendirildiği görülmüştür.
---

## 3. ARP Tablosunun İncelenmesi
* Yerel ağdaki cihazların IP ve MAC adresi eşleşmelerini saklayan ARP önbelleğini görüntülemek için ilgili komut çalıştırılmıştır.


* ip neigh show
* Çıktıda, varsayılan ağ geçidimiz olan 172.31.32.1 IP adresinin donanımsal MAC adresinin 06:9d:9b:ec:1c:eb olduğu ve durumunun REACHABLE (ulaşılabilir) olarak güncel tutulduğu tespit edilmiştir.
---

## 4. Canlı ARP Trafiğinin Analiz Edilmesi
* Arka planda çalışan ARP protokolünün istek ve yanıt süreçlerini yakalamak üzere tcpdump aracı kullanılmıştır.


* sudo tcpdump -i ens5 -n arp
* Analiz sırasında, ağ geçidinin (172.31.32.1) yerel makinemizin IP adresine ait fiziksel MAC adresini sorgulamak için bir ARP isteği gönderdiği (Request who-has...), yerel makinemizin ise kendi MAC adresiyle bu isteğe anında cevap verdiği (Reply... is-at...) canlı olarak izlenmiştir.
---

## 5. MTU Limitlerinin ve Paket Parçalanmasının Test Edilmesi
* Fiziksel ve veri bağı katmanındaki MTU sınırlarını test etmek amacıyla, ping paketlerinin parçalanmasını engelleyen -M do parametresi kullanılarak farklı boyutlarda testler gerçekleştirilmiştir.

Sistemde yapılandırılan MTU limitini (9001) aşacak şekilde 8974 byte payload (veri) ve 28 byte IP/ICMP başlığı eklenerek toplamda 9002 byte boyutunda bir paket gönderilmeye çalışılmıştır:


* ping -M do -s 8974 192.168.1.100
* Bu denemede, paket boyutu ağ kartının MTU limitini aştığı için işletim sistemi düzeyinde doğrudan hata alınmıştır.

* Ağ kartının desteklediği maksimum sınır olan 9001 byte limiti göz önünde bulundurularak paket veri boyutu (payload) 9001 - 28 = 8973 byte değerine çekilmiş ve ağ geçidine başarıyla gönderilmiştir:


* ping -M do -s 8973 172.31.32.1
* Bu komut sonucunda, paketler parçalanmaya uğramadan (0% packet loss) ağ geçidine başarıyla ulaşmış ve yanıt alınmıştır.
---
## Karşılaşılan Sorunlar ve Çözümleri

## 1. Paket Boyutu Hatası (Message Too Long)
Hata: ping: local error: message too long, mtu=9001

* Nedeni: -M do (parçalama yapma) parametresi etkinken ağ arayüzünün limitinden (MTU 9001) daha büyük bir paket gönderilmeye çalışılmasıdır. ping -M do -s 8974 komutunda, 8974 byte veriye 28 byte protokol başlığı eklendiğinde paket boyutu 9002 byte'a ulaşarak yerel arayüz limitini aşmıştır.

* Doğru Kullanım / Çözüm: Gönderilecek verinin boyutu en fazla MTU - 28 formülüne uygun olmalıdır. Komut ping -M do -s 8973 şeklinde düzeltilerek toplam boyut tam olarak 9001 byte sınırında tutulmuş ve sorun çözülmüştür.

## 2. Erişilemeyen IP Adresine Ping Gönderimi ve %100 Paket Kaybı
Hata: 169 packets transmitted, 0 received, 100% packet loss

* Nedeni: 192.168.1.100 adresine gönderilen test paketlerinin, yerel alt ağ dışında olması ve yönlendirme tablosu üzerinden bu adrese erişim sağlanamamasıdır.

* Doğru Kullanım / Çözüm: Testlerin yerel alt ağdaki aktif cihazlar veya varsayılan ağ geçidi adresi olan 172.31.32.1 üzerinden yapılması sağlanarak bağlantının kararlılığı doğrulanmıştır.

## Öğrendiklerim:

* Ağ arayüzlerinde MTU yapılandırmasının veri iletim limitlerini nasıl doğrudan etkilediğini deneyimledim.

* Paketlerin ağ üzerinde taşınırken aldığı toplam boyutun, ham veri (payload) ile protokol başlıklarının (IP ve ICMP overhead) toplamından oluştuğunu pratik hesaplamalarla kavradım.

* Jumbo Frame (MTU 9001) yapılandırmasının yüksek performanslı ağlarda paket parçalanmasını (fragmentation) engelleyerek işlemci yükünü nasıl azalttığını öğrendim.

* tcpdump aracını kullanarak ARP istek ve cevap paketlerinin yapısını ve yerel ağdaki adres çözümleme süreçlerini izledim.

* ipneigh komutu ile işletim sisteminin yerel ARP tablosunu nasıl sakladığını ve yönettiğini gözlemledim.

* Paketlerin parçalanmasını önleyen -M do bayrağı ile Path MTU Discovery (PMTUD) mekanizmasının temel çalışma mantığını anladım.
---
## Sonuç
* Bu laboratuvar çalışması; veri bağı katmanında adres çözümlemenin (ARP) nasıl gerçekleştiğini, yönlendirme kararlarının varsayılan ağ geçidi ile nasıl ilişkilendirildiğini ve fiziksel limitlerin (MTU) veri paketlerinin taşınmasındaki kritik rolünü somut çıktılarla doğrulamıştır.
