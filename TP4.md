---


---

<h2 id="tp4-">TP4 :</h2>
<h2 id="etape-1--créer-nouvel-utilisateur">ETAPE 1 : Créer nouvel utilisateur</h2>
<pre><code>etu@vm0:~$ sudo adduser newuser
info: Ajout de l'utilisateur « newuser » ...
info: Choix d'un UID/GID dans la plage 1000 à 59999 ...
info: Ajout du nouveau groupe « newuser » (1001) ...
info: Ajout du nouvel utilisateur « newuser » (1001) avec le groupe « newuser » (1001) ...
info: Création du répertoire personnel « /home/newuser » ...
info: Copie des fichiers depuis « /etc/skel » ...
Nouveau mot de passe :
Retapez le nouveau mot de passe :
passwd : mot de passe mis à jour avec succès
Modifier les informations associées à un utilisateur pour newuser
Entrer la nouvelle valeur, ou appuyer sur ENTER pour la valeur par défaut
        NOM []: newuser
        Numéro de chambre []:
        Téléphone professionnel []:
        Téléphone personnel []:
        Autre []:
Cette information est-elle correcte ? [O/n]O
info: Ajout du nouvel utilisateur « newuser » aux groupes supplémentaires « users » ...
info: Ajout de l'utilisateur « newuser » au groupe « users » ...
</code></pre>
<h2 id="etape-2--ajout-au-groupe-admin">ETAPE 2 : Ajout au groupe admin</h2>
<pre><code>     etu@vm0:~$ sudo adduser newuser adm
    info: Ajout de l'utilisateur « newuser » au groupe « adm » ...
</code></pre>
<h2 id="etape-3--ajout-utilisateur-en-dév-dun-site-dynamique">ETAPE 3 : Ajout utilisateur en dév d’un site dynamique</h2>
<pre><code>    etu@vm0:~$ sudo adduser newuser www-data
    info: Ajout de l'utilisateur « newuser » au groupe « www-data » ...
</code></pre>
<h2 id="etape-4--configuration-de-pam">ETAPE 4 : Configuration de PAM</h2>
<pre><code>    etu@vm0:~$ cat /etc/login.defs
    /etc/login.defs - Configuration control definitions for the login package.
    #
    # Three items must be defined:  MAIL_DIR, ENV_SUPATH, and ENV_PATH.
    # If unspecified, some arbitrary (and possibly incorrect) value will
    # be assumed.  All other items are optional - if not specified then
    # the described action or option will be inhibited.
...

################# OBSOLETED #######################
#                                                 #
# These options are no more handled by shadow.    #
#                                                 #
# Shadow utilities will display a warning if they #
# still appear.                                   #
#                                                 #
###################################################

# CLOSE_SESSIONS
# LOGIN_STRING
# NO_PASSWORD_CONSOLE
# QMAIL_DIR


etu@vm0:~$ cat /etc/pam.d/common-session
#
# /etc/pam.d/common-session - session-related modules common to all services
#
# This file is included from other service-specific PAM config files,
# and should contain a list of modules that define tasks to be performed
# at the start and end of interactive sessions.
...
# and here are more per-package modules (the "Additional" block)
session required        pam_unix.so
session optional        pam_systemd.so
# end of pam-auth-update config
</code></pre>
<h2 id="etape-5--verif">ETAPE 5 : Verif</h2>
<pre><code>etu@vm0:~$ Exit
...
louislapointe@oscar:~/vm$ ssh etu@fe80::baad:caff:fefe:82%vlan60
...
etu@vm0:~$ umask
0022
</code></pre>
<h2 id="etape-6--affichage-des-50-dernières-lignes-des-logs">ETAPE 6 : Affichage des 50 dernières lignes des logs</h2>
<pre><code>etu@vm0:~$ last -n 50
etu      pts/0        fe80::64f4:67ff: Fri Dec 22 22:33   still logged in
etu      pts/0        fe80::64f4:67ff: Fri Dec 22 22:32 - 22:33  (00:00)
etu      pts/0        fe80::64f4:67ff: Fri Dec 22 23:23 - 22:32  (-00:51)
reboot   system boot  6.5.0-4-amd64    Fri Dec 22 23:23   still running
etu      pts/0        fe80::64f4:67ff: Sun Dec 10 18:31 - 17:31  (-00:59)
reboot   system boot  6.5.0-4-amd64    Sun Dec 10 18:31   still running
etu      pts/0        fe80::64f4:67ff: Tue Dec  5 17:56 - 18:36  (00:39)
etu      pts/0        fe80::64f4:67ff: Tue Dec  5 16:44 - 17:13  (00:28)
reboot   system boot  6.5.0-4-amd64    Tue Dec  5 16:44   still running
etu      pts/1        fe80::64f4:67ff: Tue Dec  5 16:24 - 16:44  (00:20)
etu      pts/0        fe80::64f4:67ff: Tue Dec  5 17:07 - 16:44  (-00:23)
reboot   system boot  6.5.0-4-amd64    Tue Dec  5 17:07 - 16:44  (-00:23)
reboot   system boot  6.5.0-4-amd64    Tue Dec  5 17:06 - 16:44  (-00:21)
etu      pts/0        fe80::64f4:67ff: Tue Dec  5 16:02 - 16:03  (00:00)
reboot   system boot  6.5.0-4-amd64    Mon Dec  4 18:44 - 16:44  (21:59)
etu      pts/0        fe80::64f4:67ff: Mon Dec  4 17:33 - 17:43  (00:09)
etu      pts/0        fe80::64f4:67ff: Mon Dec  4 18:25 - 17:33  (-00:51)
reboot   system boot  6.5.0-4-amd64    Mon Dec  4 18:24 - 16:44  (22:19)
etu      pts/0        fe80::64f4:67ff: Mon Dec  4 17:09 - 17:23  (00:14)
reboot   system boot  6.5.0-4-amd64    Mon Dec  4 18:08 - 16:44  (22:35)
etu      pts/0        fe80::64f4:67ff: Mon Dec  4 17:04 - 17:05  (00:01)
etu      pts/0        fe80::64f4:67ff: Mon Dec  4 18:03 - 17:03  (-00:59)
reboot   system boot  6.5.0-4-amd64    Mon Dec  4 18:03 - 16:44  (22:40)
etu      tty1                          Tue Nov 28 17:33 - crash (6+00:30)
reboot   system boot  6.5.0-4-amd64    Tue Nov 28 18:09 - 16:44 (6+22:34)
reboot   system boot  6.5.0-4-amd64    Tue Nov 28 18:06 - 16:44 (6+22:38)
reboot   system boot  6.5.0-4-amd64    Tue Nov 28 18:04 - 16:44 (6+22:39)
etu      pts/0        2a03:7220:8083:c Tue Nov 28 16:37 - 16:53  (00:16)
etu      ttyS0                         Tue Nov 28 16:36 - 16:59  (00:23)
reboot   system boot  6.5.0-4-amd64    Tue Nov 28 17:35 - 16:44 (6+23:08)
etu      pts/0        fe80::64f4:67ff: Mon Nov 27 21:46 - 21:00  (-00:46)
reboot   system boot  6.5.0-4-amd64    Mon Nov 27 21:46 - 16:44 (7+18:57)
etu      pts/0        fe80::64f4:67ff: Mon Nov 27 18:35 - 18:44  (00:08)
reboot   system boot  6.5.0-4-amd64    Mon Nov 27 19:34 - 16:44 (7+21:09)
etu      pts/0        fe80::9803:fff:f Mon Nov 20 10:27 - 09:28  (-00:58)
reboot   system boot  6.5.0-4-amd64    Mon Nov 20 10:27 - 09:28  (-00:58)
etu      pts/0        fe80::9803:fff:f Sat Nov 18 16:07 - 16:07  (00:00)
reboot   system boot  6.5.0-4-amd64    Sat Nov 18 17:06 - 16:07  (-00:58)
etu      pts/0        fe80::9803:fff:f Thu Nov 16 17:56 - 16:57  (-00:59)
reboot   system boot  6.5.0-4-amd64    Thu Nov 16 17:56 - 16:57  (-00:58)
etu      pts/0        fe80::9803:fff:f Tue Nov 14 08:04 - 08:06  (00:02)
reboot   system boot  6.5.0-4-amd64    Tue Nov 14 09:02 - 08:06  (-00:56)
etu      pts/0        fe80::9803:fff:f Mon Nov 13 19:20 - 19:22  (00:01)
reboot   system boot  6.5.0-4-amd64    Mon Nov 13 20:19 - 19:22  (-00:57)
etu      pts/0        fe80::9803:fff:f Sun Nov 12 11:24 - 10:24  (-00:59)
reboot   system boot  6.5.0-4-amd64    Sun Nov 12 11:23 - 10:25  (-00:58)
etu      pts/0        fe80::9803:fff:f Sat Nov 11 09:28 - 09:28  (00:00)
reboot   system boot  6.5.0-4-amd64    Sat Nov 11 09:04 - 09:28  (00:24)
etu      pts/0        fe80::9803:fff:f Sat Nov 11 10:01 - 09:03  (-00:57)
reboot   system boot  6.5.0-3-amd64    Sat Nov 11 10:00 - 09:03  (-00:56)

wtmp commence Sun Mar 26 09:40:29 2023
</code></pre>

