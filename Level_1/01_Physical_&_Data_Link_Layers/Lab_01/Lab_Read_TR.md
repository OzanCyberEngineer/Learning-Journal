<!-- File: README.md -->

# Lab 01 — Physical & Data Link Layers

## ARP, MTU ve Jumbo Frame Analizi

Bu laboratuvarda AWS EC2 üzerinde çalışan Ubuntu sunucusunun ağ arayüzü incelendi. Varsayılan ağ geçidi ve komşuluk tablosu görüntülendi, ARP trafiği canlı olarak yakalandı ve ağ arayüzünün MTU sınırı farklı ICMP paket boyutlarıyla test edildi.

> Bu çalışma fiziksel donanımı doğrudan test etmekten ziyade, Linux ağ arayüzü üzerinden veri bağı ve ağ katmanı sınırlarını incelemektedir.

---

## Amaç

Bu laboratuvarın temel amaçları şunlardır:

- Aktif ağ arayüzünü ve IP yapılandırmasını belirlemek
- Ağ arayüzünün MTU değerini incelemek
- Varsayılan ağ geçidini tespit etmek
- Linux komşuluk tablosundaki IP–MAC eşleşmelerini gözlemlemek
- ARP Request ve ARP Reply paketlerini `tcpdump` ile yakalamak
- Jumbo Frame sınırını parçalanmaya izin vermeden test etmek

---

## Kullanılan Ortam ve Araçlar

| Bileşen | Kullanılan ortam veya araç |
|---|---|
| Bulut platformu | Amazon Web Services EC2 |
| İşletim sistemi | Ubuntu Server 24.04 LTS |
| Ağ arayüzü | `ens5` |
| Private IPv4 adresi | `172.31.43.154/20` |
| Varsayılan ağ geçidi | `172.31.32.1` |
| Arayüz MTU değeri | `9001` bayt |
| Ağ araçları | `iproute2`, `ping`, `tcpdump` |

> Public IP adresi ve kimlik doğrulama bilgileri güvenlik nedeniyle paylaşılmamıştır.

---

## Ağ Yapısı

```text
Ubuntu EC2 Instance
172.31.43.154/20
        |
        | ens5 — MTU 9001
        |
        v
AWS VPC Gateway
172.31.32.1
```

---

## Uygulama Adımları

### 1. Ağ arayüzünün incelenmesi

Sunucudaki ağ arayüzlerini, IP adreslerini ve MTU değerlerini görüntülemek için aşağıdaki komut çalıştırıldı:

```bash
ip address show
```

Çıktıda aktif ağ arayüzünün `ens5` olduğu görüldü. Arayüze `172.31.43.154/20` private IPv4 adresi atanmıştı ve MTU değeri `9001` olarak yapılandırılmıştı.

![Ağ arayüzü ve MTU bilgisi](images/01-network-interface-mtu.png)

### Bulgular

- `ens5` arayüzü `UP` durumundadır.
- Arayüz Amazon EC2 sanal ağına bağlıdır.
- `9001` MTU değeri Jumbo Frame kullanımına uygundur.
- `lo`, sistemin loopback arayüzüdür.

---

### 2. Varsayılan ağ geçidinin belirlenmesi

Sunucunun varsayılan rotasını görüntülemek için şu komut kullanıldı:

```bash
ip route show default
```

![Varsayılan ağ geçidi](images/02-default-route.png)

Çıktı:

```text
default via 172.31.32.1 dev ens5 proto dhcp src 172.31.43.154 metric 100
```

Bu sonuç, yönlendirme tablosunda daha özel bir rota bulunmayan trafiğin `ens5` arayüzü üzerinden `172.31.32.1` ağ geçidine gönderildiğini göstermektedir.

---

### 3. Komşuluk tablosunun incelenmesi

Linux komşuluk tablosunu görüntülemek için aşağıdaki komut çalıştırıldı:

```bash
ip neigh show
```

![Linux komşuluk tablosu](images/03-neighbor-table.png)

Örnek çıktı:

```text
172.31.32.1 dev ens5 lladdr 06:9d:9b:ec:1c:eb REACHABLE
```

Bu kayıt, `172.31.32.1` IP adresi ile ilgili MAC adresinin eşleştirildiğini göstermektedir.

`REACHABLE` durumu, söz konusu komşuya yakın zamanda başarıyla erişildiğini ve kaydın geçerli kabul edildiğini belirtir.

---

### 4. ARP trafiğinin yakalanması

`ens5` arayüzündeki ARP paketlerini yakalamak için `tcpdump` kullanıldı:

```bash
sudo tcpdump -i ens5 -n arp
```

Paket yakalama işlemi ham ağ trafiğine erişim gerektirdiği için komut `sudo` yetkisiyle çalıştırıldı.

![ARP paketlerinin yakalanması](images/04-arp-capture.png)

Yakalanan paketlerde aşağıdaki ARP mesajları gözlemlendi:

```text
ARP, Request who-has 172.31.43.154 tell 172.31.32.1
ARP, Reply 172.31.43.154 is-at 06:62:d3:8c:29:27
```

Bu iletişimde:

1. Ağ geçidi, `172.31.43.154` adresine karşılık gelen MAC adresini sordu.
2. EC2 sunucusu kendi MAC adresini içeren bir ARP Reply mesajı gönderdi.
3. IPv4 adresi ile MAC adresi arasında yerel ağda kullanılacak eşleşme oluşturuldu.

---

### 5. MTU sınırının test edilmesi

`ens5` arayüzünün MTU değeri `9001` bayttır.

IPv4 üzerinden gönderilen standart bir ICMP Echo Request paketinde toplam boyut şu şekilde hesaplanır:

```text
ICMP veri alanı + ICMP başlığı + IPv4 başlığı
```

Bu testte IPv4 seçenekleri kullanılmadığı için başlık boyutları:

```text
ICMP başlığı: 8 bayt
IPv4 başlığı: 20 bayt
Toplam ek yük: 28 bayt
```

#### MTU sınırını aşan paket

Parçalanmaya izin vermeden `8974` bayt veri gönderilmeye çalışıldı:

```bash
ping -c 3 -M do -s 8974 172.31.32.1
```

Toplam IP paket boyutu:

```text
8974 + 8 + 20 = 9002 bayt
```

Bu değer arayüzün `9001` baytlık MTU sınırını aştığı için paket gönderilmedi.

```text
ping: local error: message too long, mtu=9001
```

![MTU sınırının aşılması](images/05-mtu-exceeded.png)

#### MTU sınırına uyan paket

ICMP veri alanı `8973` bayta düşürüldü:

```bash
ping -c 6 -M do -s 8973 172.31.32.1
```

Toplam IP paket boyutu:

```text
8973 + 8 + 20 = 9001 bayt
```

![Jumbo Frame testinin başarılı olması](images/06-jumbo-frame-success.png)

Paketler parçalanmadan ağ geçidine iletildi ve `%0 packet loss` sonucu alındı.

Bu test, EC2 instance ile varsayılan ağ geçidi arasındaki doğrudan yolun `9001` baytlık MTU değerini desteklediğini göstermektedir. Sonuç, internet üzerindeki bütün uçtan uca yolların aynı MTU değerini desteklediği anlamına gelmez.

---

## Karşılaşılan Sorunlar ve Çözümleri

### Yanlış ağ arayüzü kullanılması

İlk denemede genel örneklerde sıkça kullanılan `eth0` arayüzü sorgulandı:

```bash
ip -s link show eth0
```

Ancak sunucudaki ağ arayüzünün adı `ens5` olduğu için aşağıdaki hata alındı:

```text
Device "eth0" does not exist.
```

Doğru kullanım:

```bash
ip -s link show ens5
```

---

### `tcpdump` yetki hatası

`tcpdump` normal kullanıcıyla çalıştırıldığında paket yakalama izni bulunmadığı için hata verdi:

```text
You don't have permission to perform this capture
```

Komut yönetici yetkisiyle çalıştırılarak sorun çözüldü:

```bash
sudo tcpdump -i ens5 -n arp
```

---

### MTU sınırının aşılması

`8974` baytlık ICMP veri alanına protokol başlıkları eklendiğinde toplam paket boyutu `9002` bayta ulaştı.

Bu değer `9001` MTU sınırını aştığı için sistem şu hatayı verdi:

```text
message too long, mtu=9001
```

ICMP veri alanı `8973` bayta düşürülerek toplam paket boyutu tam olarak `9001` baytta tutuldu.

---

## Öğrendiklerim

- Aktif ağ arayüzünün `ip address show` komutuyla nasıl belirlendiğini öğrendim.
- Linux sistemlerinde ağ arayüzü adının her zaman `eth0` olmadığını gördüm.
- Varsayılan ağ geçidinin yönlendirme tablosundan nasıl bulunacağını öğrendim.
- `ip neigh show` ile Linux komşuluk tablosunun nasıl incelendiğini gözlemledim.
- ARP Request ve ARP Reply paketlerini `tcpdump` ile canlı olarak yakaladım.
- Paket yakalama işlemlerinin neden yükseltilmiş yetki gerektirdiğini öğrendim.
- ICMP paket boyutunun veri alanı ve protokol başlıklarından oluştuğunu uygulamalı olarak hesapladım.
- `ping -M do` ile parçalanmaya izin vermeden MTU sınırının nasıl test edileceğini gördüm.
- Jumbo Frame desteğinin yalnızca test edilen ağ yolu için doğrulanabileceğini öğrendim.

---

## Sonuç

Bu laboratuvarda AWS EC2 üzerinde çalışan Ubuntu sunucusunun ağ arayüzü, varsayılan rotası ve komşuluk tablosu incelendi. ARP adres çözümleme süreci gerçek zamanlı olarak yakalandı ve `ens5` arayüzündeki `9001` baytlık MTU sınırı ICMP paketleriyle doğrulandı.

`8973` baytlık ICMP veri alanına sahip paket başarıyla iletilirken, `8974` baytlık veri alanına sahip paket MTU sınırını aştığı için işletim sistemi tarafından gönderilmedi.
