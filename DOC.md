# TP 3 B1 Plusieurs réseaux : routage static

## 1. Création

### Configuration réseau d'une machine CentOS

A ) Pour prouver que ma machine virtuelle à internet je demande à CentOS de m'affigher le code source de la page www.google.fr avec la commande :

    curl google.fr

Et j'obtiens bien le code source de ma page :

    <HTML><HEAD><meta http-equiv="content-type" 	content="text/html;charset=utf-8">
    <TITLE>301 Moved</TITLE></HEAD><BODY>
    <H1>301 Moved</H1>
    The document has moved
    <A HREF="http://www.google.fr/">here</A>.
    </BODY></HTML>


B) Ma machine Hôte et ma VM peuvent communiquer avec un ping :

ping VM -> hote:

    PING 192.168.43.192 (192.168.43.192) 56(84) bytes of data.
    64 bytes from 192.168.43.192: icmp_seq=1 ttl=127 time=0.489 ms
    64 bytes from 192.168.43.192: icmp_seq=2 ttl=127 time=0.596 ms
    64 bytes from 192.168.43.192: icmp_seq=3 ttl=127 time=1.05 ms
    64 bytes from 192.168.43.192: icmp_seq=4 ttl=127 time=0.841 ms
    64 bytes from 192.168.43.192: icmp_seq=5 ttl=127 time=1.32 ms
    ^C
    --- 192.168.43.192 ping statistics ---
    5 packets transmitted, 5 received, 0% packet loss, time 4002ms
    rtt min/avg/max/mdev = 0.489/0.861/1.324/0.303 ms


ping hote -> VM:

    Envoi d’une requête 'Ping'  192.168.102.10 avec 32 octets de données :
    Réponse de 192.168.102.10 : octets=32 temps<1ms TTL=64
    Réponse de 192.168.102.10 : octets=32 temps<1ms TTL=64
    Réponse de 192.168.102.10 : octets=32 temps<1ms TTL=64
    Réponse de 192.168.102.10 : octets=32 temps<1ms TTL=64
    
    Statistiques Ping pour 192.168.102.10:
        Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
    Durée approximative des boucles en millisecondes :
        Minimum = 0ms, Maximum = 0ms, Moyenne = 0ms

C) Je peux aussi afficher la table de routage avec :

    ip route

Voici ce que la console affiche:

    default via 10.0.2.2 dev enp0s3 proto dhcp metric 100
    10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.15 metric 100
    192.168.102.0/24 dev enp0s8 proto kernel scope link src 192.168.102.10 metric 101

en lisant de bas en haut, peut remarquer que la route d'un packet pour aller de la VM jusqu'à internet est 

    192.168.127.10/24 (machine)
	-->
	10.0.2.0/24 (IP réseau Ynov)
	-->
	10.0.2.2/24 (IP gateway Ynov)

### Faire joujou avec quelques commandes

#### Ping 
Comme démontré plus haut, nos deux machine peuvent bien communiquer avec un ping 

Hôte à VM :

    Envoi d’une requête 'Ping'  192.168.102.10 avec 32 octets de données :
    Réponse de 192.168.102.10 : octets=32 temps<1ms TTL=64
    Réponse de 192.168.102.10 : octets=32 temps<1ms TTL=64
    Réponse de 192.168.102.10 : octets=32 temps<1ms TTL=64
    Réponse de 192.168.102.10 : octets=32 temps<1ms TTL=64
    
    Statistiques Ping pour 192.168.102.10:
        Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
    Durée approximative des boucles en millisecondes :
        Minimum = 0ms, Maximum = 0ms, Moyenne = 0ms

VM à Hôte:

    PING 192.168.43.192 (192.168.43.192) 56(84) bytes of data.
        64 bytes from 192.168.43.192: icmp_seq=1 ttl=127 time=0.489 ms
        64 bytes from 192.168.43.192: icmp_seq=2 ttl=127 time=0.596 ms
        64 bytes from 192.168.43.192: icmp_seq=3 ttl=127 time=1.05 ms
        64 bytes from 192.168.43.192: icmp_seq=4 ttl=127 time=0.841 ms
        64 bytes from 192.168.43.192: icmp_seq=5 ttl=127 time=1.32 ms
        ^C
        --- 192.168.43.192 ping statistics ---
        5 packets transmitted, 5 received, 0% packet loss, time 4002ms
        rtt min/avg/max/mdev = 0.489/0.861/1.324/0.303 ms

#### Table de routage 

Hôte :

IPv4 Table de routage

    ===========================================================================
    Itinéraires actifs :
    Destination réseau    Masque réseau  Adr. passerelle   Adr. interface Métrique
              0.0.0.0          0.0.0.0     192.168.43.1   192.168.43.192     55
            127.0.0.0        255.0.0.0         On-link         127.0.0.1    331
            127.0.0.1  255.255.255.255         On-link         127.0.0.1    331
      127.255.255.255  255.255.255.255         On-link         127.0.0.1    331
         192.168.43.0    255.255.255.0         On-link    192.168.43.192    311
       192.168.43.192  255.255.255.255         On-link    192.168.43.192    311
       192.168.43.255  255.255.255.255         On-link    192.168.43.192    311
         192.168.56.0    255.255.255.0         On-link      192.168.56.1    281
         192.168.56.1  255.255.255.255         On-link      192.168.56.1    281
       192.168.56.255  255.255.255.255         On-link      192.168.56.1    281
        192.168.102.0    255.255.255.0         On-link     192.168.102.2    330
        192.168.102.2  255.255.255.255         On-link     192.168.102.2    330
      192.168.102.255  255.255.255.255         On-link     192.168.102.2    330
            224.0.0.0        240.0.0.0         On-link         127.0.0.1    331
            224.0.0.0        240.0.0.0         On-link      192.168.56.1    281
            224.0.0.0        240.0.0.0         On-link    192.168.43.192    311
            224.0.0.0        240.0.0.0         On-link     192.168.102.2    330
      255.255.255.255  255.255.255.255         On-link         127.0.0.1    331
      255.255.255.255  255.255.255.255         On-link      192.168.56.1    281
      255.255.255.255  255.255.255.255         On-link    192.168.43.192    311
      255.255.255.255  255.255.255.255         On-link     192.168.102.2    330
    ===========================================================================

on peut remarquer que le réseau qui permet la communication avec la vm est :

    192.168.102.0/24

VM:

    default via 10.0.2.2 dev enp0s3 proto dhcp metric 100
    10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.15 metric 100
    192.168.102.0/24 dev enp0s8 proto kernel scope link src 192.168.102.10 metric 101


on peut remarquer que ce même réseau apparaît dans le `ip route` .

#### Curl google

    <HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
    <TITLE>301 Moved</TITLE></HEAD><BODY>
    <H1>301 Moved</H1>
    The document has moved
    <A HREF="http://www.google.fr/">here</A>.
    </BODY></HTML>


ici on peut voir le code HTML de la page index.html de google.fr avec :

    curl google.fr

#### Diag 

Pour avoir l'IP de Ynov.fr:

    dig ynov.fr
   
----

     ; <<>> DiG 9.9.4-RedHat-9.9.4-72.el7 <<>> ynov.com
    ;; global options: +cmd
    ;; Got answer:
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 31324
    ;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1
    
    ;; OPT PSEUDOSECTION:
    ; EDNS: version: 0, flags:; udp: 4096
    ;; QUESTION SECTION:
    ;ynov.com.                      IN      A
    
    ;; ANSWER SECTION:
    ynov.com.               10800   IN      A       217.70.184.38
    
    ;; Query time: 43 msec
    ;; SERVER: 192.168.43.1#53(192.168.43.1)
    ;; WHEN: lun. janv. 21 21:11:06 CET 2019
    ;; MSG SIZE  rcvd: 53


On peut remarquer que l'IP de Ynov est : `217.70.184.38`

Pour avoir l'IP de Google.fr:

    dig google.fr

---

    ; <<>> DiG 9.9.4-RedHat-9.9.4-72.el7 <<>> google.fr
    ;; global options: +cmd
    ;; Got answer:
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 28472
    ;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1
    
    ;; OPT PSEUDOSECTION:
    ; EDNS: version: 0, flags:; udp: 4096
    ;; QUESTION SECTION:
    ;google.fr.                     IN      A
    
    ;; ANSWER SECTION:
    google.fr.              185     IN      A       216.58.198.195
    
    ;; Query time: 41 msec
    ;; SERVER: 192.168.43.1#53(192.168.43.1)
    ;; WHEN: lun. janv. 21 21:06:37 CET 2019
    ;; MSG SIZE  rcvd: 54


On peut remarquer que l'IP de Google est : `216.58.198.195`


## 2. Notion de ports et SSH:

### Exploration des ports locaux

Pour lister les ports en écoute, seulement sur les IPv4, en affichant les information TCP il faut utiliser la commande :

    ss -4npli

la console affiche donc :

    Netid  State      Recv-Q Send-Q Local Address:Port               Peer Address:Port
    udp    UNCONN     0      0        *:68                   *:*                   users:(("dhclient",pid=2879,fd=6))
    tcp    LISTEN     0      128      *:22                   *:*                   users:(("sshd",pid=3070,fd=3))
             cubic rto:1000 mss:536 cwnd:10 segs_in:2 lastsnd:3621152 lastrcv:3621152 lastack:3621152
    tcp    LISTEN     0      100    127.0.0.1:25                   *:*                   users:(("master",pid=3308,fd=13))
             cubic rto:1000 mss:536 cwnd:10 lastsnd:3621152 lastrcv:3621152 lastack:3621152

### SSH

La VM dispose de la version 7 de CentOS, donc il y a déjà un serveur SSH installé dessus, arrivé das putty il suffit juste d'entrer l'IP locale de la VM, 192.168.102.2 dans putty et de se connecter :

<img src="putty.PNG" >

### Firewall

1- dans la console : `nano /etc/ssh/sshd_config`

2- décommenter la ligne avec le choix du port est le mettre à plus de `1024`:

    #   IdentityFile ~/.ssh/id_ecdsa
    #   IdentityFile ~/.ssh/id_ed25519
        Port 2222
    #   Protocol 2
    #   Cipher 3des

La connexion est refusé car le port 2222 n'est pas un port que le pare feu de CentOS, ni de Windows accepte. Donc pour régler le problème il suffit juste d'ouvrir les ports sur le pare feu de l’Hôte et de la VM.

### Netcat

Après avoir lancé le serveur Netcat, on peut remarquer qu'il écoute désormais sur le port 5454.

    tcp LISTEN 0 10 *:5454 *:* users:(("nc",pid=11359,fd=4))

Mais on dois aussi ouvrir le port 5454 dans le firewall de CentOS avec les commandes suivantes :

1- on ajoute le port dans les règles du pare-feu : `firewall-cmd --zone=public --add-port=5454/tcp --permanent`

2- on redémarre le pare-feu pour prendre en compte les changements : `firewall-cmd --reload`

## 3. Le Routage

IP des PCs : 

PC-Marc: `192.168.43.192/24`
PC-Louis: `192.168.43.191/24`

IP des VMs : 

VM-Marc: `192.168.102.1/24`
VM-Louis: `192.168.102.2/24`

Après branchement par câble Ethernet et configuration des machines voici le résultat des pings entre les deux PCs:

*Ping PC-Marc -> PC-Louis* : `ping 192.168.43.191/24`

    Envoi d’une requête 'Ping'  192.168.43.191 avec 32 octets de données :
    Réponse de 192.168.43.191 : octets=32 temps=3ms TTL=64
    Réponse de 192.168.43.191 : octets=32 temps=3ms TTL=64
    Réponse de 192.168.43.191 : octets=32 temps=3ms TTL=64
    Réponse de 192.168.43.191 : octets=32 temps=3ms TTL=64
    
    Statistiques Ping pour 192.168.43.191:
        Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
    Durée approximative des boucles en millisecondes :
        Minimum = 0ms, Maximum = 0ms, Moyenne = 0ms

*Ping PC-Louis -> PC-Marc* : `ping 192.168.43.192/24`

    Envoi d’une requête 'Ping'  192.168.43.192 avec 32 octets de données :
    Réponse de 192.168.43.192 : octets=32 temps=4ms TTL=64
    Réponse de 192.168.43.192 : octets=32 temps=4ms TTL=64
    Réponse de 192.168.43.192 : octets=32 temps=4ms TTL=64
    Réponse de 192.168.43.192 : octets=32 temps=4ms TTL=64
    
    Statistiques Ping pour 192.168.43.192:
        Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
    Durée approximative des boucles en millisecondes :
        Minimum = 0ms, Maximum = 0ms, Moyenne = 0ms


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTE5OTM4NjU3MSwtNjQxOTUxMTQ0LC0yMD
AxNzg0ODYzLDE5MDM5MTMxMzgsLTEyMDU3MzI4MjAsLTE0NTgw
NDI2MjQsMjA2Nzc1Mzg4MCwxMDc5NTkxMzcyLDg5MjU4OTc0MS
wxMzg1Njc0ODgzLDM3OTEzODMzMCwtODg0NTUzNzA2LDEwOTI5
MzEyMDAsMTY2ODkyNjkwNSwtNDMxODI1NTMxLC0yMDg4NzQ2Nj
EyLC0xNjUwNjM1NjY5XX0=
-->