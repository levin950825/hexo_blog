---
title: TCP/IP 
date: 2017-07-31 19:45:43
tags: network
categories: Others
comments:
toc: true
thumbnail:
banner:
---

A beginner's notes when learning TCP/IP.
<!-- more -->

## General Overview
---

### TCP/IP Stands for:
Transmission Control Protocol/ Internet Protocol

### What do they do?
The protocols that make up TCP/IP define:

- How data is transmitted across a network (IP)
- How data should be formatted so other networked systems can understand it (TCP)

(1. 怎么包装; 2. 怎么送达)

TCP/IP provides a complete system for formatting, transmitting, and receiving data on a network

### Decentralized Data Network
- TCP/IP's decentralized nature is a key reason it's still ubiquitous today
- 2 Key TCP/IP features support decentralization:
    - **End node verification**: the two endpoints of any data transfer are responsible for making sure it was successful - no centralized control scheme
    - **Dynamic routing**: End nodes can transfer data over multiple paths, and the network chooses the best (fastest, most reliable) path for each individual data transfer

### 5 Core Networking Problem
- Addressing
    - Physical Addressing: Every network-connected hardware device has a unique ID, referred to as a MAC (Machine Access Code) ~ _Just like a phone number_
    - Low-level TCP/IP protocols use MAC address to move data across the physical network to the right device
    - But as the network grows larger, it's impossible to let one machine to listen to all the other machines
    - IP Address: logical address configured in a node's TCPIP implementation
    - IP addresses can be broken down into network, subnet, and host ID numbers
- Routing
    - Routers are special network devices that let you divide large networks into smaller subnets
    - A well-designed network uses router to create a tree-like structure
    - Routers use the logical address information in a data packet to send it to its destination
- Name Resolution
    - Those logical IP addresses are better than MAC, but still not very good for human to read. ==> Domain Names, e.g. www.google.com
    - **Name Resolution** is the process of mapping logical addresses back and forth into domain names
    - Special **name servers** store the mapping information in databases
- Flow Control
    - TCP/IP has features to guarantee reliable data transfers
- Application Support
    - Able to run multiple network apps simultaneously
    - **Ports**: logical channels provided by TCP/IP that allow multiple applications to access the network simultaneously

    
## Key Points
---
### 7 Layers of OSI
<img src="https://s6.postimg.org/t84npj9w1/14872108408581.jpg" class="img-shadow" width="400px">

IP at Layer 3.
TCP at Layer 4.

### TCP/IP Model structure:

|Layers of TCP/IP Model|Protocols|
|---|---|
|Application| DNS, DHCP, FTP, TFTP, SMTP, HTTP, RDP, Telnet, SSH|
|Transport| TCP, UDP|
|Internet| IP, ICMP, IGMP|
|Network Access|Ethernet, Token Ring, FDDI, X.25, Frame Relay, ARP, RARP|

_Encapsulation:_
Encapsulation is the process that occurs when data is to be sent out of the source computer. When data moves from upper layer to lower layer of TCP/IP protocol stack (outgoing transmission), each layer adds the corresponding header to the data.


### IP address:

- When setting up a small private network, you are free to use ANY IP-addresses. The reserved IP-address for private network is: 192.168.x.y, where x=same number on all systems and y=different/unique number on all systems.
<img src="https://s6.postimg.org/ypx9dc53l/14872125483479.png" class="img-shadow" width="500px">

The router will have its unique IP address within the Internet world, say 233.13.23.4
也就是说在其他的private network, 里面的machine也可以用192.168.10.1, etc. NAT(network address translation) is built into the Router, and will be used to translate the address.

- When you are connected to a company network, you need to ask the Network-administrator to assign you an IP-address. 
- If you are connected to the Internet, your ISP (Internet Service Provider) will assign an IP-address to you.

_Private IP Address:_
If any router sees an address starts with '10.', it will not route data for that IP address.
10.0.0.0 - 10.255.255.255:  8 bit for network, 24 bit for host ID
172.16.0.0 - 172.31.255.255: 12 for network, 20 for host 
192.168.0.0 - 192.168.255.255: 16 for network, 16 for host

_DHCP:_ (Dynamic Host Control Protocol)
On your DHCP sever, you will specify the scope of the IPs that can be assigned, say 192.168.1.100 ~ 159
and the default gateway (i.e the router), say 192.168.1.1
and the DNS address: say 192.168.1.1
You can configure a lease period on the DHCP server. That will be the time that any dynamically assigned IP can be used by another machine. At 50% time point, the machine will try to communicate and ask for renew.

_Windowing:_ 
It is the biggest concept in TCP.
The idea is, after IP connects 2 machines (A and B) together, then A starts to transmit a file (say a word doc) to B. Under TCP, A divides the file into packages, and starts sending 1 package to B. Now the window size is 1. 
Then if B receives the 1 package successfully, it acknowledges that to A. Then A starts to send 2 packages - window size becomes 2. 
Then the window size will double, double, double.
But if there is a problem so that B cannot correctly acknowledge the last index number of received packages, the window size get back to 1.

Nowadays, this protocol causes problem for real-life communication.

_Subnet Masking:_
Usually, the subnet mask is provided by your ISP or other high level server. What we need to understand is that if the subnet mask is 255.255.255.0, it means we can at most have $2^8-2 = 254 $ machines within this subnet.

If we got 2 computers that are on different subnet, they cannot talk to each other. They need a router.

<img src="https://s6.postimg.org/95uz6wjpt/14872141052756.jpg" height="300">

<img src="https://s6.postimg.org/fnih45jgx/14872143238335.png" class="img-shadow" height="300">

<img src="https://s6.postimg.org/knfxc3p3l/14872143106349.png" class="img-shadow" height="250">

Class A
0NNNNNNN.HHHHHHHH.HHHHHHHH.HHHHHHHH
Starts with 1~126
Default subnet mask: 255.0.0.0

>leading bit is always 0. 
> $2^7 = 128$ networks, 
> $2^24 - 2$ hosts$

Class B
10NNNNNN.NNNNNNNN.HHHHHHHH.HHHHHHHH
Starts with 129~191
Default subnet mask: 255.255.0.0

>$2^14 = 16,384$ networks
>$2^16 - 2$ hosts

Class C
110NNNNN.NNNNNNNN.NNNNNNNN.HHHHHHHH
Starts with 192~223
Default subnet mask: 255.255.255.0
>$2^21 networks$ networks
>$2^8 -2 = 254$ hosts

Class D (multicast)
1110...........
Starts with 224~239, the rest is not defined 

**Loopback Address** = 127.0.0.0, any data sent to this address will be directly sent back to the sender
**Network Address**: IP address where all the host address is set to be 0
**Broadcast Address**: All host address is set to be 255. Any data sent to this address will be broadcasted to the whole network.
A: 102.255.255.255, B: 168.212.255.255, C:195.195.92.255

### TCP 3-Way Handshake:

**EVENT**
Host A sends a TCP **SYN**chronize packet to Host B

Host B receives A's **SYN**

Host B sends a **SYN**chronize-**ACK**nowledgement

Host A receives B's **SYN-ACK**

Host A sends **ACK**nowledge

Host B receives **ACK** ==> TCP socket connection is **ESTABLISHED**.

**DIAGRAM**
<img src="https://s6.postimg.org/jc3kpw441/14872147436908.gif" class="img-shadow" width="200">


**Detailed Answer**
For reliable connection, the transmitting device first establishes a connection-oriented (reliable) session with its peer system, which is called three way handshake. Data is then transferred. When the data transfer is finished, connection is terminated and virtual circuit is teared down.

1. Source send a TCP SYN segment with the initial sequence number X indicating the desire to open the connection
2. When Destination receives TCP SYN, it acknowledges this with ACK(X+1) as well as its own SYN Y (this tells Source what sequence number it will start its data with and will use in further msgs). This response is called SYN/ACK
3. Source sends and ACK(ACK = Y+1) segment to the Destination indicating that the connection is set up. Data transfer can then begin 

> This handshaking process can be used to created a DOS attack:
> The client opens up the SYN connection, the server responds with the SYN/ACK, but then the client sends another SYN. The server treats this as a new connection request and keeps the previous connection open. As this is repeated over and over many times very quickly, the server quickly becomes saturated with a huge number of connec

### Closure of Connection TCP
Unlike opening a connection, closing using "4-way" method:

1. A sends "FIN" to B
2. When B receives "FIN", B sends back "ACK" to A
3. B sends "FIN" to A
4. When A receives "FIN", A sends back "ACK" to B

<img src="https://s6.postimg.org/dufkftya9/14875567718974.jpg" class="img-shadow" height="300">


### Connection-oriented and Connectionless Protocols
Connection-oriented: e.g. TCP (Transmission Control Protocol). Services: SMTP (Email), HTTP/FTP(Web browser, apps), SSH/Telnet (Remote access)
~ telephone
Connectionless: e.g. UDP (User Datagram Protocol). Services: DNS, Video Conference, DHCP
~ post office

_Difference between Connection and Connectionless Services:_

1. In connection oriented service authentication is needed while connectionless service does not need any authentication.
2. Connection oriented protocol makes a connection and checks whether message is received or not and sends again if an error occurs connectionless service protocol does not guarantees a delivery.
3. Connection oriented service is more reliable than connectionless service.
4. Connection oriented service interface is stream based and connectionless is message based.

| |TCP|UDP|
|---|---|---|
|Usage|TCP is suited for applications that require high reliability and transmission time is less critical| UDP is suitable for applications that need fast, efficient transmission, such as games. UDP's stateless nature is also useful for servers that answer small queries from huge numbers of clients|
|Use by other protocols| HTTP, FTP, SMTP, Telnet| DNS, DHCP, RIP, VOIP|
|Ordering of packets| TCP rearranges data packets in the order specified| No inherent order. If ordering is required, it has to be managed on the application layer|
|Speed| slower than UDP| faster as there is no error-checking for packets|
|Reliability| Absolute guarantee that the data transferred remains intact and arrives in the same order in which it was sent| No guarantee that the messages or packets sent would reach at all|
|Header Size| 20 bytes| 8 bytes|
|Common Header Fields| Source port, Destination port, Check sum|Source port, Destination port, Check sum|
|Data Flow Control| TCP does Flow Control. TCP requires three packets to set up a socket connection, before any user data can be sent. TCP handles reliability and congestion control.|UDP does not have an option for flow control |

### How to defend from DDoS Attack?
First, you cannot protect your company from a DDoS attack 100% of the time. 
Then, make sure that you have ample bandwidth so that you have some extra time to react
Use CDN or VPN
Next, monitor your site closely for symptoms that might indicate a DDoS attack.
Block the machine/Network that is launching the attack 





