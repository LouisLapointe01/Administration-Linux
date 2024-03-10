---


---

<h2 id="tp2-">TP2 :</h2>
<h2 id="etape-1--mise-en-place">ETAPE 1 : Mise en place</h2>
<pre><code>Creation de dossier seance3 dans vm : 

louislapointe@oscar:~/vm/seance3$ mkdir seance3

louislapointe@oscar:~/vm/seance3$ cd seance3

louislapointe@oscar:~/vm/seance3$

louislapointe@oscar:~/vm/seance3$ cp ../../masters/debian-testing-amd64.qcow2 vm3.qcow2

louislapointe@oscar:~/vm/seance3$ ../scripts/ovs-startup.sh vm3.qcow2 1024 130 ~&gt; Virtual machine filename   : vm3.qcow2 ~&gt; RAM size         : 1024MB ~&gt; SPICE VDI port number      : 6030 ~&gt; telnet console port number : 2430 ~&gt; MAC address                : b8:ad:ca:fe:00:82 ~&gt; Switch port interface      : tap130, access mode ~&gt; IPv6 LL address    : fe80::baad:caff:fefe:82%vlan60

louislapointe@oscar:~/vm/seance3$ ssh etu@fe80::baad:caff:fefe:82%vlan60 etu@fe80::baad:caff:fefe:82%vlan60's password: Linux vm0 6.5.0-4-amd64
#1 SMP PREEMPT_DYNAMIC Debian 6.5.10-1 (2023-11-03) x86_64

etu@vm0:~$ sudo apt update &amp;&amp; sudo apt -y full-upgrade &amp;&amp; sudo apt clean &amp;&amp; sudo apt autopurge

Lecture des listes de paquets... Fait Construction de l'arbre des dépendances... Fait Lecture des informations d'état... Fait
</code></pre>
<h2 id="etape-2--mise-à-jours">ETAPE 2 : Mise à jours</h2>
<h2 id="etuvm0-sudo-apt-install">etu@vm0:~$ sudo apt install</h2>
<pre><code>etu@vm0:~$ sudo apt install openvswitch-switch snapd
    Lecture des listes de paquets... Fait
    Construction de l'arbre des dépendances... Fait
    Lecture des informations d'état... Fait
    Les paquets supplémentaires suivants seront installés :
      dirmngr gnupg gnupg-l10n gnupg-utils gnupg1 gnupg1-l10n gpg gpg-agent
      gpg-wks-client gpg-wks-server gpgconf gpgsm libassuan0 libevent-2.1-7 libksba8
      liblzo2-2 libnpth0 libunbound8 libxdp1 openvswitch-common pinentry-curses
      python3-netifaces python3-openvswitch python3-sortedcontainers squashfs-tools
      uuid-runtime
</code></pre>
<h2 id="etuvm0-sudo-snap-install-lxd">etu@vm0:~$ sudo snap install lxd</h2>
<pre><code>etu@vm0:~$ sudo snap install lxd
2023-12-04T17:15:41+01:00 INFO Waiting for automatic snapd restart...
lxd 5.19-8635f82 from Canonical✓ installed
</code></pre>
<h2 id="etuvm0-sudo-adduser-etu-lxd">etu@vm0:~$ sudo adduser etu lxd</h2>
<pre><code>etu@vm0:~$ sudo adduser etu lxd
  info: Ajout de l'utilisateur « etu » au groupe « lxd » ...
</code></pre>
<h2 id="etuvm0-id--grep--o-lxd">etu@vm0:~$ id | grep -o lxd</h2>
<pre><code>etu@vm0:~$ id | grep -o lxd
lxd
</code></pre>
<h2 id="etuvm0-snap-list">etu@vm0:~$ snap list</h2>
<pre><code>Name    Version       Rev    Tracking       Publisher   Notes
core22  20230801      864    latest/stable  canonical✓  base
lxd     5.19-8635f82  26200  latest/stable  canonical✓  -
snapd   2.60.4        20290  latest/stable  canonical✓  snapd
</code></pre>
<h2 id="etuvm0-echo-path">etu@vm0:~$ echo $PATH</h2>
<pre><code>/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games:/snap/bin
</code></pre>
<h2 id="etape-3--commutateur-c-3po"><strong>ETAPE 3 : Commutateur C-3PO</strong></h2>
<h2 id="etuvm0-cat-etcnetworkinterfaces">etu@vm0:~$ cat /etc/network/interfaces</h2>
<pre><code># This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).
</code></pre>
<h2 id="etuvm0-ip-addr-ls">etu@vm0:~$ ip addr ls</h2>
<pre><code>1: lo: &lt;LOOPBACK,UP,LOWER_UP&gt; mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute
       valid_lft forever preferred_lft forever
2: enp0s1: &lt;BROADCAST,MULTICAST,UP,LOWER_UP&gt; mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether b8:ad:ca:fe:00:82 brd ff:ff:ff:ff:ff:ff
    inet 198.18.61.130/23 brd 198.18.61.255 scope global dynamic enp0s1
       valid_lft 85875sec preferred_lft 85875sec
    inet6 2001:678:3fc:3c:baad:caff:fefe:82/64 scope global dynamic mngtmpaddr proto kernel_ra
       valid_lft 86236sec preferred_lft 14236sec
    inet6 fe80::baad:caff:fefe:82/64 scope link proto kernel_ll
       valid_lft forever preferred_lft forever
</code></pre>
<h2 id="etuvm0-cat-etcnetworkinterfaces-1">etu@vm0:~$ cat /etc/network/interfaces</h2>
<pre><code>#The primary network interface
auto C-3PO
iface C-3PO inet dhcp
        ovs_type OVSBridge
        ovs_ports enp0s1
allow-C-3PO enp0s1
</code></pre>
<h2 id="etuvm0-ip-addr-ls-1">etu@vm0:~$ ip addr ls</h2>
<pre><code>4: C-3PO: &lt;BROADCAST,MULTICAST,UP,LOWER_UP&gt; mtu 1500 qdisc noqueue state UNKNOWN group default qlen 1000
    link/ether b8:ad:ca:fe:00:83 brd ff:ff:ff:ff:ff:ff
    inet 198.18.61.131/23 brd 198.18.61.255 scope global dynamic C-3PO
       valid_lft 86216sec preferred_lft 86216sec
    inet6 2001:678:3fc:3c:baad:caff:fefe:83/64 scope global dynamic mngtmpaddr proto kernel_ra
       valid_lft 86345sec preferred_lft 14345sec
    inet6 fe80::baad:caff:fefe:83/64 scope link proto kernel_ll
       valid_lft forever preferred_lft forever
</code></pre>
<h2 id="etuvm0-sudo-reboot">etu@vm0:~$ sudo reboot</h2>
<h2 id="etape-4--lacement-conteneur">Etape 4 : Lacement conteneur</h2>
<h2 id="etuvm0-lxd-init">etu@vm0:~$ lxd init</h2>
<pre><code>Would you like to use LXD clustering? (yes/no) [default=no]:
Do you want to configure a new storage pool? (yes/no) [default=yes]:
Name of the new storage pool [default=default]:
Name of the storage backend to use (btrfs, ceph, dir, lvm) [default=btrfs]:
Create a new BTRFS pool? (yes/no) [default=yes]:
Would you like to use an existing empty block device (e.g. a disk or partition)? (yes/no) [default=no]:
Size in GiB of the new loop device (1GiB minimum) [default=11GiB]:
Would you like to connect to a MAAS server? (yes/no) [default=no]:
Would you like to create a new local network bridge? (yes/no) [default=yes]: no
Would you like to configure LXD to use an existing bridge or host interface? (yes/no) [default=no]: yes
Name of the existing bridge or host interface: C-3PO
Would you like the LXD server to be available over the network? (yes/no) [default=no]:
Would you like stale cached images to be updated automatically? (yes/no) [default=yes]:
Would you like a YAML "lxd init" preseed to be printed? (yes/no) [default=no]:
</code></pre>
<h2 id="etuvm0-lxc-profile-device-set-default">etu@vm0:~$ lxc profile device set default</h2>
<pre><code>eth0 nictype bridged
To start your first container, try: lxc launch ubuntu:22.04
Or for a virtual machine: lxc launch ubuntu:22.04 --vm
</code></pre>
<h2 id="etuvm0-lxc-profile-show-default">etu@vm0:~$ lxc profile show default</h2>
<pre><code>config: {}
description: Default LXD profile
devices:
  eth0:
    name: eth0
    nictype: bridged
    parent: C-3PO
    type: nic
  root:
    path: /
    pool: default
    type: disk
name: default
used_by: []
</code></pre>
<h2 id="etuvm0-lxc-launch-imagesdebian12">etu@vm0:~$ lxc launch images:debian/12</h2>
<pre><code>c0
Creating c0
Starting c0
</code></pre>
<h2 id="etuvm0-lxc-launch-imagesubuntujammy-c1">etu@vm0:~$ lxc launch images:ubuntu/jammy c1</h2>
<pre><code>Creating c1
Starting c1
</code></pre>
<h2 id="etuvm0-lxc-ls">etu@vm0:~$ lxc ls</h2>
<pre><code>+--------------+---------+----------------------+-------------------------------------------+-----------+-----------+
|     NAME     |  STATE  |         IPV4         |                   IPV6                    |   TYPE    | SNAPSHOTS |
+--------------+---------+----------------------+-------------------------------------------+-----------+-----------+
| c0           | RUNNING | 198.18.60.82 (eth0)  | 2001:678:3fc:3c:216:3eff:fe89:dd26 (eth0) | CONTAINER | 0         |
+--------------+---------+----------------------+-------------------------------------------+-----------+-----------+
| c1           | RUNNING | 198.18.60.235 (eth0) | 2001:678:3fc:3c:216:3eff:fec5:87e8 (eth0) | CONTAINER | 0         |
+--------------+---------+----------------------+-------------------------------------------+-----------+-----------+
| modern-bream | RUNNING | 198.18.60.236 (eth0) | 2001:678:3fc:3c:216:3eff:fe8e:7d1b (eth0) | CONTAINER | 0         |
+--------------+---------+----------------------+-------------------------------------------+-----------+-----------+
</code></pre>
<h2 id="etape-5--service-web">ETAPE 5 : Service Web</h2>
<pre><code>etu@vm0:~$ for num in {0..1}; do lxc exec c$num -- adduser --disabled-password --gecos "" etu ; done 
etu@vm0:~$ for num in {0..1}; do lxc exec c$num -- /bin/bash -c "echo 'etu:-etu-' | chpasswd"; done 
etu@vm0:~$ for num in {0..1}; do lxc exec c$num -- apt install ssh nginx; done 
etu@vm0:~$ for num in {0..1}; do lxc exec c$num -- sed -i 's/#Port 22/Port 2222/g' /etc/ssh/sshd_config; done 
etu@vm0:~$ for num in {0..1}; do lxc exec c$num -- grep Port /etc/ssh/sshd_config; done 
etu@vm0:~$ for num in {0..1}; do lxc exec c$num -- systemctl restart ssh; done
etu@vm0:~$ for num in {0..1}; do lxc exec c$num -- apt install nginx; done
</code></pre>
<h2 id="louislapointeoscar-ssh--p-2222--l-8000localhost80-etu198.18.60.82">louislapointe@oscar:~$ ssh -p 2222 -L 8000:localhost:80 <a href="mailto:etu@198.18.60.82">etu@198.18.60.82</a></h2>
<pre><code>louislapointe@oscar:~$  ssh -p 2222 -L 8000:localhost:80 etu@198.18.60.82
etu@198.18.60.82's password:
Linux c0 6.5.0-5-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.5.13-1 (2023-11-29) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Sun Dec 10 18:20:37 2023 from 198.18.60.1
</code></pre>
<h2 id="etuc0-systemctl-status-nginx">etu@c0:~$ systemctl status nginx</h2>
<pre><code>etu@c0:~$ systemctl status nginx
● nginx.service - A high performance web server and a reverse p
roxy server
     Loaded: loaded (/lib/sy
stemd/system/nginx.service; enabled; preset: enable
d)
    Drop-In: /run/systemd/system/service.d
             └─zzz-lxc-service.conf
     Active: active (running) since Sun 2023-12-10 17:27:00 UTC
; 1h 2min ago
       Docs: man:nginx(8)
   Main PID: 696 (nginx)
      Tasks: 5 (limit: 1079)
     Memory: 3.2M
        CPU: 81ms
     CGroup: /system.slice/nginx.service
             ├─696 "nginx: master process /usr/sbin/nginx -g d
aemon on; master_process on;"
             ├─698 "nginx: worker process"
             ├─699 "nginx: worker process"
             ├─700 "nginx: worker process"
             └─701 "nginx: worker process"

Warning: some journal files were not opened due to insufficient permission
s.
</code></pre>

