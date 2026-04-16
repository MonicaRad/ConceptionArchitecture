# 🩺 CareConnect : L'IA au service du suivi médical

![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)
![Statut](https://img.shields.io/badge/statut-en_développement-brightgreen.svg)
![Licence](https://img.shields.io/badge/licence-MIT-green.svg)

> **CareConnect** est une plateforme de santé connectée de nouvelle génération conçue pour rapprocher médecins et patients. Grâce à un suivi des données en temps réel, une messagerie sécurisée et une analyse prédictive propulsée par l'IA, anticipez les anomalies et optimisez la prise en charge médicale.

---

## ✨ Fonctionnalités Clés

Notre application offre un écosystème complet pour la gestion de la santé :

- 🤖 **Intelligence Artificielle & Prédiction** : Analyse des historiques pour détecter les tendances et repérer les problèmes plus rapidement.
- 🚨 **Alertes en Temps Réel** : Notifications push immédiates lorsqu'une constante vitale semble anormale.
- 📊 **Tableaux de Bord Personnalisés** : Interfaces dédiées et adaptées aux besoins des médecins et des patients.
- 🔒 **Confidentialité Maximale** : Stockage sécurisé des données de santé et messagerie chiffrée.
- 📄 **Génération de Rapports** : Exportation des historiques et des diagnostics au format PDF.

---

## 🏗️ Architecture des Modules

L'application est découpée en micro-services / modules logiques pour assurer sa scalabilité et sa maintenabilité. 



Voici la liste de fonctionnalités de notre application : 


- Création / modification de comptes 
- Authentification admin/user
- Saisie manuelle de données de santé
- Système de filtrage par maladie
- Stockage sécurisé des données patient
- Analyse prédictive des historiques
- Alertes push en cas d'anomalie
- Tableau de bord médecin
- Tableau de bord patient
- Messagerie sécurisée
- Export PDF des rapports médicaux

Les fonctionnalités sont ensuite regroupées par domaine : 

| Module | Fonctionnalités incluses |
|---|---|
| User | Création / modification de comptes, authentification admin/user |
| HealthRecord | Saisie manuelle de données de santé, stockage sécurisé des données patient, filtrage par maladie |
| Analytics | Analyse prédictive des historiques |
| Dashboard | Tableau de bord médecin, tableau de bord patient, export PDF des rapports médicaux |
| Messaging | Messagerie sécurisée |
| Notification | Alertes push en cas d'anomalie |

```mermaid
graph TD
    %% --- Styles personnalisés pour rendre le schéma plus esthétique sur GitHub ---
    
    %% Définit les styles pour les acteurs (Utilisateurs)
    classDef actor fill:#f3f4f6,stroke:#374151,stroke-width:2px,color:#111827;
    
    %% Définit les styles pour les modules du système
    classDef module fill:#e0f2fe,stroke:#0284c7,stroke-width:2px,color:#0c4a6e;
    
    %% Définit les styles pour le stockage des données
    classDef storage fill:#dcfce7,stroke:#16a34a,stroke-width:2px,color:#14532d;
    
    %% Définit les styles pour les alertes (police grasse)
    classDef alert fill:#fee2e2,stroke:#dc2626,stroke-width:2px,color:#7f1d1d,font-weight:bold;

    %% --- Acteurs (Utilisateurs) ---
    Patient((👤 Patient)):::actor
    Medecin((👨‍⚕️ Médecin)):::actor

    %% --- Modules Système ---
    Auth[🔐 Auth & Profils<br/>(Module User)]:::module
    Saisie[[📝 Saisie & Filtrage<br/>(Module HealthRecord)]]: : module
    DB[(🗄️ Stockage Patient Sécurisé)]:::storage
    IA[🧠 Analyse Prédictive<br/>(Module Analytics)]:::module
    Dash[📊 Tableaux de bord & PDF<br/>(Module Dashboard)]:::module
    Msg[💬 Messagerie Sécurisée<br/>(Module Messaging)]:::module
    Notif[🔔 Alertes Push<br/>(Module Notification)]:::alert

    %% --- Actions du Patient ---
    %% 1. Connexion initiale
    Patient -->|1. Se connecte| Auth
    
    %% 2. Saisie des données de santé
    Patient -->|2. Renseigne ses constantes| Saisie
    
    %% Autres actions
    Patient -->|Consulte son suivi| Dash
    Patient <-->|Discute de son état| Msg

    %% --- Traitement des données en arrière-plan ---
    %% Sauvegarde sécurisée des données saisies
    Saisie -->|Sauvegarde chiffrée| DB
    
    %% L'IA utilise l'historique pour l'analyse
    DB -->|Fournit l'historique| IA
    
    %% L'IA déclenche une alerte si une anomalie est détectée
    IA -->|Détecte une anomalie| Notif
    
    %% Alimentation des tableaux de bord par les données et les prédictions
    DB -->|Alimente les graphiques| Dash
    IA -->|Ajoute les prédictions| Dash

    %% --- Actions du Médecin ---
    %% 1. Connexion initiale
    Medecin -->|1. Se connecte| Auth
    
    %% Reçoit des alertes immédiates en cas de danger
    Notif -.->|Alerte d'urgence| Medecin
    
    %% Consulte le tableau de bord
    Medecin -->|Supervise l'évolution| Dash
    
    %% Communique avec le patient
    Medecin <-->|Conseille le patient| Msg
    Medecin -->|Supervise l'évolution| Dash
    Medecin <-->|Conseille et rassure| Msg
 
