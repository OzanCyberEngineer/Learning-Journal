# Learning Report: Anatomy of a Web Request (End-to-End Packet Journey)

This report details the step-by-step lifecycle of an network request when a client attempts to access a web platform (e.g., `github.com`), exploring how Layer 2, 3, 4, and 7 protocols work in unison behind the scenes.

---

## 🏃‍♂️ Step-by-Step Network Flow

### Step 1: Name Resolution Layer (DNS Query)
The client machine (`192.168.1.10`) cannot route packets to an external network without knowing the logical destination IP address of the host.
* **Operation:** If the browser does not find the IP mapping in its local cache, it initiates a DNS request over UDP Port 53 to the designated DNS resolver: *"What is the IP address for github.com?"*
* **Result:** The resolver returns the public destination IP address: `140.82.113.25`. The logical target is now identified.

### Step 2: Local Address Resolution (ARP & Switching)
Before building the packet, the OS reviews its routing table (`netstat -r`) and determines that the destination IP belongs to an external network (WAN), not the local subnet (LAN). Therefore, the packet must be delivered to the default gateway (`192.168.1.1`).
* **Operation:** The system inspects its local **ARP Cache** (`arp -a`) to find the gateway's physical identity. If unknown, it broadcasts an ARP Request frame to discover the gateway's MAC address (`c0-51-5c...`).
* **Result:** The packet is encapsulated with the gateway's destination MAC address at Layer 2 and forwarded by the local Switch directly to the router's physical interface.

### Step 3: Transport Layer Security (TCP 3-Way Handshake)
To establish a secure, reliable communication channel via HTTPS, the browser targets **Port 443** on the destination server. A formal TCP connection must be established before sending high-level data.
* **Operation (The Handshake):**
    1.  Client -> Server: `SYN` (Connection synchronization request)
    2.  Server -> Client: `SYN-ACK` (Acknowledgment and server synchronization request)
    3.  Client -> Server: `ACK` (Final acknowledgment to establish the path)
* **Result:** The connection state transitions to `ESTABLISHED`, ensuring a reliable and ordered transport medium.

### Step 4: Perimeter Defense & Ingress/Egress Routing (NAT & Routing)
As the packet leaves the local area network boundary, the gateway manages the transition to the public web.
* **NAT (Network Address Translation):** The non-routable internal Private IP (`192.168.1.10`) is stripped by the router and replaced with its own legally assigned **Public IP** address.
* **Routing:** The packet is sent to the Internet Service Provider's (ISP) core routers. It travels through undersea fiber-optic lines and autonomous system routers, leaping from node to node (Hops), until it reaches GitHub's data center perimeter firewalls.

### Step 5: Server Response and Browser Rendering
GitHub's web servers process the incoming HTTP GET request and transmit the website's source code (HTML, CSS, JS) back through the established network path. The client's browser parses these data structures and renders the final visual interface on the screen.

---

## 🕵️‍♂️ Cybersecurity Perspective (Attack Surface Analysis)
Throughout this complete connection lifecycle, each technological step represents a distinct attack surface for malicious actors:
1.  **At the DNS Phase:** Attackers can deploy DNS Spoofing to poison the lookup phase, redirecting legitimate users to phishing deployment servers.
2.  **At the ARP Phase:** A local attacker can use ARP Poisoning to mimic the gateway's physical address, intercepting all outbound enterprise traffic via Man-in-the-Middle (MitM).
3.  **At the TCP Phase:** Threat actors can spoof source IPs to flood a target with unfinished handshakes, exhausting server queues and creating a Denial of Service (SYN Flood DDoS).
