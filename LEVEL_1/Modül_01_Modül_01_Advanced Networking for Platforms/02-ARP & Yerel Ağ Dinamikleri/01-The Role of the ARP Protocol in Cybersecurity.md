# 🏴‍☠️ 3. Cybersecurity Logic and Exploitation (ARP Poisoning)

There is no authentication mechanism in the ARP protocol. If a device out of nowhere falsely claims on the network, *"This is my IP, and this is my MAC address,"* other devices will accept this claim without questioning it.

---

### 😈 Attack Scenario (Man-in-the-Middle)
An attacker (MAC: `CC:CC...`) continuously sends spoofed ARP packets (**Gratuitous ARP**) to the victim (MAC: `AA:AA...`) claiming:

> *"I am the Gateway (192.168.1.1), and my MAC address is now CC:CC..."*

At the same time, the attacker tells the actual Gateway:

> *"I am the victim (192.168.1.50), and my MAC address is now CC:CC..."*

* **The Result:** Both ends update their ARP tables. When the victim attempts to access the internet, all packets route through the attacker. The attacker intercepts, inspects, or alters the packets before forwarding them to the actual Gateway, leaving both targets completely unaware of the compromise.

---

### 🛡️ Defense Mechanism: DAI (Dynamic ARP Inspection)
DAI functions as a boundary guard operating on enterprise-level switches.

1. The switch tracks the IP allocations of every device on the network during the DHCP phase to build a secure repository (**DHCP Snooping Binding Table**).
2. Whenever an ARP packet enters the network, the switch cross-references the IP-to-MAC pair within the packet against its secure table.
3. If an attacker tries to spoof the Gateway, the switch verifies: *"The IP 192.168.1.1 does not map to the MAC address on this port!"* It then drops the packet (discards it) and shuts down the port for security (**err-disable**).
