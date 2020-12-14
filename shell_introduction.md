# Introduction au Shell

---

## Qu'est-ce que le Shell ?

Le Shell est un programme qui permet d'interpréter les commandes de l'utilisateur.
C'est l'un des tout premiers moyens d'interagir avec un ordinateur.
Le shell est généralement plus puissant qu'une interface graphique utilisateur (GUI),
dans le sens où il permet d'accéder très efficacement aux fonctionnalités internes du
système d'exploitation (OS).

Souvent les outils textuels dont il dispose sont construits de manière à pouvoir être
composés. Ainsi de multiples assemblages permettent à la fois une simplicité dans
la décomposition des tâches, et une facilité de mise en oeuvre dans l'automatisation.

Les shells peuvent généralement dépendre des OS, sachant qu'il en existe une
quantité pour chacun d'entre eux. Dans le cas de Linux, le Bourne Again Shell ou bash
est très largement répandu. C'est celui qui va nous intéresser ici.

Quand on ouvre un terminal, une fenêtre s'ouvre affichant un prompt shell.
Dans le cadre du bash si aucune customisation n'a été faite il se décompose ainsi :

    [utilisateur@machine répertoire\ de\ travail]$

Généralement un shell est fait pour passer des commandes, c'est à dire, exécuter des programmes
avec ou sans arguments :

    $ date
         sam. 09 mai 2020 17:36:09 CEST

L'ajout de certains arguments permet de modifier le comportement de certains programmes.
La commande *echo* permet par example d'afficher à l'écran les arguments qui la suivent.
Un argument est une chaîne de caractère séparée du nom du programme par un espace :

    $ echo hello
        hello

Si l'on souhaite que l'argument contiennent lui-même un espace, et éviter d'ajouter un deuxième arguement
il suffit d'entourer la chaîne de guillemets :

    $ echo "Hello world!"
        Hello world!


On peut également échapper le caractère espace à l'aide d'un anti-slash :
    $ echo Hello\ world!
        Hello world!

> Exemple pour créer un répertoire Mes photos on écrit : *mkdir Mes\ photos*

---

## Les Chemins

Le shell sait quel programme (dont un certain nombre sont installés avec l'OS) utiliser
et où celui-ci ce situe dans le système de fichiers à l'aide de ce que l'on appelle une
variable d'environnement. Une variable connue et renseignée dès le lancement du shell.
Il s'agit de la variable PATH :

    $ echo $PATH
        /usr/local/sbin:/usr/local/bin:/usr/bin

Il s'agit d'une liste des chemins ordonnée dans lequel le shell va chercher les programmes.

La commande which permet de savoir dans quel répertoire se trouve le programme passé en argument,
lequel sera celui exécuté lors de l'appel par ce shell précisément :

    $ which echo
        /usr/bin/echo

Les chemins sont la description des emplacements des fichiers dans l'achitecture du système de fichers.
Sur Linux et MacOS, les répertoires sont séparés par des slashs. Le premier slash sur la gauche
symbolise le sommet du système de fichiers (celui-ci étant hiérarchique) il est appelé root ou répertoire
root, racine en français.
Sous Windows les répertoires sont généralement séparés par des anti-slash et chaque partition est la
racine de son propre système de fichier hierarchique. La partition est généralement désignée par une lettre
de l'alphabet (C:\ D:\ etc.).

Il existe 2 types de chemins :

- les chemins absolus : à partir de la racine.
- les chemins relatifs : à partir du répertoire dans lequel on se situe.

Pour savoir dans quel répertoire on se trouve actuellement il existe la commande *pwd*
pour *print working directory* (affiche le répertoire de travail) :

    $ pwd
        /home/hugo

A partir d'ici on peut changer de répertoire de travail avec la commande cd et,
en argument un chemin relatif ou absolu :

    $ cd /home

    $ pwd
        /home

Il existe un certain nombre de symboles permettant "d'expanser" des noms de répertoires :

- **~** : le répertoire de l'utilisateur courant (ie : /home/admarouj)
- **.** : le répertoire de travail
- **..** : le répertoire parent du répertoire de travail
- **-** : l'ancien répertoire de travail

Par exemple :

    $ cd ~/test/

    $ pwd
        /home/hugo/test

    $ cd ../../

    $ pwd
        /home

    $ cd -

    $ pwd
        /home/hugo/test

Cela permet de naviguer plus facilement à l'interieur du système de fichiers.

> Note : Dans le cas de script shell, lors de l'appel d'un programme on évite les chemins relatifs
> On préfère travailler avec la variable d'environnement PATH ou bien on donne le chemin absolu.

---

## Lister le Contenu d'un Répertoire et Droits

Par défaut (sans argument) un programme agit sur le répertoire de travail.
Il est alors généralement intéressant de savoir que contient ce répertoire.
La commande ls permet de lister le contenu d'un répertoire.

    $ ls
        file1  file2  file3

    $ ls ../../
        bleu rouge vert

Dans le cas de l'utilisation de certains programmes il peut être utile de connaître les arguments
que l'utilitaire accepte. La plupart des programmes implémentent des flags et des options.
Un des flags les plus utiles est --help :

    $ ls --help

permet d'afficher l'aide de la commande *ls*.

Pour lire les usages, *...* signifie 1 ou plus et *[ ]* signifie que ce qui est dans les
crochets est optionnel.
En suit généralement une brève description de la commande et à la suite les potentiels flags
disponnibles.

    $ ls -l

permet de lister au format long avec un nombre bien plus important d'information :

- le type de fichiers
- les droits : utilisateur propriétaire, groupe principal du propriétaire, autres (user, group, others)
- le nombre d'inodes (hard links)
- l'utilisateur propriétaire
- le groupe principal du propriétaire
- le mtime (modification time)
- le nom du fichier

Les droits s'organisent ainsi :

- pour les fichiers
    * r : read, autorise à lire le contenu du fichier
    * w : write, autorise à modifier le contenu du fichier
    * x : execute, autorise l'exécution du fichier
- pour les répertoires
    * r : read, autorise à lister le contenu du répertoire
    * w : write, autorise la création de fichier, la modification du nom de fichier et la suppression de fichier
    * x : execute, autorise à traverser (entrer) dans le répertoire

Enfin le - présenté par la commande :

    $ ls -l

signifie l'absence du droit.

---

## D'autres Commandes

La commande mv permet de renommer et/ou (non exclusif) déplacer un fichier:

    $ mv fichier_a_renommer ../../nouveau_fichier

La commande cp permet de copier un fichier :

    $ cp original ~/copie

La commande rm permet de supprimer un fichier :

    $ rm ~/copie

Par défaut la commande *rm* n'est pas récursive. Pour cela il est nécessaire d'ajouter le flag *-r* suivi du répertoire.
La commande *rmdir* permet également de supprimer un répertoire mais seulement si celui-ci est déjà vide.

Finalement pour créer un nouveau répertoire on utilise la commande :

    $ mkdir Mon\ Repertoire

Pour avoir encore plus d'informations sur une commande on peut se réferer aux pages de manuel de celle-ci :

    $ man ls

Cela permet généralement d'avoir une meilleure vision de ce que fait la commande, avec une navigation facilitée.

> Note : Pour quitter le programme *man*, il suffit de presser la lettre *q*.

Pour nettoyer le terminal on peut exécuter la commande :

    $ clear

ou plus facilement presser Ctrl + L.

---

## Redirection d'entrée/sortie, descripteurs de fichier et tubes

Comme il a été évoqué plus haut, l'une des caractéristiques les plus importantes du shell est qu'il permet
l'assemblage de multiples programmes via des flux. Cela permet de combiner les
fonctionnalités de différents programmes afin d'executer une tâche spécifique.

Pour intéragir avec ces flux le bash dispose de descripteurs de fichiers ou fd (pour file descriptor).
Ceux-ci sont des chiffres utilisés comme référence abstraite vers un fichier ou une ressource d'entrée/sortie (e.g pipe, IPC etc.)
Le terme de fichier est ici très large également (physique, virtuel, périphérique etc.)

Par défaut 3 ressources disposant de fd sont ouverts :

- le stdin ou (entrée standard) : fd 0, qui est saisi au clavier
- le stdout ou (sortie standard) : fd 1, qui est affiché dans le terminal
- le stderr ou (erreur standard) : fd 2, qui est affiché dans le terminal

Pour alimenter l'entrée standard à l'aide du contenu d'un fichier, on utilise le chevron gauche :

    $ ma_commande < nom_fichier

Pour vider le contenu de la sortie standard dans un fichier, on utilise le chevron droit :

    $ ma_commande > nom_fichier

> Note : Si aucun chiffre de descripteur ne préfixe les chevrons, ce sera le stdin (0) pour un gauche et le stdout (1) pour un droit.

Dans ce cas précis, uniquement la sortie d'erreur standard sera affichée dans le terminal.
Dans ce cas précis également, le contenu préexistant au fichier est écrasé, ce qui peut être pratique
si l'on souhaite vider un fichier ou créer un fichier vide on peut exécuter la commande :

    $ > nom_fichier

Pour ajouter le flux au fichier on utilise le double chevron :

    $ ma_commande >> nom_fichier

Si l'on souhaite conserver cette sortie d'erreur dans un fichier il faut préciser le descripteur de fichier :

    $ ma_commande > nom_fichier 2> erreur_log

Et si l'on souhaite simplement ajouter sans écraser :

    $ ma_commande >> nom_fichier 2>> erreur_log

Si l'on souhaite rediriger le fd i vers j :

    $ i>&j

Par exemple :

    $ ma_commande 2>&1 nom_fichier

redirige la stderr vers stdout.

Pour ouvrir un fichier et affecter un descripteur de fichier :

    $ exec 3<> nom_fichier

Et pour fermer le fichier :

    $ exec 3>&-

Cela permet d'accéder au fichier :

    $ exec 3<> nom_fichier
    $ read <&3         # Se déplacer d'une ligne vers le bas
    $ read -n 3 <&3    # Se déplacer de 3 caractères (en position 4)
    $ echo -n . >&3
    $ exec 3>&-

Le script ci-dessus permet par exemple de remplacer le 4ème caractère de la deuxième ligne du fichier par un point.
La où le shell se distingue réellement, c'est dans l'utilisation de pipe.
Cet opérateur | permet de chaîner des programmes de façon à ce que la sortie de l'un devienne l'entrée d'un autre :

    $ ls -l / | tail -n1
    $ pactl list sink-inputs | rg Volume | awk '{print $5}'

La première commande affiche le dernier item de la liste de fichiers du répertoire /.
La deuxième affiche le pourcentage de l'entrée son des destinations (sinks) audio (enceintes, casques etc.)
Cela a de multiples avantages notamment pour l'exploitation de fichiers de données.

Une commande conçue pour fonctionner avec l'opérateur pipe est xargs.
xargs lit l'entrée standard et passe chaque item en argument à la fonction suivante.
Un exemple d'application :

    $ ls *.txt | xargs wc

La première commande liste l'ensemble des fichiers .txt et wc compte le nombre de ligne de chacun des fichiers passés en argument.

Pour savoir ce qu'une commande peut faire, savoir quelle est son utilisation généralement celle-ci implémente une option --help
ou une référence (page) dans le man. man est une commande qui renvoit une section du manuel système. Pour plus de détail :

    $ man man

Appuyer sur q pour quitter.

Pour reprendre l'opérateur | on peut afficher une page man et l'ouvrir au format pdf :

    $ man ma_commande -Tpdf | zathura -

Si on le souhaite (et qu'on dispose d'un lecteur pdf)...

## Un outil versatile et puissant

Sur la plupart des systèmes Unix-like, il existe un utilisateur root.
Cet utilisateur a le droit d'accéder à l'ensemble des fichiers du système sans restriction (écriture, lecture, modification, suppression).
Généralement il est dangereux de se connecter en root sur une machine. De ce fait, on préfère donner des droits root à d'autres utilisateurs
mais uniquement sur des commandes spécifiques. Pour cela on utilise l'utilitaire sudo.

Par exemple, la luminosité d'un ordinateur portable apparaît dans un fichier système :

    /sys/class/backlight/brightness

En écrivant dans ce fichier on peut changer la luminosité de l'écran.
Néanmoins, l'écriture doit être faite par root. En effet :

    $ sudo find -L /sys/class/backlight --maxdepth 2 -name "*brightness"
    $ sudo echo 3 >/sys/class/backlight/brightness

Ne marche pas ! En effet, c'est le shell qui exécute l'écriture via >.
sudo sur la commande echo est inutile. Pour contourner le problème on utilise un autre outil pour écrire le fichier :

    $ echo 3 | sudo tee /sys/class/blacklight/brightness

Puisque c'est le programme tee qui ouvre */sys* pour l'écriture en tant que root, les permissions sont vérifiées.
Un nombre de choses intéressantes se trouve sous */sys* (contrôle des périphériques, les infos cpu, mémoire etc.)
