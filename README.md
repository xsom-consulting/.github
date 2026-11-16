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

## Ranger les dépôts : les topics

GitHub n'a pas de sous-groupes. Chaque dépôt porte un topic qui dit ce qu'il est :

| Topic | Pour | Dépôts au 16/11/2026 |
|---|---|---|
| `produit` | ce que nous vendons | fleet, secret-guard, model-advisor |
| `site` | sites publiés | ai-website, infra-xsom_website |
| `agent` | agents, veille, sécurité automatisée | xsom-agents, veille-collect, personal-agent, security, infra-linkedin_llm_bot |
| `automatisation` | scripts sans IA | automatisations |
| `modele` | dépôts modèles | template-projet-ia |
| `interne` | règles et planification | .github, product-planning |
| `veille-oss` | copies de projets open source étudiés pour la veille concurrentielle | litellm, akto, promptfoo, presidio, ggshield, betterleaks, gitleaks, trufflehog, kong, gravitee-api-management, giskard-oss, llm-guard, skill-scanner, defenseclaw |
| `labo` | essais et prototypes, sans engagement de maintenance | aucun pour l'instant |

Filtre : `https://github.com/orgs/xsom-consulting/repositories?q=topic:produit`.
