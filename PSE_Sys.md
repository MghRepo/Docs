# Système et bases de données

## Système

### Multitâche coopératif

Le multitâche coopératif est une forme simple de multitâche où chaque tâche doit explicitement permettre aux autres tâches de s'exécuter.
Cette approche simplifie l'architecture du système mais présente plusieurs inconvénients :
* Le multitâche coopératif est une forme de couplage fort. Si un des processus ne redonne pas la main à un autre processus, par exemple si le processus est buggé,
le système entier peut s'arrêter.
* Le partage de ressources (temps CPU, mémoire, accès disque, etc.) peut être inefficace.
### Multitâche préemptif

Le multitâche préemptif désigne la capacité d'un système d'exploitation à exécuter ou arrêter une tâche planifiée en cours.

Un ordonnanceur préemptif présente l'avantage d'une meilleure réactivité du système et de son évolution, mais l'inconvénient vient des situations de
compétition (lorsque le processus d'exécution accède à la même ressource avant qu'un autre processus (préempté) ait terminé son utilisation).

Dans un système d'exploitation multitâche préemptif, les processus ne sont pas autorisés à prendre un temps non défini pour s'exécuter dans le
processeur. Une quantité de temps définie est attribuée à chaque processus ; si la tâche n'est pas accomplie avant la limite fixée, le processus est
renvoyé dans la pile pour laisser place au processus suivant dans la file d'attente, qui est alors exécuté par le processeur. Ce droit de préemption
peut tout aussi bien survenir avec des interruptions matérielles.

Certaines tâches peuvent être affectées d'une priorité ; une tâche pouvant être spécifiée comme "préemptible" ou "non préemptible". Une tâche
préemptible peut être suspendue (mise à l'état "ready") au profit d'une tâche de priorité plus élevée ou d'une interruption. Une tâche non
préemptible ne peut être suspendue qu'au profit d'une interruption. Le temps qui lui est accordé est plus long, et l'attente dans la file d'attente plus
courte.

Au fur et à mesure de l'évolution des systèmes d'exploitation, les concepteurs ont quitté la logique binaire "préemptible/non préemptible" au profit
de systèmes plus fins de priorités multiples. Le principe est conservé, mais les priorités des programmes sont échelonnées.

Pendant la préemption, l'état du processus (drapeaux, registres et pointeurs d'instruction) est sauvé dans la mémoire. Il doit être rechargé dans le
processeur pour que le code soit exécuté de nouveau : c'est la commutation de contexte.

Un système d'exploitation préemptif conserve en permanence la haute main sur les tâches exécutées par le processeur, contrairement à un système
d'exploitation non préemptif, ou collaboratif, dans lequel c'est le processus en cours d'exécution qui prend la main et décide seul du moment où il la
rend. L'avantage le plus évident d'un système préemptif est qu'il peut en permanence décider d'interrompre un processus, principalement si celui-ci
échoue et provoque l'instabilité du système.

### Algorithmes d'ordonnancement

L'ordonnanceur désigne le composant du noyau du système d'exploitation choisissant l'ordre d'exécution des processus sur les processeurs d'un ordinateur.

Un processus a besoin de la ressource processeur pour exécuter des calculs ; il l'abandonne quand se produit une interruption, etc. De nombreux
anciens processeurs ne peuvent effectuer qu'un traitement à la fois. Pour les autres, un ordonnanceur reste nécessaire pour déterminer quel
processus sera exécuté sur quel processeur (c'est la notion d'affinité, très importante pour ne pas dégrader les performances). Au-delà des
classiques processeurs multicoeurs, la notion d'hyperthreading rend la question de l'ordonnancement encore un peu plus complexe.

A un instant donné, il y a souvent davantage de processus à exécuter que de processeurs.

Un des rôles de système d'exploitation et plus précisément de l'ordonnanceur du noyau, est de permettre à tous ces processus de s'exécuter à un
moment ou un autre et d'utiliser au mieux le processeur pour l'utilisateur. Pour que chaque tâche s'exécute sans se préoccuper des autres et/ou aussi
pour exécuter les tâches selon les contraintes imposées au système, l'ordonnanceur du noyau du système effectue des commutations de contexte de celui-ci.

A intervalles réguliers, le système appelle une procédure d'ordonnancement qui *élit* le prochain processus à exécuter. Si le nouveau processus est
différent de l'ancien, un changement de contexte (opération consistant à sauvegarder le contexte d'exécution de l'ancienne tâche comme les registres
du processeur) a lieu. Cette structure de données est généralement appelée PCB. Le système d'exploitation restaure l'ancien PCB de la tâche élue,
qui s'exécute alors en reprenant là où elle s'était arrêtée précedemment.

Du choix de l'algorithme d'ordonnancement dépend le comportement du système. Il existe deux grandes classes d'ordonnancement :
* *L'ordonnancement en temps partagé* présent sur la plupart des ordinateurs "classiques". Par exemple l'ordonnancement "decay" ; qui est celui par défaut
sous Unix. Il consiste en un système de priorités adaptatives, par exemple il privilégie les tâches interactives pour que leur temps de réponse soit bon.
Une sous-classe de l'ordonnancement en temps partagé sont les ordonnanceurs dits "proportional share", eux sont plus destinés aux stations de calcul et
permettent une gestion rigoureuse des ressources. On peut citer notamment "lottery" et "stride".
* *L'ordonnancement en temps réel* qui assure qu'une certaine tâche sera terminée dans un délai donné. Cela est indispendable dans les systèmes embarqués.
Un ordonnanceur temps réel est dit optimal pour un système de tâches s'il trouve une solution d'ordonnancement du système lorsque cette solution
existe. S'il ne trouve pas de solution à ce système, alors aucun autre ordonnanceur ne peut en trouver une.

Algorithmes d'ordonnancement :
* *Round-robin (ou méthode du tourniquet)* : Une petite unité de temps appelé quantum de temps est définie. La file d'attente est gérée comme une file
circulaire. L'ordonnanceur parcourt cette file et alloue un temps processeur à chacun des processus pour un intervalle de temps de l'ordre d'un quantum au
maximum. La performance de round-robin dépend fortement du choix du quantum de base.
* *Rate-monotonic scheduling (RMS)* : L'ordonnancement à taux monotone est un algorithme d'ordonnancement temps réel en ligne à priorité constante (statique).
Il attribue la priorité la plus forte à la tâche qui possède la plus petite période. RMS est optimal dans le cadre d'un système de tâches périodiques,
synchrones, indépendantes et à échéance sur requête avec un ordonnanceur préemptif. De ce fait, il n'est généralement utilisé que pour ordonnancer des tâches
vérifiant ces propriétés.
* *Earliest deadline first scheduling (EDF)* : C'est un algorithme d'ordonnancement préemptif à priorité dynamique, utilisé dans les systèmes temps réels.
Il attribue une priorité à chaque requête en fonction de l'échéance de cette dernière selon la règle : Plus l'échéance d'une tâche est proche, plus sa
priorité est grande. De cette manière, au plus vite le travail doit être réalisé, au plus il a des chances d'être exécuté.
* *FIFO* : Les premièrs processus ajoutés à la file seront les premières à être exécutés.
* *Shortest job first (SJF, ou SJN-Shortest Job Next)* : Le choix se fait en fonction du temps d'exécution estimé du processus. Ainsi l'ordonnanceur va laisser
passer en priorité le plus court des processus de la file d'attente.
* *Completely Fair Scheduler* (CFS) : L'ordonnanceur des tâches pour le noyau Linux. Il gère l'allocation de ressource processeur pour l'exécution des processus,
en maximisant l'utilisation globale CPU tout en optimisant l'interactivité. CFS utilise un arbre rouge-noir implémentant une chronologie des futures exécutions
des tâches. En effet, l'arbre trie les processus selon une valeur representative du manque de ces processus en temps d'allocation du processeur, par rapport au
temps qu'aurait alloué un processeur dit mutlitâche idéal. De plus, l'ordonnanceur utilise une granularité temporelle à la nanoseconde, rendant redondante la
notion de tranche de temps, les unités atomiques utilisées pour le partage du CPU entre processus. Cette connaissance précise signifie également qu'aucune
heuristique (basée sur la statistique, donc pouvant commettre des erreurs) n'est requise pour déterminer l'interactivité d'un processus.

### Synchronisation

En programmation concurrente, la synchronisation se réfère à deux concepts distincts mais liés : la synchronisation de processus et la
synchronisation des données. La synchronisation de processus est un mécanisme qui vise à bloquer l'exécution de certains processus à des points
précis de leur flux d'exécution, de manière que tous les processus se rejoignent à des étapes relais données, tel que prévu par le programmeur. La
synchronisation de données, elle, est un mécanisme qui vise à conserver la cohérence des données telles que vues par différents processus, dan
un environnement multitâche. Initialement, la notion de synchronisation est apparue pour la synchronisation de données.

Ces problèmes dits "de synchronisation" et même plus généralemen ceux de communication inter-processus dont ils dépendent rendent
pratiquement toujours la programmation concurrente plus difficile. Certaines méthode de programmation, appelées synchronisation non-blocante,
cherchent à éviter d'utiliser des verrous, mais elles sont encore plus difficiles à mettre en oeuvre et nécessitent la mise en place de structure de
données particulières. La mémoire transactionelle logicielle en est une.

La synchronisation de processis cherche par exemple à empêcher des programmes d'exécuter la même partie de code en même temps, ou au
contraire forcer l'exécution de deux partie de code en même temps. Dans la première hypothèse, le premier processus bloque l'accès à la section
critique avant d'y entrer et libère l'accès après y être sorti. Ce mécanisme peut être implémenté de multiples manières.

Ces mécanismes sont par exemple la barrière de synchronisation, l'usage conjoint des sémaphores et des verrous, les spinlock, le moniteur.

* *Barrière de synchronisation* : Permet de garantir qu'un certain nombre de tâches aient passé un point spécifique. Ainsi, chaque tâche qui arrivera
sur cette barrière devra attendre jusqu'à ce que le nombre spécifié de tâches soient arrivées à cette barrière.
* *Sémaphore* : Variable partagée par différents "acteurs" qui garantit que ceux-ci ne peuvent accéder de façon séquentielle à travers des opérations
atomiques, et constituela méthode utilisée couramment pour restreindre l'accès à des ressources partagées et synchroniser les processus dans un environnement
de programmation concurrente.
* *Verrous* : Permet d'assurer qu'un seul processus accède à une ressource à un instant donné. Un verrou peut être posé pour protéger un accès en lecture et
permettra à plusieurs processus de lire, mais aucun d'écrire. On dit alors que c'est un verrou partagé. Un verrou est dit exclusif lorsqu'il interdit toute
écriture et toute lecture en dehors du processus qui l'a posé. La granularité d'un verrou constitue l'étendue des éléments qu'il protège. Par exemple dans les
bases de données, un verrou peut être posé sur une ligne, un lot de ligne, une table etc.
* *Spinlocks* : Mécanisme simple de synchronisation basé sur l'attente active.
* *Moniteur* : Mécanisme de synchronisation qui permet à plusieurs threads de bénéficier de l'exclusion mutuelle et la possibiliter d'attendre (*block*)
l'invalidation d'une condition. Les moniteurs ont également un mécanisme qui permet aux autres threads de signaler que leur condition est validé. Il
est constitué d'un mutex et de variables conditionnelles.

La connaissance des dépendances entre les données est fondamentale dans la mise en oeuvre d'algorithmes parallèles, d'autant qu'un calcul peut
dépendre de plusieurs calculs préalables. Les *conditions de Bernstein* permettent de déterminer les conditions sur les données lorsque deux parties
de programme peuvent être exécutées en parallèle.

### Signaux

Un signal est une forme de communication entre processus utilisée par les systèmes de type Unix et ceux respectant les standards POSIX.
Il s'agit d'une notification asynchrone envoyée à un processus pour lui signaler l'apparition d'un événement. Quand un signal est envoyé à un
processus, le système d'exploitation interrompt l'exécution normale de celui-ci. Si le processus possède une routine de traitement pour le signal reçu,
il lance son exécution. Dans le cas contraire, il exécute la routine des signaux par défaut.

La norme POSIX (et la documentation de Linux) limite les fonctions directement ou indirectement appelables depuis cette routine de traitements des
signaux. Cette norme donne une liste exhaustive de fonctions primitives dites *async-signal safe* (en pratique les appels systèmes) qui sont les
seules à pouvoir être appelées depuis une routine de traitement de signal sans avoir un comportement indéfini. Il est donc suggéré d'avoir une
routine de traitement de signal qui positionne simplement un drapeau déclaré *volatile sig_atomic_t* qui serait testé ailleurs dans le programme.

L'appel système kill(2) permet d'envoyer, si cela est permis, un signal à un processus. La commande kill(1) utilise cet appel système pour faire de
même depuis le shell. La fonction raise(3) permet d'envoyer un signal au processus courant.

Les exceptions comme les erreurs de segmentation ou les divisions par zéro génèrent des signaux. Ici les signaux générés seront respectivement
SIGSEGV et SIGFPE. Un processus recevant ces signaux se terminera et générera un core dump par défaut.

Le noyau peut générer des signaux pour notifier les processus que quelque chose s'est passé. Par exemple, SIGPIPE est envoyé à un processus qui
essaye d'écrire dans un pipe qui a été fermé par celui qui lit. Par défaut, le programme se termine alors. Ce comportement rend la construction de
pipeline en shell aisée.

### Socket réseau

Une socket réseau est une structure logicielle comprise dans un noeud réseau qui sert de point d'arrivée pour les données envoyées et reçues à travers le réseau.
La structure et les propriétés d'une socket sont définies par une interface de programmation (API) de l'architecture réseau. Les sockets sont créées uniquement
durant l' intervalle de temps d'un processus d'une application s'exécutant dans le noeud.

Du fait de la standardisation des protocoles TCP/IP au cours du développement d'internet, le terme *socket réseau* est plus communément utilisé dans le contexte
de la *Suite des protocoles Internets*. On parle alors aussi de socket internet. Dans ce contexte, une socket est identifiée extérieurement par les autres machines
par son *addresse socket*, qui est la triade du protocole de transfert, de l'addresse IP et du numéro de port.

Une pile de protocole, habituellement fournie par le système d'exploitation est un ensemble de services permettant aux processus de communiquer à travers un réseau
utilisant les protocoles que la pile implémente. Le système d'exploitation fait passer les données utiles des paquets IP entrants à l'application correspondante
en lisant l'information de l'addresse socket des entêtes des protocoles IP et transport et en enlevant ces entêtes des données applications.

L'interface de programmation que les programmes utilisent pour communiquer avec la pile de protocole, utilisant les socket réseau, est appelée **socket API**.
Les API sockets internet sont généralement basées sur le standard socket de Berkeley. Dans le standard socket de Berkeley, les socket sont une forme de descripteur
de fichier (*read, write, open, close*).

Dans les protocoles internet standards TCP et UDP, une adresse socket est la combinaison d'une addresse IP et d'un numéro de port. Les sockets n'ont pas besoin
d'adresse source, mais si un programme lie la socket à une adresse source, la socket peut être utilisée pour recevoir et envoyer des données à cette adresse.
Basé sur cette adresse, les sockets internet délivrent les paquets applicatifs entrants au processus applicatif approprié.

Plusieurs types de sockets internet sont disponibles :

* **Datagram** : Des sockets non connectées, qui utilisent le protocole UDP (*User Datagram Protocol*). Chaque paquet envoyé ou reçu avec un socket datagramme est
adressé et routé individuellement. L'ordre ainsi que la fiabilité ne sont pas garantis, par conséquent plusieurs paquet envoyé depuis un processus à l'autre peut
arriver dans n'importe quel ordre ou bien ne pas arriver du tout. Certaines configurations spéciales peuvent être requises pour envoyer en broadcast un socket
datagramme.
* **Stream** : Des socket connectés, qui utilisent les protocoles TCP (*Transmission Control Protocol*), SCTP (*Stream Control Transmission Protocol*) ou DCCP
(*Datagram Congestion Control Protocol*). Un socket flux fournit un flot de données sans erreurs, séquencé, unique et ininterrompu avec des mécanismes prédéfinis
pour créer et détruire des connections et rapporter des erreurs. Un socket flux transmet des données de manière fiable, ordonnée sans requerir l'établissement
préalable d'un canal de communication.
* **Raw** : Permet l'envoie et la réception de paquets IP sans aucun formatage spécifique à un protocole de la couche transport. Avec les autres types de socket,
la donnée est automatiquement encapsulée selon le protocole de la couche transport choisi (TCP, UDP etc.), et l'utilisateur du socket n'a pas connaissance de
l'existence des entêtes du protocole. Quand on lit d'un socket brut, les entêtes sont généralement inclus. Lorsqu'on transmet des paquets depuis un socket brut,
l'addition automatique d'une entête est optionnelle.

### Socket IPC

Un socket de domaine Unix ou socket IPC (*inter-process communication*) et un point d'arrivée des données de communications qui permet d'échanger des données entre
des processus s'exécutant sur le même système d'exploitation hôte. Les types de socket valides dans le domaine UNIX sont :

* **SOCK_STREAM** (à comparer au TCP) - pour un socket orienté flux
* **SOCK_DGRAM** (à comparer à UDP) - pour un socket orienté datagramme qui préserve les limites des messages (comme la plupart des implémentations UNIX, les socket
de domaine UNIX datagram sont toujours fiables et ne réordonnent pas les datagrammes)
* **SOCK_SEQPACKET** (à comparer à SCTP) - pour un socket à paquets séquencés orienté connection, qui préserve les limites des messages, et livre les paquets dans
l'ordre d'envoi.

Les sockets de domaine Unix sont une composante standard des systèmes d'exploitation POSIX.

Les interfaces de programmation (API) pour les sockets de domaine Unix sont similaires à celles des sockets internet, mais au lieu d'utiliser un protocole réseau
sous-jacent, toutes les communications se placent à l'intérieur du noyau du système d'exploitation. Les socket de domaine Unix peuvent utiliser le système de 
fichiers comme adresse d'espace de noms. (Certains systèmes d'exploitation, comme Linux, offrent des espaces de noms additionnels.) Les processus référencent les
sockets de domaine Unix comme des inodes du système de fichier, ainsi 2 processus peuvent communiquer en ouvrant la même socket.

En plus de permettre l'envoi de données, les processus peuvent envoyer des descripteurs de fichiers à traver une connection de socket de domaine Unix en utilisant 
les appels systèmes sendmsg() et recvmsg(). Ceci permet au processus qui envoit d'autoriser le processus qui reçoit à accéder au descripteur de fichier auquel 
autrement le processus qui reçoit n'a pas accès. Ceci permet d'implémenter une forme rudimentaire de sécurité basée sur l'accessibilité.

### Socket Netlink

La famille de socket Netlink est une interface du noyau linux utilisée pour des communications inter-processus entre les processus de l'espace utilisateur et
du noyau et entre différents processus utilisateurs. La différence entre les sockets Netlink et les socket IPC et qu'au lieu d'utiliser l'espace de noms du système
de fichier, les processus Netlink sont généralement désignés par leur PID.

Netlink fournit une interface socket standard pour les processus utilisateurs, et une API côté noyau pour un usage interne par les modules du noyau.

### Tube anonyme

Un tube anonyme est un mécanisme de gestion de flux de donnée. Ce mécanisme inventé pour UNIX est principalement utilisé dans la communication inter-processus.
Ouvrir un tube anonyme permet de créer un flux de donnée unidirectionnel FIFO entre un processus et un autre. Ces tubes sont détruits lorsque le processus qui les
a créés disparaît, contrairement aux tubes nommés qui sont liés au système d'exploitation et qui doivent être explicitement détruits.

Ce mécanisme permet la création de filtres.

Pour les système d'exploitation de type Unix, un tube anonyme est créé grâce à un appel système qui retourne un descripteur de fichier à la suite de la création
d'un Fork qui permet d'assigner à chacun des processus son rôle de récepteur ou d'émetteur.

### Tube nommé

Comme les tubes anonymes, les tubes nommés sont des zones de données organisées en FIFO mais contrairement à ceux-ci qui sont détruits lorsque le processus qui les
a créés disparait, les tubes nommés sont liés au système d'exploitation et ils doivent être explicitement détruits.

### Passage de messages

Le modèle de passage de messages et une technique permettant de demander l'exécution d'un programme. Le passage de message utilise un modèle objet afin de
distinguer la fonction générale de ses implémentations spécifiques. Le programme appelant envoit un message et se fie à l'objet afin de sélectionner et d'exécuter
le code approprié. L'utilisation d'une couche intermédiaire, est justifiée par des besoins de distribution et d'encapsulation.

L'encapsulation suit l'idée que les objets logiciels devraient être capables d'invoquer les services d'autres objets sans avoir aucune connaissance spécifique
de leurs implémentations. L'encapsulation permet de réduire les lignes de codes ainsi qu'une plus grande maintenabilité des systèmes.

Le passage de messages distribué permet au développeur, à l'aide d'une couche fournissant les services de base de construire des systèmes constitués de sous-
systèmes s'exécutant sur des ordinateurs disparates, à différents endroit et à des horaires différents. Lorsqu'un objet distribué envoie un message, la couche 
message s'occupe de :

* Trouver d'où et de quel processus le message est issu.
* Sauvegarder le message dans une file si l'objet approprié au traitement du message n'est pas en cours d'exécution et s'occuper de l'envoyer dès que l'objet est
disponnible. Ainsi que de stocker le résultat si besoin, jusqu'à ce que l'objet qui a envoyé le message est prêt à le recevoir.
* Contrôler diverses dépendances transactionnelles pour les transactions distribuées.

### Fichier mappé en mémoire

Un fichier mappé en mémoire est un segment de mémoire virtuelle qui est la copie d'une portion de fichier ou d'une ressource de type fichier. Cette ressource est
typiquement un fichier présent sur le disque, mais cela peut également être un périphérique, un objet en mémoire partagée, ou toute autre ressource que le système
d'exploitation peut référencer à l'aide d'un descripteur de fichier. Une fois présente en mémoire, cette corrélation entre le fichier et l'espace mémoire permet
aux applications de traiter la partie mappée comme s'il s'agissait de la mémoire primaire.

Le bénéfice d'utiliser le mappage en mémoire est d'augmenter les performances d'entrée/sortie notamment sur les fichiers de gros volume. Pour les petits fichiers,
les fichiers mappés peuvent engendrer des problèmes de fragmentation interne du fait que les maps mémoires sont toujours aligné sur la taille de la page
(généralement 4Ko). Par conséquent, un fichier de 5Ko allouera 8Ko et gachera 3Ko. Accéder aux fichiers mappés en mémoire est plus rapide que d'utiliser des
opérations de lecture et d'écriture directement pour 2 raisons. Premièrement, un appel système est bien plus lent qu'un accès vers la mémoire locale du programme.
Deuxièmement, dans la plupart des systèmes d'exploitations la région mémoire mappée est la page cache du noyau, c'est à dire que cela ne nécessite aucune copie en
espace utilisateur.

Le processus de mapping mémoire est géré par le gestionnaire de mémoire virtuelle, qui est le même sous-système responsable de la pagination. Les fichiers mappés
sont chargés une page entière à la fois. La taille de la page est choisi par le système d'exploitation pour un maximum de performance. Sachant que la pagination
est un élément critique du gestionnaire de mémoire virtuelle, le chargement des portions de la taille d'une page d'un fichier en mémoire physique est généralement 
une fonction système très optimisée.

L'usage le plus commun de fichier mappé en mémoire est le chargement de processus. Lors de la création d'un processus, le système d'exploitation utilise un fichier
mappé en mémoire pour faire apparaître le fichier exécutable ainsi que tous les modules chargeable en mémoire pour exécution. La technique la plus utilisée est
la demande de pages, le fichier est chargé en mémoire physique par section (chacune d'une page), et seulement quand cette page est référencée. Dans le cas spécifique
des exécutables, cela permet à l'OS de charger de manière sélective uniquement les portions de l'image processus qui nécessitent réellement une exécution.

Un autre usage répandu pour les fichiers mappés en mémoire est le partage de fichiers entre processus multiples. Ceci permet d'eviter les fautes de pages ainsi que
les violations de segmentation.

### Partitionnement de la mémoire

Le partitionnement de la mémoire est la création d'une ou plusieurs régions dans la mémoire secondaire, telle que chaque région puisse être gérée séparément. Ces
régions sont appelées partitions. C'est généralement la première chose à faire sur un nouveau disque installé, avant qu'un système de fichier soit créé. Le
disque stocke l'information sur le positionnement et la taille des partitions dans une aire appelée table de partition que le système d'exploitation lit avant
toute autre région du disque. Chaque partition apparait alors comme un disque "logique" du point de vue du système d'exploitation qui fait usage d'une partie du 
disque réel. Les administrateurs systèmes utilisent alors un programme appelé éditeur de partitions pour créer, retailler, supprimer et manipuler les partitions.
Le partitionnement permet l'usage de différents systèmes de fichiers afin de stocker tout types de fichiers. Séparer les données utilisateur des données système
permet d'empêcher que la partition système soit pleine ce qui rendrait le système inutilisable. Le partitionnement permet aussi de simplifier la sauvegarde.
Un désavantage du partitionnement est qu'il peut être difficile d'allouer la taille adéquate à chacune des partitions, ce qui peut avoir pour conséquence de
laisser une partition avec énormément d'espace libre et une autre totalement saturée.

### Mémoire virtuelle

Le principe de mémoire virutelle repose sur l'utilisation de traduction à la volée des adresses virtuelles vue du logiciel, en adresses physiques de mémoire vive.
La mémoire virtuelle permet :

* d'utiliser de la mémoire de masse comme extension de la mémoire vive ;
* d'augmenter le taux de multiprogrammation ;
* de mettre en place des mécanismes de protection de la mémoire ;
* de partager la mémoire entre processus.

### Pagination

* Les adresses mémoires émises par le processeur sont des adresses virtuelles, indiquant la position d'un mot dans la mémoire virtuelle.
* Cette mémoire virtuelle est formée de zones de même taille, appelées pages. Une adresse virtuelle est donc un couple (numéro de page, déplacement dans la page).
La taille des pages est une puissance entière de deux, de façon à déterminersans calcul le déplacement (10 bits de poids faible de l'adresse virtuelle pour des pages
de 1024 mots), et le numéro de page (les autres bits).
* La mémoire vive est également composées de zones de même taille, apellées cadres (*frames*), dans lesquelles prennent place les pages (un cadre contient une page :
taille d'un cadre = taille d'une page). La taille de l'ensemble des cadres en mémoire vive utilisés par un processus est appelé *Resident set size*.
* Un mécanisme de traduction (*translation*) assure la conversion des adresses virtuelles en adresses physiques, en consultant une table des pages (*page table*) pour
connaître le numéro du cadre qui contient la page recherchée. L'adresse physique obtenue est le couple (numéro de cadre, déplacement).
* Il peut y avoir plus de pages que de cadres (c'est là tout l'intêret) : les pages qui ne sont pas en mémoire sont stockées sur un autre support (disque), elle
seront ramenées dans un cadre quand on en aura besoin.

La table des pages est indexée par le numéro de page. Chaque ligne est appelée "entrée dans la table des pages (*pages table entry* PTE), et contient le numéro de
cadre. La table des pages pouvant être située n'importe où en mémoire, un registre spécial (PTBR pour *Page Table Base Register*) conserve son adresse.

En pratique, le mécanisme de traduction fait partie d'un circuit électronique appelé MMU (*memory management unit*) qui contient également une partie de la table des
pages, stockée dans une mémoire associative formée de registres rapides. Ceci évite d'avoir à consulter la table des pages (en mémoire) pour chaque accès mémoire.

### Segmentation

La segmentation est une technique de découpage de la mémoire. Elle est gérée par l'unité de segmentation de l'unité de gestion mémoire (*MMU*), utilisée sur les
systèmes d'exploitation modernes, qui divise la mémoire physique (dans le cas de la segmentation pure) ou la mémoire virtuelle (dans le cas de la segmentation avec
pagination) en segments caractérisés par leur adresse de début et leur taille (*décalage*).

La segmentation permet la séparation des données du programme (entre autres segments) dans des espaces logiquement indépendants facilitant alors la programmation,
l'édition de liens et le partage inter-processus. La segmentation permet également d'offrir une plus grande protection grâce au niveau de privilège de chaque segment.

Lorsque l'unité de gestion mémoire (MMU) doit traduire une adresse logique en adresse linéaire, l'unité de segmentation doit dans un premier temps utiliser la
première partie de l'adresse, c'est à dire le selecteur de segment, pour retrouver les caractéristiques du segment (base, limit, DPL, etc.) dans la table de
descripteurs (GDT ou LDT). Puis il utilise la valeur de décalage qui référence l'adresse à l'intérieur du segment.

Il existe sur la majorité des processeurs actuels, des registres de segments (CS, DS, SS, etc.) qui contiennent le sélecteur de segment dernièrement utilisé par le
processeur qui sont utilisés pour accélérer l'accès à ces selecteurs.

Sur les processeurs récents, il existe également des registres associés à chaque registre de segment qui contiennet le descripteur de segment associé pour un accès
plus rapide aux descripteurs.

Un segment mémoire est un espace d'adressage indépendant défini par deux valeurs :

* L'adresse où il commence (aussi appelée *base*, ou *adresse de base*)
* Sa taille ou son *décalage* (aussi appelée *limite* ou *offset*)

Un segment constitue donc dans la mémoire principale un plage d'adresse continue.

Une adresse logique d'une donnée désirée est donc exprimée sous la forme (*segment, décalage*), le segment étant ré

### Types de périphériques

### Bus

### Accès à la mémoire

### Pilotes

### Shell

### Cgroups

Les cgroups sont une fonctionnalité du noyau linux qui limite, compte et isole l'utilisation des ressources (CPU, mémoire, Entrée/sortie, réseau, etc.) d'une
collection de processus.

Ces groupes de contrôle peuvent être utilisés de multiples façons :

* En accédant le système de fichier virtuel du cgroup manuellement.
* En créant et gérant des groupes à la volée en utilisant des outils tels que *cgcreate*, *cgexec* et *cgclassify* (*libcgroup*).
* A travers le "daemon moteur de règles" qui peut déplacer les processus de certains utilisateurs, groupes de manière automatique ou commander aux cgroups comme cela
a été spécifié dans sa configuration.
* Indirectement à travers d'autres logiciels utilisant les cgroups, tels que Docker, LXC, libvirt, systemd, etc.

### Espaces de noms

Un espace de noms encapsule dans une abstraction une ressource système globale qui la fait apparaître aux processus contenus dans l'espace de noms qui ont leur
propre instance isolée de la ressource globale. Les changements de la ressource globale sont visibles aux autres processus membre de l'espace de noms, mais invisible
aux autres processus. Les espaces de noms sont utilisés pour implémenter les conteneurs.

Les types d'espace de noms disponnibles dans Linux sont les suivants : Cgroup, IPC, Network, mount, PID, Time, User, UTS.

### Systemd-nspawn

Systemd-nspawn peut être utilisée pour exécuter une commande ou un OS dans un conteneur léger d'espace de noms. Il est plus puissant que *chroot* puisqu'il virtualise
la hiérarchie du système de fichier, mais aussi l'arbre de processus, les différents sous-systèmes IPC ainsi que le nom de l'hôte et du domaine.

Systemd-nspawn limite l'accès en lecture seule à différentes interfaces du noyau dans le conteneur, telles que **/sys**, **/proc/sys** ou **/sys/fs/selinux**.
Les interfaces réseaux et l'horloge système ne peuvent pas être modifiées depuis l'intérieur du conteneur. Les fichiers spéciaux ou fichiers de périphérique ne
peuvent  pas non plus être créés. Le système hôte ne peut pas être redémarrer et des modules du noyau ne peuvent pas être chargés depuis le conteneur.

Les conteneurs ainsi créés peuvent être gérés à l'aide de la commande *machinectl*.

### Conteneurisation LXC

LXC est une méthode de virtualisation au niveau du système d'exploitation permettant d'exécuter plusieurs système isolés Linux sur un système hôte de contrôle
en utilisant un unique noyau Linux.

Le noyau Linux fournit la fonctionnalité des cgroups qui permet une limitation et une priorisation des ressources (CPU, mémoire, entrées/sorties, réseau, etc.) sans
besoin d'aucune machine virtuelle, ainsi que la fonction d'isolation par espace de noms qui permet l'isolation complète d'une application du point de vue de
l'environnement opérant, incluant l'arbre des processus, la configuration réseau, les identifiants utilisateurs et les systèmes de fichiers montés.

LXC combine les cgroups du noyau et inclut l'isolation des espaces de nom pour fournir un environnement isolé à des applications.

LXC permet d'exécuter des conteneurs en tant que simple utilisateur sur l'hôte à l'aide des conteneurs dits "non-privilégiés".

### Conteneurisation Docker

Docker est un ensemble de produits de Plateforme en tant que Service qui utilise la virtualisation au niveau du système d'exploitation pour livrer des logiciels
dans des paquets appelé conteneurs. Les conteneurs sont isolés les uns des autres et embarquent leurs propres logiciels, bibliothèques et fichiers de configuration ;
ils peuvent communiquer entre eux à travers des cannaux bien définis. Tous les conteneurs sont exécuté par un noyau de système d'exploitation unique et par conséquent
utilisent moins de ressources que des machines virtuelles.

Docker peut empaqueter une application et ses dépendances dans un conteneur virtuel qui peut s'exécuter sur n'importe quel ordinateur Linux, Windows, ou macOS. Ceci
permet à l'application de s'exécuter dans toute une variété d'environnement, par exemple, sur sa propre machine, dans un cloud public ou privé. Quand il s'exécute
sur Linux, Docker utilise les mécanismes d'isolation fournis par le noyau (tels que les cgroups et les espaces de noms) et les systèmes de fichiers capables de
montage en union, pour permettre aux conteneurs de s'exécuter sur une intance Linux unique, évitant la surcharge du démarrage et de la maintenance de machines
virtuelles.

Le support des espaces de nom du noyau Linux permet d'isoler l'environnement système de la vue de l'application comme l'arbre des processus, le réseau, les
identifiants utilisateurs et les systèmes de fichiers montés, tandis que les cgroups fournissent la limitation de ressources mémoire et CPU. Docker inclut également
sa propre bibliothèque *libcontainer* permettant d'accéder directement les fonctions de virtualisations fournient par le noyau linux en plus de l'utilisation
d'interfaces de virtualisations telles que *libvirt*, *LXC* et *systemd-nspawn*.

Docker implément une API de haut niveau pour fournir des conteneurs légers exécutant des processus isolés.

Le logiciel Docker en tant qu'offre de services consiste en trois composants :

* Le Logiciel : Le daemon Docker, *dockerd*, est un processus persistant qui gère les conteneurs Docker ainsi que les objets du conteneur. Le daemon est à l'écoute
des requêtes envoyés via l'API du Docker Engine. Et le programme client *docker* fournit une interface de ligne de commande qui permet d'intéragir avec le daemon.
* Les objets : Les objets docker sont des entités diverses utilisées pour assembler une application dans Docker. Les classes principales des objets Dockers sont
les images, les conteneurs et les services.
    * Un conteneur Docker est un environnement encapsulé, standardisé qui exécute des applications. Un conteneur est géré à travers l'API Docker ou la ligne de
    commande.
    * Une image Docker est un modèle en lecture seule utilisée pour construire des conteneurs. Les images sont utilisées pour stocker et envoyer des applications.
    * Un service Docker permet aux conteneurs de se déployer sur de multiples daemons Docker. Ceci étant appelé une nuée (*swarm*), un ensemble de daemon coopérant,
    communiquant à travers l'API Docker.
* Les registres : Un registre Docker est un dépot d'image Docker. Les clients Docker se connectent aux registres pour cloner (*pull*) des images à utiliser ou déposer
(*push*) des images qu'ils ont contruites. Les dépots peuvent être publics ou privés. Les deux principaux dépots publics sont Docker Hub et Docker Cloud. Docker Hub
est le registre par défaut ou Docker recherche des images. Des registres Docker permettent également la création de notifications basée sur des évennements.

Le logiciel Docker dispose également d'outils :

* Docker Compose qui est un outil qui permet de définir et d'exécuter des applications Docker multi-conteneurs. Il utilise des fichier YAML pour configurer les
services de l'application et créé et démarre les processus de tous les conteneurs à l'aide d'une seule commande. L'interface en ligne de commande *docker-compose*
permet aux utilisateurs de passer des commandes sur des ensembles de conteneurs, par exemple pour construire des images, déployer des conteneurs, redémarrer des
conteneurs, etc.

### Orchestrateur Kubernetes

### Libvirt

### Hyperviseurs

### Netfilter

### SELinux

## Bases de données

### Algèbre relationnelle

### Langage SQL

### Administration SGBD
