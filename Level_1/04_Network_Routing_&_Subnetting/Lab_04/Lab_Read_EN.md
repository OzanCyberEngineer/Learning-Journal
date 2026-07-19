# Lab 04 — Network Layer

## Routing Table, Destination Route, and Path MTU Analysis

In this laboratory work, the routing mechanism of the kernel, the structure of active routing tables, the determination of the most suitable next-hop for a specific external destination, and the Path MTU (Maximum Transmission Unit) variations across network paths were examined on a Linux operating system.

> This work analyzes network behaviors observable in Linux and cloud environments rather than directly examining physical hardware.

## Objectives

* To analyze the active routing table in the Linux kernel line by line and determine the default gateway.
* To verify Layer 3 connectivity to the local gateway using the ICMP protocol.
* To query the egress interface and source IP address dynamically selected by the kernel for a specific target IP address.
* To trace the routing hops toward a destination in external networks.
* To observe MTU variations (Path MTU Discovery) between the local network with Jumbo Frame support and the internet lines in a cloud environment.

## Environment and Tools Used

| Component | Environment or Tool Used |
|---|---|
| Cloud platform | AWS EC2 |
| Operating system | Ubuntu Server |
| Network interface | ens5 (Elastic Network Adapter) |
| Tools used | iproute2 (ip route), ping, tracepath |

> Public IP addresses and authentication details are not shared due to security reasons.

## Network Architecture

```text
Ubuntu EC2 Instance (ens5)
Private IP: 172.31.43.154 (Local MTU: 9001)
        |
        | Local Subnet: 172.31.32.0/20
        v
AWS VPC Default Gateway
Gateway IP: 172.31.32.1 (External MTU: 1500)
        |
        v
Internet Routers
        |
        v
Destination Server (Google DNS: 8.8.8.8)
```

## Application Steps

### 1. Examining the Active Routing Table

The active routing table, which determines which network interfaces and gateways the system will forward packets through, was queried.

Command:

```bash
ip route show
```

Output:

```text
default via 172.31.32.1 dev ens5 proto dhcp src 172.31.43.154 metric 100
172.31.0.2 via 172.31.32.1 dev ens5 proto dhcp src 172.31.43.154 metric 100
172.31.32.0/20 dev ens5 proto kernel scope link src 172.31.43.154 metric 100
172.31.32.1 dev ens5 proto dhcp scope link src 172.31.43.154 metric 100
```

Technical Explanation:
When the output is analyzed, it is observed that the system resides in a local network with the `172.31.32.0/20` CIDR block and is directly connected to this network as indicated by the `scope link` expression. All unknown external network traffic (`default`) is routed to the default gateway with the IP address `172.31.32.1` using the `ens5` interface.

### 2. Testing Connectivity to the Default Gateway

ICMP echo requests were sent to verify whether the local gateway specified in the routing table is active and accessible.

Command:

```bash
ping -c 3 172.31.32.1
```

Output:

```text
PING 172.31.32.1 (172.31.32.1) 56(84) bytes of data.
64 bytes from 172.31.32.1: icmp_seq=1 ttl=64 time=0.046 ms
64 bytes from 172.31.32.1: icmp_seq=2 ttl=64 time=0.061 ms
64 bytes from 172.31.32.1: icmp_seq=3 ttl=64 time=0.087 ms

--- 172.31.32.1 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2086ms
rtt min/avg/max/mdev = 0.046/0.064/0.087/0.016 ms
```

Technical Explanation:
All 3 transmitted packets were successfully received, reporting a `0%` packet loss. The average latency resulting in a very low value like `0.064 ms` indicates that the gateway is positioned very close within the local virtualization infrastructure.

### 3. Route Query for a Specific Destination

A query was simulated to determine the decision the kernel would make for a packet to be sent to an external IP address (`8.8.8.8`) on the internet.

Command:

```bash
ip route get 8.8.8.8
```

Output:

```text
8.8.8.8 via 172.31.32.1 dev ens5 src 172.31.43.154 uid 1000
    cache
```

Technical Explanation:
The kernel selected the default route as the most specific match to reach the `8.8.8.8` destination. It was verified that packets would leave the `ens5` interface with the source IP address `172.31.43.154` and be delivered to the `172.31.32.1` gateway as the next-hop.

### 4. Tracing the Route and MTU Changes Along the Path

The Layer 3 hops leading to the destination were listed while simultaneously analyzing the MTU limits along the network paths.

Command:

```bash
tracepath -n 8.8.8.8
```

Output:

```text
1?: [LOCALHOST]                  pmtu 9001
1:  172.31.32.1                        0.147ms pmtu 1500
1:  242.14.245.131                     1.188ms asymm  6
2:  no reply
3:  74.125.49.104                      0.582ms asymm 11
4:  no reply
...
25:  no reply
```

Technical Explanation:
When the analysis is initiated, the local operating system interface is observed to support Jumbo Frames (`pmtu 9001`). However, as soon as the packet reaches the first hop (`172.31.32.1`), the Path MTU value drops to `1500` bytes. Certain intermediate routers along the path returned `no reply` due to security policies (ICMP restrictions).

## Technical Calculation or Analysis

In cloud infrastructures (within AWS VPC), the MTU size is configured to `9001 bytes (Jumbo Frame)` by default to provide high data transfer rates within the internal network. The data obtained from the `tracepath` analysis is as follows:

* **Local Host MTU Capacity (EC2):** 9001 bytes
* **Post-Gateway Internet Standard MTU Limit:** 1500 bytes

The moment the packet passes the local gateway and steps into the internet backbone, it hits the maximum packet size limit (Link MTU) on the path, and the Path MTU Discovery (PMTUD) mechanism triggers, automatically reducing the maximum packet size for this flow to 1500 bytes. This is a standard Layer 3 behavior optimized to prevent packet fragmentation over internet lines.

## What I Learned

* I learned how to read the rules, metric values, and direct connection `scope link` expressions within the local routing table using the `ip route show` command.
* I experienced analyzing the responsiveness and stability of the local gateway at the Layer 3 level using the `ping` tool.
* I learned how to query the outcome of the operating system kernel's route selection algorithm (the output of the Longest Prefix Match principle) before actually transmitting packets using the `ip route get` command.
* I learned to trace the intermediate routers a packet visits on its way to the destination and observe asymmetric path (`asymm`) conditions behind gateways using the `tracepath` tool.
* I examined on live outputs how the Jumbo Frame (9001 bytes) structure used in internal networks of cloud environments is scaled down to the standard MTU value of 1500 bytes at the gateway due to internet requirements.
* I technically understood that certain enterprise routers or firewalls along the network path may block ICMP packets, explaining why `no reply` is observed in trace outputs.

## Conclusion

In this laboratory work, Layer 3 routing steps were successfully simulated and verified using Linux network management tools. It was determined that the system routing table operates consistently, uninterrupted access to the local gateway is maintained, and routes toward external destinations are correctly established. Thanks to the `tracepath` analysis, the dynamic behavior of MTU limitations across network infrastructures and the transformation of packet size when transitioning from the cloud environment to the external world were validated with concrete technical data.
