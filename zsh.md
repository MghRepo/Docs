# Zsh

#### Table des matières

* [Introduction](#introduction)
* [Feuille de route](#feuille-de-route)
    + [Démarrage](#démarrage)
    + [Usage interactif](#usage-interactif)
        - [Completion](#completion)
        - [Étendre l'éditeur de ligne](#étendre-léditeur-de-ligne)
    + [Options](#options)
    + [Correspondance de motifs](#correspondance-de-motifs)
    + [Commentaires généraux sur la syntaxe](#commentaires-généraux-sur-la-syntaxe)
    + [Programmation](#programmation)
* [Invocation zsh](#invocation-zsh)
    + [Invocation](#invocation)
    + [Compatibilité](#compatibilité)
    + [Shell restreint](#shell-restreint)
* [Fichiers zsh](#fichiers-zsh)
    + [Fichiers de démarrage/arrêt](#fichiers-de-démarragearrêt)
    + [Fichiers](#fichiers)
* [Grammaire shell](#grammaire-shell)
    + [Commandes simples et pipes](#commandes-simples-et-pipes)
    + [Modifieurs de précommande](#modifieurs-de-précommande)
    + [Commandes complexes](#commandes-complexes)
    + [Formes alternées pour commandes complexes](#formes-alternées-pour-commandes-complexes)
    + [Mots réservés](#mots-réservés)
    + [Erreurs](#erreurs)
    + [Commentaires](#commentaires)
    + [Alias](#alias)
        - [Difficultés d'alias](#difficultés-dalias)
    + [Citations](#citations)
* [Redirection](#redirection)
    + [Ouvrir des descripteurs de fichiers à l'aide de paramètres](#ouvrir-des-descripteurs-de-fichiers-à-laide-de-paramètres)
    + [Multio](#multio)
    + [Redirections sans commandes](#redirections-sans-commandes)
* [Exécution de commande](#exécution-de-commande)
* [Fonctions](#fonctions)
    + [Fonctions chargées automatiquement](#fonctions-chargées-automatiquement)
    + [Fonctions anonymes](#fonctions-anonymes)
    + [Fonctions spéciales](#fonctions-spéciales)
        - [Fonctions Hooks](#fonctions-hooks)
        - [Fonctions pièges](#fonctions-pièges)
* [Jobs et signaux](#jobs-et-signaux)
    + [jobs](#jobs)
    + [signaux](#signaux)
* [Évaluation arithmétique](#évaluation-arithmétique)
* [Expressions conditionnelles](#expressions-conditionnelles)
* [Expansion du prompt](#expansion-du-prompt)
    + [Expansion des séquences de prompt](#expansion-des-séquences-de-prompt)
    + [Échappements de prompt simples](#échappements-de-prompt-simple)
        - [Caractères spéciaux](#caractères-spéciaux)
        - [Information de connexion](#information-de-connexion)
        - [Etat du shell](#état-du-shell)
        - [Date et heure](#date-et-heure)
        - [Effets visuels](#effets-visuels)
    + [Sous-chaînes conditionnelles dans les prompts](#sous-chaînes-conditionnelles-dans-les-prompts)
* [Expansion](#expansion)
    + [Expansion d'historique](#expansion-dhistorique)
        - [Aperçu](#aperçu)
        - [Désignateurs d'évènements](#désignateurs-dévènements)
        - [Désignateurs de mots](#désignateurs-de-mots)
        - [Modifieurs historique](#modifieurs-historique)
    + [Substitution de processus](#substitution-de-processus)
    + [Expansion de paramètres](#expansion-de-paramètres)
        - [Drapeaux d'expansion de paramètres](#drapeaux-dexpansion-de-paramètres)
        - [Règles d'expansion](#règles-dexpansion)
    + [Substitution de commande](#substitution-de-commande)
    + [Expansion arithmétique](#expansion-arithmétique)
    + [Expansion accolade](#expansion-accolade)
    + [Expansion de nom de fichier](#expansion-de-nom-de-fichier)
        - [Répertoires nommés dynamiquement](#répertoires-nommés-dynamiquement)
        - [Répertoires nommés statiquement](#répertoires-nommés-statiquement)
        - [Expansion '='](#expansion-=)
        - [Notes expansion](#notes-expansion)
    + [Génération de noms de fichiers](#génération-de-noms-de-fichiers)
        - [Opérateurs globs](#opérateurs-globs)
        - [Précédence](#précédence)
        - [Drapeaux globs](#drapeaux-globs)
        - [Correspondance approximative](#correspondance-approximative)
        - [Globs récursif](#globs-récursif)
        - [Qualifieurs globs](#qualifieurs-globs)
* [Paramètres](#paramètres)
    + [Description paramètres](#description-paramètres)
    + [Paramètres de tableau](#paramètres-de-tableau)
        - [Indice de tableau](#indice-de-tableau)
        - [Assignation d'élément de tableau](#assignation-délément-de-tableau)
        - [Drapeau d'indice](#drapeau-de-indice)
        - [Analyse d'indice](#analyse-dindice)
    + [Paramètres positionnels](#paramètres-positionnels)
    + [Paramètres locaux](#paramètres-locaux)
    + [Paramètres définis par le shell](#paramètres-définis-par-le-shell)
    + [Paramètres utilisés par le shell](#paramètres-utilisés-par-le-shell)
* [Options](#options)
    + [Spécifier des options](#spécifier-des-options)
    + [Descriptions des options](#descriptions-des-options)
        - [Changer de répertoires](#changer-de-répertoires)
        - [Complétion options](#complétion-options)
        - [Expansion et globs](#expansion-et-globs)
        - [Historique options](#historique-options)
        - [Initialisation](#initialisation)
        - [Entrées/Sorties](#entréessorties)
        - [Contrôle de job](#contrôle-de-job)
        - [Affichage](#affichage)
        - [Scripts et fonctions](#scripts-et-fonctions)
        - [Émulation de shell](#émulation-de-shell)
        - [État de shell](#état-de-shell)
        - [ZLE](#zle)
    + [Options d'alias](#options-dalias)
    + [Options courtes](#options-courtes)
        - [Ensemble par défaut](#ensemble-par-défaut)
        - [Ensemble d'émulation sh/ksh](#ensemble-démulation-shksh)
        - [À noter également](#à-noter-également)
* [Commandes intégrées au shell](#commandes-intégrées-au-shell)
* [Éditeur de ligne zsh](#éditeur-de-ligne-zsh)
    + [Description de l'éditeur](#description-de-léditeur)
    + [Raccourcis](#raccourcis)
        - [Lecture de commandes](#lecture-de-commandes)
        - [Raccourcis locaux](#raccourcis-locaux)
    + [Contenus ZLE](#contenus-zle)
    + [Widgets](#widgets)
    + [Widgets définis par l'utilisateur](#widgets-définis-par-lutilisateur)
        - [Widgets spéciaux](#widgets-spéciaux)
    + [Widgets standards](#widgets-standards)
        - [Mouvement](#mouvement)
        - [Contrôle de l'historique](#contrôle-de-lhistorique)
        - [Modifier du texte](#modifier-de-texte)
        - [Arguments](#arguments)
        - [Complétion widgets](#complétion-widgets)
        - [Divers](#divers)
        - [Objets de texte](#objets-de-texte)
    + [Surlignage de caractères](#surlignage-de-caractères)
* [Widgets de complétion](#widgets-de-complétion)
    + [Description des widgets](#description-des-widgets)
    + [Paramètres spéciaux de complétion](#paramètres-spéciaux-de-complétion)
    + [Commandes de complétion intégrées](#commandes-de-complétion-intégrées)
    + [Codes conditions de complétion](#codes-conditions-de-complétion)
    + [Contrôle de correspondance de complétion](#contrôle-de-correspondance-de-complétion)
* [Système de complétion](#système-de-complétion)
    + [Description du système de complétion](#description-du-système-de-complétion)
    + [Initialisation du système de complétion](#initialisation-du-système-de-complétion)
        - [Utilisation de compinit](#utilisation-de-compinit)
        - [Fichiers chargés automatiquement](#fichiers-chargés-automatiquement)
        - [Fonctions du système de complétion](#fonctions-du-système-de-complétion)
    + [Configuration du système de complétion](#configuration-du-système-de-complétion)
        - [Aperçu de la configuration](#aperçu-de-la-configuration)
        - [Marques standards](#marques-standards)
        - [Styles standards](#styles-standards)
    + [Fonctions de contrôle](#fonctions-de-contrôle)
    + [Commandes liables](#commandes-liables)
    + [Fonctions d'utilité](#fonctions-dutilité)
    + [Système de complétion de variable](#système-de-complétion-de-variable)
    + [Complétion de répertoires](#complétion-de-répertoires)
* [Complétion à l'aide de compctl](#complétion-à-laide-de-compctl)
    + [Types de complétions](#types-de-complétions)
    + [Description de compctl](#description-de-compctl)
    + [Drapeaux de commandes](#drapeaux-de-commandes)
    + [Drapeaux d'options](#drapeaux-doptions)
        - [Drapeaux simples](#drapeaux-simples)
        - [Drapeaux avec arguments](#drapeaux-avec-arguments)
        - [Drapeaux de contrôle](#drapeaux-de-contrôle)
    + [Complétion alternative](#complétion-alternative)
    + [Complétion étendue](#complétion-étendue)
* [Modules zsh](#Modules-zsh)
    + [Description des modules](#description-des-modules)
    + [Module zsh/attr](#module-zshattr)
    + [Module zsh/cap](#module-zshcap)
    + [Module zsh/clone](#module-zshclone)
    + [Module zsh/compctl](#module-zshcompctl)
    + [Module zsh/complete](#module-zshcomplete)
    + [Module zsh/complist](#module-zshcomplist)
        - [Complétion de listes colorée](#complétion-de-listes-colorée)
        - [Balayer des listes de complétion](#balayer-des-listes-de-complétion)
        - [Menu de sélection](#menu-de-sélection)
    + [Module zsh/computil](#module-zshcomputil)
    + [Module zsh/curses](#module-zshcurses)
        - [Curses inclus](#curses-inclus)
        - [Curses paramètres](#curses-paramètres)
    + [Module zsh/datetime](#module-zshdatetime)
    + [Module zsh/gdbm](#module-zshgdbm)
    + [Module zsh/deltochar](#module-zshdeltochar)
    + [Module zsh/example](#module-zshexample)
    + [Module zsh/files](#module-zshfiles)
    + [Module zsh/langinfo](#module-zshlanginfo)
    + [Module zsh/mapfile](#module-zshmapfile)
        - [Limitations mapfile](#limitations-mapfile)
    + [Module zsh/mathfunc](#module-zshmathfunc)
    + [Module zsh/nearcolor](#module-zshnearcolor)
    + [Module zsh/newuser](#module-zshnewuser)
    + [Module zsh/parameter](#module-zshparameter)
    + [Module zsh/pcre](#module-zshpcre)
    + [Module zsh/param/private](#module-zshparamprivate)
    + [Module zsh/regex](#module-zshregex)
    + [Module zsh/sched](#module-zshsched)
    + [Module zsh/net/socket](#module-zshnetsocket)
        - [Socket connexions externes](#socket-connexions-externes)
        - [Socket connexion internes](#socket-connexions-internes)
    + [Module zsh/stat](#module-zshstat)
    + [Module zsh/systeme](#module-zshsysteme)
        - [Systeme intégrés](#systeme-intégrés)
        - [Fonctions mathématiques systeme](#fonctions-mathématiques-systeme)
        - [Systeme paramètres](#systeme-paramètres)
    + [Module zsh/net/tcp](#module-zshnettcp)
        - [TCP connexions externes](#tcp-connexions-externes)
        - [TCP connexions internes](#tcp-connexions-internes)
        - [Fermer des connexions](#fermer-des-connexions)
    + [Module zsh/termcap](#module-zshtermcap)
    + [Module zsh/terminfo](#module-zshterminfo)
    + [Module zsh/zftp](#module-zshzftp)
        - [ZFTP sous-commandes](#zftp-sous-commandes)
        - [ZFTP paramètres](#zftp-paramètres)
        - [ZFTP fonctions](#zftp-fonctions)
    + [Module zsh/zle](#module-zshzle)
    + [Module zsh/zleparameter](#module-zshzleparameter)
    + [Module zsh/zprof](#module-zshzprof)
    + [Module zsh/zpty](#module-zshzpty)
    + [Module zsh/zselect](#module-zshzselect)
    + [Module zsh/zutil](#module-zshzutil)
* [Fonctions calendaires du système](#fonctions-calendaires-du-système)
    + [Description des fonctions calendaires](#description-des-fonctions-calendaires)
    + [Formats de fichiers et de dates](#formats-de-fichiers-et-de-dates)
        - [Format de fichier de calendrier](#format-de-fichier-de-calendrier)
        - [Format de date](#format-de-date)
        - [Format de temps relatif](#format-de-temps-relatif)
    + [Fonctions utilisateur](#fonctions-utilisateur)
        - [Fonctions systèmes calendaires utilisateur](#fonctions-systèmes-calendaires-utilisateur)
        - [Qualifieurs globs fonctions calendaires](#qualifieurs-globs-fonctions-calendaires)
    + [Style fonctions calendaires](#style-fonctions-calendaires)
    + [Fonctions utilitaires](#fonctions-utilitaires)
* [Fonctions système TCP](#fonctions-système-TCP)
    + [Description des fonctions TCP](#description-des-fonctions-tcp)
    + [Fonctions utilisateur TCP](#fonctions-utilisateur-tcp) 
        - [Entrées/Sorties basiques](#entréessorties-basiques)
        - [Gestion de session tcp](#gestion-de-session-tcp)
        - [Entrées/Sorties complexes](#entréessorties-complexes)
        - [Transfert de fichier d'une traite](#transfert-de-fichier-dune-traite)
    + [Fonctions TCP définies par l'utilisateur](#fonctions-tcp-définies-par-lutilisateur)
    + [Fonctions TCP utilitaires](#fonctions-tcp-utilitaires)
    + [Paramètres utilisateur TCP](#paramètres-utilisateur-tcp)
* [Fonctions systèmes ZFTP](#fonctions-systèmes-zftp)
    + [Descriptions des fonctions ZFTP](#description-des-fonctions-zftp)
    + [Installation ZFTP](#installation-zftp)
    + [Fonctions ZFTP](#fonctions-zftp)
        - [Ouvrir une connexion](#ouvrir-une-connexion)
        - [Gestion répertoire](#gestion-répertoire)
        - [Commandes de status](#commandes-de-status)
        - [Récupérer des fichiers](#récupérer-des-fichiers)
        - [Envoyer des fichiers](#envoyer-des-fichiers)
        - [Fermer la connexion](#fermer-la-connexion)
        - [Gestion de session ZFTP](#gestion-de-session-zftp)
        - [Marques pages](#marques-pages)
        - [Autres fonctions ZFTP](#autres-fonctions-zftp)
    + [Diverses fonctionnalités](#diverses-fonctionnalités)
        - [Configuration ZFTP](#configuration-zftp)
        - [Globs distants](#globs-distants)
        - [Réouvertures temporaires et automatiques](#réouvertures-temporaires-et-automatiques)
        - [Complétion ZFTP](#complétion-zftp)
* [Contributions des utilisateurs](#contributions-des-utilisateurs)
    + [Description des contributions](#description-des-contributions)
    + [Utilitaires](#utilitaires)
        - [Accéder à l'aide en ligne](#accéder-à-laide-en-ligne)
        - [Recompiler des fonctions](#recompiler-des-fonctions)
        - [Définition clavier](#définition-clavier)
        - [Vider l'état du shell](#vider-létat-du-shell)
        - [Manipuler les fonctions de hook](#manipuler-les-fonctions-de-hook)
    + [Rappeler les répertoires récents](#rappeler-les-répertoires-récents)
        - [Installation rappel](#installation-rappel)
        - [Utilisation rappel](#utilisation-rappel)
        - [Options rappel](#options-rappel)
        - [Configuration rappel](#configuration-rappel)
        - [Utilisation avec nommage répertoire dynamique](#utilisation-avec-nommage-répertoire-dynamiquement)
        - [Détails concernant le traitement des répertoires](#détails-concernant-le-traitement-des-répertoires)
    + [Références dynamiques abrégées aux répertoires](#références-dynamiques-abrégées-aux-répertoires)
        - [Utilisation des références dynamiques](#utilisation-des-références-dynamiques)
        - [Configuration des références dynamiques](#configuration-des-références-dynamiques)
    + [Récupérer des informations de système de contrôle de version](#récupérer-des-informations-de-système-de-contrôle-de-version)
        - [Introduction rapide](#introduction-rapide)
        - [Configuration récupération](#configuration-récupération)
        - [Bizarreries](#bizarreries)
        - [Support Quilt](#support-quilt)
        - [Description des fonctions (API publique)](#Description-des-fonctions-api-publiques)
        - [Description variable](#description-variable)
        - [Hooks vcs_info](#hooks-vcs_info)
    + [Thèmes de prompt](#thèmes-de-prompt)
        - [Installation de thèmes](#installation-de-thèmes)
        - [Sélection de thèmes](#sélection-de-thèmes)
        - [Thèmes utilitaires](#thèmes-utilitaires)
        - [Écrire des thèmes](#écrire-des-thèmes)
    + [Fonctions ZLE](#fonctions-zle)
        - [Widgets ZLE](#widgets-zle)
        - [Fonctions utilitaires ZLE](#fonctions-utilitaires-zle)
        - [Styles ZLE](#styles-zle)
    + [Gestion des exceptions](#gestion-des-exceptions)
    + [Fonctions MIME](#fonctions-mime)
    + [Fonctions mathématiques](#fonctions-mathématiques)
    + [Fonctions de configuration utilisateurs](#fonctions-de-configuration-utilisateurs)
    + [Autres fonctions](#autres-fonctions)
        - [Descriptions des autres fonctions](#description-des-autres-fonctions)
        - [Styles des autres fonctions](#styles-des-autres-fonctions)

## Introduction

Zsh est un interpréteur de commande UNIX (shell) utilisable en tant que shell de connexion interactif et en tant que processeur de
commande de script shell. De tous les shells standards, zsh ressemble le plus à *ksh* mais il inclut également de nombreuses
améliorations. Il ne fournit pas de compatibilité POSIX ni avec d'autres shells dans son mode opérationnel par défaut.

Zsh a l'édition de ligne de commande, la correction orthographique incluse, la complétion de commande programmable, les fonctions
shell (avec chargement automatique), un mécanisme d'historique, ainsi que beaucoup d'autres fonctionnalités.

## Feuille de route

Le manuel zsh, de même que le shell lui-même, est vaste et souvent compliqué. Cette section du manuel fournit quelques directions
vers les aires du shell qui sont susceptibles d'intéresser les nouveaux utilisateurs, et indique où est-ce que le reste de la
documentation du manuel peut se trouver.

### Démarrage

Au démarrage, le shell lit des commandes de divers fichiers. Ceux-ci peuvent être créés ou édité pour personnaliser le shell.

### Usage interactif

#### Complétion

#### Étendre l'éditeur de ligne

### Options

### Correspondance de motifs

### Commentaires généraux sur la syntaxe

### Programmation

## Invocation zsh

### Invocation

### Compatibilité

### Shell restreint

## Fichier zsh

### Fichiers de démarrage/arrêt

### Fichiers

## Grammaire shell

### Commandes simples et pipes

### Modifieurs de précommande

### Commandes complexes

### Formes alternées pour commandes complexes

### Mots réservés

### Erreurs

### Commentaires

### Alias

#### Difficultés d'alias

### Citations

## Redirection

### Ouvrir des descripteurs de fichiers à l'aide de paramètres

### Multio

### Redirections sans commandes

## Exécution de commande

## Fonctions

### Fonctions chargées automatiquement

### Fonctions anonymes

### Fonctions spéciales

#### Fonctions hooks

#### Fonctions pièges

## Jobs et signaux

### Jobs

### Signaux

## Évaluation arithmétique

## Expressions conditionnelles

## Expansion de prompt

### Expansion des séquences de prompt

### Échappements de prompt simples

#### Caractères spéciaux

#### Information de connexion

#### État du shell

#### Date et heure

#### Effets visuels

### Sous-chaînes conditionnelles dans les prompts

## Expansion

### Expansion d'historique

#### Aperçu

#### Désignateurs d'évènements

#### Désignateurs de mots

#### Modifieurs historique

### Substitution de processus

### Expansion de paramètres

#### Drapeaux d'expansion de paramètres

#### Règles d'expansion

### Substitution de commande

### Expansion arithmétique

### Expansion accolade

### Expansion de nom de fichier

#### Répertoires nommés dynamiquement

#### Répertoires nommés statiquement

#### Expansion '='

#### Notes expansion

### Génération de noms de fichiers

#### Opérateurs globs

#### Précédence

#### Drapeaux globs

#### Correspondance approximative

#### Globs récursifs

#### Qualifieurs globs

## Paramètres

### Description paramètres

### Paramètres de tableau

#### Indices de tableau

#### Assignation d'élément de tableau

#### Drapeau d'indice

#### Analyse d'indice

### Paramètres positionnels

### Paramètres locaux

### Paramètres définis par le shell

## Options

### Spécifier des options

### Description des options

#### Changer de répertoire

#### Complétion options

#### Expansion et globs

#### Historique options

#### Initialisation

#### Entrées/Sorties

#### Contrôle de job

#### Affichage

#### Scripts et fonctions

#### Émulation de shell

#### État de shell

#### ZLE

### Options d'alias

### Options courtes

#### Ensemble par défaut

#### Ensemble d'émulation sh/ksh

#### À noter également

## Commandes intégrées au shell

## Éditeur de ligne zsh

### Description de l'éditeur

### Raccourcis

#### Lecture de commandes

#### Raccourcis locaux

### Contenus ZLE

### Widgets

### Widgets définis par l'utilisateur

#### Widgets spéciaux

### Widgets standards

#### Mouvement

#### Contrôle de l'historique

#### Modifier du texte

#### Arguments

#### Complétion widgets

#### Divers

#### Objets de texte

### Surlignage de caractères

## Widgets de complétion

### Description des widgets

### Paramètres spéciaux de complétion

### Commandes de complétion intégrées

### Codes conditions de complétion

### Contrôle de correspondance de complétion

## Système de complétion

### Description du système de complétion

### Initialisation du système de complétion

#### Utilisation de compinit

#### Fichiers chargés automatiquement

#### Fonctions du système de complétion

### Configuration du système de complétion

#### Aperçu de la configuration

#### Marques standards

#### Styles standards

### Fonctions de contrôle

### Commandes liables

### Fonctions d'utilité

### Système de complétion de variable

### Complétion de répertoires

## Complétion à l'aide de compctl

### Types de complétions

### Description compctl

### Drapeaux de commandes

### Drapeaux d'options

#### Drapeaux simples

#### Drapeaux avec arguments

#### Drapeaux de contrôle

### Complétion alternative

### Complétion étendue

## Modules zsh

### Description des modules

### Module zsh/attr

### Module zsh/cap

### Module zsh/clone

### Module zsh/compctl

### Module zsh/complete

### Module zsh/complist

#### Complétion de listes colorée

#### Balayer des listes de complétion

#### Menu de sélection

### Module zsh/computil

### Module zsh/curses

#### Curses inclus

#### Curses paramètres

### Module zsh/datetime

### Module zsh/gdbm

### Module zsh/deltochar

### Module zsh/example

### Module zsh/files

### Module zsh/langinfo

### Module zsh/mapfile

#### Limitations mapfiles

### Module zsh/mathfunc

### Module zsh/nearcolor

### Module zsh/newuser

### Module zsh/parameter

### Module zsh/pcre

### Module zsh/param/private

### Module zsh/regex

### Module zsh/sched

### Module zsh/net/socket

#### Socket connexions externes

#### Socket connexions internes

### Module zsh/stat

### Module zsh/systeme

#### Systeme intégrés

#### Fonctions mathématiques systeme

#### Systeme paramètres

### Module zsh/net/tcp

#### TCP connexions externes

#### TCP connexions internes

#### Fermer des connexions

### Module zsh/termcap

### Module zsh/terminfo

### Module zsh/zftp

#### ZFTP sous-commandes

#### ZFTP paramètres

#### ZFTP fonctions

### Module zsh/zle

### Module zsh/zleparameter

### Module zsh/zprof

### Module zsh/zpty

### Module zsh/zselect

### Module zsh/util

## Fonctions calendaires du système

### Description des fonctions calendaires

### Formats de fichiers et de dates

#### Format de fichier de calendrier

#### Format de date

#### Format de temps relatif

### Fonctions utilisateur

#### Fonctions système calendaires utilisateur

#### Qualifieurs globs fonctions calendaires

### Style fonctions calendaires

### Fonctions utilitaires

## Fonctions système TCP

### Description des fonctions TCP

### Fonctions utilisateur TCP

#### Entrées/Sorties basiques

#### Gestion de session tcp

#### Entrée/Sorties complexes

#### Transfert de fichier d'une traite

### Fonctions TCP définies par l'utilisateur

### Fonctions TCP utilitaires

### Paramètres utilisateur TCP

## Fonctions systèmes ZFTP

### Description des fonctions ZFTP

### Installation ZFTP

### Fonctions ZFTP

#### Ouvrir une connexion

#### Gestion d'un répertoire

#### Commandes de status

#### Récupérer des fichiers

#### Envoyer des fichiers

#### Fermer la connexion

#### Gestion de session ZFTP

#### Marques pages

#### Autres fonctions ZFTP

### Diverses fonctionnalités

#### Configuration ZFTP

#### Globs distants

#### Réouvertures temporaires et automatiques

#### Complétion ZFTP

## Contribution des utilisateurs

### Description des contributions

### Utilitaires

#### Accéder à l'aide en ligne

#### Recompiler des fonctions

#### Définition clavier

#### Vider l'état du shell

#### Manipuler les fonctions de hook

### Rappeler les répertoires récents

#### Installation rappel

#### Utilisation rappel

#### Options rappel

#### Configuration rappel

#### Utilisation avec nommage répertoire dynamique

#### Détails concernant le traitement des répertoires

### Références dynamiques abrégées aux répertoires

#### Utilisation des références dynamiques

#### Configuration des références dynamiques

### Récupérer des informations de système de contrôle de version

#### Introduction rapide

#### Configuration récupération

#### Bizarreries

#### Support Quilt

#### Description des fonctions (API publique)

#### Description variable

#### Hooks vcs_info

### Thèmes de prompt

#### Installation de thèmes

#### Sélection de thèmes

#### Thème utilitaires

#### Écrire des thèmes

### Fonctions ZLE

#### Widgets ZLE

#### Fonctions utilitaires ZLE

#### Style ZLE

### Gestion des exceptions

### Fonctions MIME

### Fonctions mathématiques

### Fonctions de configuration utilisateurs

### Autres fonctions

#### Description des autres fonctions

#### Styles des autres fonctions

