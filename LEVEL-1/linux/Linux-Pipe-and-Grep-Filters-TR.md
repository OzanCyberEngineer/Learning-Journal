# Öğrenim Raporu: Linux Boru Hattı (Pipe) Mimarisi ve Grep Metin Filtreleme Aracı

Bu rapor; Linux işletim sisteminde komutları birbirine bağlayan Pipe (`|`) mekanizmasını, güçlü metin arama aracı `grep` protokolünü ve bu ikilinin siber güvenlik log analizlerindeki pratik kullanım senaryolarını incelemektedir.

---

## 1. Pipe (`|`) Nedir ve Nasıl Çalışır?
Linux felsefesinin en temel kurallarından biri, her aracın yalnızca tek bir işi mükemmel yapmasıdır. Bu araçları bir araya getirerek karmaşık görevleri çözmek için **Pipe (Boru)** mekanizması kullanılır.
* **Çalışma Mantığı:** Pipe karakteri (`|`), kendisinden önce gelen komutun standart çıktısını (`stdout`), kendisinden sonra gelen komutun standart girdisi (`stdin`) olarak bağlar. Komutlar arasında dijital bir veri köprüsü kurar.

---

## 2. `grep` (Global Regular Expression Print) Nedir?
`grep`, bir dosya veya veri akışı içinde belirli bir kalıbı (pattern), kelimeyi veya düzenli ifadeyi (Regex) arayarak sadece eşleşen satırları ekrana getiren gelişmiş bir filtreleme aracıdır.



### Siber Güvenlik Analizlerinde Kritik `grep` Parametreleri:
* **`grep -i "[ifade]"` (Case Insensitive):** Büyük/küçük harf duyarlılığını devre dışı bırakır. Örneğin, `grep -i "admin"` komutu "Admin", "ADMIN" ve "aDmIn" ifadelerinin tamamını yakalar.
* **`grep -v "[ifade]"` (Invert Match):** Ters filtreleme yapar. Belirtilen ifadenin **geçmediği** satırları listeler. Log analizlerinde bilinen güvenli/meşru trafikleri elemek ve şüpheli etkinlikleri izole etmek için kullanılır.
* **`grep -r "[ifade]" [dizin]` (Recursive):** Belirtilen klasörün altındaki tüm dosyaların içinde derinlemesine arama yapar.

---

## 3. Siber Güvenlik ve Log Analizi Pratik Senaryosu
Bir siber güvenlik analisti olarak elinde `auth.log` (sistem giriş logları) dosyası olduğunu varsayalım. Bu dosyada binlerce satır veri bulunur. Pipe ve `grep` kombinasyonunu kullanarak tehdit avcılığı (Threat Hunting) şu şekilde yapılır:

### Örnek 1: Başarısız Giriş Denemelerini Filtreleme
```bash
cat /var/log/auth.log | grep "Failed password"
