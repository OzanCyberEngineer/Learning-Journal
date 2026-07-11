# Öğrenim Raporu: Linux Süreç (Process) Yönetimi ve Sinyal Mekanizmaları

Bu rapor; Linux işletim sistemindeki süreç mimarisini, süreçlerin izlenmesini, sinyal gönderim mantığını ve işlerin arka plana alınması senaryolarını incelemektedir.

---

## 1. Bölüm: Linux'ta Süreç (Process) Nedir?
Linux'ta çalıştırdığın her komut, açtığın her program arka planda bir Süreç (Process) başlatır. Windows'taki Görev Yöneticisi'nin arka plandaki listesi gibi düşünebilirsin. Linux, çalışan her işe benzersiz bir kimlik numarası yani **PID (Process ID)** verir.

### 🕵️‍♂️ Arka Planı İzleme Komutları:
* **`ps` (Process Status):** O an sadece senin terminalinde çalışan anlık süreçleri gösterir.
* **`ps aux` veya `ps -ef`:** Sistemde çalışan, gizlenen, arka planda dönen tüm kullanıcıların tüm süreçlerini devasa bir liste halinde döker. Siber güvenlikçiler şüpheli işlem avlarken hep `ps aux` kullanır.
* **`top` veya `htop`:** Sistem kaynaklarını (işlemci, ram) anlık ve canlı olarak gösteren dinamik bir panel açar. Çıkmak için `q` tuşuna basılır.

---

## 🎯 2. Bölüm: Sinyaller ve Süreç İnfazı (kill)
Bir süreci durdurmak veya kapatmak istediğimizde Linux'ta o sürece bir Sinyal (Signal) göndeririz. En sık kullanılan iki sinyal şudur:
* **SIGTERM (Sinyal 15):** Sürece kibarca "Lütfen işini bitir ve düzgünce kapan" der.
* **SIGKILL (Sinyal 9):** Süreci hiç dinlemez, anında, zorla ve acımasızca yok eder. Virüsleri kapatırken bunu kullanırız.

### 💀 İnfaz Komutu: `kill`
Eğer `ps aux` ile şüpheli bir program buldun ve PID numarasının 4532 olduğunu gördün. Onu zorla kapatmak için terminale şu komut yazılır:

```bash
kill -9 4532
Buradaki -9, acımasız SIGKILL sinyalini temsil eder.

⏳ 3. Bölüm: İşleri Arka Plana Atmak (&, bg, fg)
Linux'ta bazen çok uzun sürecek bir siber güvenlik taraması başlatırsın. Komut çalışırken terminal kilitlenir ve tarama bitene kadar yeni komut yazamazsın. Bunu engellemek için şu taktikler kullanılır:

Komutun sonuna & eklemek: Bir komutu çalıştırırken sonuna boşluk bırakıp & koyarsan, o iş direkt arka planda (background) başlar. Terminal kilitlenmez, yeni komutlar yazmaya devam edilir.

Ctrl + Z: Ön planda çalışan ve terminali kilitleyen bir işi anlık olarak dondurur (pause) ve arka plana atar.

bg (Background): Ctrl + Z ile dondurulan işi arka planda uykudan uyandırır ve çalıştırmaya devam ettirir.

fg (Foreground): Arka planda kendi kendine çalışan bir işi tekrar gözünün önüne, yani ön plana getirir.
