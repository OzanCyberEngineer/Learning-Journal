# 🛡️ 4. Siber Güvenlik Boyutu: VLAN Hopping (Double Tagging) Saldırısı

Bir saldırgan, kendi bağlı olduğu **VLAN 10**'dan, normalde erişiminin kesinlikle yasak olduğu **VLAN 20**'ye (örneğin veritabanı ağına) router'a uğramadan sızmak isteyebilir.

---

### 😈 Saldırı Mantığı (Double Tagging)



1. **Çift Etiketleme (Double Tagging):** Saldırgan, bilgisayarından çıkardığı pakete üst üste iki tane VLAN etiketi yapıştırır. En dıştaki etikete kendi VLAN'ı olan **VLAN 10 (Native VLAN)**, içteki etikete ise hedeflediği **VLAN 20**'yi yazar.
2. **İlk Switch Aşaması:** İlk switch paketi aldığında dıştaki VLAN 10 etiketini okur, *"Bu benim Native VLAN'ım"* diyerek etiketi söker ve paketi Trunk porta doğru yönlendirir.
3. **Sızma Aşaması:** Paketin içindeki ikinci etiket olan **VLAN 20** artık en dışta kalmıştır. Bir sonraki switch bu paketi aldığında etiketinde sadece VLAN 20 gördüğü için paketi doğrudan VLAN 20 ağındaki kurbana teslim eder. Router kuralları ve firewall'lar tamamen bypass edilmiş olur.

---

### 🛡️ Savunma ve Sıkılaştırma (Hardening)



* **Native VLAN Değişimi:** Trunk portlar üzerindeki **Native VLAN ID'si**, kullanıcıların ve saldırganların bağlı olduğu hiçbir VLAN ile aynı yapılmamalıdır. Kullanılmayan sahte/özel bir VLAN ID'sine (Örn: **VLAN 999**) set edilmelidir.
* **Port Güvenliği:** Kullanılmayan tüm switch portları kapatılmalı (`shutdown`) ve varsayılan VLAN'lardan (VLAN 1) çıkarılmalıdır.
* **Etiketleme Zorunluluğu (Native VLAN Tagging):** Global olarak Native VLAN trafiğinin de her koşulda etiketlenmesi (`switchport trunk native vlan tag`) switch üzerinde aktif edilmelidir. Böylece switch, native VLAN etiketini hemen söküp paketi korumasız bırakmaz.
