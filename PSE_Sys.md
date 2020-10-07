# Système et bases de données

## Système

### Multitâche coopératif

Le multitâche coopératif est une forme simple de multitâche où chaque tâche doit explicitement permettre aux autres tâches de s'exécuter.
Cette approche simplifie l'architecture du système mais présente plusieurs inconvénients :
* Si un des processus ne redonne pas la main à un autre processus, par exemple si le processus est buggé, le système entier peut s'arrêter.
* Le partage de ressources (temps CPU, mémoire, accès disque, etc.) peut être inefficace.
* Le multitâche coopératif est une forme de couplage fort.

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
heuristique (basée sur la statistique, donc pouvant commettre des erreurs) n'est requise pour déterminer l'interactivité d'un processus

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

Ces mécanismes sont par exemple la barrière de synchronisation, l'usage conjoint des sémaphores et des verrous, les spinlock, le moniteur, les mutex.

* *Barrière de synchronisation* : Permet de garantir qu'un certain nombre de tâches aient passé un point spécifique. Ainsi, chaque tâche qui arrivera
sur cette barrière devra attendre jusqu'à ce que le nombre spécifié de tâches soient arrivées à cette barrière.
Sémaphore : Variable partagée par différents "acteurs" qui garantit que ceux-ci ne peuvent accéder de façon séquentielle à travers des opérations
atomiques, et constituela méthode utilisée couramment pour restreindre l'accès à des ressources partagées et synchroniser les processus dans un environnement
de programmation concurrente.
* *Verrous* : Permet d'assurer qu'un seul processus accède à une ressource à un instant donné. Un verrou peut être posé pour protéger un accès en lecture et
permettra à plusieurs processus de lire, mais aucun d'écrire. On dit alors que c'est un verrou partagé. Un verrou est dit exclusif lorsqu'il interdit toute
écriture et toute lecture en dehors du processus qui l'a posé. La granularité d'un verrou constitue l'étendue des éléments qu'il protège. Par exemple dans les bases
de données, un verrou peut être posé sur une ligne, un lot de ligne, une table etc.
* *Spinlocks* : Mécanisme simple de synchronisation basé sur l'attente active.
* *Moniteur* : Mécanisme de synchronisation qui permet à plusieurs threads de bénéficier de l'exclusion mutuelle et la possibiliter d'attendre (*block*)
l'invalidation d'une condition. Les moniteurs ont également un mécanisme qui permet aux autres threads de signaler que leur condition est validé. Il
est constitué d'un mutex et de variables conditionnelles.
* *Mutex* : Un mutex est une primitive de synchronisation qui permet l'exclusion mutuelle. Certains algorithmes utilisent un état pour commander l'exclusion

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

Le noyau peut générer des signaux pour notifier les processus que quelquechose s'est passé. Par exemple, SIGPIPE est envoyé à un processus qui
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
en lisant l'information de l'addresse socket des headers des protocoles IP et transport et en enlevant ces headers des données applications.

L'interface de programmation que les programmes utilisent pour communiquer avec la pile de protocole, utilisant les socket réseau, est appelée **socket API**.
Le développement de programmes applicatifs utilisant cette API est appelé programmation réseau. Les sockets API internet sont généralement basées sur le standard
socket de Berkeley. Dans le standard socket de Berkeley, les socket sont une forme de descripteur de fichier (*read, write, open, close*).

Dans les protocoles internet standards TCP et UDP, une adresse socket est la combinaison d'une addresse IP et d'un numéro de port. Les sockets n'ont pas besoin
d'adresse source, mais si un programme lie la socket à une adresse source, la socket peut être utilisée pour recevoir et envoyer des données à cette adresse.
Basé sur cette adresse, les sockets internet délivrent les paquets applicatifs entrants au processus applicatif approprié.

Plusieurs types de sockets internet sont disponibles :

* **Datagram** : Des sockets non connectées, qui utilisent le protocole UDP (*User Datagram Protocol*). Chaque paquet envoyé ou reçu avec une socket datagram est
adressé et routé individuellement. L'ordre ainsi que la fiabilité ne sont pas garantis, par conséquent plusieurs paquet envoyé depuis un processus à l'autre peut
arriver dans n'importe quel ordre ou bien ne pas arriver du tout. Certaines configurations spéciales peuvent être requises pour envoyer en broadcast une socket
datagram.
* **Stream** : Des socket connectés, qui utilisent les protocoles TCP (*Transmission Control Protocol*), SCTP (*Stream Control Transmission Protocol*) ou DCCP
(*Datagram Congestion Control Protocol*). Un socket stream fournit un flot de données sans erreurs, séquencé, unique et ininterrompu avec des mécanismes prédéfinis
pour créer et détruire des connections et rapporter des erreurs. Un socket stream transmet des données de manière fiable, ordonnée sans requerir l'établissement
préalable d'un canal.
* **Raw** : Permet l'envoie et la réception de paquets IP sans aucun formatage spécifique à un protocole de la couche transport. Avec les autres types de socket,
la donnée est automatiquement encapsulée selon le protocole de la couche transport choisi (TCP, UDP etc.), et l'utilisateur du socket n'a pas connaissance de
l'existence des headers du protocole. Quand on lit d'une socket raw, les headers sont généralement inclus. Lorsqu'on transmet des paquets depuis une socket raw,
l'addition automatique d'un header est optionnelle.

### IPC Socket

### Pipe anonyme

### Pipe nommé

### Passage de message

### Fichier mappé en mémoire

### Partitionnement de la mémoire

### Mémoire virtuelle

### Pagination

### Segmentation

### Types de périphériques

### Bus

### Accès à la mémoire

### Pilotes

## Administration

### Scripts

### Sauvegardes

### Machines virtuelles

### Conteneurisation

### Sécurité

## Bases de données

### Algèbre relationnelle

### Langage SQL

### Administration SGBD
