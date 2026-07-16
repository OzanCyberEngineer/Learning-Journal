# 🏆 Systems, DevOps & Platform Engineering Learning Roadmap

## EN

This repository is an ongoing learning journal and practical roadmap created to build a strong technical foundation in system administration, networking, cybersecurity, DevSecOps, cloud computing, Site Reliability Engineering, and platform engineering.

The roadmap combines technical documentation, security-focused analysis, practical validation exercises, and progressively advanced portfolio projects. Content is prepared in both Turkish and English and is updated throughout the learning process.

## TR

Bu depo; sistem yönetimi, ağ teknolojileri, siber güvenlik, DevSecOps, bulut bilişim, Site Reliability Engineering ve platform mühendisliği alanlarında güçlü bir teknik temel oluşturmak amacıyla hazırladığım, devam eden bir öğrenme günlüğü ve uygulama yol haritasıdır.

Yol haritası; teknik dokümantasyonlar, güvenlik odaklı değerlendirmeler, uygulamalı doğrulama çalışmaları ve seviyeler ilerledikçe geliştirilecek kapsamlı portföy projelerinden oluşmaktadır. İçerikler Türkçe ve İngilizce olarak hazırlanmakta ve öğrenme süreci boyunca düzenli olarak güncellenmektedir.

## 📍 Current Progress

| Field | Current Status |
|---|---|
| **Current Level** | Level 1 — Foundations of Systems & Security |
| **Current Module** | Module 01 — Advanced Networking for Platforms |
| **Repository Status** | 🚧 Actively Maintained |
| **Documentation Languages** | Turkish and English |

### Status Legend

- ✅ Completed
- 🚧 In Progress
- ⏳ Planned

> Completion indicators represent the current learning progress. Direct links will be added as the corresponding topic documents are committed to the repository.

## 🗺️ Learning Roadmap — Öğrenme Yol Haritası

### 🌐 Level 1: Foundations of Systems & Security — 🚧 In Progress

**Sistem ve Güvenlik Temelleri**

> Ağ iletişimi, Linux sistemleri, sistem yönetimi ve temel güvenlik prensiplerinde sağlam bir teknik altyapı oluşturmayı amaçlayan seviyedir.

#### 🌐 Module 01: Advanced Networking for Platforms — 🚧 In Progress

- ✅ **Physical & Data Link Layers** — OSI ve TCP/IP modelleri, encapsulation, fiziksel sinyaller, Ethernet frame yapısı, MAC addressing, switching, MTU, interface counters ve Layer 1/2 troubleshooting.
- ✅ **ARP & Local Network Dynamics** — ARP çalışma mantığı, neighbor table, ARP cache durumları, gratuitous ARP, proxy ARP, ARP spoofing riskleri ve Dynamic ARP Inspection.
- ✅ **VLAN & VXLAN** — Layer 2 segmentasyonu, access ve trunk portlar, IEEE 802.1Q tagging, VLAN hopping riskleri, VXLAN overlay mimarisi, VTEP ve VNI kavramları.
- ✅ **Network Routing & Subnetting** — IPv4 adresleme, CIDR, subnetting, routing table, default gateway, static ve dynamic routing temelleri, OSPF/BGP genel mantığı ve IP fragmentation.
- ⏳ **Transport Layer & Traffic Control** — TCP ve UDP, portlar ve socket'ler, TCP three-way handshake, sequence ve acknowledgment numbers, flow control, congestion control, retransmission ve connection termination.
- ⏳ **Application Layer Services** — DNS kayıt türleri, DNS resolution, DNS spoofing riskleri, DHCP DORA süreci, DHCP Snooping, HTTP/HTTPS ve TLS handshake.
- ⏳ **Load Balancing & Proxy Concepts** — Forward proxy ve reverse proxy, Layer 4 ve Layer 7 load balancing, health checks, session persistence, TLS termination ve TLS passthrough.

#### 🐧 Module 02: Enterprise Linux System Administration — ⏳ Planned

- **Linux File System & FHS** — Linux dizin hiyerarşisi, `/etc`, `/var`, `/home`, `/opt`, `/tmp` ve sanal dosya sistemleri olan `/proc` ile `/sys`.
- **Text & Stream Manipulation** — Standard input/output, pipes, redirection ve `grep`, `awk`, `sed`, `cut`, `sort`, `uniq` gibi araçlar.
- **User & Permission Management** — User ve group yönetimi, `chmod`, `chown`, `umask`, POSIX ACLs ve `sudoers` yapılandırması.
- **Process & Resource Management** — Process lifecycle, signals, `ps`, `top`, `kill`, systemd, `systemctl`, background processes ve temel resource takibi.
- **Storage & Disk Management** — Partitioning, filesystem yapıları, ext4, XFS, mount işlemleri, `/etc/fstab` ve LVM.
- **Linux Networking & Troubleshooting** — Interface yönetimi, routing, DNS çözümleme, `/etc/hosts`, `/etc/resolv.conf`, `ip`, `ss`, `tcpdump`, `dig` ve legacy `netstat` kullanımı.
- **Package & Service Management** — Paket yöneticileri, repository kavramı, servis bağımlılıkları, startup davranışı ve güvenli güncelleme yaklaşımı.

#### 🛡️ Module 03: Security Engineering & Incident Management — ⏳ Planned

- **Threat Modeling & Attack Lifecycle** — Temel threat modeling yaklaşımı, varlıklar, trust boundaries, attack surface, Cyber Kill Chain, APT ve saldırı yaşam döngüsü.
- **Network and Availability Threats** — MITM, spoofing, DoS/DDoS, SYN flood ve hizmet kesintisi senaryoları.
- **Malware & System Defense** — Worm, Trojan, ransomware, rootkit gibi malware türleri; sandboxing ve memory analysis gibi savunma ve analiz yaklaşımları.
- **Firewalling** — `iptables` ve `nftables` mimarisi, tables, chains, hooks, connection tracking ve kural değerlendirme mantığı.
- **System Auditing & Logging** — `/var/log`, systemd journal, auditd, log bütünlüğü, merkezi loglama ve güvenlik olaylarının incelenmesi.
- **Log-Based Abuse Mitigation** — Fail2Ban çalışma mantığı, authentication log analizi, geçici engelleme ve sınırlamaları.
- **Cryptography & SSH Security** — Symmetric ve asymmetric cryptography, hashing, SSH key pairs, host keys, `sshd_config` hardening ve güvenli yönetim erişimi.
- **Incident Response Fundamentals** — Detection, triage, containment, eradication, recovery, evidence preservation ve post-incident review.

### 📦 Level 2: Infrastructure as Code & Containerization — ⏳ Planned

**Bulut, Otomasyon ve Konteyner Temelleri**

> Altyapıların kodla tanımlandığı, sistem yapılandırmalarının otomatikleştirildiği ve konteyner tabanlı platformların oluşturulduğu seviyedir.

#### 📦 Module 04: Container Technologies — Docker Deep Dive — ⏳ Planned

- **Containerization vs. Virtualization** — Hypervisor tabanlı virtual machines ile operating-system-level containers arasındaki mimari farklar.
- **Linux Kernel Namespaces & Cgroups** — Container isolation ve resource control mekanizmalarının Linux kernel içindeki temelleri.
- **Container Runtime Internals** — Docker Engine, containerd, runc, OCI specifications ve Kubernetes Container Runtime Interface ilişkisi.
- **Docker Core** — Docker CLI, image ve container lifecycle, Dockerfile yazım kuralları, layers, build cache ve multi-stage builds.
- **Docker Storage** — Writable layers, volumes, bind mounts, tmpfs ve veri kalıcılığı.
- **Docker Networking** — Bridge networks, port publishing, DNS-based service discovery ve multi-host networking temelleri.
- **Container Security & Image Hardening** — Non-root containers, reduced capabilities, read-only filesystems, minimal base images, image provenance ve Trivy/Grype taramaları.
- **Docker Compose** — Çok konteynerli yerel geliştirme ve doğrulama ortamlarının tanımlanması.

#### ☸️ Module 05: Kubernetes Orchestration & Platform Fundamentals — ⏳ Planned

- **Kubernetes Architecture** — API Server, etcd, Scheduler, kube-controller-manager, cloud-controller-manager, kubelet, kube-proxy ve container runtime görevleri.
- **Kubernetes Core Objects** — Pods, ReplicaSets, Deployments, StatefulSets, DaemonSets, Jobs ve CronJobs.
- **Services & Service Discovery** — ClusterIP, NodePort, LoadBalancer Services, EndpointSlices ve CoreDNS.
- **Configuration & Secrets** — ConfigMaps, Secrets, environment variables, mounted volumes ve Kubernetes Secrets nesnelerinin güvenlik sınırlamaları.
- **Kubernetes Security & RBAC** — ServiceAccounts, Role, ClusterRole, RoleBinding, ClusterRoleBinding ve least-privilege authorization.
- **Storage in Kubernetes** — PersistentVolumes, PersistentVolumeClaims, StorageClasses, dynamic provisioning ve access modes.
- **Traffic Management** — Ingress Controllers, Gateway API temelleri, NetworkPolicy ve service mesh yaklaşımları.
- **GitOps & Declarative Delivery** — Kubernetes kaynaklarının Argo CD veya Flux ile deklaratif biçimde yönetilmesi.
- **Workload Health & Recovery** — Startup, readiness ve liveness probes; container restart davranışı ve workload controller'larının desired state'i koruma yaklaşımı.
- **Scaling & Reliability** — Horizontal Pod Autoscaler, PodDisruptionBudget, scheduling constraints, resource requests/limits ve controlled rollout stratejileri.

#### ⚙️ Module 06: Infrastructure as Code & Configuration Management — ⏳ Planned

- **Infrastructure as Code Fundamentals** — Declarative ve imperative yaklaşımlar, idempotency, drift ve repeatability.
- **Terraform Core** — Providers, resources, data sources, variables, outputs, modules, plans ve lifecycle.
- **Terraform State Security** — `terraform.tfstate` güvenliği, Amazon S3 remote state, native S3 state locking, encryption, versioning ve restricted IAM access.
- **Legacy State Locking Considerations** — DynamoDB tabanlı locking yönteminin yalnızca mevcut eski yapıların geçişi veya uyumluluk ihtiyaçları kapsamında değerlendirilmesi.
- **Ansible Fundamentals** — Agentless architecture, SSH connections, inventories, playbooks, handlers, templates ve roles.
- **Ansible Vault** — Hassas değerlerin şifrelenmesi ve secret management sınırlamaları.
- **Git & Version Control Integration** — Commit disiplini, pull requests, code review, GitFlow ve trunk-based development yaklaşımları.
- **Infrastructure Validation** — Formatting, validation, linting, policy checks ve kontrollü plan/apply süreçleri.

### 🚀 Level 3: Automation, CI/CD & Observability — ⏳ Planned

**Sürekli Entegrasyon, Otomasyon ve Gözlemlenebilirlik**

> Yazılım teslim süreçlerinin otomatikleştirildiği; sistemlerin metrics, logs ve traces kullanılarak gözlemlendiği seviyedir.

#### 🔄 Module 07: CI/CD Pipelines & GitOps — ⏳ Planned

- **CI/CD Core Concepts** — Continuous Integration, Continuous Delivery ve Continuous Deployment arasındaki farklar.
- **Pipeline Platforms** — GitHub Actions veya GitLab CI ile workflow, jobs, runners, stages, artifacts ve environment yönetimi.
- **DevSecOps & Security Gates** — SonarQube ile code quality/static analysis, Gitleaks veya TruffleHog ile secret scanning ve Trivy ile container image vulnerability scanning.
- **Artifact Management** — Docker Hub, GitHub Container Registry, Amazon ECR ve JFrog Artifactory gibi registry ve artifact repository çözümleri.
- **Secure Authentication** — Uzun ömürlü cloud credentials yerine OpenID Connect ve kısa ömürlü kimlik bilgilerinin kullanılması.
- **GitOps Delivery** — Uygulama kodu ile deployment state'inin ayrılması, declarative manifests ve reconciliation.
- **Deployment Strategies** — Rolling update, blue/green, canary ve rollback yaklaşımları.

#### 🐍 Module 08: Systems Automation & Scripting — Bash & Python — ⏳ Planned

- **Bash Scripting** — Variables, conditionals, loops, functions, arguments, exit codes ve controlled error handling.
- **Shell Safety Practices** — Quoting, input validation, temporary files, cleanup traps ve `set -Eeuo pipefail` kullanımının bağlama bağlı değerlendirilmesi.
- **Regular Expressions** — Log analizi, validation ve metin işleme için regex temelleri.
- **Automation Use Cases** — Sistem yedekleme, disk kullanım kontrolü, log rotation, service health checks ve raporlama.
- **Python for Operations** — `os`, `pathlib`, `sys`, `subprocess`, `logging`, `json` ve `requests` kullanımı.
- **API Automation** — Authentication, pagination, timeout, retry, error handling ve rate limiting.
- **Secure Automation** — Secrets yönetimi, least privilege, auditability ve destructive action korumaları.

#### 📊 Module 09: Observability, Logging & Monitoring — ⏳ Planned

- **Observability Fundamentals** — Metrics, logs ve traces arasındaki farklar ve birlikte kullanım biçimleri.
- **The Four Golden Signals** — Latency, traffic, errors ve saturation.
- **Metrics & Monitoring** — Prometheus architecture, service discovery, scraping, PromQL ve recording rules.
- **Visualization** — Grafana dashboards, variables, annotations ve veri kaynağı yönetimi.
- **Centralized Logging** — Elasticsearch tabanlı çözümler veya Grafana Loki ile log aggregation ve analysis.
- **Distributed Tracing** — OpenTelemetry instrumentation ve telemetry collection; Jaeger veya Grafana Tempo gibi tracing backend'leri.
- **Alerting** — Prometheus Alertmanager ile notification routing, grouping, inhibition ve Slack, Microsoft Teams, email veya PagerDuty entegrasyonları.
- **Operational Reliability** — Actionable alerts, alert fatigue, runbooks ve incident response bağlantısı.

### ☁️ Level 4: Cloud Computing & Resilient Architectures — ⏳ Planned

**Bulut ve Dayanıklı Sistem Mimarileri**

> Yüksek erişilebilirlik, dayanıklılık, ölçeklenebilirlik, güvenli erişim ve felaket kurtarma ilkelerine dayalı bulut mimarilerinin ele alındığı seviyedir.

#### ☁️ Module 10: Cloud Computing — AWS Focus — ⏳ Planned

- **Compute Services** — Amazon EC2, launch templates ve Auto Scaling Groups.
- **Networking in AWS** — VPC, public/private subnets, route tables, Internet Gateway, NAT Gateway, VPC endpoints ve Security Groups.
- **Identity & Access Management** — IAM users, roles, policies, trust policies ve Principle of Least Privilege.
- **Secrets & Key Management** — AWS Secrets Manager, Systems Manager Parameter Store ve AWS KMS.
- **Storage Services** — Amazon S3, EBS ve EFS arasındaki kullanım ve trade-off farkları.
- **Load Balancing** — Application Load Balancer, Network Load Balancer, health checks ve target groups.
- **Serverless & Event-Driven Architecture** — AWS Lambda, Amazon SQS, Amazon SNS ve event-driven tasarım.
- **Cloud Security & Visibility** — CloudTrail, CloudWatch, VPC Flow Logs ve temel configuration visibility.
- **Cost & Responsibility Model** — Shared Responsibility Model, tagging, lifecycle management ve cost controls.

#### 🏛️ Module 11: Site Reliability Engineering & High Availability — ⏳ Planned

- **SRE Principles** — Service Level Indicators, Service Level Objectives, Service Level Agreements ve error budgets.
- **Reliability Engineering** — Availability, durability, fault tolerance, resilience ve graceful degradation farkları.
- **Incident Management** — Incident detection, triage, escalation, communication ve operational ownership.
- **Blameless Postmortems** — Timeline, contributing factors, root-cause analysis, corrective actions ve learning culture.
- **Chaos Engineering** — Yetkili ortamlarda kontrollü, hipotez tabanlı failure experiments ve blast-radius yönetimi.
- **Disaster Recovery** — Recovery Time Objective, Recovery Point Objective, backup/restore ve recovery testing.
- **High Availability Architectures** — Multi-AZ ve Multi-Region yaklaşımları, failure domains, latency ve maliyet trade-off'ları.
- **Capacity Planning** — Traffic growth, resource saturation, headroom ve scaling decisions.
- **Operational Readiness** — Runbooks, dashboards, alerts, rollback plans ve recovery validation.

## 🏆 Section 2: Milestone Portfolio Projects

The following three progressively advanced portfolio projects are planned to validate the knowledge acquired throughout the roadmap.

Each project combines concepts from multiple modules and is designed around production-oriented security, automation, reliability, validation, and documentation practices.

### 🛡️ Project 1: Hardened & Audited Private Linux Server Infrastructure — ⏳ Planned

> **Focus:** Host Hardening, Stateful Firewalling, Security Auditing and Log-Based Abuse Mitigation  
> **Timeline:** At the end of Level 1 — Module 03

#### Overview

An Ubuntu Server instance running on Amazon EC2 and configured using layered host-hardening practices.

The project focuses on reducing the exposed attack surface, enforcing a default-deny inbound firewall policy, securing administrative access, detecting repeated authentication failures, and recording security-relevant events for auditing and incident analysis.

The project is intended as a security-focused learning environment and portfolio implementation. It is not presented as a highly available or universally production-ready architecture.

#### Core Technology Stack

- **Operating System:** Ubuntu Server on Amazon EC2
- **Firewall Framework:** `nftables`, with `iptables` concepts covered where relevant
- **Automated Abuse Mitigation:** Fail2Ban
- **Audit Framework:** Linux audit subsystem and auditd
- **System Logging:** systemd journal and Linux log files
- **Automation:** Bash reporting and validation scripts
- **Cloud Visibility:** Amazon CloudWatch where appropriate
- **Recovery:** Amazon EBS snapshots and documented restore procedures

#### Planned Security and Operational Controls

- **Default-Deny Inbound Policy:** Unsolicited inbound traffic is denied by default. Only explicitly required services are allowed.
- **Restricted Administrative Access:** SSH is limited to approved source addresses or a dedicated management path.
- **SSH Hardening:** Direct root login and password authentication are disabled. Administrative access uses public-key authentication.
- **Reduced Attack Surface:** Unnecessary services, packages and exposed ports are removed or disabled.
- **Automated Authentication Abuse Mitigation:** Fail2Ban analyzes authentication logs and temporarily blocks addresses that exceed defined failure thresholds.
- **Security Auditing:** auditd rules record selected security-relevant events involving authentication, privilege escalation and sensitive files.
- **Firewall and Authentication Visibility:** Logs are configured and reviewed without assuming that logging alone prevents attacks.
- **Automated Reporting:** Bash scripts summarize authentication failures, firewall events, banned addresses and selected audit records.
- **Recovery Validation:** Snapshot creation and restore procedures are tested and documented.

### 📦 Project 2: Declarative Provisioning of a Production-Oriented Kubernetes Environment — ⏳ Planned

> **Focus:** Cloud Provisioning, Infrastructure as Code, Configuration Management and Kubernetes Orchestration  
> **Timeline:** At the end of Level 2 — Module 06

#### Overview

An end-to-end infrastructure automation project that provisions AWS resources and configures a multi-node Kubernetes learning cluster consisting of one control-plane node and two worker nodes without relying on repeated manual console configuration.

The project demonstrates production-oriented practices such as Infrastructure as Code, automated node configuration, network isolation, secure container builds and declarative orchestration.

A cluster with one control-plane node is not a highly available control-plane architecture. This limitation will be documented explicitly and evaluated separately from the automation objectives.

#### Core Technology Stack

- **Infrastructure as Code:** Terraform
- **Configuration Management:** Ansible
- **Container Runtime:** containerd
- **Cluster Bootstrap:** kubeadm
- **Cloud Infrastructure:** AWS VPC, subnets, EC2 and S3
- **Orchestration:** Kubernetes
- **Container Build:** Docker or another OCI-compatible build workflow
- **Networking:** A selected CNI with documented NetworkPolicy support

#### Planned Deployment Architecture

- **Infrastructure Provisioning:** Terraform provisions a custom AWS VPC, public and private subnets, compute resources and supporting infrastructure through reusable modules.
- **Remote State Management:** Terraform state is stored in Amazon S3 with public access blocked, encryption, versioning, restricted IAM permissions and native state locking.
- **Automated Node Preparation:** Ansible configures kernel modules, required `sysctl` values, packages, Kubernetes prerequisites and containerd.
- **Automated Cluster Bootstrap:** Ansible initializes the control-plane node with kubeadm, handles the join process securely and connects worker nodes to the cluster.
- **Secure Container Builds:** Application workloads use multi-stage builds, minimal base images and non-root execution where supported.
- **Kubernetes Authorization:** RBAC permissions are designed according to least-privilege principles.
- **Network Isolation:** NetworkPolicies restrict unnecessary pod traffic when supported and enforced by the selected CNI.
- **Validation:** Node health, workload scheduling, networking, policy enforcement and recovery behavior are tested and documented.
- **Rebuild Test:** The environment is recreated from version-controlled Terraform and Ansible definitions to identify undocumented manual dependencies.
- **Known Limitations:** Lack of a highly available control plane, single-region scope and learning-environment cost constraints are documented explicitly.

### 🚀 Project 3: Resilient Cloud-Native GitOps & Observability Platform — ⏳ Planned

> **Focus:** DevSecOps, Declarative Delivery, SRE Practices, Observability and Multi-AZ Resilience  
> **Timeline:** At the end of Level 4 — Module 11

#### Overview

The final milestone project implements an end-to-end automated software delivery and operations workflow.

Application code is validated through CI controls, packaged as container images, published to a controlled registry, deployed declaratively through GitOps, observed through metrics, logs and traces, and evaluated through controlled recovery scenarios.

The primary architecture is planned as a single-region Amazon EKS environment distributed across multiple Availability Zones. Multi-Region disaster recovery may be evaluated as a separate extension rather than claimed by default.

#### Core Technology Stack

- **Cloud Infrastructure:** AWS
- **Kubernetes Runtime:** Amazon EKS
- **Infrastructure as Code:** Terraform
- **CI Platform:** GitHub Actions
- **Cloud Authentication:** GitHub Actions OpenID Connect with short-lived AWS credentials
- **Container Registry:** Amazon ECR
- **GitOps Engine:** Argo CD
- **Code Quality & Static Analysis:** SonarQube
- **Secret Scanning:** Gitleaks or TruffleHog
- **Container Image Scanning:** Trivy
- **Policy Enforcement:** Kyverno or an equivalent Kubernetes policy engine
- **Secrets Integration:** External Secrets Operator with AWS Secrets Manager
- **Metrics:** Prometheus
- **Visualization:** Grafana
- **Logging:** Grafana Loki
- **Tracing:** OpenTelemetry with Grafana Tempo or another compatible backend
- **Alerting:** Prometheus Alertmanager
- **Progressive Delivery:** Argo Rollouts where appropriate
- **Backup and Recovery:** Velero and cloud-native data protection mechanisms where applicable

#### Planned Engineering Principles

- **DevSecOps Pipeline:** The CI workflow performs code validation, secret-leak detection, code quality analysis, container image scanning and controlled artifact publishing.
- **Short-Lived Cloud Authentication:** GitHub Actions accesses AWS through OpenID Connect instead of long-lived static credentials.
- **GitOps Architecture:** Argo CD reconciles Kubernetes resources with version-controlled declarative manifests and surfaces configuration drift.
- **Environment Separation:** Staging and production-like environments are logically separated, with stronger account or cluster separation considered as an extension.
- **Secrets Management:** Sensitive values are stored outside Git and synchronized through controlled secret-management integrations.
- **Policy Enforcement:** Admission policies prevent selected insecure workload configurations without being treated as a complete security solution.
- **Golden Signals Observability:** Prometheus and Grafana expose latency, traffic, errors and saturation signals.
- **Centralized Logging:** Loki collects application and platform logs for troubleshooting and incident analysis.
- **Distributed Tracing:** OpenTelemetry captures and exports trace data across supported services.
- **Actionable Alerting:** Alertmanager routes grouped and prioritized alerts through defined notification channels.
- **Progressive Delivery:** Selected workloads use controlled rollout strategies with measurable promotion and rollback conditions.
- **Workload Recovery:** Kubernetes controllers and health checks replace failed workload instances under supported conditions; this does not guarantee recovery from every application or dependency failure.
- **Resilience Validation:** Controlled failure scenarios measure recovery behavior, blast radius and operational limitations.
- **Multi-AZ Deployment:** Workloads and supported cloud resources are distributed across Availability Zones to reduce selected single points of failure.
- **Backup and Restore:** Application state and Kubernetes resources are protected through documented backup and recovery procedures.
- **SRE Practices:** Service Level Indicators, Service Level Objectives, error budgets, incident response and blameless postmortems are incorporated into the project.
- **Rebuild Validation:** Infrastructure and platform components are rebuilt from version-controlled repositories without relying on undocumented deployment steps.

## 📝 Documentation Approach

Each completed topic may include, depending on its requirements:

- Turkish and English technical documentation
- Architecture and data-flow diagrams
- Security risks and defensive controls
- Hardening recommendations and their limitations
- Command and configuration examples
- Troubleshooting methodology
- Practical validation steps
- Production and laboratory environment distinctions
- Official references and standards

The goal is not to create a separate large laboratory project for every topic.

Small exercises are included when they improve understanding. Broader implementations are consolidated into milestone portfolio projects so that related technologies can be demonstrated together within realistic system boundaries.

## ⚠️ Project Status Notice

This repository represents an ongoing learning process.

Completed entries describe topics that have been studied and reviewed. Planned modules and project descriptions define the intended scope of future work.

Technologies, architectures, implementation details and project boundaries may be revised as the systems are built, tested, measured and documented.

A planned feature should not be interpreted as an already implemented or validated capability.
