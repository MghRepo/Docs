#Réseau et Logiciel

#### Table des matières

* [Réseau](#réseau)
    + [Modèle OSI](#modèle-osi)
    + [Architecture TCP/IP](#architecture-tcp-ip)
    + [Protocoles](#protocoles)
    + [Matériels d'inter-connexion](#matériel-dinter-connection)
* [Logiciel](#logiciel)
    + [Architecture logicielle](#architecture-logicielle)
    + [Haute diponibilité](#haute-disponibilité)
    + [Langages de présentation](#languages-de-présentation)
    + [Métrologie](#métrologie)

## Réseau

### Modèle OSI

Le modèle OSI (*Open Systems Interconnection*) est un modèle conceptuel qui caractérise et standardise les fonctions de
communication d'un système de télécommunication ou informatique de manière détachée de la structure interne sous-jacente et de la
technologie. Le but est de garantir l'interopérabilité des divers systèmes de communications à l'aide de protocoles de
communications standards.

Le modèle sépare le flux des données dans un système de communication en sept couches abstraites, de l'implémentation physique de la
transmission de bits à travers un médium de communications jusqu'à la plus haute représentation des données d'une application
distribuée. Chaque couche intermédiaire fournit un classe de fonctions à la couche supérieure étant elle même servie par la couche
en dessous d'elle. Ces classes de fonctionnalités sont réalisées dans le logiciel par des protocoles de communications standardisés.

| Couche         |   |              | Unité de protocole de données (*PDU*) | Fonction                                             |
| -------------- | - | ------------ | ------------------------------------- | ---------------------------------------------------- |
| Couches Hôtes  | 7 | Application  | Donnée                                | APIs de haut-niveau, partages de ressources, accès   |
|                |   |              |                                       | de fichiers distants                                 |
|                | 6 | Présentation |                                       | Traduction de données entre services réseau et une   |
|                |   |              |                                       | application ; encodage, compression et encryption    |
|                |   |              |                                       | Gestion de sessions de communications, i.e., échange |
|                | 5 | Session      |                                       | continu d'information sous la forme de multiples     |
|                |   |              |                                       | va-et-vient de transmissions entre deux noeuds       |
|                |   |              |                                       | Transmissions fiables de segments de données entre   |
|                | 4 | Transport    | Segment, Datagramme                   | points d'un réseau, incluant la segmentation,        |
|                |   |              |                                       | l'acquitemment et le multiplexage                    |
| Couches médias | 3 | Réseau       | Paquet                                | Structurant et gérant un réseau multi-noeuds,        |
|                |   |              |                                       | incluant l'adressage, le routage, le contrôle du     |
|                |   |              |                                       | traffic                                              |
|                | 2 | Liaison      | Trame                                 | Transmissions fiables de trames de données entre     |
|                |   |              |                                       | deux noeuds connectés par une couche physique        |
|                | 1 | Physique     | Bit                                   | Transmissions et réceptions de flux de bits à        |
|                |   |              |                                       | travers un médium physique                           |
| -------------- | - | ------------ | ------------------------------------- | ---------------------------------------------------- |

### Architecture TCP/IP

### Protocoles

### Matériels d'interconnexion

## Logiciels

### Architecture logicielle

### Haute disponibilité

### Langages de présentation

### Métrologie
