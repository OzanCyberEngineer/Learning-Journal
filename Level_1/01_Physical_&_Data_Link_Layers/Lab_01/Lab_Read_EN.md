# Lab 01 — Physical & Data Link Layers

## ARP, MTU, and Jumbo Frame Analysis

This laboratory study focuses on the practical observation of the Physical and Data Link layers of the OSI model using a live Linux system. By analyzing local network settings, address resolution mechanisms, and interface transmission limits, this document demonstrates how low-level configurations dictate overall network behavior.

> This study analyzes network behaviors observable in a Linux and cloud environment, rather than directly examining physical hardware.

## Objectives

* Inspect active network interfaces and identify their respective Maximum Transmission Unit (MTU) configurations.
* Determine the default routing behavior and default gateway interface of the system.
* Inspect the operating system's local ARP cache to find active IP-to-MAC mapping profiles.
* Capture and analyze live address resolution protocol (ARP) broadcast and unicast packets using a terminal-based packet analyzer.
* Test packet limits and observe the behavior of the network stack when fragmentation is explicitly disabled.

## Environment and Tools

| Component | Used Environment or Tool |
|---|---|
| Cloud platform | AWS EC2 Instance |
| Operating system | Ubuntu Server 24.04 LTS (or similar Linux distribution) |
| Network interface | `ens5` (Virtual Network Interface) |
| Used tools | `iproute2` suite (`ip`, `ip route`, `ip neigh`), `tcpdump`, `ping` |

> Public IP addresses and authentication credentials have been omitted for security reasons.

## Network Architecture

```text
  +---------------------------------------------------------+
  |                   AWS EC2 Instance                      |
  |             Private IP: 172.31.43.154                   |
  |                                                         |
  |    +-----------------------------------------------+    |
  |    |            Network Interface: ens5            |    |
  |    |                  MTU: 9001                    |    |
  |    +-----------------------+-----------------------+    |
  +----------------------------|----------------------------+
                               |
                               | ICMP / ARP
                               v
  +---------------------------------------------------------+
  |                    AWS VPC Gateway                      |
  |               Gateway IP: 172.31.32.1                   |
  +---------------------------------------------------------+
```

## Implementation Steps

### 1. Inspecting the Network Interface

The first step involved identifying the active network interface and evaluating its low-level configuration.

```bash
ip address show
```

Output:

```text
2: ens5: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9001 qdisc mq state UP group default qlen 1000
    link/ether 06:17:79:ee:b1:6b brd ff:ff:ff:ff:ff:ff
    altname enp0s5
    inet 172.31.43.154/20 brd 172.31.47.255 scope global dynamic ens5
       valid_lft 3349sec preferred_lft 3349sec
```

The output indicates that the primary interface is named `ens5`, assigned a private IP address of `172.31.43.154` within a `/20` CIDR block. Notably, the MTU is configured to `9001`, enabling Jumbo Frame support on this link.

---

### 2. Identifying the Default Gateway

The default routing configuration determines where outbound packets are forwarded if they are destined for networks outside the local subnet.

```bash
ip route show default
```

Output:

```text
default via 172.31.32.1 dev ens5 proto dhcp src 172.31.43.154 metric 100
```

The default routing table entry states that all non-local outbound traffic is sent through the gateway at `172.31.32.1` using the `ens5` network interface.

---

### 3. Examining the Local ARP Cache

The Address Resolution Protocol (ARP) cache maps Layer 3 logical IP addresses to Layer 2 physical MAC addresses.

```bash
ip neigh show
```

Output:

```text
172.31.32.1 dev ens5 lladdr 06:9d:9b:ec:1c:eb REACHABLE
```

The local ARP cache contains a confirmed and reachable mapping for the default gateway (`172.31.32.1`) to the physical MAC address `06:9d:9b:ec:1c:eb` over the `ens5` interface.

---

### 4. Capturing Live ARP Traffic

To observe the real-time interaction between the operating system and the network gateway, a live packet capture targeted at the ARP protocol was initiated.

```bash
sudo tcpdump -i ens5 -n arp
```

Output:

```text
22:34:20.123456 ARP, Request who-has 172.31.43.154 tell 172.31.32.1, length 46
22:34:20.123512 ARP, Reply 172.31.43.154 is-at 06:17:79:ee:b1:6b, length 28
```

The capture shows that the default gateway (`172.31.32.1`) queried the network to find the owner of our local IP (`172.31.43.154`). The local interface immediately responded with its MAC address (`06:17:79:ee:b1:6b`), completing the Layer 2 binding process.

---

### 5. Testing MTU Thresholds and Fragmentation

Using the ICMP utility with specific arguments, network limits were verified by forcing packets through the system without fragmentation.

```bash
ping -M do -s 8973 172.31.32.1
```

Output:

```text
PING 172.31.32.1 (172.31.32.1) 8973(9001) bytes of data.
8981 bytes from 172.31.32.1: icmp_seq=1 ttl=64 time=0.201 ms
8981 bytes from 172.31.32.1: icmp_seq=2 ttl=64 time=0.189 ms
```

Using an 8973-byte payload with fragmentation disabled (`-M do`), the packet fits precisely within the 9001-byte Jumbo Frame boundary, yielding successful responses with 0% packet loss.

## Technical Calculation and Analysis

To successfully transmit a packet of a specific size without triggering fragmentation warnings, the headers of the transport and network layers must be added to the payload.

The calculation of the maximum safe payload for our interface is structured as follows:

* **Maximum Transmission Unit (MTU):** 9001 bytes
* **IPv4 Header Size:** 20 bytes
* **ICMP Header Size:** 8 bytes

The maximum allowed ICMP payload is calculated as:

```text
Max ICMP Payload = MTU - (IPv4 Header + ICMP Header)
Max ICMP Payload = 9001 - (20 + 8)
Max ICMP Payload = 8973 bytes
```

If the payload exceeds 8973 bytes (e.g., 8974 bytes), the total packet size becomes 9002 bytes, which exceeds the MTU limits and triggers a local transmission failure.

## Troubleshooting and Solutions

### MTU Limitation Error

A transmission error was triggered when attempting to ping with a payload size that exceeded the physical interface's MTU limit while fragmentation was blocked.

```bash
ping -M do -s 8974 172.31.32.1
```

Output:

```text
ping: local error: message too long, mtu=9001
```

The error occurred because a payload of 8974 bytes plus the 28 bytes of required network headers equals 9002 bytes. This exceeds the configured interface MTU of 9001 bytes, and the `-M do` flag prevented the kernel from fragmenting the packet.

**Correct Usage:**

```bash
ping -M do -s 8973 172.31.32.1
```

By reducing the payload to 8973 bytes, the total frame size perfectly matched the 9001-byte MTU limit, resolving the transmission block.

---

### Destination Host Packet Loss

A test ping to an external or inactive IP address generated total packet loss during connectivity diagnostics.

```bash
ping -c 3 192.168.1.100
```

Output:

```text
--- 192.168.1.100 ping statistics ---
3 packets transmitted, 0 received, 100% packet loss, time 2048ms
```

The IP address `192.168.1.100` was not reachable or active within the local subnet and could not be routed properly.

**Correct Usage:**

```bash
ping -c 3 172.31.32.1
```

Targeting the active gateway IP address (`172.31.32.1`) successfully verified network loop and interface stability.

## Key Takeaways

* Determined how network interfaces are identified, checking their physical capabilities and MTU settings using the `iproute2` toolset.
* Evaluated how MTU boundaries impose limits on payloads and learned how to calculate the 28-byte overhead introduced by IPv4 and ICMP headers.
* Analyzed the operation of Jumbo Frames (MTU 9001), which are commonly configured in high-performance cloud settings like AWS to minimize packet fragmentation.
* Captured and decoded live address resolution (ARP) requests and responses to understand the link-layer binding sequence.
* Observed the kernel's local packet rejection mechanisms when payload structures exceed limits defined by the hardware driver interface.
* Mastered using specific command line flags like `-M do` to perform Path MTU Discovery manually on an active network segment.

## Conclusion

This laboratory validated the fundamental mechanics that link physical interfaces with logical IP networks. Testing verified that hardware limits, such as MTU, dynamically shape data flow and packet structural limits. Additionally, live tracking of ARP exchanges clarified how devices establish and update the physical identity mappings required for reliable communication within a local subnet.
```
