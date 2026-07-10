# Learning Report: Port Concepts, Transport Layer Protocols (TCP/UDP), and Handshake Security

This report analyzes the logical port architecture at Layer 4 (Transport Layer), contrasts the structural behaviors of TCP and UDP, details the Three-Way Handshake mechanism, and reviews related network-layer cyberattacks.

---

## 1. What is a Port?
An IP address identifies a specific endpoint on a network. However, a device often runs multiple network applications concurrently (such as web browsers, Discord, and streaming services). To direct incoming data packets to the correct application, the operating system uses logical gateways called **Ports**.

A system contains exactly **65,535** available ports, categorized into three operational ranges:
* **0 - 1023:** Well-Known Ports (Reserved for standard system services).
* **1024 - 49151:** Registered Ports (Assigned to specific applications or vendors).
* **49152 - 65535:** Dynamic/Private Ports (Used temporarily by client applications for outbound connections).

### Critical Ports in Cybersecurity Analysis:
| Port Number | Protocol / Service | Security Assessment & Definition |
| :---: | :--- | :--- |
| **21** | FTP (File Transfer Protocol) | Transmits credentials in plain text; highly vulnerable to interception. |
| **22** | SSH (Secure Shell) | Secure, encrypted remote administration protocol. |
| **23** | Telnet | Unencrypted and highly insecure; universally considered a vulnerability in modern environments. |
| **25** | SMTP (Simple Mail Transfer Protocol) | Used for email routing and delivery. |
| **53** | DNS (Domain Name System) | Handles domain name resolution (primarily utilizes UDP). |
| **80** | HTTP | Unencrypted web traffic; completely vulnerable to MitM attacks. |
| **443** | HTTPS (HTTP Secure) | Secure web traffic encrypted via SSL/TLS. |

---

## 2. TCP (Transmission Control Protocol) and the 3-Way Handshake
TCP is a connection-oriented, reliable protocol that guarantees data delivery. It implements error-checking, tracking, and automatic retransmission if packet loss occurs, ensuring data arrives intact and in correct sequence (e.g., web traffic, SSH, file transfers).

### The Three-Way Handshake Mechanism:
Before data transmission begins, TCP establishes a strict logical session using specific flag sequences:
1.  **SYN (Synchronize):** The client sends a packet with the `SYN` flag enabled, containing a random initial Sequence Number, indicating a request to open a connection.
2.  **SYN-ACK (Synchronize-Acknowledgment):** The server acknowledges the request by incrementing the client's sequence number by 1 (`ACK`) and generates its own initial sequence number, responding with a `SYN-ACK` packet.
3.  **ACK (Acknowledgment):** The client increments the server's sequence number by 1 and sends the final `ACK` packet. The session state shifts to **Established**.

---

## 3. UDP (User Datagram Protocol) - Connectionless Transport
UDP is a connectionless protocol that provides fast, unacknowledged data delivery.
* It operates without a handshake mechanism, streaming data directly to the destination without checking for packet loss, latency, or order preservation.
* **Use Cases:** Live video streaming, VoIP voice communication, and online gaming. In these scenarios, reducing latency is more critical than minor packet loss, making UDP the ideal choice.

---

## 4. Cybersecurity Perspective (Attack Vectors)



### Port Scanning
During the reconnaissance phase of a cyberattack, malicious actors use tools like **Nmap** to scan the 65,535 ports of a target IP. The goal is to discover exposed, insecure services (e.g., Port 23 Telnet) or legacy software versions containing known exploits.

### SYN Flood (DDoS Attack)
* **Attack Mechanism:** This attack exploits an inherent design limitation of the TCP handshake. An attacker floods the target server with millions of spoofed `SYN` packets per second. The server allocates system memory for each request and responds with a `SYN-ACK`, waiting for the final `ACK` to complete the connection.
* **Impact:** The attacker purposely suppresses the final `ACK` packets. This fills the server's connection wait pool (**Backlog Queue**), exhausting CPU and RAM resources. Consequently, the server crashes or becomes entirely unresponsive to legitimate users (Denial of Service).
