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

<table>
    <thead>
        <tr>
            <th colspan="3">Couche</th>
            <th>Unité de protocole de données (*PDU*)</th>
            <th>Fonctions</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <th rowspan="4">Couches Hôtes</th>
            <td>7</td>
            <td>Application</td>
            <td rowspan="3">Donnée</td>
            <td>APIs de haut-niveau, partages de ressources, accès de fichiers distants</td>
        </tr>
        <tr>
            <td>6</td>
            <td>Présentation</td>
            <td>Traduction de données entre services réseau et une application ; encodage, compression et encryption</td>
        </tr>
        <tr>
            <td>5</td>
            <td>Session</td>
            <td>Gestion de sessions de communications, i.e., échange continu d'information sous la forme de mulptiples va-et-vient
            de transmissions entre deux noeuds</td>
        </tr>
        <tr>
            <td>4</td>
            <td>Transport</td>
            <td>Segment, Datagramme</td>
            <td>Transmission fiables de segments de données entre points d'un réseau, incluant la segmentation, l'acquittement et le
            multiplexage</td>
        </tr>
        <tr>
            <th rowspan="3">Couches médias</th>
            <td>3</td>
            <td>Réseau</td>
            <td>Paquet</td>
            <td>Structurant et gérant un réseau multi-noeuds, incluant l'adressage, le routage et le contrôle du traffic</td>
        </tr>
        <tr>
            <td>2</td>
            <td>Liaison</td>
            <td>Trame</td>
            <td>Transmissions fiables de trames de données entre deux noeuds connectés par une couche physique</td>
        </tr>
        <tr>
            <td>1</td>
            <td>Physique</td>
            <td>Bit</td>
            <td>Transmissions et Réceptions de flux de bits à travers un médium physique</td>
        </tr>
    </tbody>
</table>

### Architecture TCP/IP

### Protocoles

### Matériels d'interconnexion

## Logiciels

### Architecture logicielle

### Haute disponibilité

### Langages de présentation

### Métrologie
