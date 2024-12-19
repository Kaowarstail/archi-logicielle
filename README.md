# archi-logicielle

## Contexte du projet

Un parc de stationnement pour voitures souhaite mettre en place un système informatisé de contrôle d’accès et de paiement.

Les automobilistes entrent dans le parking via une barrière d’entrée qui délivre automatiquement un ticket. Les usagers peuvent ensuite stationner leur véhicule le temps désiré.

Pour le paiement, plusieurs options sont disponibles :

- Régler leur stationnement aux automates de paiement situés à l’intérieur du parc, avant de sortir.
- Régler directement à la sortie en utilisant une carte bancaire.
- Utiliser un badge de télépéage leur permettant de sortir sans action supplémentaire, le paiement étant associé automatiquement à leur compte.

Le système doit permettre la gestion de la capacité du parking, la tarification du temps de stationnement, la supervision de l’état des barrières et des équipements, ainsi que la mise à disposition de statistiques d’utilisation.

## Objectifs

L’objectif principal est que vous réalisiez la modélisation fonctionnelle du système. Cette modélisation doit permettre :

- D’identifier les principaux acteurs (usagers, personnel, système de gestion, etc.)
- De décrire les cas d’utilisation fonctionnels (entrée, sortie, paiement, contrôle, maintenance)
- De proposer une architecture logique (schémas de classes, diagrammes de séquences, machines à états, etc.)
- D’anticiper les contraintes techniques (capacité, sécurité, temps de réponse) et les éléments de suivi et de supervision.

## Portée du système

### Fonctionnalités incluses

- Gestion des entrées : délivrance d’un ticket horodaté et ouverture de la barrière.
- Gestion des sorties : vérification du paiement, ouverture de la barrière.
- Modes de paiement :
  - Automate interne : Le conducteur paie avant de rejoindre sa voiture. Le ticket est validé et permet la sortie.
  - Paiement par carte bancaire au point de sortie : Le conducteur insère son ticket, la somme due est calculée, il paie par carte, puis la barrière s’ouvre.
  - Badge de télépéage : Lecture sans contact, le système identifie le compte client et débite automatiquement.
- Calcul du tarif : en fonction de la durée du stationnement et éventuellement d’une grille tarifaire (première heure gratuite, forfait soirée, etc.).
- Gestion des pannes et maintenance : signalement d’un automate en panne, barrière bloquée, etc.
- Administration du système : consultation des statistiques (taux d’occupation, durée moyenne de stationnement, recettes journalières), gestion des tarifs, gestion des clients abonnés (badges de télépéage).

### Fonctionnalités hors du périmètre

- Gestion physique de l’éclairage, de la vidéosurveillance, ou autres systèmes non liés directement au paiement et au contrôle d’accès.
- Système de réservation de places en ligne.
- Gestion en détail des aspects fiscaux ou comptables (uniquement l’enregistrement des transactions est requis).

## Acteurs du système

- Conducteur standard (usager ponctuel) : prend un ticket à l’entrée, paie avant la sortie via automate ou sortie carte bancaire.
- Conducteur abonné (télépéage) : possède un badge permettant la facturation automatique.
- Opérateur du parking (administrateur) : consulte les statistiques, modifie les tarifs, gère la maintenance et le support.
- Technicien de maintenance : intervient sur les automates, les barrières, consulte l’état du système.

## Cas d’utilisation principaux

### Entrée dans le parking

- Objectif : Permettre à un véhicule d’entrer.
- Description : Le conducteur s’arrête devant la barrière d’entrée, appuie sur un bouton (ou capteur de présence), le système délivre un ticket horodaté et ouvre la barrière. Le ticket indique la date, l’heure d’entrée et un numéro unique.
- Exceptions : Barrière défaillante, imprimante de tickets en panne, parking complet (dans ce dernier cas, la barrière reste fermée).

### Paiement aux automates internes

- Objectif : Permettre au conducteur de régler son stationnement avant de sortir.
- Description : L’usager insère son ticket dans l’automate. Le système calcule le montant dû. L’usager paie en espèces ou par carte. L’automate met à jour le ticket comme “payé”.
- Exceptions : Automate en panne, carte bancaire refusée, paiement incomplet.

### Paiement à la sortie

- Objectif : Permettre le règlement et la sortie du véhicule depuis la barrière de sortie.
- Description :
  - Cas standard (ticket) : Le conducteur insère son ticket. Si non payé, le système propose un paiement par carte bancaire. Une fois payé, la barrière s’ouvre.
  - Cas télépéage : Le véhicule est détecté, le badge est lu, le compte du conducteur est débité, la barrière s’ouvre sans action supplémentaire.
- Exceptions : Carte refusée, badge non valide, barrière en panne.

### Gestion du parc et administration

- Objectif : Permettre à l’opérateur d’accéder aux informations de fonctionnement du parking.
- Description : L’opérateur se connecte à une interface d’administration pour :
  - Modifier les tarifs (par heure, forfait, réduction, etc.)
  - Consulter le nombre de places occupées et disponibles
  - Suivre les transactions journalières et mensuelles, extraire des statistiques
  - Surveiller l’état des barrières, des automates, et initier des demandes de maintenance.

### Maintenance et supervision

- Objectif : Permettre au technicien de diagnostiquer et de résoudre les problèmes.
- Description : Le technicien consulte l’état des équipements (barrières, automates, lecteurs de badges) et reçoit des alertes en cas de dysfonctionnement. Il peut mettre un automate hors service, remplacer des composants, réinitialiser la barrière, etc.

## Contraintes et exigences

- Disponibilité : Le système doit être opérationnel 24h/24, 7j/7.
- Performance : Le temps d’attente pour la délivrance d’un ticket ou l’ouverture d’une barrière doit être minimal (moins de 1 secondes).
- Sécurité : Les transactions par carte bancaire doivent être sécurisées, les données des badges de télépéage protégées.
- Capacité : Le système doit gérer au moins 1000 transactions par jour, avec un pic possible à 200 véhicules par heure le matin et en fin d’après-midi.
- Évolutivité : Possibilité d’ajouter de nouveaux modes de paiement, de nouveaux tarifs, ou de nouveaux types de badges.

## Livrables attendus

- Diagramme des cas d’utilisation : Acteurs, cas d’utilisation, relations.
- Diagrammes de séquence (ou d’activités) : Illustrer les scénarios principaux (entrée, paiement, sortie, gestion des tarifs).
- Description textuelle des scénarios : Pour clarifier le comportement attendu.
- Liste de contraintes et règles métiers : Tarif, temps maximal de stationnement, conditions particulières (par exemple, tickets perdus).

- Bonus : Eventuelles propositions d’amélioration ou variantes : Intégration future de nouveaux moyens de paiement, gestion dynamique des tarifs, réservations, etc.

## Rappels : Différences entre cas d’utilisation et de séquence

Un diagramme de cas d’utilisation et un diagramme de séquence appartiennent tous deux au langage UML, mais ils répondent à des objectifs et des niveaux de détail différents :

### Diagramme de cas d’utilisation

- Objectif principal : Identifier et décrire les fonctionnalités du système du point de vue des utilisateurs (acteurs).
- Vision globale : Il donne une vue d’ensemble du système, en listant les grandes interactions entre des acteurs (humains ou externes) et le système.
- Contenu : Le diagramme de cas d’utilisation présente des acteurs (externes au système) et des cas d’utilisation (ce que fait le système pour répondre à leurs besoins).
- Niveau d’abstraction : Plutôt élevé. Il ne décrit pas le détail des échanges, mais indique quelles sont les fonctions principales accessibles et qui y a accès.
  En somme, le diagramme de cas d’utilisation répond à la question « Quelles sont les fonctionnalités offertes par le système et qui les utilise ? »

### Diagramme de séquence

- Objectif principal : Décrire le déroulement précis d’un scénario, l’enchaînement des messages échangés entre les objets ou composants du système.
- Vision détaillée : Il se focalise sur la dynamique interne, montrant comment différents éléments internes du système interagissent pour réaliser un cas d’utilisation particulier ou une partie d’un cas d’utilisation.
- Contenu : Le diagramme de séquence présente des objets disposés sur un axe horizontal, et leurs interactions temporelles (sous forme de messages) sur un axe vertical.
- Niveau de détail : Plus concret. Il montre dans quel ordre les messages sont échangés, quelles méthodes sont appelées, et les conditions pour le déroulement d’un scénario.
  Le diagramme de séquence répond à la question « Comment les éléments du système collaborent entre eux, étape par étape, pour réaliser une fonctionnalité ? »

En résumé :

- Le diagramme de cas d’utilisation décrit ce que le système doit faire et qui interagit avec lui, sans détailler comment les opérations s’enchaînent.
- Le diagramme de séquence décrit comment un scénario s’exécute au sein du système, en montrant l’enchaînement des interactions entre les différents éléments internes.

## Critères d’évaluation

- Cohérence fonctionnelle : Le modèle répond-il à tous les besoins décrits ?
- Clarté et complétude : Le périmètre est-il clairement défini, tous les cas d’utilisation sont-ils couverts ?
- Qualité de la modélisation : Bonne utilisation des diagrammes UML (ou autre notation), bonne structure du modèle, lisibilité.
- Prise en compte des contraintes : Disponibilité, sécurité, performance.
- Adaptabilité et évolutivité : La solution est-elle modulaire et extensible ?
