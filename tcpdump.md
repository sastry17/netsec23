
###
Author: Shreyas Srinivasa
NetSec 2023 - Lecture 9b Network Data Collection

# Network Data Collection with tcpdump

## Step 1: Check installation

tcpdump is installed by default in Linux. However, in this step we will check if tcpdump is installed. 

```
$ which tcpdump
/usr/sbin/tcpdump
```

## Step 2: Checking the available network interfaces

We will use the below command to check which interfaces are available in the host system for capture of traffic

```
tcpdump -D
```

you may see some options like below:
```
1.eth0 [Up, Running, Connected]
2.any (Pseudo-device that captures on all interfaces) [Up, Running]
3.lo [Up, Running, Loopback]
4.bluetooth-monitor (Bluetooth Linux Monitor) [Wireless]
5.nflog (Linux netfilter log (NFLOG) interface) [none]
6.nfqueue (Linux netfilter queue (NFQUEUE) interface) [none]
7.dbus-system (D-Bus system bus) [none]
8.dbus-session (D-Bus session bus) [none]
```

## Step 3: Capture traffic
In this step we will try to capture any traffic (on all interfaces) and print on the console 

```
sudo tcpdump --interface any -v
```

you may see some packets shown in the output as below:

```
tcpdump: data link type LINUX_SLL2
tcpdump: listening on any, link-type LINUX_SLL2 (Linux cooked v2), snapshot length 262144 bytes
12:16:30.607152 eth0  Out IP (tos 0x0, ttl 64, id 51697, offset 0, flags [DF], proto UDP (17), length 74)
    192.168.180.128.34232 > 192.168.180.2.domain: 30954+ A? contile.services.mozilla.com. (46)
12:16:30.607178 eth0  Out IP (tos 0x0, ttl 64, id 51698, offset 0, flags [DF], proto UDP (17), length 74)
    192.168.180.128.34232 > 192.168.180.2.domain: 58856+ AAAA? contile.services.mozilla.com. (46)
12:16:30.618743 eth0  B   ARP, Ethernet (len 6), IPv4 (len 4), Request who-has 192.168.180.128 tell 192.168.180.2, length 46
12:16:30.618751 eth0  Out ARP, Ethernet (len 6), IPv4 (len 4), Reply 192.168.180.128 is-at 00:0c:29:f9:7d:ee (oui Unknown), length 28
12:16:30.618907 eth0  In  IP (tos 0x0, ttl 128, id 65439, offset 0, flags [none], proto UDP (17), length 158)

```

## Step: 4 Capture traffic on specific protocol and port

### Protocol

In this step, we will capture traffic on a specific target protocol and port. For brevity, we will capture TCP traffic and on port 80.

```
sudo tcpdump -i any -c5 tcp -v
```

the above command will capture 5 TCP packets and print them on the console. If you do not see any packets after running the command, open a browser and observe the terminal for some packets. 

### Port 
Next, we will try capturing some packets on port 53 (DNS). Lets run the following command. Run the below command:
```
sudo tcpdump -i any -c5 -nn port 53 -v
```
If you do not see any packets on the console, open the browser and visit any webpage. 

## Step 5: Checking packet content
In this step, we will check the packet content. tcpdump provides two options for packet content -A for ASCII and -X for Hex. In the below example we will capture and check some HTTPS traffic in ASCII

```
sudo tcpdump -i any -c10 -nn -A port 443 -v
```
Open your browser and go to a webpage if you do not observe any packets on console

## Step 6: Writing to a pcap file
tcpdump supports writing the captured packets into a file. We will use the command below to capture packets and write to a file.
```
sudo tcpdump -i any -c10 -nn -w https-capture.pcap port 443
```
open a browser and visit a webpage to capture packets and save it. 

## Step 7: Reading a pcap file
to read a pcap file, use the below command
```
tcpdump -r https-capture.pcap
```
