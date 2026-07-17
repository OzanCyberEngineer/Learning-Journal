# Lab 01 — Physical & Data Link Layers

## ARP and Local Network Dynamics Analysis

This laboratory work analyzes the operation of the Address Resolution Protocol (ARP), neighbor table management, and live network traffic at the Linux kernel level. Gateway detection, Layer 2 connectivity tests, static entry岸 manipulations, and packet capturing procedures were performed within the scope of this study.

> This study analyzes network behaviors observable in Linux and cloud environments rather than directly inspecting physical hardware.

## Purpose

* Verification of the default gateway information and the active network interface.
* Testing end-to-end connectivity at the Layer 2 (Data Link Layer) level using the `arping` tool.
* Inspection of the Linux kernel neighbor table (`ip neigh`) and analysis of status codes.
* Application of adding and deleting static, permanent (`PERMANENT`) ARP entries within the kernel table.
* Capturing and inspecting raw ARP request and reply packets on the network interface using the `tcpdump` packet analysis tool.

## Used Environment and Tools

| Component | Used environment or tool |
|---|---|
| Cloud platform | AWS EC2 |
| Operating system | Ubuntu Server 24.04 LTS |
| Network interface | ens5 |
| Tools used | iproute2 (ip route, ip neigh), arping, tcpdump |

> Public IP addresses and authentication credentials have been omitted for security reasons.

## Network Architecture or Topology

```text
Ubuntu EC2 Instance
Private IP: 172.31.43.154
        |
        | Network Interface: ens5 (Ethernet)
        |
        v
AWS VPC Gateway / Router
Gateway IP: 172.31.32.1 [MAC: 06:9d:9b:ec:1c:eb]
```

## Application Steps

### 1. Determining the Default Gateway
A filtering command was executed to verify the active default gateway, routing table, and egress interface in the system.

Command:
```bash
ip route show | grep default
```

Output:
```text
default via 172.31.32.1 dev ens5 proto dhcp src 172.31.43.154 metric 100
```

Technical Description:
It was determined that the system's default routing destination is the IP address `172.31.32.1`, packets are sent via the `ens5` network interface, and the private IP address of the server is `172.31.43.154`.

### 2. Live ARP Query at Layer 2 (`arping`)
Direct Layer 2 connectivity testing was performed by sending ARP requests instead of ICMP (IP layer) packets to the identified default gateway.

Command:
```bash
arping -I ens5 172.31.32.1
```

Output:
```text
ARPING 172.31.32.1 from 172.31.43.154 ens5
Unicast reply from 172.31.32.1 [06:9D:9B:EC:1C:EB] 0.560ms
Unicast reply from 172.31.32.1 [06:9D:9B:EC:1C:EB] 0.589ms
Unicast reply from 172.31.32.1 [06:9D:9B:EC:1C:EB] 0.569ms
Unicast reply from 172.31.32.1 [06:9D:9B:EC:1C:EB] 0.574ms
Unicast reply from 172.31.32.1 [06:9D:9B:EC:1C:EB] 0.580ms
Unicast reply from 172.31.32.1 [06:9D:9B:EC:1C:EB] 0.577ms
Unicast reply from 172.31.32.1 [06:9D:9B:EC:1C:EB] 0.581ms
Unicast reply from 172.31.32.1 [06:9D:9B:EC:1C:EB] 0.577ms
Unicast reply from 172.31.32.1 [06:9D:9B:EC:1C:EB] 0.570ms
Unicast reply from 172.31.32.1 [06:9D:9B:EC:1C:EB] 0.584ms
```

Technical Description:
Uninterrupted unicast replies were received for the queries sent to the gateway via the `ens5` interface. As a result, the hardware (MAC) address of the gateway was verified as `06:9d:9b:ec:1c:eb`.

### 3. Inspection of the Neighbor Table
The IP-to-MAC address mapping table cached in RAM by the Linux kernel was checked.

Command:
```bash
ip neigh show
```

Output:
```text
172.31.32.1 dev ens5 lladdr 06:9d:9b:ec:1c:eb REACHABLE
```

Technical Description:
The mapping entry for the gateway was observed in the table with the status code `REACHABLE`. This indicates that the network object is accessible and actively verified within the kernel validity timeout period.

### 4. Simulating Static and Permanent ARP Entry Creation
A permanent ARP entry was manually injected into the table to simulate network security testing or specific architectural requirements.

Command:
```bash
sudo ip neigh add 192.168.1.50 lladdr 00:11:22:33:44:55 dev ens5
ip neigh show
```

Output:
```text
172.31.32.1 dev ens5 lladdr 06:9d:9b:ec:1c:eb REACHABLE
192.168.1.50 dev ens5 lladdr 00:11:22:33:44:55 PERMANENT
```

Technical Description:
The sample MAC address was successfully defined for the IP address `192.168.1.50`. The `PERMANENT` keyword in the output states that this entry is static, immune to kernel timeout mechanisms, and cannot be overwritten by incoming dynamic ARP packets.

### 5. Deleting the Created Static ARP Entry
The injected static entry was removed to maintain table integrity and revert the system to its default state.

Command:
```bash
sudo ip neigh del 192.168.1.50 dev ens5
ip neigh show
```

Output:
```text
172.31.32.1 dev ens5 lladdr 06:9d:9b:ec:1c:eb REACHABLE
```

Technical Description:
The targeted IP address was removed from the neighbor table using the `del` parameter, clearing the cache at the kernel level back to its initial state.

### 6. Monitoring Live ARP Traffic with tcpdump
A packet analysis session was initiated to capture raw packets traversing the network interface and verify the protocol loop.

Command:
```bash
sudo tcpdump -pni ens5 arp
```

Output:
```text
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on ens5, link-type EN10MB (Ethernet), snapshot length 262144 bytes
19:03:09.064825 ARP, Request who-has 172.31.43.154 tell 172.31.32.1, length 28
19:03:09.064844 ARP, Reply 172.31.43.154 is-at 06:62:d3:8c:29:27, length 28
^C
2 packets captured
2 packets received by filter
0 packets dropped by kernel
```

Technical Description:
A raw Layer 2 ARP Request (who-has) packet sent by the gateway (`172.31.32.1`) to discover the MAC address of our local server (`172.31.43.154`) was captured. Immediately following, it was verified on live traffic that our server responded with an ARP Reply (is-at) packet announcing its physical hardware address (`06:62:d3:8c:29:27`).

## Technical Calculation or Analysis

The payload size of the captured standard ARP packets was analyzed and verified to be fully compliant with RFC standards:

```text
Hardware Type (2B) + Protocol Type (2B) + Hardware Length (1B) + Protocol Length (1B) + Operation Code (2B) + Sender MAC (6B) + Sender IP (4B) + Target MAC (6B) + Target IP (4B)
2 + 2 + 1 + 1 + 2 + 6 + 4 + 6 + 4 = 28 bytes
```

The `length 28` statement in the `tcpdump` output proves that the raw ARP payload encapsulated within the Ethernet frame exactly matches this structural calculation.

## Lessons Learned

* Learned how to analyze the default gateway address and active route priorities via the `ip route` command output.
* Experienced performing endpoint connectivity tests directly on the Data Link Layer (Layer 2) using the `arping` tool, independent of the IP layer.
* Understood the structure and operation of the neighbor table (`ip neigh show`), where the Linux kernel stores IP-to-MAC address mappings.
* Practiced the architectural differences between `REACHABLE` and `PERMANENT` status codes, observing the role of static ARP entries in network hardening.
* Mastered protocol-specific filtering (`arp`) on live network interfaces and managed packet analysis processes using the `tcpdump` tool.
* Matched theoretical protocol steps with practical console outputs by examining the `Request (who-has)` and `Reply (is-at)` loop in real-time traffic.
* Verified through output analysis why an ARP payload structurally occupies exactly 28 bytes of data.

## Conclusion

In this laboratory work, address resolution processes between Layer 2 and Layer 3 were analyzed on a Linux server running within the AWS cloud infrastructure. Using `iproute2` utilities, `arping`, and `tcpdump`, both the kernel space memory tables and raw traffic passing through the network interface were successfully manipulated and inspected. 

During the executed steps, the ARP relationship established with the gateway was validated, static entry processes were handled, and the captured packet sizes were observed to match theoretical standards precisely.
