# Hello World

<a href="https://twitter.com/alxsysl">Twitter</a> \| <a href="https://www.instagram.com/alxsys/">Instagram</a> \| <a href="https://www.linkedin.com/in/iskendersoysal/">LinkedIn</a> \| iskendersoysal@protonmail.com

# Nano Hotkeys

CTRL + 6 = Mark Set
CTRL + Y = Begining of file
CTRL + E = End of line
CTRL + K = Cut
ALT + 6 = Copy
CTRL + U = Paste

# DOCKERFILE directives:

https://docs.docker.com/engine/reference/builder/

# Disable Hyper-V through cmd (Err:"verr_vmx_no_vmx")

dism.exe /Online /Disable-Feature:Microsoft-Hyper-V

# Cygwin Setup

Download <a href="https://www.cygwin.com/setup-x86_64.exe"> Cygwin </a>

Select 'lynx' package during setup.  

After installation run Cygwin and apply following commands in order to install commandline installer for Cygwin:

`lynx -source rawgit.com/transcode-open/apt-cyg/master/apt-cyg > apt-cyg`

`install apt-cyg /bin`

Usage is same as apt-get:
`apt-cyg install nano` 

# Look for a file/folder in Linux machine

`sudo find / | grep <string>`

# Shutdown Linux Machine

`sudo shutdown -h now`

# Tracking Malicious Scanning Activity Using Tshark
Tshark is the command line interface for the computer network unit (frame, packet, datagram) analyzer application Wireshark. It provides sniffing all layers of the traffic on the network which is connected to, and besides the wireshark filters can be used also bash commands can be used for filtering and sorting outputs from sniff files or during sniffing. Various Linux distributions also have another command line computer network unit analyzer called Tcpdump which is installed by default.
To be able to use Tshark on your Linux machine all you have to do install Wireshark on your machine with the relevant package manager.

•	First you need to check all dependencies:
- For RPM package manager based distributions:
sudo yum install gcc gcc-c++ bison flex libpcap-devel qt-devel gtk3-devel rpm-build libtool c-ares-devel qt5-qtbase-devel qt5-qtmultimedia-devel qt5-linguist desktop-file-utils
- For Debian based distributions:
sudo apt-get install build-essential checkinstall libcurl4-openssl-dev bison flex qt5-default qttools5-dev libssl-dev libgtk-3-dev libpcap-d

•	Install Wireshark, then tshark would be available for use with command line:
For RPM package manager based distributions:
-	sudo yum install wireshark wireshark-qt
For Debian based distributions, :
-	sudo apt-get install wireshark 
•	Some useful tshark command parameters:
-	-i : The network interface of the host machine which will be sniffed
-	-Y : Filter to be used for displaying packets
-	-r : Capture file to be read
-	-w : File which will be the captured packets written on with pcapng format
-	-T : Output format (fields, json, etc.)
-	-e : Field to print if -T is selected as "fields". (e.g. tcp.port)

•	Some useful wireshark filters to use with -Y parameter:
-	ip.host : Any source or destination host IP address to inspect bidirectional traffic
-	ip.src : Source IP address for outgoing network units
-	ip.dst : Destination IP address for incoming network units
-	tcp/udp.port : Specify which port traffic should be displayed
-	tcp/udp.flags : Define which packets should be displayed with relevant TCP or UDP flags

You can observe burst of ARP packets if some host is trying to learn all alive hosts by broadcasting ARP requests (usually done with nmap or netdiscover) to every IP address available in relevant subnet that is being used on your network. Attacker might also try to learn alive hosts by ping sweep if ICMP echo-request packets are not dropped in your network. Delay between scan intervals between each IP can be prolonged in order to decrease detectability of scanning. IP address for the host which scanning initiated from is 192.168.56.104 for all examples below. Here we can see a fast network scan is initiated with ARP requests:

-	Command: 
tshark -i eth0 -Y 'arp'
-	Output:
1 0.000000000 PcsCompu_30:a2:72 → Broadcast    ARP 42 Who has 192.168.56.1? Tell 192.168.56.104
2 0.001415100 PcsCompu_30:a2:72 → Broadcast    ARP 42 Who has 192.168.56.2? Tell 192.168.56.104
3 0.002762354 PcsCompu_30:a2:72 → Broadcast    ARP 42 Who has 192.168.56.3? Tell 192.168.56.104
4 0.004714815 PcsCompu_30:a2:72 → Broadcast    ARP 42 Who has 192.168.56.4? Tell 192.168.56.104
5 0.006660983 PcsCompu_30:a2:72 → Broadcast    ARP 42 Who has 192.168.56.5? Tell 192.168.56.104
6 0.008658743 PcsCompu_30:a2:72 → Broadcast    ARP 42 Who has 192.168.56.6? Tell 192.168.56.104
7 0.010039706 PcsCompu_30:a2:72 → Broadcast    ARP 42 Who has 192.168.56.7? Tell 192.168.56.104
8 0.011434585 PcsCompu_30:a2:72 → Broadcast    ARP 42 Who has 192.168.56.8? Tell 192.168.56.104
9 0.012516033 PcsCompu_30:a2:72 → Broadcast    ARP 42 Who has 192.168.56.9? Tell 192.168.56.104
...
 
IP protocol scan is yet another way for attacker to discover alive hosts, you can observe burst of destination unreachable ICMP packets originated from target machines with command below in a IP protocol scanning:

-	Command: 
tshark -i eth0 -Y 'icmp.type==3 and icmp.code==2'
-	Output: 
522 3.167653065 192.168.56.105 → 192.168.56.104 ICMP 62 Destination unreachable (Protocol unreachable)
523 3.167670319 192.168.56.105 → 192.168.56.104 ICMP 62 Destination unreachable (Protocol unreachable)
524 3.167672757 192.168.56.100 → 192.168.56.104 ICMP 70 Destination unreachable (Protocol unreachable)
525 3.167675033 192.168.56.100 → 192.168.56.104 ICMP 70 Destination unreachable (Protocol unreachable)
526 3.167748616 192.168.56.105 → 192.168.56.104 ICMP 62 Destination unreachable (Protocol unreachable)
527 3.167752020 192.168.56.105 → 192.168.56.104 ICMP 62 Destination unreachable (Protocol unreachable)
528 3.167753988 192.168.56.105 → 192.168.56.104 ICMP 62 Destination unreachable (Protocol unreachable)
529 3.167861516 192.168.56.100 → 192.168.56.104 ICMP 70 Destination unreachable (Protocol unreachable)
530 3.167868412 192.168.56.100 → 192.168.56.104 ICMP 70 Destination unreachable (Protocol unreachable)
…

If we apply filter 'tcp.flags == 0x002' which is the indicator flag for SYN packets, we will also be able to see if this source machine tried to stealth scan any alive host's ports via sending TCP-SYN packets to see if they are open, closed or filtered. In the following output we can observe source machine selected a dynamic port 58756 to start the scan against target machines and sent TCP-SYN packets randomly to known/important static ports between 1 to 49151 in order to learn what services are running on the machines and what ports are filtered or open. If destination port is open, target machine will reply to source machine's request with a TCP-SYN+ACK packet, if it is closed machine will reply with TCP-SYN+RST packet.

-	Command: 
tshark -i eth0 -Y 'tcp.flags == 0x002 and ip.src == 192.168.56.104'
-	Output:
2409 3.296070335 192.168.56.104 → 192.168.56.105 TCP 58 58756 → 49152 [SYN] Seq=0 Win=1024 Len=0 MSS=1460
 2412 3.296216461 192.168.56.104 → 192.168.56.100 TCP 58 58756 → 49152 [SYN] Seq=0 Win=1024 Len=0 MSS=1460
 2416 3.296411357 192.168.56.104 → 192.168.56.105 TCP 58 58756 → 2105 [SYN] Seq=0 Win=1024 Len=0 MSS=1460
 2419 3.296561397 192.168.56.104 → 192.168.56.100 TCP 58 58756 → 2105 [SYN] Seq=0 Win=1024 Len=0 MSS=1460
 2421 3.296702081 192.168.56.104 → 192.168.56.105 TCP 58 58756 → 1500 [SYN] Seq=0 Win=1024 Len=0 MSS=1460
 2424 3.296877124 192.168.56.104 → 192.168.56.100 TCP 58 58756 → 1500 [SYN] Seq=0 Win=1024 Len=0 MSS=1460
 2425 3.297024724 192.168.56.104 → 192.168.56.105 TCP 58 58756 → 714 [SYN] Seq=0 Win=1024 Len=0 MSS=1460
 2427 3.297162749 192.168.56.104 → 192.168.56.100 TCP 58 58756 → 714 [SYN] Seq=0 Win=1024 Len=0 MSS=1460
 2429 3.297332668 192.168.56.104 → 192.168.56.105 TCP 58 58756 → 6059 [SYN] Seq=0 Win=1024 Len=0 MSS=1460
 2431 3.297489069 192.168.56.104 → 192.168.56.100 TCP 58 58756 → 6059 [SYN] Seq=0 Win=1024 Len=0 MSS=1460
 2433 3.297605973 192.168.56.104 → 192.168.56.105 TCP 58 58756 → 1031 [SYN] Seq=0 Win=1024 Len=0 MSS=1460
 2436 3.297845913 192.168.56.104 → 192.168.56.100 TCP 58 58756 → 1031 [SYN] Seq=0 Win=1024 Len=0 MSS=1460
 2437 3.297909578 192.168.56.104 → 192.168.56.105 TCP 58 58756 → 2005 [SYN] Seq=0 Win=1024 Len=0 MSS=1460
 2438 3.298023847 192.168.56.104 → 192.168.56.100 TCP 58 58756 → 2005 [SYN] Seq=0 Win=1024 Len=0 MSS=1460
...

Msrpc service running and relevant port (135) is open on target machine, response originated from target machine:
-	Command: 
tshark -i eth0 -Y 'tcp.port==135 and ip.src == 192.168.56.105'
-	Output:
578 3.624844432 192.168.56.105 → 192.168.56.104 TCP 60 135 → 55299 [SYN, ACK] Seq=0 Ack=1 Win=64240 Len=0 MSS=1460

SSH port (22) is closed on target machine, response originated from target machine:
-	Command: 
tshark -i eth0 -Y 'tcp.port==22 and ip.src == 192.168.56.105'
-	Output:
524 7.685295528 192.168.56.105 → 192.168.56.104 TCP 60 22 → 51614 [RST, ACK] Seq=1 Ack=1 Win=0 Len=0

Due to some privilege restrictions on the host machine which is used for scanning, attacker might not be able to initiate stealth SYN scan but instead can start TCP connect scan. In this type of scan, in addition to initial SYN sent from the source and SYN-ACK sent back from the target, you should observe that host machine sent an ACK packet to target machine to continue with the TCP handshake and then it will send RST-ACK packet to target machine:

-	Command: 
tshark -i eth0 -Y 'tcp.port==135' (Again, 135 is an open port on target machine)
-	Output:
519 3.659834221 192.168.56.104 → 192.168.56.105 TCP 74 34070 → 135 [SYN] Seq=0 Win=64240 Len=0 MSS=1460 SACK_PERM=1 
529 3.660039327 192.168.56.105 → 192.168.56.104 TCP 66 135 → 34070 [SYN, ACK] Seq=0 Ack=1 Win=65535 Len=0 MSS=1460 
530 3.660042513 192.168.56.104 → 192.168.56.105 TCP 54 34070 → 135 [ACK] Seq=1 Ack=1 Win=64256 Len=0
532 3.660104602 192.168.56.104 → 192.168.56.105 TCP 54 34070 → 135 [RST, ACK] Seq=1 Ack=1 Win=64256 Len=0

Attacker can also execute UDP scan to discover the hosts and open ports. Sometimes open UDP ports are disregarded by administrators since most of the services use TCP and UDP scanning takes too much time and effort, but there are many exploitable UDP services that might be installed on the machines too. Like in TCP-SYN flood, you should observe burst of requests on various ports. You can simply detect UDP scan with the following filter:

-	Command: 
tshark -i eth0 -Y 'icmp.type==3 and icmp.code==3'
-	Output:
487 3.390934944 192.168.56.100 → 192.168.56.104 ICMP 70 Destination unreachable (Port unreachable)
488 3.390949919 192.168.56.100 → 192.168.56.104 ICMP 70 Destination unreachable (Port unreachable)
489 3.390968448 192.168.56.100 → 192.168.56.104 ICMP 70 Destination unreachable (Port unreachable)
490 3.390971032 192.168.56.100 → 192.168.56.104 ICMP 70 Destination unreachable (Port unreachable)
491 3.390973479 192.168.56.100 → 192.168.56.104 ICMP 70 Destination unreachable (Port unreachable)
499 3.393619554 192.168.56.100 → 192.168.56.104 ICMP 70 Destination unreachable (Port unreachable)
...

FIN, PSH and URG flags are manipulated in outgoing TCP packets to conduct XMAS scan. It can be observed that following filter prints out the packets that indicated relevant source host scanned the ports of two other machines.

-	Command:
tshark -i eth0 -Y 'tcp.flags==0x029'
-	Output:
3266 4.114724959 192.168.56.104 → 192.168.56.100 TCP 54 40020 → 5225 [FIN, PSH, URG] Seq=1 Win=1024 Urg=0 Len=0
3267 4.114757846 192.168.56.104 → 192.168.56.100 TCP 54 40020 → 3493 [FIN, PSH, URG] Seq=1 Win=1024 Urg=0 Len=0
3268 4.114794841 192.168.56.104 → 192.168.56.100 TCP 54 40020 → 1138 [FIN, PSH, URG] Seq=1 Win=1024 Urg=0 Len=0
3269 4.114839269 192.168.56.104 → 192.168.56.100 TCP 54 40020 → 1309 [FIN, PSH, URG] Seq=1 Win=1024 Urg=0 Len=0
3280 5.130209962 192.168.56.104 → 192.168.56.105 TCP 54 40021 → 48080 [FIN, PSH, URG] Seq=1 Win=1024 Urg=0 Len=0
3281 5.130286894 192.168.56.104 → 192.168.56.105 TCP 54 40020 → 4045 [FIN, PSH, URG] Seq=1 Win=1024 Urg=0 Len=0
3282 5.130312740 192.168.56.104 → 192.168.56.105 TCP 54 40020 → 1839 [FIN, PSH, URG] Seq=1 Win=1024 Urg=0 Len=0
3283 5.130333063 192.168.56.104 → 192.168.56.105 TCP 54 40020 → 6646 [FIN, PSH, URG] Seq=1 Win=1024 Urg=0 Len=0
...

PROTECTION / MITIGATION

-	A network automation system which will try to detect any kind of malicious scanning and will have the privilege to block the layer 2 communication of malicious computers to the network.
-	Configuring the network firewall to drop suspicious packets with alerting level intervals and ping packets.
-	Using logical and protective rule tables on operating system firewalls, getting rid of redundant services running on machines.
-	Keeping all operating systems up-to-date, which might reduce or block the exploitablility of open ports.
-	For hardening the defense mechanisms against network host & port probe attacks redirecting open ports to honeypot machines and to dummy services with firewall configurations is also a productive way in order to create attack notification systems to indetify the intrusion beforehand and build countermeasures quickly for open ports in real systems.

