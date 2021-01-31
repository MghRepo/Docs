# Réseau et Logiciel

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
technologie. Le but est de garantir l'interopérabilité des divers systèmes de communication à l'aide de protocoles de
communication standards.

Le modèle sépare le flux des données dans un système de communication en sept couches abstraites, de l'implémentation physique de la
transmission de bits à travers un médium de communication jusqu'à la plus haute représentation des données d'une application
distribuée. Chaque couche intermédiaire fournit un classe de fonctions à la couche supérieure étant elle même servie par la couche
en dessous d'elle. Ces classes de fonctionnalités sont réalisées dans le logiciel par des protocoles de communication standardisés.

<table>
    <thead>
        <tr>
            <th colspan="3">Couche</th>
            <th>Unité de protocole de données (*PDU*)</th>
            <th>protocoles TCP/IP</th>
            <th>Fonctions</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <th rowspan="4">Couches Hôtes</th>
            <td>7</td>
            <td>Application</td>
            <td rowspan="3">Donnée</td>
            <td></td>
            <td>APIs de haut-niveau, partages de ressources, accès de fichiers distants</td>
        </tr>
        <tr>
            <td>6</td>
            <td>Présentation</td>
            <td>MIME-SSL-TLS-XDR</td>
            <td>Traduction de données entre services réseau et une application ; encodage, compression et encryption</td>
        </tr>
        <tr>
            <td>5</td>
            <td>Session</td>
            <td>Sockets(établissement de session TCP/RTP/PPTP)</td>
            <td>Gestion de sessions de communications, i.e., échange continu d'information sous la forme de mulptiples va-et-vient
            de transmissions entre deux noeuds</td>
        </tr>
        <tr>
            <td>4</td>
            <td>Transport</td>
            <td>Segment, Datagramme</td>
            <td>TCP-UDP-SCTP-DCCP</td>
            <td>Transmission fiables de segments de données entre points d'un réseau, incluant la segmentation, l'acquittement et le
            multiplexage</td>
        </tr>
        <tr>
            <th rowspan="3">Couches médias</th>
            <td>3</td>
            <td>Réseau</td>
            <td>Paquet</td>
            <td>IP-IPsec-ICMP-IGMP-OSPF-RIP</td>
            <td>Structurant et gérant un réseau multi-noeuds, incluant l'adressage, le routage et le contrôle du traffic</td>
        </tr>
        <tr>
            <td>2</td>
            <td>Liaison</td>
            <td>Trame</td>
            <td>PPP-SLIP</td>
            <td>Transmissions fiables de trames de données entre deux noeuds connectés par une couche physique</td>
        </tr>
        <tr>
            <td>1</td>
            <td>Physique</td>
            <td>Bit</td>
            <td></td>
            <td>Transmissions et Réceptions de flux de bits à travers un médium physique</td>
        </tr>
    </tbody>
</table>

Les protocoles de communication permettent à une entité sur un hôte d'intéragir avec une entité correspondante sur la même couche
dans un hôte différent. La définitions des services, comme le modèle OSI, décrit de manière abstraite la fonction de la couche (N-1)
pour la couche (N), ou N est une des septs couches de protocoles opérante sur l'hôte local.

A chaque niveau N, deux entités d'appareils communicants (couches N pairs) échangent des unités de protocole de données (PDUs) par
le moyen de la couche protocole N. Chaque PDU contient une charge, appelée unité de service de données (SDU), ainsi que les entêtes
et pieds reliés au protocole.

Le processus de données entre deux appareils compatibles OSI communicants est le suivant :

1. La donnée à transmettre est composée au niveau de la couche la plus haute de l'appareil transmetteur (couche *N*) dans une unité
de protocole de données.
2. Le *PDU* est passé à la couche *N-1*, ou il est reconnu comme une unité de service de données (*SDU*).
3. Au niveau de la couche *N-1* le *SDU* est concaténé avec une entête, un pied, ou les deux, produisant un *PDU de couche N-1*. Il
est alors envoyé à la couche *N-2*.
4. Le processus continue jusqu'à ce que la couche la plus basse soit atteinte, depuis laquelle la donnée est transmise à l'appareil
récepteur.
5. Au niveau de l'appareil récepteur, la donnée est passée de la couche la plus basse à la couche la plus haute comme une suite de
*SDUs* tandis quelle est pelée successivement de chaque entête ou pied de couche jusqu'à atteindre la couche la plus haute, où la
donnée restante est consommée.

### Architecture TCP/IP

La suite des protocoles internet est un modèle conceptuel et un ensemble de protocoles de communication utilisés par internet et les
réseaux informatiques similaires. Elle est connue plus communément sous le nom d'architecture **TCP/IP** du fait que les protocoles
sur lesquels elles s'appuie sont,le protocole de contrôle de transmission (TCP) et le protocole internet (IP). Son implémentation
est une pile de protocoles.

La suite des protocoles internet fournit une communication de données boût en boût en spécifiant comment la donnée doit être
enpaquetée, adressée, transmise, routée et reçue. Cette fonctionnalité est organisée en quatre couches d'abstraction, qui
classifient tous les protocoles rattachés en fonction de l'étendue de leur implication réseau. De la couche la plus basse à la
couche la plus haute :

* Liaison : contient des méthodes de communications pour des données appartennant à un unique segment réseau (ou lien).
* Internet : fournit l'interconnexion entre réseaux indépendants.
* Transport : gère la communication d'hôte à hôte.
* Application : fournit l'échange de données inter-processus pour les applications.

Les trois couches les plus hautes du modèle OSI, i.e. la couche application, présentation et session, ne sont pas distinguées
séparément dans l'architecture TCP/IP. Néanmoins il n'y a aucune contrainte sur le fait que la pile de protocoles TCP/IP impose une
architecture monolithique au dessus de la couche transport. Par exemple le protocole applicatif NFS fonctionne au dessus du
protocole de présentation de représentation externe de données (XDR), qui lui-même s'appuie sur le protocole d'appel de procedures
distant (RPC). RPC fournit une transmission fiable des enregistrements, de façon qu'il puisse utiliser le protocole de transport UDP
de manière sûre.

La fonctionnalité de la couche session peut se retrouver dans des protocoles tels que HTTP et SMTP et de manière encore plus
évidente dans Telnet et le protocole d'initialisation de session (SIP). La fonctionnalité de la couche session est également
réalisée par la numérotation de port qui appartient à la couche transport de la suite TCP/IP. Les fonctions de la couche
présentation est réalisée dans les applications TCP/IP à l'aide du standard MIME dans l'échange de données.

### Protocoles

### Matériels d'interconnexion

## Logiciels

### Architecture logicielle

### Haute disponibilité

### Langages de présentation

### Métrologie
