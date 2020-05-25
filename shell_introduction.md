# Introduction au Shell

---

## Qu'est-ce que le Shell ?

Le Shell est un programme qui permet d'interpréter les commandes de l'utilisateur.
C'est l'un des tout premier moyen d'intéragir avec un ordinateur.
Le shell est généralement plus puissant qu'une interface graphique utilisateur (GUI),
dans le sens où il permet d'accéder très efficacement aux fonctionnalités interne du
système d'exploitation (OS).

Souvent les outils textuels dont il dispose sont construit de manière à pouvoir être
composés. Ainsi de multiples assemblage permettent à la fois une simplicité dans
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
et où celui-ci ce situe dans le système de fichier à l'aide de ce que l'on appelle une
variable d'environnement. Une variable connue et renseignée dès le lancement du shell.
Il s'agit de la variable PATH :

    $ echo $PATH
        /usr/local/sbin:/usr/local/bin:/usr/bin

Il s'agit d'une liste des chemins ordonnée dans lequel le shell va chercher les programmes.

La commande which permet de savoir dans quel répertoire se trouve le programme passé en argument,
lequel sera celui exécuté lors de l'appel par ce shell précisément :

    $ which echo
        /usr/bin/echo

Les chemins sont la description des emplacements des fichiers dans l'achitecture du système de ficher.
Sur Linux et MacOS, les répertoires sont séparés par des slash. Le premier slash sur la gauche
symbolise le sommet du système de fichier (celui-ci étant hiérarchique) il est appelé root ou répertoire
root, racine en français.
Sous Windows les répertoires sont généralement séparés par des anti-slash et chaque partition est la
racine de son propre système de fichier hierarchique. La partition est généralement désignée par une lettre
de l'alphabet (C:\ D:\ etc.).

Il existe 2 type de chemins :

- les chemins absolus : à partir de la racine.
- les chemins relatifs : à partir du répertoire dans lequel on se situe.

Pour savoir dans quel répertoire on se trouve actuellement il existe la commande *pwd*
pour *print working directory* (affiche le répertoire de travail) :

    $ pwd
        /home/admarouj

A partir d'ici on peut changer de répertoire de travail avec la commande cd et,
en argument un chemin relatif ou absolu :

    $ cd /home

    $ pwd
        /home

Il existe un certain nombre de symbole permettant "d'expanser" des noms de répertoires :

- **~** : le répertoire de l'utilisateur courant (ie : /home/admarouj)
- **.** : le répertoire de travail
- **..** : le répertoire parent du répertoire de travail
- **-** : l'ancien répertoire de travail

Par exemple :
    $ cd ~/test/

    $ pwd
        /home/admarouj/test

    $ cd ../../

    $ pwd
        /home

    $ cd -

    $ pwd
        /home/admarouj/test

Cela permet de naviguer plus facilement à l'interieur du système de fichier.

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
        admarouj  khq  admsrenn

Dans le cas de l'utilisation de certains programmes il peut être utile de connaître les arguments
que l'utilitaire accepte. La plupart des programmes implémentent des flags et des options.
Un des flags les plus utiles est --help :

    $ ls --help

permet d'afficher l'aide de la commande *ls*.

Pour lire les usages, *...* signifie 1 ou plus et *[]* signifie que ce qui est dans les
crochets est optionnel.
En suit généralement une brève description de la commande et à la suite les potentiels flags
disponnibles.

    $ ls -l

permet de lister au format long avec un nombre bien plus important d'information :

- le type de fichier
- les droits : utilisateur propriétaire, groupe principal du propriétaire, autres (user, group, others)
- le nombre d'inodes (hard links)
- l'utilisateur propriétaire
- le groupe principal du propriétaire
- le mtime (modification time)
- le nom du fichier

Les droits s'organisent ainsi :

- pour les fichier
    * r : read, autorise à lire le contenu du fichier
    * w : write, autorise à modifier le contenu du fichier
    * x : execute, autorise l'exécution du fichier
- pour les répertoire
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

Pour avoir encore plus d'information sur une commande on peut se réferer aux pages de manuel de celle-ci :

    $ man ls

Cela permet généralement d'avoir une meilleure vision de ce que fait la commande, avec une navigation facilitée.

> Note : Pour quitter le programme *man*, il suffit de presser la lettre *q*.

Pour nettoyer le terminal on peut exécuter la commande :

    $ clear

ou plus facilement presser Ctrl + L.

---

## Les Descripteurs de fichier et Tubes

Comme il a été évoqué plus haut, l'une des caractéristiques les plus importante du shell est qu'il permet
l'assemblage de multiples programmes via des flux. Cela permet de combiner les
fonctionnalités de différents programmes afin d'executer une tâche spécifique.

Pour intéragir avec ces flux le bash dispose de descripteurs de fichiers ou fd (pour file descriptor).
Ceux-ci sont des chiffres utilisés comme référence abstraite vers un fichier ou une ressource d'entrée/sortie.

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

Pour ouvrir un fichier et pour affecter un descripteur de fichier :

    $ exec 3<> nom_fichier

Enfin pour fermer le fichier :

    $ exec 3>&-


