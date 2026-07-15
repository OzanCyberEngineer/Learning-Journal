# 🏆 Global System, DevOps & Platform Engineering Roadmap

Bu depo, sıfırdan başlayarak küresel standartlarda (SRE, DevOps, Platform Mühendisliği) uzmanlık seviyesine ulaşmak için oluşturulmuş teorik, pratik ve siber güvenlik odaklı mühendislik kütüphanemdir. Her modül mülakat standartlarına ve canlı üretim ortamı (production-grade) mimarilerine göre tasarlanmıştır.

---

## 🗺️ PROJE İLERLEME HARİTASI (ROADMAP)

### 🌐 LEVEL 1: FOUNDATIONS OF SYSTEMS & SECURITY (Sistem ve Güvenlik Temelleri)
> Sunucu odasının, işletim sisteminin ve veri iletiminin kalbine indiğimiz temel seviye.

#### 🌐 Modül 01: Advanced Networking for Platforms 
*  [**Physical & Data Link Layers:** OSI vs TCP/IP, MAC Adresi, Fiziksel Sinyaller, **MTU (Maximum Transmission Unit)** mantığı.](./LEVEL_1/Modül_01_Modül_01_Advanced%20Networking%20for%20Platforms/01-Physical%20&%20Data%20Link%20Layers)
*  [**ARP & Yerel Ağ Dinamikleri:** ARP Protokolü, ARP Table, ARP Poisoning (Zehirlenme), **DAI (Dynamic ARP Inspection)**.](./LEVEL_1/Modül_01_Modül_01_Advanced%20Networking%20for%20Platforms/02-ARP%20&%20Yerel%20Ağ%20Dinamikleri)
*  [**VLAN (Virtual LAN) & VXLAN:** Klasik L2 izolasyonu, VLAN Hopping / Double Tagging saldırıları, **VXLAN Overlay (Tünel) mimarisi**, VTEP ve VNI kavramları.](./LEVEL_1/Modül_01_Modül_01_Advanced%20Networking%20for%20Platforms/03-VLAN%20(Virtual%20LAN)%20&%20VXLAN)
*  [**Network Routing & Subnetting:** IPv4 Yapısı, CIDR, Subnetting (Alt ağ hesaplama), Gateway, **IP Fragmentation (Parçalanma)**, Static vs Dynamic Routing (BGP/OSPF mantığı).](./LEVEL_1/Modül_01_Modül_01_Advanced%20Networking%20for%20Platforms/04-Network%20Routing%20&%20Subnetting)
*  **Transport Layer & Traffic Control:** TCP vs UDP, Portlar, TCP 3-Way Handshake, Windowing, Congestion Control (Tıkanıklık kontrolü).
*  **Application Layer Services:** DNS Altyapısı (A, CNAME, TXT, MX), DNS Spoofing, **DHCP & DORA Süreci, DHCP Snooping Güvenliği**, HTTP/HTTPS ve TLS/SSL Handshake detayları.
*  **Load Balancing & Proxy Concepts:** Forward Proxy vs. Reverse Proxy, L4 (TCP/UDP) vs. L7 (HTTP) Load Balancing mantığı, SSL Termination vs SSL Passthrough.
---
#### 🐧 Modül 02: Enterprise Linux System Administration
*  **Linux File System & FHS:** Klasör hiyerarşisi (`/etc`, `/var`), sanal dosya sistemleri (`/proc`, `/sys`).
*  **Text & Stream Manipulation:** Pipes (`|`), `grep`, `awk`, `sed` filtrelemeleri.
*  **User & Permission Management:** User/Group mantığı, `chmod`, `chown`, **POSIX ACLs (Gelişmiş izinler)**, `Sudoers` yapılandırması.
*  **Process & Resource Management:** `ps`, `top`, `kill`, **Systemd & Systemctl (Servis mimarisi)**, arka plan süreçleri.
*  **Storage & Disk Management:** **LVM (Logical Volume Manager)** ile dinamik disk büyütme, Dosya sistemleri (ext4, XFS), Mount işlemleri.
*  **Linux Networking & Troubleshooting:** IP tanımlama, `/etc/resolv.conf`, `hosts` dosyası, `ip route`, `ss`, `netstat`, `tcpdump` ve `dig` kullanımı.
---
#### 🛡️ Modül 03: Security Engineering & Incident Management 
*  **Threat Modeling & Attacks:** Cyber Kill Chain, APT'ler, DoS/DDoS (SYN Flood), MITM.
*  **Malware & System Defense:** Malware türleri (Worm, Trojan, Sandbox), Forensics için RAM'in önemi, **Linux Hardening Standartları**.
*  **Firewalling:** **iptables** ve **nftables** çalışma mantığı, zincirler (chains) ve kural tanımlama.
*  **System Auditing & Logging:** `/var/log` analizi, **Auditd (Linux denetim sistemi)**, fail2ban mantığı.
*  **Cryptography & SSH Security:** Simetrik/Asimetrik Şifreleme, SSH Key-Pair mantığı, SSH Hardening (`sshd_config` sıkılaştırma).

---

### 📦 LEVEL 2: INFRASTRUCTURE AS CODE & CONTAINERIZATION (Bulut ve Otomasyon Dünyası)
> Altyapıları kodla yönettiğimiz ve modern konteyner teknolojileriyle ölçeklendirdiğimiz seviye.

#### 📦 Modül 04: Container Technologies (Docker Deep-Dive) 
*  **Containerization vs Virtualization:** VM'ler (Hypervisor) ile Konteyner (Container) arasındaki mimari farklar.
*  **Linux Kernel Namespaces & Cgroups:** Docker'ın arkasındaki gerçek Linux teknolojileri.
*  **Container Runtime Internals:** `containerd`, `runc` ve Kubernetes bağlantısını sağlayan **CRI (Container Runtime Interface)** kavramları.
*  **Docker Core:** Docker CLI, Dockerfile yazım kuralları, Çok katmanlı (Multi-stage) Build işlemleri.
*  **Docker Storage & Networking:** Volumes, Bind Mounts, Bridge vs. Overlay Networks.
*  **Container Security & Image Hardening:** Root yetkisi olmadan çalışan (non-root) konteynerler, **Trivy/Grype** ile imaj zafiyet taramaları.
*  **Docker Compose:** Çok konteynerli yerel uygulama ortamları oluşturma.
---
#### ☸️ Modül 05: Kubernetes (Production-Grade Orchestration) 
*  **Kubernetes Architecture:** Control Plane (API Server, Etcd, Scheduler, K-C-M) vs. Worker Nodes (Kubelet, Kube-proxy, Container Runtime).
*  **Kubernetes Core Objects:** Pods, ReplicaSets, Deployments, Services (ClusterIP, NodePort, LoadBalancer).
*  **Configuration & Secrets:** ConfigMaps, Secrets, Env Variables.
*  **Kubernetes Security & RBAC:** Rol tabanlı erişim kontrolü (Role, ClusterRole, RoleBinding).
*  **Storage in K8s:** PV (Persistent Volume), PVC (Persistent Volume Claim), StorageClasses.
*  **Advanced Traffic Control:** Ingress Controllers (Nginx, Traefik), CoreDNS, **Service Mesh (Istio / Linkerd - mTLS)** kavramları.
*  **GitOps & Declarative GitOps Agents:** Kubernetes kaynaklarının **ArgoCD** veya **FluxCD** ile yönetimi.
*  **Scaling & Reliability:** HPA (Horizontal Pod Autoscaler), Liveness & Readiness Probes (Kendi kendini iyileştiren sistemler).
---
#### ⚙️ Modül 06: Infrastructure as Code (IaC) & Configuration Management
*  **Infrastructure as Code (IaC) Basics:** Deklaratif vs. İmperatif yaklaşım.
*  **Terraform (Altyapı Sağlama):** Providers, Resources, Modules.
*  **Terraform State Security:** `terraform.tfstate` güvenliği, AWS S3 ve DynamoDB ile **State Locking** mekanizması.
*  **Ansible (Yapılandırma Yönetimi):** Agentless mimari, SSH bağlantıları, Inventory, Playbooks, Roles.
*  **Ansible Vault:** Hassas verileri, şifreleri ve SSH anahtarlarını şifreleyerek saklama.
*  **Git & Version Control Integration:** Git branching stratejileri (GitFlow, Trunk-based).

---

### 🚀 LEVEL 3: AUTOMATION, CI/CD & OBSERVABILITY (Sürekli Entegrasyon ve İzlenebilirlik)
> Yazılımları üretim ortamına (Production) kesintisiz aktarma ve anlık metriklerle sistemleri izleme.

#### 🔄 Modül 07: CI/CD Pipelines & GitOps 
*  **CI/CD Core Concepts:** Continuous Integration, Continuous Delivery, Continuous Deployment farkları.
*  **Pipeline Platforms:** **GitHub Actions** veya **GitLab CI** kullanarak otomatize pipeline tasarımı.
*  **DevSecOps & Security Gates:** Pipeline içerisine otomatik kod analiz araçları (**SonarQube**) ve secret tarayıcıları (**Gitleaks/TruffleHog**) entegrasyonu.
*  **Artifact Management:** Docker imajlarının depolanması (Docker Hub, GitHub Container Registry - GHCR, JFrog Artifactory).
---
#### 🐍 Modül 08: Systems Automation & Scripting (Bash & Python) 
*  **Bash Scripting (Intermediate):** Variables, If-Else, Loops, Functions, Exit Codes, Error Handling (`set -e`).
*  **Regex (Regular Expressions) Mastery:** Log manipülasyonu ve metin analizi için Regex kalıpları.
*  **Automation Use-Cases:** Otomatik sistem yedekleme, log rotasyonu (logrotate) scriptleri, disk doluluk kontrolü.
*  **Python for DevOps:** `os`, `sys`, `subprocess` modülleri ile sistem yönetimi, **Python Requests kütüphanesi** ile API'larla konuşma.
---
#### 📊 Modül 09: Observability, Logging & Monitoring (Sistem İzleme) 
*  **The 4 Golden Signals of SRE:** Latency (Gecikme), Traffic (Trafik), Errors (Hatalar), Saturation (Doygunluk) metrikleri.
*  **Metrics & Monitoring:** **Prometheus** mimarisi (Pull-based metrik toplama), **Grafana** ile Dashboard tasarımı.
*  **Centralized Logging:** **ELK Stack (Elasticsearch, Logstash, Kibana)** veya **Grafana Loki** ile log toplama ve log analizi.
*  **Distributed Tracing (Dağıtık İzleme):** Mikroservisler arası istek takibi için **Jaeger** veya **OpenTelemetry** mantığı.
*  **Alerting:** Kritik eşikler için Slack, Microsoft Teams veya PagerDuty entegrasyonu.

---

### ☁️ LEVEL 4: CLOUD COMPUTING & ENTERPRISE ARCHITECTURES (Bulut ve Büyük Sistem Mimarileri)
> Global ölçekte çökmeyen, her saniye erişilebilir sistem mimarileri inşa etme.

#### ☁️ Modül 10: Cloud Computing (AWS Focus) 
*  **Compute Services:** EC2 (Sanal Sunucular), Auto Scaling Groups.
*  **Networking in Cloud:** VPC (Virtual Private Cloud), Subnets, Route Tables, Internet Gateways, NAT Gateways, Security Groups.
*  **Identity & Access Management (IAM):** Roles, Users, Policies, Principal of Least Privilege (En az yetki kuralı).
*  **AWS Secrets Manager & KMS:** Uygulama şifrelerini ve gizli anahtarlarını AWS üzerinde güvenli şekilde yönetme.
*  **Storage Services:** S3 (Nesne depolama), EBS (Blok depolama), EFS (Paylaşımlı depolama).
*  **Serverless Architecture:** **AWS Lambda** ve **SQS/SNS** kuyruk mekanizmaları ile olay güdümlü mimariler.
---
#### 🏛️ Modül 11: Site Reliability Engineering (SRE) & High Availability 
*  **SRE Principles:** SLA (Service Level Agreement), SLO (Service Level Objective), SLI (Service Level Indicator) kavramları.
*  **Chaos Engineering (Kaos Mühendisliği):** Canlı sistem dayanıklılık testleri (**Chaos Monkey** felsefesi).
*  **Post-Mortem & Incident Blameless Culture:** Sistem çöküşlerinden sonra suçsuz raporlama standartları ve kök neden analizleri.
*  **Disaster Recovery (DR):** RTO (Recovery Time Objective) ve RPO (Recovery Point Objective) hesaplama, Backup & Restore stratejileri.
*  **High Availability (HA) Architectures:** Multi-AZ (Bölge), Multi-Region deployment mimarileri.
---

## 🏆 SECTION 2: MILESTONE PORTFOLIO PROJECTS

The following three production-grade projects will be built on AWS to validate the skills acquired at the end of each major milestone.

---

### 🛡️ PROJECT 1: Hardened & Audited Private Linux Server Infrastructure
> **Focus:** Host Hardening, Stateful Firewalling, Auditing, and Intrusion Prevention  
> **Timeline:** *At the end of Level 1 (Module 03).*

#### 📌 Overview
An AWS EC2 instance running Ubuntu/Debian configured as a hardened host. Designed to deflect unauthorized authentication attempts, actively drop unapproved networking frames, and record all shell-level transactions to immutable logs.

#### 🛠️ Core Technology Stack
* **OS:** Ubuntu 22.04 LTS (AWS EC2)
* **Firewalling:** `iptables` / `nftables`
* **IPS Engine:** `Fail2Ban`
* **Audit System:** Linux Kernel `Auditd`
* **Shell Layer:** Bash (Automated reporting scripts)

#### 📐 Security Architectures Defined
* **Zero-Trust Input Rules:** Blocks all unsolicited entry points. The packet filter is configured to only allow port 22 (restricted management subnet) and port 443 (application runtime).
* **SSHD Tightening:** Root login is explicitly set to `no`. Password validation is deactivated in favor of Ed25519 cryptographic public keys.
* **Active Intrusion Defusal:** `Fail2Ban` reads local syslog authentication failures. IP addresses exhibiting sequential failure logs (over 3 retries) are dynamically banned for 1 hour at the kernel firewall level.
* **Immutable Auditing:** Custom audit configurations monitor the system's runtime critical assets, specifically target logging write events to `/etc/shadow` and execution events triggered by sudo actions.

---

### 📦 PROJECT 2: Declarative Provisioning of Production-Ready Kubernetes
> **Focus:** Cloud Provisioning, Infrastructure as Code, Configuration Management, and Orchestration  
> **Timeline:** *At the end of Level 2 (Module 06).*

#### 📌 Overview
An end-to-end automation pipeline that provisions the entire cloud infrastructure on AWS and configures a multi-node, production-grade Kubernetes cluster (1 Control Plane node, 2 Worker nodes) without a single manual console action.

#### 🛠️ Core Technology Stack
* **IaC Engine:** Terraform
* **Configuration:** Ansible
* **Container Runtime:** `containerd`
* **Bootstrap Engine:** `kubeadm`
* **Cloud Infrastructure:** AWS (VPC, Subnets, EC2, S3, DynamoDB)

#### 📐 Deployment Architecture Defined
* **State-Locked Cloud (Terraform):** Deploys a customized AWS VPC with Public and Private Subnets across diverse AZs. Employs S3 for storing state variables with DynamoDB-backed concurrency control (state locking).
* **Automated Node Prep (Ansible):** Ansible scripts connect via SSH, configuring system-level pre-requisites like loading essential kernel modules, adjusting sysctl values, and configuring the container execution engine.
* **Bootstrap Orchestration:** Ansible automatically executes `kubeadm init` on the control plane, secures the admin configuration token, and seamlessly joins worker nodes to form the secure cluster network.
* **Least-Privilege Isolation:** Deployments execute custom microservices packaged via secure multi-stage Dockerfiles. NetworkPolicies enforce strict pod-to-pod Layer 4 communication paths.

---

### 🚀 PROJECT 3: Zero-Trust, Self-Healing Global GitOps & Observability Platform
> **Focus:** DevSecOps, Declarative CD, SRE Metriology, and Multi-AZ Fault-Tolerance  
> **Timeline:** *At the end of Level 4 (Module 11).*

#### 📌 Overview
The final milestone project implementing a complete, automated DevOps lifecycle. Code is written locally, pushed to version control, secured dynamically, deployed declaratively, monitored proactively, and configured to self-heal in the event of component failure.

#### 🛠️ Core Technology Stack
* **CI Engine:** GitHub Actions
* **GitOps Engine:** ArgoCD
* **Static Analysis:** SonarQube & TruffleHog
* **SRE Logging & Metrics:** Prometheus, Grafana, Loki
* **Runtime Target:** Kubernetes (AWS EKS or Self-Managed Multi-Node Cluster)

#### 📐 Engineering Principles Implemented
* **DevSecOps Pipeline:** Code validation pipeline incorporating secret-leak checks via `TruffleHog`, code static analysis via `SonarQube`, and docker layer analysis via `Trivy` before ECR push.
* **GitOps Architecture (ArgoCD):** Deploys a synchronization controller in the cluster. Changes are reconciled directly from declarative Git manifests. Manual configuration drifts are instantly overridden and reverted back to the Git source of truth.
* **Golden Signals Observability:** Prometheus scrapes system and runtime statistics. Grafana dashboards visualize service latency, throughput, error percentages, and thread saturation.
* **Automated Resilience:** Advanced K8s probes check the health status of active pods. If data
