# Application de santé

Ce projet est une application pour les médecins. Elle leur permet de suivre les données de leurs patients, que ceux-ci renseignent de manière périodique pour assurer un suivi en temps réel, de discuter entre eux et de recevoir des alertes lorsqu’une valeur semble anormale. Le système utilise aussi l’intelligence artificielle pour analyser les informations et repérer les problèmes plus rapidement.

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

### 📊 Diagramme de flux de l'application
*(Ce graphique s'affichera automatiquement sur GitHub grâce à Mermaid.js)*

```mermaid
graph TD
    %% Définition des acteurs
    P((👤 Patient))
    M((👨‍⚕️ Médecin))

    %% Modules principaux
    subgraph Core System
        User[🛡️ Module User\nAuth & Profils]
        HR[📂 Module HealthRecord\nSaisie & Stockage]
        Ana[🧠 Module Analytics\nIA & Prédiction]
        Dash[📊 Module Dashboard\nInterfaces & PDF]
        Msg[💬 Module Messaging\nChat Sécurisé]
        Notif[🔔 Module Notification\nAlertes Push]
    end

    %% Interactions Patient
    P -->|Se connecte| User
    P -->|Saisit ses données| HR
    P -->|Consulte ses stats| Dash
    P <-->|Discute avec le médecin| Msg

    %% Interactions Système
    HR -->|Analyse continue| Ana
    Ana -->|Détecte une anomalie| Notif
    Ana -->|Alimente| Dash

    %% Interactions Médecin
    M -->|Se connecte| User
    M -->|Supervise via le TdB| Dash
    M <-->|Conseille le patient| Msg
    Notif -.->|Alerte d'urgence| M

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
 
