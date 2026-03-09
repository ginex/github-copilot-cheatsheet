# GitHub Copilot – Aide‑mémoire (mars 2026)

## Personnalisation des instructions

Texte ajouté automatiquement au prompt de chaque requête pour orienter le comportement de Copilot  
(conventions de code, technologies, règles métier, architecture…).  
Évite de répéter le contexte à chaque conversation.

### Instructions de dépôt
- **Globales** : `.github/copilot-instructions.md` — injectées dans chaque requête, pour tout le dépôt.
- **Ciblées** : `.github/instructions/*.instructions.md` — instructions appliquées sélectivement via `applyTo` (glob patterns comme `applyTo: "**/*.ts"`).

### Fichier `AGENTS.md`
Définit un ou plusieurs **agents personnalisés** (ex. : *security-reviewer*, *api-designer*, *migration-agent*).  
Chargé comme **contexte système** lors de l’exécution du mode Agent.  
Peut être décliné par sous-dossiers pour un comportement spécifique.

### Instructions personnelles
Sur GitHub.com — définissent des préférences personnelles permanentes (style de code, technologies, format de réponses).  
Appliquées dans n’importe quel dépôt ou IDE.

### Instructions d’organisation
Définissent des règles globales appliquées à tous les projets d’une organisation.

### Copilot Memory
Copilot peut mémoriser certaines préférences récurrentes de l’utilisateur pour les réutiliser automatiquement.  
Complète les Instructions personnelles sans les remplacer.

### Limitations
- N’affectent **pas** les suggestions inline, uniquement le **chat** et le **mode Agent**.  
- Les instructions très longues peuvent être partiellement ignorées si la fenêtre contextuelle est saturée.  
- `.copilotignore` filtre surtout le chat, et son support en mode Agent dépend de l’éditeur (meilleur dans VS Code Insiders).  

---

## Commandes, Prompt Files, Skills & Hooks

### Commandes personnalisées (Prompt Files)
Fichiers dans `.github/prompts/*.prompt.md`.  
Permettent :
- d’invoquer un prompt via `/nom-commande`,
- de définir modèle / agent / outils / MCP,
- d'afficher un hint dans le chat.

### Skills
Extensions de capacités pour les agents.  
Dossier contenant un `.md` + scripts, données ou templates.  
Chargées automatiquement lorsque pertinentes pour la tâche de l’utilisateur.

### Hooks
Fichiers JSON dans `hooks/*.json`.  
Permettent d’intercepter les étapes du mode Agent :  
- `pre-plan`, `post-plan`  
- `pre-apply`, `post-apply`  
- `pre-tool`, `post-tool`  
Utilisés pour :
- imposer des règles (garde-fous),
- déclencher validations,
- automatiser workflows (tests, PR, vérifications).

---

## Modes de chat (mises à jour 2026)

| Mode / Agent | Description |
|--------------|-------------|
| **Ask** | Questions, exploration, explications. Lecture seule. |
| **Edit** | Modifications unitaires sur un ou plusieurs fichiers (avec validation). |
| **Plan** | *Nouveau* : création d’un plan structuré avant toute modification. Recherche dans le codebase, analyse, questions de clarification. **Aucun changement sans validation.** |
| **Agent** | Exécution autonome : modifications multi-fichiers, lancement de commandes, tests, création de PR. Adapté aux tâches complexes. |

---

## Sources automatiques de contexte

Copilot utilise automatiquement :
- le fichier actif,  
- les fichiers ouverts,  
- la sélection,  
- la structure du dossier,  
- les symboles du projet,  
- le diff Git en cours,  
- l’historique du chat,  
- les instructions (personnelles, dépôt, organisation),  
- les agents (`AGENTS.md`), skills et hooks,  
- les règles `.copilotignore` (selon l’éditeur).

---

## Variables de contexte (Chat)

Utilisables via `#variable`.

| Variable | Description |
|----------|-------------|
| `#file` | Inclure un fichier précis |
| `#selection` | Limiter au texte sélectionné |
| `#codebase` | Recherche sémantique sur tout le code |
| `#terminal` | Inclure la sortie du terminal |
| `#problems` | Inclure erreurs/avertissements |
| `#sym` | Fournir un symbole précis (fonction, classe…) |

---

## Sélection du modèle (2026)

| Modèle | Usage recommandé |
|--------|------------------|
| **GPT‑4o** | Modèle général supérieur. Très bon équilibre vitesse/qualité. |
| **GPT‑4o mini** | Tâches simples, fixes rapides, renommages. |
| **GPT‑4.1** | Raisonnement plus stable et contextes plus longs. |
| **Claude 3 Sonnet** | Raisonnement complexe, refactoring profond. |
| **Claude 3 Haiku** | Rapide, léger, bon pour tâches courantes. |
| **Claude 3 Opus** | Spécifications longues, architecture, audit de code. |
| **Gemini 1.5 Pro** | Contextes très longs, analyse multi‑fichier. |
| **o1 / o3‑mini** | Raisonnement étape‑par‑étape très structuré. Peut ignorer style. |

---

## Intégrations & Outils

### MCP (Model Context Protocol)
Standard permettant aux modèles d’interagir avec :
- bases de données,
- APIs internes,
- systèmes de fichiers,
- outils maison.

### Copilot Extensions
Plugins Marketplace (Jira, Docker, Sentry…).  
Différent de MCP : packagé et distribué comme extension.

### Copilot CLI
Disponible via `gh copilot` :
- `gh copilot suggest` — suggère commandes shell  
- `gh copilot explain` — explique une commande  

### Revue automatique des PR
Copilot peut analyser une PR et commenter automatiquement.

### Prompts alternatifs (expérimental)
VS Code permet d’utiliser des variantes du prompt système.

---

## Sécurité & Confidentialité

### Exclusion de fichiers
- `.copilotignore` — exclure fichiers/dossiers du contexte transmis au modèle  
  (syntaxe identique à `.gitignore`)
- Paramétrable via VS Code / politiques d’organisation.

### Bonnes pratiques
- Ne jamais exposer secrets ou credentials.  
- Utiliser hooks + skills pour imposer des règles.  
- Maintenir une séparation claire entre planification et exécution.  

---

## Ressources communautaires

### Awesome Copilot
Dépôt communautaire contenant :
- instructions,  
- agents,  
- skills,  
- hooks,  
- prompt files,  
- exemples et best practices.

---

## Prompt Engineering

- **Questions / Réponses** : demander une liste de questions oui/non pour affiner.  
- **Pour & contre** : obtenir plusieurs solutions avec comparaison.  
- **Étape par étape** : forcer le raisonnement structuré.  
- **Rôle** : *"Agis comme un architecte senior..."*  
- **Auto‑correction** : analyser une conversation dans une nouvelle session propre.