---
title: Compte Rendu - Vote2Earn Platform
draft: false
author: Killian Diboues
---

**Application de vote avec système de récompenses déployée sur Azure**

---

## Table des Matières
1. Vue d'ensemble
2. Architecture DevOps
3. Architecture Application
4. Résultats & Déploiements

---

## Vue d'ensemble

### Objectif du Projet
Créer une plateforme de vote complète avec :
- Authentification utilisateur (Login/Register)
- Création et gestion de sondages
- Système de vote en temps réel
- Dashboard utilisateur et admin
- Déploiement cloud scalable

### Stack Technologique
| Composant | Technologie |
|-----------|-------------|
| **Frontend** | React 19 + TypeScript + Vite + Tailwind CSS |
| **Backend** | Node.js + Express + TypeScript |
| **Base de Données** | PostgreSQL 15 |
| **Conteneurisation** | Docker & Docker Compose |
| **Orchestration** | Kubernetes (Local: Minikube, Cloud: Azure AKS) |
| **Infrastructure-as-Code** | Terraform |
| **Monitoring** | Prometheus + Grafana |
| **CI/CD** | GitHub Actions |

---

---

## ARCHITECTURE DEVOPS

### 1. Infrastructure Locale (Docker Compose)

#### Architecture
```
┌─────────────────────────────────────────────┐
│       DÉVELOPPEMENT LOCAL (Docker)          │
├─────────────────────────────────────────────┤
│  Frontend        Backend        PostgreSQL   │
│  (Nginx)         (Node.js)      (Port 5432) │
│ (Port 8080)      (Port 5000)                │
└─────────────────────────────────────────────┘
```

#### Configuration Docker Compose
**Fichier :** `docker-compose.yml`

**Services :**
1. **PostgreSQL** (`db`)
   - Image : `postgres:15-alpine`
   - Persistance : Volume Docker `db_data`
   - Port : `5432`
   - Variables : Chargées depuis `.env`

2. **Backend** (API Express)
   - Build : `./backend/Dockerfile`
   - Port : `5000`
   - Dépendances : PostgreSQL
   - Runtime : Node.js avec TypeScript (tsx)

3. **Frontend** (React + Nginx)
   - Build : `./frontend/Dockerfile`
   - Port : `8080` → `80` (Nginx)
   - Dépendances : Backend
   - Build optimisé avec Vite

#### Lancement Local
```bash
# Démarrage complet
docker-compose up --build

# Accès
- Frontend : http://localhost:8080
- Backend API : http://localhost:3000
- Database : localhost:5432
```

<video controls src="/DevOps/TD/vid-2.mp4" title="Title"></video>

---

### 2. Infrastructure Kubernetes - Local (Minikube)

#### Architecture
```
┌──────────────────────────────────────┐
│       MINIKUBE CLUSTER               │
├──────────────────────────────────────┤
│  Namespace: default                  │
│  ┌────────────────────────────────┐  │
│  │  Pod Frontend (Replica 1)      │  │
│  │  + Service: NodePort (31000)   │  │
│  ├────────────────────────────────┤  │
│  │  Pod Backend (Replica 1)       │  │
│  │  + Service: ClusterIP (5000)   │  │
│  ├────────────────────────────────┤  │
│  │  Pod PostgreSQL (Replica 1)    │  │
│  │  + Service: ClusterIP (5432)   │  │
│  └────────────────────────────────┘  │
└──────────────────────────────────────┘
```

#### Manifests Kubernetes
**Fichiers :** Dossier `k8s/local/`

| Fichier | Description |
|---------|-------------|
| `backend-local.yml` | Deployment + Service pour Backend |
| `frontend-local.yml` | Deployment + Service pour Frontend |
| `configmap-local.yml` | Variables de configuration |
| Autres manifests | Secrets, ConfigMap partagés |

#### Déploiement Local
```bash
# Script de déploiement automatisé
./scripts/deploy-local.sh

# Vérifications
kubectl get pods                    # Voir tous les pods
kubectl get svc                     # Voir tous les services
kubectl logs -f pod/[pod-name]      # Logs en temps réel
```


<video controls src="/DevOps/TD/vid-3.mp4" title="Title"></video>
<video controls src="/DevOps/TD/vid-4.mp4" title="Title"></video>

---

### 3. Infrastructure Cloud (Azure AKS)

#### Architecture Complète
```
┌─────────────────────────────────────────────────────┐
│              AZURE CLOUD INFRASTRUCTURE             │
├─────────────────────────────────────────────────────┤
│                                                     │
│  ┌─────────────────────────────────────────┐        │
│  │     Terraform (IaC)                     │        │
│  │  • Groupe de ressources Azure           │        │
│  │  • Cluster AKS (managed Kubernetes)     │        │
│  │  • Azure Container Registry (ACR)       │        │
│  │  • PostgreSQL Azure Managed Database    │        │
│  └─────────────────────────────────────────┘        │
│                                                     │
│  ┌─────────────────────────────────────────┐        │
│  │     Cluster AKS (3 nodes)               │        │
│  │  ┌─────────────────────────────────┐    │        │
│  │  │  Frontend Pod (3 replicas)      │    │        │
│  │  │  + LoadBalancer Service (IP)    │    │        │
│  │  ├─────────────────────────────────┤    │        │
│  │  │  Backend Pod (3 replicas)       │    │        │
│  │  │  + ClusterIP Service            │    │        │
│  │  ├─────────────────────────────────┤    │        │
│  │  │  Prometheus + Grafana           │    │        │
│  │  │  (Monitoring)                   │    │        │
│  │  └─────────────────────────────────┘    │        │
│  └─────────────────────────────────────────┘        │
│                                                     │
│  ┌─────────────────────────────────────────┐        │
│  │  PostgreSQL Azure Managed Database      │        │
│  │  (Multi-AZ, Auto-backup)                │        │
│  └─────────────────────────────────────────┘        │
│                                                     │
└─────────────────────────────────────────────────────┘
```

#### Infrastructure-as-Code (Terraform)

**Fichiers :** Dossier `terraform/`

| Fichier | Contenu |
|---------|---------|
| `main.tf` | Configuration Terraform + Groupe de ressources |
| `aks.tf` | Cluster Kubernetes Azure |
| `acr.tf` | Azure Container Registry (registre d'images) |
| `variables.tf` | Variables paramétrables |
| `outputs.tf` | Outputs (IPs, endpoints) |

**Ressources Terraform créées :**
```hcl
- azurerm_resource_group          # Groupe de ressources
- azurerm_kubernetes_cluster      # Cluster AKS
- azurerm_container_registry      # Registre Docker
- azurerm_postgresql_server       # Base de données managée
```

#### Déploiement Cloud

**Étape 1 : Créer l'infrastructure avec Terraform**
```bash
cd terraform
terraform init
terraform plan      # Vérifier les changements
terraform apply     # Créer l'infrastructure
```

**Étape 2 : Déployer l'application**
```bash
./scripts/deploy.sh

# Récupérer l'IP publique du frontend
kubectl get svc frontend
```
<video controls src="/DevOps/TD/vid-5.mp4" title="Title"></video>
![alt text](/DevOps/TD/image.png)
<video controls src="/DevOps/TD/vid-6.mp4" title="Title"></video>

---

### 4. CI/CD Pipeline avec GitHub Actions

#### Vue d'ensemble du Pipeline

**Fichier :** `.github/workflows/ci.yml`

Le pipeline CI/CD automatise l'intégralité du processus de test, build et déploiement sur chaque push vers les branches `main` , ainsi que sur les pull requests.

#### Architecture du Pipeline

```
Event (Push/PR)
     ↓
┌─────────────────────────────────────┐
│  Job 1: backend-test                │
│  • Setup Node.js 20                 │
│  • npm ci (install)                 │
│  • npm run build (TypeScript check) │
│  • npm test (Jest tests)            │
│  Status: PASS/FAIL                  │
└─────────────────────────────────────┘
     ↓
┌─────────────────────────────────────┐
│  Job 2: frontend-build              │
│  • Setup Node.js 20                 │
│  • npm ci (install)                 │
│  • npm run build (Vite build)       │
│  Status: PASS/FAIL                  │
└─────────────────────────────────────┘
     ↓
┌─────────────────────────────────────┐
│  Job 3: check-infrastructure        │
│  • Azure Login                      │
│  • Vérifier AKS cluster existe      │
│  Status: exists=true/false          │
└─────────────────────────────────────┘
     ↓
┌─────────────────────────────────────┐
│  Job 4: build-push (si infra=true)  │
│  • Azure Login                      │
│  • Docker build Backend             │
│  • Docker push to ACR               │
│  • Docker build Frontend            │
│  • Docker push to ACR               │
│  Status: Images prêtes en ACR       │
└─────────────────────────────────────┘
     ↓
┌─────────────────────────────────────┐
│  Job 5: deploy (si build=success)   │
│  • Azure Login                      │
│  • Kubectl apply configs            │
│  • Kubectl rollout restart          │
│  Status: App déployée en AKS        │
└─────────────────────────────────────┘
```

#### Détail des Jobs

**Job 1 : Backend Test**
```yaml
Name: Backend Build & Test
Trigger: Sur toute modification du backend
Étapes:
  1. Checkout code
  2. Setup Node.js 20 avec cache npm
  3. npm ci (installation déterministe)
  4. npm run build (compilation TypeScript)
  5. npm test (tests Jest)
     - Variables d'env de test injectées
     - JWT_SECRET, DB_NAME, etc.
```

**Job 2 : Frontend Build**
```yaml
Name: Frontend Build
Trigger: Sur toute modification du frontend
Étapes:
  1. Checkout code
  2. Setup Node.js 20 avec cache npm
  3. npm ci (installation déterministe)
  4. npm run build (build Vite optimisé)
```

**Job 3 : Check Infrastructure**
```yaml
Name: Check if Infrastructure Exists
Dépendances: Aucune (en parallèle)
Étapes:
  1. Azure Login (credentials from secrets)
  2. Vérifier l'existence du cluster AKS
     az aks show --name aks-vote2earn
  3. Output: exists=true/false
  
Utilisation:
  - Si infra n'existe pas, skip build-push & deploy
  - Si infra existe, procéder au build et déploiement
```

**Job 4 : Build & Push**
```yaml
Name: Build & Push to ACR
Dépendances: backend-test, frontend-build, check-infrastructure
Condition: Si check-infrastructure.exists == 'true'
Étapes:
  1. Checkout code
  2. Azure Login
  3. ACR Login (acrvote2earn.azurecr.io)
  4. Docker Build & Push Backend
     - Tag: acrvote2earn.azurecr.io/backend:latest
  5. Docker Build & Push Frontend
     - Tag: acrvote2earn.azurecr.io/frontend:latest
  
Résultat:
  - Images disponibles dans Azure Container Registry
  - Prêtes à être déployées en Kubernetes
```

**Job 5 : Deploy**
```yaml
Name: Deploy to AKS
Dépendances: build-push
Étapes:
  1. Checkout code
  2. Azure Login
  3. Setup kubelogin (authentification AKS)
  4. Get Kubernetes Context (récupérer credentials AKS)
  5. Appliquer manifests Kubernetes:
     - kubectl apply -f k8s/configmap.yml
     - kubectl apply -f k8s/postgres.yml
     - kubectl apply -f k8s/backend.yml
     - kubectl apply -f k8s/frontend.yml
     - kubectl apply -f k8s/monitoring/prometheus.yml
     - kubectl apply -f k8s/monitoring/grafana.yml
  6. Force Restart deployments
     - kubectl rollout restart deployment/backend
     - kubectl rollout restart deployment/frontend
     (Force Kubernetes à tirer les nouvelles images)
  
Résultat:
  - Deployments mis à jour avec les nouvelles images
  - Anciens pods terminés, nouveaux pods démarrés
  - Application en production sur Azure AKS
```

#### Variables et Secrets Utilisés

**GitHub Secrets (à configurer dans Settings > Secrets and variables):**
```
AZURE_CLIENT_ID             # ID du service principal Azure
AZURE_CLIENT_SECRET         # Secret du service principal
AZURE_SUBSCRIPTION_ID       # ID de la souscription Azure
AZURE_TENANT_ID             # Tenant ID Azure
```

**Variables d'Environnement Backend Test:**
```
JWT_SECRET: "test_secret"
JWT_EXPIRES_IN: "1d"
DB_NAME: "test_db"
DB_USER: "test_user"
DB_HOST: "localhost"
DB_PORT: "5432"
```

#### Triggers du Pipeline

| Event | Branches | Action |
|-------|----------|--------|
| Push | main | Lancer tous les jobs |
| Pull Request | main | Lancer tous les jobs sauf deploy |
| Manual | N/A | Disponible via GitHub UI |

#### Workflow de Développement avec CI/CD

```
1. Developer fait un commit & push
   ↓
2. GitHub Actions déclenche le workflow
   ↓
3. Tests backend + build frontend en parallèle
   ↓
   Si les 2 échouent → STOP, notifier developer
   ↓
4. Vérifier si infrastructure Azure existe
   ↓
   Si non → STOP, message: "Run deploy.sh locally first"
   ↓
5. Si tout OK → Build & Push images à ACR
   ↓
6. Déployer en Kubernetes
   ↓
7. App live en production
   ↓
8. Notifier du succès/erreur
```

#### Exemple : Cycle Complet

**Scénario 1 : Push réussi**
```
23:45 Developer fait un commit & push sur 'main'
23:46 GitHub Actions démarre le workflow
23:47 Backend tests: PASS (2 min)
23:47 Frontend build: PASS (2 min)
23:48 Check infrastructure: EXISTS (30s)
23:49 Build & Push: COMPLETE (2 min)
23:51 Deploy to AKS: COMPLETE (1 min)
23:52 App live avec les changements
```

**Scénario 2 : Erreur lors des tests**
```
00:05 Developer fait un commit & push
00:06 GitHub Actions démarre
00:08 Backend tests: FAIL (lint error)
00:08 Frontend build: PASS
00:09 Pipeline arrêté, notification d'erreur envoyée
Developer corrige le problème et re-push
```

**Scénario 3 : Infrastructure n'existe pas**
```
00:15 Developer fait un commit & push
00:16 GitHub Actions démarre
00:18 Backend tests: PASS
00:18 Frontend build: PASS
00:19 Check infrastructure: NOT FOUND
00:19 Pipeline s'arrête gracieusement
Message: "Run ./scripts/deploy.sh locally to create infrastructure first"
```

#### Monitoring du Pipeline

**Accès aux Logs:**
```
1. GitHub Repository → Actions tab
2. Sélectionner le workflow le plus récent
3. Cliquer sur le job pour voir les détails
4. Logs détaillés pour chaque étape
```

**Status Badge:**
```markdown
[![CI Status](https://github.com/user/repo/workflows/CI/badge.svg)](https://github.com/user/repo/actions)
```

![alt text](/DevOps/TD/image-1.png)
![alt text](/DevOps/TD/image-2.png)
![alt text](/DevOps/TD/image-3.png)
![alt text](/DevOps/TD/image-4.png)
![alt text](/DevOps/TD/image-5.png)
![alt text](/DevOps/TD/image-6.png)
![alt text](/DevOps/TD/image-7.png)

---

### 5. Scripts de Déploiement

#### Scripts Disponibles
**Fichiers :** Dossier `scripts/`

| Script | Objectif |
|--------|----------|
| `deploy-local.sh` | Déploiement sur Minikube |
| `deploy.sh` | Création infra + déploiement cloud |
| `update.sh` | Mise à jour de l'application en production |
| `destroy.sh` | Destruction de l'infrastructure |

**Exemple : deploy-local.sh**
```bash
#!/bin/bash
# 1. Démarrer Minikube
# 2. Builder les images Docker
# 3. Charger images dans Minikube
# 4. Appliquer les manifests Kubernetes
# 5. Afficher l'accès à l'app
```

#### Mises à Jour en Production
```bash
# Option 1 : Via GitHub Actions (automatique)
git push → Workflow CI/CD démarre → Deploy auto

# Option 2 : Manual update
# Build nouvelle image
docker build -t acrvote2earn.azurecr.io/backend:latest ./backend

# Push sur Azure Container Registry
docker push acrvote2earn.azurecr.io/backend:latest

# Redéployer (Kubernetes tire la nouvelle image)
./scripts/update.sh
```

---

### 6. Monitoring & Observabilité

#### Stack Monitoring
**Fichiers :** Dossier `k8s/monitoring/`

**Composants :**
1. **Prometheus**
   - Collection métriques
   - Retention : 15 jours
   - Port : `9090`

2. **Grafana**
   - Dashboards de visualisation
   - Port : `3000`
   - Datasource : Prometheus

#### Accès Monitoring
```bash
# Port-forward Prometheus
kubectl port-forward svc/prometheus 9090:9090
# http://localhost:9090

# Port-forward Grafana
kubectl port-forward svc/grafana 3000:3000
# http://localhost:3000 (admin/admin)
```

![alt text](/DevOps/TD/image-73.png)
![alt text](/DevOps/TD/image-74.png)

---

### 7. Sécurité DevOps

#### Bonnes Pratiques Implémentées

**Secrets Management**
- Variables sensibles en Kubernetes Secrets
- Fichier : `k8s/secrets.yml`
- Pas de credentials en git

**Network Policies**
- Services ClusterIP pour internal communication
- LoadBalancer uniquement pour Frontend public
- Backend non exposé directement

**Container Security**
- Images Alpine (petite surface d'attaque)
- Port mapping restrictif
- No root user dans conteneurs

**GitHub Actions Security**
- Secrets stockés sécurisement
- Credentials Azure en GitHub Secrets
- Aucun token exposé dans les logs

**Helm Charts** (Optionnel)
- Templating pour prod/staging
- Versioning cohérent

---

## ARCHITECTURE APPLICATION

### 1. Backend (API Express)

#### Arborescence
```
backend/
├── src/
│   ├── index.ts              # Point d'entrée
│   ├── config/
│   │   ├── config.ts         # Configuration générale
│   │   └── database.ts       # Config Sequelize + PostgreSQL
│   ├── controllers/
│   │   ├── auth.controller.ts      # Login/Register
│   │   └── user.controller.ts      # Gestion utilisateurs
│   ├── routes/
│   │   ├── index.ts          # Router principal
│   │   ├── auth.routes.ts    # Routes auth
│   │   ├── poll.routes.ts    # Routes sondages
│   │   └── user.routes.ts    # Routes utilisateurs
│   ├── models/
│   │   ├── User.ts           # Modèle utilisateur
│   │   ├── Poll.ts           # Modèle sondage
│   │   ├── PollOption.ts     # Options du sondage
│   │   ├── Vote.ts           # Votes utilisateurs
│   │   └── associations.ts   # Relations ORM
│   ├── middleware/
│   │   └── auth.middleware.ts   # Authentification JWT
│   ├── migrations/
│   │   └── 20251117222926-create-initial-tables.js
│   ├── types/
│   │   └── express/
│   │       └── index.d.ts    # Types Express custom
│   └── __tests__/
│       └── health.test.ts    # Tests unitaires
├── package.json
├── tsconfig.json
├── jest.config.ts
└── Dockerfile
```

#### Stack Backend
- **Runtime :** Node.js 20+
- **Framework :** Express.js
- **ORM :** Sequelize (TypeScript)
- **Base Données :** PostgreSQL
- **Auth :** JWT (jsonwebtoken)
- **Sécurité :** Helmet, CORS, express-validator
- **Logging :** Morgan
- **Testing :** Jest

#### Endpoints Principaux

**Authentication**
```
POST   /api/auth/register      # Créer compte
POST   /api/auth/login         # Se connecter
POST   /api/auth/logout        # Se déconnecter
```

**Sondages**
```
GET    /api/polls              # Lister tous les sondages
POST   /api/polls              # Créer un sondage (admin)
GET    /api/polls/:id          # Détails d'un sondage
PUT    /api/polls/:id          # Modifier un sondage
DELETE /api/polls/:id          # Supprimer un sondage
```

**Votes**
```
POST   /api/polls/:id/vote     # Voter
GET    /api/polls/:id/results  # Résultats du sondage
```

**Utilisateurs**
```
GET    /api/users/profile      # Mon profil
PUT    /api/users/profile      # Mettre à jour profil
GET    /api/users/admin        # Dashboard admin (admin only)
```

#### Modèle de Données

```
┌─────────────┐
│    User     │
├─────────────┤
│ id (PK)     │
│ email       │
│ password    │
│ username    │
│ isAdmin     │
│ createdAt   │
└──────┬──────┘
       │ (1:N)
       │
       ↓
┌─────────────┐
│    Poll     │
├─────────────┤
│ id (PK)     │
│ title       │
│ description │
│ createdBy   │ (FK User)
│ createdAt   │
└──────┬──────┘
       │ (1:N)
       │
       ↓
┌──────────────────┐
│  PollOption      │
├──────────────────┤
│ id (PK)          │
│ pollId (FK)      │
│ title            │
│ voteCount        │
└────────┬─────────┘
         │ (1:N)
         │
         ↓
      ┌─────────┐
      │  Vote   │
      ├─────────┤
      │ id (PK) │
      │ userId  │ (FK User)
      │ optionId│ (FK PollOption)
      │ votedAt │
      └─────────┘
```

#### Démarrage Backend
```bash
cd backend
npm install
npm run dev         # Mode développement
npm run build       # Build TypeScript
npm start           # Production
```

---

### 2. Frontend (React + Vite)

#### Arborescence
```
frontend/
├── src/
│   ├── main.tsx              # Point d'entrée
│   ├── App.tsx               # Composant root
│   ├── index.css             # Styles globaux
│   ├── App.css
│   ├── components/
│   │   ├── auth/
│   │   │   ├── LoginForm.tsx      # Form login
│   │   │   └── RegisterForm.tsx   # Form register
│   │   └── layout/
│   │       └── Header.tsx         # Navigation
│   ├── pages/
│   │   ├── LoginPage.tsx               # Page login
│   │   ├── RegisterPage.tsx            # Page register
│   │   ├── HomePage.tsx                # Accueil
│   │   ├── DashboardPage.tsx           # Dashboard user
│   │   ├── AdminDashboardPage.tsx      # Dashboard admin
│   │   ├── PollsPage.tsx               # Liste sondages
│   │   ├── CreatePollPage.tsx          # Créer sondage
│   │   ├── EditPollPage.tsx            # Modifier sondage
│   │   ├── PollDetailPage.tsx          # Détails + vote
│   │   └── ProfilePage.tsx             # Profil utilisateur
│   ├── contexts/
│   │   └── AuthContext.tsx       # État auth global (Context API)
│   ├── lib/
│   │   ├── api.ts              # Client API (Axios)
│   │   └── axios.ts            # Configuration Axios
│   ├── assets/                 # Ressources (images, etc)
│   └── TestComponent.tsx        # Composant test
├── public/
├── index.html
├── package.json
├── tsconfig.json
├── vite.config.ts
├── tailwind.config.js          # Configuration Tailwind CSS
├── eslint.config.js
├── nginx.conf                  # Config Nginx (production)
└── Dockerfile
```

#### Stack Frontend
- **Framework :** React 19
- **Build Tool :** Vite
- **Langage :** TypeScript
- **State Management :** Context API + Custom Hooks
- **API Client :** Axios
- **Form Handling :** React Hook Form
- **Validation :** Zod
- **Styling :** Tailwind CSS + PostCSS
- **UI Components :** Heroicons
- **Routing :** React Router v7
- **Data Fetching :** TanStack React Query v5

#### Pages Principales

| Page | Route | Description |
|------|-------|-------------|
| **HomePage** | `/` | Accueil, présentation |
| **LoginPage** | `/login` | Formulaire login |
| **RegisterPage** | `/register` | Créer compte |
| **DashboardPage** | `/dashboard` | Tableau de bord user |
| **PollsPage** | `/polls` | Liste de tous les sondages |
| **PollDetailPage** | `/polls/:id` | Détails + vote |
| **CreatePollPage** | `/polls/create` | Créer un nouveau sondage |
| **EditPollPage** | `/polls/:id/edit` | Modifier un sondage |
| **AdminDashboardPage** | `/admin` | Dashboard admin |
| **ProfilePage** | `/profile` | Profil utilisateur |

#### État Authentification

**AuthContext.tsx**
```typescript
interface AuthContextType {
  user: User | null;
  isAuthenticated: boolean;
  login: (email, password) => Promise<void>;
  register: (email, password, username) => Promise<void>;
  logout: () => void;
  loading: boolean;
}
```

#### Démarrage Frontend
```bash
cd frontend
npm install
npm run dev         # Mode développement (port 5173)
npm run build       # Build pour production
npm run preview     # Aperçu du build
npm run lint        # Vérifier le code
```

<video controls src="/DevOps/TD/Vid-1.mp4" title="Title"></video>

---

### 3. Flux de Données (Data Flow)

#### Authentication Flow
```
1. User remplit formulaire Login
   ↓
2. RegisterForm/LoginForm envoie POST à /api/auth/login
   ↓
3. Backend vérifie credentials
   ↓
4. Si OK → génère JWT token
   ↓
5. Frontend stocke token en localStorage
   ↓
6. Axios ajoute token à chaque requête (header Authorization)
   ↓
7. AuthContext met à jour état global
   ↓
8. Redirection vers Dashboard
```

#### Vote Flow
```
1. User clique bouton "Voter"
   ↓
2. Frontend appelle POST /api/polls/:id/vote
   ↓
3. Backend :
   - Vérifie authentification (middleware JWT)
   - Valide que user peut voter
   - Enregistre le vote en DB
   ↓
4. Frontend reçoit réponse
   ↓
5. Page se met à jour (résultats affichés)
```

---

### 4. Fonctionnalités Principales

#### Authentification
- Inscription (email, username, password)
- Connexion (email, password)
- JWT tokens
- Logout
- Rôles (User / Admin)

#### Gestion Sondages
- Créer sondage (admin)
- Lister sondages
- Voir détails sondage
- Modifier sondage (créateur)
- Supprimer sondage (créateur/admin)

#### Système de Vote
- Voter sur une option
- Voir résultats en temps réel
- Une seule option par user par sondage (constraint DB)
- Historique des votes user

#### Dashboard Utilisateur
- Voir mes sondages créés
- Voir mes votes
- Statistiques personnelles

#### Dashboard Admin
- Voir tous les users
- Voir tous les sondages
- Statistiques globales
- Gérer utilisateurs / sondages

---

### 5. Tests

#### Tests Backend
**Fichier :** `backend/__tests__/health.test.ts`

```bash
cd backend
npm test        # Lance Jest
```

#### Tests Frontend
À implémenter avec Vitest/Jest

```bash
cd frontend
npm test        # En cours de setup
```

---

---

## RÉSULTATS & DÉPLOIEMENTS

### 1. Checklist de Déploiement

#### Développement Local (Docker)
- [x] Docker Compose configuré (3 services)
- [x] Images Docker optimisées
- [x] Volumes persistants pour DB
- [x] Env variables gérées
- [x] Tout démarre avec 1 commande

#### Kubernetes Local (Minikube)
- [x] Manifests Kubernetes écrits
- [x] Deployments + Services configurés
- [x] ConfigMap pour configuration
- [x] Secrets pour sensibles
- [x] Script de déploiement automatisé

#### Cloud (Azure AKS)
- [x] Terraform IaC
- [x] Cluster AKS créé (3+ nodes)
- [x] ACR pour images Docker
- [x] PostgreSQL managed database
- [x] LoadBalancer pour Frontend
- [x] Monitoring (Prometheus + Grafana)
- [x] GitHub Actions CI/CD

#### Application
- [x] Backend API complète
- [x] Frontend React interactive
- [x] Authentification + JWT
- [x] CRUD sondages complets
- [x] Système de vote
- [x] Dashboard user & admin
- [x] Validation des données
- [x] Gestion d'erreurs

---

### 2. Résultats Observables

#### En Local
- [X] Docker containers running (docker ps)
- [X] Frontend accessible sur http://localhost:8080
- [X] Backend API répondant sur :3000
- [X] Database connectée

#### Sur Minikube
- [X] Minikube cluster running
- [X] Pods en état Running
- [X] Services exposés (minikube service frontend)
- [X] Logs de chaque service
- [X] Performance metrics (CPU/Memory)

#### Sur Azure AKS
- [X] Ressources Azure créées (Portal)
- [X] Cluster AKS avec 3 nodes
- [X] ACR avec images pushées
- [X] Application live sur IP publique
- [X] Grafana dashboard actif
- [X] Logs centralisés
- [X] Coûts Azure estimés

---

### 3. Points Forts de l'Architecture

**Moderne & Scalable**
- Kubernetes pour orchestration
- Terraform pour IaC
- Infrastructure immuable

**Sécurisée**
- JWT authentication
- Secrets en Kubernetes
- CORS / Helmet configurés
- Network isolation
- GitHub Actions avec secrets sécurisés

**Fiable**
- Multi-replica deployments
- Health checks
- Persistent storage
- Database backups

**Monitorée**
- Prometheus + Grafana
- Logs centralisés
- Métriques d'application
- Alertes

**Easy Deploy**
- Scripts automatisés
- CI/CD pipeline GitHub Actions
- Documentation complète
- Reproducible environment

---

### 6. Points d'Amélioration Futurs

**À explorer :**
- [ ] Auto-scaling (HPA - Horizontal Pod Autoscaler)
- [ ] Service mesh (Istio) pour traffic management
- [ ] Ingress controller pour routing avancé
- [ ] Kustomize ou Helm pour templating
- [ ] ArgoCD pour GitOps deployment
- [ ] Backend caching (Redis)
- [ ] CDN pour frontend assets
- [ ] WebSocket pour real-time updates
- [ ] Unit & Integration tests plus complets
- [ ] Code coverage tracking dans CI/CD

---

## Résumé Exécutif

**Vote2Earn** est une plateforme de vote complète déployée sur Kubernetes avec une infrastructure entièrement gérée par code (Terraform). L'application est composée d'une API Express et d'un frontend React, accessible localement via Docker Compose, sur Kubernetes local (Minikube), ou sur Azure AKS.

Un pipeline CI/CD entièrement automatisé via GitHub Actions gère les tests, le build des images Docker, leur publication dans Azure Container Registry, et le déploiement en production sur le cluster AKS.

**Points clés :**
- Architecture moderne et scalable (Kubernetes)
- Infrastructure-as-Code (Terraform)
- CI/CD pipeline automatisé (GitHub Actions)
- Monitoring complet (Prometheus + Grafana)
- Application feature-complete (Auth, Polls, Votes)
- Déploiement simple et automatisé
- Prête pour production

---

**Dernière mise à jour :** Décembre 2025

*Pour plus d'informations, consulter le fichier README.md du projet Vote2Earn.*
