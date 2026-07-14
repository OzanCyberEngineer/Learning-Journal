# 🌐 VLAN’ın Anatomisi ve 802.1Q Standartı

Yerel ağ bölümleme, yayın alanı (broadcast domain) yönetimi ve switch'ler arası trunking mekanizmalarının temel çalışma mantığı.

---

## 🏢 1. VLAN’ın Anatomisi: Yerel Ağda Böl ve Yönet

Geleneksel bir ağda, switch'e bağlı her cihaz aynı **Broadcast Domain (Yayın Alanı)** içindedir. Bir bilgisayar ağa yeni katıldığında veya bir IP'nin MAC adresini öğrenmek istediğinde (ARP isteği) tüm ağa adeta bağırır: *"Bu IP kimde?"* Switch de bu paketi alır, gelen port hariç tüm portlara kopyalayarak iletir.

### Sorun: Yayın Fırtınaları ve Güvenlik Açıkları
* **Broadcast Storm (Yayın Fırtınası):** Eğer bir şirkette 500+ cihaz varsa ve hepsi sürekli broadcast yapıyorsa, ağ trafiği bir süre sonra kilitlenme noktasına gelir.
* **Güvenlik Zafiyeti:** İnsan Kaynakları departmanındaki bir bilgisayar ile Muhasebe departmanındaki bir bilgisayarın birbirlerinin paketlerini switch seviyesinde görebilmesi ciddi bir siber güvenlik açığıdır.

### Çözüm: Virtual LAN (VLAN)
Switch portlarını mantıksal olarak gruplandırırız:
* **Port 1-5:** ➔ `VLAN 10` (Muhasebe)
* **Port 6-10:** ➔ `VLAN 20` (İnsan Kaynakları)

Artık `VLAN 10`'daki bir cihaz broadcast gönderdiğinde, switch bu paketi sadece `VLAN 10`'a üye olan portlara iletir. Ağ trafiği rahatlar ve departmanlar arası ilk izolasyon duvarı örülmüş olur.

---

## ⚡ Switch'ler Arası İletişim ve 802.1Q Standartı

Elimizde iki farklı switch varsa ve bu switch'lerin ikisinde de `VLAN 10` üyeleri bulunuyorsa, her VLAN için switch'ler arasına ayrı fiziksel kablolar çekilmez. Bunun yerine switch'ler arasında, tüm VLAN trafiklerinin tek bir hat üzerinden geçebildiği bir **Trunk Port (Ortak Hat)** yapısı kurulur.

Trafik bu trunk hattan geçerken karışmasın diye Ethernet paketinin içine **802.1Q** adı verilen 4-baytlık bir etiket (tag) eklenir. Bu etiketin içindeki en kritik alan **VLAN ID** alanıdır:

* **Matematiksel Limit:** VLAN ID alanı tam 12-bit büyüklüğündedir. Bu da teorik olarak şu limit sınırını getirir:
  $$2^{12} = 4096$$
* **Kullanılabilir Sınır:** Geleneksel ağlarda en fazla **4094 adet kullanılabilir VLAN** oluşturulabilir (VLAN `0` ve VLAN `4095` rezerve edilmiştir).
