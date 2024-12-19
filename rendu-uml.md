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

### Diagramme : Entrée dans le parking

```mermaid
flowchart TB
    %% Acteurs impliqués
    subgraph Acteurs
        Conducteur["Conducteur"]
        SystemeGestion["Système"]
        Technicien["Technicien"]
    end

    %% Cas principal : Entrée normale
    subgraph Cas_Principal
        CP1["Arrêt barrière"]
        CP2["Capteur détecte"]
        CP3["Demande ticket"]
        CP4["Ticket délivré"]
        CP5["Barrière ouverte"]
        CP6["Entrée parking"]
    end

    Conducteur --> CP1
    CP1 --> CP2
    CP2 --> CP3
    CP3 --> CP4
    CP4 --> CP5
    CP5 --> CP6
    SystemeGestion --> CP4
    SystemeGestion --> CP5

    %% Cas alternatifs
    subgraph Cas_Alternatifs
        CA1["Parking complet"]
        CA2["Imprimante HS"]
        CA3["Barrière HS"]
        CA4["Capteur HS"]
    end

    %% Parking complet
    CP1 --> CA1
    CA1 --> CA1_1["Panneau complet"]
    CA1_1 --> CA1_2["Demande ticket"]
    CA1_2 --> CA1_3["Ticket délivré"]
    CA1_3 --> CP5

    %% Panne de l'imprimante
    CP3 --> CA2
    CA2 --> CA2_1["Imprimante HS détectée"]
    CA2_1 --> CA2_2["Message panne"]
    CA2_2 --> CA2_3["Technicien répare"]
    CA2_3 --> CA2_4["Autre barrière ou attente"]
    Technicien --> CA2_3

    %% Barrière défaillante
    CP5 --> CA3
    CA3 --> CA3_1["Barrière ne s'ouvre pas"]
    CA3_1 --> CA3_2["Alerte technicien"]
    CA3_2 --> CA3_3["Redirection ou attente"]
    Technicien --> CA3_2

    %% Capteur défaillant
    CP1 --> CA4
    CA4 --> CA4_1["Capteur HS"]
    CA4_1 --> CA4_2["Demande manuelle"]
    CA4_2 --> CA4_3["Assistance technicien"]
    SystemeGestion --> CA4_3
    Technicien --> CA4_3

    %% Résultat attendu
    subgraph Résultat
        R1["Entrée valide"]
        R2["Entrée valide"]
    end

    CP6 --> R1
    CA1_3 --> R2
    CA2_4 --> R2
    CA3_3 --> R2
    CA4_3 --> R2

```

### Diagramme : Paiement aux automates internes

```mermaid
flowchart TB
    %% Acteurs impliqués
    subgraph Acteurs
        Conducteur["Conducteur"]
        SystemeGestion["Système"]
    end

    %% Cas principal : Paiement normal
    subgraph Cas_Principal
        CP1["Insère ticket"]
        CP2["Calcule durée et montant"]
        CP3["Choix paiement"]
        CP4["Validation paiement"]
        CP5["Ticket marqué payé"]
        CP6["Récupère ticket"]
    end

    Conducteur --> CP1
    CP1 --> CP2
    CP2 --> CP3
    CP3 --> CP4
    CP4 --> CP5
    CP5 --> CP6
    SystemeGestion --> CP2
    SystemeGestion --> CP4
    SystemeGestion --> CP5

    %% Cas alternatifs
    subgraph Cas_Alternatifs
        CA1["Automate en panne"]
        CA2["Carte refusée"]
        CA3["Paiement incomplet"]
    end

    %% Automate en panne
    CP1 --> CA1
    CA1 --> CA1_1["Message 'Automate indisponible'"]
    CA1_1 --> CA1_2["Redirigé vers autre automate"]
    SystemeGestion --> CA1_1

    %% Carte refusée
    CP3 --> CA2
    CA2 --> CA2_1["Carte refusée détectée"]
    CA2_1 --> CA2_2["Message 'Autre mode paiement'"]
    CA2_2 --> CA2_3["Paie espèces ou nouvelle carte"]
    CA2_3 --> CP4
    SystemeGestion --> CA2_1

    %% Paiement incomplet
    CP4 --> CA3
    CA3 --> CA3_1["Montant incorrect"]
    CA3_1 --> CA3_2["Demande montant restant"]
    CA3_2 --> CA3_3["Paiement complété"]
    CA3_3 --> CP5
    SystemeGestion --> CA3_1

    %% Résultat attendu
    subgraph Résultat
        R1["Ticket payé"]
        R2["Dirigé vers solution alternative"]
    end

    CP6 --> R1
    CA1_2 --> R2
    CA2_3 --> R2
    CA3_3 --> R1

```

### Diagramme : Paiement à la sortie

```mermaid
flowchart TB
    %% Acteurs impliqués
    subgraph Acteurs
        Conducteur["Conducteur"]
        SystemeGestion["Système"]
        Operateur["Opérateur"]
        Technicien["Technicien"]
    end

    %% Cas principal : Paiement à la borne avec un ticket
    subgraph Cas_Principal_Ticket
        CP1["Insère ticket"]
        CP2["Ticket vérifié"]
        CP3["Ticket payé ?"]
        CP4["Paiement carte"]
        CP5["Validation paiement"]
        CP6["Barrière ouverte"]
    end

    Conducteur --> CP1
    CP1 --> CP2
    CP2 --> CP3
    CP3 -- "Oui" --> CP6
    CP3 -- "Non" --> CP4
    CP4 --> CP5
    CP5 --> CP6
    SystemeGestion --> CP2
    SystemeGestion --> CP3
    SystemeGestion --> CP5

    %% Cas alternatif : Système de scan défaillant
    subgraph Cas_Scan_Defaillant
        CA1["Ticket non scanné"]
        CA2["Contact interphone"]
        CA3["Tarif calculé manuellement"]
        CA4["Paiement réalisé"]
    end

    CP2 --> CA1
    CA1 --> CA2
    CA2 --> Operateur
    Operateur --> Technicien
    Operateur --> CA3
    CA3 --> CA4
    CA4 --> CP6

    %% Cas alternatif : Carte bancaire refusée
    subgraph Cas_Carte_Refusee
        CB1["Carte refusée"]
        CB2["Message autre paiement"]
        CB3["Contact interphone"]
        CB4["Informations enregistrées"]
    end

    CP4 --> CB1
    CB1 --> CB2
    CB2 --> CB3
    CB3 --> Operateur
    CB4 --> CP6
    Operateur --> CB4

    %% Cas alternatif : Ticket perdu
    subgraph Cas_Ticket_Perdu
        TP1["Ticket perdu"]
        TP2["Contact interphone"]
        TP3["Identification véhicule"]
        TP4["Tarif forfaitaire appliqué"]
    end

    CP1 --> TP1
    TP1 --> TP2
    TP2 --> Operateur
    Operateur --> TP3
    TP3 --> TP4
    TP4 --> CP6

    %% Cas principal : Télépéage
    subgraph Cas_Telepeage
        TT1["Détection véhicule"]
        TT2["Lecture badge"]
        TT3["Compte débité"]
        TT4["Barrière ouverte"]
    end

    Conducteur --> TT1
    TT1 --> TT2
    TT2 --> TT3
    TT3 --> TT4
    SystemeGestion --> TT3

    %% Cas alternatif : Carte refusée au télépéage
    subgraph Cas_Telepeage_Carte_Refusee
        TCR1["Carte refusée"]
        TCR2["Message erreur"]
        TCR3["Contact interphone"]
        TCR4["Informations enregistrées"]
    end

    TT3 --> TCR1
    TCR1 --> TCR2
    TCR2 --> TCR3
    TCR3 --> Operateur
    Operateur --> TCR4
    TCR4 --> TT4

    %% Cas alternatif : Badge non valide
    subgraph Cas_Badge_Invalide
        BI1["Badge invalide"]
        BI2["Message contact opérateur"]
        BI3["Autre mode paiement"]
    end

    TT2 --> BI1
    BI1 --> BI2
    BI2 --> BI3
    BI3 --> TT3

    %% Cas alternatif : Barrière en panne
    subgraph Cas_Barriere_Panne
        BP1["Barrière en panne"]
        BP2["Contact interphone"]
        BP3["Ouverture barrière"]
    end

    TT4 --> BP1
    BP1 --> BP2
    BP2 --> Operateur
    Operateur --> BP3
    BP3 --> TT4

```
### Diagramme : Gestion du parc et administration

```mermaid
flowchart TB
    %% Acteurs impliqués
    subgraph Acteurs
        Operateur["Opérateur"]
        SystemeGestion["Système"]
        EquipeTechnique["Équipe technique"]
    end

    %% Cas principal : Gestion administrative
    subgraph Cas_Principal
        CP1["Connexion sécurisée"]
        CP2["Consulte taux d'occupation"]
        CP3["Consulte transactions"]
        CP4["Modifie paramètres"]
        CP5["Ajoute alertes maintenance"]
    end

    Operateur --> CP1
    CP1 --> CP2
    CP1 --> CP3
    CP1 --> CP4
    CP1 --> CP5
    SystemeGestion --> CP2
    SystemeGestion --> CP3
    SystemeGestion --> CP4
    SystemeGestion --> CP5

    %% Cas alternatif : Système indisponible
    subgraph Cas_Alternatif
        CA1["Système hors ligne"]
        CA2["Alerte envoyée"]
        CA3["Diagnostic par équipe technique"]
    end

    CP1 --> CA1
    CA1 --> CA2
    CA2 --> EquipeTechnique
    EquipeTechnique --> CA3

```


### Diagramme : Maintenance et supervision

```mermaid
flowchart TB
    %% Acteurs impliqués
    subgraph Acteurs
        Technicien["Technicien"]
        SystemeGestion["Système"]
        Utilisateur["Utilisateur"]
    end

    %% Cas principal : Diagnostic et intervention
    subgraph Cas_Principal
        CP1["Consulte alertes"]
        CP2["Identifie équipement défaillant"]
        CP3["Effectue intervention"]
        CP4["Réparation sur place"]
        CP5["Remplacement de pièces"]
        CP6["Réinitialisation du système"]
    end

    Technicien --> CP1
    CP1 --> CP2
    CP2 --> CP3
    CP3 --> CP4
    CP3 --> CP5
    CP3 --> CP6
    SystemeGestion --> CP1
    SystemeGestion --> CP2

    %% Cas alternatif : Alerte non signalée automatiquement
    subgraph Cas_Alternatif
        CA1["Problème signalé manuellement"]
        CA2["Technicien informé"]
        CA3["Procédure de diagnostic"]
    end

    Utilisateur --> CA1
    CA1 --> CA2
    CA2 --> CP1

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