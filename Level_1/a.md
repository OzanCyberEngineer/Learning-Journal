ubuntu@ip-172-31-42-76:~$ dig google.com

; <<>> DiG 9.18.39-0ubuntu0.24.04.5-Ubuntu <<>> google.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 63665
;; flags: qr rd ra; QUERY: 1, ANSWER: 6, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 65494
;; QUESTION SECTION:
;google.com.                    IN      A

;; ANSWER SECTION:
google.com.             30      IN      A       142.251.20.138
google.com.             30      IN      A       142.251.20.100
google.com.             30      IN      A       142.251.20.113
google.com.             30      IN      A       142.251.20.101
google.com.             30      IN      A       142.251.20.139
google.com.             30      IN      A       142.251.20.102

;; Query time: 0 msec
;; SERVER: 127.0.0.53#53(127.0.0.53) (UDP)
;; WHEN: Thu Jul 23 07:31:08 UTC 2026
;; MSG SIZE  rcvd: 135

ubuntu@ip-172-31-42-76:~$ dig google.com +trace

; <<>> DiG 9.18.39-0ubuntu0.24.04.5-Ubuntu <<>> google.com +trace
;; global options: +cmd
.                       51      IN      NS      g.root-servers.net.
.                       51      IN      NS      i.root-servers.net.
.                       51      IN      NS      c.root-servers.net.
.                       51      IN      NS      b.root-servers.net.
.                       51      IN      NS      d.root-servers.net.
.                       51      IN      NS      h.root-servers.net.
.                       51      IN      NS      f.root-servers.net.
.                       51      IN      NS      a.root-servers.net.
.                       51      IN      NS      l.root-servers.net.
.                       51      IN      NS      k.root-servers.net.
.                       51      IN      NS      m.root-servers.net.
.                       51      IN      NS      j.root-servers.net.
.                       51      IN      NS      e.root-servers.net.
;; Received 239 bytes from 127.0.0.53#53(127.0.0.53) in 2 ms

com.                    172800  IN      NS      a.gtld-servers.net.
com.                    172800  IN      NS      b.gtld-servers.net.
com.                    172800  IN      NS      c.gtld-servers.net.
com.                    172800  IN      NS      d.gtld-servers.net.
com.                    172800  IN      NS      e.gtld-servers.net.
com.                    172800  IN      NS      f.gtld-servers.net.
com.                    172800  IN      NS      g.gtld-servers.net.
com.                    172800  IN      NS      h.gtld-servers.net.
com.                    172800  IN      NS      i.gtld-servers.net.
com.                    172800  IN      NS      j.gtld-servers.net.
com.                    172800  IN      NS      k.gtld-servers.net.
com.                    172800  IN      NS      l.gtld-servers.net.
com.                    172800  IN      NS      m.gtld-servers.net.
com.                    86400   IN      DS      19718 13 2 8ACBB0CD28F41250A80A491389424D341522D946B0DA0C0291F2D3D7 71D7805A
com.                    86400   IN      RRSIG   DS 8 1 86400 20260805050000 20260723040000 57780 . RCAEWHXqVrOOrICm4FH8TT+CnLgp5suhuG1lZG5Bf2WkuoilDjvXV80j gC8qS7Ems4H7hqMyxb7uT3rY8UMp9vH9a4fRtA+K2o8162yc3Rd8MXqX Fvr4+B1gPkYUK2Dql2DBcH6BiiVLQdMYj/I14KDDiU36z7nUXs7JN4lS am3lUG7yOUzenowC6vg+aUz5x7lp/hJAsn/5xuqIinfepxVCuixaB8Wg heWTRAFhljSu0uugPLWQzbWKXWUY1RPcCk/dRnNflXLY3zH2cKCGfxH4 v4FP4sHBmDzXjcT+KKWJknyA1rXvDpnFTpDj4trfrNcVv9CqEfBozGe1 we+15Q==
;; Received 1170 bytes from 192.203.230.10#53(e.root-servers.net) in 1 ms

;; UDP setup with 2001:501:b1f9::30#53(2001:501:b1f9::30) for google.com failed: network unreachable.
;; no servers could be reached
;; UDP setup with 2001:501:b1f9::30#53(2001:501:b1f9::30) for google.com failed: network unreachable.
;; no servers could be reached
;; UDP setup with 2001:501:b1f9::30#53(2001:501:b1f9::30) for google.com failed: network unreachable.
;; UDP setup with 2001:503:39c1::30#53(2001:503:39c1::30) for google.com failed: network unreachable.
;; UDP setup with 2001:500:d937::30#53(2001:500:d937::30) for google.com failed: network unreachable.
google.com.             172800  IN      NS      ns2.google.com.
google.com.             172800  IN      NS      ns1.google.com.
google.com.             172800  IN      NS      ns3.google.com.
google.com.             172800  IN      NS      ns4.google.com.
CK0POJMG874LJREF7EFN8430QVIT8BSM.com. 900 IN NSEC3 1 1 0 - CK0Q3UDG8CEKKAE7RUKPGCT1DVSSH8LL NS SOA RRSIG DNSKEY NSEC3PARAM
CK0POJMG874LJREF7EFN8430QVIT8BSM.com. 900 IN RRSIG NSEC3 13 2 900 20260729002619 20260721231619 41446 com. 3cqJDSCFGwu0pearZfCdSOAPoV3A1BfC4b9MGWebb1DeIIDbOLWTFL5h GCmw5NZpZr8Zfxr/10P5Uj228nllMw==
S84BOR4DK28HNHPLC218O483VOOOD5D8.com. 900 IN NSEC3 1 1 0 - S84BR9CIB2A20L3ETR1M2415ENPP99L8 NS DS RRSIG
S84BOR4DK28HNHPLC218O483VOOOD5D8.com. 900 IN RRSIG NSEC3 13 2 900 20260730012224 20260723001224 41446 com. llL8tQziPIA3S/2xSmOaqbc9YvFhS3sjDYXmB8dul6pWVyaEcg5+GXp1 04F3fvMBqero87Nkk+0bmhC5/z8sPQ==
;; Received 644 bytes from 192.41.162.30#53(l.gtld-servers.net) in 6 ms

;; UDP setup with 2001:4860:4802:32::a#53(2001:4860:4802:32::a) for google.com failed: network unreachable.
google.com.             300     IN      A       142.251.13.101
google.com.             300     IN      A       142.251.13.138
google.com.             300     IN      A       142.251.13.102
google.com.             300     IN      A       142.251.13.113
google.com.             300     IN      A       142.251.13.139
google.com.             300     IN      A       142.251.13.100
;; Received 135 bytes from 216.239.34.10#53(ns2.google.com) in 20 ms
