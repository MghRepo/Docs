# Système et bases de données

#### Table des matières

* [Système](#système)
    + [Multitâche coopératif](#multitâche-coopératif)
    + [Multitâche préemptif](#multitâche-préemptif)
    + [Algorithmes d'ordonnancement](#algorithmes-dordonnancement)
    + [Synchronisation](#synchronisation)
    + [Signaux](#signaux)
    + [Socket réseau](#socket-réseau)
    + [Socket IPC](#socket-ipc)
    + [Socket Netlink](#socket-netlink)
    + [Tube anonyme](#tube-anonyme)
    + [Tube nommé](#tube-nommé)
    + [Passage de messages](#passage-de-messages)
    + [Fichier mappé en mémoire](#fichier-mappé-en-mémoire)
    + [Partitionnement de la mémoire](#partitionnement-de-la-mémoire)
    + [MBR](#mbr)
    + [GPT](#gpt)
    + [Mémoire virtuelle](#mémoire-virtuelle)
    + [Pagination](#pagination)
    + [Segmentation](#segmentation)
    + [Sysfs](#sysfs)
    + [Udev](#udev)
    + [Bus](#bus)
    + [Accès direct à la mémoire](#accès-direct-à-la-mémoire)
    + [Pilotes](#pilotes)
    + [BIOS](#bios)
    + [UEFI](#uefi)
    + [Shell](#shell)
    + [Cgroups](#cgroups)
    + [Espaces de noms](#espaces-de-noms)
    + [Systemd-nspawn](#systemd-nspawn)
    + [Conteneurisation LXC](#conteneurisation-lxc)
    + [Conteneurisation Docker](#conteneurisation-docker)
    + [Orchestrateur Kubernetes](#orchestrateur-kubernetes)
    + [Libvirt](#libvirt)
    + [Hyperviseurs](#hyperviseurs)
    + [Netfilter](#netfilter)
    + [SELinux](#seLinux)
* [Bases de données](#bases-de-données)
    + [Algèbre relationnelle](#algèbre-relationnelle)
    + [Langage SQL](#langage-sql)
    + [Administration SGBD](#administration-sgbd)

## Système

### Multitâche coopératif

Le multitâche coopératif est une forme simple de multitâche où chaque tâche doit explicitement permettre aux autres tâches de
s'exécuter. Cette approche simplifie l'architecture du système mais présente plusieurs inconvénients :

* Le multitâche coopératif est une forme de couplage fort. Si un des processus ne redonne pas la main à un autre processus, par
exemple si le processus est buggé, le système entier peut s'arrêter.
* Le partage de ressources (temps CPU, mémoire, accès disque, etc.) peut être inefficace.

### Multitâche préemptif

Le multitâche préemptif désigne la capacité d'un système d'exploitation à exécuter ou arrêter une tâche planifiée en cours.

Un ordonnanceur préemptif présente l'avantage d'une meilleure réactivité du système et de son évolution, mais l'inconvénient
vient des situations de compétition (lorsque le processus d'exécution accède à la même ressource avant qu'un autre processus
(préempté) ait terminé son utilisation).

Dans un système d'exploitation multitâche préemptif, les processus ne sont pas autorisés à prendre un temps non défini pour
s'exécuter dans le processeur. Une quantité de temps définie est attribuée à chaque processus ; si la tâche n'est pas accomplie
avant la limite fixée, le processus est renvoyé dans la pile pour laisser place au processus suivant dans la file d'attente, qui
est alors exécuté par le processeur. Ce droit de préemption peut tout aussi bien survenir avec des interruptions matérielles.

Certaines tâches peuvent être affectées d'une priorité ; une tâche pouvant être spécifiée comme "préemptible" ou "non
préemptible". Une tâche préemptible peut être suspendue (mise à l'état "ready") au profit d'une tâche de priorité plus élevée ou
d'une interruption. Une tâche non préemptible ne peut être suspendue qu'au profit d'une interruption. Le temps qui lui est
accordé est plus long, et l'attente dans la file d'attente plus courte.

Au fur et à mesure de l'évolution des systèmes d'exploitation, les concepteurs ont quitté la logique binaire "préemptible/non
préemptible" au profit de systèmes plus fins de priorités multiples. Le principe est conservé, mais les priorités des programmes
sont échelonnées.

Pendant la préemption, l'état du processus (drapeaux, registres et pointeurs d'instruction) est sauvé dans la mémoire. Il doit
être rechargé dans le processeur pour que le code soit exécuté à nouveau : c'est la commutation de contexte.

Un système d'exploitation préemptif conserve en permanence la haute main sur les tâches exécutées par le processeur,
contrairement à un système d'exploitation non préemptif, ou collaboratif, dans lequel c'est le processus en cours d'exécution
qui prend la main et décide seul du moment où il la rend. L'avantage le plus évident d'un système préemptif est qu'il peut en
permanence décider d'interrompre un processus, principalement si celui-ci échoue et provoque l'instabilité du système.

### Algorithmes d'ordonnancement

L'ordonnanceur désigne le composant du noyau du système d'exploitation choisissant l'ordre d'exécution des processus sur les
processeurs d'un ordinateur.

Un processus a besoin de la ressource processeur pour exécuter des calculs ; il l'abandonne quand se produit une interruption,
etc. De nombreux anciens processeurs ne peuvent effectuer qu'un traitement à la fois. Pour les autres, un ordonnanceur reste
nécessaire pour déterminer quel processus sera exécuté sur quel processeur (c'est la notion d'affinité, très importante pour ne
pas dégrader les performances). Au-delà des classiques processeurs multicoeurs, la notion d'hyperthreading rend la question de
l'ordonnancement encore un peu plus complexe.

A un instant donné, il y a souvent davantage de processus à exécuter que de processeurs.

Un des rôles de système d'exploitation et plus précisément de l'ordonnanceur du noyau, est de permettre à tous ces processus de
s'exécuter à un moment ou un autre et d'utiliser au mieux le processeur pour l'utilisateur. Pour que chaque tâche s'exécute sans
se préoccuper des autres et/ou aussi pour exécuter les tâches selon les contraintes imposées au système, l'ordonnanceur du noyau
du système effectue des commutations de contexte de celui-ci.

A intervalles réguliers, le système appelle une procédure d'ordonnancement qui *élit* le prochain processus à exécuter. Si le
nouveau processus est différent de l'ancien, un changement de contexte (opération consistant à sauvegarder le contexte
d'exécution de l'ancienne tâche comme les registres du processeur) a lieu. Cette structure de données est généralement appelée
PCB (process control block). Le système d'exploitation restaure l'ancien PCB de la tâche élue, qui s'exécute alors en reprenant
là où elle s'était arrêtée précédemment.

Du choix de l'algorithme d'ordonnancement dépend le comportement du système. Il existe deux grandes classes d'ordonnancement :

* **L'ordonnancement en temps partagé** présent sur la plupart des ordinateurs "classiques". Par exemple l'ordonnancement
"decay" ; qui est celui par défaut sous Unix. Il consiste en un système de priorités adaptatives, par exemple il privilégie les
tâches interactives pour que leur temps de réponse soit bon. Une sous-classe de l'ordonnancement en temps partagé sont les
ordonnanceurs dits "proportional share", eux sont plus destinés aux stations de calcul et permettent une gestion rigoureuse des
ressources. On peut citer notamment "lottery" et "stride".
* **L'ordonnancement en temps réel** qui assure qu'une certaine tâche sera terminée dans un délai donné. Cela est indispensable
dans les systèmes embarqués. Un ordonnanceur temps réel est dit optimal pour un système de tâches s'il trouve une solution
d'ordonnancement du système lorsque cette solution existe. S'il ne trouve pas de solution à ce système, alors aucun autre
ordonnanceur ne peut en trouver une.

Algorithmes d'ordonnancement :

* **Round-robin (ou méthode du tourniquet)** : Une petite unité de temps appelé quantum de temps est définie. La file d'attente
est gérée comme une file circulaire. L'ordonnanceur parcourt cette file et alloue un temps processeur à chacun des processus
pour un intervalle de temps de l'ordre d'un quantum au maximum. La performance de round-robin dépend fortement du choix du
quantum de base.
* **Rate-monotonic scheduling (RMS)** : L'ordonnancement à taux monotone est un algorithme d'ordonnancement temps réel en ligne
à priorité constante (statique). Il attribue la priorité la plus forte à la tâche qui possède la plus petite période. RMS est
optimal dans le cadre d'un système de tâches périodiques, synchrones, indépendantes et à échéance sur requête avec un
ordonnanceur préemptif. De ce fait, il n'est généralement utilisé que pour ordonnancer des tâches vérifiant ces propriétés.
* **Earliest deadline first scheduling (EDF)** : C'est un algorithme d'ordonnancement préemptif à priorité dynamique, utilisé
dans les systèmes temps réels. Il attribue une priorité à chaque requête en fonction de l'échéance de cette dernière selon la
règle : Plus l'échéance d'une tâche est proche, plus sa priorité est grande. De cette manière, au plus vite le travail doit être
réalisé, au plus il a des chances d'être exécuté.
* **FIFO** : Les premiers processus ajoutés à la file seront les premières à être exécutés.
* **Shortest job first (SJF, ou SJN-Shortest Job Next)** : Le choix se fait en fonction du temps d'exécution estimé du
processus. Ainsi l'ordonnanceur va laisser passer en priorité le plus court des processus de la file d'attente.
* **Completely Fair Scheduler** (CFS) : L'ordonnanceur des tâches pour le noyau Linux. Il gère l'allocation de ressource
processeur pour l'exécution des processus, en maximisant l'utilisation globale CPU tout en optimisant l'interactivité. CFS
utilise un arbre rouge-noir implémentant une chronologie des futures exécutions des tâches. En effet, l'arbre trie les processus
selon une valeur representative du manque de ces processus en temps d'allocation du processeur, par rapport au temps qu'aurait
alloué un processeur dit multitâche idéal. De plus, l'ordonnanceur utilise une granularité temporelle à la nanoseconde, rendant
redondante la notion de tranche de temps, les unités atomiques utilisées pour le partage du CPU entre processus. Cette
connaissance précise signifie également qu'aucune heuristique (basée sur la statistique, donc pouvant commettre des erreurs)
n'est requise pour déterminer l'interactivité d'un processus.

### Synchronisation

En programmation concurrente, la synchronisation se réfère à deux concepts distincts mais liés : la synchronisation de processus
et la synchronisation des données. La synchronisation de processus est un mécanisme qui vise à bloquer l'exécution de certains
processus à des points précis de leur flux d'exécution, de manière que tous les processus se rejoignent à des étapes relais
données, tel que prévu par le programmeur. La synchronisation de données, elle, est un mécanisme qui vise à conserver la
cohérence des données telles que vues par différents processus, dans un environnement multitâche. Initialement, la notion de
synchronisation est apparue pour la synchronisation de données.

Ces problèmes dits "de synchronisation" et même plus généralement ceux de communication inter-processus dont ils dépendent
rendent pratiquement toujours la programmation concurrente plus difficile. Certaines méthode de programmation, appelées
synchronisation non-blocante, cherchent à éviter d'utiliser des verrous, mais elles sont encore plus difficiles à mettre en
oeuvre et nécessitent la mise en place de structure de données particulières. La mémoire transactionnelle logicielle en est une.

La synchronisation de processus cherche par exemple à empêcher des programmes d'exécuter la même partie de code en même temps,
ou au contraire forcer l'exécution de deux partie de code en même temps. Dans la première hypothèse, le premier processus bloque
l'accès à la section critique avant d'y entrer et libère l'accès après y être sorti. Ce mécanisme peut être implémenté de
multiples manières.

Ces mécanismes sont par exemple la barrière de synchronisation, l'usage conjoint des sémaphores et des verrous, les spinlock, le
moniteur.

* **Barrière de synchronisation** : Permet de garantir qu'un certain nombre de tâches aient passé un point spécifique. Ainsi,
chaque tâche qui arrivera sur cette barrière devra attendre jusqu'à ce que le nombre spécifié de tâches soient arrivées à cette
barrière.
* **Sémaphore** : Variable partagée par différents "acteurs" qui garantit que ceux-ci ne peuvent accéder de façon séquentielle à
travers des opérations atomiques, et constitue la méthode utilisée couramment pour restreindre l'accès à des ressources
partagées et synchroniser les processus dans un environnement de programmation concurrente.
* **Verrous** : Permet d'assurer qu'un seul processus accède à une ressource à un instant donné. Un verrou peut être posé pour
protéger un accès en lecture et permettra à plusieurs processus de lire, mais aucun d'écrire. On dit alors que c'est un verrou
partagé. Un verrou est dit exclusif lorsqu'il interdit toute écriture et toute lecture en dehors du processus qui l'a posé. La
granularité d'un verrou constitue l'étendue des éléments qu'il protège. Par exemple dans les bases de données, un verrou peut
être posé sur une ligne, un lot de ligne, une table etc.
* **Spinlocks** : Mécanisme simple de synchronisation basé sur l'attente active.
* **Moniteur** : Mécanisme de synchronisation qui permet à plusieurs threads de bénéficier de l'exclusion mutuelle et la
possibilités d'attendre (*block*) l'invalidation d'une condition. Les moniteurs ont également un mécanisme qui permet aux autres
threads de signaler que leur condition est validé. Il est constitué d'un mutex et de variables conditionnelles.

La connaissance des dépendances entre les données est fondamentale dans la mise en oeuvre d'algorithmes parallèles, d'autant
qu'un calcul peut dépendre de plusieurs calculs préalables. Les *conditions de Bernstein* permettent de déterminer les
conditions sur les données lorsque deux parties de programme peuvent être exécutées en parallèle.

### Signaux

Un signal est une forme de communication entre processus utilisée par les systèmes de type Unix et ceux respectant les standards
POSIX. Il s'agit d'une notification asynchrone envoyée à un processus pour lui signaler l'apparition d'un événement. Quand un
signal est envoyé à un processus, le système d'exploitation interrompt l'exécution normale de celui-ci. Si le processus possède
une routine de traitement pour le signal reçu, il lance son exécution. Dans le cas contraire, il exécute la routine des signaux
par défaut.

La norme POSIX (et la documentation de Linux) limite les fonctions directement ou indirectement appelables depuis cette routine
de traitements des signaux. Cette norme donne une liste exhaustive de fonctions primitives dites *async-signal safe* (en
pratique les appels systèmes) qui sont les seules à pouvoir être appelées depuis une routine de traitement de signal sans avoir
un comportement indéfini. Il est donc suggéré d'avoir une routine de traitement de signal qui positionne simplement un drapeau
déclaré *volatile sig_atomic_t* qui serait testé ailleurs dans le programme.

L'appel système kill(2) permet d'envoyer, si cela est permis, un signal à un processus. La commande kill(1) utilise cet appel
système pour faire de même depuis le shell. La fonction raise(3) permet d'envoyer un signal au processus courant.

Les exceptions comme les erreurs de segmentation ou les divisions par zéro génèrent des signaux. Ici les signaux générés seront
respectivement SIGSEGV et SIGFPE. Un processus recevant ces signaux se terminera et générera un core dump par défaut.

Le noyau peut générer des signaux pour notifier les processus que quelque chose s'est passé. Par exemple, SIGPIPE est envoyé à
un processus qui essaye d'écrire dans un pipe qui a été fermé par celui qui lit. Par défaut, le programme se termine alors. Ce
comportement rend la construction de pipeline en shell aisée.

### Socket réseau

Une socket réseau est une structure logicielle comprise dans un noeud réseau qui sert de point d'arrivée pour les données
envoyées et reçues à travers le réseau. La structure et les propriétés d'une socket sont définies par une interface de
programmation (API) de l'architecture réseau. Les sockets sont créées uniquement durant l' intervalle de temps d'un processus
d'une application s'exécutant dans le noeud.

Du fait de la standardisation des protocoles TCP/IP au cours du développement d'Internet, le terme *socket réseau* est plus
communément utilisé dans le contexte de la *Suite des protocoles Internets*. On parle alors aussi de socket internet. Dans ce
contexte, une socket est identifiée extérieurement par les autres machines par son *adresse socket*, qui est la triade du
protocole de transfert, de l'adresse IP et du numéro de port.

Une pile de protocole, habituellement fournie par le système d'exploitation est un ensemble de services permettant aux processus
de communiquer à travers un réseau utilisant les protocoles que la pile implémente. Le système d'exploitation fait passer les
données utiles des paquets IP entrants à l'application correspondante en lisant l'information de l'adresse socket des entêtes
des protocoles IP et transport et en enlevant ces entêtes des données applications.

L'interface de programmation que les programmes utilisent pour communiquer avec la pile de protocole, utilisant les socket
réseau, est appelée **socket API**. Les API sockets internet sont généralement basées sur le standard socket de Berkeley. Dans
le standard socket de Berkeley, les socket sont une forme de descripteur de fichier (*read, write, open, close*).

Dans les protocoles internet standards TCP et UDP, une adresse socket est la combinaison d'une adresse IP et d'un numéro de
port. Les sockets n'ont pas besoin d'adresse source, mais si un programme lie la socket à une adresse source, la socket peut
être utilisée pour recevoir et envoyer des données à cette adresse. Basé sur cette adresse, les sockets internet délivrent les
paquets applicatifs entrants au processus applicatif approprié.

Plusieurs types de sockets internet sont disponibles :

* **Datagram** : Des sockets non connectées, qui utilisent le protocole UDP (*User Datagram Protocol*). Chaque paquet envoyé ou
reçu avec un socket datagramme est adressé et routé individuellement. L'ordre ainsi que la fiabilité ne sont pas garantis, par
conséquent plusieurs paquet envoyé depuis un processus à l'autre peut arriver dans n'importe quel ordre ou bien ne pas arriver
du tout. Certaines configurations spéciales peuvent être requises pour envoyer en broadcast un socket datagramme.
* **Stream** : Des socket connectés, qui utilisent les protocoles TCP (*Transmission Control Protocol*), SCTP (*Stream Control
Transmission Protocol*) ou DCCP (*Datagram Congestion Control Protocol*). Un socket flux fournit un flot de données sans
erreurs, séquencé, unique et ininterrompu avec des mécanismes prédéfinis pour créer et détruire des connections et rapporter des
erreurs. Un socket flux transmet des données de manière fiable, ordonnée sans requérir l'établissement préalable d'un canal de
communication.
* **Raw** : Permet l'envoie et la réception de paquets IP sans aucun formatage spécifique à un protocole de la couche transport.
Avec les autres types de socket, la donnée est automatiquement encapsulée selon le protocole de la couche transport choisi (TCP,
UDP etc.), et l'utilisateur du socket n'a pas connaissance de l'existence des entêtes du protocole. Quand on lit d'un socket
brut, les entêtes sont généralement inclus. Lorsqu'on transmet des paquets depuis un socket brut, l'addition automatique d'une
entête est optionnelle.

### Socket IPC

Un socket de domaine Unix ou socket IPC (*inter-process communication*) et un point d'arrivée des données de communications qui
permet d'échanger des données entre des processus s'exécutant sur le même système d'exploitation hôte. Les types de socket
valides dans le domaine UNIX sont :

* **SOCK_STREAM** (à comparer au TCP) - pour un socket orienté flux
* **SOCK_DGRAM** (à comparer à UDP) - pour un socket orienté datagramme qui préserve les limites des messages (comme la plupart
des implémentations UNIX, les socket de domaine UNIX datagram sont toujours fiables et ne réordonnent pas les datagrammes)
* **SOCK_SEQPACKET** (à comparer à SCTP) - pour un socket à paquets séquencés orienté connection, qui préserve les limites des
messages, et livre les paquets dans l'ordre d'envoi.

Les sockets de domaine Unix sont une composante standard des systèmes d'exploitation POSIX.

Les interfaces de programmation (API) pour les sockets de domaine Unix sont similaires à celles des sockets internet, mais au
lieu d'utiliser un protocole réseau sous-jacent, toutes les communications se placent à l'intérieur du noyau du système
d'exploitation. Les socket de domaine Unix peuvent utiliser le système de fichiers comme adresse d'espace de noms. (Certains
systèmes d'exploitation, comme Linux, offrent des espaces de noms additionnels.) Les processus référencent les sockets de
domaine Unix comme des inodes du système de fichier, ainsi 2 processus peuvent communiquer en ouvrant la même socket.

En plus de permettre l'envoi de données, les processus peuvent envoyer des descripteurs de fichiers à travers une connection de
socket de domaine Unix en utilisant les appels systèmes sendmsg() et recvmsg(). Ceci permet au processus qui envoie d'autoriser
le processus qui reçoit à accéder au descripteur de fichier auquel autrement le processus qui reçoit n'a pas accès. Ceci permet
d'implémenter une forme rudimentaire de sécurité basée sur l'accessibilité.

### Socket Netlink

La famille de socket Netlink est une interface du noyau Linux utilisée pour des communications inter-processus entre les
processus de l'espace utilisateur et du noyau et entre différents processus utilisateurs. La différence entre les sockets
Netlink et les socket IPC et qu'au lieu d'utiliser l'espace de noms du système de fichiers, les processus Netlink sont
généralement désignés par leurs PIDs.

Netlink fournit une interface socket standard pour les processus utilisateurs, et une API côté noyau pour un usage interne par
les modules du noyau.

### Tube anonyme

Un tube anonyme est un mécanisme de gestion de flux de donnée. Ce mécanisme inventé pour UNIX est principalement utilisé dans la
communication inter-processus. Ouvrir un tube anonyme permet de créer un flux de donnée unidirectionnel FIFO entre un processus
et un autre. Ces tubes sont détruits lorsque le processus qui les a créés disparaît, contrairement aux tubes nommés qui sont
liés au système d'exploitation et qui doivent être explicitement détruits.

Ce mécanisme permet la création de filtres.

Pour les système d'exploitation de type Unix, un tube anonyme est créé grâce à un appel système qui retourne un descripteur de
fichier à la suite de la création d'un Fork qui permet d'assigner à chacun des processus son rôle de récepteur ou d'émetteur.

### Tube nommé

Comme les tubes anonymes, les tubes nommés sont des zones de données organisées en FIFO mais contrairement à ceux-ci qui sont
détruits lorsque le processus qui les a créés disparait, les tubes nommés sont liés au système d'exploitation et ils doivent
être explicitement détruits.

### Passage de messages

Le modèle de passage de messages et une technique permettant de demander l'exécution d'un programme. Le passage de message
utilise un modèle objet afin de distinguer la fonction générale de ses implémentations spécifiques. Le programme appelant envoie
un message et se fie à l'objet afin de sélectionner et d'exécuter le code approprié. L'utilisation d'une couche intermédiaire,
est justifiée par des besoins de distribution et d'encapsulation.

L'encapsulation suit l'idée que les objets logiciels devraient être capables d'invoquer les services d'autres objets sans avoir
aucune connaissance spécifique de leurs implémentations. L'encapsulation permet de réduire les lignes de codes ainsi qu'une plus
grande maintenabilité des systèmes.

Le passage de messages distribué permet au développeur, à l'aide d'une couche fournissant les services de base de construire des
systèmes constitués de sous-systèmes s'exécutant sur des ordinateurs disparates, à différents endroit et à des horaires
différents. Lorsqu'un objet distribué envoie un message, la couche message s'occupe de :

* Trouver d'où et de quel processus le message est issu.
* Sauvegarder le message dans une file si l'objet approprié au traitement du message n'est pas en cours d'exécution et s'occuper
de l'envoyer dès que l'objet est disponible. Ainsi que de stocker le résultat si besoin, jusqu'à ce que l'objet qui a envoyé le
message est prêt à le recevoir.
* Contrôler diverses dépendances transactionnelles pour les transactions distribuées.

### Fichier mappé en mémoire

Un fichier mappé en mémoire est un segment de mémoire virtuelle qui est la copie d'une portion de fichier ou d'une ressource de
type fichier. Cette ressource est typiquement un fichier présent sur le disque, mais cela peut également être un périphérique,
un objet en mémoire partagée, ou toute autre ressource que le système d'exploitation peut référencer à l'aide d'un descripteur
de fichier. Une fois présente en mémoire, cette corrélation entre le fichier et l'espace mémoire permet aux applications de
traiter la partie mappée comme s'il s'agissait de la mémoire primaire.

Le bénéfice d'utiliser le mappage en mémoire est d'augmenter les performances d'entrée/sortie notamment sur les fichiers de gros
volume. Pour les petits fichiers, les fichiers mappés peuvent engendrer des problèmes de fragmentation interne du fait que les
maps mémoires sont toujours aligné sur la taille de la page (généralement 4Ko). Par conséquent, un fichier de 5Ko allouera 8Ko
et gâchera 3Ko. Accéder aux fichiers mappés en mémoire est plus rapide que d'utiliser des opérations de lecture et d'écriture
directement pour 2 raisons. Premièrement, un appel système est bien plus lent qu'un accès vers la mémoire locale du programme.
Deuxièmement, dans la plupart des systèmes d'exploitation la région mémoire mappée est la page cache du noyau, c'est à dire que
cela ne nécessite aucune copie en espace utilisateur.

Le processus de mapping mémoire est géré par le gestionnaire de mémoire virtuelle, qui est le même sous-système responsable de
la pagination. Les fichiers mappés sont chargés une page entière à la fois. La taille de la page est choisi par le système
d'exploitation pour un maximum de performance. Sachant que la pagination est un élément critique du gestionnaire de mémoire
virtuelle, le chargement des portions de la taille d'une page d'un fichier en mémoire physique est généralement une fonction
système très optimisée.

L'usage le plus commun de fichier mappé en mémoire est le chargement de processus. Lors de la création d'un processus, le
système d'exploitation utilise un fichier mappé en mémoire pour faire apparaître le fichier exécutable ainsi que tous les
modules chargeable en mémoire pour exécution. La technique la plus utilisée est la demande de pages, le fichier est chargé en
mémoire physique par section (chacune d'une page), et seulement quand cette page est référencée. Dans le cas spécifique des
exécutables, cela permet à l'OS de charger de manière sélective uniquement les portions de l'image processus qui nécessitent
réellement une exécution.

Un autre usage répandu pour les fichiers mappés en mémoire est le partage de fichiers entre processus multiples. Ceci permet
d'éviter les fautes de pages ainsi que les violations de segmentation.

### Partitionnement de la mémoire

Le partitionnement de la mémoire est la création d'une ou plusieurs régions dans la mémoire secondaire, telle que chaque région
puisse être gérée séparément. Ces régions sont appelées partitions. C'est généralement la première chose à faire sur un nouveau
disque installé, avant qu'un système de fichier soit créé. Le disque stocke l'information sur le positionnement et la taille des
partitions dans une aire appelée table de partition que le système d'exploitation lit avant toute autre région du disque. Chaque
partition apparait alors comme un disque "logique" du point de vue du système d'exploitation qui fait usage d'une partie du
disque réel. Les administrateurs systèmes utilisent alors un programme appelé éditeur de partitions pour créer, retailler,
supprimer et manipuler les partitions. Le partitionnement permet l'usage de différents systèmes de fichiers afin de stocker tout
types de fichiers. Séparer les données utilisateur des données système permet d'empêcher que la partition système soit pleine ce
qui rendrait le système inutilisable. Le partitionnement permet aussi de simplifier la sauvegarde. Un désavantage du
partitionnement est qu'il peut être difficile d'allouer la taille adéquate à chacune des partitions, ce qui peut avoir pour
conséquence de laisser une partition avec énormément d'espace libre et une autre totalement saturée.

### MBR

Un **enregistrement de démarrage maître (MBR)** est type spécial de secteur de démarrage situé au commencement d'un appareil de
stockage informatique partitionné tel qu'un disque dur.

Le MBR contient l'information du comment les partitions logiques, contenant le système de fichiers, sont organisées sur cet
appareil. Le MBR contient également du code exécutable pour fonctionner comme chargeur pour un système d'exploitation installé
généralement en laissant la main au second étage du chargeur, ou en conjonction avec chacun des enregistrement de démarrage de
volume (VBR). Ce code MBR est généralement désigné comme un chargeur d'ammorçage.

L'organisation de la table de partition au niveau du MBR limite le maximum d'espace de stockage adressable d'un disque
partitionné à 2To. Par conséquent, le shema de partitionnement MBR est progressivement remplacé par le schema de table de
partitionnement GUID (GPT).

<TODO>

### GPT

La **table de partition GUID (GPT)** est un standard pour la disposition de tables de partitions sur un appareil de stockage
informatique, tels que les disques durs ou les SSDs, en utilisant des identifiants uniques universels, aussi connus en tant
qu'identifiants uniques globaux (GUIDs). Ce standard fait parti des standards d'interface microgicielle extensible unifiée
(UEFI), il peut néanmoins être utilisé pour des système BIOS, du fait des limitations des tables de partitions MBR.

Tous les systèmes d'exploitation récents supportent GPT.

<TODO>

### Mémoire virtuelle

Le principe de mémoire virtuelle repose sur l'utilisation de traduction à la volée des adresses virtuelles vue du logiciel, en
adresses physiques de mémoire vive. La mémoire virtuelle permet :

* d'utiliser de la mémoire de masse comme extension de la mémoire vive ;
* d'augmenter le taux de multiprogrammation ;
* de mettre en place des mécanismes de protection de la mémoire ;
* de partager la mémoire entre processus.

### Pagination

Les adresses mémoires émises par le processeur sont des adresses virtuelles, indiquant la position d'un mot dans la mémoire
virtuelle. Cette mémoire virtuelle est formée de zones de même taille, appelées pages. Une adresse virtuelle est donc un couple
(numéro de page, déplacement dans la page). La taille des pages est une puissance entière de deux, de façon à déterminer sans
calcul le déplacement (10 bits de poids faible de l'adresse virtuelle pour des pages de 1024 mots), et le numéro de page (les
autres bits). La mémoire vive est également composées de zones de même taille, appelées cadres (*frames*), dans lesquelles
prennent place les pages (un cadre contient une page : taille d'un cadre = taille d'une page). La taille de l'ensemble des
cadres en mémoire vive utilisés par un processus est appelé *Resident set size*. Un mécanisme de traduction (*translation*)
assure la conversion des adresses virtuelles en adresses physiques, en consultant une table des pages (*page table*) pour
connaître le numéro du cadre qui contient la page recherchée. L'adresse physique obtenue est le couple (numéro de cadre,
déplacement). Il peut y avoir plus de pages que de cadres (c'est là tout l'intérêt) : les pages qui ne sont pas en mémoire sont
stockées sur un autre support (disque), elle seront ramenées dans un cadre quand on en aura besoin.

La table des pages est indexée par le numéro de page. Chaque ligne est appelée "entrée dans la table des pages (*pages table
entry* PTE), et contient le numéro de cadre. La table des pages pouvant être située n'importe où en mémoire, un registre spécial
(PTBR pour *Page Table Base Register*) conserve son adresse.

En pratique, le mécanisme de traduction fait partie d'un circuit électronique appelé MMU (*memory management unit*) qui contient
également une partie de la table des pages, stockée dans une mémoire associative formée de registres rapides. Ceci évite d'avoir
à consulter la table des pages (en mémoire) pour chaque accès mémoire.

### Segmentation

La segmentation est une technique de découpage de la mémoire. Elle est gérée par l'unité de segmentation de l'unité de gestion
mémoire (*MMU*), utilisée sur les systèmes d'exploitation modernes, qui divise la mémoire physique (dans le cas de la
segmentation pure) ou la mémoire virtuelle (dans le cas de la segmentation avec pagination) en segments caractérisés par leur
adresse de début et leur taille (*décalage*).

La segmentation permet la séparation des données du programme (entre autres segments) dans des espaces logiquement indépendants
facilitant alors la programmation, l'édition de liens et le partage inter-processus. La segmentation permet également d'offrir
une plus grande protection grâce au niveau de privilège de chaque segment.

Lorsque l'unité de gestion mémoire (MMU) doit traduire une adresse logique en adresse linéaire, l'unité de segmentation doit
dans un premier temps utiliser la première partie de l'adresse, c'est à dire le sélecteur de segment, pour retrouver les
caractéristiques du segment (base, limit, DPL, etc.) dans la table de descripteurs (GDT ou LDT). Puis il utilise la valeur de
décalage qui référence l'adresse à l'intérieur du segment.

Il existe sur la majorité des processeurs actuels, des registres de segments (CS, DS, SS, etc.) qui contiennent le sélecteur de
segment dernièrement utilisé par le processeur qui sont utilisés pour accélérer l'accès à ces sélecteurs.

Sur les processeurs récents, il existe également des registres associés à chaque registre de segment qui contiennent le
descripteur de segment associé pour un accès plus rapide aux descripteurs.

Un segment mémoire est un espace d'adressage indépendant défini par deux valeurs :

* L'adresse où il commence (aussi appelée *base*, ou *adresse de base*)
* Sa taille ou son *décalage* (aussi appelée *limite* ou *offset*)

Un segment constitue donc dans la mémoire principale un plage d'adresse continue. Les segments se chevauchent. On peut donc
adresser la même zone mémoire avec plusieurs couples segment/offset.

Il existe différents types de segment :

* Les segments de données statiques
* Les segments de données globales
* Les segments de code
* Les segments d'état de tâche

### Sysfs

Sysfs est un système de fichiers virtuel introduit par le noyau Linux 2.6. Sysfs permet d'exporter depuis l'espace noyau vers
l'espace utilisateur des informations sur les périphériques du système et leur pilotes, et est également utilisé pour configurer
certaines fonctionnalités du noyau.

Pour chaque objet ajouté à l'arbre des modèles de pilotes (pilotes, périphériques, classe de périphériques), un répertoire est
créé dans sysfs. La relation parent/enfant est représentée sous la forme de sous-répertoires dans */sys/devices/* (représentant
la couche physique). Le sous-répertoire */sys/bus/* est peuplé de liens symboliques, représentant la manière dont chaque
périphérique appartient aux différents bus. */sys/class/* montre les périphérique regroupés en classes, comme les périphériques
réseau par exemple, pendant que */sys/block/* contient les périphériques de type bloc.

Pour les périphériques et leurs pilotes, des attributs peuvent être créés. Ce sont de simples fichiers, la seule contrainte est
qu'ils ne peuvent contenir chacun qu'une seule valeur et/ou n'autoriser le renseignement que d'une valeur (au contraire de
certains fichiers de procfs, qui nécessitent d'être longuement parcourus). Ces fichiers sont placés dans le sous-répertoire du
pilote correspondant au périphérique. L'utilisation de groupes d'attributs est possible en créant un sous-répertoire peuplé
d'attributs.

Sysfs est utilisé par quelques utilitaires pour accéder aux informations concernant le matériel et ses pilotes (des modules du
noyau comme udev par exemple). Des scripts ont été écrits pour accéder aux informations obtenues précédemment via procfs, et
certains scripts permettent la configuration du matériel et de leur pilote via leurs attributs.

Sysfs s'appuie sur ramfs. Un système de fichiers temporaire très simple monté en RAM.

### Udev

Udev est un gestionnaire de périphérique pour le noyau Linux. Udev gère principalement des noeuds périphériques dans le
répertoire
*/dev/*. Udev traite également tous les événements dans l'espace utilisateurs lors de l'ajout ou de la suppression d'un
périphérique, ainsi que du chargement des microgiciels.

Les pilotes font parti du noyau Linux, dans le sens où leurs fonctions principales incluent la découverte de périphérique, la
détection des changements d'états, et autres fonctions matérielles similaires de bas niveau. Après chargement du pilote de
périphérique en mémoire depuis le noyau, les événements détectés sont envoyés au daemon de l'espace utilisateur *udevd*. C'est
le gestionnaire de périphérique, *udevd*, qui récupère tout ces événements et qui ensuite décide de la suite à donner. A cette
fin, *udevd* dispose d'un ensemble de fichiers de configurations, pouvant être ajustés par l'administrateur suivant ses besoins.

* Dans le cas d'un nouvel appareil de stockage USB, *udevd* est notifié par le noyau qui lui-même notifie le udisksd-daemon. Ce
daemon pourra alors monter le système de fichiers.
* Dans le cas d'une nouvelle connection de câble Ethernet à la carte d'interface réseau Ethernet (NIC), *udevd* est notifié par
le noyau qui lui-même notifie le NetworkManager-daemon. Le NetworkManager-daemon pourra alors démarrer le daemon client dhcp
pour cette NIC, ou bien configurer la connection à l'aide d'une configuration manuelle quelconque.

Contrairement aux systèmes traditionnels UNIX, ou les noeuds périphériques contenus dans le répertoire */dev* était un ensemble
de fichiers statique, le gestionnaire de périphérique Linux udev fournit dynamiquement, uniquement les noeuds des périphériques
actuellement disponibles au système :

* udev fournit un nommage de périphérique persistant, qui ne dépend pas de, par exemple, l'ordre de connection des appareils au
système.
* udev s'exécute entièrement en espace utilisateur. Une conséquence est que udev peut exécuter des programmes arbitraires pour
composer un nom pour le périphérique fonction de ses propriétés, avant que le noeud soit créé; d'ailleurs, l'ensemble du
processus de nommage est également interruptible et s'exécute avec une priorité basse.

Udev est divisé en trois parties :

* La bibliothèque *libudev* qui permet l'accès aux informations des périphériques; qui est maintenant inclue dans *systemd*.
* Le daemon de l'espace utilisateur *udevd* qui gère */dev* virtuel.
* L'utilitaire d'administration en ligne de commande *udevadm* pour des diagnostics.

Le système reçoit des appels depuis le noyau via des sockets netlink.

### Bus

Un bus est un système de communication qui permet de transférer des données entre les composants d'un ordinateur ou entre
ordinateurs. Cette expression couvre tous les composants matériels (fils, fibre optique, etc.) et logiciels, protocoles de
communications inclus. Les bus informatiques modernes peuvent à la fois utiliser des connections parallèles et séries avec hubs.
C'est par exemple le cas de l'USB.

Un bus d'adresses est un bus utilisé pour spécifier une adresse physique. Quand un processeur ou un périphérique bénéficiant
d'un accès direct à la mémoire (*DMA-enabled*) a besoin de lire ou d'écrire en mémoire, il spécifie en emplacement mémoire sur
le bus d'adresses (la valeur à lire ou à écrire est alors envoyé sur le bus de données). La largeur du bus d'adresse détermine
la quantité de mémoire qu'un système peut adresser.

Accéder un octet individuel requiert généralement d'écrire ou de lire une largeur de bus complète (un mot) à la fois. Dans ce
cas, les bits les moins signifiants du bus d'adresses peuvent même ne pas être implémentés - il revient en effet au contrôleur
d'isoler l'octet individuel demandé du mot complet transmis.

### Accès direct à la mémoire

L'accès direct à la mémoire (*DMA*) est un procédé informatique où les données circulant de, ou vers un périphérique sont
transférées directement par un contrôleur adapté vers la mémoire principale, sans intervention du microprocesseur si ce n'est
pour lancer et conclure le transfert. La conclusion du transfert ou la disponibilité du périphérique peuvent être signalés par
interruption.

### Pilotes

Un pilote est un programme qui opère et contrôle un périphérique. Un pilote fournit une interface logicielle au matériel,
permettant au système d'exploitation et aux autres programmes d'accéder aux fonctions matérielles sans avoir besoin de connaître
en détail le périphérique à utiliser.

Le pilote communique avec le périphérique via le bus informatique auquel celui-ci est connecté. Lorsqu'un programme appelant
invoque une routine du pilote, celui-ci va envoyer une commande au périphérique. Une fois que le périphérique renvoie des
données au pilote, le pilote peut invoquer des routines du programme à l'origine de l'appel.

Les pilotes sont dépendant du matériel et spécifiques au système d'exploitation. Il fournissent généralement la gestion des
interruptions à n'importe quelle interface matérielle asynchrone nécessaire.

### BIOS

Le **BIOS** est un microgiciel utilisé pour l'initialisation du matériel pendant le processus de démarrage, et pour fournir des
services à l'exécution pour les systèmes d'exploitation et les programmes. Le microgiciel BIOS est préinstallé sur la carte
mère, c'est le premier logiciel à s'exécuter lors de la mise sous tension.

Le BIOS dans les PCs modernes initialise et teste les composants matériels du système, et charge un chargeur d'amorçage depuis
l'appareil de stockage de masse qui initialise un système d'exploitation.

La plupart des implémentations du BIOS sont spécifique à un modèle de carte mère, en s'interfaçant avec différents appareil et
en particulier le chipset. Anciennement, le microgiciel BIOS était stocké sur une puce ROM de la carte mère. De nos jours, le
contenu du BIOS est stocké sur de la mémoire flash de façon à pouvoir être réécrite sans enlever la puce de la carte mère. Ceci
permet une mise à jour aisée du BIOS, mais également l'infection de l'ordinateur via des rootkits du BIOS. De plus, si une mise
à jour du BIOS échoue cele peut potentiellement rendre la carte mère inutilisable.

L'interface microgicielle extensible unifiée (UEFI) est un successeur du BIOS, il vise à résoudre ses limitations techniques.

### UEFI

**L'interface microgicielle extensible unifiée (UEFI)** est une spécification qui définit une interface logicielle entre un
système d'exploitation et une plateforme microgicielle. UEFI remplace l'ancienne interface microgicielle BIOS dont elle reprend
généralement l'ensemble des services. L'UEFI peut supporter les diagnostics et réparations distants, même sans système
d'exploitation.

<TODO>

### Shell

Le Shell est un programme qui permet d'interpréter les commandes de l'utilisateur. C'est l'un des tout premiers moyens
d'interagir avec un ordinateur. Le shell est généralement plus puissant qu'une interface graphique utilisateur (GUI), dans le
sens où il permet d'accéder très efficacement aux fonctionnalités internes du système d'exploitation (OS).

Souvent les outils textuels dont il dispose sont construits de manière à pouvoir être composés. Ainsi de multiples assemblages
permettent à la fois une simplicité dans la décomposition des tâches, et une facilité de mise en oeuvre dans l'automatisation.

Les shells peuvent généralement dépendre des OS, sachant qu'il en existe une quantité pour chacun d'entre eux.

### Cgroups

Les cgroups sont une fonctionnalité du noyau Linux qui limite, compte et isole l'utilisation des ressources (CPU, mémoire,
entrée/sortie, réseau, etc.) d'une collection de processus.

Ces groupes de contrôle peuvent être utilisés de multiples façons :

* En accédant le système de fichier virtuel du cgroup manuellement.
* En créant et gérant des groupes à la volée en utilisant des outils tels que *cgcreate*, *cgexec* et *cgclassify*
(*libcgroup*).
* A travers le "daemon moteur de règles" qui peut déplacer les processus de certains utilisateurs, groupes de manière
automatique ou commander aux cgroups comme cela a été spécifié dans sa configuration.
* Indirectement à travers d'autres logiciels utilisant les cgroups, tels que Docker, LXC, libvirt, systemd, etc.

### Espaces de noms

Un espace de noms encapsule dans une abstraction une ressource système globale qui la fait apparaître aux processus contenus
dans l'espace de noms qui ont leur propre instance isolée de la ressource globale. Les changements de la ressource globale sont
visibles aux autres processus membre de l'espace de noms, mais invisible aux autres processus. Les espaces de noms sont utilisés
pour implémenter les conteneurs.

Les types d'espace de noms disponibles dans Linux sont les suivants : Cgroup, IPC, Network, mount, PID, Time, User, UTS.

### Systemd-nspawn

Systemd-nspawn peut être utilisée pour exécuter une commande ou un OS dans un conteneur léger d'espace de noms. Il est plus
puissant que *chroot* puisqu'il virtualise la hiérarchie du système de fichiers, mais aussi l'arbre des processus, les
différents sous-systèmes IPC ainsi que le nom de l'hôte et du domaine.

Systemd-nspawn limite l'accès en lecture seule à différentes interfaces du noyau dans le conteneur, telles que **/sys**,
**/proc/sys** ou **/sys/fs/selinux**. Les interfaces réseaux et l'horloge système ne peuvent pas être modifiées depuis
l'intérieur du conteneur. Les fichiers spéciaux ou fichiers de périphérique ne peuvent  pas non plus être créés. Le système hôte
ne peut pas être redémarré et des modules du noyau ne peuvent pas être chargés depuis le conteneur.

Les conteneurs ainsi créés peuvent être gérés à l'aide de la commande *machinectl*.

### Conteneurisation LXC

LXC est une méthode de virtualisation au niveau du système d'exploitation permettant d'exécuter plusieurs système isolés Linux
sur un système hôte de contrôle en utilisant un unique noyau Linux.

Le noyau Linux fournit la fonctionnalité des cgroups qui permet une limitation et une priorisation des ressources (CPU, mémoire,
entrées/sorties, réseau, etc.) sans besoin d'aucune machine virtuelle, ainsi que la fonction d'isolation par espace de noms qui
permet l'isolation complète d'une application du point de vue de l'environnement opérant, incluant l'arbre des processus, la
configuration réseau, les identifiants utilisateurs et les systèmes de fichiers montés.

LXC combine les cgroups du noyau et inclut l'isolation des espaces de nom pour fournir un environnement isolé à des
applications.

LXC permet d'exécuter des conteneurs en tant que simple utilisateur sur l'hôte à l'aide des conteneurs dits "non-privilégiés".

### Conteneurisation Docker

Docker est un ensemble de produits de Plateforme en tant que Service qui utilise la virtualisation au niveau du système
d'exploitation pour livrer des logiciels dans des paquets appelé conteneurs. Les conteneurs sont isolés les uns des autres et
embarquent leurs propres logiciels, bibliothèques et fichiers de configuration ; ils peuvent communiquer entre eux à travers des
canaux bien définis. Tous les conteneurs sont exécuté par un noyau de système d'exploitation unique et par conséquent utilisent
moins de ressources que des machines virtuelles.

Docker peut empaqueter une application et ses dépendances dans un conteneur virtuel qui peut s'exécuter sur n'importe quel
ordinateur Linux, Windows, ou macOS. Ceci permet à l'application de s'exécuter dans toute une variété d'environnement, par
exemple, sur sa propre machine, dans un cloud public ou privé. Quand il s'exécute sur Linux, Docker utilise les mécanismes
d'isolation fournis par le noyau (tels que les cgroups et les espaces de noms) et les systèmes de fichiers capables de montage
en union, pour permettre aux conteneurs de s'exécuter sur une instance Linux unique, évitant la surcharge du démarrage et de la
maintenance de machines virtuelles.

Le support des espaces de nom du noyau Linux permet d'isoler l'environnement système de la vue de l'application comme l'arbre
des processus, le réseau, les identifiants utilisateurs et les systèmes de fichiers montés, tandis que les cgroups fournissent
la limitation de ressources mémoire et CPU. Docker inclut également sa propre bibliothèque *libcontainer* permettant d'accéder
directement les fonctions de virtualisation fournies par le noyau Linux en plus de l'utilisation d'interfaces de virtualisation
telles que *libvirt*, *LXC* et *systemd-nspawn*.

Docker implémente une API de haut niveau pour fournir des conteneurs légers exécutant des processus isolés.

Le logiciel Docker en tant qu'offre de services consiste en trois composants :

* **Le Logiciel** : Le daemon Docker, *dockerd*, est un processus persistant qui gère les conteneurs Docker ainsi que les objets
du conteneur. Le daemon est à l'écoute des requêtes envoyés via l'API du Docker Engine. Et le programme client *docker* fournit
une interface de ligne de commande qui permet d'interagir avec le daemon.
* **Les objets** : Les objets docker sont des entités diverses utilisées pour assembler une application dans Docker. Les classes
principales des objets Dockers sont les images, les conteneurs et les services.
    + Un conteneur Docker est un environnement encapsulé, standardisé qui exécute des applications. Un conteneur est géré à
    travers l'API Docker ou la ligne de commande.
    + Une image Docker est un modèle en lecture seule utilisée pour construire des conteneurs. Les images sont utilisées pour
    stocker et envoyer des applications.
    + Un service Docker permet une mise à l'échelle des conteneurs sur de multiples daemons. Ceci étant appelé une nuée
    (*swarm*), un ensemble de daemon coopérant, communiquant à travers l'API Docker.
* **Les registres** : Un registre Docker est un dépôt d'image Docker. Les clients Docker se connectent aux registres pour cloner
(*pull*) des images à utiliser ou déposer (*push*) des images qu'ils ont construites. Les dépôts peuvent être publics ou privés.
Les deux principaux dépôts publics sont Docker Hub et Docker Cloud. Docker Hub est le registre par défaut ou Docker recherche
des images. Des registres Docker permettent également la création de notifications basée sur des évènements.

Le logiciel Docker dispose également d'outils :

* **Docker Compose** qui est un outil qui permet de définir et d'exécuter des applications Docker multi-conteneurs. Il utilise
des fichier YAML pour configurer les services de l'application et créé et démarre les processus de tous les conteneurs à l'aide
d'une seule commande. L'interface en ligne de commande *docker-compose* permet aux utilisateurs de passer des commandes sur des
ensembles de conteneurs, par exemple pour construire des images, déployer des conteneurs, redémarrer des conteneurs, etc. Les
commandes liées à la manipulation d'image, ou les options interactives sont inutiles dans Docker Compose car elle s'adressent à
un conteneur unique. Le fichier docker-compose.yml est utiliser pour définir les services de l'application et inclut plusieurs
options de configuration. Par exemple, l'option *build* définit les options de configuration telles que le chemin du Dockerfile,
l'option *command* permet de surcharger les commandes Docker par défaut, etc.
* **Docker Swarm** fournit nativement un fonctionnalité de grappe pour les conteneurs Docker qui transforme un groupe de Docker
engine en un unique Docker engine virtuel. L'interface ligne de commande *docker swarm* permet aux utilisateurs d'exécuter des
conteneurs Swarm, de créer des mots-clefs, lister les noeuds de la grappe, etc. L'interface ligne de commande *docker node*
permet aux utilisateurs d'exécuter diverses commandes pour gérer des noeuds dans la nuée, par exemple, lister les noeuds de la
nuée, mettre à jour les noeuds, supprimer les noeuds d'une nuée. Docker gère les nuées en utilisant l'algorithme de consensus
Raft. Selon Raft, pour qu'une mise à jour se fasse, la majorité des noeuds de la nuée doivent s'accorder sur celle-ci.

### Orchestrateur Kubernetes

Kubernetes est un système d'orchestration de conteneurs open source permettant d'automatiser le déploiement, la mise à l'échelle
et la gestion d'applications informatiques.

Beaucoup de services de cloud offrent des plateformes ou infrastructures en tant que service (PaaS, IaaS) basées sur Kubernetes
sur lesquelles Kubernetes peut être déployé comme service fournisseur de plateformes.

Kubernetes définit un ensemble de primitives, qui collectivement fournissent des mécanismes de déploiement, de maintien et de
mise à l'échelle d'applications basé sur les ressources CPU, mémoire et autres métriques personnalisées. Kubernetes est
lâchement couplé et extensible pour s'accorder à différentes charges de travail. Cette extensibilité est fournit en grande
partie par l'API Kubernetes, utilisée par des composants internes ainsi que les extensions et conteneurs exécutés sur
Kubernetes. La plateforme exerce son contrôle sur les ressources de calcul et de stockage et définissant ces ressources comme
objets, ceux-ci pouvant par la suite être gérés comme tels.

Kubernetes suit l'architecture Maître-Esclave. Les composants de Kubernetes peuvent être divisés entre ceux qui gèrent les
noeuds individuels et ceux qui font partie du plan de contrôle.

Le maître Kubernetes est l'unité principale de contrôle du cluster, il gère la charge de travail et dirige les communications à
travers le système. Le plan de contrôle de Kubernetes consiste en divers composants, chacun ayant sa propre tâche, pouvant être
exécuté soit sur un simple noeud maître, soit sur plusieurs maîtres pour des clusters à haute disponibilité. Les différents
composants du plan de contrôle Kubernetes sont les suivants :

* **etcd** : etcd est une base de données clef-valeur, légère, persistante et distribuée qui stocke de manière fiable les
données de configuration du cluster, donnant une représentation de l'état global du cluster à l'instant t. *etcd* est un système
qui favorise la cohérence à la disponibilité dans l'éventualité d'une partition réseau. Cette cohérence est cruciale pour
ordonnancer correctement les services opérants. Le serveur de l'API Kubernetes utilise l'API de visualisation d'etcd pour
surveiller le cluster et déployer des changements configuration critiques ou simplement restaurer n'importe quelle divergence
d'état du cluster tels qu'il était déclaré par celui qui l'a déployé.
* **Le serveur d'API** : Le serveur d'API est un composant clé qui sert l'API Kubernetes via des JSON en HTTP, qui fournit à la
fois les interfaces internes et externes à Kubernetes. Le serveur d'API traite et valide les requêtes REST et met à jour l'état
des objets dans etcd, de fait permettant aux clients de configurer les charges de travail et les conteneurs à travers les
noeuds.
* **L'ordonnanceur** : L'ordonnanceur est un composant qui, sur la base de la disponibilité ressource, sélectionne un noeud sur
lequel s'exécute un pod non ordonnancé (entité de base géré par l'ordonnanceur). L'ordonnanceur suit l'usage ressource sur
chacun des noeuds afin de s'assurer que la charge de travail n'est pas planifiée en excès de la ressource disponible. A cette
fin, l'ordonnanceur doit connaître les conditions et disponibilités de la ressource et autres contraintes définies par
l'utilisateur, les directives politiques telles que la qualité de service, les conditions d'affinité ou de non-affinité, la
localisation des données etc. Le rôle de l'ordonnanceur est d'accorder la ressource disponible à la charge de travail demandée.
* **Le gestionnaire de contrôle** : Un contrôleur est une boucle de réconciliation qui amène l'état courant du cluster vers
l'état désiré du cluster, en communicant via le serveur d'API pour créer, mettre à jour et supprimer les ressources qu'il gère
(pods, services, extrémités, etc.). Le gestionnaire de contrôle est un processus qui gère un ensemble de contrôleurs du noyau
Kubernetes. Un type de contrôleur est un contrôleur de réplication, qui s'occupe de la réplication et de la mise à l'échelle en
exécutant un nombre de copies de pods spécifiées à travers le cluster. Il s'occupe également de créer des pods de remplacement,
si les noeuds sous-jacents sont en erreur. D'autres contrôleurs qui sont une partie du noyau Kubernetes incluent le contrôleur
DaemonSet pour exécuter exactement un pod sur chaque machine (ou sous-ensemble de machines), et un contrôleur de travail pour
exécuter des pods jusqu'à fin d'exécution par exemple pour des traitements batchs. L'ensemble des pods qu'un contrôleur gère est
déterminé par l'étiquette des sélecteurs faisant partie de la définition du contrôleur.

Un noeud, est une machine où des conteneurs (charge de travail) sont déployés. Chaque noeud à l'intérieur du cluster doit
exécuter un environnement d'exécution de conteneurs tel que Docker, ainsi que les composants mentionnés ci-dessous, à des fins
de communication avec le maître pour la configuration réseau de ces conteneurs.

* **Kubelet** : Kubelet est responsable de l'état d'exécution de chaque noeud, il s'assure que tous les conteneurs du noeud sont
sains. Il prend en charge le démarrage, l'arrêt et la maintenance des conteneurs d'application organisés en pods tels que l'a
décidé le plan de contrôle. Kublet surveille l'état d'un pod, s'il n'est pas dans l'état désiré, le pod est redéployé sur le
même noeud. Le statut du noeud est relayé sur une période de quelques secondes via des messages au maître. Si le maître détecte
un échec de noeud, le contrôleur de réplication observe ce changement de statut et lance des pods sur d'autres noeuds sains.
* **Kube-proxy** : Le Kube-proxy est une implémentation d'un proxy réseau et d'un répartiteur de charge, il prend en charge
l'abstraction de service ainsi que d'autres opérations réseau. Il est responsable du routage du traffic vers le conteneur
approprié basé sur l'adresse IP et le numéro de port de la requête qui arrive.
* **Environnement** d'exécution de conteneur : Un conteneur réside dans un pod. Le conteneur est le niveau de micro-service le
plus bas, qui contient l'application en cours d'exécution, les bibliothèques et leurs dépendances. Ils peuvent également avoir
une adresse IP externe.

L'unité d'ordonnancement de base dans Kubernetes est un pod. Un pod est un groupement de composants conteneurisés. Un pod
consiste en un ou plusieurs conteneurs garantis de se trouver sur le même noeud.

Chaque pod dans Kubernetes est assigné à une adresse IP unique à l'intérieur du cluster, permettant aux applications d'utiliser
des ports sans risques de conflits. A l'intérieur du pod, chaque conteneur peut faire référence à chaque autre sur le localhost,
mais un conteneur à l'intérieur d'un pod n'a aucun moyen de s'adresser directement à un autre conteneur dans un autre pod ; pour
cela, il doit utiliser l'adresse IP du pod.

Un pod peut définir un volume, tel qu'un répertoire du disque local ou un disque réseau, et l'exposer aux conteneurs du pod. Les
pods peuvent être gérés manuellement via l'API Kubernetes, ou leur gestion peut être déléguée à un contrôleur. De tels volumes
sont aussi la base des fonctionnalités Kubernetes de ConfigMaps (pour fournir un accès à la configuration à travers le système
de fichier visible au conteneur) et Secrets (pour fournir les certificats nécessaires à l'accès sécurisé à des ressources
distantes, en donnant uniquement aux conteneurs autorisés, ces certificats sur leur système de fichier visible).

La fonction d'un ReplicaSet est de maintenir un ensemble stable de pods répliqués pouvant être exécutés à tout moment. En tant
que tel, il est souvent utilisé pour garantir la disponibilité d'un nombre de pods identiques spécifique.

Les ReplicaSets est également un mécanisme de rassemblement qui permet à Kubernetes de maintenir pour un pod donné un nombre
d'instance défini à l'avance. La définition d'un ensemble de réplique utilise un sélecteur, dont l'évaluation résulte en
l'évaluation de tous les pods qui lui sont associés.

Un service Kubernetes est un ensemble de pods travaillant de concert, tel une couche d'une application multi-couche. L'ensemble
de pods qui constitue un service sont définis par un sélecteur d'étiquette. Kubernetes fournit deux modes de découverte de
service, en utilisant des variables d'environnement, ou en utilisant le DNS Kubernetes. La découverte de service assigne un
adresse IP fixe et un nom DNS au service, et réparti la charge du traffic en utilisant un DNS round-robin pour les connections
réseaux à cette adresse IP au milieu des pods vérifiant ce sélecteur (même si des erreurs peuvent amener les pods à passer d'une
machine à une autre). Par défaut un service est exposé à l'intérieur d'un cluster (par exemple les pods back-end peuvent être
groupés en service, recevant des requêtes de la part des pods front-end réparties entre eux), mais un service peut également
être exposé en dehors d'un cluster (par exemple pour que les clients puissent accéder aux pods front-end).

Les systèmes de fichier dans le conteneur Kubernetes fournit par défaut un stockage éphémère. Cela signifie qu'un redémarrage du
pod effacera toute les données sur ces conteneurs, et par conséquent, cette forme de stockage est, excepté dans le cas
d'applications triviales, relativement limitante. Un volume Kubernetes fournit un stockage permanent qui existe pendant la durée
d'existence du pod lui-même. Ce stockage peut également être utilisé comme disque partagé pour les conteneurs du pod. Ces
volumes sont montés à des points de montages spécifique à l'intérieur du conteneur définit par la configuration du pod, et ne
peuvent être montés aux autres volumes ou liés à ceux-ci. Le même volume peut être monté à différent endroits dans l'arbre du
système de fichiers par différents conteneurs.

Kubernetes fournit un partitionnement des ressources qu'il gère dans des ensembles disjoints appelés espace de noms. L'usage de
ces espaces de noms est destiné aux environnements possédant un grand nombre d'utilisateurs répartis dans plusieurs équipes, ou
projets, ou même à séparer des environnements tels que le développement, l'intégration et la production.

Un problème applicatif thématique est de décider ou stocker et gérer les informations de configuration, dont certaines peuvent
contenir des données sensibles. Les données de configuration peuvent être relativement hétérogènes comme de petits fichiers de
configurations individuels ou de grands fichiers de configuration ou des documents JSON/XML. Kubernetes permet de traiter ce
genre de problème à l'aide de deux mécanismes assez proches : "configmaps" et "secrets", les deux permettent des changements de
configuration sans nécessiter la reconstruction de l'application. Les données de configmaps et de secrets sont accessibles à
chaque objets de l'instance de l'application auxquels ils ont été liés au déploiement. Un secret et/ou un configmaps sont
uniquement envoyé à un noeud si un pod sur ce noeud le demande. Kubernetes le gardera en mémoire sur ce noeud. Un fois que le
pod qui dépend du secret ou de la configmap est supprimé, la copie en mémoire de tous les secrets et configmaps liés est
également supprimée. La donnée est accessible au pod de deux façons : en tant que variables d'environnements (que Kubernetes
créé lors du démarrage du pod) ou disponible dans le système de fichier du conteneur visible dans le pod.

La donnée elle même est stockée sur le maître qui est hautement sécurisé et dont personne ne doit avoir d'accès de connexion. La
plus grande différence entre un secret et une configmap est que le contenu de la donnée dans un secret est encodé en base 64. Il
peut également être chiffré. Les secrets sont souvent utilisés pour stocker des certificats, des mots de passe et des clefs ssh.

Il est très facile de réaliser une mise à l'échelle d'applications sans conditions d'état : Il suffit d'ajouter plus de pods
d'exécution - quelque chose que Kubernetes fait très bien. Les charges de travail avec condition d'état sont bien plus
difficiles, de fait que l'état doit être conservé si un pod est redémarré, et si l'application doit être redimensionnée alors
l'état devra possiblement être redistribué. Les bases de données sont des exemples de charge de travail avec condition d'état.
Lors d'une exécution en mode haute disponibilité, beaucoup de bases de données ont la notion d'instance primaire et
d'instance(s) secondaires. Dans ce cas la notion d'ordonnancement d'instances est important.

### Libvirt

Libvirt est une API open-source, ainsi qu'un daemon et un outil de gestion pour gérer des plateformes de virtualisation. Elle
peut être utilisée pour gérer KVM, Xen, VMware, ESXi, QEMU et autres technologies de virtualisation. Ces APIs sont utilisées
très largement au niveau de la couche d'orchestration des hyperviseurs pour le développement de solutions de clouds.

### Hyperviseurs

Un hyperviseur est un logiciel, microgiciel ou matériel qui créé et exécute des machines virtuelles. Un ordinateur sur lequel un
hyperviseur exécute une ou plusieurs machines virtuelles est appelé hôte, et chaque machine virtuelle est appelée invitée.
L'hyperviseur présente le système d'exploitation invité à l'aide d'une plateforme d'exploitation virtuelle et gère l'exécution
des systèmes d'exploitations invités. Plusieurs instances de divers systèmes d'exploitation peuvent partager les même ressources
matérielles virtualisées. A l'instar des mécanismes de virtualisation implémentés au niveau du système d'exploitation
(conteneurs), les systèmes d'exploitations invités ne doivent pas forcément partager un même noyau.

Il existe deux types d'hyperviseurs :

* **Type-1 natif** : Ces hyperviseurs s'exécutent directement sur le matériel de l'hôte pour contrôler le matériel et gérer les
systèmes d'exploitation invités.
* **Type-2 hébergés** : Ces hyperviseurs s'exécutent sur un système d'exploitation conventionnel comme n'importe quel autre
programme. Un système d'exploitation invité s'exécute en tant que processus de l'hôte. Les hyperviseur de type-2 créent une
abstraction pour les systèmes d'exploitation invités depuis le système d'exploitation hôte.

Cette distinction entre les deux types n'est pas toujours claire. Par exemple KVM et bhyve sont des modules noyau qui transforme
le système d'exploitation hôte en hyperviseur de type-1. En même temps, les distributions Linux et FreeBSD sont aussi des
systèmes d'exploitations conventionnels, où des applications entrent en concurrence les unes avec les autres pour les ressources
VM et ils peuvent être aussi catégorisé comme des hyperviseurs de type-2.

### Systemd

Systemd est un système et un gestionnaire de service pour les systèmes d'exploitation Linux. Lorsqu'il est exécuté en tant que
premier processus au démarrage, il agit comme le système init et met en place et gère les services de l'espace utilisateur. Des
instances séparées sont démarrées pour les utilisateurs connectés afin de démarrer leurs services.

D'habitude, systemd n'est pas invoqué directement par l'utilisateur, mais est installé comme le lien symbolique /sbin/init et
lancé au début du démarrage.

Lorsqu'il est exécuté en tant qu'instance système, systemd interprète le fichier de configuration system.conf et les fichier
présent dans les répertoires de system.conf.d ; lorsque lancé en tant qu'instance utilisateur, systemd interprète le fichier de
configuration user.conf et les fichiers dans les répertoires user.conf.d

systemd fournit un système de dépendances entre des entités variées appelées "unités" de 11 types différents. Les unités
encapsulent des objets utiles pour le démarrage et la gestion du système. La grande majorité des unités sont configurées dans
des fichiers de configuration d'unité, néanmoins certaines peuvent être créées automatiquement depuis d'autres configurations,
dynamiquement d'un état système ou de façon programmée à l'exécution. Une unité peut être "active" (i.e. démarrée, liée,
connectée, etc. selon le type d'unité), ou "inactive" (i.e. stoppée, déliée, déconnectée, etc.) ou bien dans le processus
d'activation ou de désactivation, c'est à dire entre les deux états. Un état spécial "échoué" est également disponible, très
similaire à "inactive" il se déclenche lorsque le service a échoué d'une certaine façon (le processus a renvoyé un code
d'erreur, ou a crashé, une opération a time out, ou après un nombre trop important de redémarrages). Si l'état apparaît, la
cause sera tracée pour référence ultérieure. Il est à noté que plusieurs types d'unité peuvent avoir un nombre de sous-états
additionnels, qui sont reliés aux 5 états d'unité généraux décrits ici.

Les unités suivantes sont disponibles :

1. Les unités de **services**, qui démarrent et contrôlent des daemons et les processus qui les composent.
2. Les unités de **sockets**, qui encapsulent des IPC locaux ou des sockets réseaux du système, utiles pour l'activation basée
sur des sockets.
3. Les unités de **cibles** utilisées afin de grouper des unités, ou de fournir des points de synchronisation connus au
démarrage.
4. Les unités de **périphériques** qui expose les périphériques de noyau dans systemd et peuvent être utilisées pour de
l'activation basée sur des périphériques.
5. Les unités de **montages**, pour contrôler les points de montage dans le système de fichiers.
6. Les unités de **montages automatiques**, afin d'automatiser les opérations de montage à la demande et le démarrage
parallélisé.
7. Les unités de **timers** afin de déclencher l'activation d'autres unités sur la base de timers.
8. Les unités de **swap**, très similaires aux unités de montages qui encapsulent les partitions mémoires dédiées au swap ou des
fichiers du système d'exploitation.
9. Les unités de **chemins** qui peuvent être utilisées afin d'activer d'autres service lorsque les objets du système de
fichiers changent ou sont modifiés.
10. Les unités de **tranches** qui sont utilisées afin de grouper des unités qui gèrent des processus systèmes dans un arbre
hiérarchique pour des questions de gestion des ressources.
11. Les unités de **scope** similaires aux unités de services mais pour gérer et démarrer des processus externes. Ils sont créés
de façon programmée en utilisant les interfaces de bus de systemd.

> Units : service, socket, target, device, mount, automount, timer, swap, path, slice, scope

Systemd chargera implicitement et automatiquement les unités depuis le disque - si celles-ci ne sont pas déjà chargées - dès
qu'elles seront requises par des opérations. Ainsi, le fait qu'une unité est chargée ou non est invisible aux clients. On
utilise `systemctl list-units --all pour lister de manière compréhensive les unités actuellement chargées. N'importe quelle
unité pour laquelle aucune de conditions ci-dessus ne s'applique est rapidement déchargée. Il est à noter que quand une unité
est déchargée de la mémoire courante ses données sont également libérées. Néanmoins les données ne sont généralement pas
perdues, du fait qu'un enregistrement dans le journal de log est généré déclarant les ressources consommées à l'arrêt d'une
unité.

Les processus lancés par systemd sont placés dans des cgroups nommés d'après l'unité à laquelle ils appartiennent dans la
hiérarchie privée de systemd. systemd fait usage de cela afin de garder efficacement la trace des processus. L'information du
Control group est gérée par le noyau, et est accessible dans la hiérarchie du système de fichier (sous /sys/fs/cgroup/systemd/),
ou bien dans des outils tels que systemd-cgls.

Systemd contient un système transactionnel minimal : si une unité doit s'activer ou se désactiver, il l'ajoutera avec toutes ses
dépendances dans une transaction temporaire. Il vérifiera alors si la transaction est cohérente (i.e. si l'ordonnancement de
toutes les unités ne contient pas de boucle). Si ce n'est pas le cas, systemd essaiera de résoudre le problème, et supprimera
les jobs non essentiels de la transaction ce qui pourra faire disparaître la boucle. Systemd essaiera également de supprimer les
jobs non essentiels qui arrêteraient un service déjà actif. Enfin il vérifiera si les jobs de la transaction contredisent des
jobs déjà en attente dans la file et optionnellement annulera la transaction. Si tout a marché et que la transaction est
cohérente et minimale dans son impact, elle est fusionnée avec tous les jobs en attente dans la file d'exécution. Cela signifie
qu'avant l'exécution d'une opération demandée, systemd vérifiera en premier lieu qu'elle ait un sens, essaiera de résoudre si
possible, et échouera uniquement s'il n'est pas possible de la faire fonctionner.

Il est à noter que les transactions sont générées indépendamment de l'état des unités à l'exécution, ce qui a pour conséquence
par exemple que si un job de démarrage est requis sur une unité déjà active, il génèrera quand même une transaction et
réveillera toute dépendance inactive (et causera par propagation la demande d'autres jobs tels que définis par leurs relations).
Ceci du fait que le job enfilé est comparé à l'exécution à l'état de l'unité cible et est marqué comme réussi et complet quand
les deux sont satisfaits. Cependant ce job ressort également les autres dépendances du fait de la relation définie et mène donc,
dans notre exemple, à l'enfilage des jobs de démarrage de toutes ces unités inactives.

Systemd contient des implémentation natives de tâches diverses faisant parti du processus de démarrage et ayant besoins d'être
exécutées. Par exemple, il fixe le nom de l'hôte ou configure le périphérique réseau de loopback. Il sert également à mettre en
place et monter plusieurs systèmes de fichiers d'API, tels que /sys/ ou /proc/.

### Netfilter

Netfilter est un framework fournit par le noyau Linux et qui permet diverses opérations en lien avec le réseau pouvant être
implémentées sous la forme de gestionnaires configurables. Netfilter offre plusieurs fonctions et opérations pour le filtrage
des paquets, la traduction d'adresses et de ports réseaux, ce qui fournit la fonctionnalité requise pour diriger les paquets à
travers le réseau et d'interdire à certains l'accès aux lieux sensibles d'un réseau.

Netfilter présente un ensemble de hooks à l'intérieur du noyau Linux, permettant à des modules du noyau spécifiques
d'enregistrer des fonctions de rappel avec la pile réseau du noyau. Ces fonctions s'appliquent généralement au traffic sous
forme de règles de modification et de filtrage qui sont appelées pour chaque paquet qui traverse le hook respectif dans la pile
réseau.

nftables est un sous-système du noyau Linux et la nouvelle partie de Netfilter qui fournit la classification et le filtrage des
paquets réseaux. Elles sont disponibles depuis le noyau Linux 3.13. nftables se substitue aux iptables, elles présentent les
avantages d'une moindre réplication du code et d'une plus grande simplicité d'extension pour de nouveaux protocoles. nftables
est configuré via l'utilitaire d'espace utilisateur *nft*.

Nftables utilise les blocs de construction de l'infrastructure Netfilter, tels que les hooks existant dans la pile réseau, la
connection au système de traçage, le composant d'enfilage de l'espace utilisateur, et le sous-système de logs.

Le moteur du noyau nftables ajoute une machine virtuelle simpliste au noyau Linux, capable d'exécuter un bytecode pour inspecter
un paquet réseau et prendre les décisions concernant la gestion de ce paquet. Elle peut obtenir des données de la part du paquet
lui-même, regarder les métadonnées associées (par exemple l'interface d'entrée), et gérer les données de traçage de connections.
Les opérateurs de comparaison arithmétiques et bits à bits peuvent être utilisés pour prendre des décisions en fonction de ces
données. La machine virtuelle est aussi capable de réaliser des manipulations sur des ensembles de donnéees (typiquement des
adresses IP), en permettant de remplacer de multiples opérations de comparaisons par un ensemble d'inspections.

Nftables offre une API améliorée côté espace utilisateur qui permet des remplacements atomiques d'une ou plusieurs règles
pare-feu dans une seule transaction Netlink. Ceci accélère les changements de configuration pare-feu pour les installations
ayant de grands ensembles de règles ; cela peut également permettre d'éviter des situations de compétition durant le changement
de règle.

### SELinux

SELinux (*Security Enhanced Linux*) est un module de sécurité du noyau Linux qui fournit des politiques de sécurité de contrôle
d'accès, dont le contrôle d'accès mandataire (MAC) en addition au contrôle d'accès discrétionnaire existant (DAC). SELinux
répond fondamentalement aux questions telles que :

"Est-ce que **le sujet** peut faire cette **action** à cet **objet** ?" e.g : "Est-ce qu'un serveur web peut accéder aux
fichiers dans le répertoire home des utilisateurs ?"

Un noyau Linux intégrant SELinux impose des politiques de contrôles d'accès mandataires qui confine les programmes utilisateurs
et les services système, ainsi que les accès aux fichiers et aux ressources réseaux. Limiter les privilèges au minimum requis
pour fonctionner réduit ou élimine les capacités de ces programmes et daemons à causer des dommages si ceux-ci sont compromis ou
défaillants (par exemple via des dépassements de tampons ou des mauvaises configurations). Ce mécanisme de confinement
fonctionne indépendamment des mécanismes de contrôle d'accès discrétionnaire traditionnels de Linux. Le concept de
superutilisateur n'existe pas, et ne partage pas les raccourcis bien connus des mécanismes de sécurité traditionnels, tel qu'une
dépendance aux binaires setuid/setgid.

Lorsqu'une application ou un processus, reconnu comme un sujet, effectue une requête d'accès à un objet, tel qu'un fichier,
SELinux vérifie à l'aide d'un cache de vecteur d'accès (AVC), où les permissions des sujets et des objets sont mises en cache.

Si SELinux ne trouve pas matière à trancher à propos de cet accès dans le cache, il envoit une requête au serveur de sécurité.
Le serveur de sécurité vérifie le contexte de sécurité de l'application ou du processus et du fichier. Le contexte de sécurité
est appliqué depuis la base de données de politiques SELinux. La permission est donnée ou refusée.

Si la permission est refusée, un message "avc: denied" sera visible dans */var/log.messages*.

Chaque processus et ressource système a une étiquette spéciale de sécurité appelée contexte SELinux. Un contexte SELinux est un
identifiant qui s'abstrait des détails du systèmes et se concentre sur les propriétés de sécurité de l'objet. Cela permet un
référencement des objets cohérent dans la politique SELinux mais supprime également toute ambiguité que l'on peut retrouver avec
d'autres méthodes d'identification ; par exemple, un fichier peut avoir plusieurs noms de chemins valides sur un système qui
utilise des montages liés.

La politique SELinux utilise ces contextes dans une série de règles qui définissent comment un processus peut intéragir avec les
autres ainsi que les différentes ressources systèmes. Par défaut, la politique ne permet aucune intéraction à moins qu'une régle
explicite en permette l'accès.

Le contexte SELinux contient plusieurs champs : utilisateur, role, type, et niveau de sécurité. L'information du type SELinux
est sans doute la plus importante quand il s'agit de la politique SELinux, du fait que la règle la plus commune de la politique
SELinux qui définit les intéractions autorisées entre les processus et les ressources systèmes utilise les types SELinux et non
le contexte en entier. Le type SELinux finit habituellement par *_t* (e.g. *httpd_t*).

## Bases de données

### Algèbre relationnelle

<TODO>

### Langage SQL

<TODO>

### Administration SGBD

<TODO>
