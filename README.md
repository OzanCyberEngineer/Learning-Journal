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

## 🗺️ Learning Roadmap — Öğrenme Yol Haritası

### 🌐 LEVEL 1: FOUNDATIONS OF SYSTEMS & SECURITY  
**Sistem ve Güvenlik Temelleri**

> Ağ iletişimi, işletim sistemleri, sistem yönetimi ve temel güvenlik prensiplerinde sağlam bir altyapı oluşturduğumuz seviye.

#### 🌐 Module 01: Advanced Networking for Platforms — 🚧 In Progress

- ✅ [**Physical & Data Link Layers:** OSI ve TCP/IP modelleri, MAC adresleme, fiziksel sinyaller ve MTU (Maximum Transmission Unit) mantığı.](./LEVEL_1/Modül_01_Modül_01_Advanced%20Networking%20for%20Platforms/01-Physical%20&%20Data%20Link%20Layers)
- ✅ [**ARP & Local Network Dynamics:** ARP protokolü, ARP tablosu, ARP Poisoning ve DAI (Dynamic ARP Inspection).](./LEVEL_1/Modül_01_Modül_01_Advanced%20Networking%20for%20Platforms/02-ARP%20&%20Yerel%20Ağ%20Dinamikleri)
- ✅ [**VLAN & VXLAN:** Layer 2 izolasyonu, VLAN Hopping, Double Tagging, VXLAN overlay mimarisi, VTEP ve VNI kavramları.](./LEVEL_1/Modül_01_Modül_01_Advanced%20Networking%20for%20Platforms/03-VLAN%20(Virtual%20LAN)%20&%20VXLAN)
- ✅ [**Network Routing & Subnetting:** IPv4 yapısı, CIDR, subnetting, gateway, IP fragmentation ve statik/dinamik routing kavramları.](./LEVEL_1/Modül_01_Modül_01_Advanced%20Networking%20for%20Platforms/04-Network%20Routing%20&%20Subnetting)
- ⏳ **Transport Layer & Traffic Control:** TCP ve UDP, portlar, TCP three-way handshake, windowing ve congestion control.
- ⏳ **Application Layer Services:** DNS altyapısı, DNS kayıt türleri, DNS Spoofing, DHCP ve DORA süreci, DHCP Snooping, HTTP/HTTPS ve TLS handshake.
- ⏳ **Load Balancing & Proxy Concepts:** Forward proxy, reverse proxy, Layer 4 ve Layer 7 load balancing, SSL termination ve SSL passthrough.

---

#### 🐧 Module 02: Enterprise Linux System Administration — ⏳ Planned

- **Linux File System & FHS:** `/etc`, `/var`, `/proc`, `/sys` ve temel Linux dosya sistemi hiyerarşisi.
- **Text & Stream Manipulation:** Pipes, `grep`, `awk`, `sed` ve metin akışlarının işlenmesi.
- **User & Permission Management:** Kullanıcı ve grup yönetimi, `chmod`, `chown`, POSIX ACLs ve `sudoers` yapılandırması.
- **Process & Resource Management:** `ps`, `top`, `kill`, arka plan süreçleri, `systemd` ve `systemctl`.
- **Storage & Disk Management:** LVM, ext4, XFS, disk bölümleme ve mount işlemleri.
- **Linux Networking & Troubleshooting:** IP yapılandırması, `/etc/resolv.conf`, `/etc/hosts`, `ip route`, `ss`, `netstat`, `tcpdump` ve `dig`.

---

#### 🛡️ Module 03: Security Engineering & Incident Management — ⏳ Planned

- **Threat Modeling & Attack Concepts:** Cyber Kill Chain, APT, DoS/DDoS, SYN Flood ve MITM saldırıları.
- **Malware & System Defense:** Temel zararlı yazılım türleri, bellek analizinin önemi ve Linux hardening standartları.
- **Firewalling:** `iptables` ve `nftables` mimarisi, chain yapıları ve güvenlik kuralları.
- **System Auditing & Logging:** `/var/log`, `auditd`, kimlik doğrulama kayıtları ve Fail2Ban.
- **Cryptography & SSH Security:** Simetrik/asimetrik şifreleme, SSH anahtarları ve `sshd_config` hardening uygulamaları.

---

<details>
<summary><strong>📦 LEVEL 2: INFRASTRUCTURE AS CODE & CONTAINERIZATION — ⏳ Planned</strong></summary>

<br>

> Altyapıları kodla yönettiğimiz, sistem yapılandırmalarını otomatikleştirdiğimiz ve konteyner tabanlı platformlar oluşturduğumuz seviye.

#### 📦 Module 04: Container Technologies — Docker Deep Dive

- **Containerization vs Virtualization:** Sanal makineler ve konteynerler arasındaki mimari farklar.
- **Linux Kernel Namespaces & Cgroups:** Süreç izolasyonu ve kaynak sınırlandırma mekanizmaları.
- **Container Runtime Internals:** `containerd`, `runc` ve CRI (Container Runtime Interface).
- **Docker Core:** Docker CLI, Dockerfile yapısı, image layer mantığı ve multi-stage builds.
- **Docker Storage & Networking:** Volumes, bind mounts, bridge ve overlay networks.
- **Container Security & Image Hardening:** Non-root containers, minimal base images ve Trivy/Grype ile image scanning.
- **Docker Compose:** Çok konteynerli yerel uygulama ortamlarının oluşturulması.

---

#### ☸️ Module 05: Kubernetes — Production-Oriented Orchestration

- **Kubernetes Architecture:** Control plane ve worker node bileşenleri.
- **Kubernetes Core Objects:** Pods, ReplicaSets, Deployments ve Services.
- **Configuration & Secrets:** ConfigMaps, Secrets ve environment variables.
- **Kubernetes Security & RBAC:** Role, ClusterRole, RoleBinding ve least-privilege erişim.
- **Storage in Kubernetes:** Persistent Volumes, Persistent Volume Claims ve StorageClasses.
- **Advanced Traffic Control:** Ingress controllers, CoreDNS ve service mesh kavramları.
- **GitOps Agents:** Argo CD veya Flux CD ile deklaratif kaynak yönetimi.
- **Scaling & Reliability:** HPA, liveness probes, readiness probes ve workload recovery.

---

#### ⚙️ Module 06: Infrastructure as Code & Configuration Management

- **IaC Fundamentals:** Deklaratif ve imperatif yaklaşımlar.
- **Terraform:** Providers, resources, variables, outputs ve reusable modules.
- **Terraform State Security:** Remote state, erişim kontrolü, şifreleme, versioning ve uygun locking mekanizmaları.
- **Ansible:** Agentless mimari, inventory, playbooks, roles ve SSH tabanlı yapılandırma yönetimi.
- **Ansible Vault:** Hassas bilgilerin ve yapılandırma sırlarının korunması.
- **Git Integration:** GitFlow, trunk-based development ve altyapı kodunun versiyonlanması.

</details>

---

<details>
<summary><strong>🚀 LEVEL 3: AUTOMATION, CI/CD & OBSERVABILITY — ⏳ Planned</strong></summary>

<br>

> Yazılım teslim süreçlerini otomatikleştirdiğimiz; sistemleri metrik, log ve dağıtık izleme verileriyle gözlemlediğimiz seviye.

#### 🔄 Module 07: CI/CD Pipelines & GitOps

- **CI/CD Concepts:** Continuous Integration, Continuous Delivery ve Continuous Deployment farkları.
- **Pipeline Platforms:** GitHub Actions veya GitLab CI ile otomatik pipeline tasarımı.
- **DevSecOps Security Gates:** SonarQube, Gitleaks, TruffleHog ve image vulnerability scanning araçlarının pipeline entegrasyonu.
- **Artifact Management:** Docker Hub, GitHub Container Registry ve artifact repository kavramları.
- **GitOps Workflow:** Deklaratif deployment süreçleri, pull-based delivery ve configuration drift yönetimi.

---

#### 🐍 Module 08: Systems Automation & Scripting

- **Bash Scripting:** Variables, conditions, loops, functions, exit codes ve hata yönetimi.
- **Regular Expressions:** Log analizi ve metin işleme için regex kalıpları.
- **Automation Use Cases:** Sistem yedekleme, disk kontrolü, log rotasyonu ve tekrarlayan operasyonların otomasyonu.
- **Python for DevOps:** `os`, `sys`, `subprocess`, API entegrasyonu ve sistem yönetimi.
- **Security Automation:** Log parsers, network utilities ve güvenlik kontrollerinin otomasyonu.

---

#### 📊 Module 09: Observability, Logging & Monitoring

- **The Four Golden Signals:** Latency, traffic, errors ve saturation.
- **Metrics & Monitoring:** Prometheus mimarisi ve Grafana dashboard tasarımı.
- **Centralized Logging:** ELK Stack veya Grafana Loki ile merkezi log yönetimi.
- **Distributed Tracing:** Jaeger ve OpenTelemetry temelleri.
- **Alerting:** Kritik eşikler, alarm kuralları ve bildirim entegrasyonları.
- **Operational Visibility:** Metrik, log ve trace verilerinin birlikte değerlendirilmesi.

</details>

---

<details>
<summary><strong>☁️ LEVEL 4: CLOUD COMPUTING & ENTERPRISE ARCHITECTURES — ⏳ Planned</strong></summary>

<br>

> Yüksek erişilebilirlik, dayanıklılık, ölçeklenebilirlik ve felaket kurtarma ilkelerine dayalı bulut mimarilerini ele aldığımız seviye.

#### ☁️ Module 10: Cloud Computing — AWS Focus

- **Compute Services:** EC2 ve Auto Scaling Groups.
- **Cloud Networking:** VPC, public/private subnets, route tables, internet gateways, NAT gateways ve security groups.
- **Identity & Access Management:** Users, roles, policies ve Principle of Least Privilege.
- **Secrets & Encryption:** AWS Secrets Manager ve AWS KMS.
- **Storage Services:** S3, EBS ve EFS.
- **Serverless Architecture:** AWS Lambda, SQS, SNS ve event-driven architecture.
- **Monitoring & Governance:** CloudWatch, logging, tagging ve temel maliyet görünürlüğü.

---

#### 🏛️ Module 11: Site Reliability Engineering & High Availability

- **SRE Fundamentals:** SLA, SLO, SLI ve error budget kavramları.
- **Chaos Engineering:** Kontrollü hata senaryoları ve dayanıklılık testleri.
- **Blameless Postmortems:** Kök neden analizi ve iyileştirme odaklı olay değerlendirmesi.
- **Disaster Recovery:** RTO, RPO, backup ve restore stratejileri.
- **High Availability:** Multi-AZ ve gerektiğinde Multi-Region mimari yaklaşımları.
- **Reliability Engineering:** Capacity planning, graceful degradation ve operasyonel hazırlık.

</details>

---

## 🏆 Milestone Portfolio Projects

The following three progressively advanced portfolio projects are planned to validate the knowledge acquired throughout the roadmap.

Each project will combine concepts from multiple modules and will be developed using production-oriented security, automation, reliability, and documentation practices.

---

### 🛡️ PROJECT 1: Hardened & Audited Linux Server Infrastructure — ⏳ Planned

> **Focus:** Host Hardening, Firewalling, Auditing and Intrusion Prevention  
> **Timeline:** At the end of Level 1 — Module 03

#### 📌 Overview

An AWS EC2 instance running Ubuntu Server, configured using layered host-hardening practices.

The project focuses on reducing the exposed attack surface, enforcing a default-deny firewall policy, securing administrative access, detecting repeated authentication failures, and recording security-relevant system events for auditing and incident analysis.

#### 🛠️ Core Technology Stack

- **Operating System:** Ubuntu Server on AWS EC2
- **Firewalling:** `nftables` or `iptables`
- **Intrusion Prevention:** Fail2Ban
- **Security Auditing:** Linux `auditd`
- **Automation:** Bash
- **Cloud Platform:** AWS

#### 📐 Planned Security Controls

- **Default-Deny Inbound Policy:** Unsolicited inbound traffic is denied by default. Only explicitly required services are permitted.
- **Restricted Administrative Access:** SSH access is limited to approved source addresses or a dedicated management path.
- **SSH Hardening:** Direct root login and password authentication are disabled. Administrative access uses Ed25519 public-key authentication.
- **Automated Brute-Force Mitigation:** Fail2Ban analyzes authentication logs and temporarily blocks addresses exceeding defined failure thresholds.
- **Security Auditing:** `auditd` rules record security-relevant events involving authentication, privilege escalation, sensitive files and administrative commands.
- **Automated Reporting:** Bash scripts summarize authentication failures, firewall activity, banned addresses and relevant audit events.
- **Documentation:** Architecture notes, configuration explanations, validation steps and security findings are documented in the project repository.

---

### 📦 PROJECT 2: Automated Provisioning of a Production-Oriented Kubernetes Environment — ⏳ Planned

> **Focus:** Cloud Provisioning, Infrastructure as Code, Configuration Management and Orchestration  
> **Timeline:** At the end of Level 2 — Module 06

#### 📌 Overview

An end-to-end infrastructure automation project that provisions an AWS environment and configures a multi-node Kubernetes learning cluster consisting of one control-plane node and two worker nodes.

The project is designed to demonstrate production-oriented practices such as Infrastructure as Code, automated node configuration, network isolation, secure container images and declarative orchestration.

#### 🛠️ Core Technology Stack

- **Infrastructure as Code:** Terraform
- **Configuration Management:** Ansible
- **Container Runtime:** `containerd`
- **Cluster Bootstrap:** `kubeadm`
- **Cloud Platform:** AWS
- **Orchestration:** Kubernetes

#### 📐 Planned Architecture

- **Infrastructure Provisioning:** Terraform provisions the required AWS networking, compute and supporting resources using reusable infrastructure modules.
- **Remote State Management:** Terraform state is stored remotely, protected from public access and configured with appropriate locking, encryption and versioning controls.
- **Automated Node Configuration:** Ansible configures kernel modules, required system parameters, container runtime components, packages and Kubernetes prerequisites.
- **Automated Cluster Bootstrap:** Ansible initializes the control-plane node using `kubeadm`, retrieves the required join command securely and connects the worker nodes to the cluster.
- **Secure Container Builds:** Application workloads use optimized multi-stage Dockerfiles and run with reduced privileges where possible.
- **Workload Isolation:** Kubernetes RBAC and NetworkPolicies restrict unnecessary access and pod-to-pod communication.
- **Validation:** Cluster health, node status, workload scheduling, network policies and recovery behavior are tested and documented.

---

### 🚀 PROJECT 3: Resilient Cloud-Native GitOps & Observability Platform — ⏳ Planned

> **Focus:** DevSecOps, GitOps, Observability, Reliability and Multi-AZ Infrastructure  
> **Timeline:** At the end of Levels 3 and 4

#### 📌 Overview

The final milestone project will implement a complete automated software delivery lifecycle.

Application code will be validated through CI security controls, packaged as container images, deployed declaratively through GitOps and monitored using centralized metrics, logs and alerting. The platform will also include controlled resilience tests and documented recovery behavior.

#### 🛠️ Core Technology Stack

- **CI Engine:** GitHub Actions
- **GitOps Engine:** Argo CD
- **Code Quality Analysis:** SonarQube
- **Secret Scanning:** Gitleaks or TruffleHog
- **Container Image Scanning:** Trivy
- **Container Registry:** Amazon ECR or GitHub Container Registry
- **Metrics:** Prometheus
- **Visualization:** Grafana
- **Logging:** Grafana Loki
- **Runtime Platform:** Kubernetes on AWS

#### 📐 Planned Engineering Practices

- **DevSecOps Pipeline:** The CI pipeline performs code validation, secret scanning, code quality analysis, container image vulnerability scanning and controlled artifact publishing.
- **GitOps Architecture:** Argo CD continuously reconciles Kubernetes resources with declarative Git manifests and identifies unauthorized configuration drift.
- **Golden Signals Observability:** Prometheus collects application and infrastructure metrics. Grafana dashboards visualize latency, traffic, error rates and resource saturation.
- **Centralized Logging:** Grafana Loki collects application and platform logs to support troubleshooting and incident analysis.
- **Automated Recovery:** Kubernetes health probes and workload controllers detect unhealthy application instances and replace failed pods when recovery conditions are met.
- **Alerting and Incident Visibility:** Alerting rules notify the responsible channel when defined service or infrastructure thresholds are exceeded.
- **Resilience Validation:** Controlled failure scenarios are used to observe recovery behavior and document operational limitations.
- **Multi-AZ Design:** Cloud resources are distributed across multiple Availability Zones where supported by the selected architecture.
- **Operational Documentation:** Architecture diagrams, deployment procedures, failure scenarios, limitations and postmortem-style findings are documented.

---

## 🎯 Repository Goals

- Build a strong foundation in systems, networking and security.
- Learn modern DevOps and platform engineering practices progressively.
- Document technical concepts in both Turkish and English.
- Convert theoretical knowledge into practical implementations.
- Build progressively more advanced portfolio projects.
- Develop a security-first and reliability-focused engineering mindset.

---

## 📝 Documentation Approach

Each completed topic may include:

- Turkish technical documentation
- English technical documentation
- Security risks and hardening notes
- Commands, configuration examples or diagrams
- Practical validation steps where appropriate
- References and further reading

Not every topic requires a separate laboratory project. Small practical exercises will be added where they improve understanding, while larger concepts will be combined inside the milestone portfolio projects.

---

## ⚠️ Project Status Notice

This repository represents an ongoing learning process.

Planned modules and project descriptions define the intended scope of the roadmap. Technologies, architectures and implementation details may be revised as the projects are built, tested and documented.
