# AssistantIA - Frontend

## Vue d'ensemble
Frontend React moderne pour l'application AssistantIA, offrant une interface utilisateur premium avec authentification JWT, gestion multi-conversationnelle et interactions quasi instantanées avec l’IA via API HTTP.

---

### Technologies utilisées
React 18 avec Vite pour un développement rapide

React Router DOM v6 pour la navigation

Fetch API (via un wrapper `api.js`) pour les requêtes HTTP

CSS custom avec animations modernes (Glassmorphism)

JWT stocké côté client (localStorage) pour la session

> Note : certaines sections ci-dessous décrivent aussi des évolutions prévues (tests, CI/CD, monitoring). Voir la section "Notes de cohérence" en bas.

---

### Architecture du projet

```text
FrontEnd/
├── public/
│   ├── index.html
│   └── assets/
├── src/
│   ├── pages/
│   │   ├── Login.jsx
│   │   ├── Register.jsx
│   │   ├── Chat.jsx
│   │   ├── HistoryPanel.jsx
│   │   ├── AdminPanel.jsx
│   │   └── Profile.jsx
│   ├── App.jsx
│   ├── App.css
│   ├── main.jsx
│   └── api.js
├── .env
├── .gitignore
├── package.json
├── vite.config.js
└── README.md
```

Installation rapide

    Prérequis
    Node.js 18+ et npm/yarn
    Backend AssistantIA en cours d'exécution (http://localhost:8000
    )

    Installation

# Clonez le projet
git clone https://github.com/aureliencandillier-cyber/Assistant_IA
cd AssistantIA/FrontEnd

# Installez les dépendances
```bash
npm install
```
# ou
yarn install

    Configuration
    Créez un fichier .env à la racine du dossier FrontEnd :

VITE_API_URL=http://localhost:8000
VITE_APP_NAME=AssistantIA
VITE_APP_VERSION=1.0.0

    Lancement du serveur de développement

# Mode développement
```bash
npm run dev
```
# ou
yarn dev

L'application sera disponible sur http://localhost:5173

    Build pour production

# Build pour production
```bash
npm run build
```
# ou
yarn build

# Preview du build
```bash
npm run preview
```
# ou
yarn preview

Architecture d'authentification
Flux JWT

Connexion : L'utilisateur se connecte via /auth/login
Token : Le backend retourne un token JWT valide (par défaut 15 minutes côté backend)
Stockage : Le token est stocké dans localStorage
Requêtes : api.js ajoute automatiquement le header Authorization: Bearer <token>
Expiration : En cas de token invalide/expiré, le frontend nettoie la session et redirige vers /login
Sécurité côté client

Stockage : JWT stocké dans localStorage
Auto-déconnexion : suppression du token lorsque l'API retourne une 401
Protection des routes : navigation conditionnelle basée sur la présence du token
👤 Affichage du compte connecté (NOUVEAU)

Dès l’arrivée dans le chat, le frontend appelle /auth/me afin de :

    afficher en permanence l’email du compte connecté

    afficher un badge administrateur si role=admin

    conditionner l’accès à certaines fonctionnalités (ex: bouton Admin)

Système de chat
Caractéristiques principales

Messages quasi instantanés avec feedback visuel
Multi-conversations : gestion de plusieurs threads
Historique : récupération des conversations et messages depuis l’API
Interface responsive : optimisée pour mobile et desktop
Composants du chat

    Chat (Chat.jsx)
    Gestion de l’état de la conversation active, envoi des messages, affichage du header.

    Chargement du compte (/auth/me)

    Chargement automatique de la dernière conversation au démarrage (voir section dédiée)

    Envoi vers /ai/chat

    HistoryPanel (HistoryPanel.jsx)
    Liste des conversations et actions :

    sélectionner une conversation

    supprimer une conversation

    effacer tout l’historique

    renommer une conversation (NOUVEAU)

    AdminPanel (AdminPanel.jsx) (NOUVEAU)
    Fenêtre popup visible uniquement pour les admins :

    liste des utilisateurs

    recherche

    suppression ciblée

    Profile (Profile.jsx)
    Suppression du compte utilisateur + logout

🗂️ Gestion des conversations
Création de conversation

Manuelle : Utilisateur clique sur "Nouvelle conversation"
Automatique : créée au premier message sans conversation active
Organisation

Tri : par date de mise à jour côté backend (updated_at décroissant)
Recherche : amélioration possible côté frontend (non implémentée à ce stade)
Historique Panel

Interface premium : design glassmorphism
Actions rapides : sélectionner, renommer, supprimer, effacer tout
🏷️ Renommage des conversations (NOUVEAU)

Le frontend permet de renommer une conversation depuis l’historique :

    mode édition inline sur le titre

    sauvegarde via PATCH /conversations/{id} avec { "title": "..." }

    si la conversation renommée est active, le header du chat est mis à jour immédiatement

🧠 Chargement automatique de la dernière conversation

Au chargement de l’écran principal (Chat.jsx) :

    appel GET /conversations

    si au moins une conversation existe, sélection automatique de la plus récente (index 0)

    affichage immédiat du titre réel dans le header

    chargement des messages via GET /history/{conversation_id}

Objectif UX : éviter d’afficher “Nouvelle conversation” si l’utilisateur revient sur une conversation existante.
🛠️ Interface Admin (NOUVEAU)
Fonctionnement

Le bouton Admin apparaît uniquement si /auth/me retourne role=admin.
Capacités

    ouverture d’une popup de gestion

    listing des utilisateurs via GET /users (admin only)

    suppression ciblée via DELETE /users/{id} (admin only)

    protection : un admin ne peut pas se supprimer depuis cette fenêtre

Design System
Principes de design

Glassmorphism : effets de transparence et flou
Micro-interactions : animations subtiles pour le feedback
Responsive First : breakpoints adaptatifs
Palette de couleurs

--primary: #6366f1;      /* Bleu-violet principal */
--secondary: #8b5cf6;    /* Violet secondaire */
--success: #10b981;      /* Vert succès */
--danger: #ef4444;       /* Rouge erreur */
--warning: #f59e0b;      /* Orange avertissement */
--glass: rgba(255, 255, 255, 0.95); /* Fond glass */

Services API
Configuration API (Fetch wrapper)

Les requêtes HTTP sont centralisées via api.js :

    ajout du token JWT automatiquement

    normalisation des erreurs (ex: 401)

    simplification des appels (apiFetch("/route", {method, body}))

Endpoints consommés

# Authentication
POST   /auth/register
POST   /auth/login
GET    /auth/me

# Chat & AI
POST   /ai/chat

# Conversations
GET    /conversations
POST   /conversations
PATCH  /conversations/{id}      (NOUVEAU)
DELETE /conversations/{id}

# History
GET    /history/{conversation_id}
DELETE /history

# Users
DELETE /users/me
GET    /users                   (admin only) (NOUVEAU)
DELETE /users/{id}              (admin only)

Gestion des erreurs

Feedback utilisateur : messages d'erreur contextualisés
Fallback UI : états d'erreur (historique / admin)
Logging : console + affichage utilisateur
Responsive Design
Breakpoints

/* Mobile First */
sm: 640px   /* Mobile */
md: 768px   /* Tablet */
lg: 1024px  /* Desktop */
xl: 1280px  /* Large Desktop */

Sécurité frontend
Bonnes pratiques implémentées

Clear on Logout : nettoyage complet au logout
Auto-logout sur 401 : suppression du token quand le backend refuse la session
Aucune clé IA côté client : la clé reste côté backend
Workflows
Flux d'authentification

Login -> /auth/login -> store token -> redirect Chat -> /auth/me

Flux de conversation

User input -> POST /ai/chat -> receive answer -> update messages
If no conversation_id -> backend creates one -> frontend stores it

Tests (évolutions possibles)

Stratégie de test (prévu)
Unitaires : Jest + Testing Library
E2E : Playwright ou Cypress

Commandes de test (à mettre en place si ajoutées au projet)

npm run test

Déploiement (évolutions possibles)

Build optimisé

npm run build

Variables d'environnement (exemple)

VITE_API_URL=https://api.assistantia.com
VITE_ENV=production

Monitoring & Analytics (évolutions possibles)

Exemples d’outils possibles :

    Sentry (error tracking)

    Google Analytics (analytics)

Dépannage

    Échec d'authentification

    Vérifiez que le backend est en cours d'exécution

    Vérifiez les logs réseau DevTools

    Videz le localStorage et réessayez

    Messages non envoyés

    Vérifiez que le token JWT n'a pas expiré

    Vérifiez la route /ai/chat côté backend

    Interface lente

    Videz le cache du navigateur

    Réduisez le nombre de conversations chargées (pagination future)