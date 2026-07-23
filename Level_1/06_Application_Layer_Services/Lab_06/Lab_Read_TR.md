# Lab 02 — Application Layer & DNS Analysis

## DNS Sorgulama, Özyinelemeli (Iterative) İzinleme ve HTTP Başlık Analizi

Bu laboratuvar çalışmasında, Linux işletim sisteminde DNS çözümleme mekanizmaları, kök (root) ve TLD sunucuları üzerinden yinelemeli DNS sorgusu takibi (`dig +trace`), yerel DNS çözümleyici yapılandırması (`resolvectl status`) ve HTTP/2 protokol başlıkları (`curl`) analiz edilmiştir.

> Bu çalışma fiziksel donanımı doğrudan incelemekten ziyade, Linux ve bulut ortamında gözlemlenebilen uygulama katmanı ve DNS davranışlarını analiz etmektedir.

## Amaç

* `dig` komutu ile A kaydı sorgusu gerçekleştirerek DNS yanıt yapılarını analiz etmek.
* `dig +trace` parametresi ile kök DNS sunucularından başlayarak hiyerarşik (iterative) DNS çözümleme adımlarını gözlemlemek.
* Sistemdeki IPv6 ağ yapılandırmasının eksikliğinden kaynaklanan DNS bağlantı hatalarını tespit etmek.
* `systemd-resolved` servisinin durumunu ve AWS VPC yerel DNS sunucusu (`172.31.0.2`) ile etkileşimini `resolvectl status` komutuyla incelemek.
* `curl` aracı ile HTTPS isteklerinin HTTP/2 yanıt başlıklarını ve yönlendirme (301 Redirect) davranışlarını analiz etmek.

## Kullanılan Ortam ve Araçlar

| Bileşen | Kullanılan ortam veya araç |
|---|---|
| Bulut platformu | AWS EC2 (eu-central-1) |
| İşletim sistemi | Ubuntu Server 24.04 LTS |
| Ağ arayüzü | ens5 |
| Kullanılan araçlar | BIND dig (9.18.39), systemd-resolved (resolvectl), curl |

> Public IP adresi ve kimlik doğrulama bilgileri güvenlik nedeniyle paylaşılmamıştır.

## Ağ Yapısı veya Mimari

```text
Ubuntu EC2 Instance (ens5)
  |
  |-- Local Stub Resolver (127.0.0.53:53)
  |      |
  |      +--> AWS VPC DNS Server (172.31.0.2:53)
  |
  +-- Direct External DNS Queries (dig +trace)
         |
         +--> Root DNS Servers (.)
         +--> TLD DNS Servers (.com)
         +--> Authoritative DNS Servers (google.com NS)
```

## Uygulama Adımları

### 1. Temel DNS A kaydı sorgusunun yapılması

Ağ üzerindeki bir alan adının IP adres karşılıklarını ve yerel stub resolver davranışını incelemek amacıyla standart `dig` sorgusu çalıştırılmıştır.

Komut:

```bash
dig google.com
```

Çıktı:

```text
; <<>> DiG 9.18.39-0ubuntu0.24.04.5-Ubuntu <<>> google.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 63665
;; flags: qr rd ra; QUERY: 1, ANSWER: 6, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 65494
;; QUESTION SECTION:
;google.com.			IN	A

;; ANSWER SECTION:
google.com.		30	IN	A	142.251.20.138
google.com.		30	IN	A	142.251.20.100
google.com.		30	IN	A	142.251.20.113
google.com.		30	IN	A	142.251.20.101
google.com.		30	IN	A	142.251.20.139
google.com.		30	IN	A	142.251.20.102

;; Query time: 0 msec
;; SERVER: 127.0.0.53#53(127.0.0.53) (UDP)
;; WHEN: Thu Jul 23 07:31:08 UTC 2026
;; MSG SIZE  rcvd: 135
```

Sorgu sonucunda `google.com` alan adının A kayıtları için 6 adet IPv4 adresi dönmüştür. Sorgunun yerel `127.0.0.53` stub resolver üzerinden iletildiği ve yanıt süresinin 0 ms olduğu görülmüştür.

### 2. Hiyerarşik (Iterative) DNS çözümleme adımlarının izlenmesi

DNS çözümleme adımlarının root sunuculardan başlayarak yetkili (authoritative) ad sunucularına kadar nasıl ilerlediğini izlemek için `+trace` parametresi kullanılmıştır.

Komut:

```bash
dig google.com +trace
```

Çıktı:

```text
; <<>> DiG 9.18.39-0ubuntu0.24.04.5-Ubuntu <<>> google.com +trace
;; global options: +cmd
.			51	IN	NS	g.root-servers.net.
.			51	IN	NS	i.root-servers.net.
.			51	IN	NS	c.root-servers.net.
.			51	IN	NS	b.root-servers.net.
.			51	IN	NS	d.root-servers.net.
.			51	IN	NS	h.root-servers.net.
.			51	IN	NS	f.root-servers.net.
.			51	IN	NS	a.root-servers.net.
.			51	IN	NS	l.root-servers.net.
.			51	IN	NS	k.root-servers.net.
.			51	IN	NS	m.root-servers.net.
.			51	IN	NS	j.root-servers.net.
.			51	IN	NS	e.root-servers.net.
;; Received 239 bytes from 127.0.0.53#53(127.0.0.53) in 2 ms

com.			172800	IN	NS	a.gtld-servers.net.
com.			172800	IN	NS	b.gtld-servers.net.
...
com.			172800	IN	NS	m.gtld-servers.net.
;; Received 1170 bytes from 192.203.230.10#53(e.root-servers.net) in 1 ms

;; UDP setup with 2001:501:b1f9::30#53(2001:501:b1f9::30) for google.com failed: network unreachable.
;; no servers could be reached
...
google.com.		172800	IN	NS	ns2.google.com.
google.com.		172800	IN	NS	ns1.google.com.
google.com.		172800	IN	NS	ns3.google.com.
google.com.		172800	IN	NS	ns4.google.com.
```

`+trace` sorgusu sırasında ilk olarak root sunucu listesi (`.`), ardından `.com` TLD sunucuları (`gtld-servers.net`) ve son aşamada `google.com` yetkili ad sunucuları (`ns1-ns4.google.com`) sorgulanmıştır. İzleme sırasında sistemde IPv6 yönlendirmesi bulunmadığından IPv6 adreslerine yapılan UDP istekleri başarısız olmuş, sorgu IPv4 üzerinden tamamlanmıştır.

### 3. Sistem DNS servis durumunun ve DNS sunucu adresinin incelenmesi

Ubuntu üzerinde aktif DNS istemci yapılandırmasını ve `systemd-resolved` servisinin kullandığı üst DNS sunucusunu doğrulamak için `resolvectl status` komutu çalıştırılmıştır.

Komut:

```bash
resolvectl status
```

Çıktı:

```text
Global
       Protocols: -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
resolv.conf mode: stub

Link 2 (ens5)
    Current Scopes: DNS
         Protocols: +DefaultRoute -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
Current DNS Server: 172.31.0.2
       DNS Servers: 172.31.0.2
        DNS Domain: eu-central-1.compute.internal
```

Ağ arayüzü `ens5` üzerinde upstream DNS sunucusu olarak AWS VPC DNS sunucusu olan `172.31.0.2` IP adresinin tanımlı olduğu ve `resolv.conf` dosyasının stub modunda çalıştığı tespit edilmiştir.

### 4. HTTP/2 Başlıklarının ve SSL/TLS Katmanının İncelenmesi

Hedef sunucuya HTTPS protokolü üzerinden istek atılarak dönen HTTP yanıt başlıkları ve sunucu davranışı incelenmiştir.

Komut:

```bash
curl -I [https://google.com](https://google.com)
```

Çıktı:

```text
HTTP/2 301 
location: [https://www.google.com/](https://www.google.com/)
content-type: text/html; charset=UTF-8
content-security-policy-report-only: object-src 'none';base-uri 'self';script-src 'nonce-PIjSgROdIRJug6WohRr7AA' 'strict-dynamic' 'report-sample' 'unsafe-eval' 'unsafe-inline' https: http:;report-uri [https://csp.withgoogle.com/csp/gws/other-hp](https://csp.withgoogle.com/csp/gws/other-hp)
date: Thu, 23 Jul 2026 08:07:29 GMT
expires: Sat, 22 Aug 2026 08:07:29 GMT
cache-control: public, max-age=2592000
server: gws
content-length: 220
x-xss-protection: 0
x-frame-options: SAMEORIGIN
alt-svc: h3=":443"; ma=2592000,h3-29=":443"; ma=2592000
```

Sunucu isteğe HTTP/2 protokolü ile `301 Moved Permanently` yanıtı vermiş ve istemciyi `https://www.google.com/` adresine yönlendirmiştir. `alt-svc` başlığında ise HTTP/3 (QUIC) desteği sunulduğu gözlemlenmiştir.

## Karşılaşılan Sorunlar ve Çözümleri

### IPv6 DNS Sorgularının Başarısız Olması

`dig google.com +trace` komutu çalıştırıldığında yetkili ad sunucularının IPv6 adreslerine yapılan bağlantılarda hata oluşmuştur.

Gerçek hata mesajı:

```text
;; UDP setup with 2001:501:b1f9::30#53(2001:501:b1f9::30) for google.com failed: network unreachable.
;; no servers could be reached
```

Hatanın teknik nedeni, EC2 sunucusunda yalnızca IPv4 ağ yapılandırmasının bulunması ve IPv6 dış ağ erişim/yönlendirme rotasının tanımlı olmamasıdır. `dig` aracı varsayılan olarak hem IPv4 hem IPv6 adreslerini denediğinden erişilemeyen IPv6 soketleri için bu uyarı verilmiştir.

Doğru kullanım:

Sorgunun yalnızca IPv4 protokolü üzerinden yapılması zorunlu kılınarak IPv6 zaman aşımları ve hata mesajları engellenebilir:

```bash
dig google.com +trace -4
```

## Öğrendiklerim

* `dig` komut çıktısında `NOERROR` durum kodunun sorgunun başarıyla tamamlandığını gösterdiğini öğrendim.
* Linux sistemlerde `127.0.0.53` adresinin local `systemd-resolved` stub resolver olarak görev yaptığını gözlemledim.
* `dig +trace` parametresi ile DNS çözümlemesinin sırasıyla Root (.), TLD (.com) ve Yetkili Ad Sunucuları seviyelerinde hiyerarşik olarak gerçekleştiğini inceledim.
* AWS EC2 VPC ortamında varsayılan DNS resolver IP adresinin subnet ağ adresinin 2. IP adresi (`172.31.0.2`) olarak atandığını doğruladım.
* Sunucuda IPv6 rotası tanımlı olmadığında UDP/53 üzerinden IPv6 DNS sorgularının `network unreachable` hatası verdiğini tespit ettim.
* `curl -I` komutu ile sunucudan dönen HTTP/2 301 yönlendirme başlıklarını ve HTTP/3 (`alt-svc`) destek bildirimlerini analiz ettim.

## Sonuç

Bu laboratuvarda, Linux/Ubuntu ortamında DNS sorgu süreçleri ve HTTP/2 protokol başlıkları teknik olarak incelenmiştir. `dig` sorguları ile local stub resolver ve özyinelemeli (iterative) DNS çözümleme adımları gözlemlenmiş; VPC seviyesinde DNS sunucu yapılandırması `resolvectl status` komutu ile doğrulanmıştır.

Testler sırasında sistemdeki IPv6 eksikliğinden kaynaklanan UDP bağlantı hataları analiz edilmiş ve çözüm yolları değerlendirilmiştir. Son aşamada `curl` aracı ile HTTPS bağlantısı kurulmuş, sunucu yanıt başlıkları ve yönlendirme mekanizmaları başarıyla incelenmiştir.
