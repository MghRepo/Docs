# Environnement du système d'information

#### Table des matières

* [Normalisation](#normalisation)
    + [ITIL](#itil)
    + [COBIT](#cobit)
* [Notions générales sur le droit de l'informatique](#notions-générales-sur-le-droit-de-linformatique)
    + [Protection des données individuelles](#protection-des-données-individuelles)
    + [L'usage de la messagerie](#lusage-de-la-messagerie)
    + [Rôle de la CNIL](#rôle-de-la-cnil)
    + [Licences](#licences)
* [Organisation du travail](#organisation-du-travail)
* [Fonctions de PSE](#fonctions-de-pse)
* [Plan de secours et plan de continuité](#plan-de-secours-et-plan-de-continuité)

## Normalisation

### ITIL

ITIL (*Information Technology Infrastructure Library*) est un ensemble d'ouvrages recensant les bonnes pratiques de gestion du
système d'information.

C'est un référentiel méthodologique très large qui aborde les sujets suivants :

* Comment organiser un système d'information ?
* Comment améliorer l'efficacité d'un système d'information ?
* Comment réduire les risques ?
* Comment augmenter la qualité des services informatiques ?

Les recommandations d'ITIL positionnent des blocs organisationnels et des flux d'information.

### COBIT

COBIT (*Control Objectives for Information and related Technology*) est un référentiel de bonnes pratiques d'audit informatique et
de gouvernance des systèmes d'information.

## Notions générales sur le droit de l'informatique

### Protection des données individuelles

### L'usage de la messagerie

### Rôle de la CNIL

### Licences

## Organisation du travail

## Fonctions de PSE

DISI :

* Veiller à l'optimisation des performances des matériels et à celle du taux de disponibilité du système informatique.
* Assurer la gestion des sécurités d'accés et des sauvegardes, ainsi que l'administration des bases de données.
* Assurer la mise en oeuvre d'automates d'exploitation et de programmes utilitaires.
* Exercer des mission d'assistance et de conseil des équipes d'exploitation.
* Exercer un support technique pour les cellules d'assistance directe et pour l'administration des configurations informatiques
implantées dans les services locaux.

Service centraux :

* Participer à la conception technique des systèmes de données et de traitements afin d'optimiser l'usage du système informatique et
préparer les modalité de mise en exploitation.
* Procéder aux études préalables, aux aquisitions de matériels informatiques et de logiciels système.
* Participer aux travaux sur les systèmes gros systèmes, X86, plateformes virtualisées et Cloud.
* Gérer les techniques de cette exploitation ainsi que le suivi et l'environnement de cette exploitation.

## Plan de secours

### Plan de continuité d'activité

Un plan de continuité d'activité (PCA), a pour but de garantir la survie de l'entreprise en cas de sinistre important touchant le
système informatique. Il s'agit de redémarrer l'activité le plus rapidement possible avec le minimum de perte de données. Ce plan
est un des points essentiels de la politique de sécurité informatique d'une entreprise.

Pour qu'un plan de continuité soit réellement adapté aux exigents de l'entreprise, il doit reposer sur une analyse de risque et une
analyse d'impact :

* **L'analyse de risque** débute par une identification des menaces sur l'informatique. Les menaces peuvent être internes ou
externes à l'entreprise. On déduit ensuite le risque qui découle des menaces identifiées ; on mesure l'impact possible de ces
risques. Enfin, on décide de mettre en oeuvre des mesures d'atténuation des risques en se concentrant sur ceux qui ont un impact
significatif. Par exemple, si le risque de panne d'un équipement risque de tout paralyser, on installe un équipement redondant. Les
mesures d'atténuation de risque qui sont mises en oeuvre diminuent le niveau de risque, mais elles ne l'annulent pas : il subsiste
toujours un risque résiduel, qui sera couvert soit par le plan de continuité, soit par d'autres moyens (assurance, voire acceptation
du risque).
* **L'analyse d'impact** consiste à évaluer quel est l'impact d'un risque qui se matérialise et à déterminer à partir de quand cet
impact est intolérable, généralement parce qu'il met en danger les processus essentiels (donc, la survie) de l'entreprise.L'analyse
d'impact se fait sur la base de désastres : on considère des désastres extrêmes voire improbables (par exemple, la destruction
totale du bâtiment) et on détermine les impacts financiers, humains, légaux, etc., pour des durées d'interruption de plus en plus
longues jusqu'à ce qu'on atteigne l'impact maximal tolérable. Le résultat principal de l'analyse d'impact est donc une donnée
temporelle : c'est la durée maximale admissible d'une interruption de chaque processus de l'entreprise. En tenant compte des
ressources informatiques (réseaux, serveurs, PCs, etc.) dont chaque processus dépend, on peut déduire le temps maximal
d'indisponibilité de chacune de ces ressources, en d'autres termes, le temps maximal après lequel une ressource informatique doit
avoir été remise en fonction.

Une analyse de risque réussie est le résultat d'une action collective impliquant tous les acteurs du système d'information :
techniciens, utilisateurs et managers.

### Plan de reprise d'activité

Un plan de reprise d'activité (PRA) constitue l'ensemble des procédures documentées lui permettant de rétablir et de reprendre ses
activités en s'appuyant sur des mesures temporaires adoptées pour répondre aux exigeces métier habituelles après un incident.

Le plan de reprise d'activité comprend les tâche suivantes :

* Identification des activités critiques ;
* Identification des ressources ;
* Identification des solutions pour le maintien des activités critiques.
