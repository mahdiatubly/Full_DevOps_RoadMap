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
