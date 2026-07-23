# Lab 02 — Application Layer & DNS Analysis

## DNS Querying, Iterative Resolution, and HTTP Header Analysis

In this laboratory exercise, DNS resolution mechanisms, iterative DNS query tracing (`dig +trace`) via root and TLD servers, local DNS resolver configuration (`resolvectl status`), and HTTP/2 protocol response headers (`curl`) were analyzed on a Linux operating system.

> This study analyzes network behaviors observable in a Linux and cloud environment rather than examining physical hardware directly.

## Objectives

* To perform A record queries using the `dig` command and analyze DNS response structures.
* To observe hierarchical (iterative) DNS resolution steps starting from root DNS servers using the `dig +trace` parameter.
* To identify DNS connection errors resulting from the lack of IPv6 network configuration on the system.
* To inspect the status of the `systemd-resolved` service and its interaction with the local AWS VPC DNS server (`172.31.0.2`) using the `resolvectl status` command.
* To analyze HTTP/2 response headers and redirection (301 Redirect) behaviors of HTTPS requests using the `curl` tool.

## Environment and Tools

| Component | Used Environment or Tool |
|---|---|
| Cloud Platform | AWS EC2 (eu-central-1) |
| Operating System | Ubuntu Server 24.04 LTS |
| Network Interface | ens5 |
| Tools Used | BIND dig (9.18.39), systemd-resolved (resolvectl), curl |

> Public IP addresses and authentication credentials have been omitted for security reasons.

## Network Architecture

```text
Ubuntu EC2 Instance (ens5)
  |
  |-- Local Stub Resolver (127.0.0.53:53)
  |      |
  |      +--> AWS VPC DNS Server (172.31.0.2:53)
  |
  +-- Direct External DNS Queries (dig +trace)
         |
         +--> Root DNS Servers (.)
         +--> TLD DNS Servers (.com)
         +--> Authoritative DNS Servers (google.com NS)
```

## Implementation Steps

### 1. Performing a Basic DNS A Record Query

A standard `dig` query was executed to inspect the IP address mappings of a domain name and observe the behavior of the local stub resolver.

Command:

```bash
dig google.com
```

Output:

```text
; <<>> DiG 9.18.39-0ubuntu0.24.04.5-Ubuntu <<>> google.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 63665
;; flags: qr rd ra; QUERY: 1, ANSWER: 6, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 65494
;; QUESTION SECTION:
;google.com.			IN	A

;; ANSWER SECTION:
google.com.		30	IN	A	142.251.20.138
google.com.		30	IN	A	142.251.20.100
google.com.		30	IN	A	142.251.20.113
google.com.		30	IN	A	142.251.20.101
google.com.		30	IN	A	142.251.20.139
google.com.		30	IN	A	142.251.20.102

;; Query time: 0 msec
;; SERVER: 127.0.0.53#53(127.0.0.53) (UDP)
;; WHEN: Thu Jul 23 07:31:08 UTC 2026
;; MSG SIZE  rcvd: 135
```

The query returned 6 IPv4 addresses in the answer section for `google.com`. The query was directed through the local `127.0.0.53` stub resolver, yielding a query time of 0 ms due to local resolution/caching.

### 2. Tracing Hierarchical (Iterative) DNS Resolution Steps

The `+trace` option was used to follow the iterative resolution process starting from the root DNS servers down to the authoritative name servers.

Command:

```bash
dig google.com +trace
```

Output:

```text
; <<>> DiG 9.18.39-0ubuntu0.24.04.5-Ubuntu <<>> google.com +trace
;; global options: +cmd
.			51	IN	NS	g.root-servers.net.
.			51	IN	NS	i.root-servers.net.
.			51	IN	NS	c.root-servers.net.
.			51	IN	NS	b.root-servers.net.
.			51	IN	NS	d.root-servers.net.
.			51	IN	NS	h.root-servers.net.
.			51	IN	NS	f.root-servers.net.
.			51	IN	NS	a.root-servers.net.
.			51	IN	NS	l.root-servers.net.
.			51	IN	NS	k.root-servers.net.
.			51	IN	NS	m.root-servers.net.
.			51	IN	NS	j.root-servers.net.
.			51	IN	NS	e.root-servers.net.
;; Received 239 bytes from 127.0.0.53#53(127.0.0.53) in 2 ms

com.			172800	IN	NS	a.gtld-servers.net.
com.			172800	IN	NS	b.gtld-servers.net.
...
com.			172800	IN	NS	m.gtld-servers.net.
;; Received 1170 bytes from 192.203.230.10#53(e.root-servers.net) in 1 ms

;; UDP setup with 2001:501:b1f9::30#53(2001:501:b1f9::30) for google.com failed: network unreachable.
;; no servers could be reached
...
google.com.		172800	IN	NS	ns2.google.com.
google.com.		172800	IN	NS	ns1.google.com.
google.com.		172800	IN	NS	ns3.google.com.
google.com.		172800	IN	NS	ns4.google.com.
```

The trace demonstrated querying the root server list (`.`), followed by the `.com` TLD servers (`gtld-servers.net`), and finally reaching the authoritative name servers for `google.com` (`ns1-ns4.google.com`). During resolution, UDP requests to IPv6 addresses failed due to missing IPv6 routing, allowing the process to fall back and complete over IPv4.

### 3. Inspecting System DNS Service Status and Resolver Configuration

The `resolvectl status` command was executed to inspect the active DNS client configuration and verify the upstream DNS resolver managed by `systemd-resolved`.

Command:

```bash
resolvectl status
```

Output:

```text
Global
       Protocols: -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
resolv.conf mode: stub

Link 2 (ens5)
    Current Scopes: DNS
         Protocols: +DefaultRoute -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
Current DNS Server: 172.31.0.2
       DNS Servers: 172.31.0.2
        DNS Domain: eu-central-1.compute.internal
```

The output confirmed that interface `ens5` uses `172.31.0.2` (AWS VPC DNS Server) as its upstream DNS server, operating with `resolv.conf` in stub mode.

### 4. Analyzing HTTP/2 Headers and Response Metadata

An HTTPS HEAD request was sent to the target server to inspect the HTTP response headers and server-side redirection behavior.

Command:

```bash
curl -I [https://google.com](https://google.com)
```

Output:

```text
HTTP/2 301 
location: [https://www.google.com/](https://www.google.com/)
content-type: text/html; charset=UTF-8
content-security-policy-report-only: object-src 'none';base-uri 'self';script-src 'nonce-PIjSgROdIRJug6WohRr7AA' 'strict-dynamic' 'report-sample' 'unsafe-eval' 'unsafe-inline' https: http:;report-uri [https://csp.withgoogle.com/csp/gws/other-hp](https://csp.withgoogle.com/csp/gws/other-hp)
date: Thu, 23 Jul 2026 08:07:29 GMT
expires: Sat, 22 Aug 2026 08:07:29 GMT
cache-control: public, max-age=2592000
server: gws
content-length: 220
x-xss-protection: 0
x-frame-options: SAMEORIGIN
alt-svc: h3=":443"; ma=2592000,h3-29=":443"; ma=2592000
```

The server responded via HTTP/2 with a `301 Moved Permanently` status code, redirecting the client to `https://www.google.com/`. Additionally, the `alt-svc` header indicated support for HTTP/3 (QUIC) over port 443.

## Issues Encountered and Solutions

### Failure of IPv6 DNS Resolution Requests

While executing `dig google.com +trace`, errors occurred when attempting to contact authoritative name servers via IPv6 addresses.

Real Error Message:

```text
;; UDP setup with 2001:501:b1f9::30#53(2001:501:b1f9::30) for google.com failed: network unreachable.
;; no servers could be reached
```

The underlying technical cause was that the EC2 instance was provisioned with IPv4 networking only, lacking an active IPv6 interface or default route. Because `dig` attempts both IPv4 and IPv6 resolution by default, queries to unreachable IPv6 endpoints timed out.

Correct Usage:

To prevent IPv6 timeout warnings and restrict queries exclusively to IPv4, the `-4` flag can be passed:

```bash
dig google.com +trace -4
```

## Key Takeaways

* Confirmed that the `NOERROR` status code in a `dig` response header signifies successful query processing.
* Identified `127.0.0.53` as the local `systemd-resolved` stub resolver interface on Ubuntu Linux systems.
* Traced the hierarchical nature of DNS resolution across Root (.), TLD (.com), and Authoritative levels using `dig +trace`.
* Verified that the default DNS resolver in an AWS VPC environment is mapped to the second IP address of the primary subnet block (`172.31.0.2`).
* Observed that DNS queries over UDP/53 generate a `network unreachable` error when IPv6 routes are absent on the host network interface.
* Analyzed HTTP/2 redirection headers and HTTP/3 service advertising (`alt-svc`) using `curl -I`.

## Conclusion

This laboratory exercise analyzed application layer DNS resolution mechanisms and HTTP response behaviors within a Linux/Ubuntu cloud environment. Using `dig`, both local stub resolver operations and iterative root-to-leaf DNS resolution steps were examined, while `resolvectl status` validated the VPC-level upstream resolver configurations.

Network layer limitations, specifically the lack of IPv6 routing, were identified through connection timeout logs and addressed using IPv4-specific flags. Finally, HTTPS header inspections via `curl` illustrated real-world HTTP/2 status codes, URI redirection policies, and modern HTTP/3 alternative service options.
