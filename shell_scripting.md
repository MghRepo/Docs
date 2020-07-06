# Scripts Shell et Outils

---

## Scripts Shell

La plupart des shells ont un langage de script qui leur est propre.
Ce qui fait que le langage de script shell est différent de langages de scripts plus traditionnels
c'est qu'il est avant tout fait pour de l'administration système. Créer des commandes pipes, écrire dans des fichiers,
lire l'entrée standard etc. Dans cette section, le langage bash étant le plus répandu, c'est celui que nous utiliserons.

Pour assigner des variable en bash, on utilise la syntaxe foo=bar et on accède au contenu de la variable avec
$foo. "foo = bar" ne marche pas puisque le bash l'interprète comme l'appel du programme foo avec 2 arguments = et bar.
En général dans les scripts shell le caractère espace sépare les arguments.

Les chaînes de caractères en bash peuvent être définies à l'aide des délimiteurs ' et ", mais ils ne sont
pas équivalents. Les chaînes délimitées à l'aide de la simple quote sont des litéraux et ne substituent
pas de variables contrairement aux chaînes délimitées à l'aide d'une double quote :

    $ foo=bar
    $ echo "$foo"
        bar
    $ echo '$foo'
        $foo

Comme la plupart des langages de programmation, bash supporte des techniques de contrôle du flux d'exécution tel que
if, case, while et for. On peut également définir des fonctions en bash qui peuvent prendre des arguments.
Exemple :

    mcd () {
        mkdir -p "$1"
        cd "$1"
    }

Ici $1 est le premier argument de la fonction. Contrairement aux autres langages de script, bash utilise
un nombre assez important de variables spéciales qui font référence à des arguments, codes d'erreurs
etc. Voir liste ci-dessous :

    - $0 - Nom du script
    - $1 à $9 - Arguments du script. $1 est le premier argument (resp. autres)
    - $@ - Tous les arguments
    - $# - Nombre d'arguments
    - $? - Code retour de la commande précédente (très utile pour un match par exemple)
    - $$ - Identifiant de processus (PID) du shell courant.
    - $! - Identifiant de processus (PID) de la dernière commande passée en background.
    - !! - Ensemble de la dernière commande passée (arguments inclus). Très utile si on a oublié le sudo (sudo !!).
    - $_ - Dernier argument de la dernière commande. Dans un shell interactif on peut également faire Esc suivi du point.

Les commandes génèrent généralement une sortie via stdout, des erreurs via stderr, et un code retour. Le code retour
permet de savoir le résultat d'exécution, qui peut être utilisé à la suite par d'autres scripts ou commandes.
Généralement, une valeur de 0 signifie que tout s'est bien passé et tout autre valeur est une erreur.

Néanmoins, ce code peut aussi être utilisé pour des commandes conditionnées en utilisant les opérateurs && (et)
et || (ou). Les commandes peuvent également être séparées sur la même ligne avec le ;. La commande true aura toujours
un code retour à 0 tandis que false aura toujours un code à 1. Exemples :

    false || echo "Oups, raté !"

    true || echo "N'est par affiché"

    true && echo "Les choses se sont bien passées."

    false && echo "Pas affiché"

    false ; echo "Ca marche !"

Parfois on souhaite avoir le contenu de la sortie d'une commande dans une variable. On peut le faire à l'aide
d'une substitution de commande. Quand on écrit $(commande) le bash exécutera la commande et substitura son
contenu dans le script avant l'exécution de celui-ci. Par exemple, si on écrit :

    for file in $(ls)

Le shell appelera dans un premier temps ls et parcourera ces valeurs par la suite.

Le bash permet également de substituer une commande de cette façon :

    diff <(ls foo) <(ls bar)

Cette commande montre la différence entre fichiers dans les répertoires foo et bar.

Puisque cela a été relativement rapide, voyons un exemple que montre quelques trucs qu'on peut faire.
Le script parcourera les arguments que nous lui donnerons, grep la chaîne foobar, et l'ajoutera comme
commentaire ci celle-ci est absente.

    #!/bin/bash

    echo "Début du programme à $(date)" # Date sera substituée

    echo "Exécution du program $0 avec $# arguments avec le pid $$"

    for fichier in $@; do
        grep foobar $file 2>&1 >/dev/null
        if [[ $? -ne 0 ]]; then
            echo "Le fichier $fichier ne contient pas le mot foobar, ajout en cours !"
            echo "# foobar" >> "$file"
        fi
    done

Dans le test de comparaison on teste si $? est égal à 0. Bash implémente de nombreuses comparaisons de la sorte (voir man test).
Dans un test on essaye généralement d'utiliser les doubles crochets, les chances de faire des erreurs sont moindres.
Même si cela n'est pas le standard POSIX.
