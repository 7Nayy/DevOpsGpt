# DevOpsGPT

Projet réalisé dans le cadre de l'évaluation finale **Software Engineering & DevOps** — ESILV.

> Application web de chat propulsée par GPT-4, avec historique persistant et pipeline CI/CD automatisé.

---

## Structure du projet

```
DevOpsGpt/
├── backend/          # API Node.js (port 3000)
│   └── Dockerfile
├── frontend/         # Interface utilisateur Vite (port 5173)
│   └── Dockerfile
├── exercice1/        # Livrables conception (diagrammes + dictionnaire de données)
├── docker-compose.yml
└── .github/
    └── workflows/
        └── main.yml  # Pipeline CI/CD
```

---

## Exercice 1 — Conception Logicielle

Les livrables sont dans le dossier [`exercice1/`](./exercice1/) :

| Fichier | Description |
|---|---|
| `diagramme-contexte.jpg` | Diagramme de contexte : Utilisateur ↔ DevOpsGPT ↔ API GPT-4 / BDD |
| `flowchart.jpg` | Flowchart du traitement d'un message avec filtre de modération |
| `dictionnaire-donnees.pdf` | Dictionnaire de données de l'entité `Message` |

### Dictionnaire de données — `Message`

| Attribut | Type | Description |
|---|---|---|
| `id` | UUID / Integer | Identifiant unique (clé primaire) |
| `contenu` | String (TEXT) | Texte du message envoyé par l'utilisateur |
| `reponse` | String (TEXT) | Réponse générée par l'API GPT-4 |
| `created_at` | Timestamp | Date et heure de création |
| `user_id` | UUID / Integer (FK) | Référence vers l'utilisateur auteur |
| `is_flagged` | Boolean | Vrai si le message a été bloqué par la modération |

---

## Exercice 2 — Git & Docker

### User Story — Abonnement Premium

> *En tant qu'utilisateur premium, je veux souscrire à un abonnement payant afin d'accéder à des fonctionnalités avancées (historique illimité, modèles GPT plus puissants).*

### Lancer l'application avec Docker Compose

```bash
# Copier et renseigner la clé API
cp .env.example .env

# Démarrer tous les services
docker compose up --build
```

| Service | URL |
|---|---|
| Frontend | http://localhost:5173 |
| Backend API | http://localhost:3000 |
| Redis | localhost:6379 |

### Variables d'environnement

Créer un fichier `.env` à la racine :

```env
OPENAI_API_KEY=sk-...
```

---

## Exercice 3 — CI/CD GitHub Actions

Le workflow `.github/workflows/main.yml` se déclenche :
- à chaque `push` sur la branche `main` → installe les dépendances et lance les tests
- à chaque tag `v*` (ex: `v1.0.0`) → installe, teste **et** déploie

### Créer un tag et déclencher le déploiement

```bash
git tag v1.0.0
git push origin v1.0.0
```

### Configurer le secret

Dans **Settings → Secrets and variables → Actions** du repo GitHub, ajouter :

| Nom | Description |
|---|---|
| `OPENAI_API_KEY` | Clé API OpenAI utilisée au déploiement |

---

## Versioning

| Tag | Description |
|---|---|
| `v1.0.0` | Version initiale — Dockerisation + CI/CD opérationnel |