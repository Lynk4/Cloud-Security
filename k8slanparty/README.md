# K8s LAN Party Challenge

#### https://k8slanparty.com/

---


<img width="1380" height="859" alt="Screenshot 2025-09-01 at 2 55 52 PM" src="https://github.com/user-attachments/assets/16587dd1-1b28-4b40-960a-ce343551d555" />


---

## **Recon**


<img width="1379" height="859" alt="Screenshot 2025-09-01 at 2 56 48 PM" src="https://github.com/user-attachments/assets/7b17ad15-5ed8-45ce-a486-5beef81e4ff7" />



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



