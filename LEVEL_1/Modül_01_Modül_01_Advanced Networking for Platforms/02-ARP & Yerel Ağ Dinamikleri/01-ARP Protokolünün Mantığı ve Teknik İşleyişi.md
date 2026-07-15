# 🔌 Modül 01 / Konu 02: ARP Protokolü, Tablosu ve Ağ Dinamikleri

Bir gün sana junior bir mühendis gelip *"Ağda IP adresi varken neden bir de MAC adresine ve ARP'ye ihtiyaç duyuyoruz?"* diye sorarsa, ona şu hikayeyi anlat:

> 🏢 **Analoji: Plaza ve Çalışanlar**
> 
> Büyük bir plazada (yerel ağ / switch) çalıştığını düşün. Plazada her çalışanın bir TC Kimlik Numarası (IP Adresi) ve bir de fiziksel olarak oturduğu Masa Numarası (MAC Adresi) var.
> 
> Sen Ahmet'e (IP: 192.168.1.20) acil bir evrak göndermek istiyorsun. Ahmet'in TC kimlik numarasını biliyorsun ama plazada hangi masada oturduğunu (MAC adresi) bilmiyorsun. Plazadaki güvenlik görevlisi (Switch) ise sadece masaların yerini bilir, kimsenin TC kimliğiyle ilgilenmez.
> 
> Sen plazanın ortasına çıkıp avazın çıktığı kadar bağırırsın (**Broadcast - ARP Request**):
> *"TC kimlik numarası 192.168.1.20 olan Ahmet kim? Hangi masada oturuyorsun?!"*
> 
> Plazadaki herkes bu sesi duyar ama sadece Ahmet üzerine alınır ve sana sessizce cevap verir (**Unicast - ARP Reply**):
> *"Ben buradayım! Masa numaram: Masa-F24"*.
> 
> Sen bu bilgiyi unutmamak için hemen cebindeki not defterine (**ARP Table / Cache**) yazarsın: *"Ahmet = Masa-F24"*. Böylece her evrak göndereceğinde plazanın ortasında bağırmak zorunda kalmazsın.
---
# ⚙️ 2. Protokolün Teknik İşleyişi

Şimdi bu hikayeyi bit seviyesinde teknik gerçekliğe dönüştürelim. Bir bilgisayar yerel ağda bir paket göndereceğinde şu adımları izler:

---

### 📍 Adım 1: ARP Cache (Tablo) Kontrolü
İşletim sistemi, hedef IP'ye ait bir MAC adresinin halihazırda RAM'de tutulan ARP tablosunda olup olmadığını kontrol eder.

* **Eğer kayıt varsa:** L3 paketini L2 çerçevesinin (Frame) içine koyar ve gönderir.
* **Eğer kayıt yoksa:** Paketi beklemeye (queue) alır ve bir ARP Request hazırlar.

---

### 📍 Adım 2: ARP Request (İstek - Broadcast)
* **Kaynak IP:** `192.168.1.10` | **Kaynak MAC:** `00:AA:BB:11:22:33`
* **Hedef IP:** `192.168.1.20` | **Hedef MAC:** `FF:FF:FF:FF:FF:FF` (Ağdaki her cihaza gitmesi için rezerve edilmiş Broadcast adresi)

Switch bu hedef MAC'i (`FF:FF:FF:FF:FF:FF`) gördüğü an, gelen port hariç tüm aktif portlara bu paketi kopyalar.

---

### 📍 Adım 3: ARP Reply (Cevap - Unicast)
* Ağdaki diğer cihazlar paketi açar, "Hedef IP" alanında kendi IP'lerini görmeyince paketi sessizce çöpe atar.
* Ancak `192.168.1.20` IP'li cihaz paketi tanır. Kendi ARP tablosuna da istek atan cihazın IP-MAC bilgisini kaydeder (ileride o da konuşacak çünkü). Ardından sadece isteği gönderen cihaza yönlendirilmiş bir cevap hazırlar:

* **Kaynak IP:** `192.168.1.20` | **Kaynak MAC:** `00:AA:BB:44:55:66`
* **Hedef IP:** `192.168.1.10` | **Hedef MAC:** `00:AA:BB:11:22:33` (Doğrudan hedefli - Unicast)

> 💡 **Yönlendirme Dinamiği:**
> Switch bu sefer hedef MAC'i bildiği için paketi sadece ilgili istemcinin bağlı olduğu porta yönlendirir. İstemci bu adresi tablosuna yazar.
