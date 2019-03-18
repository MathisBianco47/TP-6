# B1 Réseau 2018 - TP6

**Déroulement et rendu du TP**

vous aurez besoin de :

+ Virtualbox
+ GNS3

les machines virtuelles Linux :

+ l'OS devra être CentOS 7 (en version minimale)
+ pas d'interface graphique (que de la ligne de commande)

les routeurs Cisco :

+ l'iOS devra être celui d'un Cisco 3640

les switches :

+ devront être de bêtes "Ethernet Switch" dans GNS3
+ peut-être qu'on utilisera de l'iOU si jamais certains d'entre vous avancent vite (switch Cisco)
il y a beaucoup de ligne de commande dans ce TP, préférez les copier/coller aux screens

**1. Aires OSPF**

OSPF met en place des area ou aires. C'est principalement utilisé pour faire scale l'infrastructure (augmenter sa taille) sans flood le réseau

Nous allons nous servir des aires OSPF pour distinguer :

une aire "backbone"
c'est l'area 0
toutes les autres y sont connectées
donc tout trafic qui change d'aire passe forcément par celle-ci
le WAN (~= internet) est souvent accessible depuis l'aire "backbone"
une aire pour les clients
ce sera l'area 1
X clients seront ajoutés
leur adressage IP et leur table de routage seront gérées automatiquement
présence d'un serveur DHCP
une aire pour les services d'infrastructures
ce sera l'area 2
dans un cas réel on peut trouver tout un tas de trucs ici, beaucoup de services internes
nous on aura un p'tit serveur web ou un netcat simple, pour simuler un service disponible

**2.Réseaux IP et aires OSPF**

**2. Mise en place du lab**

 mise en place de mon router1

+ Routeur1# `conf t`
+ Routeur1(config)# interface eth0/0
+ Routeur1(config-if)# ip address `10.6.202.254 255.255.255.0`
+ Routeur1(config-if)# no shut
+ Routeur1(config-if)# exit

+ Routeur1(config)# interface eth0/1
+ Routeur1(config-if)# ip address 10.6.100.1 255.255.255.252
+ Routeur1(config-if)# no shut
+ Routeur1(config-if)# exit

+ Routeur1(config)# interface eth0/2
+ Routeur1(config-if)# ip address 10.6.100.5 255.255.255.252
+ Routeur1(config-if)# no shut
+ Routeur1(config-if)# exit
+ Routeur1(config)# exit

**les IPs attribuées aux interfaces :**

Routeur1# `show ip int br`

**définir leurs noms de domaine**

Routeur1# `conf t`

**Checklist VMs**

j'utilise: $ `sudo nano /etc/sysconfig/network-scripts/ifcfg-enp0s3`

**je relance l'interface**

`$ sudo ifdown enp0s3
 $ sudo ifup enp0s3`
 
 + modifier leurs noms
 
 $ echo... sudo tee /etc/hostname


**LE PING FONCTIONNE**



**Passons à la configuration de l'OSPF**

+ Pour activer OSPF

`routeur1.tp6.b1# conf t
 routeur1.tp6.b1(config)# router ospf 1`


Pour configurer l'id-router, je procède comme sur le tp 5

**Lab 3 : Let's end this properly**

1. NAT : accès internet

Utilisation de GNS3

Configuration de mon #Routeur 4

`$ ping 8.8.8.8 fonctionnent`

je modifie mon fichier avec `modifier mon fichier resolv.conf : $ sudo nano /etc/resolv.conf `, `nameserver 8.8.8.8.`

manip
`show ip` route pour voir
`ping 8.8.8.8`
`curl google.com`
faire une requête Web à la main vers un serveur web full HTTP (pas de HTTPS)

**2. Un service d'infra**

je peux lancer alors le serveur avec  `$ sudo systemctl start nginx`

Ma machine `client1.tp6.b1` le curl vers server1.tp6.b1 fonctionne, il reçoit l'html.


**4. Serveur DNS**

Les ports firewall

+ $ sudo firewall-cmd --add-port=53/tcp --permanent

+ $ sudo firewall-cmd --add-port=53/udp --permanent


démarrage du DNS

+ $ sudo systemctl start named
+ $ sudo systemctl enable named


