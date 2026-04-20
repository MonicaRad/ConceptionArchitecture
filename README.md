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

L'application est découpée en domaine métier pour assurer sa scalabilité et sa maintenabilité. 

Les fonctionnalités sont ensuite regroupées par domaine : 

| Module | Fonctionnalités incluses |
|---|---|
| User | Création / modification de comptes, authentification admin/user |
| Record | Saisie manuelle de données de santé, stockage sécurisé des données patient, filtrage par maladie |
| Analytics | Analyse prédictive des historiques |
| Messaging | Messagerie sécurisée |
| Notification | Alertes push en cas d'anomalie |

---
```mermaid
graph LR
    %% --- Styles ---
    classDef actor fill:#ffc213,stroke:#374151,stroke-width:2px,color:#111827;
    classDef module fill:#e0f2fe,stroke:#0284c7,stroke-width:2px,color:#0c4a6e;
    classDef storage fill:#dcfce7,stroke:#16a34a,stroke-width:2px,color:#14532d;
    classDef alert fill:#fee2e2,stroke:#dc2626,stroke-width:2px,color:#7f1d1d,font-weight:bold;

    %% --- 1. Origine (Verrouillés à gauche) ---
    subgraph "Acteurs (Entrée)"
        direction TB
        Patient(("Patient")):::actor
        Medecin(("Médecin")):::actor

        %% Maintient le Patient en premier
        Patient ~~~ Medecin
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

    subgraph "Service Analytics"
        IA["Analytics AI"]:::module
        DB_IA[("DB Intelligence")]:::storage
    end

    %% --- 3. Fin de chaîne (Verrouillés à droite) ---
    subgraph "Service Alertes"
        Notif["Notifications"]:::alert
        DB_Notif[("DB Logs Alertes")]:::storage
    end

    %% --- ASTUCE ANTI-RETOURNEMENT ---
    %% Ces liens invisibles (~~~) forcent la lecture stricte de gauche à droite !
    Patient ~~~ Auth
    Auth ~~~ Saisie
    Saisie ~~~ IA
    IA ~~~ Notif

    %% --- Flux de données principaux ---
    Patient -->|"Se connecte"| Auth
    Medecin -->|"Se connecte"| Auth
    Auth <--> DB_User

    Patient -->|"Saisie données"| Saisie
    Saisie <--> DB_Health

    DB_Health -->|"Alimente service Analytics"| IA
    IA <--> DB_IA

    Patient <-->|"Échange sécurisé"| Msg
    Medecin <-->|"Conseil médical"| Msg
    Msg <--> DB_Msg

    %% --- Flux d'Alerte (Retour vers les acteurs) ---
    IA -->|"Anomalie détectée"| Notif
    Notif <--> DB_Notif

    %% Syntaxe valide qui ne cassera plus le visuel grâce à l'astuce ci-dessus
    Notif -.->|"Alerte Push"| Patient
    Notif -.->|"Alerte Push"| Medecin
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
 
Les données sont structurées et doivent rester cohérentes entre elles. Pour les messages on a le JSONB pour les stocker en document.

#### Décision
On utilise PostgreSQL.

#### Conséquences
Ce choix permet de gérer facilement les relations entre données et faire des requetes simples/complexes.


### Architecture microservices

en rouge : asynchrone
en bleu : synchrone

<img width="1035" height="653" alt="image" src="https://github.com/user-attachments/assets/4ca8b2dc-4282-471b-876a-14fe8f633424" />


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
Le service analytics manipule des données d’analyse avec des donnees de predictions bien precises

#### Décision
On utilise PostGre.

#### Conséquences
On peut stocker facilement des données et faire des requetes simples ou complexes

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
L'analyse peut prendre 2 secondes. On ne veut pas que l'application du patient "freeze" pendant ce temps. On envoie la donnée dans Kafka, et le service Analytics la traitera dès qu'il est disponible.

#### Décision
On utilise Kafka.

#### Conséquences
Les services communiquent de façon asynchrone.  

### Communication Event-Driven (Kafka)

<img width="1163" height="408" alt="Screenshot from 2026-04-20 09-53-23" src="https://github.com/user-attachments/assets/e579ab08-ad7a-4d94-bd4a-55ceff5c483f" />

La communication entre les services est principalement asynchrone via Apache Kafka. Ce choix permet de découpler les services et de garantir que le système reste réactif même sous forte charge.
* Le Service Record produit un événement sur le topic health-records.
* Le Service IA consomme ce topic, charge un modèle IA et produit une prediction.
* Le Service Alerte consomme la prédiction et, si nécessaire, produit un événement alerts.
* Le Service Messagerie consomme l'alerte et déclenche l'envoi vers l'utilisateur final.
  
