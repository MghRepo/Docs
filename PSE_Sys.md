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

Barrière de synchronisation : Permet de garantir qu'un certain nombre de tâches aient passé un point spécifique. Ainsi, chaque tâche qui arrivera
sur cette barrière devra attendre jusqu'à ce que le nombre spécifié de tâches soient arrivées à cette barrière.
Sémaphore : Variable partagée par différents "acteurs" qui garantit que ceux-ci ne peuvent accéder de façon séquentielle à travers des opérations
atomiques, et constituela méthode utilisée couramment pour restreindre l'accès à des ressources partagées et synchroniser les processus dans un environnement
de programmation concurrente.
Verrous :
Spinlocs :
Moniteur :
Mutex :

La connaissance des dépendances entre les données est fondamentale dans la mise en oeuvre d'algorithmes parallèles, d'autant qu'un calcul peut
dépendre de plusieurs calculs préalables. Les *conditions de Bernstein* permettent de déterminer les conditions sur les données lorsque deux parties
de programme peuvent être exécutées en parallèle.

### Communications inter-processus

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
