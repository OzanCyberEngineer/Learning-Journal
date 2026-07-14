# 🚨 VLAN & VXLAN Security: Vulnerabilities and Attack Vectors

An analysis focused on the security dimension of network segmentation, exploring how attackers bypass physical and virtual boundaries to achieve lateral movement.

---

## 🎯 1. VLAN Hopping Attacks

Since firewalls are deployed at the network layer, an attacker's primary objective is to breach VLAN boundaries and infiltrate adjacent networks (**Lateral Movement**). This section covers the infiltration of a restricted VLAN to which the attacker is not physically connected, through two popular methods:

### A) Switch Spoofing
* **The Mechanism:** Switch ports are typically categorized as either **Access Ports** (connected to end devices) or **Trunk Ports** (connected to other switches). Switches automatically negotiate which ports will function as trunk links using **DTP (Dynamic Trunking Protocol)**. An attacker sends spoofed DTP packets from a computer connected to a standard switch port, stating to the switch: *"I am also a switch, let us establish a trunk link between us."* If the switch accepts this, it transitions the port into trunk mode. Consequently, traffic from all active VLANs across the network begins flowing directly to the attacker's machine.
* **Weakness:** Leaving switch ports open to dynamic negotiation (DTP) by default.
* **Hardening Solution:** All ports intended for user/end-device connectivity must be manually configured as `switchport mode access`, and DTP must be explicitly disabled via `switchport nonegotiate`.

### B) Double Tagging
* **The Mechanism:** An attacker places two overlapping VLAN tags onto a single transmitted packet. For example: **Outer Tag:** `VLAN 10` (The attacker's assigned VLAN) and **Inner Tag:** `VLAN 20` (The intended victim's restricted VLAN). When this packet reaches the first switch, the switch strips away the outer `VLAN 10` tag and forwards the packet across the trunk port to the adjacent switch. However, the inner `VLAN 20` tag remains untouched inside the packet. Upon receiving the packet, the second switch processes the remaining tag and routes it directly to the victim on `VLAN 20`. This allows the attacker to unilaterally inject packets into the victim's network segment.
* **Weakness:** Leaving the switch's **Native VLAN** value (the default VLAN used to accept untagged frames) mapped to the same ID where active users reside (typically `VLAN 1`).
* **Hardening Solution:** The Native VLAN value must be explicitly mapped to an unused dummy ID (e.g., `VLAN 999`) where no active production hosts or users exist.

---

## 🦠 2. VXLAN Vulnerabilities & Tunnel Exploitation

Because VXLAN operates entirely via software and dynamically routes connections, legacy hardware-centric switch security controls prove insufficient in this domain.

* **Unencrypted Tunnel Traffic:** Standard VXLAN tunnel transit (utilizing **UDP Port 4789**) is entirely unencrypted. If an adversary compromises the underlying physical backbone network (**Underlay**) and executes packet sniffing, they can openly read all cloud infrastructure or Kubernetes cluster internal Pod traffic, including raw passwords and sensitive data blocks.
* **Rogue VTEP (Fake Tunnel Endpoint):** An attacker who successfully compromises the physical underlay network can configure their host machine to act as an unauthorized, malicious VTEP. This enables them to inject forged VXLAN packets into the transport path, successfully forcing external data packets inside a secure Kubernetes cluster without possessing valid authorization.
