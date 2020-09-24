# Système et bases de données

## Système

### Multitâche coopératif

Le multitâche coopératif est une forme simple de multitâche où chaque tâche doit explicitement permettre aux autres tâche de s'exécuter.
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

L'ordonnanceur désique le composant du noyau du système d'exploitation choisissant l'ordre d'exécution des processus sur les processeurs d'un ordinateur.

### Synchronisation des processus

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

### Administration SGBD
