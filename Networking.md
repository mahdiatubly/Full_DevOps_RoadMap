# Networking

- The Server Message Block protocol (SMB protocol) is a client-server communication protocol used for sharing access to files, printers, serial ports and other resources on a network. SMB relies on the TCP and IP protocols for transport. This combination potentially allows file sharing over complex, interconnected networks, including the public Internet. The SMB server component uses TCP port 445.(works on presentation layer)
- DHCP is an application layer protocol
- To get network interfaces on the host, use:

        $ip link
        # To get details about the ip that your pc uses
        $ip add
        # To reports interface statistics
        $ip -s link
        # To enable or disable an interface
        $ip link set [inteface name] [up OR down]
        # To modify IP address config
        $ip [add OR delete]


- To sniff the packets on an interface:

        # To write the result to a file use => -w
        $tcpdump -i [interface name] -w [file name]

- APIPA Adress (Automatic Private IP Addressing): assigns a class B address from `169.254.0.0` to `169.254.255.255` to the client when DHCP server is either permanently or temporarly unavailable.
  -IGMP protocol is used in the multicas addressing to config group memebership & IP addresses.
- Anycast: Group of hosts with same IP, closest will be chosen.
- 0.0.0.0/8: commonly used by clients to request DHCP lease