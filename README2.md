# Internet of Things - Groupe infrastructure
Dans le cadre du cours IOT à la HEIG-VD, Suisse.

## Introduction
[Introduction à ce repository]
Par exemple: Ce repository est consacré à la partie...

## Technologies utilisées
[Technologies utilisées]

## Spécificités

Il faut être connecté au réseau de l'école ou utiliser un VPN pour pouvoir atteindre les différents composants du système.

#### VM sur le serveur de l'école :
* Ubuntu 18.04 LTS bionic beaver 
* 50GB de mémoire
* 2GB de RAM
* CPU DualCore
* Adresse IP:  10.192.72.26  
* Nom de domaine: iot_lora.lan.iict.ch
* docker: installation de docker-ce, package du 04 mai 2018
#### Gateway : 
* Raspberry Pi 3 B
* RASPBIAN STRETCH LITE 4.14 (pas de bureau graphique)
* SD card 4GB
* connexion à un écran: HDMI
* connexion à la console Raspberry via un câble série. L'émulateur de terminal (PuTTY par exemple) 
* connexion internet: Ethernet (réception adresse du pool DHCP de l'école)
* carte Lora: iC880A-SPI


## Déploiement
Dans ce chapitre, nous allons voir comment a été déployée l'infrastructure et comment installer et accéder aux différents composants du système.

### Accès à la VM sur le serveur de l'école
Pour accéder à la VM sur le serveur de l'école, il faut tout d'abord se connecter au réseau de l'école puis ouvrir un terminal et taper une des deux commandes suivantes:


##### ssh -l heiguser 10.192.72.26  


##### ssh -l heiguser iot_lora.lan.iict.ch

Remarque : il est possible qu'à ce stade, un message d'erreur apparaisse en indiquant qu'il n'est pas possible d'ajouter l'hôte à la liste des hôtes connus. Ignorez et tapez "yes" pour continuer.

Un mot de passe est ensuite demandé pour établir la connexion à distance. Tapez le mot de passe suivant pour accéder au compte "heiguser" (compte root):

##### groupe-infra-pass

### Accès à l'interface graphique du LoRa Server
Ouvrez votre navigateur et accéder au lien suivant:

<https://iot_lora.lan.iict.ch:8080>


Votre navigateur vous dira qu'il ne fait pas confiance au certificat du serveur : ajoutez une exception pour continuer.

Il faut ensuite saisir l'utilisateur "admin" et le mot de passe "groupe-infra-pass-interface".


### Lancement de la topologie LoRa depuis la VM
Dans le cas où la topologie docker ne serait pas lancée sur le serveur (impossibilité de se connecter à l'interface graphique du LoRa Server), il faut accéder à la VM via un terminal comme expliqué plus haut, au compte "heiguser" puis au dossier /home/heiguser/home/Documents/repos/loraserver-docker.
Tapez la commande suivante pour démarrer le LoRa Server:

##### docker-compose up 

### Accès à la gateway
Pour accéder à la gateway, il y a deux possibilités: soit on veut accéder à distance via l'interface graphique du LoRa Server, soit directement via un câble série ou HDMI.

Quelle que soit la méthode de connexion, le user et le mot de passe sont respectivement "pi" et "raspberry".

####  Câble HDMI
La connexion est aisée puisqu'il suffit simplement de connecter un câble HDMI au Raspberry faisant office de gateway et de le relier à un écran pour le output. Si le Raspberry est en train de tourner, l'image l'interface s'affiche automatiquement à l'écran.

#### Câble série
Avec le câble série, il faut être attentif au branchement suivant sur la gateway:
![Branchement câble série](./images/raspberry_serie.png)

Branchez ensuite la partie USB sur votre machine et à l'aide d'un émulateur de terminal (PuTTY par exemple), spécifiez une connexion série avec un port 115200 et l'hôte COM qui correspond à celui configuré pour le périphérique USB de votre machine (peut être configuré sur le gestionnaire de périphériques).

#### Depuis le LoRa Server
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
### Installation de la gateway
Ce chapitre explique comment mettre en place une gateway à partir d'un Raspberry Pi modèle 2B.

#### installation de l'OS Raspbian
La première étape consiste à installer un système d'exploitation sur la carte SD du Raspberry. 
L'OS choisit est décrit plus haut dans les spécificités.
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
#### Mosquito
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!

## Conclusion
[Points à améliorer, points en suspens, améliorations futures, ...]
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!

