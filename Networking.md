$nslookup [DN] [DNS example 8.8.8.8]# Networking

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

        $nslookup [DN] [DNS example 8.8.8.8]
        # Or to get more details
        $dig [DN]
        $dig @[DN]

- APIPA Adress (Automatic Private IP Addressing): assigns a class B address from `169.254.0.0` to `169.254.255.255` to the client when DHCP server is either permanently or temporarly unavailable.
  -IGMP protocol is used in the multicas addressing to config group memebership & IP addresses.
- Anycast: Group of hosts with same IP, closest will be chosen.
- 0.0.0.0/8: commonly used by clients to request DHCP lease
- Nmap ("network mapper") is a very common and useful utility for discovering nodes on the network and scaning that node to discover open ports:

          $nmap [IP address or IP addr range]
          # To conduct a simple network scan by using nmap:
          $nmap -sn 10.1.16.0/24
          # To get he OS of he scanned host use -O
          #Run the following command to conduct an extensive nmap scan of the LAMP server:
          $nmap -A 10.1.16.12
  
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

- Routers serve both to link physically remote networks and subdivide autonomous IP networks into multiple subnets. Router placement is primarily driven by the IP networks and subnets that have been created; Hosts with addresses in the same subnet or IP network must not be separated by a router.
Conversely, hosts with addresses in different subnets or IP networks must be separated by a router. Edge routers designed to work with DSL or cable broadband access methods are called small office/home office (SOHO) routers.

- An internal router has no public interfaces.

- Configuring a router's physical interface with multiple virtual interfaces connected to separate virtual LAN (VLAN) IDs over a trunk.

- Layer 3 capable switch: Switch appliance capable of IP routing between virtual LAN (VLAN) subnets using hardware-optimized path selection and forwarding.

-  SSH can be used to communicate with the router via the IP address of any configured interface.

- The route command is used to view and modify the routing table of end system Windows and Linux hosts. A nonpersistent route can be added using the following general syntax:

        $route add -net 192.168.3.0 netmask 255.255.255.0 metric 2 dev eth0
        
- `traceroute` is supported on Linux and router OSes (such as Cisco IOS). traceroute uses UDP probe messages by default. The command issues a UDP probe for port 32767 with a TTL of 1. The first hop should reduce this to zero and respond with an ICMP Time Exceeded message. The command then increments the TTL by one and sends a second probe, which should reach the second hop router. This process is repeated until the end node is reached, which should reply with an ICMP Port Unreachable response.
**The output shows the number of hops, the IP address of the ingress interface of the router or host (that is, the interface from which the router receives the probe), and the time taken to respond to each probe in milliseconds (ms). If no acknowledgment is received within the timeout period, an asterisk is shown against the probe. Note that while this could indicate that the router interface is not responding, it could also be that the router is configured to drop packets with expired TTLs silently.** traceroute can be configured to send ICMP Echo Request probes rather than UDP by using `traceroute -I` . The `traceroute -6` or `traceroute6` commands are used for IPv6 networks. On a Windows system, the same function is performed using the tracert command. `tracert` uses ICMP Echo Request probes by default. You can use the -d switch to suppress name resolution, -h to specify the maximum number of hops (the default is 30), and -w to specify a timeout in ms (the default is 4000).

- If you suspect a problem with router configuration and the network topology, use traceroute to try to identify where the network path is failing and the route or show route commands to investigate the routing tables of intermediate systems at that point in the path.

- missing route: Troubleshooting issue where a routing table does not contain a required entry due either to manual misconfiguration or failure of a dynamic routing protocol update.

- A routing loop occurs when two routers use one another as the path to a network. Packets caught in a routing loop circle around until the TTL expires. One symptom of a potential routing loop is for routers to generate ICMP Time Exceeded error messages.You can use traceroute to diagnose a routing loop by looking for IP addresses that appear multiple times in the output.

- Asymmetrical routing Topology where the return path is different to the forward path, Asymmetric routing is problematic where the return path is much higher latency than the forward path or where the difference between the paths causes stateful firewall or network address translation (NAT) devices to filter or drop communications. You should use traceroute from both sender and receiver to compare the per-hop latency to identify where the routing topology is misconfigured.

-  point-to-point: A point-to-point topology is one where two nodes have a dedicated connection to one another.

-  star topology: In a star network, each node is connected to a central point, typically a switch or a router. The central point mediates communications between the attached nodes. When a device such as a hub is used, the hub receives signals from a node and repeats the signal to all other connected nodes. Therefore the bandwidth is still shared between all nodes. When a device such as a switch is used, point-to-point links are established between each node as required. The circuit established between the two nodes can use the full bandwidth capacity of the network media.

- A topology often used in WANs where each device has (in theory) a point-to-point connection with every other device (fully connected); in practice, only the more important devices are directly interconnected (partial mesh).

- In a ring topology, all of the computers are connected in a circle. The ring comprises a series of point-to-point links between each device. Signals pass from device to device in a single direction with the signal regenerated at each device. The physical ring topology is no longer used on LANs, but it does remain a feature of many WANs.

- Bus topology: a shared access media where all nodes attach directly to a single cable segment.

- A network that uses a combination of physical or logical topologies. In practice most networks use hybrid topologies. For example, modern types of Ethernet are physically wired as stars but logically operate as buses. Nodes within the same logical bus segment are in the same collision domain. When Ethernet is deployed with a legacy hub appliance, this can be described as a physical star-logical bus topology.

- Ethernet has no concept of time to live, so layer 2 broadcast traffic could continue to loop through a network with multiple paths indefinitely.

- The spanning tree protocol (STP)(Protocol that prevents layer 2 network loops by dynamically blocking switch ports as needed.) is a means for the bridges or switches to organize themselves into a hierarchy. The switch at the top of the hierarchy is the root. The switch with the lowest ID, comprising a priority value and the MAC address, will be selected as the root.

-  switching loop: Troubleshooting issue where layer 2 frames are forwarded between switches or bridges in an endless loop.

-  broadcast storm: Traffic that is recirculated and amplified by loops in a switching topology, causing network slowdowns and crashing switches.

- Normally, the network would be designed with a 1:1 mapping between VLANs and subnets.Implementing VLANs can reduce broadcast traffic

-  trunks: Backbone link established between switches and routers to transport frames for multiple virtual LANs (VLANs).

- A packet addressed to the loopback address can be received on any available hardware interface.

- The port number is used in conjunction with the source IP address to form a socket where it's a combination of a TCP/UDP port number and IP address. A client socket can form a connection with a server socket to exchange data. Each socket is bound to a software process. Only one process can operate a socket at any one time.

- Transmission Control Protocol (TCP) works at the Transport layer to provide connection-oriented, guaranteed communication using acknowledgements to ensure that delivery has occurred. If packets are missing, they can be retransmitted. TCP can be used for unicast transmission only. CP takes data from the Application layer as a stream of bytes and divides it up into segments, each of which is given a header. The TCP segments become the payload of the underlying IP datagrams. The use of sequencing, acknowledgments, and retransmissions means that TCP requires numerous header fields to maintain state information. The main fields in the header of a TCP segment are: Source port, Destination port, Sequence number, Sequence number (A Negative Acknowledgement (NAK or NACK) forces retransmission.), Window (The amount of data the host is willing to receive before sending another acknowledgement.), etc.

- Servers can (usually) support thousands or even millions of TCP connections simultaneously.

- A host can also end a session abruptly using a reset (RST) segment. This would not be typical behavior and might need to be investigated. A server or security appliance might refuse connections using RST, a client or server application might be faulty, or there could be some sort of suspicious scanning activity ongoing.                                  
- UDP header size is 8 bytes, compared to 20 bytes (or more) for TCP.

-  IP scanner: Utility that can probe a network to detect which IP addresses are in use by hosts. IP scanning can be performed using lightweight standalone open source or commercial tools, such as Nmap, AngryIP, or PRTG. Windows Server is bundled with a DDI product. Other notable vendors and solutions include ManageEngine, Infoblox, SolarWinds, Bluecat, and Men -and- Mice.

-  Nmap Security Scanner: A highly adaptable, open-source network scanner used primarily to scan hosts and ports to locate services and detect vulnerabilities.

- The netstat command allows you to check the state of ports on the local host. You can use netstat to check for service misconfigurations, such as a host running a web or FTP server that a user installed without authorization. You may also be able to identify suspicious remote connections to services on the local host or from the host to remote IP addresses.On Linux, running netstat without switches shows active connections of any type. If you want to show different connection types, you can use the switches for Internet connections for TCP ( -t ) and UDP ( -u ), raw connections ( -w ), and UNIX® sockets/local server ports ( -x ). Using the -a switch includes ports in the listening state in the output. -l shows only ports in the listening state, omitting established connections. For example, the following command shows listening and established Internet connections (TCP and UDP) only: netstat -tua. Another common task is to identify which software process is bound to a socket. On Windows, -o shows the Process ID (PID) number that has opened the port, while -b shows the process name. In Linux, use -p to show the PID and process name. netstat can also be set to run continuously. In Windows, run netstat nn , where nn is the refresh interval in seconds (press Ctrl+C to stop); in Linux, run netstat -c .

- Many of the tools used for host discovery can also perform remote port scanning. As with host discovery, there are many different techniques for performing port scans. Some techniques are designed for covert use (to try to avoid detection of the scanning activity by the target) and some are designed to probe beyond security barriers, such as firewalls.

- As examples, the following represent some of the main types of scanning that Nmap can perform:


        1. TCP SYN (-sS)-This is a fast technique (also referred to as half-open scanning) as the scanning host requests a connection without acknowledging it. The target's response to the scan's SYN packet identifies the port state.
        2. TCP connect (-sT)-A half-open scan requires Nmap to have privileged access to the network driver so that it can craft packets. If privileged access is not available, Nmap must use the OS to attempt a full TCP connection. This type of scan is less stealthy.
        3. UDP scans (-sU)-Scan UDP ports. As these do not use ACKs, Nmap needs to wait for a response or timeout to determine the port state, so UDP scanning can take a long time. A UDP scan can be combined with a TCP scan.
        4. Port range (-p)-By default, Nmap scans 1,000 commonly used ports. Use the -p argument to specify a port range. You can also use --top-ports n , where n is the number of commonly used ports to scan. The frequency statistics for determining how commonly a port is used are stored in the nmap-services configuration file.

When services are discovered, you can use Nmap with the -sV or -A switch to probe a host more intensively to discover the software or software version operating each port. The process of identifying an OS or software application from its responses to probes is called fingerprinting.

-  Protocol analyzer: Utility that can parse the header fields and payloads of protocols in captured frames for display and analysis. One function of a protocol analyzer is to parse each frame in a stream of traffic to reveal its header fields and payload contents in a readable format. This is referred to as packet analysis. Analyzing protocol data at the frame or packet level will help to identify protocol or service misconfigurations. As a live stream or capture file can contain hundreds or thousands of frames, you can use display filters to show only particular frame or sequence of frames. Another useful option is to use the Follow TCP Stream context command to reconstruct the packet contents for a TCP session.
Another function of a protocol analyzer is to perform traffic analysis. Rather than reading each frame individually, you use the tool to monitor statistics related to communications flows, such as bandwidth consumed by each protocol or each host, identifying the most active network hosts, monitoring link utilization and reliability, and so on. In Wireshark, you can use the Statistics menu to access traffic analysis tools.

- Sometimes, the DHCP lease process is called the DORA process: Discover, Offer, Request, and Ack(nowledge).

- Scope: Range of consecutive IP addresses in the same subnet that a DHCP server can lease to clients.

- A Windows client can be forced to release a lease by issuing a command such as ipconfig. In Linux, the utility dhclient is often used for this task, though modern distributions might use NetworkManager or systemd-networkd. A Windows host that fails to obtain a lease will revert to an automatic IP address (APIPA) or link-local configuration and select an address in the 169.254.0.0/16 range. Linux might use link-local addressing, set the address to unknown (0.0.0.0), or leave the interface unconfigured. 

- DHCP Options
When the DHCP server offers a configuration to a client, at a minimum it must supply an IP address and subnet mask. Typically, it will also supply other IP-related settings, known as DHCP options. Each option is identified by a tag byte or decimal value between 0 and 255 (though neither 0 nor 255 can be used as option values). Some widely used options include:

        The default gateway (IP address of the router).
        The IP address(es) of DNS servers that can act as resolvers for name queries.
        The DNS suffix (domain name) to be used by the client.
        Other useful server options, such as time synchronization (NTP), file transfer (TFTP), or VoIP proxy.

- A reservation is a mapping of a MAC address or interface ID to a specific IP address within the DHCP server's address pool.

-  IP helper: Command set in a router OS to support DHCP relay and other broadcast forwarding functionality.

- IPv6's Stateless Address Autoconfiguration (SLAAC) process can locate routers (default gateways) and generate a host address with a suitable network prefix automatically. In this context, the role of a DHCP server in IPv6 is different. DHCPv6 is often just used to provide additional option settings, rather than leases for host IP addresses. The format of messages is different, but the process of DHCP server discovery and address leasing (if offered) is fundamentally the same. As IPv6 does not support broadcast, clients use the multicast address ff02::1:2 to discover a DHCP server. DHCPv6 uses ports 546 (clients) and 547 (servers), rather than ports 68 and 67 as in DHCPv4.

-  An example of an FQDN might be nut.widget.example. An FQDN is made up of the host name and a domain suffix. In the example, the host name is nut and the domain suffix is widget.example. This domain suffix consists of the domain name widget within the top-level domain (TLD) .example . A domain suffix could also contain subdomains between the host and domain name. The trailing dot or period represents the root of the hierarchy. 

        The total length of an FQDN cannot exceed 253 characters, with each label
        (part of the name defined by a period) no more than 63 characters (excluding the periods).
        A DNS label should use letter, digit, and hyphen characters only. 
        A label should not start with a hyphen. Punctuation characters such as the period (.) or forward slash (/) should not be used.
        DNS labels are not case-sensitive.

- Iterative lookups: DNS query type whereby a server responds with information from its own data store only.

-  Recursive lookup: DNS query type whereby a server submits additional queries to other servers to obtain the requested information.

- Resource records: Data file storing information about a DNS zone. The main records are as follows: A (maps a host name to an IPv4 address), AAAA (maps to an IPv6 address), CNAME (an alias for a host name), MX (the IP address of a mail server), and PTR (allows a host name to be identified from an IP address).

- Authoritative name server: DNS server designated by a name server record for the domain that holds a complete copy of zone records.

- Authoritative name server: DNS server designated by a name server record for the domain that holds a complete copy of zone records.

- A Canonical Name (CNAME) (or alias) record is used to configure an alias for an existing address record (A or AAAA). For example, the IP address of a web server with the host record lamp could also be resolved by the alias www . CNAME records are also often used to make DNS administration easier. For example, an alias can be redirected to a completely different host temporarily during system maintenance. here is nothing to stop an administrator configuring multiple address records to point different host names to the same IP address. Using CNAME records is usually considered better practice, however. It is also possible to configure multiple A or AAAA records with the same host name but different IP addresses. This is usually done as a basic load balancing technique, referred to as round robin DNS.

-  Service (SRV) record contains the service name and port on which a particular application is hosted. SRV records are often used to locate VoIP or media servers. SRV records are also an essential part of the infrastructure supporting Microsoft’s Active Directory; they are used by clients to locate domain controllers, for instance. As with MX, SRV records can be configured with a priority value.

- DomainKeys Identified Mail (DKIM): records are used to decide whether you should allow received email from a given source, preventing spam and mail spoofing. DKIM can use encrypted signatures to prove that a message really originated from the domain it claims.

- A reverse DNS query returns the host name associated with a given IP address. This information is stored in a reverse lookup zone as a pointer (PTR) record.

- Reverse lookup zones are not mandatory and are often omitted from DNS servers, as they can be used by hackers to sequentially work through a range of IP addresses to discover useful or interesting device names, which can then be targeted by other hacking mechanisms.

- A DNS server is usually configured to listen for queries on UDP port 53. Some DNS servers are also configured to allow connections over TCP port 53, as this allows larger record transfers (over 512 bytes). Larger transfers might be required if IPv6 is deployed on the network or if the DNS servers are using a security protocol (DNSSEC).

- To get the FQDN of your server use:

        $hostname --fqdn


- The Windows PowerShell environment provides a more sophisticated scripted environment that you can use to issue cmdlets to test DNS name resolution (and change DNS settings as well, if required). PowerShell provides a cmdlet called Resolve-DnsName, which allows a more flexible method of testing name resolution than nslookup, as it allows testing of the different methods of name resolution (HOSTS file, DNS cache, and DNS server).

- HTTP Headers and Payload: The response and request formats are defined in the HTTP header. The HTTP payload is usually used to serve HyperText Markup Language (HTML) web pages, HTTP also features a forms mechanism (POST) that enables a user to submit data from the client to the server. HTTP is nominally a stateless protocol; this means that the server is not required to preserve information about the client during a session.

- The main web server platforms are Apache®, Microsoft Internet Information Server (IIS), and nginx.

- SSL/TLS works as a layer between the Application and Transport layers of the TCP/IP stack, or, in OSI terms, at the Session layer. It's normally used to encrypt TCP connections. When it is used with the HTTP application, it is referred to as HTTP Secure (HTTPS). TLS can also be used to secure other TCP application protocols, such as FTP, POP3/IMAP, SMTP, and LDAP.

- TLS can also be used with UDP, referred to as Datagram Transport Layer Security (DTLS), most often in virtual private networking (VPN) solutions.

- Encrypted traffic between the client and server is sent over TCP port 443 (by default), rather than the open and unencrypted port 80. 

- An FTP client connects to TCP port 21 on an FTP server and opens a chosen dynamic client port number (n). The TCP port 21 control port is used to transfer commands and status information, but not for data transfer. Data transfer can operate in one of two modes: active or passive. In active mode, the client sends a PORT command specifying its chosen data connection port number (typically n+1), and the server opens the data connection between the chosen client port and TCP port 20 on the server. In passive mode, the client opens a data port (again, typically n+1) and sends the PASV command to the server's control port. The server then opens a random high port number and sends it to the client using the PORT command. The client then initiates the connection between the two ports. Active FTP poses a configuration problem for some firewalls, as the server is initiating the inbound connection, but there is no way of predicting which port number will be utilized. However, not all FTP servers and clients can operate in passive mode.

- Secure FTP (SFTP): A secure version of the File Transfer Protocol that uses a Secure Shell (SSH) tunnel as an encryption method to transfer, access, and manage files.

- Explicit TLS (FTPES)-Use the AUTH TLS command to upgrade an unsecure connection established over port 21 to a secure one. This protects authentication credentials. The data connection for the actual file transfers can also be encrypted (using the PROT command).

- Implicit TLS (FTPS)-Negotiate an SSL/TLS tunnel before the exchange of any FTP commands. This mode uses the secure port 990 for the control connection. FTPS is tricky to configure when there are firewalls between the client and server. Consequently, FTPES is usually the preferred method.

- On a Windows network, the File/Print Sharing Service is provided by the Server Message Block (SMB) protocol. SMB allows a host to share its directories/files and printers to make them available for other machines to use. Support for SMB in UNIX- or Linux-based machines and network attached storage (NAS) appliances is provided by using the Samba software suite, which allows a Windows client to access a Linux host as though it were a Windows file or print server. SMB version 3 supports message encryption, which can be enabled on a file server or on a per-share basis. An encrypted share can only be accessed by an SMB 3.0 or higher client. SMB has gone through several updates, with SMB3 as the current version. SMB1 has very serious security vulnerabilities and is now disabled by default on current Windows versions 

- By default, the relational dabases ports are unsecure. However, the RDBMS server can be installed with a certificate and configured to enable TLS transport encryption. NoSQL databases are typically accessed using an application programming interface (API) over HTTPS.

-  Simple Mail Transfer Protocol (SMTP): Application protocol used to send mail between hosts on the Internet. Messages are sent between servers over TCP port 25 or submitted by a mail client over secure port TCP/587. SMTP is useful only to deliver mail to hosts that are permanently available. SMTP communications can be secured using the TLS version of the protocol (SMTPS). There are two ways for SMTP to use TLS:

        STARTTLS-This is a command that upgrades an existing unsecure connection to use TLS. This is also referred to as explicit TLS or opportunistic TLS.
        SMTPS-This establishes the secure connection before any SMTP commands (HELO, for instance) are exchanged. This is also referred to as implicit TLS.

- Mail clients can use port 25 to submit messages to the server for delivery, but this is not best practice. Use of port 25 is typically reserved for relay between servers.

- Post Office Protocol (POP): Application protocol that enables a client to download email messages from a server mailbox to a client over port TCP/110 or secure port TCP/995.  A POP client application, such as Microsoft Outlook® or Mozilla Thunderbird®, establishes a connection to the POP server on TCP port 110. The user is authenticated (by username and password), and the contents of his or her mailbox are downloaded for processing on the local PC. Generally speaking, the messages are deleted from the mailbox server when they are downloaded, though some clients have the option to leave messages on the server. POP can be secured by using TLS encryption. The default TCP port for secure POP (POP3S) is port 995.

-  Internet Message Access Protocol (IMAP): Application protocol providing a means for a client to access and manage email messages stored in a mailbox on a remote server. IMAP4 utilizes TCP port number 143, while the secure version IMAPS uses TCP/993. IMAP supports permanent connections to a server and connecting multiple clients to the same mailbox simultaneously. It also allows a client to manage the mailbox on the server (to organize messages in folders and to control when they are deleted, for instance) and to create multiple mailboxes.

- In a Windows environment, the proprietary Messaging Application Programming Interface (MAPI) protocol is typically used to access Microsoft Exchange mailboxes. MAPI uses HTTPS as a secure transport protocol.

- Voice over IP (VoIP): Generic name for protocols that carry voice traffic over data networks.

- Telnet: Application protocol supporting unsecure terminal emulation for remote host management. Telnet runs over TCP port 23.

-   Remote Desktop Protocol (RDP) is Microsoft's protocol for operating remote GUI connections to a Windows machine. RDP uses TCP port 3389. The administrator can specify permissions to connect to the server via RDP and can configure encryption on the connection. RDP is mainly used for the remote administration of a Windows server or client, but another function is to publish software applications on a server, rather than installing them locally on each client (application virtualization). RDP clients are available for other operating systems, including Linux, macOS, iOS, and Android so you can connect to a Windows desktop remotely using a non-Windows device. There are also open-source RDP server products, such as xrdp

- Network Time Protocol (NTP) enables the synchronization of these time-dependent applications. NTP works over UDP on port 123. Client hosts (application servers and workstations) usually obtain the time by using a modified form of the protocol called Simple NTP (SNTP). SNTP works over the same port as NTP. A host that supports only SNTP cannot act as a time source for other hosts. In Windows, the Time Service can be configured by using the w32tm command. In Linux, the ntp package can be configured via /etc/ntp.conf. If a local stratum 1 server cannot be implemented on the local network, the time source can be configured using one or more public NTP server pools, such as time.google.com, time.windows.com, time.apple.com, time.nist.gov, or pool.ntp.org.

- Simple Network Management Protocol (SNMP): Application protocol used for monitoring and managing network devices. SNMP works over UDP ports 161 and 162 by default.

- Syslog: Application protocol and event logging format enabling different appliances and software applications to transmit logs or event records to a central server. Syslog works over UDP port 514 by default.

- Monitoring is aligned with incident response; analysis is aligned with investigating the cause of incidents or preventing incidents in the first place.

-  Latency: The time it takes for a signal to reach the recipient. A video application can support a latency of about 80 ms, while typical latency on the Internet can reach 1000 ms at peak times. Latency is a particular problem for 2-way applications, such as VoIP (telephone) and online conferencing.

-  Jitter: Variation in the time it takes for a signal to reach the recipient. Jitter manifests itself as an inconsistent rate of packet delivery. If packet loss or delay is excessive, then noticeable audio or video problems (artifacts) are experienced by users. You can test the latency of a link using tools such as ping, pathping, and mtr. You can also use mtr to calculate jitter. When assessing latency, you need to consider the Round Trip Time (RTT). VoIP is generally expected to require an RTT of less than 300 ms. Jitter should be 30 ms or less. The link should also not exhibit more than 1 percent packet loss.

-  Differentiated Services (DiffServ): Header field used to indicate a priority value for a layer 3 (IP) packet to facilitate Quality of Service (QoS) or Class of Service (CoS) scheduling.

- NetFlow: Cisco-developed means of reporting network flow information to a structured database. NetFlow allows better understanding of IP traffic flows as used by different network applications and hosts.

- `show ip rip` shows the routes learned by RIP

- To extend the number of available ports might a network administrator interconnect managed switches

- Routing Information Protocol (RIP) has a default administrative distance (AD) value of 120. In AD, lower values are preferred, so RIP is less trusted than other protocols.

- To get MAC addresses of a network use the folloeing command:

        $sudo netdiscover -r [IP/mask]

- You can display SSH and web server information by conducting banner grabbing (the purpose of a "banner grab" connection is to detect software name and version information). Run the following command to conduct a banner grab by using netcat: 

        $nc -vn 10.1.16.12 22

-  The two DNS resource record types NS records contain the IP address of a DNS server that can resolve names and IP addresses. SOA records contain domain contact and responsibility information

- IMAP and POP3 are client email retrieval protocols, SMTP is primarily an email transfer protocol

- Common Vulnerabilities and Exposures (CVE) is a dictionary of vulnerabilities in published operating systems and applications software ( cve.mitre.org ). Automated vulnerability scanning software makes use of this dictionary to develop tests to discover vulnerabilities on live systems.

- Security Information and Event Management (SIEM): Solution that provides real-time or near-real-time analysis of security alerts generated by network hardware and applications.

- The key difference from passive vulnerability scanning is that an attempt is made to actively test security controls and exploit any vulnerabilities discovered. Pen testing is an intrusive assessment technique. For example, a vulnerability scan may reveal that an SQL Server has not been patched to safeguard against a known exploit. A penetration test would attempt to use the exploit to perform code injection and compromise the server.

- Privileged access management (PAM) refers to policies, procedures, and technical controls to prevent the malicious abuse of privileged accounts by internal threat actors and to mitigate risks from weak configuration control over privileges. These controls identify and document privileged accounts, giving visibility into their use, and manage the credentials used to access them.

- Vendor management is a process for selecting supplier companies and evaluating the risks inherent in relying on a third-party product or service.

- An automated scanner is configured with a list of known vulnerabilities to scan for. By definition, zero-day vulnerabilities are unknown to the vendor or to security practitioners. A zero-day is detected either through detailed manual research or because an exploit is discovered. Therefore, automated scanner can't catch zero-day vulnerabilities.

- The type of data used to create a credential is called an authentication factor. Authentication factors fall into the following categories:

Knowledge factor-something you know (such as a password).
Ownership factor-something you have (such as a smart card).
Human or biometric factor-something you are (such as a fingerprint).
Behavioral factor-something you do (such as making a signature).
Location factor-somewhere you are (such as using a mobile device with location services).

- Local Security Authority (LSA) compares the submitted credential to a hash stored in the Security Accounts Manager (SAM) database, which is part of the registry. This is also referred to as interactive logon. the LSA can pass the credentials for authentication to a network service. The preferred system for network authentication is based on Kerberos, but legacy network applications might use NT LAN Manager (NTLM) authentication.

- In Linux, local user account names are stored in /etc/passwd. When a user logs in to a local interactive shell, the password is checked against a hash stored in /etc/shadow. Interactive login over a network is typically accomplished using Secure Shell (SSH). With SSH, the user can be authenticated using cryptographic keys instead of a password.

- A pluggable authentication module (PAM) is a package for enabling different authentication providers, such as smart card login (tecmint.com/configure-pam-in-centos-ubuntu-linux). The PAM framework can also be used to implement authentication to network servers.

-  Single sign-on (SSO) system allows the user to authenticate once to a local device and be authorized to access compatible application servers without having to enter credentials again. In Windows, SSO is provided by the Kerberos framework.

- Kerberos: Single sign-on authentication and authorization service that is based on a time-sensitive ticket-granting system. Key Distribution Center (KDC) authenticate the user then give them the granting ticket.

- As encryption using a public key is relatively slow; rather than encrypting the whole message using a public key, more typically, the public key is used to encrypt a symmetric encryption key for use in a single session and exchange it securely. The symmetric session key is then used to encrypt the actual message. A symmetric key can perform both encryption and decryption.

- When you want to authenticate yourself to others, you create a signature and sign it by encrypting the signature with your private key. You give others your public key to use to decrypt the signature. As only you know the private key, everyone can be assured that only you could have created the signature.

- The basic problem with public key cryptography lies in proving the identity of the owner of a public key. The system is vulnerable to an on-path attack where a threat actor substitutes your public key for their own. Public key infrastructure (PKI) aims to prove that the owners of public keys are who they say they are.

- Extensible Authentication Protocol (EAP): Framework for negotiating authentication methods that enables systems to use hardware-based identifiers, such as fingerprint scanners or smart card readers, for authentication, and establish secure tunnels through which to submit credentials.

- RADIUS typically uses UDP ports 1812 and 1813. Each RADIUS client must be configured with the IP address of the RADIUS server plus the same shared secret.

- TACACS+ is often used in authenticating administrative access to routers and switches. It uses TCP over port 49 and the reliable delivery offered by TCP makes it easier to detect when a server is down.

- Your network monitor is recording high numbers of ICMP Time Exceeded notifications. That indicates a routing loop, where packets circulate between two routers until the time to live (TTL) is exceeded.

- When the Firebox uses RADIUS for an authentication attempt:

         The user tries to authenticate, either through a browser-based HTTPS connection to the device over port 4100,
        or through a connection using Mobile VPN with IPSec. The device reads the user name and password. The device creates
        a message called an Access-Request message and sends it to the RADIUS server. The device uses the RADIUS shared secret
        in the message. The password is always encrypted in the Access-Request message. The RADIUS server makes sure that the
        Access-Request message is from a known client (the Firebox). If the RADIUS server is not configured to accept the device
        as a client, the server discards the Access-Request message and does not send a message back.

        If the device is a client known to the RADIUS server and the shared secret is correct, the server looks at the authentication
        method requested in the Access-Request message. If the Access-Request message uses an allowed authentication method, the RADIUS
        server gets the user credentials from the message and looks for a match in a user database. If the user name and password match
        an entry in the database, the RADIUS server can get additional information about the user from the user database (such as remote
        access approval, group membership, logon hours, and so on).The RADIUS server checks to see whether it has an access policy or a
        profile in its configuration that matches all the information it has about the user. If such a policy exists, the server sends 
        a response.

        If any of the previous conditions fail, or if the RADIUS server has no matching policy, it sends an Access-Reject message 
        that shows authentication failure. The RADIUS transaction ends and the user is denied access. If the Access-Request message
        meets all the previousconditions, RADIUS sends an Access-Accept message to the device. The RADIUS server uses the shared 
        secret for any response it sends. If the shared secret does not match, the device rejects the RADIUS response. To see 
        diagnostic log messages for authentication, Set the Diagnostic Log Level and change the log level for the 
        Authentication category. 

- network segmentation enforcement: Enforcing a security zone by separating a segment of the network from access by the rest of the network. This could be accomplished using firewalls or VPNs or VLANs. A physically separate network or host (with no cabling or wireless links to other networks) is referred to as air-gapped.

- A zone is an area of the network where the security configuration is the same for all hosts within it. Network traffic between zones should be strictly controlled using a security device-typically a firewall.

- A perimeter network is a secured boundary between the Internet and an organization’s private network and uses two firewalls placed on either side of the perimeter network zone. The edge firewall restricts traffic on the external/public interface and allows permitted traffic to the hosts in the perimeter zone subnet. The edge firewall can be referred to as the screening firewall or router. The internal firewall filters communications between hosts in the perimeter and hosts on the LAN. This firewall is often described as the choke firewall. A choke point is a purposefully narrow gateway that facilitates better access control and easier monitoring.

- Packet filtering describes the earliest type of firewall. All firewalls can still perform this basic function. A packet filtering firewall is configured by specifying rules in a network access control list (ACL). Each rule defines a specific type of data packet and the appropriate action to take when a packet matches the rule. An action can be either to deny (block or drop the packet, and optionally log an event) or to accept (let the packet pass through the firewall). A packet filtering firewall works at Layer 3 of the OSI model to inspect the headers of IP packets. This means that rules can be based on the information found in those headers:

        IP filtering-Accepting or denying traffic based on its source and/or destination IP address.
        Protocol ID/type (TCP, UDP, ICMP, routing protocols, and so on).
        Port filtering/security-Accepting or denying a packet based on source and destination port numbers (TCP or UDP application type).
        Port numbers are contained in TCP or UDP headers (layer 4), rather than the IP datagram header, but packet filtering firewalls are still 
        almost always described as working at layer 3. They can inspect only port numbers and not any other layer 4 header information.

A packet filtering firewall is stateless. This means that it does not preserve information about the connection between two hosts. Each packet is analyzed independently with no record of previously processed packets. This type of filtering requires the least processing effort, but it can be vulnerable to attacks that are spread over a sequence of packets. A stateless firewall can also introduce problems in traffic flow, especially when some sort of load balancing is being used or when clients or servers need to make use of dynamically assigned ports.

- Stateful Inspection Firewalls: A circuit-level stateful inspection firewall addresses these problems by maintaining stateful information about the session established between two hosts (including malicious attempts to start a bogus session). Information about each session is stored in a dynamically updated state table. A stateful firewall operates at Layer 5 (Session) of the OSI model. When a packet arrives, the firewall checks it to confirm whether it belongs to an existing connection. If it does not, it applies the ordinary packet filtering rules to determine whether to allow it. Once the connection has been allowed, the firewall allows traffic to pass unmonitored, in order to conserve processing effort. 

- The basic function of a network firewall is to inspect packets and determine whether to block them or allow them to pass. By contrast, a proxy server forwards requests and responses on behalf of its clients. Rather than inspecting traffic as it passes through, the proxy deconstructs each packet, performs analysis, then rebuilds the packet and forwards it on, providing it conforms to the rules. This type of device is placed in a perimeter network.

- Forward Proxies: A forwarding proxy server provides for protocol-specific outbound traffic. For example, you might deploy a web proxy that enables client hosts to connect to websites and secure websites on the Internet. A proxy server must understand the application it is servicing. A web proxy must be able to parse and modify HTTP and HTTPS commands (and potentially HTML, too). Some proxy servers are application-specific; others are multipurpose. A multipurpose proxy is one configured with filters for multiple protocol types, such as HTTP, FTP, and SMTP. The main benefit of a proxy server is that clients connect to a specified point within the perimeter network for web access. This provides for a degree of traffic management and security. In addition, most web proxy servers provide caching engines, whereby frequently requested web pages are retained on the proxy, negating the need to refetch those pages for subsequent requests.
Proxy servers can generally be classed as non-transparent or transparent. A nontransparent server means that the client must be configured with the proxy server address and port number to use it. The port on which the proxy server accepts client connections is often configured as port 8080. A transparent (or "forced" or "intercepting") proxy intercepts client traffic without the client having to be reconfigured. A transparent proxy must be implemented on a switch or router or other inline network appliance.

- Reverse Proxies: A reverse proxy server provides for protocol-specific inbound traffic. For security purposes, it is inadvisable to place application servers, such as messaging and VoIP servers, in the perimeter network, where they are directly exposed to the Internet. Instead, you can deploy a reverse proxy and configure it to listen for client requests from a public network (the Internet) and create the appropriate request to the internal server on the corporate network. Reverse proxies can publish applications from the corporate network to the Internet in this way. In addition, some reverse proxy servers can handle the encryption/decryption and authentication issues that arise when remote users attempt to connect to corporate servers, reducing the overhead on those servers. Typical applications for reverse proxy servers include publishing a web server, publishing messaging or conferencing applications, and enabling POP/IMAP mail retrieval.

- NAT service builds a table of public to private address mappings. Each new session creates a new public-private address binding in the table. When the session is ended or times out, the binding is released for use by another host. 

- In NAT Port Address Translation (PAT): Maps private host IP addresses onto a single public IP address. Each host is tracked by assigning it a random high TCP port for communications.

- Network Access Control (NAC): General term for the collected protocols, policies, and hardware that authenticate and authorize access to a network at the device level.

- Intrusion detection system (IDS): Security appliance or software that uses passive hardware sensors to monitor traffic on a specific segment of the network.  Typically, an IDS is positioned behind a firewall. The aim is to detect suspicious traffic that the firewall has not blocked, providing defense in depth. 

- Intrusion prevention system (IPS): Security appliance or software that combines detection capabilities with functions that can actively block attack. One typical preventive measure is to end the session by sending a TCP reset packet to the attacking host. Another option is for the sensor to apply a temporary filter on the firewall to block the attacker's IP address (shunning). Other advanced measures include throttling bandwidth to attacking hosts, applying complex firewall filters, and even modifying suspect packets to render them harmless.

- A screened subnet. The edge or screening firewall is the public interface while the choke firewall is the LAN interface. The screened subnet is therefore configured as a perimeter network preventing hosts on the Internet being directly connected to hosts on the LAN.

-  spanning tree protocol (STP): Protocol that prevents layer 2 network loops by dynamically blocking switch ports as needed.
