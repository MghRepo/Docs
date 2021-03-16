# Haskell

#### Table des matières
* [Introduction](#introduction)
* [Valeurs, Types, etc.](#valeurs-types-etc)
    + [Types polymorphiques](#types-polymorphiques)
    + [Types définis par l'utilisateur](#types-définis-par-lutilisateur)
        - [Types récursifs](#types-récursifs)
    + [Types synonymes](#types-synonymes)
    + [Types intégrés](#types-intégrés)
        - [Lister des compréhensions et des séquences arithmétiques](#lister-des-compréhensions-et-des-séquences-arithmétiques)
        - [Chaînes](#chaînes)
* [Fonctions](#fonctions)
    + [Abstractions lambda](#abstractions-lambda)
    + [Opérateurs infixes](#opérateurs-infixes)
        - [Sections](#sections)
        - [Déclarations de fixité](#declaration-de-fixite)
    + [Fonctions non-strictes](#fonctions-non-strictes)
    + [Structures de données "infinies"](#structures-de-données-infinies)
    + [Fonction erreur](#fonction-erreur)
* [Expressions de cas et correspondances de motifs](#expressions-de-cas-et-correspondances-de-motifs)
    + [Sémantiques des correspondances de motifs](#sémantiques-des-correspondances-de-motifs)
    + [Exemple](#exemple)
    + [Expressions de cas](#expressions-de-cas)
    + [Motif paresseux](#motif-paresseux)
    + [Cadrage lexical et formes imbriquées](#cadrage-lexical-et-formes-imbriquées)
    + [Schéma](#schéma)
* [Classes de type et surcharge](#classes-de-type-et-surcharge)
* [Déclaration de type](#déclaration-de-type)
    + [Déclaration de nouveau type](#déclaration-de-nouveau-type)
    + [Étiquettes de champs](#etiquettes-de-champs)
    + [Constructeurs de données stricts](#constructeurs-de-données-stricts)
* [Entrées/Sorties](#entréessorties)
    + [Opérations I/O basiques](#opérations-io-basiques)
    + [Programmer à l'aide d'actions](#programmer-à-laide-dactions)
    + [Gestion des exceptions](#gestion-des-exceptions)
    + [Files, canaux et poignées](#files-canaux-et-poignées)
    + [Haskell et programmation impérative](#haskell-et-programmation-impérative)
* [Classes Haskell standards](#classes-haskell-standards)
    + [Classes d'égalité et d'ordre](#classes-dégalité-et-dordre)
    + [Classe d'énumération](#classe-dénumération)
    + [Classes de lecture et d'affichage](#classes-de-lecture-et-daffichage)
* [A propos des monads](#a-propos-des-monads)
    + [Classes monadiques](#classes-monadiques)
    + [Monads inclus](#monads-inclus)
    + [Utiliser les monads](#utiliser-les-monads)
* [Nombres](#nombres)
    + [Structure de classe numérique](#structure-de-classe-numérique)
    + [Nombres construits](#nombres-construits)
    + [Coercitions numériques et littéraux surchargés](#coercitions-numériques-et-littéraux-surchargés)
    + [Types numériques par défaut](#types-numériques-par-défaut)
* [Modules](#modules)
    + [Noms qualifiés](#noms-qualifiés)
    + [Types de données abstraites](#types-de-données-abstraites)
    + [Plus de fonctionnalités](#plus-de-fonctionnalités)
* [Pièges de typage](#pièges-de-typage)
    + [Polymorphisme laissé lié](#polymorphisme-laissé-lié)
    + [Surcharge numérique](#surcharge-numérique)
    + [Restriction monomorphique](#restriction-monomorphique)
* [Tableaux](#tableaux)
    + [Types d'index](#types-dindex)
    + [Création de tableau](#création-de-tableau)
    + [Accumulation](#accumulation)
    + [Mises à jour incrémentales](#mises-à-jour-incrémentales)
    + [Exemple : Multiplication de matrices](#exemple--multiplication-de-matrices)

## Introduction

Haskell est un langage de programmation statiquement typé, purement fonctionnel avec inférence de type et évaluation paresseuse.

## Valeurs, Types, etc.

Du fait que Haskell est un langage purement fonctionnel, tous les calculs sont issus de l'évaluation *d'expressions* (termes
syntaxiques) afin de produire des *valeurs* (des entités abstraites désignées comme réponses). Chaque valeurs est associée à un
*type*. (Intuitivement, les types peuvent être considérés comme des ensembles de valeurs.) Des exemples d'expressions incluent des
valeurs atomiques telles que l'entier 5, le caractère 'a', et la fonctiomn \x -> x+1, ainsi que des valeurs structurées telles que
la liste [1, 2, 3] et le couple ('b', 4).

De la même manière que les expressions dénotent des valeurs, les types d'expressions sont des termes syntaxiques qui dénotent des
types de valeurs (ou juste *types*). Des exemples de types d'expressions incluent les types atomiques **Integer** (entiers de
précision infinie), **Char** (caractères), **Integer -> Integer**(fonctions associant un entier à un entier), ainsi que les types
structurés **[Integer]** (Listes d'entiers homogènes) et **(Char, Integer)** (paires caractère, entier).

Toutes les valeurs Haskell sont dites "first-class" -- elles peuvent être passées en tant qu'arguments à des fonctions, et
retournées en tant que résultats, placées dans des structures de données, etc. En revanche, les types Haskell ne sont *pas*
first-class. Les types décrivent des valeurs en un sens, et l'association d'une valeur avec un type et appellé *typage*. En
utilisant les exemples de valeurs et de types ci-dessus, on écrit des typages ainsi :

    5   ::  Integer
    'a' ::  Char
    inc ::  Integer -> Integer
    [1, 2, 3]   ::  [Integer]
    ('b', 4)    ::  (Char, Integer)

Le "::" peut être lu "appartient au type".

Les fonctions en Haskell sont généralement définies par une série *d'équations*. Par exemple, la fonction **inc** peut être définie
par une seule équation :

    inc n   = n+1

Une équation est un exemple d'une *déclaration*. Un autre genre de déclaration et une *déclaration de signature de type*, dans
laquelle on peut déclarer un typage explicite pour **inc** :

    inc     :: Integer -> Integer

Quand on souhaite indiquer qu'une expression *e₁* peut être évaluée, ou "réduite", à une autre expression ou valeur *e₂*, on
écrira :

    e₁ => e₂

Par exemple, on notera :

    inc (inc 3)     =>      5

Le système de types statique de Haskell définit la relation formelle entre types et valeurs. Le système de types statique garantit
que les programmes Haskell sont *type safe* ; c'est à dire, que le développeur n'a pas, d'une manière ou d'une autre, confondus des
types. Par exemple, on ne peut pas additioner ensemble deux caractères, ainsi l'expression 'a'+'b' ne respecte pas le typage.
L'avantage principal des langages statiquement typés est bien connu : Toutes les erreurs de types sont détectées à la compilation.
Toutes les erreurs ne sont pas détectées par le système de type ; une expression telle que 1/0 est typable mais son évaluation
résultera en une erreur à l'exécution. Mais le système de type permet de trouver de nombreuses erreurs lors de la compilation, à
l'utilisateur de raisonner plus facilement sur les programmes, et également au compilateur de générer un code efficient (par
exemple, aucune marque de types à l'exécution ou de tests ne sont requis).

Le système de type garantit également que les signatures de types fournies par l'utilisateur sont correctes. En fait, le système de
type de Haskell est assez puissant pour permettre de nous éviter d'écrire n'importe quelle signature de type ; on dit que le système
de types infère pour nous les bons types. Néanmoins le placement judicieux de signatures de types telles que nous l'avons donnée
pour **inc** est une bonne idée, puisque les signatures de types sont une forme très efficace de documentation et aide à mettre en
lumière les erreurs de programmation.

> Note : Les identifiants dénotant des types spécifiques ont des identifiants capitalisés, tels que **Integer** et **Char**, mais
pas les identifiants qui dénotent des valeurs, tels que **inc**. Ceci n'est pas seulement une convention : elle est requise par la
syntaxe lexicale de Haskell. La casse des autres caractères est également prise en compte : **foo, fOo,** et **fOO** sont des
identifiants distincts.

### Types polymorphiques

### Types définis par l'utilisateur

#### Types récursifs

### Types synonymes

### Types intégrés

#### Lister des compréhensions et des séquences arithmétiques

#### Chaînes

## Fonctions

### Abstractions lambda

### Opérateurs infixes

#### Sections

#### Déclarations de fixité

### Fonctions non-strictes

### Structures de données "infinies"

### Fonction erreur

## Expression de cas et correspondance de motifs

### Sémantiques des correspondances de motifs

### Exemple

### Expression de cas

### Motif paresseux

### Cadrage lexical et formes imbriquées

### Schéma

## Classes de type et surcharge

## Déclaration de type

### Déclaration de nouveau type

### Étiquettes de champs

### Constructeurs de données stricts

## Entrées/Sorties

### Opérations I/O basiques

### Programmer à l'aide d'actions

### Gestion des exceptions

### Files, canaux et poignées

### Haskell et programmation impérative

## Classes Haskell standards

### Classes d'égalité et d'ordre

### Classe d'énumération

### Classes de lecture et d'affichage

## A propos des monads

### Classes monadiques

### Monads inclus

### Utiliser les monads

## Nombres

### Structure de classe numérique

### Nombres construits

### Coercitions numériques et littéraux surchargés

### Types numériques par défaut

## Modules

### Noms qualifiés

### Types de données abstraites

### Plus de fonctionnalités

## Pièges de typage

### Polymorphisme laissé lié

### Surcharge numérique

### Restriction monomorphique

## Tableaux

### Types d'index

### Création de tableau

### Accumulation

### Mises à jour incrémentales

### Exemple : Multiplication de matrices
