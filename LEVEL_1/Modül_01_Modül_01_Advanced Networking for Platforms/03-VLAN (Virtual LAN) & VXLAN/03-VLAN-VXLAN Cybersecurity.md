# 🛡️ 4. Cybersecurity Perspective: VLAN Hopping (Double Tagging) Attack

An attacker might want to bypass routing and firewalls to compromise a restricted network, sneaking from their own **VLAN 10** directly into **VLAN 20** (such as a secure database network).

---

### 😈 Attack Logic (Double Tagging)



1. **Double Tagging:** The attacker crafts a packet with two nested VLAN tags. They set the outer tag to their own VLAN, **VLAN 10 (Native VLAN)**, and the inner tag to the targeted **VLAN 20**.
2. **First Switch Process:** When the first switch receives the packet, it inspects the outer VLAN 10 tag. Recognizing it as its Native VLAN, the switch strips this tag off and forwards the frame over the Trunk port.
3. **The Leap (Hopping):** The inner **VLAN 20** tag is now exposed as the outer tag. When the neighboring switch receives this frame, it reads only the VLAN 20 tag and delivers the packet directly to the victim on the VLAN 20 network, completely bypassing routers and firewalls.

---

### 🛡️ Defense and Hardening



* **Native VLAN Modification:** The **Native VLAN ID** on trunk ports must never match any VLAN that active users or potential attackers are assigned to. It should be changed to an unused, dummy VLAN ID (e.g., **VLAN 999**).
* **Port Security:** All unused physical switch ports must be administratively shut down (`shutdown`) and removed from default VLANs (VLAN 1).
* **Enforce Native VLAN Tagging:** Enable explicit tagging for Native VLAN traffic (`switchport trunk native vlan tag`) globally on the switches. This prevents the first switch from stripping the native tag and sending it untagged.
