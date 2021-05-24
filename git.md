# Git

#### Table des matières

* [Introduction](#introduction)
    + [Contrôle de version](#contrôle-de-version)
    + [Première configuration](#première-configuration)
    + [Aide](#aide)
    + [Créer un dépôt Git](#créer-un-dépôt-git)
    + [Enregistrer des changements](#enregistrer-des-changements)
    + [Historique des commits](#historique-des-commits)
    + [Défaire les choses](#défaire-les-choses)
    + [Dépôts distants](#dépôts-distants)
    + [Marquage](#marquage)
    + [Alias](#alias)
* [Branches](#branches)
    + [Branches en quelques mots](#branches-en-quelques-mots)
    + [Création de branches et fusion](#création-de-branches-et-fusion)
    + [Gestion des branches](#gestion-des-branches)
    + [Flux de travail](#flux-de-travail)
    + [Branches distantes](#branches-distantes)
    + [Rebaser](#rebaser)
* [Serveur](#serveur)
    + [Les protocoles](#les-protocoles)
    + [Installer git sur un serveur](#installer-git-sur-un-serveur)
    + [Générer votre clef SSH publique](#générer-votre-clef-ssh-publique)
    + [Mise en place du serveur](#mise-en-place-du-serveur)
    + [Daemon](#daemon)
    + [HTTP intelligent](#http-intelligent)
    + [GitLab](#git-lab)
* [Git distribué](#git-distribué)
    + [Flux de travails distribués](#flux-de-travails-distribués)
    + [Contribuer à un projet](#contribuer-à-un-projet)
    + [Maintenir un projet](#maintenir-un-projet)
* [GitHub](#github)
    + [Configuration du profil](#configuration-du-profil)
    + [Contribuer à un projet](#contribuer-à-un-projet)
    + [Maintenir un projet](#gérer-un-projet)
    + [Gérer une organisation](#gérer-une-organisation)
    + [GitHub scripting](#gihub-scripting)
* [Outils](#outils)
    + [Sélection de révision](#sélection-de-révision)
    + [Stagging interactif](#stagging-intéractif)
    + [Nettoyer et cacher](#nettoyer-et-cacher)
    + [Signer son travail](#signer-son-travail)
    + [Rechercher](#rechercher)
    + [Réécrire l'historique](#réécrire-lhistorique)
    + [Reset démystifié](#reset-démystifié)
    + [Fusion avancée](#fusion-avancée)
    + [Rerere](#rerere)
    + [Débogage](#débogage)
    + [Sous-modules](#sous-modules)
    + [Rassembler](#rassembler)
    + [Remplacer](#remplacer)
    + [Stockage certifié](#stockage-certifié)
* [Personnalisation](#personnalisation)
    + [Configuration](#configuration)
    + [Attributs](#attributs)
    + [Hooks](#hooks)
* [Fonctionnement interne](#fonctionnement-interne)

## Introduction

### Contrôle de version

Un contrôle de version est un système qui enregistre les changements d'un fichier ou d'un ensemble de fichiers à travers le
temps de façon à pouvoir rappeler ultérieurement des versions spécifiques.

#### Les systèmes de contrôle de version locaux

Un VCS local est constitué d'une simple base de données qui garde tous les changements de fichiers sous contrôle de révision.

Un des outils VCS les plus populaire était un système appelé RCS. RCS fonctionne en gardant un ensemble de patch (i.e. Les
différences entre fichiers) avec un format spécial sur le disque ; il peut alors recréer n'importe quel fichier à l'instant
choisi en sommant l'ensemble des patchs.

#### Les systèmes de contrôle de version centralisés

Le problème majeur que les gens ont rencontrés et qu'ils ont eu besoin de collaborer avec d'autres développeurs sur d'autres
systèmes. Les systèmes de contrôle de version centralisés ont été développés pour résoudre ce problème. Ces systèmes (tels que
CVS, Subversion, et Perforce) ont un unique serveur contenant tous les fichiers versionnés, ainsi qu'un certain nombre de client
qui récupèrent des fichiers depuis ce lieu central.

Cette configuration offre de nombreux avantage. Par exemple, chacun sait plus ou moins ce que les autres font sur le projet. Les
administrateurs ont un contrôle très fin sur quelle action un sujet peut effectuer, il est bien plus facile d'administrer un
CVCS que de traiter avec des bases de données locales sur chacun des clients.

Néanmoins, cette configuration a également de sérieux inconvénients. Le plus évident étant le maillon faible que représente le
serveur central. Si ce serveur reste inaccessible pour une heure, alors durant ce laps de temps personne ne peut collaborer ou
sauver les changements versionnés de ce sur quoi ils travaillent. Si le disque dur de la base de données centrale est corrompue,
et qu'un archivage n'a pas été fait, l'intégralité du travail est perdu -- l'historique complet du projet est perdu excepté les
quelques captures esseulées que les contributeurs ont par hasard sur leur machines locales. Les VCSs locaux souffrent du même
problème.

#### Les système de contrôle de version distribués

Dans un système de contrôle de version distribué (tel que Git, Mercurial, Bazaar ou Darcs), les clients ne récupèrent pas
uniquement les dernières captures des fichiers ; mais répliquent l'ensemble du dépôt, dont l'historique complet. Ainsi, si un
serveur est détruit, et que ces systèmes collaboraient avec ce serveur, n'importe lequel de ces dépôts clients peut être recopié
sur le serveur pour le restaurer. Chaque clone est vraiment une sauvegarde complète de l'ensemble des données.

De plus, beaucoup de ces systèmes permettent de traiter efficacement de multiples dépôts distants, et ainsi de travailler avec
différents groupes de personnes de diverses façons simultanément à l'intérieur d'un même projet. Ceci permet de mettre en place
plusieurs types de chaînes de travail ce qui n'est pas possible avec des systèmes centralisés, tels que les modèles
hiérarchiques.

### Première configuration

### Aide

### Créer un dépôt Git

### Enregistrer des changements

### Historique des commits

### Défaire les choses

### Dépôts distants

### Marquage

### Alias

## Branches

### Branches en quelques mots

### Création de branches et fusion

### Gestion des branches

### Flux de travail

### Branches distantes

### Rebaser

## Serveur

### Les protocoles

### Installer git sur un serveur

### Générer votre clef SSH publique

### Mise en place du serveur

### Daemon

### HTTP intelligent

### GitLab

## Git distribué

### Flux de travails distribués

### Contribuer à un projet

### Maintenir un projet

### Gérer une organisation

### GitHub scripting

## Outils

### Sélection de révision

### Stagging interactif

### Nettoyer et cacher

### Signer son travail

### Rechercher

### Réécrire l'historique

### Reset démystifié

### Fusion avancée

### Rerere

### Débogage

### Sous-modules

### Rassembler

### Remplacer

### Stockage certifié

## Personnalisation

### Configuration

### Attributs

### Hooks

## Fonctionnement interne
