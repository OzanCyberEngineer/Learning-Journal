# 🏆 Global System, DevOps & Platform Engineering Roadmap

Bu depo, sıfırdan başlayarak küresel standartlarda (SRE, DevOps, Platform Mühendisliği) uzmanlık seviyesine ulaşmak için oluşturulmuş teorik, pratik ve siber güvenlik odaklı mühendislik kütüphanemdir. Her modül mülakat standartlarına ve canlı üretim ortamı (production-grade) mimarilerine göre tasarlanmıştır.

---

## 🗺️ PROJE İLERLEME HARİTASI (ROADMAP)

### 🌐 LEVEL 1: FOUNDATIONS OF SYSTEMS & SECURITY (Sistem ve Güvenlik Temelleri)
> Sunucu odasının, işletim sisteminin ve veri iletiminin kalbine indiğimiz temel seviye.

#### 🌐 Modül 01: Advanced Networking for Platforms `[BEKLEMEDE ⏳]`
* [x] [**Physical & Data Link Layers:** OSI vs TCP/IP, MAC Adresi, Fiziksel Sinyaller, **MTU (Maximum Transmission Unit)** mantığı.](./LEVEL_1/Modül_01_Modül_01_Advanced%20Networking%20for%20Platforms/Physical%20&%20Data%20Link%20Layers)
* [ ] **ARP & Yerel Ağ Dinamikleri:** ARP Protokolü, ARP Table, ARP Poisoning (Zehirlenme), **DAI (Dynamic ARP Inspection)**.
* [ ] **VLAN (Virtual LAN) & VXLAN:** Klasik L2 izolasyonu, VLAN Hopping / Double Tagging saldırıları, **VXLAN Overlay (Tünel) mimarisi**, VTEP ve VNI kavramları.
* [ ] **Network Routing & Subnetting:** IPv4 Yapısı, CIDR, Subnetting (Alt ağ hesaplama), Gateway, **IP Fragmentation (Parçalanma)**, Static vs Dynamic Routing (BGP/OSPF mantığı).
* [ ] **Transport Layer & Traffic Control:** TCP vs UDP, Portlar, TCP 3-Way Handshake, Windowing, Congestion Control (Tıkanıklık kontrolü).
* [ ] **Application Layer Services:** DNS Altyapısı (A, CNAME, TXT, MX), DNS Spoofing, **DHCP & DORA Süreci, DHCP Snooping Güvenliği**, HTTP/HTTPS ve TLS/SSL Handshake detayları.
* [ ] **Load Balancing & Proxy Concepts:** Forward Proxy vs. Reverse Proxy, L4 (TCP/UDP) vs. L7 (HTTP) Load Balancing mantığı, SSL Termination vs SSL Passthrough.
---
#### 🐧 Modül 02: Enterprise Linux System Administration `[BEKLEMEDE ⏳]`
* [ ] **Linux File System & FHS:** Klasör hiyerarşisi (`/etc`, `/var`), sanal dosya sistemleri (`/proc`, `/sys`).
* [ ] **Text & Stream Manipulation:** Pipes (`|`), `grep`, `awk`, `sed` filtrelemeleri.
* [ ] **User & Permission Management:** User/Group mantığı, `chmod`, `chown`, **POSIX ACLs (Gelişmiş izinler)**, `Sudoers` yapılandırması.
* [ ] **Process & Resource Management:** `ps`, `top`, `kill`, **Systemd & Systemctl (Servis mimarisi)**, arka plan süreçleri.
* [ ] **Storage & Disk Management:** **LVM (Logical Volume Manager)** ile dinamik disk büyütme, Dosya sistemleri (ext4, XFS), Mount işlemleri.
* [ ] **Linux Networking & Troubleshooting:** IP tanımlama, `/etc/resolv.conf`, `hosts` dosyası, `ip route`, `ss`, `netstat`, `tcpdump` ve `dig` kullanımı.
---
#### 🛡️ Modül 03: Security Engineering & Incident Management `[BEKLEMEDE ⏳]`
* [ ] **Threat Modeling & Attacks:** Cyber Kill Chain, APT'ler, DoS/DDoS (SYN Flood), MITM.
* [ ] **Malware & System Defense:** Malware türleri (Worm, Trojan, Sandbox), Forensics için RAM'in önemi, **Linux Hardening Standartları**.
* [ ] **Firewalling:** **iptables** ve **nftables** çalışma mantığı, zincirler (chains) ve kural tanımlama.
* [ ] **System Auditing & Logging:** `/var/log` analizi, **Auditd (Linux denetim sistemi)**, fail2ban mantığı.
* [ ] **Cryptography & SSH Security:** Simetrik/Asimetrik Şifreleme, SSH Key-Pair mantığı, SSH Hardening (`sshd_config` sıkılaştırma).

---

### 📦 LEVEL 2: INFRASTRUCTURE AS CODE & CONTAINERIZATION (Bulut ve Otomasyon Dünyası)
> Altyapıları kodla yönettiğimiz ve modern konteyner teknolojileriyle ölçeklendirdiğimiz seviye.

#### 📦 Modül 04: Container Technologies (Docker Deep-Dive) `[BEKLEMEDE ⏳]`
* [ ] **Containerization vs Virtualization:** VM'ler (Hypervisor) ile Konteyner (Container) arasındaki mimari farklar.
* [ ] **Linux Kernel Namespaces & Cgroups:** Docker'ın arkasındaki gerçek Linux teknolojileri.
* [ ] **Container Runtime Internals:** `containerd`, `runc` ve Kubernetes bağlantısını sağlayan **CRI (Container Runtime Interface)** kavramları.
* [ ] **Docker Core:** Docker CLI, Dockerfile yazım kuralları, Çok katmanlı (Multi-stage) Build işlemleri.
* [ ] **Docker Storage & Networking:** Volumes, Bind Mounts, Bridge vs. Overlay Networks.
* [ ] **Container Security & Image Hardening:** Root yetkisi olmadan çalışan (non-root) konteynerler, **Trivy/Grype** ile imaj zafiyet taramaları.
* [ ] **Docker Compose:** Çok konteynerli yerel uygulama ortamları oluşturma.
---
#### ☸️ Modül 05: Kubernetes (Production-Grade Orchestration) `[BEKLEMEDE ⏳]`
* [ ] **Kubernetes Architecture:** Control Plane (API Server, Etcd, Scheduler, K-C-M) vs. Worker Nodes (Kubelet, Kube-proxy, Container Runtime).
* [ ] **Kubernetes Core Objects:** Pods, ReplicaSets, Deployments, Services (ClusterIP, NodePort, LoadBalancer).
* [ ] **Configuration & Secrets:** ConfigMaps, Secrets, Env Variables.
* [ ] **Kubernetes Security & RBAC:** Rol tabanlı erişim kontrolü (Role, ClusterRole, RoleBinding).
* [ ] **Storage in K8s:** PV (Persistent Volume), PVC (Persistent Volume Claim), StorageClasses.
* [ ] **Advanced Traffic Control:** Ingress Controllers (Nginx, Traefik), CoreDNS, **Service Mesh (Istio / Linkerd - mTLS)** kavramları.
* [ ] **GitOps & Declarative GitOps Agents:** Kubernetes kaynaklarının **ArgoCD** veya **FluxCD** ile yönetimi.
* [ ] **Scaling & Reliability:** HPA (Horizontal Pod Autoscaler), Liveness & Readiness Probes (Kendi kendini iyileştiren sistemler).
---
#### ⚙️ Modül 06: Infrastructure as Code (IaC) & Configuration Management `[BEKLEMEDE ⏳]`
* [ ] **Infrastructure as Code (IaC) Basics:** Deklaratif vs. İmperatif yaklaşım.
* [ ] **Terraform (Altyapı Sağlama):** Providers, Resources, Modules.
* [ ] **Terraform State Security:** `terraform.tfstate` güvenliği, AWS S3 ve DynamoDB ile **State Locking** mekanizması.
* [ ] **Ansible (Yapılandırma Yönetimi):** Agentless mimari, SSH bağlantıları, Inventory, Playbooks, Roles.
* [ ] **Ansible Vault:** Hassas verileri, şifreleri ve SSH anahtarlarını şifreleyerek saklama.
* [ ] **Git & Version Control Integration:** Git branching stratejileri (GitFlow, Trunk-based).

---

### 🚀 LEVEL 3: AUTOMATION, CI/CD & OBSERVABILITY (Sürekli Entegrasyon ve İzlenebilirlik)
> Yazılımları üretim ortamına (Production) kesintisiz aktarma ve anlık metriklerle sistemleri izleme.

#### 🔄 Modül 07: CI/CD Pipelines & GitOps `[BEKLEMEDE ⏳]`
* [ ] **CI/CD Core Concepts:** Continuous Integration, Continuous Delivery, Continuous Deployment farkları.
* [ ] **Pipeline Platforms:** **GitHub Actions** veya **GitLab CI** kullanarak otomatize pipeline tasarımı.
* [ ] **DevSecOps & Security Gates:** Pipeline içerisine otomatik kod analiz araçları (**SonarQube**) ve secret tarayıcıları (**Gitleaks/TruffleHog**) entegrasyonu.
* [ ] **Artifact Management:** Docker imajlarının depolanması (Docker Hub, GitHub Container Registry - GHCR, JFrog Artifactory).
---
#### 🐍 Modül 08: Systems Automation & Scripting (Bash & Python) `[BEKLEMEDE ⏳]`
* [ ] **Bash Scripting (Intermediate):** Variables, If-Else, Loops, Functions, Exit Codes, Error Handling (`set -e`).
* [ ] **Regex (Regular Expressions) Mastery:** Log manipülasyonu ve metin analizi için Regex kalıpları.
* [ ] **Automation Use-Cases:** Otomatik sistem yedekleme, log rotasyonu (logrotate) scriptleri, disk doluluk kontrolü.
* [ ] **Python for DevOps:** `os`, `sys`, `subprocess` modülleri ile sistem yönetimi, **Python Requests kütüphanesi** ile API'larla konuşma.
---
#### 📊 Modül 09: Observability, Logging & Monitoring (Sistem İzleme) `[BEKLEMEDE ⏳]`
* [ ] **The 4 Golden Signals of SRE:** Latency (Gecikme), Traffic (Trafik), Errors (Hatalar), Saturation (Doygunluk) metrikleri.
* [ ] **Metrics & Monitoring:** **Prometheus** mimarisi (Pull-based metrik toplama), **Grafana** ile Dashboard tasarımı.
* [ ] **Centralized Logging:** **ELK Stack (Elasticsearch, Logstash, Kibana)** veya **Grafana Loki** ile log toplama ve log analizi.
* [ ] **Distributed Tracing (Dağıtık İzleme):** Mikroservisler arası istek takibi için **Jaeger** veya **OpenTelemetry** mantığı.
* [ ] **Alerting:** Kritik eşikler için Slack, Microsoft Teams veya PagerDuty entegrasyonu.

---

### ☁️ LEVEL 4: CLOUD COMPUTING & ENTERPRISE ARCHITECTURES (Bulut ve Büyük Sistem Mimarileri)
> Global ölçekte çökmeyen, her saniye erişilebilir sistem mimarileri inşa etme.

#### ☁️ Modül 10: Cloud Computing (AWS Focus) `[BEKLEMEDE ⏳]`
* [ ] **Compute Services:** EC2 (Sanal Sunucular), Auto Scaling Groups.
* [ ] **Networking in Cloud:** VPC (Virtual Private Cloud), Subnets, Route Tables, Internet Gateways, NAT Gateways, Security Groups.
* [ ] **Identity & Access Management (IAM):** Roles, Users, Policies, Principal of Least Privilege (En az yetki kuralı).
* [ ] **AWS Secrets Manager & KMS:** Uygulama şifrelerini ve gizli anahtarlarını AWS üzerinde güvenli şekilde yönetme.
* [ ] **Storage Services:** S3 (Nesne depolama), EBS (Blok depolama), EFS (Paylaşımlı depolama).
* [ ] **Serverless Architecture:** **AWS Lambda** ve **SQS/SNS** kuyruk mekanizmaları ile olay güdümlü mimariler.
---
#### 🏛️ Modül 11: Site Reliability Engineering (SRE) & High Availability `[BEKLEMEDE ⏳]`
* [ ] **SRE Principles:** SLA (Service Level Agreement), SLO (Service Level Objective), SLI (Service Level Indicator) kavramları.
* [ ] **Chaos Engineering (Kaos Mühendisliği):** Canlı sistem dayanıklılık testleri (**Chaos Monkey** felsefesi).
* [ ] **Post-Mortem & Incident Blameless Culture:** Sistem çöküşlerinden sonra suçsuz raporlama standartları ve kök neden analizleri.
* [ ] **Disaster Recovery (DR):** RTO (Recovery Time Objective) ve RPO (Recovery Point Objective) hesaplama, Backup & Restore stratejileri.
* [ ] **High Availability (HA) Architectures:** Multi-AZ (Bölge), Multi-Region deployment mimarileri.
