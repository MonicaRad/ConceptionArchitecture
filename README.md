# 🩺 CareConnect : L'IA au service du suivi médical

![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)
![Statut](https://img.shields.io/badge/statut-en_développement-brightgreen.svg)
![Licence](https://img.shields.io/badge/licence-MIT-green.svg)

> **CareConnect** est une plateforme de santé connectée de nouvelle génération conçue pour rapprocher médecins et patients. Grâce à un suivi des données en temps réel, une messagerie sécurisée et une analyse prédictive propulsée par l'IA, anticipez les anomalies et optimisez la prise en charge médicale.

---

## Fonctionnalités clés

Notre application offre un écosystème complet pour la gestion de la santé :

- **Intelligence Artificielle & Prédiction** : Analyse des historiques pour détecter les tendances et repérer les problèmes plus rapidement.
- **Alertes en Temps Réel** : Notifications push immédiates lorsqu'une constante vitale semble anormale.
- **Tableaux de Bord Personnalisés** : Interfaces dédiées et adaptées aux besoins des médecins et des patients.
- **Confidentialité Maximale** : Stockage sécurisé des données de santé et messagerie chiffrée.
- **Génération de Rapports** : Exportation des historiques et des diagnostics au format PDF.

---

## Architecture des modules

L'application est découpée en micro-services / modules logiques pour assurer sa scalabilité et sa maintenabilité. 

Les fonctionnalités sont ensuite regroupées par domaine : 

| Module | Fonctionnalités incluses |
|---|---|
| User | Création / modification de comptes, authentification admin/user |
| HealthRecord | Saisie manuelle de données de santé, stockage sécurisé des données patient, filtrage par maladie |
| Analytics | Analyse prédictive des historiques |
| Dashboard | Tableau de bord médecin, tableau de bord patient, export PDF des rapports médicaux |
| Messaging | Messagerie sécurisée |
| Notification | Alertes push en cas d'anomalie |


graph TD
    %% --- Styles ---
    classDef actor fill:#f3f4f6,stroke:#374151,stroke-width:2px,color:#111827;
    classDef module fill:#e0f2fe,stroke:#0284c7,stroke-width:2px,color:#0c4a6e;
    classDef storage fill:#dcfce7,stroke:#16a34a,stroke-width:2px,color:#14532d;
    classDef alert fill:#fee2e2,stroke:#dc2626,stroke-width:2px,color:#7f1d1d,font-weight:bold;

    %% --- Acteurs en haut ---
    Patient(("👤 Patient")):::actor
    Medecin(("👨‍⚕️ Médecin")):::actor

    %% --- Services & Bases de Données (Architecture Microservices) ---
    
    subgraph "Service Utilisateur"
        Auth["🔐 Auth & Profils"]:::module
        DB_User[("🗄️ DB Users")]:::storage
    end

    subgraph "Service Santé"
        Saisie["📝 Saisie & Filtrage"]:::module
        DB_Health[("🗄️ DB Records")]:::storage
    end

    subgraph "Service IA"
        IA["🧠 Analytics AI"]:::module
        DB_IA[("🗄️ DB Intelligence")]:::storage
    end

    subgraph "Service Messages"
        Msg["💬 Messaging"]:::module
        DB_Msg[("🗄️ DB Chats")]:::storage
    end

    subgraph "Service Alertes"
        Notif["🔔 Notifications"]:::alert
        DB_Notif[("🗄️ DB Logs Alertes")]:::storage
    end

    %% --- Flux de données ---

    %% Connexion
    Patient & Medecin -->|"Se connectent"| Auth
    Auth <--> DB_User

    %% Saisie et Analyse
    Patient -->|"Saisie données"| Saisie
    Saisie <--> DB_Health
    DB_Health -->|"Alimenter le module Analytics"| IA
    IA <--> DB_IA

    %% Alertes (Double notification)
    IA -->|"Anomalie détectée"| Notif
    Notif <--> DB_Notif
    Notif -.->|"Alerte d'urgence"| Patient
    Notif -.->|"Alerte d'urgence"| Medecin

    %% Communication
    Patient <-->|"Échange sécurisé"| Msg
    Medecin <-->|"Conseil médical"| Msg
    Msg <--> DB_Msg
