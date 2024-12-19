## Diagramme des cas d’utilisation

```mermaid
flowchart TB
    subgraph Acteurs
        ConducteurStandard["Conducteur standard"]
        ConducteurAbonne["Conducteur abonné"]
        Operateur["Opérateur du parking"]
        Technicien["Technicien de maintenance"]
    end

    subgraph Cas_d_Utilisation
        UC1["Entrée dans le parking"]
        UC2["Paiement aux automates internes"]
        UC3["Paiement à la sortie"]
        UC4["Gestion du parc et administration"]
        UC5["Maintenance et supervision"]
    end

    ConducteurStandard --> UC1
    ConducteurStandard --> UC2
    ConducteurStandard --> UC3

    ConducteurAbonne --> UC1
    ConducteurAbonne --> UC3

    Operateur --> UC4
    Technicien --> UC5

```
## Diagrammes de séquence

### Entrée dans le parking

```mermaid
sequenceDiagram
    participant Conducteur
    participant Barrière
    participant Système
    Conducteur->>Barrière: Appuie sur le bouton
    Barrière->>Système: Demande de ticket
    Système-->>Barrière: Délivre un ticket horodaté
    Barrière-->>Conducteur: Ouvre la barrière
```
- Objectif : Permettre à un véhicule d’entrer.
- Description : Le conducteur s’arrête devant la barrière d’entrée, appuie sur un bouton (ou capteur de présence), le système délivre un ticket horodaté et ouvre la barrière. Le ticket indique la date, l’heure d’entrée et un numéro unique.
- Exceptions : Barrière défaillante, imprimante de tickets en panne, parking complet (dans ce dernier cas, la barrière reste fermée).


### Paiement aux automates internes

```mermaid
sequenceDiagram
    participant Conducteur
    participant Automate
    participant Système
    Conducteur->>Automate: Insère le ticket
    Automate->>Système: Demande de calcul du montant dû
    Système-->>Automate: Montant dû
    Conducteur->>Automate: Paie (espèces ou carte)
    Automate->>Système: Met à jour le ticket comme "payé"
    Automate-->>Conducteur: Ticket validé
```

- Objectif : Permettre au conducteur de régler son stationnement avant de sortir.
- Description : L’usager insère son ticket dans l’automate. Le système calcule le montant dû. L’usager paie en espèces ou par carte. L’automate met à jour le ticket comme “payé”.
- Exceptions : Automate en panne, carte bancaire refusée, paiement incomplet.

### Paiement à la sortie

```mermaid
sequenceDiagram
    participant Conducteur
    participant Barrière
    participant Système
    Conducteur->>Barrière: Insère le ticket
    Barrière->>Système: Vérifie le paiement
    alt Ticket non payé
        Système-->>Barrière: Propose paiement par carte
        Conducteur->>Barrière: Paie par carte
        Système-->>Barrière: Valide le paiement
    end
    Barrière-->>Conducteur: Ouvre la barrière
```

- Objectif : Permettre le règlement et la sortie du véhicule depuis la barrière de sortie.
- Description :
Cas standard (ticket) : Le conducteur insère son ticket. Si non payé, le système propose un paiement par carte bancaire. Une fois payé, la barrière s’ouvre.
- Cas télépéage : Le véhicule est détecté, le badge est lu, le compte du conducteur est débité, la barrière s’ouvre sans action supplémentaire.
Exceptions : Carte refusée, badge non valide, barrière en panne.

### Gestion du parc et administration

```mermaid
sequenceDiagram
    participant Operateur
    participant InterfaceAdmin
    participant Système
    Operateur->>InterfaceAdmin: Se connecte
    InterfaceAdmin->>Système: Demande de modification des tarifs
    Système-->>InterfaceAdmin: Confirme la modification
    InterfaceAdmin-->>Operateur: Modification réussie
```

- Objectif : Permettre à l’opérateur d’accéder aux informations de fonctionnement du parking.
- Description : L’opérateur se connecte à une interface d’administration pour :
Modifier les tarifs (par heure, forfait, réduction, etc.)
Consulter le nombre de places occupées et disponibles
Suivre les transactions journalières et mensuelles, extraire des statistiques
Surveiller l’état des barrières, des automates, et initier des demandes de maintenance.

### Maintenance et supervision

```mermaid
sequenceDiagram
    participant Technicien
    participant Système
    participant Equipement
    Technicien->>Système: Consulte l'état des équipements
    Système-->>Technicien: Affiche l'état des équipements
    alt Equipement en panne
        Système-->>Technicien: Envoie une alerte
        Technicien->>Equipement: Diagnostique le problème
        Technicien->>Système: Met l'équipement hors service
        Technicien->>Equipement: Remplace ou répare le composant
        Technicien->>Système: Réinitialise l'équipement
        Système-->>Technicien: Confirme la remise en service
    end
```
- Objectif : Permettre au technicien de diagnostiquer et de résoudre les problèmes.
- Description : Le technicien consulte l’état des équipements (barrières, automates, lecteurs de badges) et reçoit des alertes en cas de dysfonctionnement. Il peut mettre un automate hors service, remplacer des composants, réinitialiser la barrière, etc.


### Liste de contraintes et règles métiers

- Tarif : Calcul du tarif en fonction de la durée du stationnement et éventuellement d’une grille tarifaire (première heure gratuite, forfait soirée, etc.).
- Temps maximal de stationnement : Définir une durée maximale de stationnement.
- Conditions particulières : Gestion des tickets perdus, pannes des automates, barrières bloquées, etc.
- Disponibilité : Le système doit être opérationnel 24h/24, 7j/7.
- Performance : Le temps d’attente pour la délivrance d’un ticket ou l’ouverture d’une barrière doit être minimal (moins de 1 seconde).
- Sécurité : Les transactions par carte bancaire doivent être sécurisées, les données des badges de télépéage protégées.
- Capacité : Le système doit gérer au moins 1000 transactions par jour, avec un pic possible à 200 véhicules par heure le matin et en fin d’après-midi.
- Évolutivité : Possibilité d’ajouter de nouveaux modes de paiement, de nouveaux tarifs, ou de nouveaux types de badges.