# Internet of Things - Groupe infrastructure
Dans le cadre du cours IOT à la HEIG-VD, Suisse.

## Introduction
Le groupe infrastructure est responsable de mettre en place une solution permettant à des capteurs de communiquer par réseau LoRaWAN avec une application web développée par les groupes front-end et back-end.

Pour cela, il a été décidé de mettre en place notre propre infrastructure LoRaWAN afin de pouvoir héberger l'application web sur un serveur LoRaWAN que nous allons installer sur une machine se trouvant dans le réseau interne de l'école. Une base de données PostgreSQL tourne sur le serveur LoRaWAN pour stocker des données de configuration relatives au protocole LoRaWAN.

L'objectif est donc de mettre en place une infrastructure réseau en installant un network-server LoRaWAN et ses différents composants afin que les capteurs LoRa et l'application web puissent communiquer entre eux.

### Architecture LoRaWAN simplifiée

![Architecture LoRaWAN simplifiée](images/architecture_LoRaWAN_simplifiee.jpg)

### Architecture LoRaWAN détaillée

![Architecture LoRaWAN détaillée](images/architecture_detaillee.png)


## Technologies utilisées

Pour commencer à mettre en place notre infrastructure réseau, nous avons choisi d'utiliser un network-server LoRaWAN open-source mis à disposition par CableLabs sur le site [loraserver.io](www.loraserver.io).

Le network-server est formé de plusieurs composants que nous allons décrire ci-dessous.

![Network-server architecture](images/network-server_architecture.png)


### Devices LoRa

Les devices LoRa sont dans notre cas les capteurs qui envoient des données au réseau LoRaWAN en passant par les LoRa gateways.

### LoRa Gateway

Antenne LoRa qui est connectée au module de routage. Elle envoie les transmissions radio LoRa qu'elle reçoit des capteurs au network-server. Elle implémente un packet-forwarder permettant d'interfacer le hardware LoRa.

### LoRa Gateway Bridge

Le LoRa Gateway Bridge est responsable de la communication avec la gateway. Il transforme les paquets tranportés sur le protocole UDP en payloads JSON sur le protocole MQTT.


### Router

Ce module est chargé de gérer le statut de la gateway et d'organiser les transmissions (scheduling) car une gateway peut effetuer uniquement une transmission à la fois. Il est connecté à un ou plusieurs brokers.

### Broker

Le broker est la partie centrale du réseau. Il est responsable du mapping entre les devices LoRa et la ou les applications tournant sur le serveur d'applications. Son rôle est de rediriger un message en provenance d'un certain capteur à la bonne application et dans le cas du chemin inverse de distribuer le message au bon routeur qui va communiquer avec une gateway qui va elle à son tour relayer le message par transmissions radio LoRa au capteur destinataire.

A noter qu'il peut y avoir plusieurs brokers au sein du network-server.

### Network-server

Le network-server est responsable des fonctionnalités spécifiques à LoRaWAN. Il a pour tâche de contrôler chaque device et chaque application enregistrée. Pour ce faire, il demande au serveur d'applications si le node (device) est autorisé dans le réseau et, si oui, quels paramètres il faut appliquer à ce node.

### Handler

Le handler doit gérer les données relatives à une ou plusieurs applications. Pour ce faire, il se connecte à un broker où sont enregistrés les différents capteurs et applications ainsi que le mapping entre eux.

C'est également dans le handler que les données sont cryptées ou décryptées.

### Gateway Protocol Translation

Quand une gateway reçoit un message transmis avec LoRa, elle encapsule et redirige le message au réseau LoRaWAN. Beaucoup de gateways utilisent le même protocole de référence proposé par LoRa mais d'autres protocoles ont été développés pour des backends spécifiques.

### Downlink configuration

Avec LoRaWAN, le temps de réponse du downlink, c'est-à-dire le message allant de l'application au capteur, est dépendant de la localisation géographique de la gateway.

Le module de routage est chargé de gérer les informations relatives aux gateways existantes dont leur position géographique et doit déterminer comment retourner une réponse au capteur.

Après chaque requête qu'un capteur envoie à l'application (message uplink), le router créé deux fenêtres de réponse. La première a lieu exactement 1 seconde après avoir reçu le message du capteur et la deuxième 2 secondes après avoir reçu le message. Donc pour chaque requête reçue sur chaque gateway, le router construit deux downlink configurations.

Afin de pouvoir sélectionner la meilleure option de gateway par laquelle envoyer la réponse au capteur, le router calcule un score pour chaque downlink configuration. Le score pour chaque configuration est influencé par plusieurs facteurs tels que le temps de propagation dans l'air, la force du signal, le taux d'utilisation de la gateway et les réponses déjà planifiées par le router (scheduling). Par exemple plus le signal est fort, meilleure est la qualité de la transmission et plus la gateway est utilisée, plus celle-ci doit probablement être fiable.

### Déduplication

LoRaWAN est un protocole radio à longue portée, ce qui signifie qu'il est plus que probable que plusieurs gateways reçoivent et retransmettent un message envoyé par un device LoRa, ce qui génère une déduplication du message envoyé. Le broker est chargé de gérer les messages reçus afin de filtrer les messages dupliqués et de n'envoyer qu'une seule fois le message à l'application.

Les messages dupliqués peuvent quand même s'avérer utiles. Les métadonnées de ces messages peuvent être analysées pour trouver la position exacte du device LoRa qui a envoyé la requête et aussi pour mettre à jour les données permettant au module de routage d'améliorer le calcul du score des downlink configurations de manière à rediriger les réponses (messages downlink) aux gateways de façon optimale.

### LoRa App Server

Le LoRa App Server est comme son nom l'indique un serveur d'applications. Il est open-source et est responsable de faire l'inventaire des devices communicant avec l'infrastructure LoRaWAN. Il gère les demandes d'accès des devices aux applications et la réception des payloads de l'application.

LoRa App Server dispose d'une interface web permettant à l'administrateur de gérer les utilisateurs, les devices et les applications autorisés à passer par l'infrastucture du réseau LoRaWAN. Grâce aux APIs RESTful et gRPC, il est possible de faire de l'intégration de services externes.

Lors des transmissions entre les applications et les devices LoRa, les données sont envoyées et reçues en utilisant les protocoles MQTT et/ou HTTP(S).


#### REST API pour le LoRa App Server

![REST API pour le LoRa App Server](images/LoRaAppServer_REST_API.png)

#### Exemple d'applications ajoutées

![Exemple d'applications ajoutées](images/exemple_applications.png)









## Spécificités
[Spécificités à savoir pour la partie du projet (possibilités de configuration)]

## Déploiement
Dans ce chapitre, nous allons voir comment est déployée l'infrastructure

## Conclusion
[Points à améliorer, points en suspens, améliorations futures, ...]

## Documentation supplémentaire

- [loraserver.io](www.loraserver.io) website
- [Network documentation](https://www.thethingsnetwork.org/docs/network/) on TTN website