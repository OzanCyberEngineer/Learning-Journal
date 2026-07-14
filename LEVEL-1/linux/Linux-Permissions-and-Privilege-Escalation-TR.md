# Öğrenim Raporu: Linux Yetki Mantığı ve Yetki Yükseltme Temelleri

Bu rapor; Linux işletim sistemindeki dosya izin mekanizmasını, sayısal yetki değerlerini ve bu yapılandırmaların siber güvenlik (yetki yükseltme) perspektifinden analizini incelemektedir.

---

## 🔐 1. Bölüm: Linux'ta Yetki Mantığı (Kim Bu Kullanıcılar?)
Linux, Windows'a göre çok daha katı bir "Aramıza mesafe koyalım" sistemidir. Güvenlik duvarı daha dosya seviyesinde başlar. Bir dosya oluşturulduğunda Linux ona 3 farklı grup için yetki yazar:
* **User (u):** Dosyanın sahibi olan tek bir kişi (Örneğin sen: okeanos).
* **Group (g):** Aynı yetkilere sahip kullanıcıların toplandığı departman (Örneğin: siber_ekip).
* **Others (o):** Sistemdeki diğer herkes (Geri kalan tüm dünya).

Ve bu 3 grup için sadece 3 temel izin tanımlanabilir:
* **r (Read - Okuma):** Dosyanın içeriğine cat ile bakabilir mi? (Matematiksel değeri: 4)
* **w (Write - Yazma):** Dosyayı düzenleyebilir veya silebilir mi? (Matematiksel değeri: 2)
* **x (Execute - Çalıştırma):** Bu dosya bir program/betik ise, çalıştırabilir mi? (Matematiksel değeri: 1)

---

## 🔢 2. Bölüm: "755" veya "644" Sihri (Rakamların Anlamı)
Siber güvenlik dokümanlarında hep şu tarz şeyler görürsün: "Dosyaya 777 yetkisi ver", "Şu exploit'i 755 yap". Burada tamamen basit bir toplama işlemi döner:
* **7** = $4 \text{ (Read)} + 2 \text{ (Write)} + 1 \text{ (Execute)}$ ➡️ (Her şeye izni var, tam yetki)
* **5** = $4 \text{ (Read)} + 0 + 1 \text{ (Execute)}$ ➡️ (Okur ve çalıştırır ama içeriği değiştiremez)
* **4** = $4 \text{ (Read)} + 0 + 0$ ➡️ (Sadece okuyabilir, başka hiçbir şey yapamaz)

Komut satırında yan yana 3 rakam yazdığında sırasıyla User | Group | Others anlamına gelir.

> **💡 Örnek - chmod 755 siber.sh:** Dosyanın sahibi (7) her şeyi yapar; gruptakiler (5) ve dünyadaki diğer herkes (5) sadece okur ve çalıştırır.

---

## 🏴‍☠️ 3. Bölüm: Siber Güvenlik Bakış Açısı (Yetki Yükseltme)
Bir hacker sisteme sızdığında genellikle okeanos gibi düşük yetkili, sıradan bir kullanıcı haklarıyla içeri girer. Ancak sıradan kullanıcılar sistem dosyalarını değiştiremez, logları silemez. Amaç, sistemin mutlak hakimi olan Root (Süper Kullanıcı) yetkisine ulaşmaktır. Buna siber güvenlikte **Privilege Escalation (Yetki Yükseltme)** denir.

* **Tehlikeli 777 Hatası:** Acemi sistem yöneticileri bir program veya web sitesi çalışmadığında üşenip klasöre chmod 777 çakarlar. Bu, dünyadaki herkesin o dosyayı silebileceği veya içine zararlı kod enjekte edebileceği anlamına gelir. Hackerlar sistemde 777 yetkili dosya aramaya bayılırlar.

---

## 🧪 4. Bölüm: Terminalde Nasıl Görünür?
Linux'ta bir klasörün içindeyken `ls -l` (detaylı listeleme) yazdığında sol tarafta şöyle çizgiler görürsün:

```bash
-rwxr-xr-x 1 okeanos siber_ekip 1024 Jul 11 hack_araci.sh
En baştaki o harfleri üçer üçer ayırarak oku:

rwx ➡️ Sahibi (okeanos) okur, yazar, çalıştırır (7).

r-x ➡️ Grubu (siber_ekip) okur, çalıştırır (5).

r-x ➡️ Diğer herkes okur, çalıştırır (5).

Eğer bir dosyayı çalıştırılabilir yapmak istiyorsan terminalde yazacağın sihirli komut şudur:

Bash
chmod +x dosya_adi.sh
Bu komut dosyaya direkt çalıştırma izni enjekte eder.
