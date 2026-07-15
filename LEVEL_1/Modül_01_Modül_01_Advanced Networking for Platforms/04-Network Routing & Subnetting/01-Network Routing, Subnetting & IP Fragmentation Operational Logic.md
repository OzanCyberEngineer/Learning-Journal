# 🌐 Module 01 / Topic 04: Network Routing, Subnetting & IP Fragmentation

### 🏢 Analogy: Two Different Neighborhoods and the Postal System



Imagine you live in a massive city. In this city, there are two distinct neighborhoods:
* **Lower Neighborhood** (`192.168.1.0/24`)
* **Upper Neighborhood** (`192.168.2.0/24`)

* **Switch (Local Postman):** Knows only the houses within their own neighborhood. When you write a letter to your neighbor in the Lower Neighborhood and drop it in the mailbox, the local postman (**Switch**) collects it and delivers it directly to your neighbor's door. They never step outside the neighborhood boundaries.
* **Subnet Mask (The Boundary Line):** A logical line that helps you determine whether the recipient of your letter resides in the same neighborhood as you. You look at the address; if it's within your boundary, you hand the letter to the local postman. If it's outside, you know you must send the letter to the central post office at the edge of the neighborhood.
* **Default Gateway (Neighborhood Post Office):** If the letter is destined for someone in the Upper Neighborhood, the local postman doesn't have the authority to deliver it. You must drop the letter off at the main post office at the edge of the neighborhood (**Gateway**).
* **Router (Intercity Cargo Network):** The massive transit system connecting neighborhood post offices. It takes the letter from the Lower Neighborhood post office, looks at its internal roadmaps (**Routing Table**), decides that *"the fastest route to the Upper Neighborhood is via the highway,"* and forwards the package in the correct direction.
---
# ⚙️ 2. IP Addressing and Subnetting Mathematics

Every device connected to the internet must possess a logical identifier: an **IP (Internet Protocol) Address**. Today, we predominantly rely on the 32-bit **IPv4** standard across the globe.

A 32-bit IP address (e.g., `192.168.1.50`) consists of 4 contiguous 8-bit (1 Byte) segments. Each of these segments is called an **Octet**.

---

### 🔍 The Two Components of an IP Address

An IP address is always divided into two key components:
1. **Network ID:** Identifies which specific network (neighborhood) the device belongs to.
2. **Host ID:** Identifies the specific device (building number) within that network.

The mathematical tool used to segregate these two parts is the **Subnet Mask**.



---

### 🔀 CIDR (Classless Inter-Domain Routing) Notation

In the early days of networking, IP addresses were grouped into rigid classes (Class A, B, C), which led to massive address wastage. Today, we utilize the modern and highly flexible system known as **CIDR**.

#### Example Analysis: `192.168.1.0/24`
The `/24` suffix (Prefix) indicates how many bits of the subnet mask, counting from left to right, are set to "1" (dedicated to the network portion).

* **Binary Representation:** `11111111.11111111.11111111.00000000` (exactly 24 consecutive ones)
* **Decimal Representation:** `255.255.255.0`
* **Host Portion:** The remaining 8 bits ($32 - 24 = 8$) represent the block available for host allocation.
* **Total IP Count:** Within this network, we can mathematically generate a total of $2^8 = 256$ addresses.

---

### ⚠️ Critical Rule: Reserved Addresses

The very first and the very last addresses generated in any subnet can never be assigned to physical devices (such as computers or servers):

1. **First Address (Network Address):** Represents the network identity itself (e.g., `192.168.1.0`).
2. **Last Address (Broadcast Address):** Reserved for transmitting packets to all devices within that network simultaneously (e.g., `192.168.1.255`).

Consequently, the formula for calculating the **usable host capacity** of a subnet is:

$$\text{Usable Hosts} = 2^{\text{Host Bits}} - 2$$

*(For our example network: $2^8 - 2 = 254$ usable hosts)*
---

### 🗺️ 3. Routing Dynamics
Routers utilize Routing Tables, which act like world roadmaps, to deliver packets to their destinations. When an IP packet arrives at a router, the following technical pipeline is executed:

Packet Ingestion: The router strips the L2 header and inspects the Destination IP address inside the L3 header.

Table Lookup: It scans the routing table from top to bottom.

Longest Prefix Match (LPM): If multiple route entries match the destination IP, the router always selects the most specific entry (the one with the largest subnet mask or the highest / prefix value).

Example: Suppose the destination is 10.0.1.5. If the table contains both 10.0.0.0/16 and 10.0.1.0/24 entries, the router selects the /24 route because it offers a narrower and more precise destination path.

Default Route (0.0.0.0/0): If the router cannot find any matching entry for the destination IP, instead of dropping the packet immediately, it forwards it to the Default Route, which serves as the gateway to the rest of the world.

### 💥 4. IP Fragmentation and MTU Limits
We previously learned that the MTU (Maximum Transmission Unit) defaults to 1500 Bytes on standard ethernet networks. But what happens if a router is forced to transmit a large packet over a link with a smaller MTU capacity (such as a VPN tunnel)?

This is where IP Fragmentation—the built-in scalability mechanism of Layer 3—comes into play.

### ⚙️ How Fragmentation Works
Suppose we have a 1500-byte IP packet that must traverse a link/tunnel with an MTU limit of 1400 bytes. The router splits this packet into fragments:

Fragment 1: Formatted to fit the 1400-byte limit. The More Fragments (MF) flag in its IP header is set to 1 (indicating "there are more fragments following this one"). The Fragment Offset is set to 0 because it represents the very beginning of the payload.

Fragment 2: Formatted with the remaining 100 bytes (plus the IP header). Since this is the final piece, its MF flag is set to 0 (signaling "I am the final fragment"). The Fragment Offset field is populated with the exact byte boundary marking where this piece starts relative to the original payload.

⚠️ Critical Note: Intermediate routers along the path do not reassemble fragmented packets. The reassembly process is strictly performed by the ultimate destination host at the operating system level.
