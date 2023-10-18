Niveau 1
Pour les gens souhaitant taper dans le niveau 1, on va revoir les notions suivantes :

---

Linux
Virtualisation
Réseaux internes
Gestion des utilisateurs Linux

---

Enoncé:
🎯 Quels sont les deux différents types d'hyperviseur existant, et quelles sont leur différence ?

L'hyperviseur de type 1,  s’exécute directement sur le matériel de l’hôte. L'hyperviseur de type 2, s’exécute sous forme d’une couche logicielle sur un système d’exploitation, comme n’importe quel autre programme informatique.

Créez deux machines virtuelles sur votre ordinateur avec votre hyperviseur de choix. Nommez-les respectivement landing-vm1 et landing-vm2.
Utilisez une image Rocky Linux pour l'installation.
Donnez-leur des cartes réseau NAT, et ⏹️ associez-leur les adresses IP 172.16.64.2 et 172.16.64.3.
[landing-vm1](./landing-vm1.png)
[landing-vm2](./landing-vm2.png)
Connectez-vous en SSH aux machines
Vérifiez la configuration correcte des machines :
🎰 Changez le nom d'hôte des machines pour avoir respectivement vm-landing1 et vm-landing2

```
[root@vm-landing2 ~]# sudo cat /etc/hostname
vm-landing2
[root@vm-landing1 ~]# sudo cat /etc/hostname
vm-landing1
```
🎰 Trouvez l'adresse IP locale des machines
```
[root@vm-landing1 ~]# ip addr show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:71:e2:92 brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic noprefixroute enp0s3
       valid_lft 86246sec preferred_lft 86246sec
    inet6 fe80::a00:27ff:fe71:e292/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:fb:65:93 brd ff:ff:ff:ff:ff:ff
    inet 172.16.64.2/24 brd 172.16.64.255 scope global noprefixroute enp0s8
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fefb:6593/64 scope link
       valid_lft forever preferred_lft forever
```
```
[root@vm-landing2 ~]# ip addr show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:71:e2:92 brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic noprefixroute enp0s3
       valid_lft 85943sec preferred_lft 85943sec
    inet6 fe80::a00:27ff:fe71:e292/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:4d:93:b1 brd ff:ff:ff:ff:ff:ff
    inet 172.16.64.3/24 brd 172.16.64.255 scope global noprefixroute enp0s8
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe4d:93b1/64 scope link
       valid_lft forever preferred_lft forever
```
🎯 Quelle est l'adresse de broadcast ?
vm-landing1 =  172.16.64.255
🎰 Trouvez le masque de sous-réseau des machines

```
[root@vm-landing1 ~]# ip addr
 inet 172.16.64.2/24 brd 172.16.64.255 scope global noprefixroute enp0s8
```
```
[root@vm-landing2 ~]# ip addr show
inet 172.16.64.3/24 brd 172.16.64.255 scope global noprefixroute enp0s8
```
/24 donc  255.255.255.0.

🎰 Trouvez l'adresse MAC des machines
```
[root@vm-landing2 ~]# ip addr show
link/ether 08:00:27:4d:93:b1
```
```
[root@vm-landing1 ~]# ip addr   
 link/ether 08:00:27:fb:65:93
```
🎰 Pingez l'adresse publique du site www.ynov.com avec une des deux machines
```
[root@vm-landing1 ~]# ping www.ynov.com
PING www.ynov.com (172.67.74.226) 56(84) bytes of data.
64 bytes from 172.67.74.226 (172.67.74.226): icmp_seq=1 ttl=56 time=15.2 ms
^C64 bytes from 172.67.74.226: icmp_seq=2 ttl=56 time=16.1 ms

--- www.ynov.com ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 5055ms
rtt min/avg/max/mdev = 15.151/15.643/16.136/0.492 ms
```
Sur chaque machine, créez respectivement un utilisateur labo-user1 et labo-user2.

🎰 Essayez de lire le contenu du fichier /var/log/lastlog.
```
[labo-user2@vm-landing2 root]$ cat /var/log/tallylog
cat: /var/log/tallylog: Permission denied
```
```
[labo-user1@vm-landing1 root]$ cat /var/log/tallylog
cat: /var/log/tallylog: Permission denied
```
🎯 Que se passe-t-il ? Pourquoi ?
je ne peux pas y accéder car j'ai pas les permissions 
Ajoutez ces utilisateurs au groupe wheel et retentez
🎯 Que se pase-t-il ? Pourquoi ?
je ne peux pas y accéder car j'ai pas les permissions 
```
[labo-user2@vm-landing2 root]$ sudo usermod -aG wheel labo-user2
```

```
[labo-user1@vm-landing1 root]$ cat /var/log/tallylog
cat: /var/log/tallylog: Permission denied
```
Retentez avec l'utilisateur root
🎯 Que se pase-t-il ? Pourquoi ?
le fichier est vide mais je peux y accéder car j'ai les droits
```
[labo-user2@vm-landing2 root]$ sudo cat /var/log/tallylog
[sudo] password for labo-user2:
```
🎰 Pingez vm-landing2 avec vm-landing1
Sur la machine landing-vm1 :
```
[labo-user2@vm-landing2 root]$ ping 172.16.64.2
PING 172.16.64.2 (172.16.64.2) 56(84) bytes of data.
64 bytes from 172.16.64.2: icmp_seq=1 ttl=64 time=0.327 ms
64 bytes from 172.16.64.2: icmp_seq=2 ttl=64 time=0.366 ms
64 bytes from 172.16.64.2: icmp_seq=3 ttl=64 time=0.352 ms
^C
--- 172.16.64.2 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 1999ms
rtt min/avg/max/mdev = 0.327/0.348/0.366/0.016 ms

```

Installez les paquets sl, dnsmasq et htop. 🎰 Vérifiez leur version
j'ai eu un train pour sl :)
```
[root@vm-landing1 ~]# sl --version
[root@vm-landing1 ~]# dnsmasq --version
Dnsmasq version 2.85  Copyright (c) 2000-2021 Simon Kelley
Compile time options: IPv6 GNU-getopt DBus no-UBus no-i18n IDN2 DHCP DHCPv6 no-Lua TFTP no-conntrack ipset auth cryptohash DNSSEC loop-detect inotify dumpfile

This software comes with ABSOLUTELY NO WARRANTY.
Dnsmasq is free software, and you are welcome to redistribute it
under the terms of the GNU General Public License, version 2 or 3.
[root@vm-landing1 ~]# htop --version
htop 3.2.2
[root@vm-landing1 ~]# sl --version
```
Changez le DNS pour celui de CloudFlare puis 🎰 pingez google.com
```
[root@vm-landing1 ~]# cat /etc/sysconfig/network-scripts/ifcfg-enp0s8
NAME=enp0s8
DEVICE=enp0s8

BOOTPROTO=static
ONBOOT=yes

IPADDR=172.16.64.2
NETMASK=255.255.255.0

DNS1=1.1.1.1
[root@vm-landing1 ~]# ping google.com
PING google.com (142.250.179.110) 56(84) bytes of data.
64 bytes from par21s20-in-f14.1e100.net (142.250.179.110): icmp_seq=1 ttl=115 time=24.3 ms
^C64 bytes from 142.250.179.110: icmp_seq=2 ttl=115 time=22.2 ms

--- google.com ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 5052ms
rtt min/avg/max/mdev = 22.207/23.267/24.328/1.060 ms
```
Sur la machine landing-vm2 :

Installez les paquets hotop, fail2ban et unzip. 🎰 Vérifiez leur version
```
[root@vm-landing2 ~]# htop --version
htop 3.2.2
[root@vm-landing2 ~]# fail2ban-client --version
Fail2Ban v1.0.2
[root@vm-landing2 ~]# unzip --version
caution:  both -n and -o specified; ignoring -o
UnZip 6.00 of 20 April 2009, by Info-ZIP.  Maintained by C. Spieler.  Send
```
Changez le DNS pour celui de Google puis 🎰 pingez cloudflare.com
```
[root@vm-landing2 ~]# cat /etc/sysconfig/network-scripts/ifcfg-enp0s8
NAME=enp0s8
DEVICE=enp0s8

BOOTPROTO=static
ONBOOT=yes

IPADDR=172.16.64.3
NETMASK=255.255.255.0

DNS1=1.1.1.1
[root@vm-landing2 ~]# ping google.com
PING google.com (142.250.179.110) 56(84) bytes of data.
64 bytes from par21s20-in-f14.1e100.net (142.250.179.110): icmp_seq=1 ttl=115 time=21.9 ms
^C64 bytes from 142.250.179.110: icmp_seq=2 ttl=115 time=24.4 ms

--- google.com ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 5052ms
rtt min/avg/max/mdev = 21.850/23.120/24.391/1.270 ms
```
🎰 Trouvez et affichez la route par défaut présente sur la machine
```
[root@vm-landing2 ~]# ip route show default
default via 10.0.2.2 dev enp0s3 proto dhcp src 10.0.2.15 metric 100
```
Sur la machine landing-vm1, changez la carte réseau en Host-Only.
🎯 Quelle est l'utilité de ce type de carte réseau ?
j'étais déjà en host-only pour me permettre de me connecter en ssh.
🎰 Pingez landing-vm2 avec landing-vm1, que se passe-t-il ?