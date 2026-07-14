# ☁️ VXLAN ve Sanallaştırmanın Getirdiği Büyük Sıçrama

Geleneksel VLAN mimarilerinin bulut ekosistemlerindeki yetersizlikleri ve VXLAN tabanlı Overlay (Yazılımsal) ağların çalışma mantığı.

---

## 🛑 Sanallaştırma Duvarı: VLAN Neden Yetersiz Kaldı?

Dünya bulut teknolojilerine (AWS, Google Cloud) ve Kubernetes gibi devasa orkestrasyon sistemlerine geçtiğinde geleneksel VLAN mimarisi üç temel sebepten ötürü duvara tosladı:

* **Milyonlarca Kiracı (Multi-tenancy):** AWS gibi bir devde milyonlarca müşteri (tenant) bulunur ve her müşterinin sanal ağının diğerlerinden tamamen izole olması gerekir. 4094 adet olan VLAN limiti, bu devasa ölçekte adeta okyanusta bir damla gibi kaldı.
* **Fiziksel Altyapı Bağımlılığı:** VLAN'lar doğrudan fiziksel switch donanımlarına bağımlıdır. Bir sanal makine (VM) veya Kubernetes Pod'u bir fiziksel sunucudan diğerine taşındığında (*Live Migration*), ağ yapılandırmasının da switch seviyesinde anlık değişmesi gerekir ki bu dinamik bulut dünyasında imkansızdır.
* **Spanning Tree (STP) Belası:** Katman 2 (L2) ağlarda döngüleri (loop) engellemek için çalışan STP protokolü, yedek hatları tamamen kapatır. Bu durum, veri merkezindeki fiziksel ağ kablolarının ve bant genişliğinin yarısının atıl kalması anlamına gelir.

---

## 🛠️ Çözüm: VXLAN (Virtual Extensible LAN) ve Overlay Ağlar

VXLAN, fiziksel altyapıyı (**Underlay**) tamamen soyutlayarak, onun üzerinde tamamen yazılımsal bir sanal ağ (**Overlay**) kurma mantığıdır. Bu mimari çalışma biçimine literatürde **Mac-in-UDP** denir.

### Çalışma Mantığı
Bir VM veya Pod, başka bir sunucudaki Pod'a paket göndermek istediğinde sistem şu adımları izler:
1. Sistem, standart Katman 2 (L2) Ethernet paketini yakalar.
2. Bu orijinal paketin önüne bir **VXLAN Başlığı** ve standart bir **Katman 4 UDP Başlığı** ekler.
3. Alıcı fiziksel sunucunun IP adresini hedef olarak yazarak paketi fiziksel ağa fırlatır.
4. Fiziksel switch'ler paketin içindeki sanal ağ katmanıyla ilgilenmez; sadece standart bir UDP paketi taşıdıklarını varsayarak trafiği yönlendirirler.

### Temel Özellikler:
* **16 Milyon Sanal Ağ:** VXLAN başlığında 24-bitlik **VNI (VXLAN Network Identifier)** alanı kullanılır. Bu alan sayesinde oluşturulabilecek benzersiz izole ağ sayısı tam olarak şudur:
  $$2^{24} = 16.777.216$$
  Bu sayede 16 milyondan fazla benzersiz sanal ağ inşa edilebilir.
* **VTEP (VXLAN Tunnel Endpoint):** Paketleri kapsülleme (paketleme) ve kapsülden çıkarma işlemlerini gerçekleştiren uç noktalardır. Kubernetes altyapılarında kullanılan *Calico* veya *Flannel* gibi CNI (Container Network Interface) eklentileri, her bir Worker Node'u yazılımsal birer VTEP noktasına dönüştürür. Pod'lar fiziksel alt ağı hiç görmeden, doğrudan bu VTEP tünelleri (Overlay) üzerinden birbiriyle güvenle haberleşir.
