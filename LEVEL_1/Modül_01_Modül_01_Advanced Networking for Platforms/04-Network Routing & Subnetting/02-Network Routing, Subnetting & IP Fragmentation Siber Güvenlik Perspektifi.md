# 🛡️ 5. Siber Güvenlik Boyutu: Teardrop Attack (Parçalanma İstismarı)

Saldırganlar, IP parçalanma (fragmentation) mekanizmasını işletim sistemlerini tamamen çökertmek için sinsi bir silaha dönüştürebilirler.

---

### 😈 Saldırı Mantığı (Overlapping Fragments)

Saldırgan, kurban sunucuya bilerek **Fragment Offset** değerleri birbiri üzerine binen (**overlapping**) sahte paket parçaları gönderir.



* **Çelişkili Veri:** Örneğin gönderilen ilk parça *"0 ile 100. byte'lar arası veri"* derken, hemen arkasından gelen ikinci parça *"50 ile 150. byte'lar arası veri"* iddiasıyla gelir.
* **Sistem Çöküşü:** Kurban sunucu bu parçaları işletim sisteminin RAM'inde birleştirmeye (reassembly) çalıştığında, ağ yığını (network stack) iki paket arasındaki çelişkiyi çözemez. Matematiksel bir hesap hatasına düşerek belleği taşır (**buffer overflow**) ve sunucu mavi ekran (BSOD) veya kernel panic vererek tamamen çöker. Bu saldırı türüne **Teardrop Attack** denir.

---

### 🛡️ Savunma ve Mitigasyon (Mitigation)

* **Güvenlik Duvarı Sıkılaştırma:** Modern güvenlik duvarları (Firewall) ve IPS/IDS sistemleri, hedefe ulaşmadan önce binen offset değerlerine sahip çelişkili paketleri tespit eder ve daha işletim sistemine ulaşmadan doğrudan drop eder (çöpe atar).
* **İşletim Sistemi Güncellemeleri:** Güncel tüm Linux, Windows ve macOS ağ yığınları, bu tarz hatalı hesaplamaları yakalayıp belleği taşırmadan paketleri güvenli bir şekilde reddedecek şekilde sıkılaştırılmıştır.
