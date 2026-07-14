# 🛡️ Section 5: Cybersecurity Perspective & Defense/Hardening Techniques

Vulnerabilities at Layer 1 and Layer 2 **cannot be seen** by software-defined security solutions like Web Application Firewalls (WAFs) or enterprise firewalls. This is because those devices inspect Layer 3 and above. Consequently, attacks at these lower layers target the core infrastructure directly and are highly destructive.

---

### 1. MAC Flooding (Paralyzing the Switch)
An enterprise switch keeps track of which device (MAC address) is connected to which physical port in its memory table, known as the **CAM (Content Addressable Memory) Table**. When a packet arrives, the switch checks this table and forwards the packet only to the destination port.

An attacker sends thousands of packets with fake source MAC addresses to a single switch port within seconds (**MAC Flooding**). Because the CAM table has limited memory, it quickly fills up with these fake entries, causing the switch to malfunction and enter a fail-safe mode.

* **The Result:** The switch begins acting like a **"Hub"**. This means it broadcasts every incoming packet to all physical ports, regardless of the target. The attacker can now monitor (**Sniff**) the network traffic of all other corporate users without any extra effort.

> 🛠️ **Defense / Hardening Techniques (Port Security):**
> Enterprise switches must be hardened at the port level by limiting the maximum number of MAC addresses a port can learn (e.g., `switchport port-security maximum 1`). If more than the allowed number of MAC addresses is detected on that port, the switch automatically shuts down the port or drops the attacker's packets.

---

### 2. MAC Spoofing (Identity Theft)
Many corporate environments use "MAC-Based Authentication" (MAB) or static IP assignments to authorize devices. An attacker copies the MAC address of an authorized target device (like an administrator's laptop) onto their own network card:


🛡️ Defense / Hardening Techniques (802.1X Authentication):
You should never trust MAC addresses alone, as they are very easy to spoof. Instead, integrate 802.1X (RADIUS/TACACS+) authentication. When a device plugs in a cable, it must authenticate using a username/password or a digital certificate. The switch will not allow data traffic on the port until authentication succeeds

🚀 Section 6: MTU and "Dark" Network Issues in Modern System Architecture:
* [ Standard Packet: 1500 Byte ] ---> (Passes Successfully ✅)
* [ Tunnel Packet (VXLAN): 1550 Byte ]
* ---> [ MTU: 1500 Limit ] --->
* (Packet Drop / Fragmentation Issues ❌)
  
⚠️ What is an MTU Black Hole?
If the physical switches in your underlay network are still configured with the default 1500-byte MTU, these large encapsulated packets cannot pass. Routers will try to fragment them, and if they cannot, the packets are dropped.

Users will complain that websites load only halfway, or large files (like pushing Docker images) freeze mid-way, while small packets (like Ping) work perfectly. In networking, this is called an MTU Black Hole.

💡 Architectural Solution: Jumbo Frames
To prevent this issue in modern data centers, Kubernetes environments, and cloud infrastructures, the MTU of physical switches and servers is increased from the default 1500 to 9000 Bytes (Jumbo Frames). This ensures that even with additional tunneling headers, packets are never fragmented, allowing them to travel in one piece with maximum performance.
