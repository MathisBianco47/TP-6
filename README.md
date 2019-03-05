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
