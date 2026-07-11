# Öğrenim Raporu: Linux Dosya/Klasör Yönetimi ve Temel Log Okuma Komutları

Bu rapor; Linux işletim sisteminde dosya ve dizin (klasör) operasyonlarını, veri silme mekanizmalarının risklerini ve siber güvenlik analizlerinde log dosyalarını incelemek için kullanılan temel dosya okuma komutlarını ele almaktadır.

---

## 1. Klasör ve Dosya Operasyonları

Linux'ta grafik arayüz (GUI) olmadan, tamamen komut satırı üzerinden dosya sistemi mimarisini yönetmek için aşağıdaki core komutlar kullanılır:

### A. Klasör Yönetimi
* **`mkdir [klasör_adı]` (Make Directory):** Belirtilen dizinde yeni ve boş bir klasör oluşturur.
* **`rmdir [klasör_adı]` (Remove Directory):** Sadece **içi tamamen boş** olan klasörleri silmek için güvenli bir yöntemdir. Klasör içinde tek bir dosya bile varsa hata verir.

### B. Dosya Yönetimi ve Riskli Komutlar
* **`touch [dosya_adı]`:** Belirtilen isimde 0 bayt boyutunda, boş bir metin dosyası oluşturur veya mevcut bir dosyanın zaman damgasını (timestamp) günceller.
* **`rm [dosya_adı]` (Remove):** Dosyayı sistemden kalıcı olarak siler. 
    * *Kritik Güvenlik Notu:* Linux CLI mimarisinde yerleşik bir "Geri Dönüşüm Kutusu" (Trash Bin) yoktur. Komut yürütüldüğü an veri blokları üzerindeki işaretçiler silinir.
* **`rm -rf [hedef]` (Recursive & Force):** Sistemdeki en tehlikeli komutlardan biridir. Klasör dolu olsa dahi, kullanıcıya hiçbir onay sormadan (`-f`) ve tüm alt dizinleri kapsayacak şekilde (`-r`) zorla siler. Yetkili (Root) kullanıcının yanlış dizinde bu komutu çalıştırması sistemi tamamen yok edebilir.

---

## 2. Siber Güvenlik Analizinde Dosya ve Log Okuma Yöntemleri

Mavi Takım (Blue Team) analistleri ve sızma testi uzmanları, sistem konfigürasyonlarını (`/etc/passwd`, sunucu ayarları vb.) ve olay günlüklerini (logs) incelemek için farklı okuma modları kullanır:



* **`cat [dosya_adı]` (Concatenate):** Dosya içeriğinin tamamını tek bir hamlede terminal ekranına basar. Küçük boyutlu yapılandırma dosyalarını hızlıca okumak için idealdir, ancak devasa log dosyalarında terminal belleğini şişirir.
* **`less [dosya_adı]`:** Büyük boyutlu dosyalar ve loglar için optimize edilmiştir. Dosyanın tamamını RAM'e yüklemek yerine sayfa sayfa açar. Ok tuşları ile satır satır gezinmeyi sağlar. `q` tuşu ile oturum kapatılır.
* **`head -n [satır_sayısı] [dosya_adı]`:** Bir dosyanın yalnızca belirtilen sayıdaki **ilk satırlarını** (varsayılan 10) gösterir. Dosya başlığındaki meta verileri kontrol etmek için uygundur.
* **`tail -n [satır_sayısı] [dosya_adı]`:** Bir dosyanın sadece **son satırlarını** ekrana getirir.
    * *Mavi Takım İpucu (`tail -f`):* `-f` (follow) parametresi ile kullanıldığında (`tail -f /var/log/auth.log`), log dosyasına anlık olarak düşen yeni satırları canlı olarak ekrana yansıtır. Canlı siber saldırı analitiğinde en çok kullanılan komuttur.
