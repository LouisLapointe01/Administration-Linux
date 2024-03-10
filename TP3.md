---


---

<h2 id="tp3-">TP3 :</h2>
<h2 id="etape-1--correction-script">ETAPE 1 : Correction script</h2>
<pre><code>louislapointe@oscar:~/vm$ nano checkup.sh
louislapointe@oscar:~/vm$ cat checkup.sh
#!/bin/bash

    set  -e

    GreenOnBlack='\E[32m'

    echo  "~&gt; Check-up gestion des machines virtuelles"

    # Arborescence

    if [ ! -L ~/masters ]; then

    echo  "Création du lien vers le catalogue des images de machines virtuelles."

    ln  -s  /var/cache/kvm/masters  ~

    fi

    if [ ! -d ~/vm ]; then

    echo  "Création du dossier des images de machines virtuelles."

    mkdir  ~/vm

    ln  -s  ~/masters/scripts  ~/vm/

    fi

    echo  -e  "${GreenOnBlack}~&gt; Arborescence prête."

    tput  sgr0

    # Analyse des fichiers image

    count=0

    listFilename="$HOME/image-list-info.json"

    echo  -n  "[" &gt; "$listFilename"

    imageList=$(find ~ -type f \( -iname \*.qcow2 -o -iname \*.raw \)

    -printf '%p ')

    for  file  in  $imageList

    do

    echo  "$((++count)) : $file"

    if ! (pgrep  -u  "${USER}" | grep  -q  -o  "${file##*/}\ ") ; then

    qemu-img  info  --output=json  --backing-chain  "$file" | tr  -d  '[]' &gt;&gt; "$listFilename"

    sed  -i  '/^$/d'  "$listFilename" &amp;&amp; sed  -i  '$s/$/\,/'  "$listFilename"

    fi

    done

    sed  -i  '$ s/,/\n]/g'  "$listFilename"

    echo  -e  "${GreenOnBlack}~&gt; $count fichier(s) image(s) présent(s)."

    tput  sgr0

    # Liste des machines virtuelles actives

    IFS=$'\n'

    count=0

    vmArray=( "$(pgrep -u "${USER}" |  grep 'qemu-system-x86_64\ ' ||  true)" )

    if [ ${#vmArray[@]} -gt 0 ]; then

    for  vm  in  "${vmArray[@]}"

    do

    echo  -n  "$((++count)) : "

    echo  "$vm" | grep  -Po  '(?&lt;=-name\ ).*(?=-m)' | tr  -d  '\n'

    echo  -n  "utilise le cordon "

    echo  "$vm" | grep  -Po  'tap\d{1,3}'

    done

    fi

    echo  -e  "${GreenOnBlack}~&gt; $count machine(s) virtuelle(s) active(s)."

    tput  sgr0

    exit  0
</code></pre>
<h2 id="etape-2--install-web-server">ETAPE 2 : Install-web-server</h2>
<pre><code>etu@vm0:~$ sudo apt install task-web-server
[sudo] Mot de passe de etu :
Lecture des listes de paquets... Fait
Construction de l'arbre des dépendances... Fait
Lecture des informations d'état... Fait
Les paquets supplémentaires suivants seront installés :
  analog apache2 apache2-bin apache2-data apache2-doc apache2-utils
 ...

etu@vm0:~$ umask
0022
</code></pre>
<h2 id="etape-3--index.html">ETAPE 3 : index.html</h2>
<pre><code>etu@vm0:~$ nano index.html
etu@vm0:~$ cat index.html
&lt;!DOCTYPE html&gt;
&lt;html&gt;
  &lt;head&gt;
    &lt;metacharset="utf-8"&gt;
    &lt;title&gt;PACAUD VALERIAN&lt;/title&gt;
  &lt;/head&gt;
  &lt;body&gt;
    &lt;p&gt;PACAUD VALERIAN&lt;/p&gt;
    &lt;p&gt;ADMIN SYSTEME LINUX&lt;/p&gt;
    &lt;p&gt;TP3&lt;/p&gt;
  &lt;/body&gt;
&lt;/html&gt;

etu@vm0:~$ cat /etc/hosts
127.0.0.1       localhost
127.0.1.1       vm0
127.0.0.2       newsite
# The following lines are desirable for IPv6 capable hosts
::1     localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters

etu@vm0:/etc$ sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/newsitez.conf
cp: impossible d'évaluer '/etc/apache2/sites-available/000-default.conf': Aucun fichier ou dossier de ce type
</code></pre>

