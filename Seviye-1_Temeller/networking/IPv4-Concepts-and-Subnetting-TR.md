# Öğrenim Raporu: IPv4 Mimarisi, Alt Ağlara Bölme (Subnetting) ve Ağ Bölümleme Güvenliği

Bu rapor; Katman 3 (Ağ Katmanı) seviyesindeki mantıksal adresleme mantığını, CIDR yapısını, iç/dış IP ayrımını ve bu yapıların ağ güvenliği ile savunma stratejilerindeki rolünü incelemektedir.

---

## 1. IPv4 Adreslemesi ve Anatomisi
Fiziksel ve değişmez olan MAC adreslerinin aksine, IP (Internet Protocol) adresleri cihazların ağ üzerindeki mantıksal yerini belirleyen dinamik "posta adresleri" gibidir. İnsan gözünün okuyabilmesi için noktalı onluk (dotted-decimal) formatta yazılsa da bilgisayarlar arka planda bunu ikilik (binary) sistemde işler.

* **Örnek:** `192.168.1.5` -> `11000000.10101000.00000001.00000101`

Bir IP adresi her zaman iki ana bileşenden oluşur:
1.  **Network ID (Ağ Kimliği):** Cihazın hangi alt ağda veya organizasyonda yer aldığını belirtir.
2.  **Host ID (Cihaz Kimliği):** O ağ içerisindeki spesifik cihazı tanımlar.

---

## 2. Subnetting (Alt Ağlara Bölme) ve CIDR Mantığı
Büyük bir yapıda tüm cihazları tek bir ağ (broadcast domain) içerisine koymak iki büyük probleme yol açar:
* **Gürültü (Broadcast Storms):** ARP istekleri gibi tüm ağa yayılan paketler, gürültü oluşturarak ağ performansını düşürür ve kilitlenmelere yol açar.
* **Güvenlik Zafiyeti:** Ağ bölümlemesi yapılmadığında, düşük yetkili bir kullanıcı (örneğin bir stajyer veya misafir), kritik departmanların (IK, Finans, Siber Güvenlik) ağ trafiğini koklayabilir (sniffing).

**Subnetting**, büyük bir ağı daha küçük, yönetilebilir ve güvenli alt ağlara bölme sanatıdır.

### Subnet Mask (Alt Ağ Maskesi) ve CIDR
Bir IP adresinin hangi kısmının ağa, hangi kısmının cihaza ait olduğunu **Subnet Mask** belirler. 
* `255.255.255.0` maskesi, ilk 3 oktetin ağ kimliği olduğunu gösterir. 
* Bunu siber güvenlik dökümanlarında uzun uzun yazmak yerine **CIDR (Classless Inter-Domain Routing)** gösterimiyle eğik çizgi kullanarak yazarız (Örn: `/24`). Buradaki 24 sayısı, binary gösterimdeki yan yana gelen "1" adetlerini ifade eder (`11111111.11111111.11111111.00000000`).

---

## 3. İç (Private) ve Dış (Public) IP Ayrımı ile NAT
İnternet trafiğinin analizi ve güvenlik duvarı (Firewall) kuralları için bu ayrım hayati önem taşır.
* **Public IP:** İnternet üzerinde dünya genelinde benzersiz olan, doğrudan dış dünyaya açık ve yönlendirilebilen yasal IP adresleridir.
* **Private IP:** Sadece yerel ağlarda (LAN) ücretsiz ve tekrar tekrar kullanılabilen adreslerdir (Örn: `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`). Bu IP'ler internete doğrudan çıkamaz.
* **NAT (Network Address Translation):** Yerel ağdaki birçok cihazın (iç IP'lerin), dış dünyaya tek bir yasal Public IP üzerinden çıkmasını sağlayan yönlendirici (Router/Modem) teknolojisidir.

---

## 4. Siber Güvenlik Perspektifi (Savunma Stratejileri)

### Ağ Bölümleme (Network Segmentation)
Güvenli bir mimaride, dış dünyaya açık olan Web Sunucuları ile kritik Veritabanı (Database) sunucuları aynı ağa konulmaz. Web sunucusu `10.0.1.X` alt ağına, veritabanı ise `10.0.5.X` alt ağına yerleştirilir ve araya sıkı kural setlerine sahip bir **Firewall (Güvenlik Duvarı)** konumlandırılır. Böylece web sunucusu hacklense bile saldırganın iç ağda yatayda hareket etmesi (lateral movement) engellenir.

### IP Spoofing ve Firewall Sınır Koruması
Saldırganlar, özellikle DDoS saldırılarında kimliklerini gizlemek için paketlerin "Kaynak IP" (Source IP) kısmını sahte IP'lerle değiştirirler (IP Spoofing). Bilinçli bir güvenlik mühendisi, Firewall üzerinde *"Dış dünyadan (WAN bacağından) gelen bir paketin kaynak IP'si asla RFC 1918 (192.168.x.x, 10.x.x.x) iç IP bloklarımız olamaz"* kuralını yazar ve bu sahte paketleri anında engeller.

---

## 5. Mühendislik Çıkarımı ve Analiz
İç ağdan dış ağa yapılan trafik geçişleri (NAT süreçleri), siber savunmanın en kritik sınır hattıdır (Edge Security). İçerideki bir cihazın (Cihaz A) internetteki bir sunucuya istek atıp yanıt beklemesi esnasında, modemin/router'ın durum tablosunu (State Table) takip eden saldırganlar trafiği manipüle etmeye veya kötü amaçlı paket enjekte etmeye çalışabilir.

Bu sınır hattını korumak için şu savunma mekanizmaları aktif olarak uygulanmalıdır:
1.  **Stateful Inspection (Durumsal İnceleme):** Güvenlik duvarları, dışarıdan gelen bir paketin gerçekten içerideki bir cihaz tarafından başlatılan bir isteğe yanıt olup olmadığını sıkı bir şekilde doğrulamalıdır. İçeriden başlatılmayan hiçbir dış istek içeri alınmamalıdır.
2.  **SPI (Stateful Packet Inspection) ve NAT Güvenliği:** Modemin NAT/PAT (Port Address Translation) tablolarının manipüle edilmesini (NAT Injection/Bleeding) önlemek için güncel yazılımlar (firmware) kullanılmalı ve sınır koruma cihazlarında şüpheli veri akışlarını engelleyen IDS/IPS sistemleri konumlandırılmalıdır.
