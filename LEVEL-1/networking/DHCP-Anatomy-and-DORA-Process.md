# 🌐 DHCP Anatomy & The DORA Process

A technical overview of how network devices automatically acquire IP addresses, subnet masks, default gateways, and DNS server configurations through the four-step DORA handshake.

---

## 🏢 1. The DORA Handshake Schema

The automatic IP assignment mechanism operates through a structured four-step communication protocol between the client and the DHCP server:


   
   Client                                    DHCP Server
     │                                             │
     │ ─── 1. DHCPDISCOVER (Broadcast - L2/L3) ──> │
     │                                             │
     │ <── 2. DHCPOFFER (Unicast / Broadcast) ──── │
     │                                             │
     │ ─── 3. DHCPREQUEST (Broadcast) ───────────> │
     │                                             │
     │ <── 4. DHCPACK (Unicast / Broadcast) ────── │
     │                                             │
---
##🔍 2. Step-by-Step Operational Mechanics

1. Discover (Broadcast)
When a device newly connects via a network cable or Wi-Fi, it lacks an IP address. It transmits a DHCPDISCOVER packet across the network, essentially proclaiming: "Is there a DHCP server on this network? Assign me an IP!" Because the host does not yet possess an IP identity, this packet is broadcasted at both Layer 2 and Layer 3:

Layer 2: Source MAC: Device's Own MAC ➔ Destination MAC: FF:FF:FF:FF:FF:FF

Layer 3: Source IP: 0.0.0.0 ➔ Destination IP: 255.255.255.255

Consequently, this request reaches every single node within the local network segment.

2. Offer
The active DHCP server on the network intercepts the broadcast request. It selects an available IP address from its configured allocation pool (IP Pool) and prepares a proposal for the client. This DHCPOFFER packet contains vital networking configurations intended for the client, including the proposed IP address, subnet mask, Lease Time, and the default gateway address.

3. Request (Broadcast)
The client receives the proposal. In environments where multiple DHCP servers reside and multiple offers are generated, the client typically accepts the first offer that arrives. It then transmits a DHCPREQUEST packet as a widespread Broadcast, stating: "I have accepted the IP address Y offered by server X; all other servers may withdraw their proposals, thank you!"

4. Ack (Acknowledgment)
The corresponding DHCP server processes the request, binds the assigned IP address to the client's unique MAC address within its local database for reservation, and issues a final DHCPACK (Acknowledge) packet back to the device. Upon receiving this packet, the client officially becomes a functional member of the network infrastructure.
