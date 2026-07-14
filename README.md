# 📘 Technical Learning Journal & Progress Reports

Welcome to my personal learning journal. This repository serves as a professional progress report where I document my computer engineering and cybersecurity studies step-by-step. 

My goal is to showcase not just what I learn, but how I analyze technologies and implement security best practices.

---

## 📌 Contents & Roadmap

# 🏆 Global System, DevOps & Platform Engineering Roadmap

Bu depo, sıfırdan başlayarak küresel standartlarda (SRE, DevOps, Platform Mühendisliği) uzmanlık seviyesine ulaşmak için oluşturulmuş teorik ve pratik mühendislik kütüphanemdir. Her modül mülakat standartlarına ve canlı üretim ortamı (production-grade) mimarilerine göre tasarlanmıştır.

---

## 🗺️ PROJE İLERLEME HARİTASI (ROADMAP)

### 🌐 LEVEL 1: FOUNDATIONS OF SYSTEMS & SECURITY (Sistem ve Güvenlik Temelleri)
> Sunucu odasının, işletim sisteminin ve veri iletiminin kalbine indiğimiz temel seviye.

#### 🌐 Modül 01: Advanced Networking for Platforms `[100% TAMAMLANDI ✅]`
* [x] **Physical & Data Link Layers:** OSI & TCP/IP, MAC, ARP, **VLAN (Virtual LAN)** ve **VXLAN**.
* [x] **Network Routing & Subnetting:** IPv4, CIDR, Subnetting, Gateway, Static vs Dynamic Routing.
* [x] **Application Layer Services:** DNS (A, CNAME, TXT, MX kayıtları), **DHCP**, HTTP/HTTPS ve TLS/SSL el sıkışma detayları.
* [x] **Transport Layer & Traffic Control:** TCP/UDP, Portlar, **TCP 3-Way Handshake, Windowing, congestion control (tıkanıklık kontrolü)**.
* [x] **Load Balancing & Proxy Concepts:** **Forward Proxy vs. Reverse Proxy**, L4 (TCP) vs. L7 (HTTP) Load Balancing mantığı.
* *👉 [Modül Detayları ve Laboratuvar Notları](LEVEL-1/README(Network).md) dosyası altında dökümante edilmiştir.*

#### 🐧 Modül 02: Enterprise Linux System Administration `[100% TAMAMLANDI ✅]`
* [x] **Linux File System & FHS:** Klasör hiyerarşisi (`/etc`, `/var`, `/proc`, `/sys`).
* [x] **Text & Stream Manipulation:** Pipes (`|`), `grep`, `awk`, `sed` filtrelemeleri.
* [x] **User & Permission Management:** User/Group, `chmod`, `chown`, **POSIX ACLs (Gelişmiş izinler)**, **Sudoers** yapılandırması.
* [x] **Process & Resource Management:** `ps`, `top`, `kill`, **Systemd & Systemctl (Servis yönetimi)**, arka plan süreçleri.
* [x] **Storage & Disk Management:** **LVM (Logical Volume Manager)** (Canlıda disk büyütme), File Systems (ext4, XFS), Mount işlemleri.
* *👉 [Modül Detayları ve Laboratuvar Notları](./LEVEL-1/README(linux).md) dosyası altında dökümante edilmiştir.*

#### 🛡️ Modül 03: Security Engineering & Incident Management `[75% DEVAM EDİYOR 🔥]`
* [x] **Threat Modeling & Attacks:** Cyber Kill Chain, APT'ler, DoS/DDoS (SYN Flood), MITM, ARP Poisoning.
* [x] **Malware & System Defense:** Malware türleri (Worm, Trojan, Sandbox), Forensics için RAM'in önemi, **Linux Hardening**.
* [x] **System Auditing & Logging:** `/var/log` analizi, **Auditd (Linux denetim sistemi)**, fail2ban mantığı.
* [ ] 🔴 **Cryptography & SSH Security:** Simetrik/Asimetrik Şifreleme, **SSH Key-Pair mantığı**, SSH Hardening (`sshd_config` sıkılaştırma).
* *👉 [Modül Detayları ve Laboratuvar Notları](./LEVEL-1/README(cyber_threat).md) dosyası altında dökümante edilmiştir.*

---

### 📦 LEVEL 2: INFRASTRUCTURE AS CODE & CONTAINERIZATION (Bulut ve Otomasyon Dünyası)
> Altyapıları kodla yönettiğimiz ve modern konteyner teknolojileriyle ölçeklendirdiğimiz seviye.

#### 📦 Modül 04: Container Technologies (Docker Deep-Dive) `[BEKLEMEDE ⏳]`
* [ ] **Containerization vs Virtualization:** VM'ler (Hypervisor) ile Konteyner (Container) arasındaki mimari farklar.
* [ ] **Linux Kernel Namespaces & Cgroups:** Docker'ın arkasındaki gerçek Linux teknolojileri.
* [ ] **Docker Core:** Docker CLI, Dockerfile yazım kuralları, Çok katmanlı (Multi-stage) Build işlemleri.
* [ ] **Docker Storage & Networking:** Volumes, Bind Mounts, Bridge vs. Overlay Networks.
* [ ] **Docker Compose:** Çok konteynerli yerel uygulama ortamları oluşturma.

#### ☸️ Modül 05: Kubernetes (Production-Grade Orchestration) `[BEKLEMEDE ⏳]`
* [ ] **Kubernetes Architecture:** Control Plane (API Server, Etcd, Scheduler, K-C-M) vs. Worker Nodes (Kubelet, Kube-proxy, Container Runtime).
* [ ] **Kubernetes Core Objects:** Pods, ReplicaSets, Deployments, Services (ClusterIP, NodePort, LoadBalancer).
* [ ] **Configuration & Secrets:** ConfigMaps, Secrets, Env Variables.
* [ ] **Storage in K8s:** PV (Persistent Volume), PVC (Persistent Volume Claim), StorageClasses.
* [ ] **Advanced Traffic Control:** Ingress Controllers (Nginx, Traefik), CoreDNS.
* [ ] **Scaling & Reliability:** HPA (Horizontal Pod Autoscaler), Liveness & Readiness Probes (Kendi kendini iyileştiren sistemler).

#### ⚙️ Modül 06: Infrastructure as Code (IaC) & Configuration Management `[BEKLEMEDE ⏳]`
* [ ] **Infrastructure as Code (IaC) Basics:** Deklaratif vs. İmperatif yaklaşım.
* [ ] **Terraform (Altyapı Sağlama):** Providers, Resources, State File yönetimi (Locking), Modules.
* [ ] **Ansible (Yapılandırma Yönetimi):** Agentless mimari, SSH bağlantıları, Inventory, Playbooks, Roles, Ansible Galaxy.
* [ ] **Git & Version Control Integration:** Git branching stratejileri (GitFlow, Trunk-based).

---

### 🚀 LEVEL 3: AUTOMATION, CI/CD & OBSERVABILITY (Sürekli Entegrasyon ve İzlenebilirlik)
> Yazılımları üretim ortamına (Production) kesintisiz aktarma ve anlık metriklerle sistemleri izleme.

#### 🔄 Modül 07: CI/CD Pipelines & GitOps `[BEKLEMEDE ⏳]`
* [ ] **CI/CD Core Concepts:** Continuous Integration, Continuous Delivery, Continuous Deployment farkları.
* [ ] **Pipeline Platforms:** **GitHub Actions** veya **GitLab CI** kullanarak otomatize pipeline tasarımı.
* [ ] **Artifact Management:** Docker imajlarının depolanması (Docker Hub, GitHub Container Registry - GHCR, JFrog Artifactory).
* [ ] **GitOps Paradigm:** Kod tabanlı dağıtım, **ArgoCD** veya **FluxCD** mantığı.

#### 🐍 Modül 08: Systems Automation & Scripting (Bash & Python) `[BEKLEMEDE ⏳]`
* [ ] **Bash Scripting (Intermediate):** Variables, If-Else, Loops, Functions, Exit Codes, Error Handling (`set -e`).
* [ ] **Automation Use-Cases:** Otomatik sistem yedekleme, log rotasyonu (logrotate) scriptleri, disk doluluk kontrolü.
* [ ] **Python for DevOps:** `os`, `sys`, `subprocess` modülleri ile sistem yönetimi, **Python Requests kütüphanesi** ile API'larla konuşma.

#### 📊 Modül 09: Observability, Logging & Monitoring (Sistem İzleme) `[BEKLEMEDE ⏳]`
* [ ] **Metrics & Monitoring:** **Prometheus** mimarisi (Pull-based metrik toplama), **Grafana** ile Dashboard tasarımı.
* [ ] **Centralized Logging:** **ELK Stack (Elasticsearch, Logstash, Kibana)** veya **Grafana Loki** ile log toplama ve log analizi.
* [ ] **Alerting:** Kritik eşikler için Slack, Microsoft Teams veya PagerDuty entegrasyonu.

---

### ☁️ LEVEL 4: CLOUD COMPUTING & ENTERPRISE ARCHITECTURES (Bulut ve Büyük Sistem Mimarileri)
> Global ölçekte çökmeyen, her saniye erişilebilir sistem mimarileri inşa etme.

#### ☁️ Modül 10: Cloud Computing (AWS Focus) `[BEKLEMEDE ⏳]`
* [ ] **Compute Services:** EC2 (Sanal Sunucular), Auto Scaling Groups.
* [ ] **Networking in Cloud:** VPC (Virtual Private Cloud), Subnets, Route Tables, Internet Gateways, NAT Gateways, Security Groups.
* [ ] **Identity & Access Management (IAM):** Roles, Users, Policies, Principal of Least Privilege (En az yetki kuralı).
* [ ] **Storage Services:** S3 (Nesne depolama), EBS (Blok depolama), EFS (Paylaşımlı depolama).

#### 🏛️ Modül 11: Site Reliability Engineering (SRE) & High Availability `[BEKLEMEDE ⏳]`
* [ ] **SRE Principles:** SLA (Service Level Agreement), SLO (Service Level Objective), SLI (Service Level Indicator) kavramları.
* [ ] **Disaster Recovery (DR):** RTO (Recovery Time Objective) ve RPO (Recovery Point Objective) hesaplama, Backup & Restore stratejileri.
* [ ] **High Availability (HA) Architectures:** Multi-AZ (Bölge), Multi-Region deployment mimarileri. 

---
💡 *Note: This journal is updated regularly as I progress through my technical roadmap.*
