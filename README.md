# Two-office extended ACL | Project 9

Cork-PC can open the Dublin staff website even though it cannot ping the same server. This Packet Tracer lab shows how an extended IPv4 ACL filters a specific type of traffic between two hosts.

![Two-office topology with Dublin web server](images/topology.png)

## Design
| Device | Address | Role |
|---|---|---|
| Cork-PC | 192.168.20.11/24 | Client; gateway 192.168.20.1 |
| R2-CORK | G0/0: 192.168.20.1; G0/1: 10.0.0.2 | Filters packets entering from the Cork LAN |
| R1-dublin | G0/1: 10.0.0.1; G0/0: 192.168.10.1 | Routes to the Dublin LAN |
| DUBLIN-WEB | 192.168.10.20/24 | HTTP server; gateway 192.168.10.1 |

The routers use OSPF area 0 across 10.0.0.0/30. DUBLIN-WEB connects to the Dublin switch. Dublin's final configuration has no ACL 10 applied.

## Policy
ACL 100 on R2-CORK G0/0 inbound denies ICMP echo requests from Cork-PC to DUBLIN-WEB. Its second rule permits all remaining IP traffic, including HTTP. It does not restrict the client to HTTP only.

## Verify
Open [the lab](lab/Project-9-Extended-ACL.pkt). On Cork-PC, compare these tests against the same server:

| Test | Before ACL | After ACL |
|---|---|---|
| Command Prompt: ping 192.168.10.20 | 4/4 replies | 0/4 replies |
| Browser: http://192.168.10.20 | Page opened | Page opened |

On R2-CORK, use `show access-lists` to see which rule matched. The recorded deny count was four. These are results from the original lab session; the simulation was not independently rerun during packaging.

[Router configs and commands](configs/) · [Recorded results](tests/Results.txt) · [Combined evidence](images/Project-9-Network-Evidence.jpg)
