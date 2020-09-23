# Algorithmique et méthodes de programmation

## Structures de contrôles :

En programmation informatique, une structure de contrôle est une instruction particulière à un langage de programmation impératif pouvant dévier
le flot de contrôle du programme la contenant lorsqu'elle est exécutée. Si, au plus bas niveau, l'éventail se limite généralement aux branchements et
aux appels de sous-programme, les langages structurés offrent des constructions plus élaborées comme les alternatives (if, if-else, switch...), les
boucles (while, do-while, for...) ou encore les appels de fonction. Outre les structures usuelles, la large palette des structures de contrôle s'étend
des constructions de gestion d'exceptions (try-catch...) fréquemment trouvés dans les langages de haut niveau aux particularismes de certains
langages comme les instructions différées (defer) de Go.

### Structures de contrôle séquentielles :

Un programme informatique impératif est une suite d'instructions. Un registre interne de processeur, le compteur ordinal(PC), est chargé de
mémoriser l'adresse de la prochaine instruction à exécuter.

La plupart des instructions d'un programme sont exécutées séquentiellement : après le traitement de l'instruction courante le compteur ordinal est
incrémenté, et la prochaine instruction est chargée.

La séquence est donc la structure de contrôle implicite. Elle donne l'ordre d'exécution des instructions, souvent séparées par un point-virgule ou par
des retours chariots.

Un programme s'arrête généralement après l'exécution de la dernière instruction. La plupart des langages de programmation proposent également
une ou plusieurs instructions pour stopper l'exécution du programme à une position arbitraire.

Selon l'environnement d'exécution sous-jacent (système d'exploitation ou microprocesseur), cet arrêt peut être définitif ou correspondre à une
suspension de l'exécution du programme en attendant un évenement externe : c'est par exemple le fonctionnement habituel de la plupart des
instructions d'entrée sorties qui bloquent le flot d'exécution (mécanisme d'interruption avec stockage en mémoire tampon) jusqu'à ce que le
périphérique concerné ait terminé de traiter les données.

###Structures de contrôle itératives

Ces instructions permettent de réaliser une machine à états finis, cela signifie que leur seul effet de bord est de modifier un registre qui correspond à
l'état courant du programme.

Dans un processeur, cet état correspod à la valeur du compter ordinal.

Commandes à étiquettes :
	- Sauts inconditionnels
	- Sauts conditionnels
	- Sous programmes, commandes de sorties de boucles

Commandes de blocs :
	- Blocs d'instructions
	- Alternatives (if, then, else, switch)
	- Boucles (do, whilei, each)

### Extensions de la notion de boucles

Un compteur permet de réaliser une boucle associée à une variable entière ou un pointeur qui sera incrémentée à chaque itération. Il est souvent
utilisé pour exploiter les données d'une collection indexée (boucle for).

Un itérateur (ou curseur ou énumérateur) est un objet qui permet de réaliser une boucle parcourant tous les éléments contenus dans une structure
de données.

### Sous-programmes

Un sous-programme permet la réutilisation d'une partie du code et ainsi le développement des algorithmes récursifs.

Beacoup de langages modernes ne supportent pas directement la notion de sous-programme au profit de constructions de haut niveau qui
peuvent être appelées, d'un langage à l'autre *procédure, fonction, méthode, ou *routine*. Ces constructions ajoutent la notion de passage de paramètres
et surtout le cloisonnement des espaces de nom pour éviter que le sous-programme ait un effet de bord sur la routine appelante.

Il existe diverses extensions à la notion de procédure comme les coroutines (routine avec suspension), signaux, et slots (signaux implémentés pour les objets),
fonctions de rappel (callback, traitement post-fonction), méthodes virtuelles (programmation par contrat, methode implémentée dans les classes héritées)
Elles permettent de modifier dynamiquement, c'est à dire à l'exécution, la structure du flot d'exécution du programme.

### Exceptions

Tout programme en exécution peut être sujet à des erreurs pour lesquelles des stratégies de détection et de réparation sont possibles. Ces erreurs
ne sont pas des bugs mais des conditions exceptionnelles dans le déroulement normal d'une partie d'un programme.

### Programmation multitâche

Dans un système multitâche, plusieurs flots d'exécutions, appelés processus légers, s'exécutent simultanément.

Il est alors nécessaire d'assurer la synchroisation de ces flots d'exécution. Dans la plupart des langages, cela est réalisé via des bibliothèques externes ;
certains d'entre eux intègrent néanmoins des structures de contrôle permentant d'agir sur des tâches concourantes.

### Programmation événementielle

La programmation évenementielle est une autre façon de contrôler le flot d'exécutions d'un programme. Il s'agit de créer des gestionnaires qui
viendront s'abonner à une boucle mère, chargée d'aiguiller les évènements qui affectent le logiciel.

## Algorithme de tri

Un algorithme de tri est, en informatique ou en mathématiques, un algorithme qui permet d'organiser une collection d'objets selon une relation
d'ordre déterminée. Les objets à trier sont des éléments d'un ensemble muni d'un ordre total. Il est par exemple fréquent de trier des entier selon la
relation d'ordre usuelle "est inférier ou égal à". Les algorithmes de tris sont utilisés dans de très nombreuses situations. Ils sont en particulier utiles
à de nombreux algorithmes plus complexes dont certains algorithmes de recherche, comme la recherche dichotomique. Ils peuvent également servir
pour mettre des données sous forme canonique ou les rendre plus lisibles pour l'utilisateur.

La collection à trier est souvent donnée sous forme de tableau, afin de permettre l'accès direct aux différents éléments de la collection, ou sous forme
de liste, ce qui peut se révéler être plus adapté à certains algorithmes et l'usage de la programmation fonctionnelle.

Bon nombre d'algorithmes de tri procèdent par comparaison successives, et peuvent donc être définis indépendamment de l'ensemble auquel
appartiennent les éléments de la relation d'ordre associée. Un même algorithme peut par exemple être utiliser pour trier les réels selon la relation
d'ordre usuelle "est inférieur ou égal à" et des chaînes de caractères selon l'ordre lexicographique. Ces algorithmes se prêtent naturellement à une
implémentation polymorphe (différents types).

Les algorithmes de tri sont souvent étudiés dans les cours d'algorithmique pour introduire des notions comme la complexité algorithmique ou la
terminaison.

###Critère de classification

La classification des algorithmes de tri est très importante, car elle permet de choisir l'algorithme le plus adapté au problème traité, tout en tenant
compte des contraites imposées par celui-ci. Les principales caractéristiques qui permettent de différencier les algorithmes de tri, outre leur principe
de fonctionnement, sont la complexité temporelle, la complexité spatiale et le caractère stable.

La complexité temporelle (en moyenne ou dans le pire des cas) mesure le nombre d'opération élémentaires effectuées pour trier une collection
d'éléments. C'est un critère majeur pour comparer les algorithmes de tri, puisque c'est une estimation directe du temps d'exécution de
l'algorithme. Dans le cas des algorithmes de tri par comparaison, la complexité en temps est le plus souvent assimilable au nombre de
comparaisons effectuées, la comparaison et l'échange éventuel de deux valeurs s'effectuant en temps constant.

La complexité spatiale (en moyenne ou dans le pire des cas) représente, quant à elle, la quantité de mémoire dont va avoir besoin l'algorithme
pour s'exécuter. Celle-ci peut dépendre, comme le temps d'exécution, de la taille de l'entrée. Il est fréquent que les complexités spatiales en
moyenne et dans le pire des cas soient identiques. C'est souvent le cas lorsqu'une complexité est donnée sans indication
supplémentaire.

Un tri est dit *en place* s'il n'utilise qu'un nombre très limité de variables et qu'il modifie directement la structure qu'il est en train de trier. Ceci nécessite
l'utilisation d'une structure de donnée adaptée (un tableau par exemple). Ce caractère peut être très important si on ne dispose pas de beaucoup de
mémoire.

Toutefois, on ne déplace pas, en général, les données elles-mêmes, mais on modifie seulement des références (ou pointeurs) vers ces dernières.

Un tri est dit *stable* s'il préserve l'ordonnancement initial des éléments que l'ordre considère comme égaux (tri à bulles, tri par insertion et le tri par fusion).

Un tri *interne* s'effectue entièrement en mémoire centrale tandis qu'un tris *externe* utilise des fichiers sur une mémoire de masse pour trier des volumes
trop importants pour pouvoir tenir en mémoire centrale.

Certains algorithmes permettent d'exploiter les capacités multitâches de la machine. Notons également, que certains algorithmes, notamment ceux
qui fonctionnent par insertion, peuvent être lancés sans connaître l'intégralité des données à trier; on peut alors trier et produire les données à trier
en parallèle.

###Exemples d'algorithmes de tri

Algorithmes rapides T(n)=O(nlog n) :
	- Tri fusion (merge sort) : Pour une entrée donnée, l'algorithme la divise en deux parties de tailles
	similaires, trie chacune d'elles en utilisant le même algorithme, puis fusionne les deux parties triées. Il se prête aussi bien à des
	implémenations sur listes que sur tableaux.
	- Tri rapide (quicksort) : Une valeur est choisie comme pivot et les éléments plus petits que le pivot sont
	dissociés, par échanges successifs, des éléments plus grands que le pivot ; chacun de ces deux sous-ensembles est ensuite trié de la même
	manière. On peut rendre la complexité quasiment indépendante des données en utilisant un pivot aléatoire ou en appliquant au tableau une
	permutation aléatoire avant de le trier.
	- Tris par tas (heap sort) : Il s'agit d'une amélioration du tri par sélection. L'idée est la même (insérer les élément un à un dans une structure déjà triée, mais
	l'algorithme utilise une structure de tas, souvent implémentée au moyen d'un tableau.
	- Introsort : Il s'agit d'un hybride du tri rapide et du tri par tas.
	- Tri arborescent : L'idée est d'insérer les éléments un à un dans l'arbre binaire de recherche, puis de lire l'arbre selon un parcours en profondeur.
	Un arbre binaire de recherche(ABR) est un arbre binaire dans lequel chaque noeud possède une clé, telle que chaque noeud du sous-arbre *gauche* ait une
	clé inférieure ou égale à celle du noeud considéré, et que chaque noeud du sous-arbre *droit* possède une clé supérieure ou égale à celle-ci.
	- Smoothsort

Algorithmes moyennement rapides :
	- Tri de Shell (shell sort) : Ce tri repose sur le tri par insertion des sous-suites de l'entrée obtenues en prenant les éléments espacés d'un pas constant, pour une suite
	de pas prédéfinie. La complexité varie selon le choix de cette suite.
	- Tri à peigne (comb sort) : Il s'agit d'une variante plus efficace du tri à bulles, ne comparant pas uniquement des éléments consécutifs. On peut dire qu'il est au tri à
	bulles ce que le tri de Shell est au tri par insertion.
	- Tri par insertion : Ce tri souvent utilisé naturellement pour trier des cartes à jouer : les valeurs sont insérées les unes après les autres dans une liste triée
	(initialement vide). C'est souvent le plus rapide et le plus utilisé pour trier les entrées de petite taille. Il est également efficace pour des
	entrées déjà presque triées.
	- Tri à bulles : L'algorithme consiste à parcourir l'entrée du début à la fin et pour chaque couple d'éléments consécutifs, à les intervertir s'ils sont mal
	ordonnés. Cette opération est répétée jusqu'à ce que la structure soit triée (aucune intervention lors du dernier passage). Cet algorithme est
	peu efficace et rarement utilisé en pratique ; son intêret est principalement pédagogique.
	- Tri cocktail : Il s'agit d'une variante du tri à bulles dans laquelle l'entrée est alternativement parcourue dans les deux sens. S'il permet de traiter de manière
	plus efficace quelques cas problématiques pour le tri à bulles, il reste essentiellement similaire à ce dernier et l'intérêt est encore une fois
	principalement pédagogique.
	- Tri pair-impair : Il s'agit d'une variante du tri à bulles, qui procède en comparant successivement tous les éléments d'index pairs avec les éléments d'index
	impairs qui les suivent, puis inversement. On va ainsi commencer en comparant le premier élément au second, le troisième au quatrième, etc.,
	puis l'on comparera le second élément au troisième, le quatrième au cinquième. L'opération est répétée jusqu'à ce que la structure soit
	triée.

Algorithmes lents :
	- Tri par selection :

## Structures de données :

Une structure de données est une manière d'organiser les données pour les traiter plus facilement. Une structure de données est
une mise en oeuvre concrète d'un type abstrait.

### Pile

Une pile est une structure de données fondée sur le principe "dernier arrivé, premier sorti" (LIFO),
ce qui veut dire qu'en général, le dernier élément ajouté à la pile, sera le premier à en sortir.

La plupart des microprocesseurs gèrent nativement une pile. Elle correspond alors à une zone de la mémoire, et le processeur retient l'adresse du
dernier élément.

Voici les primitives communément utilisées pour manipuler les piles. Il n'existe pas de normalisation pour les
primitives de manipulation de pile. Leurs noms sont donc indiqués de manière informelle. Seules les trois
premières sont réellement indispensables, les autres pouvant s'en déduire :
	- "Empiler" (push) : ajoute un élément sur la pile.
	- "Depiler" (pull) : enlève un élément de la pile et le renvoie.
	- "La pile est-elle vide ?" : renvoie vrai si la pile est vide, faux sinon.
	- "Nombre d'éléments de la pile" : renvoie le nombre d'élément de la pile.
	- "Quel est l'élément de tête ?" (peek ou top) : renvoie l'élément de tête sans le dépiler.
	- "Vider la liste" (clear) : dépiler tous les éléments.

### File

Une file est une structure de donnée basée sur le principe de "premier entré, premier sorti" (FIFO),
les premiers éléments ajoutés à la file seront les premier à en être retirés.

Les queues servent à organiser le traitement séquentiel des blocs de données d'origines diverses.

La théorie des files d'attente, élaborée pour le dimensionnement des réseau téléphoniques, relie le nombre d'usagers, le nombre de canaux
disponibles, le temps d'occupation moyen du canal, et le temps d'attente à prévoir (Loi de Poisson).

Cette structure est utilisée par exemple :
	- en général, pour mémoriser temporairement des transactions qui doivent attendre pour être traitées ;
	- les serveurs d'impression, qui traitent ainsi les requêtes dans l'ordre dans lequel elles arrivent, et les insèrent dans une file d'attente (spool)
	- certains moteurs multitâches, dans les systèmes d'exploitation, qui doivent accorder du temps-machine à chaque tâche, sans en privilégier
	aucune ;
	- un algorithme de parcours en largeur utilise une file pour mémoriser les noeuds visités ;
	- pour créer toutes sortes de mémoires tampons (buffers) ;
	- En gestion des stocks les algorithmes doivent respecter la gestion physique des stocks pour assurer la cohérence physique/valorisation.

Voici les primitives communément utilisées pour manipuler les files. Il n'existe pas de normalisation pour les primitives de manipulation de file. Leurs
nom sont donc indiqués de manière informelle.

	- "Enfiler" (enqueue) : ajouter un élément dans la file.
	- "Defiler" (dequeue) : renvoie le prochain élément de la file, et le retire de la file.
	- "La file est-elle vide ?" : renvoie "vrai" si la file est vide, "faux" sinon.
	- "Nombre d'élément dans la file" : renvoie le nombre d'élément dans la file.

### Liste

Une liste est une structure de données permettant de regrouper des données de manière à pouvoir y accéder librement
(contrairement aux files et aux piles).

Voici les primitives communément utilisées pour manipuler des listes ; il n'existe pas de normalisation pour les primitives de manipulation de listes,
leurs noms respectifs sont donc indiqués de manière informelle.

Primitives de base :
	- "Insérer" (Add) : ajoute un élément dans la liste ;
	- "Retirer" (Remove) : retire un élément de la liste ;
	- "La liste est-elle vide ?" (IsNull) : renvoie "vrai" si la liste est vide, "faux" sinon ;
	- "Nombre d'éléments dans la liste (Length)" : renvoie le nombre d'éléments dans la liste.

Primitives auxiliaires fréquemment rencontrées :
	- "Premier" (First) : retourne le premier élément dans la liste ;
	- "Dernier" (Last) : retourne le dernier élément dans la liste ;
	- "Prochain" (Next) : retourne le prochain élémnet dans la liste ;
	- "Précédent" (Previous) : retourne l'élément qui précède dans la liste ;
	- "Cherche" (find) : cherche si un élément précis est contenu dans la liste et retourne sa position.

Une liste est un conteneur d'éléments, où chaque élément contient la donnée, ainsi que d'autres informations permettant la récupération des
données au sein de la liste. La nature (les types) de ces informations caractérise un type différent de liste.

On peut distinguer, de manière générale, deux types de liste :
	- les tableaux ;
	- les listes chaînées.

Dans un tableau, l'accès à un élément se fait à l'aide d'un index qui représente l'emplacement de l'élément dans la
structure.

Les données présentes dans un tableau sont contiguës en mémoire. Cela induit une taille de
tableau fixe. Cependant certains langages de haut niveau fournissent des tableaux qui modifient
leur taille en fonction de leur utilisation : on parle alors de
tableau à taille dynamique. Mais leur implémentation utilise le principe des listes chaînées.
Les tableaux peuvent également avoir plusieurs dimensions, représentées par une séquence d'indices. Dans ce cas, si n est la dimension du tableau
(où n est un entier naturel non nul), les éléments du tableau de dimension 1 (le 1er indice de la séquence) pointent chacun vers un autre
(sous-)tableau de dimension n-1.

Contrairement à un tableau, la taille d'une liste chaînée n'a pas de limite autre que celle de la mémoire disponible. Cette limitation est
franchie par le fait que chaque élément peut pointer, suivant le type de liste chaînée, vers un ou plusieurs éléments de la liste en utilisant
une définition récursive. Ainsi, pour augmenter la taille d'une liste chaînée, il suffit de créer un nouvel élément et de faire pointer certains
éléments, déjà présents au sein de la liste, vers le nouvel élément.

Il existe deux grand types de liste chainée :
	- les listes simplement chaînées : chaque élément dispose d'un pointeur sur l'élément suivant (ou successeur) de la liste. Le parcours se fait dans
	un seul sens ;
	- les listes doublement chaînées : chaque élément dispose de deux pointeurs, respectivement sur l'élément suivant (ou successeur) et sur l'élément
	précédent (ou prédécesseur). Le parcours peut alors se faire dans deux sens, mutuellement opposés : de successeur en successeur, ou de
	prédécesseur en prédécesseur.

A cela on peut ajouter une propriété : le cycle. Cette fois-ci, la liste chaînée forme une boucle. Dès qu'on atteint la "fin" de la liste et qu'on désire
continuer, on se retrouve sur le "premier" élément de la liste. Dans ce cas, la notion de début ou de fin de chaîne n'a plus de raison d'être.

### Arbre enraciné

En théorie de graphes, un arbre enraciné ou une arborescence est un graphe acyclique orienté possédant
une unique racine, et tel que tous les noeuds sauf la racine ont un unique parent.

Dans un arbre, on distingue deux catégories d'éléments :
	- les *feuilles* (ou noeuds externes), éléments ne possédant pas de fil dans l'arbre ;
	- les *noeuds* interne, éléments possédant des fils (sous-branches).

La *racine* de l'arbre est l'unique noeud ne possédant pas de parent. Les noeuds (les pères avec leurs fils) sont reliés entre eux par une *arête*. Selon le
contexte, un noeud peut désigner un noeud interne ou externe (feuille) de l'arbre.

La *profondeur* d'un noeud est la distance, i.e. le nombre d'arêtes, de la racine au noeud. La *hauteur* d'une arbre est la plus grande profondeur d'une
feuille de l'arbre. La *taille* d'un arbre est son nombre de noeuds (en comptant les feuilles ou non), la longueur de cheminement est la somme des
profondeurs de chacune des feuilles.

Les arbres peuvent être étiquetés. Dans ce cas, chaque noeud possède une *étiquette*, qui est en quelque sorte le "contenu" du noeud. L'étiquette
peut être très simple : un nombre entier, par exemple. Elle peut également être aussi complexe que l'on veut : un objet, une instance d'une structure
de donnée, un pointeur, etc. Il est presque toujours obligatoire de pouvoir comparer les étiquettes selon une relation d'ordre total, afin d'implanter
les algorithmes sur les arbres.

Les fichier et dossier dans un système de fichiers sont généralement organisés sous forme arborescente.

Les arbre sont en fait rarement utilisés en tant que tels, mais de nombreux types d'arbres avec une structure plus restrictive existent et sont
couramment utilisés en algorithmique, notamment pour gérer des bases de données, ou pour l'indexation de fichiers. Ils permettent alors des
recherches rapides et efficaces. Par exemple :
	- Les arbres binaires dont chaque noeud a au plus deux fils : ils sont en fait utilisés sous forme d'arbres binaires de recherche, de tas, d'AVL, ou
	encore d'arbres rouge-noir. Les deux derniers exemples sont des cas particuliers d'arbres équilibrés, c'est à dire dont les sous-branches
	ont presque la même hauteur.
	- Les arbres n-aires qui sont une généralisation des arbres binaires : chaque noeud a au plus *n* fils. Les arbres 2-3-4 et les arbres B en sont des
	exemples d'utilisation et sont eux aussi des arbres équilibrés.

Pour construire un arbre à partir de cases ne contenant que des informations, on peut procéder de l'une des trois façons suivantes :
	1. Créer une structure de données composée de :
		1. l'étiquette (la valeur contenue dans le noeud),
		2. un lien vers *chaque* noeud fils,
		3. un arbre particulier, l'arbre vide, qui permet de caractériser les feuilles. Une feuille a pour fils des arbres vides uniquement.
	2. Créer une structure de données composée de :
		1. l'étiquette (la valeur contenue dans le noeud),
		2. un lien vers le "premier" noeud fils (noeud fils gauche le cas échéant),
		3. un autre lien vers le noeud frère (le "premier" noeud frère sur la droite le cas échéant).
	3. Créer une structure de données composée de :
		1. l'étiquette (la valeur contenue dans le noeud),
		2. un lien vers le noeud père.

On note qu'il existe d'autres types de représentation propres à des cas particuliers d'arbres. Par exemple, le tas est représenté par un tableau
d'étiquettes.

Les parcours d'arbre sont des processus de visites des sommets d'un arbre, par exemple pour trouver une
valeur.

Le *parcours en largeur* correspond à un parcours par niveau de noeuds de l'arbre. Un niveau est un ensemble
de noeuds internes ou de feuilles situés à la même profondeur - on parle aussi de noeud ou de feuille de
même hauteur dans l'arbre considéré. L'ordre de parcours d'un niveau donné est habituellement conféré, de manière récursive, par l'ordre de
parcours des noeuds parents - noeuds de niveau immédiatement supérieur.

Le *parcours en profondeur* est un parcours récursif sur un arbre. Dans le cas général, deux ordres sont possibles :
	- Parcours en profondeur préfixe : dans ce mode de parcours, le noeud courant est traité avant ses descendants.
	- Parcours en profondeur suffixe : dans ce mode de parcours, le noeud courant est traité après ses descendants.

Pour les arbres binaires, on peut également faire un *parcours infixe*, c'est à dire traiter le noeud courant entre les noeuds gauche et droit.

### Arbre binaire de recherche

Un arbre binaire de recherche ou ABR (en anglais, binary search tree ou BST) est
une structure de données représentant un ensemble ou un tableau associatif dont les clefs appartiennent
à un ensemble totalement ordonné. Un arbre binaire de recherche permet des opérations rapides pour
rechercher une clé, insérer ou supprimer une clé.

Un arbre binaire de recherche est un arbre binaire dans lequel chaque noeud possède une clé, telle que chaque noeud du sous-arbre *gauche* ait une
clé inférieure ou égale à celle du noeud considéré, et que chaque noeud du sous-arbre *droit* possède une clé supérieure ou égale à celle-ci - selon
la mise en oeuvre de l'ABR, on pourra interdire ou non des clés de valeur égale. Les noeuds que l'on ajoute deviennent des feuilles de l'arbre.

La recherche dans un arbre binaire d'un noeud ayant une clé particulière est un procédé récursif. On
commence par examiner la racine. Si la clé est la clé recherchée, l'algorithme se termine et renvoie la racine.
Si elle est strictement inférieure, alors elle est dans le sous-arbre gauche, sur lequel on effectue alors
récursivement la recherche. De même si la clé recherchée est strictement supérieure à la clé de la racine, la
recherche continue dans le sous-arbre droit. Si on atteint une feuille dont la clé n'est pas celle recherchée, on
sait alors que la clé recherchée n'appartient à aucun noeud, elle ne figure donc pas dans l'arbre de
recherche. On peut comparer l'exploration d'un arbre binaire de recherche avec la recherche par dichotomie
qui procède à peu près de la même manière sauf qu'elle accède directement à chaque élément d'un tableau
au lieu de suivre les liens. La différence entre les deux algorithmes est que, dans la recherche dichotomique,
on suppose avoir un critère de découpage de l'espace en deux parties que l'on n'a pas dans la recherche dans un arbre.

Cette opération requiert un temps en O(log n) dans le cas moyen, mais O(n) dans le cas critique où l'arbre est complètement déséquilibré et
ressemble à une liste chaînée. Ce problème est écarté si l'arbre est équilibré par rotation au fur et à mesure des insertions pouvant créer des listes
trop longues.

L'insertion d'un noeud commence par une recherche : on cherche la clé du noeud à insérer ; lorsqu'on arrive à une feuille, on ajoute le noeud comme
fils de la feuille en comparant sa clé à celle de la feuille : si elle est inférieure, le nouveau noeud sera à gauche ; sinon il sera à droite.

Sa complexité est la même que pour la recherche.

Il est aussi possible d'écrire une procédure d'ajout d'élément à la racine d'un arbre binaire. Cette opération requiert la même compléxité mais est
meilleure en terme d'accès aux éléments.

Pour la suppression, on commence par rechercher la clé du noeud à supprimer dans l'arbre. Plusieurs cas sont à considérer, une fois que le noeud à
supprimer a été trouvé à partir de sa clé :
	- *Suppression d'une feuille* : Il suffit de l'enlever de l'arbre puisqu'elle n'a pas de fils.
	- *Suppression de noeud avec un enfant* : Il faut l'enlever de l'arbre en le remplaçant par son fils.
	- *Suppresson d'un noeud avec deux enfants* : Supposons que le noeud à supprimer soit appelé N. On échange le noeud N avec son sucesseur
	le plus proche (le noeud le plus à gauche du sous-arbre droit) ou son plus proche prédécesseur (le noeud le plus à droite du sous-arbre gauche).
	Cela permet de garder à la fin de l'opération une structure d'arbre binaire de recherche. Puis on applique à nouveau la procédure de suppression à N,
	qui est maintenant une feuille ou un noeud avec un seul fils.

Ce choix d'implémentation peut contribuer à déséquilibrer l'arbre. En effet, puisque ce sont toujours des feuilles du sous-arbre gauche qui sont
supprimées, une utilisation fréquente de cette fonction amènera à un arbre plus lourd à droite qu'à gauche. On peut remédier à cela en alternant
successivement la suppression du minimum du fils droit avec celle maximum du fils gauche, plutôt que toujours choisir ce dernier. Il est par
exemple possible d'utiliser un facteur aléatoire : le programme aura une chance sur deux de choisir le fils droit et une chance sur deux de choisir le
fils gauche.

Dans tous les cas cette opération requiert de parcourir l'arbre de la racine jusqu'à une feuille : le temps d'exécution est donc proportionnel à la
profondeur de l'arbre qui vaut n dans le pire des cas, d'où une complexité maximale O(n).

On peut récupérer les clés d'un arbre binaire de recherche dans l'ordre croissant en réalisant un parcours infixe. Il faut concaténer dans cet ordre la
liste triée obtenue récursivement par parcours du fils gauche à la racine puis à celle obtenue récursivement par parcours du fils droit. Il est possible de
le faire dans l'ordre inverse en commençant par le sous-arbre droit. Le parcours de l'arbre se fait en temps linéaire, puisqu'il doit passer par chaque
noeud une seule fois.

On peut dès lors créer un algorithme de tri simple mais peu efficace, en insérant toutes les clés que l'on veut trier dans un nouvel arbre binaire de
recherche puis en parcourant de manière ordonnée cet arbre comme ci-dessus.

Les arbres binaires de recherche peuvent servir d'implémentation au type abstrait de file de priorité. En effet, les opérations d'insertion d'une clé et
de test au vide se font avec des complexités avantageuses (respectivement en O(log n) et en O(1)).
Pour l'opération de suppression de la plus grande clé, il suffit de parcourir l'arbre depuis sa racine en choisissant le fils droit de chaque noeud, et de
supprimer la feuille terminale. Cela demande un nombre d'opérations égal à la hauteur de l'arbre, donc une complexité logarithmique. L'avantage notoire de
cette représentation d'une file de priorité est qu'avec un processus similaire, on dispose d'une opération de suppression de la plus petite clé en temps
logarithmique également.

L'insertion et la suppression s'exécutent en O(h) où h est la hauteur de l'arbre. Cela s'avère particulièrement coûteux quand l'arbre est très
déséquilibré (un arbre peigne par exemple, dont la hauteur est linéaire en le nombre de clés), et on gagne donc en efficacité à équilibrer les arbres
au cours de leur utilisation. Il existe des techniques pour obtenir des arbres équilibrés, c'est à dire une hauteur logarithmique en
nombre d'éléments :
	- les arbres AVL
	- les arbres rouge-noir
	- les B-arbres

### Tas

Un tas est une structure de données de type arbre tel que pour tous noeuds A et B de l'arbre tels que B soit un fils de A : clé(A)>=clé(B) (ou inversément).
Les primitives du tas sont : enfiler et defiler.

Pour enfiler un élément, on le place comme feuille, puis on fait "remonter" l'élément pour maintenir la priorité du tas. L'opération peut être réalisée en O(log n).

Quand on défile un élément d'un tas, c'est toujours celui de priorité maximale. Il correspond donc à la racine du tas. L''opération peut conserver la
structure de tas, avec une complexité de O(log n) ; en effet, il ne reste alors qu'à réordonner l'arbre privé de sa racine pour en faire un nouveau tas.
 
## Programmation orientée objet

La programmation orientée objet (POO), ou programmation par objet, est un paradigme de programmation informatique basé sur le
concept *d'objets*, qui peuvent contenir du code et des données : les données sous la forme de champs (attributs
ou propriétés), et le code sous la forme de procédures (méthodes).

Une capacité des objets est que ses procédures peuvent accéder et souvent modifier ses propres champs de données (*this* ou *self*).
En POO, les programmes informatiques sont créés de façon à pouvoir intéragir les uns avec les autres. Les langages de POO peuvent
être très divers, mais les plus populaires sont basés sur la notion de classe, cela signifie que les objets sont des instances de
classes, qui déterminent également leurs types.

Une *classe* regroupe des membres, méthodes et propriétés (attributs) communs à un ensemble d'objets.
La classe déclare, d'une part, des attributs représentant l'état des objets et, d'autres part, des méthodes représentant leur comportement.

Il est possible de restreindre l'ensemble d'objets représenté par une classe A grâce à un mécanisme *d'héritage*. Dans ce cas, on crée une nouvelle
classe B liée à la classe A et qui ajoute de nouvelles propriétés.

Dans la programmation par objets, chaque objet est typé. Le type définit la syntaxe et la sémantique des messages auxquels peut répondre un objet.
Il correspond donc, à peu de chose près, à l'interface de l'objet.

Un objet peut appartenir à plus d'un type, c'est le *polymorphisme*, cela permet d'utiliser des objets de types différents là où est attendu un objet d'un
certain type.

## Compilation

Un compilateur est un programme qui transforme un code *source* en code *objet*. Généralement, le code source est écrit dans
un langage de programmation (le langage source), il est de haut niveau d'abstraction, et facilement compréhensible par l'humain. Le code objet est
généralement écrit en langage de plus bas niveau (appelé langage cible), par exemple un langage d'assemblage ou langage machine, afin de créer
un programme exécutable par une machine.

Un compilateur effectue les opérations suivantes : analyse lexicale, pré-traitement (préprocesseur), analyse syntaxique (parsing), analyse
sémantique, et génération de code optimisé. La compilation est souvent suivie d'une étape d'édition de liens, pour générer un fichier exécutable.

On distingue deux options de compilation :
	- Ahead-of-time (AOT), où il faut compiler le programme avant de lancer l'application : la situation traditionnelle.
	- Compilation à la volée (just-in-time, en abrégé JIT) : cette faculté est apparue dans les années 1980 (par exemple Tcl/Tk)

La tache principale d'un compilateur est de produire un code objet correct qui s'exécutera sur un ordinateur. La plupart des compilateurs permettent
d'optimiser le code, c'est-à-dire qu'ils vont chercher à améliorer la vitesse d'exécution, ou réduire l'occupation mémoire du programme.

Un compilateur fonctionne par analyse-synthèse : au lieu de remplacer chaque construction du langage source par une suite équivalente de constructions
du langage cible, il commence par analyser le texte source pour en construire une *représentation intermédiaire* qu'il traduit à son tour en langage cible.

On sépare le compilateur en au moins deux parties : une partie avant (ou frontale), parfois appelée "souche", qui lit le texte source et produit la
représentation intermédiaire ; et la partie arrière (ou finale), qui parcours cette représentation pour produire le texte cible. Dans un compilateur
idéal, la partie avant est indépendante du langage cible, tandis que la partie arrière est indépendante du langage source (voir LLVM).

L'implémentation d'un langage de programmation peut être interprétée ou compilée. Cette réalisation est un compilateur ou un interpréteur, et un
langage de programmation peut avoir une implémentation compilée, et une autre interprétée.

On parle de compilation si la traduction est faite avant l'exécution, et d'interprétation si la traduction est finie pas à pas, durant l'exécution.

Les premiers compilateurs ont été écrits directement en langage assembleur, un langage symbolique élémentaire correspondant aux instructions du
processeur cible et quelques structures de contrôle légèrement plus évoluées. Ce langage symbolique doit être assemblé (et non compilé) et lié pour
obtenir une version exécutable. En raison de sa simplicité, un programme simple suffit à le convertir en instruction machines.

Les compilateurs actuels sont généralement écrits dans le langage qu'ils doivent compiler : il ne dépend alors plus d'un autre langage pour être produit.

Dans ce cas, il est complexe de détecter un bogue de compilateur. Le *bootstrap* oblige donc les programmeurs de compilateurs à contourner les bugs des
compilateurs existants.
