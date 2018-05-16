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
#### Gateway : 
* Raspberry Pi 3 B
* RASPBIAN STRETCH LITE 4.14 (pas de bureau graphique)
* SD card 4GB
* connexion à un écran: HDMI
* connexion à la console Raspberry via un câble série. L'émulateur de terminal (PuTTY par exemple) doit spécifier le port 115200 et l'hôte COM qui correspond à celui configuré pour le périphérique USB de la machine qui émule.
* connexion internet: Ethernet (réception adresse du pool DHCP de l'école)
* carte Lora: iC880A-SPI


## Déploiement
Dans ce chapitre, nous allons voir comment a été déployée l'infrastructure et comment installer et accéder aux différents composants du système.

### Accès à la VM sur le serveur de l'école
ssh -l heiguser 10.192.72.26  
ou 
ssh -l heiguser iot_lora.lan.iict.ch

mot de passe groupe-infra-pass


Votre navigateur vous dira qu'il ne fait pas confiance au certificat du serveur ==> Il faut ajouter une exception

On a un accès root sur cette machine, IL NE FAUT SURTOUT PAS CHANGER LES CREDENTIALS DU COMPTE ROOT 



J'ai du donc installer docker-ce depuis un autre repo officiel (nightly) mais qui change pas mal (j'ai pris le package du 04 mai 2018) on devra donc surveiller ça et se rappeler de faire des updates de docker car si jamais on a des problèmes ça sera à cause de ça. 


nouveau mot de passe admin pour l'interface graphique du serveur 
groupe-infra-pass-interface

### Mise en place de la VM sur le serveur de l'école (topologie Docker)

### Accès à la gateway

### Installation de la gateway

## Conclusion
[Points à améliorer, points en suspens, améliorations futures, ...]

