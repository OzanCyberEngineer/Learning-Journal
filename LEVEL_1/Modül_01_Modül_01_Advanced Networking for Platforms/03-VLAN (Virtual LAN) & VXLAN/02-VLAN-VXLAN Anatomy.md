# ⚙️ 2. Anatomy of VLAN (Virtual LAN) and 802.1Q Technology

Partitioning a physical switch into multiple logical switches at the software level is called **VLAN**. Ports on a switch are categorized into two primary types:

* **Access Port:** A port that belongs to only a single VLAN, used to connect end devices like computers, servers, or printers. Devices connected to this port do not carry any tag in their packets; they transmit standard ethernet packets.
* **Trunk Port:** A port acting as a gateway that carries traffic from multiple VLANs across a single physical cable to another switch or a router.



---

### 🏷️ IEEE 802.1Q (VLAN Tagging)

When a packet enters through an Access port and leaves via a Trunk port, the switch inserts a **4-byte "Tag"** into the middle of the original ethernet frame according to the **IEEE 802.1Q** standard.



The most critical part of this tag is the **12-bit VLAN ID** field. The receiving switch inspects this tag to identify which VLAN the packet belongs to, strips the tag off, and forwards the clean packet to the appropriate destination Access port.

---

### ⚠️ Limits of Traditional VLAN and Its Collapse in the Cloud Era

While VLAN is an excellent technology for traditional networks, it has hit two major brick walls in massive cloud data centers (like AWS) and modern Kubernetes clusters:

1. **The 4096 Limit (Scalability Issue):** Because the VLAN ID field is only 12-bit, it is mathematically limited to a maximum of $2^{12} = 4096$ unique VLANs. For multi-tenant public cloud providers hosting millions of customers, this limit is laughably small.
2. **Physical Switch Dependency (Rigidity):** Extending a VLAN or allowing communication between two hosts requires that VLAN ID to be manually configured on every physical router and switch along the path. This slow, physical-dependent method cannot keep up with the agility of the cloud, where thousands of virtual machines and pods are spun up and torn down every single second
---
# 🚀 3. VXLAN (Virtual Extensible LAN) and Overlay Networks

The hero that rescued the network infrastructure of the cloud and Kubernetes world is **VXLAN**. VXLAN is a tunneling (**Overlay**) technology that encapsulates Layer 2 (Ethernet) frames inside Layer 3 (IP) packets for transport.

---

### 🌟 Revolutions Introduced by VXLAN

* **16 Million Network Isolations:** VXLAN utilizes a **24-bit VNI (VXLAN Network Identifier)** field for tagging. This mathematically enables up to $2^{24} = 16.777.216$ (approximately 16 million) unique, isolated virtual networks!
* **Physical Infrastructure Independence:** It completely eliminates the need to configure thousands of VLANs on physical switches. Physical switches only need to route standard IP packets.

---

### 🏗️ Underlay vs. Overlay Concept



* **Underlay (Physical Infrastructure):** The physical cables, switches, and routers on the data center floor. Their sole responsibility is to route IP packets from point A to point B as fast as possible. They are completely unaware of the virtual networks or which Pod is talking to which.
* **Overlay (Virtual Network):** The software-defined virtual networks running on top of the physical infrastructure, connected via virtual tunnels. (CNIs in Kubernetes like **Calico**, **Flannel**, or **Cilium** build exactly this overlay layer).

---

### 🔌 VTEP (VXLAN Tunnel End Point)

These represent the entry and exit points of the tunnel. This unit is responsible for encapsulation and decapsulation.



* When a virtual machine or a Kubernetes Pod sends a frame, the local **VTEP** intercepts this L2 frame, encapsulates it inside a standard **UDP packet (typically Port 4789)**, and sends it to the remote VTEP.
* The destination VTEP receives the packet, strips away (decapsulates) the outer UDP wrapper, and delivers the original L2 ethernet frame to the target pod/machine. A VTEP can be implemented in software (**Linux Kernel**) or hardware (**ASIC on smart switches**).
