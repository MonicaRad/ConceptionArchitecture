# 🩺 CareConnect : L'IA au service du suivi médical

![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)
![Statut](https://img.shields.io/badge/statut-en_développement-brightgreen.svg)

> **CareConnect** est une plateforme de santé connectée de nouvelle génération conçue pour rapprocher médecins et patients. Grâce à un suivi des données en temps réel, une messagerie sécurisée et une analyse prédictive propulsée par l'IA, anticipez les anomalies et optimisez la prise en charge médicale.

---

## Fonctionnalités clés

Notre application simplifie le suivi médical grâce aux outils suivants :

* **Gestion des comptes** : Création et modification facile des profils.
* **Connexion sécurisée** : Accès séparés et sécurisés pour les médecins et les patients.
* **Suivi de santé** : Renseignement manuel et régulier des constantes médicales par le patient.
* **Filtrage intelligent** : Tri rapide des dossiers et des données selon les maladies.
* **Données protégées** : Stockage privé et hautement sécurisé de toutes les informations médicales.
* **Analyse par IA** : Le système étudie l'historique pour repérer et prévoir les problèmes de santé.
* **Alertes en temps réel** : Notifications directes (Push) dès qu'une donnée médicale est anormale.
* **Espace Médecin** : Un écran dédié pour surveiller facilement tous ses patients.
* **Espace Patient** : Un écran personnel pour suivre sa propre santé au quotidien.
* **Messagerie privée** : Discussion directe et confidentielle entre le patient et son médecin.
* **Rapports PDF** : Exportation et impression des historiques médicaux en un clic.

---

## Architecture des modules

L'application est découpée en micro-services / modules logiques pour assurer sa scalabilité et sa maintenabilité. 

Les fonctionnalités sont ensuite regroupées par domaine : 

| Module | Fonctionnalités incluses |
|---|---|
| User | Création / modification de comptes, authentification admin/user |
| HealthRecord | Saisie manuelle de données de santé, stockage sécurisé des données patient, filtrage par maladie |
| Analytics | Analyse prédictive des historiques |
| Messaging | Messagerie sécurisée |
| Notification | Alertes push en cas d'anomalie |

---

```mermaid
graph LR
    %% --- Styles ---
    classDef actor fill:#f3f4f6,stroke:#374151,stroke-width:2px,color:#111827;
    classDef module fill:#e0f2fe,stroke:#0284c7,stroke-width:2px,color:#0c4a6e;
    classDef storage fill:#dcfce7,stroke:#16a34a,stroke-width:2px,color:#14532d;
    classDef alert fill:#fee2e2,stroke:#dc2626,stroke-width:2px,color:#7f1d1d,font-weight:bold;
    classDef device fill:#fef08a,stroke:#ca8a04,stroke-width:2px,color:#713f12;

    %% --- 1. Origine (Verrouillés à gauche) ---
    subgraph "Acteurs (Entrée)"
        direction TB
        Patient(("Patient")):::actor
        Medecin(("Médecin")):::actor
    end

    %% --- 2. Cœur du Système (Centre) ---
    subgraph "Service Utilisateur"
        Auth["Auth & Profils"]:::module
        DB_User[("DB Users")]:::storage
    end

    subgraph "Service Santé"
        Saisie["Saisie & Filtrage"]:::module
        DB_Health[("DB Records")]:::storage
    end

    subgraph "Service Messages"
        Msg["Messaging"]:::module
        DB_Msg[("DB Chats")]:::storage
    end

    subgraph "Service IA"
        IA["Analytics AI"]:::module
        DB_IA[("DB Intelligence")]:::storage
    end

    %% --- 3. Fin de chaîne (Verrouillés à droite) ---
    subgraph "Service Alertes"
        Notif["Notifications"]:::alert
        DB_Notif[("DB Logs Alertes")]:::storage
    end

    subgraph "Réception (Sortie)"
        direction TB
        AppPatient(("Mobile Patient")):::device
        AppMedecin(("Mobile Médecin")):::device
    end

    %% --- Flux de données principaux ---
    Patient -->|"Se connecte"| Auth
    Medecin -->|"Se connecte"| Auth
    Auth <--> DB_User

    Patient -->|"Saisie données"| Saisie
    Saisie <--> DB_Health

    DB_Health -->|"Alimente l'IA"| IA
    IA <--> DB_IA

    Patient <-->|"Échange sécurisé"| Msg
    Medecin <-->|"Conseil médical"| Msg
    Msg <--> DB_Msg

    %% --- Flux d'Alerte (Vers la droite) ---
    IA -->|"Anomalie détectée"| Notif
    Notif <--> DB_Notif

    %% Les alertes finissent leur course à droite
    Notif -.->|"Alerte Push"| AppPatient
    Notif -.->|"Alerte Push"| AppMedecin
    %% Les alertes finissent leur course à droite
    Notif -.->|"Alerte Push"| AppPatient
    Notif -.->|"Alerte Push"| AppMedecin
```

## Diagramme de classe 

<img width="1103" height="459" alt="image" src="https://github.com/user-attachments/assets/ef8aa94e-96ab-4789-bc35-74336364a21d" />


## Architecture des modules

### Architecture n-tiers

<img width="904" height="780" alt="image" src="https://github.com/user-attachments/assets/e7df02fa-6dd1-4c43-8198-7d1f1aa2c2a2" />

#### Architecture Decision Records

#### ADR-001 - Choix de PostgreSQL
**Statut :** Accepté

#### Contexte
 
Les données sont structurées et doivent rester cohérentes entre elles.

#### Décision
On utilise PostgreSQL.

#### Conséquences
Ce choix permet de gérer facilement les relations entre données et d’assurer des transactions fiables.  


### Architecture microservices

en rouge : asynchrone
en bleu : synchrone

<img width="1162" height="560" alt="image" src="https://github.com/user-attachments/assets/7137cc31-056a-40d0-addb-f51deba52d5a" />

#### Architecture Decision Records

#### ADR-001 - Base de données du service utilisateur
**Statut :** Accepté

#### Contexte
Le service utilisateur gère des données structurées comme les noms, les mails qui doivent avoir une structure précise

#### Décision
On utilise PostgreSQL.

#### Conséquences
Les données sont bien organisées et toujours sous le bon format

---

#### ADR-002 - Base de données du service record
**Statut :** Accepté

#### Contexte
Le service record gère des données structurées qui doivent rester fiables et simples à consulter. Surtout au niveau des entrees du patient, on doit pouvoir facilement faire des filtrages 

#### Décision
On utilise PostgreSQL.


---

#### ADR-003 - Base de données du service messagerie
**Statut :** Accepté

#### Contexte
Le service messagerie traite des messages qui peuvent avoir des formats différents et des longueurs differentes et meme des pieces jointes

#### Décision
On utilise MongoDB.

#### Conséquences
Le schéma est plus souple et plus simple à faire évoluer.

---

#### ADR-004 - Base de données du service analytics
**Statut :** Accepté

#### Contexte
Le service analytics manipule des données d’analyse qui peuvent changer de structure selon les besoins. 

#### Décision
On utilise MongoDB.

#### Conséquences
On peut stocker facilement des données variées et des résultats intermédiaires.

---
#### ADR-005 - Base de données du service alerte
**Statut :** Accepté

#### Contexte
Le service alerte gère des notifications, des messages differents et des paramètres qui peuvent évoluer.

#### Décision
On utilise MongoDB.

#### Conséquences
Le modèle reste flexible et adapté aux changements.

---
#### ADR-006 - Choix du bus d’événements
**Statut :** Accepté

#### Contexte
Les différents services doivent échanger des événements sans être trop liés entre eux. 

#### Décision
On utilise Kafka.

#### Conséquences
Les services communiquent de façon asynchrone.  


