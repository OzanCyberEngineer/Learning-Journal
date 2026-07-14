# 🛡️ Siber Tehdit Dünyası ve Olay Yönetim Metodolojisi

Siber savunmanın temel taşı, savaştığınız düşmanı iyi tanımaktır. Güvenli bir altyapı inşa etmek için tehdit aktörlerinin motivasyonlarını bilmeli, saldırı adımlarını haritalandırmalı ve standartlara uygun olay müdahale süreçlerini yönetebilmeliyiz.

---

## 👥 1. Tehdit Aktörleri: Sınıflandırma ve Motivasyonlar

Siber dünyadaki saldırganlar tek bir tip değildir. Teknik yeteneklerine, bütçelerine ve amaçlarına göre ayrılırlar:

* **Script Kiddie (Hevesli Amatörler):** Hazır hack araçlarını ne işe yaradığını tam bilmeden kullanan, genellikle ses getirmek veya sadece hava atmak isteyen kitle.
* **Hacktivist:** Siyasi, sosyal veya dini bir amacı yaymak için siber saldırı düzenleyen gruplar (Örn: *Anonymous*). Genellikle web sitesi çökertme veya sızıntı yapma odaklıdırlar.
* **Siber Suçlular (Cyber Criminals):** Tek motivasyonları para olan profesyonel yapılar. Genellikle fidye yazılımları (Ransomware) ile sistemleri kilitleyip fidye isterler.
* **APT (Advanced Persistent Threat - Gelişmiş Israrlı Tehditler):** Devlet destekli, sınırsız bütçeli, yıllarca fark edilmeden sistemlerde casusluk yapabilecek kapasitede olan askeri veya istihbari hacker grupları. Savunma sanayisi ve kritik altyapılardaki en büyük hedef, bu tehlikeli aktörleri tespit edip engellemektir.

---

## 🎯 2. Saldırı Metodolojisi: Siber Ölüm Zinciri (Cyber Kill Chain)

Profesyonel bir saldırgan (özellikle bir APT grubu) sisteme asla rastgele saldırmaz. Dünyada kabul görmüş standart bir siber saldırı zinciri vardır. Buna **Cyber Kill Chain** denir ve tam 7 adımdan oluşur:


[Keşif] ➔ [Silahlandırma] ➔ [İletim] ➔ [Sızma] ➔ [Kurulum] ➔ [Komuta Kontrol] ➔ [Hedef]

Reconnaissance (Keşif): Hedef hakkında bilgi toplama. (Örn: Açık portların tespiti, çalışanların e-postaları, kullanılan yazılım versiyonları).

Weaponization (Silahlandırma): Keşif aşamasında bulunan bir açığa uygun zararlı yazılımı (Exploit/Malware) hazırlama ve paketleme.

Delivery (İletim): Bu silahı hedefe ulaştırma (Örn: Oltalama e-postası gönderme veya enfekte bir USB kullanma).

Exploitation (Sızma/İstismar): Hedef sistemdeki güvenlik açığını tetikleyerek içeriye ilk adımı atma.

Installation (Kurulum): İçeride kalıcı olabilmek için arka kapı (Backdoor) yerleştirme veya sistem ayarlarını sabote etme.

Command & Control (C2 - Komuta Kontrol): İçerideki zararlı yazılımla dışarıdaki hacker sunucusu arasında bağlantı kurarak sistemi uzaktan yönetmeye başlama.

Actions on Objectives (Hedefe Ulaşma): Son darbe. Verileri çalma (Exfiltration), sistemleri sabote etme veya fidye yazılımı ile şifreleme.

---
## 🚨 3. Savunma Perspektifi ve Olay Yönetimi (Incident Management)
Bir güvenlik mühendisi olarak amacımız, bu 7 adımdan ne kadar erken bir aşamada saldırganı yakalarsak, hasarı o kadar minimumda tutmaktır.

Olay (Incident) Nedir? Sistemlerimizde normal akışın dışına çıkan, bilgi güvenliğini tehdit eden her türlü şüpheli durumdur (Örn: Gece 03:00'te bir kullanıcının üst üste 100 kez yanlış şifre girmesi).

Olay Müdahale (Incident Response - IR) Süreci:

Sınırlandırma (İzolasyon): Saldırganın diğer sistemlere sıçramasını engellemek için tehlike altındaki sunucunun ağ bağlantısını derhal kesmek.

İnceleme (Kök Neden Analizi): Linux üzerindeki /var/log/ log dosyalarını, firewall veya sistem loglarını inceleyerek saldırganın içeriye nereden sızdığını (Root Cause) bulmak.

Temizleme ve Kurtarma: Tespit edilen açığı kapatmak, sistemi zararlılardan arındırmak ve güvenli yedeklerden sistemi tekrar ayağa kaldırmak.
