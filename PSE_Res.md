# Réseau et Logiciel

#### Table des matières

* [Réseau](#réseau)
    + [Modèle OSI](#modèle-osi)
    + [Architecture TCP/IP](#architecture-tcpip)
    + [Protocoles](#protocoles)
    + [Matériels d'inter-connexion](#matériel-dinterconnexion)
* [Logiciels](#logiciels)
    + [Architecture logicielle](#architecture-logicielle)
    + [Haute diponibilité](#haute-disponibilité)
    + [Langages de présentation](#langages-de-présentation)
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
            <td>NFS</td>
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
            <td>Bluetooth-CAN bus-Ethernet Physical Layer-SMB-USB Physical Layer</td>
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

#### Couche application :

* DHCP (*Dynamic Host Configuration Protocol*) : protocole des gestion réseau utilisé sur des réseaux IP, où un serveur DHCP assigne
dynamiquement des adresses IP et autres paramètres de configuration réseaux à chaque appareil, de façon à ce qu'ils puissent
communiquer avec d'autres réseaux IP. DHCP utilise le protocole UDP. (Port 67 pour le serveur, 68 pour le client)
* DNS (*Domain Name System*) : système de nommage dynamique et hiérarchisé pour appareils, services et autres ressources connectées
* à Internet ou un réseau privé. DNS utilise UDP pour les requêtes de moins de 512 octets sinon il utilise TCP. (Port 53)
* FTP (*File Transfer Protocol*) : protocole réseau standard utilisé pour le transfert de fichiers depuis un serveur à un client.
FTP est construit sur un modèle d'architecture client-serveur en utilisant des connexions de contrôles et de données séparées entre
le client et le serveur. Les utilisateurs FTP peuvent s'authentifier eux-même à l'aide d'un protocole d'authentification en clair,
généralement sous la forme d'un nom d'utilisateur et d'un mot de passe, mais ils peuvent se connecter de manière anonyme si le
serveur est configuré en ce sens. Pour des transmission sécurisées protégeant le nom d'utilisateur et le mot de passe, et qui
encryptent le contenu, FTP est souvent sécurisé à l'aide de SSL/TLS (FTPS) ou bien remplacé par le protocole de transfert de fichier
SSH (SFTP). Le client FTP initie des connexions TCP selon différents modes. (Port 21 pour le serveur)
* HTTP (*Hypertext Transfer Protocol*) : protocole de la couche application pour systèmes d'information distribués, collaboratifs,
hypermedia. HTTP est la base de la communication de données pour le World Wide Web, où des documents hypertextes incluent des
hyperliens pour d'autres ressources que l'utilisateur peut accéder facilement, par exemple par un click utilisateur ou en tapant à
l'écran dans un navigateur web. Le client initie une connexion TCP. (Port 80 ou 8080)
* HTTPS (*Hypertext Transfer Protocol Secure*) : est une extension de HTTP. Il est utilisé pour une communication sécurisé à travers
un réseau, et très largement répandu sur Internet. En HTTPS, le protocole de communication est crypté avec la sécurité de la couche
transport (TLS) ou, précedemment la couche de sockets sécurisée (SSL). Le protocole est par conséquent désigné également par HTTP
sur TLS, ou HTTP sur SSL. (Port 443)
* IMAP (*Internet Message Access Protocol*) : est un protocole Internet standard utilisé par les clients emails pour récupérer les
messages d'un serveur de messagerie à travers une connexion TCP/IP. (Port 143 et 993 pour IMAP sur SSL/TLS)
* LDAP (*Lightwight Directory Access Protocol*) : est un protocole applicatif standard permettant d'accéder et de maintenir des
répertoires de services d'information distribués à travers un réseau IP. Les répertoires de services jouent un rôle important dans
le développement des applications intranets et Internet en permettant le partage d'informations à propos d'utilisateurs, de systèmes
, de réseaux, de services, et d'applications à travers le réseau. LDAP utilise TCP et UDP. (Port 389 et 636 pour LDAP sur SSL/TLS)
* ONC/RPC (*Open Networking Computing/Remote Procedure Call*) : est un système d'appel procedural distant.
* RIP (*Routing Information Protocol*) : est un protocole de routage IP de type vecteur s'appuyant sur l'algorithme de détermination
des routes décentralisé Bellman-Ford. Il permet à chaque routeur de communiquer aux routeurs voisins. La métrique utilisée est la
distance qui sépare un routeur d'un réseau IP déterminé quant au nombre de sauts. RIP utilise UPD. (Port 520)
* SIP (*Session Initiation Protocol*) : est un protocole de signalisation utilisé pour initier, maintenir et terminer des sessions
en temps réel, qui inclut des applications de messageries, vocales et vidéo. Les clients SIP utilisent TCP ou UDP. (Port 5060 et
5061 pour SIP sur SSL/TLS)
* SMTP (*Simple Mail Transfer Protocol*) : est un protocole de communication pour la transmission de mail. Les serveurs mails et
autres agents de transferts utilisent SMPT pour envoyer et recevoir des messages mails. Les serveurs SMTP utilisent le protocole
TCP. (Port 25)
* SSH (*Secure Shell*) : est un protocole réseau cryptographique pour des service réseaux sécurisés opérants sur des réseaux
non-sécurisés. SSH utilise une architecture client-serveur en connectant un client SSH à un serveur. SSH utilise TCP. (Port 22)
* TLS/SSL (*Transport Layer Security/Secure Sockets Layer*) : sont des protocoles cryptographiques permettant des communications
sécurisées à travers un réseau. Le protocole TLS a pour but principal de garantir le caractère privé et l'intégrité de la donnée
entre deux applications communicantes ou plus. Une connexion entre un client et un serveur doit quand elle est sécurisé par TLS
avoir une ou plusieurs des propriétés suivantes :
    + La connexion est privée (ou sécurisée) par un algorithme cryptographique symmétrique pour chiffrer les données transmises. Les
    clefs de cette encryption symmétrique sont générées de manière unique et à chaque connexion, elle sont créées à partir d'un
    secret partagé négocié au début de la session. Le serveur et le client négocient les détails de l'algorithme cryptographique
    utilisé avant que le premier octet de données soit échangé. La négociation du secret partagé est à la fois sécurisé (ne peut
    être attaqué à l'aide d'un connexion intermédiaire) et fiable (aucun attaquant ne peut modifier les communications pendant le
    processus de négociation sans être détecté).
    + L'identité des parties en communication peut être authentifiée via une clef cryptographique publique. Cette authentification
    est requise pour le serveur et optionnelle pour le client.
    + La connexion est fiable du fait que chaque message transmis inclus un message de vérification d'intégrité en utilisant un
    message de code d'authentification pour prévenir les pertes non-détectées ou l'altération des données durant la transmission.
En plus des propriétés ci-dessus, un configuration TLS peut fournir des propriétés de sécurisation supplémentaires telles que la
confidentialité persistante, assurant qu'aucune découverte future des clefs cryptographiques ne puisse être utilisée pour déchiffrer
une communication TLS enregistrée par le passé.

#### Couche transport :

* TCP (*Transmission Control Protocol*) : est un des protocoles principals de la suite des protocoles internet. Il a été développé à
l'origine dans l'implémentation réseau initiale pour complémenter le protocole internet (IP). Par conséquent, la suite entière est
communément connue sous le nom d'architecture TCP/IP. TCP fournit de flux d'octets vérifiés ordonnés et fiables entre applications
s'exécutant sur des hôtes communiquant via un réseau IP. TCP est orienté connexion, et une connexion entre client et serveur est
établie avant q'une donnée puisse être envoyée. Le serveur doit écouter (ouverture passive) les requêtes de connexion des clients
avant qu'une connexion soit établie. Un handshaking en trois temps (ouverture active), une retransmission, et une  détection
d'erreurs permet une grande fiabilité mais ajoute de la latence. Les applications qui ne requiert pas un service de flux de données
fiable peuvent utiliser le protocole datagramme utilisateur (UDP), qui fournit un service datagramme sans connexion qui priorise
le temps à la fiabilité. TCP permet d'éviter la congestion réseau. Néanmoins, le TCP est vulnérable aux attaques de déni de service,
au piratage de connexion, attaque par veto TCP et redémarrage de la connexion.
* UDP (*User Datagram Protocol*) : est un des protocoles principals de la suite des protocoles internet. UDP utilise un modèle de
communication sans connexion très simple à l'aide d'un minimum de mécanismes protocolaires. UDP fournit des sommes de vérification
pour l'intégrité des données, et des numéros de ports pour adresser différentes fonctions au niveau de la source et de la
destination du datagramme. Il ne contient pas de dialogue d'handshaking et par conséquent expose le programme utilisateur aux
problèmes éventuels de fiabilité de la connexion réseau sous-jacente ; il n'y a aucune garantie de livraison, d'ordre ni de double
protection. Si une correction d'erreur est nécessaire au niveau de l'interface réseau, une application utilisera plutôt le protocole
de contrôle de transmission (TCP) ou le protocole de transmission de contrôle de flux (SCTP) implémentés pour cet usage. UDP est
adapté aux usages où ni les contrôles ni les corrections d'erreurs ne sont nécessaires ou sont à la charge de l'application ; UDP
évite la surchage d'un tel processus dans la pile de protocole. Les applications temporellement sensibles utilisent souvent UDP
du fait qu'il est souvent préferable d'oublier des paquets plutôt que d'attendre des paquets retransmis, ce qui peut ne pas être une
option dans un système temps réel.
* DCCP (*Datagram Congestion Control Protocol*) :
* SCTP (*Stream Control Transmission Protocol*) :
* RSVP (*Ressource Reservation Protocol*) :

#### Couche internet :

* IPv4
* IPv6
* ICMP
* ICMPv6
* ECN
* IGMP
* IPsec

#### Couche Liaison :

* ARP
* NDP
* OSPF
* L2TP
* PPP
* MAC

### Matériels d'interconnexion

## Logiciels

### Architecture logicielle

### Haute disponibilité

### Langages de présentation

### Métrologie
