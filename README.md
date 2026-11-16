# xsom-consulting/.github

Règles et briques communes à tous les dépôts de l'organisation. Le profil public est dans `profile/README.md`.

## Workflows réutilisables

Chaque dépôt les appelle au lieu d'en garder une copie ; une correction ici vaut pour tous.

| Fichier | Ce qu'il fait |
|---|---|
| `secrets.yml` | gitleaks sur tout l'historique ; caractères cachés dans `AGENTS.md` et `CLAUDE.md` (secret-guard) |
| `ci-python.yml` | ruff, pytest |
| `ci-node.yml` | pnpm : lint, typecheck, test (si le script existe), audit |
| `ci-go.yml` | vet, test, govulncheck |
| `revue-claude.yml` | relecture de la PR par Claude, un commentaire unique ; coûte des crédits API, secret `ANTHROPIC_API_KEY` |

Exemple, dans `.github/workflows/ci.yml` d'un dépôt :

```yaml
on: [pull_request, workflow_dispatch]
jobs:
  secrets:
    uses: xsom-consulting/.github/.github/workflows/secrets.yml@main
  python:
    uses: xsom-consulting/.github/.github/workflows/ci-python.yml@main
```

Sur un dépôt privé, chaque exécution consomme des minutes payantes : préférez `workflow_dispatch` seul tant que le projet est jeune.

## Modèles

- `template-projet-ia` : point de départ de tout nouveau projet (règles pour l'IA, CI, secrets).
- PR et tickets : `.github/PULL_REQUEST_TEMPLATE.md`, `.github/ISSUE_TEMPLATE/`, appliqués aux dépôts qui n'ont pas les leurs.

## Ranger les dépôts : groupes, topics, préfixes

GitHub n'a pas de sous-groupes. Un groupe, c'est trois choses : un topic (le filtre), un préfixe pour tout nouveau dépôt (GitHub trie par ordre alphabétique), et une section de la page d'accueil réservée aux membres (`.github-private`).

| Groupe (topic) | Préfixe des nouveaux dépôts | Dépôts au 16/11/2026 |
|---|---|---|
| `produit` | aucun : noms de marque | fleet, secret-guard, model-advisor |
| `agents` | `agent-` | xsom-agents, veille-collect, security, personal-agent, agent-linkedin |
| `sites` | `site-` | ai-website, site-xsom |
| `microservices` | `svc-` | aucun |
| `infrastructure` | `infra-` | .github |
| `templates` | `template-` | template-projet-ia |
| `data` | `data-` | aucun |
| `automation` | `auto-` | automatisations |

- Un groupe vide n'a pas de dépôt d'avance : il naît avec son premier dépôt.
- Les produits, surtout ceux publiés en open source, gardent leur nom de marque, sans préfixe.
- Plusieurs services livrés ensemble vont dans un seul dépôt `svc-…`, un dossier par service.
- Renommer un dépôt existant : seulement si aucun code ne l'importe par son nom (un module Go, par exemple). GitHub redirige l'ancien nom.

Filtre : `https://github.com/orgs/xsom-consulting/repositories?q=topic:agents`.
