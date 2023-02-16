# Networking

- The Server Message Block protocol (SMB protocol) is a client-server communication protocol used for sharing access to files, printers, serial ports and other resources on a network. SMB relies on the TCP and IP protocols for transport. This combination potentially allows file sharing over complex, interconnected networks, including the public Internet. The SMB server component uses TCP port 445.(works on presentation layer)
- DHCP is an application layer protocol
- To get network interfaces on the host, use:

        $ip link
        # To get details about the ip that your pc uses
        $ip add
        # To reports interface statistics
        $ip -s link
        # To enable or disable an interface (These changes are not persistent and apply only to the running configuration, unless run as part of a startup script)
        $ip link set [inteface name] [up OR down]
        # To modify IP address config
        $ip [add OR delete]

- To sniff the packets on an interface:

        # To write the result to a file use => -w
        $tcpdump -i [interface name] -w [file name]
        # Intercepting traffic with tcpdump on other server:
        $sudo tcpdump -i eth0 -vv dst 10.1.16.12 and port http -w http.txt
        # Another example
        $sudo tcpdump -i eth0 -vv icmp -w ping.txt
        # To read the text file
        $sudo tcpdump -r http.txt

- To get the IP addr of a domain name:

        $nslookup [DN]
        # Or to get more details
        $dig [DN]

- APIPA Adress (Automatic Private IP Addressing): assigns a class B address from `169.254.0.0` to `169.254.255.255` to the client when DHCP server is either permanently or temporarly unavailable.
  -IGMP protocol is used in the multicas addressing to config group memebership & IP addresses.
- Anycast: Group of hosts with same IP, closest will be chosen.
- 0.0.0.0/8: commonly used by clients to request DHCP lease
- Nmap ("network mapper") is a very common and useful utility for discovering nodes on the network and scaning that node to discover open ports:

  $nmap [IP address]
  
- The Hypertext Transfer Protocol (HTTP) does not encrypt network traffic or confirm the identity of the destination web server, hence the importance of HTTPS connectivity for secure web browsing.
- Ping uses ICMP(layer 3), which does not operate at the Transport layer (where port numbers exist).
- In Windows to change the IP of a host statically:

        $netsh interface ip set address "Ethernet" static 10.1.254.2 255.255.255.0 10.1.254.254

- A router to reach a 10.1.16.0/16 subnet from a 10.1.254.0/24 subnet, but it cannot reach the router configured as its default gateway (unsurprisingly, as it doesn't exist). This can produce a mix of Request timed out errors (because ARP cannot identify the default gateway) and Destination unreachable errors (because there is no other route in the local routing table)

- Applying inconsistent subnet masks may or may not break communications between hosts and can sometimes be difficult to identify. Hosts in the same subnet should be configured with the same mask, ideally from an autoconfiguration service such as DHCP.

- Ping the IP address of the local host to verify it was added correctly and to verify that the network adapter is functioning properly. If you cannot ping your own address, there might have been a configuration error, or the network adapter or adapter driver could be faulty.

- To show ARP cache contents.


        $arp -a (or arp -g )
        # Adds an entry to the ARP cache.
        $arp -s IPAddress MACAddress
        # To delete all entries in the ARP cache
        $arp -d *
        
- In Linux, the ip neigh command shows entries in the local ARP cache (replacing the old arp command).

- If ping probes are unsuccessful, one of two messages are commonly received (add -c to send a specific number of ping packets):

        - Destination host unreachable-There is no routing information (that is, the local computer does not
          know how to get to that IP address). This might be caused by some sort of configuration error on the
          local host, such as an incorrect default gateway, by a loss of connectivity with a router,
          or by a routing configuration error.
        - No reply (Request timed out.)-The host is unavailable or cannot route a reply to your computer. Requests
          time out when the TTL is reduced to 0 because the packet is looping (because of a corrupted routing table),
          when congestion causes delays, or when a host does not respond.
          
- If Windows detects a duplicate IP address, it will display a warning and disable IP. Linux does not typically check for duplicate IP addresses. If there are two systems with duplicate IPs, a sort of race condition will determine which receives traffic.

-  On Linux, you can use the arping tool `arping -D` to report duplicate replies.

- To diagnose MAC address issues, use the arp utility to verify the MAC addresses recorded for each host and ipconfig or ip neigh to check the MAC address assigned to the interface. Also check the MAC address and ARP tables on any switches and routers involved in the communications path. You can use a protocol analyzer to examine ARP traffic and identify which IP hosts are attempting to claim the same MAC address.

- In Windows, you can view the DNS servers using ipconfig /all. In Linux, the DNS server addresses are recorded in /etc/resolv.conf

- Multicast groups are established at layer 3 by protocols such as Internet Group Management Protocol (IGMP). IGMP snooping means the switch reads IGMP messages and can determine if the host on an access port or one or more hosts in a VLAN have joined a multicast group. Multicast traffic is filtered from ports and VLANs that have no hosts participating in the multicast group.

- In IPv6 The first 3 bits (001) indicate that the address is within the global scope.The next 45 bits are allocated in a hierarchical manner to regional registries and from them to ISPs and end users. The next 16 bits identify site-specific subnet addresses. The final 64 bits are the interface ID.

- In IPv6 Link local addresses span a single subnet (they are not forwarded by routers). In IPv6, an interface must always be configured with a link local address. One or more routable addresses can be assigned to the interface in addition to the link local address.

- The Neighbor Discovery (ND) protocol performs some of the functions on an IPv6 network that ARP and ICMP perform under IPv4. The main functions of ND are:
1. Address autoconfiguration-Enables a host to configure IPv6 addresses for its interfaces automatically and detect whether an address is already in use on the local network, by using neighbor solicitation (NS) and neighbor advertisement (NA) messages.
2. Prefix discovery-Enables a host to discover the known network prefixes that have been allocated to the local segment. This facilitates next-hop determination (whether a packet should be addressed to a local host or a router). Prefix discovery uses router solicitation (RS) and router advertisement (RA) messages. 
3. Local address resolution-Allows a host to discover other nodes and routers on the local network (neighbors). This process also uses neighbor solicitation (NS) and neighbor advertisement (NA) messages.
4. Redirection-Enables a router to inform a host of a better route to a particular destination.

- stateless address autoconfiguration (SLAAC)(IPv6) == DHCP (IPv4)

- Error messaging-ICMPv6 supports the same sort of destination unreachable and time exceeded messaging as ICMPv4. One change is the introduction of a Packet Too Big class of error. Under IPv6, routers are no longer responsible for packet fragmentation and reassembly, so the host must ensure that they fit in the MTUs of the various links used.

-  Tunneling can be used to deliver IPv6 packets across an IPv4 network. Tunneling means that IPv6 packets are inserted into IPv4 packets and routed over the IPv4 network to their destination. Routing decisions are based on the IPv4 address until the packets approach their destinations, at which point the IPv6 packets are stripped from their IPv4 carrier packets and forwarded according to IPv6 routing rules. This carries a high protocol overhead and is not nearly as efficient as operating dual stack hosts.

-  Generic Routing Encapsulation (GRE). GRE allows a wide variety of Network layer protocols to be encapsulated inside virtual point-to-point links. This protocol has the advantage that because it was originally designed for IPv4, it is considered a mature mechanism and can carry both v4 and v6 packets over an IPv4 network.

- Service (srv) records are essential for clients to be able to join a domain and authenticate with Active Directory.

-  Note that the switches do not count as hops

- Distance vector: Algorithm used by routing protocols that select a forwarding path based on the next hop router with the lowest hop count to the destination network.

-  Link state: Algorithm used by routing protocols that build a complete network topology to use to select optimum forwarding paths.

- A distance vector algorithm relies on directly connected neighbors for information about remote networks. By contrast, a link state algorithm allows a router to store the complete network topology and assess the least-cost paths from this topology database.

- Dynamic Routing: Entry in the routing table that has been learned from another router via a dynamic routing protocol.

- Convergence: Process whereby routers agree on routes through the network to establish the same network topology in their routing tables (steady state). The time taken to reach steady state is a measure of a routing protocol’s convergence performance.

- A network under the administrative control of a single owner is referred to as an autonomous system (AS). An Interior Gateway Protocol (IGP) is one that identifies routes within an AS.

- An Exterior Gateway Protocol (EGP) is one that can advertise routes between autonomous systems. An EGP includes a field to communicate the network's autonomous system ID and allows network owners to determine whether they can use paths through another organization's network.

- The Routing Information Protocol (RIP) is a distance vector routing protocol & IGP uses UDP on port 520 or 521. RIP only considers a single piece of information about the network topology-the next hop router to reach a given network or subnet (vector). It considers only one metric to select the optimal path to a given destination network-the one with the lowest hop count (distance). RIP sends regular updates (typically every 30 seconds) of its entire routing database to neighboring routers.

- RIPng (next generation) is a version of the protocol designed for IPv6. RIPng uses UDP port 521.

- Distance vector algorithms provide for slower convergence than link state algorithms. For more complex networks with redundant paths, other dynamic routing protocols should be considered.

- Enhanced IGRP (EIGRP)(There are versions for IPv4 and IPv6): is usually classed as an advanced distance vector or hybrid routing protocol. Like RIP, EIGRP is a distance vector protocol because it relies on neighboring routers to report paths to remote networks. Unlike RIP, which is based on a simple hop count metric, EIGRP uses a metric composed of administrator weighted elements. The two default elements are bandwidth and delay. Where RIP sends periodic updates of its entire routing information base, EIGRP sends a full update when it first establishes contact with a neighbor and thereafter only sends updates when there is a topology change.Unlike RIP, EIGRP is a native IP protocol, which means that it is encapsulated directly in IP datagrams, rather than using TCP or UDP. It is tagged with the protocol number 88 in the Protocol field of the IP header. Updates are transmitted using multicast addressing.

- Open Shortest Path First (OSPF)(IGP) is the most widely adopted link state protocol. It is suited to large organizations with multiple redundant paths between networks. It has better convergence performance than RIP.

-  Border Gateway Protocol (BGP) is designed to be used between routing domains in a mesh internetwork and as such is used as the routing protocol on the Internet, primarily between ISPs.

- Autonomous system numbers (ASN) are allocated to ISPs by IANA via the various regional registries. When BGP is used within an AS, it is referred to as Interior BGP (IBGP), and when implemented between autonomous systems, it is referred to as Exterior BGP (EBGP). BGP works with classless network prefixes called Network Layer Reachability Information (NLRI). BGP works over TCP on port 179.

-  Administrative distance (AD): Metric determining the trustworthiness of routes derived from different routing protocols. Routers prefer the learned route because it has a lower AD.

-  Variable length subnet masking (VLSM): Using network prefixes of different lengths within an IP network to create subnets of different sizes. Without VLSM, you have to allocate subnetted ranges of addresses that are the same size and use the same subnet mask throughout the network. VLSM allows different length netmasks to be used within the same IP network, allowing more flexibility in the design process.
