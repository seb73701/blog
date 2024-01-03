---
title: TCPDump (Commande)
description: 
published: true
date: 2023-04-21T11:16:14.001Z
tags: 
editor: markdown
dateCreated: 2023-04-21T11:16:09.609Z
---

====== TCPDump (Commande) ======
----

**tcpdump** est un outil de capture et d'analyse réseau. Il permet d'avoir une analyse en direct du réseau ou d'enregistrer la capture dans un fichier afin de l'analyser pour plus tard. Il permet d'écrire des filtres afin de sélectionner les paquets à capturer/analyser.

Il est basé sur la **libpcap** pour la capture, ce qui permet à ses sauvegardes d'être compatible avec d'autres analyseurs réseaux, comme **Wireshark** (//anciennement Ethereal//). Cette bibliothèque étant multiplate-forme, **tcpdump** est lui aussi porté sur la plupart des architectures.

C'est un outil indispensable à l'administration et au débugage d'applications réseaux.

----

===== Analyser le trafic sur toutes les interfaces =====

**tcpdump** n'est pas installé par défaut, il faut donc l'installer au préalable :

  * Sur **Ubuntu 16.04**

<code># apt install tcpdump</code>

  * Sur **CentOS 7**

<code># yum install tcpdump</code>

Quand on utilise **tcpdump** sans option, il va analyser le trafic de toutes les interfaces.

<code># tcpdump
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on eth0, link-type EN10MB (Ethernet), capture size 262144 bytes
07:41:25.886307 IP li339-47.members.linode.com.ssh > 169.255.7.5.44284: Flags [P.], seq 1435074392:1435074508, ack 4135933864, win 381, options [nop,nop,TS val 3387567505 ecr 18335689], length 116
07:41:25.886932 IP li339-47.members.linode.com.49063 > resolver08.dallas.linode.com.domain: 61296+ PTR? 5.7.255.169.in-addr.arpa. (42)
07:41:26.133811 IP 169.255.7.5.44284 > li339-47.members.linode.com.ssh: Flags [.], ack 0, win 722, options [nop,nop,TS val 18335757 ecr 3387567484], length 0
07:41:26.133851 IP li339-47.members.linode.com.ssh > 169.255.7.5.44284: Flags [P.], seq 116:232, ack 1, win 381, options [nop,nop,TS val 3387567753 ecr 18335757], length 116
07:41:26.142929 IP resolver08.dallas.linode.com.domain > li339-47.members.linode.com.49063: 61296 NXDomain 0/0/0 (42)
.....
.....
07:41:26.680521 IP li339-47.members.linode.com.ssh > 169.255.7.5.44284: Flags [P.], seq 2724:3132, ack 1, win 381, options [nop,nop,TS val 3387568299 ecr 18335894], length 408
^C
17 packets captured
18 packets received by filter
0 packets dropped by kernel
</code>

The format of the source is **source.port** where the **source** can be the **hostname** or **the IP address**. You can see on the first line, the packet captured at the timestamp ''**07:41:25.886307**'' is an ''**IP protocol**'' which is originated at hostname ''**li339-47.members.linode.com**'' and ''**port ssh**'', that is why you see ''**li339-47.members.linode.com.ssh**''. The packet is destined for ''**169.255.7.5.44284**'' ack flag.

You need to hit cancel button in order to stop it. If you need more information, **tcpdump** provides several options that enhance or modify its output:

  * ''**-i interface**'' : Listen on the specified interface.
  * ''**-n**'' : Don’t resolve hostnames. You can use ''**-nn**'' to don’t resolve hostnames or port names.
  * ''**-t**'' : Don't print a timestamp on each dump line.
  * ''**-X**'' : Show the packet’s contents in both hex and ASCII.
  * ''**-v**'', ''**-vv**'', ''**-vvv**'' : Increase the amount of packet information you get back.
  * ''**-c N**'' : Only get ''**N**'' number of packets and then stop.
  * ''**-s**'' : Define the snaplen (size) of the capture in bytes. Use ''**-s0**'' to get everything, unless you are intentionally capturing less.
  * ''**-S**'' : Print absolute sequence numbers.
  * ''**-q**'' : Show less protocol information.
  * ''**-w file**'' : Write the raw packets to file rather


----

===== List available interfaces =====

You can list the available interface with the -D parameter

<code>
# tcpdump -D
1.eth0
2.nflog (Linux netfilter log (NFLOG) interface)
3.nfqueue (Linux netfilter queue (NFQUEUE) interface)
4.any (Pseudo-device that captures on all interfaces)
5.lo [Loopback]
</code>

With that, you can choose which interface to use.

----

===== Analyze a specific interface and limit packets =====

Now you can decide to analyze the traffic on a specified interface with the -i parameter and limit the number of packets to capture with -c

<code>
# tcpdump -i eth0 -c 5
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on eth0, link-type EN10MB (Ethernet), capture size 262144 bytes
08:57:09.186418 IP li339-47.members.linode.com.ssh > 169.255.7.5.44284: Flags [P.], seq 1435431156:1435431272, ack 4135945080, win 419, options [nop,nop,TS val 3392110805 ecr 19471515], length 116
08:57:09.186855 IP li339-47.members.linode.com.33326 > resolver08.dallas.linode.com.domain: 9787+ PTR? 5.7.255.169.in-addr.arpa. (42)
08:57:09.335228 IP 134.119.220.87.45873 > li339-47.members.linode.com.60342: Flags [S], seq 3684168813, win 1024, length 0
08:57:09.335264 IP li339-47.members.linode.com.60342 > 134.119.220.87.45873: Flags [R.], seq 0, ack 3684168814, win 0, length 0
08:57:09.378999 IP 134.119.220.87.45873 > li339-47.members.linode.com.25070: Flags [S], seq 3509221600, win 1024, length 0
5 packets captured
13 packets received by filter
0 packets dropped by kernel
</code>

----

===== Capture data using ip address and port =====

As you can see on the capture above, we don't have the port number and the IP address of the source. You can use the -nn in order to have it

<code>
# tcpdump -i eth0 -c 5 -nn
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on eth0, link-type EN10MB (Ethernet), capture size 262144 bytes
09:17:09.572425 IP 96.126.114.47.22 > 169.255.7.5.44284: Flags [P.], seq 1435457792:1435457908, ack 4135947356, win 419, options [nop,nop,TS val 3393311191 ecr 19771613], length 116
09:17:09.605048 IP 96.126.114.47.32887 > 204.11.201.10.123: NTPv4, Client, length 48
09:17:09.663754 IP 204.11.201.10.123 > 96.126.114.47.32887: NTPv4, Server, length 48
09:17:09.785600 IP 169.255.7.5.44284 > 96.126.114.47.22: Flags [.], ack 0, win 722, options [nop,nop,TS val 19771669 ecr 3393311183], length 0
09:17:09.785646 IP 96.126.114.47.22 > 169.255.7.5.44284: Flags [P.], seq 116:700, ack 1, win 419, options [nop,nop,TS val 3393311404 ecr 19771669], length 584
5 packets captured
5 packets received by filter
0 packets dropped by kernel
</code>

----

===== Intercept packet from a specific port =====

You can decide to intercept packets to a specified port number with port parameter.

<code>
# tcpdump -i eth0 -c 5 -nn port 22
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on eth0, link-type EN10MB (Ethernet), capture size 262144 bytes
09:27:27.773270 IP 96.126.114.47.22 > 169.255.7.5.44284: Flags [P.], seq 1435459900:1435460016, ack 4135948192, win 419, options [nop,nop,TS val 3393929392 ecr 19926162], length 116
09:27:27.773357 IP 96.126.114.47.22 > 169.255.7.5.44284: Flags [P.], seq 116:232, ack 1, win 419, options [nop,nop,TS val 3393929392 ecr 19926162], length 116
09:27:28.032620 IP 169.255.7.5.44284 > 96.126.114.47.22: Flags [.], ack 0, win 722, options [nop,nop,TS val 19926230 ecr 3393929384], length 0
09:27:28.032655 IP 96.126.114.47.22 > 169.255.7.5.44284: Flags [P.], seq 232:648, ack 1, win 419, options [nop,nop,TS val 3393929652 ecr 19926230], length 416
09:27:28.032668 IP 169.255.7.5.44284 > 96.126.114.47.22: Flags [.], ack 116, win 722, options [nop,nop,TS val 19926230 ecr 3393929392], length 0
5 packets captured
6 packets received by filter
0 packets dropped by kernel
</code>

----

===== Intercept packet by ignoring a specific port =====

You can decide to ignore a port when you intercept packets. This is possible with the not port parameter

<code>
# tcpdump -i eth0 -c 5 -nn not port 22
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on eth0, link-type EN10MB (Ethernet), capture size 262144 bytes
11:15:53.784094 IP 134.119.220.87.45873 > 96.126.114.47.32724: Flags [S], seq 1210911834, win 1024, length 0
11:15:53.784139 IP 96.126.114.47.32724 > 134.119.220.87.45873: Flags [R.], seq 0, ack 1210911835, win 0, length 0
11:15:53.910633 IP 134.119.220.87.45873 > 96.126.114.47.32724: Flags [R], seq 1210911835, win 1200, length 0
11:15:53.911319 IP 134.119.220.87 > 96.126.114.47: ICMP host 134.119.220.87 unreachable - admin prohibited, length 48
11:15:56.327699 IP 134.119.220.87.45873 > 96.126.114.47.18566: Flags [S], seq 3213454109, win 1024, length 0
5 packets captured
6 packets received by filter
0 packets dropped by kernel
</code>

----

===== Intercept packets from a specific protocol =====

You can decide to capture icmp or tcp packets only

<code>
# tcpdump -i eth0 -c 5 -nn tcp
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on eth0, link-type EN10MB (Ethernet), capture size 262144 bytes
09:49:33.371487 IP 96.126.114.47.22 > 169.255.7.5.44284: Flags [P.], seq 1435550388:1435550504, ack 4135954104, win 438, options [nop,nop,TS val 3395254990 ecr 20257561], length 116
09:49:33.371612 IP 96.126.114.47.22 > 169.255.7.5.44284: Flags [P.], seq 116:232, ack 1, win 438, options [nop,nop,TS val 3395254990 ecr 20257561], length 116
09:49:33.371788 IP 96.126.114.47.22 > 169.255.7.5.44284: Flags [P.], seq 232:452, ack 1, win 438, options [nop,nop,TS val 3395254991 ecr 20257561], length 220
09:49:33.371956 IP 96.126.114.47.22 > 169.255.7.5.44284: Flags [P.], seq 452:648, ack 1, win 438, options [nop,nop,TS val 3395254991 ecr 20257561], length 196
09:49:33.631626 IP 169.255.7.5.44284 > 96.126.114.47.22: Flags [.], ack 116, win 722, options [nop,nop,TS val 20257629 ecr 3395254981], length 0
5 packets captured
7 packets received by filter
0 packets dropped by kernel
</code>

You can just replace tcp by icmp for that one

----

===== Record log to some specific file =====

It is possible to save the captured packets in a file. By default, when capturing packets into a file, it will save only 68 bytes of the data from the each packet. Rest of the information is ignored. You can use -s to tell tcpdump how many bytes for each packets to save and specify 0 as packets snapshot length tells tcpdump to save whole packet.

<code>
# tcpdump -i eth0 -c 5 -nn tcp -w packets-record.cap -s 0
tcpdump: listening on eth0, link-type EN10MB (Ethernet), capture size 262144 bytes
5 packets captured
5 packets received by filter
0 packets dropped by kernel
</code>

----

===== Read tcpdump record file =====

You can't read the content of a file which saves tcpdump packets with the common commands such as cat or less but you need to use the -r parameter of the tcpdump command

<code>
# tcpdump -r packets-record.cap 
reading from file packets-record.cap, link-type EN10MB (Ethernet)
10:06:25.310077 IP li339-47.members.linode.com.ssh > 169.255.7.5.44284: Flags [P.], seq 1435573932:1435573976, ack 4135958592, win 457, options [nop,nop,TS val 3396266929 ecr 20510549], length 44
10:06:25.565590 IP 169.255.7.5.44284 > li339-47.members.linode.com.ssh: Flags [.], ack 0, win 722, options [nop,nop,TS val 20510616 ecr 3396266919], length 0
10:06:25.565633 IP li339-47.members.linode.com.ssh > 169.255.7.5.44284: Flags [P.], seq 44:160, ack 1, win 457, options [nop,nop,TS val 3396267184 ecr 20510616], length 116
10:06:25.570384 IP 169.255.7.5.44284 > li339-47.members.linode.com.ssh: Flags [.], ack 44, win 722, options [nop,nop,TS val 20510617 ecr 3396266929], length 0
10:06:25.827438 IP 169.255.7.5.44284 > li339-47.members.linode.com.ssh: Flags [.], ack 160, win 722, options [nop,nop,TS val 20510681 ecr 3396267184], length 0
</code>

----

===== Capture packets with more information =====

You can scan the network more deeply. You can use the combinations of the command to filter what you need

<code>
# tcpdump -i eth0 -c 5 -ttttnnvvS
tcpdump: listening on eth0, link-type EN10MB (Ethernet), capture size 262144 bytes
2018-04-10 10:32:36.073756 IP (tos 0x10, ttl 64, id 14601, offset 0, flags [DF], proto TCP (6), length 96)
    96.126.114.47.22 > 169.255.7.5.44284: Flags [P.], cksum 0x8404 (incorrect -> 0x570b), seq 1435611412:1435611456, ack 4135969472, win 495, options [nop,nop,TS val 3397837693 ecr 20903238], length 44
2018-04-10 10:32:36.073896 IP (tos 0x10, ttl 64, id 14602, offset 0, flags [DF], proto TCP (6), length 168)
    96.126.114.47.22 > 169.255.7.5.44284: Flags [P.], cksum 0x844c (incorrect -> 0x14ec), seq 1435611456:1435611572, ack 4135969472, win 495, options [nop,nop,TS val 3397837693 ecr 20903238], length 116
2018-04-10 10:32:36.074118 IP (tos 0x10, ttl 64, id 14603, offset 0, flags [DF], proto TCP (6), length 200)
    96.126.114.47.22 > 169.255.7.5.44284: Flags [P.], cksum 0x846c (incorrect -> 0x52d8), seq 1435611572:1435611720, ack 4135969472, win 495, options [nop,nop,TS val 3397837693 ecr 20903238], length 148
2018-04-10 10:32:36.083469 IP (tos 0x8, ttl 53, id 26190, offset 0, flags [none], proto ICMP (1), length 68)
    134.119.220.87 > 96.126.114.47: ICMP host 134.119.220.87 unreachable - admin prohibited, length 48
        IP (tos 0x28, ttl 48, id 23212, offset 0, flags [DF], proto TCP (6), length 40)
    96.126.114.47.47317 > 134.119.220.87.45873: Flags [R.], cksum 0x5362 (correct), seq 0, ack 96384300, win 0, length 0
2018-04-10 10:32:36.084338 IP (tos 0x0, ttl 244, id 32726, offset 0, flags [none], proto TCP (6), length 40)
    134.119.220.87.45873 > 96.126.114.47.47317: Flags [R], cksum 0x4ec2 (correct), seq 96384300, win 1200, length 0
5 packets captured
5 packets received by filter
0 packets dropped by kernel
</code>

----

===== Capture packets coming from a remote host =====

To only show packets which come from a specific IP, you use the src parameter

<code>
# tcpdump -i eth0 -c 5 -ttttnnvvS src host 96.126.114.1
tcpdump: listening on eth0, link-type EN10MB (Ethernet), capture size 262144 bytes
2018-04-10 11:27:28.498964 ARP, Ethernet (len 6), IPv4 (len 4), Reply 96.126.114.1 is-at 00:00:0c:9f:f0:06, length 46
2018-04-10 11:28:08.614258 ARP, Ethernet (len 6), IPv4 (len 4), Reply 96.126.114.1 is-at 00:00:0c:9f:f0:06, length 46
2018-04-10 11:28:53.621982 ARP, Ethernet (len 6), IPv4 (len 4), Reply 96.126.114.1 is-at 00:00:0c:9f:f0:06, length 46
2018-04-10 11:29:33.511165 ARP, Ethernet (len 6), IPv4 (len 4), Reply 96.126.114.1 is-at 00:00:0c:9f:f0:06, length 46
2018-04-10 11:30:13.837251 ARP, Ethernet (len 6), IPv4 (len 4), Reply 96.126.114.1 is-at 00:00:0c:9f:f0:06, length 46
5 packets captured
5 packets received by filter
0 packets dropped by kernel
</code>

You can see the requests packets

----

===== Capture packets in the destination to a remote host =====

It is possible to show only packages that have a specific destination. For example, you can show packages in the destination to the router

<code>
# tcpdump -i eth0 -c 5 -ttttnnvvS dst host 96.126.114.1
tcpdump: listening on eth0, link-type EN10MB (Ethernet), capture size 262144 bytes
2018-04-10 11:34:15.107495 ARP, Ethernet (len 6), IPv4 (len 4), Request who-has 96.126.114.1 tell 96.126.114.47, length 28
2018-04-10 11:35:00.547492 ARP, Ethernet (len 6), IPv4 (len 4), Request who-has 96.126.114.1 tell 96.126.114.47, length 28
2018-04-10 11:35:47.907837 ARP, Ethernet (len 6), IPv4 (len 4), Request who-has 96.126.114.1 tell 96.126.114.47, length 28
2018-04-10 11:36:12.867576 ARP, Ethernet (len 6), IPv4 (len 4), Request who-has 96.126.114.1 tell 96.126.114.47, length 28
2018-04-10 11:36:39.534063 ARP, Ethernet (len 6), IPv4 (len 4), Request who-has 96.126.114.1 tell 96.126.114.47, length 28
5 packets captured
5 packets received by filter
0 packets dropped by kernel
</code>

Here you can see the replies packets

----

===== Capture both incoming and outgoing packets of a specific host =====

In the two commands above, we used src and dst to capture incoming and outgoing packets from a specific host in two different times. But it is possible to do it directly in one command with only the host parameter

<code>
# tcpdump -i eth0 -c 5 -ttttnnvvS host 96.126.114.1
tcpdump: listening on eth0, link-type EN10MB (Ethernet), capture size 262144 bytes
2018-04-10 11:37:49.720992 ARP, Ethernet (len 6), IPv4 (len 4), Request who-has 96.126.114.1 tell 96.126.114.47, length 28
2018-04-10 11:37:49.725683 ARP, Ethernet (len 6), IPv4 (len 4), Reply 96.126.114.1 is-at 00:00:0c:9f:f0:06, length 46
2018-04-10 11:38:14.894130 ARP, Ethernet (len 6), IPv4 (len 4), Request who-has 96.126.114.1 tell 96.126.114.47, length 28
2018-04-10 11:38:14.900008 ARP, Ethernet (len 6), IPv4 (len 4), Reply 96.126.114.1 is-at 00:00:0c:9f:f0:06, length 46
2018-04-10 11:38:39.854051 ARP, Ethernet (len 6), IPv4 (len 4), Request who-has 96.126.114.1 tell 96.126.114.47, length 28
5 packets captured
5 packets received by filter
0 packets dropped by kernel
</code>

Now you can see both request and reply packets.

----

===== Capture packet using port range =====

It is possible to use a range of ports in order to capture your network traffic.

<code>
# tcpdump -i eth0 -c 3 -nns 0 portrange 20-23
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on eth0, link-type EN10MB (Ethernet), capture size 262144 bytes
11:59:45.996312 IP 96.126.114.47.22 > 169.255.7.5.44284: Flags [P.], seq 1435738516:1435738632, ack 4136021820, win 875, options [nop,nop,TS val 3403067615 ecr 22210718], length 116
11:59:45.996512 IP 96.126.114.47.22 > 169.255.7.5.44284: Flags [P.], seq 116:232, ack 1, win 875, options [nop,nop,TS val 3403067615 ecr 22210718], length 116
11:59:45.996728 IP 96.126.114.47.22 > 169.255.7.5.44284: Flags [P.], seq 232:452, ack 1, win 875, options [nop,nop,TS val 3403067616 ecr 22210718], length 220
3 packets captured
5 packets received by filter
0 packets dropped by kernel
</code>

----

----

----

----

===== Basics =====

Below are a few options you can use when configuring tcpdump. They’re easy to forget and/or confuse with other types of filters, e.g., Wireshark, so hopefully this page can serve as a reference for you, as it does me. here are the main ones I like to keep in mind depending on what I’m looking at.

===== Options =====

-i any : Listen on all interfaces just to see if you’re seeing any traffic.
-i eth0 : Listen on the eth0 interface.
-D : Show the list of available interfaces
-n : Don’t resolve hostnames.
-nn : Don’t resolve hostnames or port names.
-q : Be less verbose (more quiet) with your output.
-t : Give human-readable timestamp output.
-tttt : Give maximally human-readable timestamp output.
-X : Show the packet’s contents in both hex and ascii.
-XX : Same as -X, but also shows the ethernet header.
-v, -vv, -vvv : Increase the amount of packet information you get back.
-c : Only get x number of packets and then stop.
-s : Define the snaplength (size) of the capture in bytes. Use -s0 to get everything, unless you are intentionally capturing less.
-S : Print absolute sequence numbers.
-e : Get the ethernet header as well.
-q : Show less protocol information.
-E : Decrypt IPSEC traffic by providing an encryption key.

The default snaplength as of tcpdump 4.0 has changed from 68 bytes to 96 bytes. While this will give you more of a packet to see, it still won’t get everything. Use -s 1514 or -s 0 to get full coverage.

==== Expressions ====

In tcpdump, Expressions allow you to trim out various types of traffic and find exactly what you’re looking for. Mastering the expressions and learning to combine them creatively is what makes one truly powerful with tcpdump.

There are three main types of expression: type, dir, and proto.

Type options are: host, net, and port.
Direction lets you do src, dst, and combinations thereof.
Proto(col) lets you designate: tcp, udp, icmp, ah, and many more.
==== Examples ====

So, now that we’ve seen what our options are, let’s look at some real-world examples that we’re likely to see in our everyday work.

===== basic communication =====

Just see what’s going on, by looking at all interfaces.

<code># tcpdump -i any</code>

===== specific interface =====

Basic view of what’s happening on a particular interface.

<code># tcpdump -i eth0</code>

===== raw output view =====

Verbose output, with no resolution of hostnames or port numbers, absolute sequence numbers, and human-readable timestamps.

<code># tcpdump -ttttnnvvS</code>

===== find traffic by ip =====

One of the most common queries, this will show you traffic from 1.2.3.4, whether it’s the source or the destination.

<code># tcpdump host 1.2.3.4</code>

===== seeing more of the packet with hex output =====

Hex output is useful when you want to see the content of the packets in question, and it’s often best used when you’re isolating a few candidates for closer scrutiny.

<code># tcpdump -nnvXSs 0 -c1 icmp</code>

<code>
tcpdump: listening on eth0, link-type EN10MB (Ethernet), 23:11:10.370321 IP 
(tos 0x20, ttl  48, id 34859, offset 0, flags [none], length: 84) 
69.254.213.43 > 72.21.34.42: icmp 64: echo request seq 0 
        0x0000:  4520 0054 882b 0000 3001 7cf5 45fe d52b  E..T.+..0.|.E..+
        0x0010:  4815 222a 0800 3530 272a 0000 25ff d744  H."..50'..%..D
        0x0020:  ae5e 0500 0809 0a0b 0c0d 0e0f 1011 1213  .^..............
        0x0030:  1415 1617 1819 1a1b 1c1d 1e1f 2021 2223  .............!"#
        0x0040:  2425 2627 2829 2a2b 2c2d 2e2f 3031 3233  $%&'()*+,-./0123
        0x0050:  3435 3637                                4567
1 packets captured
1 packets received by filter
0 packets dropped by kernel
</code>

TABLE 2. — VERBOSE CAPTURE OF AN ICMP PACKET.

===== filtering by source and destination =====

It’s quite easy to isolate traffic based on either source or destination using src and dst.

<code>
# tcpdump src 2.3.4.5
# tcpdump dst 3.4.5.6
</code>

===== finding packets by network =====

To find packets going to or from a particular network, use the net option. You can combine this with the src or dst options as well.

<code># tcpdump net 1.2.3.0/24</code>

===== show traffic related to a specific port =====

You can find specific port traffic by using the port option followed by the port number.

<code># tcpdump port 3389</code>

<code># tcpdump src port 1025</code>

===== show traffic of one protocol =====

If you’re looking for one particular kind of traffic, you can use tcp, udp, icmp, and many others as well.

<code># tcpdump icmp</code>

===== show only ip6 traffic =====

You can also find all IP6 traffic using the protocol option.

<code># tcpdump ip6</code>

===== find traffic using port ranges =====

You can also use a range of ports to find traffic.

<code># tcpdump portrange 21-23</code>

===== find traffic based on packet size =====

If you’re looking for packets of a particular size you can use these options. You can use less, greater, or their associated symbols that you would expect from mathematics.

<code># tcpdump less 32</code>

<code># tcpdump greater 64</code>

<code># tcpdump <= 128</code>

===== writing captures to a file =====

It’s often useful to save packet captures into a file for analysis in the future. These files are known as PCAP (PEE-cap) files, and they can be processed by hundreds of different applications, including network analyzers, intrusion detection systems, and of course by tcpdump itself. Here we’re writing to a file called capture_file using the -w switch.

<code># tcpdump port 80 -w capture_file</code>

===== reading pcap files =====

You can read PCAP files by using the -r switch. Note that you can use all the regular commands within tcpdump while reading in a file; you’re only limited by the fact that you can’t capture and process what doesn’t exist in the file already.

<code># tcpdump -r capture_file</code>

===== Advanced =====

Now that we’ve seen what we can do with the basics through some examples, let’s look at some more advanced stuff.

===== it’s all about the combinations =====

Being able to do these various things individually is powerful, but the real magic of tcpdump comes from the ability to combine options in creative ways in order to isolate exactly what you’re looking for. There are three ways to do combinations, and if you’ve studied programming at all they’ll be pretty familar to you.

<code>
AND 
and or &&
OR 
or or ||
EXCEPT 
not or !
</code>

Here are some examples of combined commands.

===== from specific ip and destined for a specific port =====

Let’s find all traffic from 10.5.2.3 going to any host on port 3389.

<code>tcpdump -nnvvS src 10.5.2.3 and dst port 3389</code>

===== from one network to another =====
Let’s look for all traffic coming from 192.168.x.x and going to the 10.x or 172.16.x.x networks, and we’re showing hex output with no hostname resolution and one level of extra verbosity.

<code>tcpdump -nvX src net 192.168.0.0/16 and dst net 10.0.0.0/8 or 172.16.0.0/16</code>

===== non icmp traffic going to a specific ip =====

This will show us all traffic going to 192.168.0.2 that is not ICMP.

<code>tcpdump dst 192.168.0.2 and src net and not icmp</code>

===== traffic from a host that isn’t on a specific port =====

This will show us all traffic from a host that isn’t SSH traffic (assuming default port usage).

<code>tcpdump -vv src mars and not dst port 22</code>

As you can see, you can build queries to find just about anything you need. The key is to first figure out precisely what you’re looking for and then to build the syntax to isolate that specific type of traffic.

==== complex grouping and special characters ====


Also keep in mind that when you’re building complex queries you might have to group your options using single quotes. Single quotes are used in order to tell tcpdump to ignore certain special characters—in this case below the “( )” brackets. This same technique can be used to group using other expressions such as host, port, net, etc. Take a look at the command below.

# Traffic that’s from 10.0.2.4 AND destined for ports 3389 or 22 (incorrect)
<code># tcpdump src 10.0.2.4 and (dst port 3389 or 22)</code>

If you tried to run this otherwise very useful command, you’d get an error because of the parenthesis. You can either fix this by escaping the parenthesis (putting a before each one), or by putting the entire command within single quotes:

# Traffic that’s from 10.0.2.4 AND destined for ports 3389 or 22 (correct)
<code># tcpdump 'src 10.0.2.4 and (dst port 3389 or 22)'</code>

===== isolating specific tcp flags =====


You can also capture traffic based on specific TCP flag(s).

The filters below find these various packets because tcp[13] looks at offset 13 in the tcp header, the number represents the location within the byte, and the !=0 means that the flag in question is set to 1, i.e. it’s on.
===== Show me all URGENT (URG) packets… =====


<code># tcpdump 'tcp[13] & 32!=0'</code>

===== Show me all ACKNOWLEDGE (ACK) packets… =====


<code># tcpdump 'tcp[13] & 16!=0'</code>

===== Show me all PUSH (PSH) packets… =====


<code># tcpdump 'tcp[13] & 8!=0'</code>

===== Show me all RESET (RST) packets… =====


<code># tcpdump 'tcp[13] & 4!=0'</code>

===== Show me all SYNCHRONIZE (SYN) packets… =====


<code># tcpdump 'tcp[13] & 2!=0'</code>

===== Show me all FINISH (FIN) packets… =====


<code># tcpdump 'tcp[13] & 1!=0'</code>

===== Show me all SYNCHRONIZE/ACKNOWLEDGE (SYNACK) packets… =====


<code># tcpdump 'tcp[13]=18'</code>

Only the PSH, RST, SYN, and FIN flags are displayed in tcpdump‘s flag field output. URGs and ACKs are displayed, but they are shown elsewhere in the output rather than in the flags field.
As with most powerful tools, however, there are multiple ways to do things. The example below shows another way to capture packets with specific TCP flags set.

<code># tcpdump 'tcp[tcpflags] == tcp-syn'</code>

# Capture RST Flags Using the tcpflags option…

<code># tcpdump 'tcp[tcpflags] == tcp-rst'</code>

# Capture FIN Flags Using the tcpflags option…

<code># tcpdump 'tcp[tcpflags] == tcp-fin'</code>

The same technique can be used for the other flags as well; they have been omitted in the interest of space.
identifying noteworthy traffic

Finally, there are a few quick recipes you’ll want to remember for catching specific and specialized traffic, such as malformed / likely-malicious packets.

# packets with both the rst and syn flags set (this should never be the case)

<code># tcpdump 'tcp[13] = 6'</code>

# find cleartext http get requests

<code># tcpdump 'tcp[32:4] = 0x47455420'</code>

# find ssh connections on any port (via banner text)

<code># tcpdump 'tcp[(tcp[12]>>2):4] = 0x5353482D'</code>

# packets with a ttl less than 10 (usually indicates a problem or use of traceroute)

<code># tcpdump 'ip[8] < 10'</code>

# packets with the evil bit set (hacker trivia more than anything else)

<code># tcpdump 'ip[6] & 128 != 0'</code>
Source : [[https://danielmiessler.com/study/tcpdump/]]

===== Conclusion =====

Tcpdump offers some options to filter the packets of the traffic to capture. Although packet sniffers are useful diagnostic tools, they can also be abused. For instance, unscrupulous individuals can run packet sniffers to capture passwords that others send over the network. Depending on your
network configuration, this trick can work even if the packet sniffer isn’t running on either the sending or the receiving computer. For this reason, many organizations have policies forbidding the use of packet sniffers
except under limited circumstances.