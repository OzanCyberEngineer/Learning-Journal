# 🌐 VLAN Anatomy & The 802.1Q Standard

A fundamental look into local network segmentation, broadcast domain management, and inter-switch trunking mechanisms.

---

## 🏢 1. VLAN Anatomy: Divide and Conquer in Local Networks

In a traditional network, every device connected to a switch shares the same **Broadcast Domain**. When a computer newly joins the network or needs to resolve a MAC address for an IP via an ARP request, it broadcasts to the entire network: *"Who has this IP?"* The switch receives this packet and floods it to all ports except the one it arrived on.

### The Problem: Broadcast Storms & Security Gaps
* **Broadcast Storm:** If an enterprise scales to 500+ devices on a single flat network, constant broadcast traffic eventually congests the network to a halt.
* **Security Risk:** An administrative machine in the Human Resources department and a computer in the Finance department sharing the same switch-level packet visibility represents a critical security exposure.

### The Solution: Virtual LAN (VLAN)
We logically segment switch ports into distinct groups:
* **Ports 1-5:** ➔ `VLAN 10` (Finance)
* **Ports 6-10:** ➔ `VLAN 20` (Human Resources)

When a device in `VLAN 10` sends a broadcast, the switch restricts the packet delivery solely to ports belonging to `VLAN 10`. This relieves overall network traffic and establishes the initial isolation barrier between departments.

---

## ⚡ Inter-Switch Communication & The 802.1Q Standard

When VLAN 10 members are distributed across two different physical switches, running separate physical cables for each VLAN is highly inefficient. Instead, a single physical link called a **Trunk Port** is established between switches to carry multiplexed traffic for all VLANs.

To prevent frames from mixing across this trunk link, a 4-byte header called the **802.1Q tag** is inserted directly into the Ethernet packet. The critical field within this tag is the **VLAN ID**:

* **Mathematical Limit:** The VLAN ID field is exactly 12 bits in size. This yields a mathematical limit of:
  $$2^{12} = 4096$$
* **Usable Range:** In traditional networks, we can create a maximum of **4094 usable VLANs** (VLAN `0` and VLAN `4095` are reserved for system use).
