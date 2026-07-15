# 🏆 Systems, DevOps & Platform Engineering Learning Roadmap

Bu depo; sistem yönetimi, ağ teknolojileri, siber güvenlik, DevOps, SRE ve platform mühendisliği alanlarında güçlü bir teknik temel oluşturmak amacıyla hazırladığım, devam eden bir öğrenme günlüğü ve uygulama yol haritasıdır.

Yol haritası; teorik dokümantasyon, güvenlik perspektifi, uygulamalı çalışmalar ve seviyeler ilerledikçe geliştirilecek kapsamlı portföy projelerinden oluşmaktadır. İçerikler Türkçe ve İngilizce olarak hazırlanmakta ve öğrenme süreci boyunca düzenli olarak güncellenmektedir.

---

## 📍 Current Progress

- **Current Level:** Level 1 — Foundations of Systems & Security
- **Current Module:** Module 01 — Advanced Networking for Platforms
- **Repository Status:** 🚧 Actively Maintained
- **Documentation Languages:** Turkish & English

### Status Legend

- ✅ **Completed**
- 🚧 **In Progress**
- ⏳ **Planned**

---

## 🗺️ LEARNING ROADMAP — ÖĞRENME YOL HARİTASI

### 🌐 LEVEL 1: FOUNDATIONS OF SYSTEMS & SECURITY — 🚧 IN PROGRESS
**Sistem ve Güvenlik Temelleri**
> Ağ iletişimi, işletim sistemleri, sistem yönetimi ve temel güvenlik prensiplerinde sağlam bir altyapı oluşturduğumuz seviye.

#### 🌐 Modül 01: Advanced Networking for Platforms — 🚧 In Progress
*  ✅ [**Physical & Data Link Layers:** OSI vs TCP/IP, MAC Adresi, Fiziksel Sinyaller, **MTU (Maximum Transmission Unit)** mantığı.](./LEVEL_1/Modül_01_Modül_01_Advanced%20Networking%20for%20Platforms/01-Physical%20&%20Data%20Link%20Layers)
*  ✅ [**ARP & Yerel Ağ Dinamikleri:** ARP Protokolü, ARP Table, ARP Poisoning (Zehirlenme), **DAI (Dynamic ARP Inspection)**.](./LEVEL_1/Modül_01_Modül_01_Advanced%20Networking%20for%20Platforms/02-ARP%20&%20Yerel%20Ağ%20Dinamikleri)
*  ✅ [**VLAN (Virtual LAN) & VXLAN:** Klasik L2 izolasyonu, VLAN Hopping / Double Tagging saldırıları, **VXLAN Overlay (Tünel) mimarisi**, VTEP ve VNI kavramları.](./LEVEL_1/Modül_01_Modül_01_Advanced%20Networking%20for%20Platforms/03-VLAN%20(Virtual%20LAN)%20&%20VXLAN)
*  ✅ [**Network Routing & Subnetting:** IPv4 Yapısı, CIDR, Subnetting (Alt ağ hesaplama), Gateway, **IP Fragmentation (Parçalanma)**, Static vs Dynamic Routing (BGP/OSPF mantığı).](./LEVEL_1/Modül_01_Modül_01_Advanced%20Networking%20for%20Platforms/04-Network%20Routing%20&%20Subnetting)
*  ⏳ **Transport Layer & Traffic Control:** TCP vs UDP, Portlar, TCP 3-Way Handshake, Windowing, Congestion Control (Tıkanıklık kontrolü).
*  ⏳ **Application Layer Services:** DNS Altyapısı (A, CNAME, TXT, MX), DNS Spoofing, **DHCP & DORA Süreci, DHCP Snooping Güvenliği**, HTTP/HTTPS ve TLS/SSL Handshake detayları.
*  ⏳ **Load Balancing & Proxy Concepts:** Forward Proxy vs. Reverse Proxy, L4 (TCP/UDP) vs. L7 (HTTP) Load Balancing mantığı, SSL Termination vs SSL Passthrough.
---
#### 🐧 Modül 02: Enterprise Linux System Administration — ⏳ Planned
*  **Linux File System & FHS:** Klasör hiyerarşisi (`/etc`, `/var`), sanal dosya sistemleri (`/proc`, `/sys`).
*  **Text & Stream Manipulation:** Pipes (`|`), `grep`, `awk`, `sed` filtrelemeleri.
*  **User & Permission Management:** User/Group mantığı, `chmod`, `chown`, **POSIX ACLs (Gelişmiş izinler)**, `Sudoers` yapılandırması.
*  **Process & Resource Management:** `ps`, `top`, `kill`, **systemd & systemctl (Servis mimarisi)**, arka plan süreçleri.
*  **Storage & Disk Management:** **LVM (Logical Volume Manager)** ile dinamik disk büyütme, Dosya sistemleri (ext4, XFS), Mount işlemleri.
*  **Linux Networking & Troubleshooting:** IP tanımlama, `/etc/resolv.conf`, `hosts` dosyası, `ip route`, `ss`, `netstat`, `tcpdump` ve `dig` kullanımı.
---
#### 🛡️ Modül 03: Security Engineering & Incident Management — ⏳ Planned
*  **Threat Modeling & Attacks:** Cyber Kill Chain, APT'ler, DoS/DDoS (SYN Flood), MITM.
*  **Malware & System Defense:** Malware türleri (Worm, Trojan, Sandbox), Forensics için RAM'in önemi, **Linux Hardening Standartları**.
*  **Firewalling:** **iptables** ve **nftables** çalışma mantığı, zincirler (chains) ve kural tanımlama.
*  **System Auditing & Logging:** `/var/log` analizi, **auditd (Linux denetim sistemi)**, Fail2Ban mantığı.
*  **Cryptography & SSH Security:** Simetrik/Asimetrik Şifreleme, SSH Key-Pair mantığı, SSH Hardening (`sshd_config` sıkılaştırma).

---

### 📦 LEVEL 2: INFRASTRUCTURE AS CODE & CONTAINERIZATION — ⏳ PLANNED
**Bulut ve Otomasyon Dünyası**
> Altyapıları kodla yönettiğimiz, sistem yapılandırmalarını otomatikleştirdiğimiz ve konteyner tabanlı platformlar oluşturduğumuz seviye.

#### 📦 Modül 04: Container Technologies (Docker Deep-Dive) — ⏳ Planned
*  **Containerization vs Virtualization:** VM'ler (Hypervisor) ile Konteyner (Container) arasındaki mimari farklar.
*  **Linux Kernel Namespaces & Cgroups:** Docker'ın arkasındaki gerçek Linux teknolojileri.
*  **Container Runtime Internals:** `containerd`, `runc` ve Kubernetes bağlantısını sağlayan **CRI (Container Runtime Interface)** kavramları.
*  **Docker Core:** Docker CLI, Dockerfile yazım kuralları, Çok katmanlı (Multi-stage) Build işlemleri.
*  **Docker Storage & Networking:** Volumes, Bind Mounts, Bridge vs. Overlay Networks.
*  **Container Security & Image Hardening:** Root yetkisi olmadan çalışan (non-root) konteynerler, **Trivy/Grype** ile imaj zafiyet taramaları.
*  **Docker Compose:** Çok konteynerli yerel uygulama ortamları oluşturma.
---
#### ☸️ Modül 05: Kubernetes (Production-Grade Orchestration) — ⏳ Planned
*  **Kubernetes Architecture:** Control Plane (API Server, Etcd, Scheduler, K-C-M) vs. Worker Nodes (Kubelet, Kube-proxy, Container Runtime).
*  **Kubernetes Core Objects:** Pods, ReplicaSets, Deployments, Services (ClusterIP, NodePort, LoadBalancer).
*  **Configuration & Secrets:** ConfigMaps, Secrets, Env Variables.
*  **Kubernetes Security & RBAC:** Rol tabanlı erişim kontrolü (Role, ClusterRole, RoleBinding).
*  **Storage in K8s:** PV (Persistent Volume), PVC (Persistent Volume Claim), StorageClasses.
*  **Advanced Traffic Control:** Ingress Controllers (Nginx, Traefik), CoreDNS, **Service Mesh (Istio / Linkerd - mTLS)** kavramları.
*  **GitOps & Declarative GitOps Agents:** Kubernetes kaynaklarının **Argo CD** veya **Flux CD** ile yönetimi.
*  **Scaling & Reliability:** HPA (Horizontal Pod Autoscaler), Liveness & Readiness Probes (Kendi kendini iyileştiren sistemler).
---
#### ⚙️ Modül 06: Infrastructure as Code (IaC) & Configuration Management — ⏳ Planned
*  **Infrastructure as Code (IaC) Basics:** Deklaratif vs. İmperatif yaklaşım.
*  **Terraform (Altyapı Sağlama):** Providers, Resources, Modules.
*  **Terraform State Security:** `terraform.tfstate` güvenliği, AWS S3 ve DynamoDB ile **State Locking** mekanizması.
*  **Ansible (Yapılandırma Yönetimi):** Agentless mimari, SSH bağlantıları, Inventory, Playbooks, Roles.
*  **Ansible Vault:** Hassas verileri, şifreleri ve SSH anahtarlarını şifreleyerek saklama.
*  **Git & Version Control Integration:** Git branching stratejileri (GitFlow, Trunk-based).

---

### 🚀 LEVEL 3: AUTOMATION, CI/CD & OBSERVABILITY — ⏳ PLANNED
**Sürekli Entegrasyon ve İzlenebilirlik**
> Yazılım teslim süreçlerini otomatikleştirdiğimiz; sistemleri metrik, log ve dağıtık izleme verileriyle gözlemlediğimiz seviye.

#### 🔄 Modül 07: CI/CD Pipelines & GitOps — ⏳ Planned
*  **CI/CD Core Concepts:** Continuous Integration, Continuous Delivery, Continuous Deployment farkları.
*  **Pipeline Platforms:** **GitHub Actions** veya **GitLab CI** kullanarak otomatize pipeline tasarımı.
*  **DevSecOps & Security Gates:** Pipeline içerisine otomatik kod analiz araçları (**SonarQube**) ve secret tarayıcıları (**Gitleaks/TruffleHog**) entegrasyonu.
*  **Artifact Management:** Docker imajlarının depolanması (Docker Hub, GitHub Container Registry - GHCR, JFrog Artifactory).
---
#### 🐍 Modül 08: Systems Automation & Scripting (Bash & Python) — ⏳ Planned
*  **Bash Scripting (Intermediate):** Variables, If-Else, Loops, Functions, Exit Codes, Error Handling (`set -e`).
*  **Regex (Regular Expressions) Mastery:** Log manipülasyonu ve metin analizi için Regex kalıpları.
*  **Automation Use-Cases:** Otomatik sistem yedekleme, log rotasyonu (logrotate) scriptleri, disk doluluk kontrolü.
*  **Python for DevOps:** `os`, `sys`, `subprocess` modülleri ile sistem yönetimi, **Python Requests kütüphanesi** ile API'larla konuşma.
---
#### 📊 Modül 09: Observability, Logging & Monitoring (Sistem İzleme) — ⏳ Planned
*  **The 4 Golden Signals of SRE:** Latency (Gecikme), Traffic (Trafik), Errors (Hatalar), Saturation (Doygunluk) metrikleri.
*  **Metrics & Monitoring:** **Prometheus** mimarisi (Pull-based metrik toplama), **Grafana** ile Dashboard tasarımı.
*  **Centralized Logging:** **ELK Stack (Elasticsearch, Logstash, Kibana)** veya **Grafana Loki** ile log toplama ve log analizi.
*  **Distributed Tracing (Dağıtık İzleme):** Mikroservisler arası istek takibi için **Jaeger** veya **OpenTelemetry** mantığı.
*  **Alerting:** Kritik eşikler için Slack, Microsoft Teams veya PagerDuty entegrasyonu.

---

### ☁️ LEVEL 4: CLOUD COMPUTING & ENTERPRISE ARCHITECTURES — ⏳ PLANNED
**Bulut ve Büyük Sistem Mimarileri**
> Yüksek erişilebilirlik, dayanıklılık, ölçeklenebilirlik ve felaket kurtarma ilkelerine dayalı bulut mimarilerini ele aldığımız seviye.

#### ☁️ Modül 10: Cloud Computing (AWS Focus) — ⏳ Planned
*  **Compute Services:** EC2 (Sanal Sunucular), Auto Scaling Groups.
*  **Networking in Cloud:** VPC (Virtual Private Cloud), Subnets, Route Tables, Internet Gateways, NAT Gateways, Security Groups.
*  **Identity & Access Management (IAM):** Roles, Users, Policies, Principle of Least Privilege (En az yetki kuralı).
*  **AWS Secrets Manager & KMS:** Uygulama şifrelerini ve gizli anahtarlarını AWS üzerinde güvenli şekilde yönetme.
*  **Storage Services:** S3 (Nesne depolama), EBS (Blok depolama), EFS (Paylaşımlı depolama).
*  **Serverless Architecture:** **AWS Lambda** ve **SQS/SNS** kuyruk mekanizmaları ile olay güdümlü mimariler.
---
#### 🏛️ Modül 11: Site Reliability Engineering (SRE) & High Availability — ⏳ Planned
*  **SRE Principles:** SLA (Service Level Agreement), SLO (Service Level Objective), SLI (Service Level Indicator) kavramları.
*  **Chaos Engineering (Kaos Mühendisliği):** Canlı sistem dayanıklılık testleri (**Chaos Monkey** felsefesi).
*  **Post-Mortem & Incident Blameless Culture:** Sistem çöküşlerinden sonra suçsuz raporlama standartları ve kök neden analizleri.
*  **Disaster Recovery (DR):** RTO (Recovery Time Objective) ve RPO (Recovery Point Objective) hesaplama, Backup & Restore stratejileri.
*  **High Availability (HA) Architectures:** Multi-AZ (Bölge), Multi-Region deployment mimarileri.
---

## 🏆 SECTION 2: MILESTONE PORTFOLIO PROJECTS

The following three progressively advanced portfolio projects are planned to validate the knowledge acquired throughout the roadmap.

Each project will combine concepts from multiple modules and will be developed using production-oriented security, automation, reliability, and documentation practices.

---

### 🛡️ PROJECT 1: Hardened & Audited Private Linux Server Infrastructure — ⏳ Planned

> **Focus:** Host Hardening, Stateful Firewalling, Auditing and Intrusion Prevention  
> **Timeline:** *At the end of Level 1 (Module 03).*

#### 📌 Overview

An AWS EC2 instance running Ubuntu Server, configured using layered host-hardening practices.

The project focuses on reducing the exposed attack surface, enforcing a default-deny firewall policy, securing administrative access, detecting repeated authentication failures, and recording security-relevant system events for auditing and incident analysis.

#### 🛠️ Core Technology Stack

* **OS:** Ubuntu Server (AWS EC2)
* **Firewalling:** `iptables` / `nftables`
* **Intrusion Prevention:** Fail2Ban
* **Audit System:** Linux Kernel `auditd`
* **Shell Layer:** Bash (Automated reporting scripts)

#### 📐 Planned Security Controls

* **Default-Deny Inbound Policy:** Unsolicited inbound traffic is denied by default. Only explicitly required services are permitted.
* **Restricted Administrative Access:** SSH access is limited to approved source addresses or a dedicated management path.
* **SSH Hardening:** Direct root login and password authentication are disabled. Administrative access uses Ed25519 public-key authentication.
* **Automated Brute-Force Mitigation:** Fail2Ban analyzes authentication logs and temporarily blocks addresses exceeding defined failure thresholds.
* **Security Auditing:** Custom `auditd` rules record security-relevant events involving authentication, privilege escalation, sensitive files and administrative commands.
* **Automated Reporting:** Bash scripts summarize authentication failures, firewall activity, banned addresses and relevant audit events.

---

### 📦 PROJECT 2: Declarative Provisioning of a Production-Oriented Kubernetes Environment — ⏳ Planned

> **Focus:** Cloud Provisioning, Infrastructure as Code, Configuration Management and Orchestration  
> **Timeline:** *At the end of Level 2 (Module 06).*

#### 📌 Overview

An end-to-end infrastructure automation project that provisions the required cloud infrastructure on AWS and configures a multi-node Kubernetes learning cluster consisting of one control-plane node and two worker nodes without manual console configuration.

The project is designed to demonstrate production-oriented practices such as Infrastructure as Code, automated node configuration, network isolation, secure container images and declarative orchestration.

#### 🛠️ Core Technology Stack

* **IaC Engine:** Terraform
* **Configuration:** Ansible
* **Container Runtime:** `containerd`
* **Bootstrap Engine:** `kubeadm`
* **Cloud Infrastructure:** AWS (VPC, Subnets, EC2, S3, DynamoDB)
* **Orchestration:** Kubernetes

#### 📐 Planned Deployment Architecture

* **Infrastructure Provisioning:** Terraform provisions a customized AWS VPC, public and private subnets, compute resources and supporting infrastructure using reusable modules.
* **Remote State Management:** Terraform state is stored remotely, protected from public access and configured with appropriate locking, encryption and versioning mechanisms.
* **Automated Node Preparation:** Ansible connects through SSH and configures required kernel modules, `sysctl` values, packages, Kubernetes prerequisites and the container runtime.
* **Automated Cluster Bootstrap:** Ansible initializes the control-plane node using `kubeadm`, retrieves the required join command securely and connects the worker nodes to the cluster.
* **Secure Container Builds:** Application workloads use secure multi-stage Dockerfiles and run with reduced privileges where possible.
* **Least-Privilege Isolation:** Kubernetes RBAC and NetworkPolicies restrict unnecessary access and pod-to-pod Layer 4 communication.
* **Validation:** Cluster health, node status, workload scheduling, security policies and recovery behavior are tested and documented.

---

### 🚀 PROJECT 3: Resilient Cloud-Native GitOps & Observability Platform — ⏳ Planned

> **Focus:** DevSecOps, Declarative CD, SRE Practices, Observability and Multi-AZ Fault Tolerance  
> **Timeline:** *At the end of Level 4 (Module 11).*

#### 📌 Overview

The final milestone project implements a complete automated software delivery lifecycle.

Application code is validated through CI security controls, packaged as container images, deployed declaratively through GitOps, monitored proactively and configured to recover from supported component failures.

#### 🛠️ Core Technology Stack

* **CI Engine:** GitHub Actions
* **GitOps Engine:** Argo CD
* **Code Quality Analysis:** SonarQube
* **Secret Scanning:** Gitleaks / TruffleHog
* **Container Image Scanning:** Trivy
* **SRE Logging & Metrics:** Prometheus, Grafana, Loki
* **Runtime Target:** Kubernetes (AWS EKS or Self-Managed Multi-Node Cluster)

#### 📐 Planned Engineering Principles

* **DevSecOps Pipeline:** The CI pipeline performs code validation, secret-leak detection, code quality analysis, container image vulnerability scanning and controlled artifact publishing before deployment.
* **GitOps Architecture:** Argo CD continuously reconciles Kubernetes resources with declarative Git manifests and detects unauthorized configuration drift.
* **Golden Signals Observability:** Prometheus collects application and infrastructure metrics. Grafana dashboards visualize service latency, traffic, error rates and resource saturation.
* **Centralized Logging:** Grafana Loki collects application and platform logs to support troubleshooting and incident analysis.
* **Automated Resilience:** Kubernetes health probes and workload controllers detect unhealthy application instances and replace failed pods when recovery conditions are met.
* **Alerting:** Defined service and infrastructure thresholds generate notifications through an integrated communication channel.
* **Resilience Validation:** Controlled failure scenarios are used to observe recovery behavior and document operational limitations.
* **Multi-AZ Architecture:** Supported cloud resources are distributed across multiple Availability Zones to reduce single points of failure.

---

## 📝 Documentation Approach

Each completed topic may include—depending on requirements—technical documentation in Turkish and English, safety risks, hardening notes, command examples, configurations, diagrams, and practical verification steps.

The aim is not to create a separate laboratory project for every topic. Small exercises will be included where they aid understanding, while more comprehensive applications will be consolidated into milestone portfolio projects.

---

## ⚠️ Project Status Notice

This repository represents an ongoing learning process.

Planned modules and project descriptions define the intended scope of the roadmap. Technologies, architectures and implementation details may be revised as the projects are built, tested and documented.
