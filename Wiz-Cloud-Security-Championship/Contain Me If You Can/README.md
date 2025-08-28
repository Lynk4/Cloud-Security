# Contain Me If You Can 

---

<div align="center">
  <img src="https://github.com/user-attachments/assets/3693d72e-ba84-401e-a0ba-2949bceb2596">
</div>


---

### **Scanning the network.**

```bash
root@e8bd7e375226:~# ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.19.0.3  netmask 255.255.0.0  broadcast 172.19.255.255
        ether 02:42:ac:13:00:03  txqueuelen 0  (Ethernet)
        RX packets 4813  bytes 39057229 (39.0 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 4336  bytes 259696 (259.6 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 2020  bytes 81885 (81.8 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 2020  bytes 81885 (81.8 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

root@e8bd7e375226:~# nmap -sN 172.19.0.0/24 -v
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-08-28 14:07 UTC
Initiating ARP Ping Scan at 14:07
Scanning 255 hosts [1 port/host]

root@e8bd7e375226:~# nmap -sN 172.19.0.0/24   
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-08-28 14:07 UTC
Nmap scan report for 172.19.0.1
Host is up (0.0000040s latency).
Not shown: 999 closed tcp ports (reset)
PORT     STATE         SERVICE
5432/tcp open|filtered postgresql
MAC Address: 02:42:21:CD:CC:E4 (Unknown)

Nmap scan report for postgres_db.user_db_network (172.19.0.2)
Host is up (0.0000050s latency).
Not shown: 999 closed tcp ports (reset)
PORT     STATE         SERVICE
5432/tcp open|filtered postgresql
MAC Address: 02:42:AC:13:00:02 (Unknown)

Nmap scan report for e8bd7e375226 (172.19.0.3)
Host is up (0.0000030s latency).
All 1000 scanned ports on e8bd7e375226 (172.19.0.3) are in ignored states.
Not shown: 1000 closed tcp ports (reset)

Nmap done: 256 IP addresses (3 hosts up) scanned in 3.28 seconds
root@e8bd7e375226:~#
```

---

- hostname: postgres_db.user_db_network (172.19.0.2)
- e8bd7e375226 (172.19.0.3)

---

### postgres_db.user_db_network (172.19.0.2) scan

- PostgreSQL exposed inside the network.
  
```bash
root@e8bd7e375226:~# nmap -sV -p- 172.19.0.2
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-08-28 14:18 UTC
Nmap scan report for postgres_db.user_db_network (172.19.0.2)
Host is up (0.0000070s latency).
Not shown: 65534 closed tcp ports (reset)
PORT     STATE SERVICE    VERSION
5432/tcp open  postgresql PostgreSQL DB 9.6.0 or later
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port5432-TCP:V=7.94SVN%I=7%D=8/28%Time=68B06552%P=x86_64-pc-linux-gnu%r
SF:(SMBProgNeg,8C,"E\0\0\0\x8bSFATAL\0VFATAL\0C0A000\0Munsupported\x20fron
SF:tend\x20protocol\x2065363\.19778:\x20server\x20supports\x203\.0\x20to\x
SF:203\.0\0Fpostmaster\.c\0L2147\0RProcessStartupPacket\0\0");
MAC Address: 02:42:AC:13:00:02 (Unknown)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 7.02 seconds
```

---




