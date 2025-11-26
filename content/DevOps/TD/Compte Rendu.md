---
title: Compte Rendu d'avancement DevOps
draft: false
---

## Table des matières
- [Vue d'ensemble](#vue-densemble)
- [Lab 0 : Configuration de l'environnement](#lab-0--configuration-de-lenvironnement)
- [Lab 1 : Introduction au déploiement d'applications](#lab-1--introduction-au-dploiement-dapplications)
- [Lab 2 : Infrastructure as Code (IaC)](#lab-2--infrastructure-as-code-iac)
- [Lab 3 : Stratégies d'Orchestration](#lab-3--stratégies-dorchestration)
- [Lab 4 : Versioning, Build et Tests Automatisés](#lab-4--versioning-build-et-tests-automatisés)
- [Lab 5 : CI/CD avec Kubernetes](#lab-5--cicd-avec-kubernetes)
- [Lab 6 : Multi-Environnements et Microservices](#lab-6--multi-environnements-et-microservices)
- [Synthèse et prochaines étapes](#synthèse-et-prochaines-étapes)
- [Roadmap prévisionnelle](#roadmap-prévisionnelle)
- [Conclusion](#conclusion)
- [Annexes](#annexes)

## Vue d'ensemble
Ce document résume les travaux pratiques réalisés par notre groupe dans le cadre du cours de DevOps. Nous avons progressé de la mise en place de l'environnement local jusqu'au déploiement d'architectures microservices automatisées sur le cloud (AWS).

## Lab 0 : Configuration de l'environnement
### Objectif
Préparer le poste de travail pour les développements futurs.

### Travaux réalisés
- Installation et configuration des outils essentiels :
  - `Git` pour le versioning
  - `Node.js` et `NPM` pour l'application exemple
- Pour les membres sous Windows : mise en place d'un environnement compatible Unix via `Cygwin` ou `WSL` pour exécuter les scripts Bash fournis.
- Vérification de l'environnement avec les commandes :
  - `node -v`
  - `npm -v`

## Lab 1 : Introduction au déploiement d'applications
### Objectif
Comprendre les bases du déploiement manuel sur un serveur.

### Travaux réalisés
- Développement local :
  - Création et exécution d'une application `Node.js` "Hello World" en local sur le port 8080.
- Déploiement AWS EC2 :
  - Provisionnement manuel d'une instance EC2, configuration des Security Groups, connexion via SSH.
  - Installation de `Node.js` sur le serveur distant et lancement de l'application pour la rendre accessible via l'IP publique.

## Lab 2 : Infrastructure as Code (IaC)
### Objectif
Automatiser la gestion de l'infrastructure pour remplacer les actions manuelles.

### Approches expérimentées
- Scripts Ad Hoc : automation via scripts Bash pour tâches simples.
- Gestion de configuration (`Ansible`) : configuration automatisée des serveurs EC2.
- Templating de serveur (`Packer`) : création d'images machines (AMI) pré-configurées.
- Provisioning (`OpenTofu`) : déploiement, mise à jour et destruction de l'infrastructure de manière déclarative.

## Lab 3 : Stratégies d'Orchestration
### Objectif
Explorer les méthodes d'orchestration pour le déploiement d'applications.

### Travaux réalisés
- Orchestration de serveurs (`Ansible`) :
  - Inventaire dynamique et déploiement avec stratégies de mise à jour (rolling updates).
- Orchestration de VMs (`Packer` & `OpenTofu`) :
  - Création d'images VM immuables et déploiement d'un groupe d'autoscaling (ASG) derrière un Load Balancer (ALB).
- Orchestration de conteneurs (`Docker` & `Kubernetes`) :
  - Conteneurisation de l'application puis déploiement sur cluster Kubernetes local et sur AWS (EKS).
- Serverless (`AWS Lambda`) :
  - Déploiement de fonctions serverless exposées via API Gateway en utilisant `OpenTofu`.

## Lab 4 : Versioning, Build et Tests Automatisés
### Objectif
Professionnaliser le cycle de développement logiciel.

### Travaux réalisés
- `Git` & `GitHub` : mise en place d'un flux collaboratif avec branches et Pull Requests.
- Système de build : configuration de scripts NPM pour automatiser les tâches courantes (lancement, packaging).
- Tests automatisés : écriture et exécution de tests unitaires et d'intégration avec `Jest` et `SuperTest`.

## Lab 5 : CI/CD avec Kubernetes
### Objectif
Mettre en place un pipeline d'intégration et de déploiement continus.

### Travaux réalisés
- Pipeline CI (`GitHub Actions`) : automatisation des tests applicatifs et d'infrastructure à chaque commit ou PR.
- Authentification OIDC : configuration de l'authentification sécurisée entre `GitHub Actions` et `AWS` via OpenID Connect.
- CD & GitOps : déploiement automatisé de l'infrastructure et de l'application via `OpenTofu` dans le pipeline CI/CD.

## Lab 6 : Multi-Environnements et Microservices
### Objectif
Gérer des environnements isolés et des architectures complexes.

### Travaux réalisés
- Isolation des environnements : utilisation de comptes AWS séparés ou de workspaces OpenTofu pour Dev / Staging / Production.
- Microservices : déploiement d'une architecture composée de plusieurs services (Frontend / Backend) interconnectés dans Kubernetes.
- Service Discovery : configuration de la découverte de services au sein du cluster Kubernetes pour permettre la communication entre microservices.

## Synthèse et prochaines étapes

Synthèse
- Le MVP (gestion utilisateurs + gestion des sondages côté backend) est opérationnel.
- Infrastructure et environnements locaux configurés ; base de données connectée au backend.
- Travail restant ciblé sur UI, microservice IA et industrialisation (CI/CD + Kubernetes).

Priorités immédiates (ordre, responsable, estimation)
1. UI — Tableau de bord & page de réponse au sondage
   - Responsable : Frontend
   - Estimation : 1–2 sprints
   - Livrable : page listant les sondages et permettant de répondre
2. Microservice IA (recommandation)
   - Responsable : Data/Backend
   - Estimation : 1 sprint + dockerisation
   - Livrable : API (Flask/FastAPI) renvoyant scoring/IDs triés
3. CI (tests & build) + Push d'images Docker
   - Responsable : DevOps
   - Estimation : 1 sprint
   - Actions : tests unitaires automatisés (`Jest`, `Pytest`), build & push images vers registre (Docker Hub / ECR)
4. CD & Kubernetes (manifests / Helm)
   - Responsable : DevOps
   - Estimation : 1–2 sprints
   - Actions : manifests Deployments/Services/Ingress, pipeline de déploiement

Mesures de succès (KPI)
- Pipeline CI vert à chaque PR (tests + build)
- Image IA déployable et répondant sous 200ms en interne
- Déploiement Kubernetes automatisé et reproduisible

## Roadmap Prévisionnelle

Voici les étapes planifiées pour finaliser le projet :

#### Phase 1 : Finalisation du "Core" (Semaine N)
- [x] Gestion Utilisateurs (Connexion/Inscription).
- [x] Gestion Admin des sondages (CRUD Backend).
- [ ] **Développement Frontend :** Page "Tableau de bord" et interface de réponse au sondage.
- [ ] **Développement Backend :** API de récupération des sondages pour l'utilisateur.

#### Phase 2 : Intégration "Data/IA" (Semaine N+1)
- [ ] Développement du microservice IA (Recommandation simple).
- [ ] Dockerisation du service IA.
- [ ] Communication inter-conteneurs : Le Backend envoie les infos user à l'IA -> L'IA renvoie les IDs des sondages triés.

#### Phase 3 : Industrialisation DevOps (Semaine N+2)
- [ ] Mise en place des tests automatisés (Jest pour le web, Pytest pour l'IA).
- [ ] Configuration du Workflow GitHub Actions (Build & Push Docker).
- [ ] Déploiement sur Kubernetes (fichiers YAML ou Helm Chart).
- [ ] Vérification finale du cycle complet (Code -> Pipeline -> Prod).

## Conclusion

Le projet est sur de bons rails : le socle d'authentification et la persistance sont fonctionnels, ce qui permet de paralléliser le travail sur l'interface utilisateur et le microservice IA. L'effort suivant consiste à automatiser les tests et les builds, puis connecter ces étapes à un pipeline CD vers Kubernetes pour obtenir un flux de livraison complet et reproductible.

Actions immédiates recommandées
- Planifier les tâches UI et IA en sprints distincts.
- Prioriser la mise en place des tests automatisés pour réduire les risques lors du déploiement.
- Mettre en place un pipeline CI simple qui build et pousse les images avant d'ajouter le CD.

## Annexes
- Liens, commandes, captures et références utiles.


## 1. Vision du Projet et Architecture Cible

Notre projet consiste en une application web microservices permettant aux utilisateurs de répondre à des sondages rémunérés. L'architecture est conçue pour être **cloud-native**, modulaire et scalable, répondant directement aux objectifs du projet final (Annexe 2 du sujet).

### Le Concept
Une plateforme minimaliste où la confiance est centrale. Les utilisateurs s'inscrivent, renseignent un profil et accèdent à un tableau de bord.
* **Innovation Data :** Pour personnaliser l'expérience, nous intégrons une **IA locale** (conteneurisée) qui analyse les profils (âge, centres d'intérêt, historique) pour recommander les sondages les plus pertinents via un filtrage collaboratif léger ou un système de scoring.
* **Système de Récompense :** Accumulation de points et conversion en récompenses.

### Alignement avec les Objectifs de l'Unité
Ce projet nous permet de mettre en pratique l'ensemble des concepts vus en TD :
* **Microservices (Lab 6) :** Séparation claire entre le Frontend (Interface utilisateur), le Backend (Gestion utilisateurs/sondages), la Base de Données et le Service IA.
* **Conteneurisation (Lab 3) :** Chaque brique (y compris l'IA) sera encapsulée dans Docker.
* **Orchestration (Lab 3 & 5) :** Déploiement prévu sur Kubernetes.
* **Data Science & DevOps :** Intégration d'un cycle de vie "Data" (le modèle de recommandation) dans un pipeline DevOps classique.

---

## 2. État d'Avancement Actuel

Nous avons franchi la première étape critique : la mise en place du socle technique et de la gestion des utilisateurs (MVP - Minimum Viable Product).

### Fonctionnalités Réalisées (Application)
* **Gestion des Utilisateurs (Frontend & Backend) :**
    * Système complet d'inscription et de connexion sécurisée (Authentification).
    * Interface frontend fonctionnelle pour la connexion, l'enregistrement et la déconnexion.
    * Persistance des données utilisateurs en base de données.
* **Gestion des Sondages (Backend & DB) :**
    * Modélisation de la base de données pour stocker les sondages.
    * Création des endpoints (API) et logique métier permettant à un administrateur de créer et gérer les sondages en base.

### Réalisations DevOps & Infrastructure
* **Environnement de Développement :** Configuration des dépôts Git et initialisation de l'environnement local (Node.js/Python).
* **Base de Données :** Instance de base de données opérationnelle et connectée au backend.

---

## 3. Analyse des Écarts et Objectifs Restants

Pour atteindre les objectifs finaux du module (CI/CD complet, déploiement Kubernetes, intégration Data), voici les chantiers restants.

### A. Développement Applicatif & Data
L'objectif est de transformer notre socle d'authentification en une plateforme active.
1.  **Interface Utilisateur (Tableau de bord) :** Créer la vue listant les sondages disponibles (actuellement uniquement en base de données).
2.  **Moteur de Recommandation (IA) :**
    * Développement du script Python (Scikit-learn ou logique simple) pour le scoring des sondages.
    * Conteneurisation de ce script dans une image Docker légère.
    * Création d'une API simple (Flask/FastAPI) pour que le Backend puisse interroger l'IA.
3.  **Système de Points :** Implémenter la logique d'incrémentation des points à la fin d'un sondage.

### B. DevOps & Industrialisation (Cœur du module)
C'est ici que nous appliquerons les acquis des Labs 4 et 5.
1.  **Pipeline CI (GitHub Actions) :**
    * Automatiser les tests unitaires (Backend et API de l'IA).
    * Automatiser la construction et le push des images Docker (Frontend, Backend, IA) sur un registre (Docker Hub ou ECR).
2.  **Pipeline CD & Kubernetes :**
    * Écriture des manifestes Kubernetes (Deployments, Services, Ingress).
    * Déploiement automatique sur le cluster cible.

---

## Conclusion

Le projet est sur de bons rails : le socle d'authentification et la persistance sont fonctionnels, ce qui permet de paralléliser le travail sur l'interface utilisateur et le microservice IA. L'effort suivant consiste à automatiser les tests et les builds, puis connecter ces étapes à un pipeline CD vers Kubernetes pour obtenir un flux de livraison complet et reproductible.

Actions immédiates recommandées
- Planifier les tâches UI et IA en sprints distincts.
- Prioriser la mise en place des tests automatisés pour réduire les risques lors du déploiement.
- Mettre en place un pipeline CI simple qui build et pousse les images avant d'ajouter le CD.

## Annexes
- Liens, commandes, captures et références utiles.