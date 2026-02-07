# 🤖 AssistantIA

**AssistantIA** est une application complète d’assistant conversationnel basé sur l’IA, conçue avec une architecture **Frontend / Backend découplée**, sécurisée et orientée bonnes pratiques industrielles.

Le projet met l’accent sur :
- la **fiabilité des échanges**
- la **gestion multi-conversationnelle**
- la **sécurité (JWT, isolation des données)**
- une architecture claire et maintenable

---

## 🧩 Architecture globale

```text
AssistantIA/
├── BackEnd/     # API FastAPI sécurisée (auth, conversations, IA)
├── FrontEnd/    # Application React (UI, chat, historique)
└── README.md    # Présentation globale (ce fichier)
```
🔗 Documentation détaillée

Chaque partie dispose de sa propre documentation complète :
🔹 Backend – API FastAPI

    Authentification JWT

    Gestion des utilisateurs et rôles

    Conversations multi-threads

    Intégration d’un LLM externe (OpenRouter)

    Sécurité, isolation des données, traçabilité

📘 👉 Lire le README Backend
➡️ https://github.com/aureliencandillier-cyber/Assistant_IA/blob/dev/BackEnd/README.md
🔹 Frontend – Application React

    Interface moderne (React + Vite)

    Authentification JWT côté client

    Gestion des conversations et de l’historique

    UX fluide et responsive

    Interface Admin conditionnelle

📘 👉 Lire le README Frontend
➡️ https://github.com/aureliencandillier-cyber/Assistant_IA/blob/dev/FrontEnd/README.md
🎯 Objectifs du projet

    Démontrer une architecture full-stack propre

    Appliquer une rigueur d’ingénierie (sécurité, séparation des responsabilités)

    Mettre en pratique des concepts clés :

        API REST

        Authentification stateless

        Gestion d’état frontend

        Intégration IA côté serveur

    Servir de projet vitrine orienté développement IA / MLOps

🚀 Statut

Projet fonctionnel, en évolution continue, utilisé comme support de montée en compétences en développement IA, backend, frontend et MLOps.

📩 Pour toute question ou échange technique :
Aurélien Candillier