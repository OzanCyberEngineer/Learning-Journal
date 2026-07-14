# Learning Report: DNS Architecture, Redirection Attacks, and DNS Security Protocols

This report explains the operational workflow of the Domain Name System (DNS) at Layer 7 (Application Layer), analyzes critical redirection attack vectors, and discusses modern security protocols designed to prevent DNS manipulation.

---

## 1. What is DNS and How Does It Work?
Computers communicate across networks strictly using logical IP addresses. Since it is impractical for humans to memorize numerical IP spaces for every website, the Domain Name System (DNS) acts as a global, hierarchical phonebook that translates human-readable domain names (e.g., `github.com`) into computer-readable IP addresses.

### The DNS Lookup Query Chain:
When a user types a domain name into a browser and hits Enter, the operating system triggers the following sequential lookup process within milliseconds:
1.  **DNS Cache:** The system first checks its local memory: *"Have I visited this site recently? Is the IP address already cached locally?"*
2.  **Local Hosts File:** The OS inspects a highly prioritized local text file used for manual name resolution (located at `C:\Windows\System32\drivers\etc\hosts` on Windows systems).
3.  **Recursive Resolver (DNS Server):** If no local mapping exists, the request is forwarded to the gateway, and then to the Internet Service Provider's (ISP) or third-party DNS resolvers (e.g., Google `8.8.8.8`, Cloudflare `1.1.1.1`) to fetch the IP address.
4.  **Connection Establishment:** The retrieved IP address is saved to the local cache, and the browser initiates a direct connection to the destination server.

---

## 2. Cybersecurity Perspective (DNS Layer Attacks)



### DNS Spoofing / Cache Poisoning
* **Attack Mechanism:** This occurs when an attacker executes a Man-in-the-Middle (MitM) attack via ARP poisoning on a local network or compromises a vulnerable DNS server. When the victim requests the IP for `mybank.com`, the attacker injects a fraudulent DNS response faster than the legitimate server, pointing the victim to a malicious IP address (`192.168.1.50`).
* **Impact:** Even though the correct domain remains visible in the browser's address bar, the victim is transparently redirected to a fake landing page (Phishing) designed to steal sensitive credentials.

### Local Hosts File Manipulation (Trojan/Malware Attack)
* **Attack Mechanism:** If a malicious payload (such as a Trojan) compromises a system and gains administrative/root privileges, it directly alters the local `hosts` file. It silently appends a malicious entry like: `192.168.1.50 instagram.com`.
* **Impact:** Since the local `hosts` file has absolute priority over external DNS servers, the operating system redirects all traffic destined for Instagram directly to the attacker's server without ever querying the internet. This bypasses any secure upstream DNS configurations.

---

## 3. Engineering Insight and Modern Defensive Controls (Blue Team Perspective)
The legacy DNS protocol was inherently designed without encryption or built-in authentication, operating over UDP port 53 in plain-text. This flaw allows attackers to easily intercept or spoof traffic. To mitigate these risks, the security industry has developed robust **authentication layers and cryptographic protocols**:

1.  **DNSSEC (Domain Name System Security Extensions):** Adds cryptographic signatures to DNS records. This allows the client to **authenticate** that the received DNS response is legitimate, unaltered, and originated from the true authoritative nameserver.
2.  **DoH (DNS over HTTPS) & DoT (DNS over TLS):** Encapsulates DNS queries within a secure, encrypted tunnel (HTTPS or TLS). This prevents local network sniffers from monitoring domain queries or injecting forged IP responses.
3.  **Endpoint Protection & Hosts Monitoring:** To prevent unauthorized modifications to the local `hosts` file, endpoint security solutions (EDR/Antivirus) must continuously monitor file integrity, and strict access control lists (ACLs) should be applied to keep the file read-only.
