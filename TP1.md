---


---

<h2 id="tp1-louis-lapointe">TP1 Louis Lapointe</h2>
<h2 id="etape-1--adresses-ipv4-et-ipv6-et-resolver-dns">ETAPE 1 : Adresses ipv4 et ipv6 et resolver DNS</h2>
<h2 id="adresses-ipv4-et-ipv6-">Adresses ipv4 et ipv6 :</h2>
<p>netsh int ip show int :</p>
<pre><code>PS C:\Users\luigi&gt; netsh int ip show int

Idx     Mét         MTU          État                Nom
---  ----------  ----------  ------------  ---------------------------
  1          75  4294967295  connected     Loopback Pseudo-Interface 1
 48           1        1500  connected     Connexion au réseau local
  4          45        1500  connected     Wi-Fi
 54          25        1500  disconnected  OpenVPN Connect DCO Adapter
  7          25        1500  disconnected  Connexion au réseau local* 1
  6          65        1500  disconnected  Connexion réseau Bluetooth
 17          25        1500  connected     Ethernet 2

</code></pre>
<p>netsh int ipv4 show dnsservers:</p>
<pre><code>Configuration pour l'interface « Connexion au réseau local »
    Serveurs DNS configurés statiquement : Aucun
    Enregistrer avec le suffixe :           Principale uniquement

</code></pre>
<p>netsh int ipv6 show dnsservers:</p>
<pre><code>Configuration pour l'interface « Connexion au réseau local »
    Serveurs DNS configurés statiquement : 2001:678:3fc::1
    Enregistrer avec le suffixe :           Principale uniquement

</code></pre>
<p>netsh int ip show dnsservers :</p>
<pre><code>PS C:\Users\luigi&gt; netsh int ip show dnsservers
Configuration pour l'interface « Connexion au réseau local »
    Serveurs DNS configurés statiquement : 172.16.0.2
    Enregistrer avec le suffixe :           Principale uniquement

</code></pre>
<p>netsh int ip show addr:</p>
<pre><code>Configuration pour l'interface « Connexion au réseau local »
    DHCP activé :                         Non
    Adresse IP :                           172.16.196.94
    Préfixe de sous-réseau :               172.16.196.92/30 (masque 255.255.255.252)
    Métrique de l'interface :             1

</code></pre>
<p>netsh int ipv6 show addr :</p>
<pre><code>PS C:\Users\luigi&gt; netsh int ipv6 show addr
 
Interface 48 : Connexion au réseau local

Addr Type  État DAD    Vie valide Pers. Fav. Adresse
---------  ----------- ---------- ---------- ------------------------
Manuel     Préféré       infinite   infinite 2a03:7220:8083:c4c4::1016
Autre      Préféré       infinite   infinite fe80::3cfb:22ad:834c:b95f%48

</code></pre>
<p>netsh int ip show route:</p>
<pre><code>Publier  Type      Mét  Préfixe                   Idx  Nom passerelle/interface
-------  --------  ---  ------------------------  ---  ------------------------
Non      Manuel    0    0.0.0.0/0
 4  172.17.128.1
Non      Manuel    256  0.0.0.0/1
48  172.16.196.93

</code></pre>
<p>netsh int ipv6 show route:</p>
<pre><code>Non      Système   256  255.255.255.255/32
Publier  Type      Mét  Préfixe                   Idx  Nom passerelle/interface
-------  --------  ---  ------------------------  ---  ------------------------
Non      Manuel    256  ::/1                       48  fe80::8
Non      Système   256  ::1/128                     1  Loopback Pseudo-Interface 1

</code></pre>
<h2 id="resolver-dns-">Resolver DNS :</h2>
<p>nslookup oscar.infra.stri :</p>
<pre><code>Serveur :   UnKnown
Address:  2001:678:3fc::1

Nom :    oscar.infra.stri
Addresses:  2001:678:3fc:3::7
          172.16.0.7

</code></pre>
<p>ping 9.9.9.9:</p>
<pre><code>PS C:\Users\luigi&gt; ping 9.9.9.9

Envoi d’une requête 'Ping'  9.9.9.9 avec 32 octets de données :
Réponse de 9.9.9.9 : octets=32 temps=49 ms TTL=60
Réponse de 9.9.9.9 : octets=32 temps=69 ms TTL=60
Réponse de 9.9.9.9 : octets=32 temps=116 ms TTL=60
Réponse de 9.9.9.9 : octets=32 temps=126 ms TTL=60

Statistiques Ping pour 9.9.9.9:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 49ms, Maximum = 126ms, Moyenne = 90ms

</code></pre>
<h1 id="etape-2--ssh">Etape 2 : SSH</h1>
<h2 id="connexion-ssh-">Connexion SSH :</h2>
<p>ssh -p 2222 louislapointe@oscar.infra.stri :</p>
<pre><code>PS C:\Users\luigi&gt; ssh -p 2222 louislapointe@oscar.infra.stri
louislapointe@oscar.infra.stri's password:
Creating home directory for louislapointe.
Linux oscar 6.5.0-4-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.5.10-1 (2023-11-03) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
-----
                  __             _
|  ||_  _   o _  /  \ _ _  _  _   )
|/\|| )(_)  |_)  \__/_)(_ (_||   o

        An opponent, similar to Mallory, but not necessarily malicious.

        https://en.wikipedia.org/wiki/Alice_and_Bob

-----

</code></pre>
<h2 id="fichier-.ssh-">Fichier .ssh :</h2>
<pre><code># Read more about SSH config files: https://linux.die.net/man/5/ssh_config

Host oscar6

HostName 2001:678:3fc:3::7

User louislapointe

Port 2222

ForwardAgent yes

  

# hyperviseur Oscar avec VPN et DNS

Host oscar

HostName oscar.infra.stri

User louislapointe

Port 2222

ForwardAgent yes

</code></pre>
<h2 id="générer-créer-copier-clefs-">Générer créer copier clefs :</h2>
<pre><code>    PS C:\Users\luigi&gt; ssh-keygen.exe -t ed25519
Generating public/private ed25519 key pair.
Enter file in which to save the key (C:\Users\luigi/.ssh/id_ed25519):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in C:\Users\luigi/.ssh/id_ed25519
Your public key has been saved in C:\Users\luigi/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:mHf3H9L/rhjDc97EYtTqTQSOL+hL/Vw87EhfBWzNHjM luigi@Pc-de-Louis
The key's randomart image is:
+--[ED25519 256]--+
|                 |
|             . o |
|              =Eo|
|       o     + =+|
|      o S . o o =|
|       . . = +o=.|
|          o BoB=B|
|         o  .&amp;=O=|
|          o...*=O|
+----[SHA256]-----+
PS C:\Users\luigi&gt; ls .\.ssh\


    Répertoire : C:\Users\luigi\.ssh


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        21/11/2023     18:03            332 config
-a----        21/11/2023     18:38            411 id_ed25519
-a----        21/11/2023     18:38            100 id_ed25519.pub
-a----        21/11/2023     18:05            977 known_hosts
-a----        21/11/2023     17:36            106 known_hosts.old

 
PS C:\Users\luigi&gt; ssh oscar “mkdir -p .ssh &amp;&amp; touch .ssh/authorized_keys”
louislapointe@oscar.infra.stri's password:
PS C:\Users\luigi&gt; ssh oscar “chmod 600 .ssh/authorized_keys”
louislapointe@oscar.infra.stri's password:
PS C:\Users\luigi&gt; type .ssh/id_ed25519.pub | ssh oscar “cat &gt;&gt; .ssh/authorized_keys”

</code></pre>
<h1 id="etape-3-">Etape 3 :</h1>
<h2 id="création-de-lien-symbolique-">Création de lien symbolique :</h2>
<pre><code>$ ln -s /var/cache/kvm/masters
$ mkdir vm
ln -s ../masters/scripts/
cp ../masters/debian-testing-amd64.qcow2 .
cp ../masters/debian-testing-amd64.QCOW2_OVMF_VARS.fd .

</code></pre>
<h2 id="vérification-masters-vm-">Vérification Masters VM :</h2>
<pre><code>louislapointe@oscar:~$ pwd
/home/louislapointe
louislapointe@oscar:~$ ls
masters  vm
louislapointe@oscar:~$

</code></pre>
<h2 id="vérification-vm-">Vérification VM :</h2>
<pre><code>louislapointe@oscar:~/vm$ pwd
/home/louislapointe/vm
louislapointe@oscar:~/vm$ ls
debian-testing-amd64.qcow2  scripts
louislapointe@oscar:~/vm$

</code></pre>
<h2 id="lancement-vm-">Lancement VM :</h2>
<pre><code>louislapointe@oscar:~/vm$ ./scripts/ovs-startup.sh debian-testing-amd64.qcow2  2048 130
~&gt; Virtual machine filename   : debian-testing-amd64.qcow2
~&gt; MAC address                : b8:ad:ca:fe:00:82
~&gt; IPv6 LL address            : fe80::baad:caff:fefe:82%vlan60

ssh etu@fe80::baad:caff:fefe:82%vlan60
etu@vm0:~$

</code></pre>
<h2 id="ip-public-attribué-à-ma-machine-">IP public attribué à ma machine :</h2>
<pre><code>etu@vm0:~$ ip a
inet6 2001:678:3fc:3c:baad:caff:fefe:82/64

</code></pre>
<h2 id="modification-du-.sshconfig-">Modification du .ssh/config :</h2>
<pre><code># Read more about SSH config files: https://linux.die.net/man/5/ssh_config

Host oscar6

HostName 2001:678:3fc:3::7

User louislapointe

Port 2222

ForwardAgent yes

  

# hyperviseur Oscar avec VPN et DNS

Host oscar

HostName oscar.infra.stri

User louislapointe

Port 2222

ForwardAgent yes

  

Host vm0

HostName 2001:678:3fc:3c:baad:caff:fefe:82

AddressFamily inet6

Port 2222

User etu

</code></pre>
<h1 id="etape-4-">Etape 4 :</h1>
<h2 id="connexion-à-la-vm-">Connexion à la vm :</h2>
<pre><code>ssh vm0

ssh -p 2222 -L 6030:localhost:6030 oscar

</code></pre>
<h2 id="aprés-téléchargement-de-virt-viewer-“remote-viewer”-">Aprés téléchargement de Virt-Viewer “Remote Viewer” :</h2>
<pre><code>spice://localhost:6030

</code></pre>

