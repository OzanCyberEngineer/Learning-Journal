# Öğrenim Raporu: Linux Dosya Sistemi Mimarisi ve Temel Navigasyon Komutları

Bu rapor; Linux işletim sisteminin hiyerarşik dosya yapısını, temel dizin gezinme komutlarını ve siber güvenlik operasyonlarında ilk keşif (Reconnaissance) esnasında bu komutların rolünü incelemektedir.

---

## 1. Linux Dosya Sistemi Felsefesi
Windows'tan farklı olarak Linux mimarisinde `C:` veya `D:` gibi mantıksal sürücü kavramları yoktur. Her şey en tepedeki tek bir kök dizinden (**`/` - Root**) başlar ve bir ağaç gibi aşağıya doğru dallanır. Siber güvenlikte *"Linux'ta her şey bir dosyadır (Everything is a file)"* kuralı geçerlidir; donanımlar, süreçler ve ağ kartları bile bu ağacın altında birer dosya olarak temsil edilir.



---

## 2. Temel Navigasyon ve Keşif Komutları

Siber güvenlik analizlerinde bir sisteme ilk erişim sağlandığında (ister terminal üzerinden bir sunucu yönetimi, ister sızma testinde elde edilen bir shell olsun) durum tespiti için şu üç temel komut kullanılır:

### A. `pwd` (Print Working Directory)
* **Tanım:** Kullanıcının o anda Linux dosya ağacının tam olarak hangi dalında (klasöründe) bulunduğunu gösterir.
* **Güvenlik Karşılığı:** Saldırı veya savunma esnasında çalıştırılacak scriptlerin/komutların hangi dizin bağlamında yürütüleceğini bilmek, yanlışlıkla sistem dosyalarına zarar vermeyi önler.

### B. `ls` (List)
* **Tanım:** Mevcut dizinin içinde bulunan dosya ve klasörleri listeler.
* **Kritik Parametre (`ls -la`):** Gizli dosyaları (başında nokta olan `.bash_history` gibi) ve dosyaların izin/sahiplik durumlarını detaylıca gösterir. Hackerların bıraktığı gizli arka kapıları (backdoor) yakalamak için `ls -la` sıklıkla kullanılır.

### C. `cd` (Change Directory)
* **Tanım:** Klasörler arasında geçiş yapmayı (ışınlanmayı) sağlar. Örneğin `cd /etc` komutu, sistemin kalbine geçişi sağlar.

---

## 3. Sistemin Kalbi: `/etc` Dizini
Yapılan pratik çalışmada `cd /etc` komutu ile geçiş yapılan bu dizin, Linux işletim sisteminin en kritik noktalarından biridir.
* **Önemi:** Bu dizin, sistemin ve çalışan tüm servislerin (SSH, Web Server, Firewall vb.) **yapılandırma (configuration) dosyalarını** barındırır.
* **Siber Güvenlik Perspektifi:** Bir sistem yöneticisi bu dizini sıkılaştırmak (hardening) zorundadır. Saldırganlar ise bu dizindeki dosyaları (Örn: `/etc/passwd`) okuyarak sistemdeki kullanıcı adlarını ifşa etmeye (enumeration) çalışırlar.
