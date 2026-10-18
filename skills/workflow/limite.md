---
name: limite
description: Évite de brûler ta limite d'usage Claude Code sur du travail facile. Installe deux sous-agents Haiku (Explore pour la recherche et la lecture de code, chores pour les éditions mécaniques et commandes simples) et une note dans CLAUDE.md qui route chaque tâche facile vers eux, pour que la session principale (Opus/Sonnet) ne garde que la planification, le debug et les changements multi-fichiers. À utiliser quand l'utilisateur dit "limite", atteint sa limite Claude trop vite, ou veut déléguer les tâches faciles à des modèles moins chers.
---

# limite : Déléguer les tâches faciles à Haiku

## 🎯 Objectif

Claude Code fait tourner chaque tâche sur le modèle de la session. Sur Opus, ça inclut trouver un fichier, lire un log, renommer un import — et chaque action consomme de la limite d'usage. Même l'agent `Explore` intégré hérite du modèle de session.

**Ce skill déplace le travail facile vers Haiku** (modèle plus léger et moins cher), pour garder Opus/Sonnet focus sur ce qui demande du jugement.

**Résultat :** Tu touches moins vite ta limite, et tu économises des tokens pour les tâches qui en valent la peine.

---

## 📋 Scope (où installer les agents)

### Deux options :

**Option A : Scope utilisateur (défaut)**
- Les agents vont dans `~/.claude/agents/`
- La note va dans `~/.claude/CLAUDE.md`
- **Actif pour TOUS tes projets Claude Code**

**Option B : Scope projet (`--project`)**
- Les agents vont dans `.claude/agents/` du projet courant
- La note va dans `./CLAUDE.md` à la racine du projet
- **Actif pour CE projet uniquement**

**Choix par défaut :** utilisateur. Si l'utilisateur dit "pour ce projet", utiliser projet.

---

## 📂 Fichiers à créer

Ce skill a besoin de 3 templates (à créer dans un sous-dossier `templates/` de ton skill) :

### Template 1 : `templates/Explore.md`

Remplace l'agent `Explore` intégré par une version qui tourne sur Haiku.

```markdown
---
name: Explore
description: Fast read-only search agent for locating code. Use it to find files by pattern, grep for symbols or keywords, or answer "where is X defined / which files reference Y." Runs on Haiku to save usage limit on easy search tasks.
model: haiku
tools: Read, Glob, Grep, WebFetch, WebSearch
---

# Explore Agent (Haiku-powered)

You are a fast, read-only search agent. Your job is to locate code quickly without consuming expensive model tokens.

## Rules

- You are READ-ONLY. Never edit, write, or delete files.
- You run on Haiku (cheap). Keep it that way.
- Use Glob for file patterns, Grep for content search, Read for inspection.
- Return concise, structured answers. Don't paraphrase code you can quote.
- If the task requires judgment (code review, design decisions, cross-file analysis), STOP and say so. That belongs to the main session.

## When invoked

1. Understand the search query.
2. Choose the fastest tool (Glob for filenames, Grep for content).
3. Return findings with file paths and line numbers.
4. Keep output under 200 words unless the user asked for more.

## Scope

- Finding files: YES
- Reading files: YES
- Grep for symbols: YES
- Explaining what code does: BRIEF (one sentence per file max)
- Reviewing code quality: NO (that's Opus/Sonnet territory)
- Suggesting changes: NO
- Writing or editing: NO
```

---

### Template 2 : `templates/chores.md`

Agent pour les éditions mécaniques (renommages, petits fixes).

```markdown
---
name: chores
description: Mechanical edits and simple commands. Use for renaming imports, fixing typos in a single file, running simple shell commands, formatting a file. Runs on Haiku to save usage limit. Stops itself when a task touches more than 2 files or needs a design decision.
model: haiku
tools: Read, Edit, Write, Bash, Glob, Grep
---

# Chores Agent (Haiku-powered)

You handle mechanical edits — the kind of task that needs no judgment.

## Rules

- You run on Haiku. Keep tasks simple.
- Max 2 files per task. If more, STOP and tell the main session.
- No design decisions. No refactoring. No "while I'm at it".
- One-line edits, renames, formatting, running commands : OK.
- Anything that needs judgment : STOP and defer.

## When invoked

1. Understand the exact mechanical change requested.
2. Do it in the fewest tool calls possible.
3. Report what was done in one sentence.

## OK to do

- Rename a variable in one file
- Fix a typo
- Add a missing import
- Run `npm install`, `composer install`, `git status`
- Format a file with Prettier/PHP-CS-Fixer
- Delete a dead file (if confirmed)
- Move a file (simple rename)

## NOT OK (defer to main session)

- Refactor a function
- Add a new feature
- Fix a bug that spans multiple files
- Make a design decision
- Review code quality
- Write complex logic
- Modify configuration files that affect behavior

## When to STOP

- The task touches more than 2 files
- You need to decide between multiple approaches
- The user's request is ambiguous
- You'd need to understand business logic to proceed

In those cases, say: "This needs the main session — too complex for a Haiku chore."
```

---

### Template 3 : `templates/CLAUDE-note.md`

Note à ajouter dans le CLAUDE.md pour router les tâches.

```markdown
## Models

**Delegate easy work to Haiku sub-agents to save usage limit on the main session.**

### Route to `Explore` sub-agent (Haiku) when :
- Finding a file by name or pattern
- Grepping for a symbol or keyword across the codebase
- Reading a file to answer "where is X defined"
- Listing files in a directory
- Any read-only search or inspection task

### Route to `chores` sub-agent (Haiku) when :
- Renaming a variable in a single file
- Fixing a typo
- Adding a missing import
- Running simple shell commands (`npm install`, `git status`, etc.)
- Formatting a file
- Any mechanical edit touching ≤ 2 files

### Keep on the main session (Opus/Sonnet) :
- Planning features
- Debugging complex bugs
- Reviewing code quality
- Multi-file refactoring
- Design decisions
- Writing complex logic
- Anything that needs judgment

The main session should orchestrate: delegate the easy work, keep the hard thinking.
```

---

## 🔧 Étapes d'installation

### Étape 1 : Lire les 3 templates

Lire `templates/Explore.md`, `templates/chores.md`, et `templates/CLAUDE-note.md` depuis le dossier du skill.

### Étape 2 : Pour chaque agent (Explore + chores)

Vérifier le dossier agents cible (le créer s'il n'existe pas) :

**Si le fichier n'existe pas :**
- Écrire le template.

**Si le fichier existe et est identique :**
- Skip.

**Si le fichier existe et est différent :**
- Afficher le diff.
- Demander à l'utilisateur avant d'écraser.
- **Ne JAMAIS écraser silencieusement.**

### Étape 3 : Lire `templates/CLAUDE-note.md`

Vérifier le `CLAUDE.md` cible :

**Si CLAUDE.md a déjà une section `## Models` (ou équivalent dans une autre langue) :**
- Laisser tel quel.
- Mentionner à l'utilisateur qu'une section existe déjà.

**Sinon :**
- Ajouter la section à la fin, traduite dans la langue du reste du CLAUDE.md.
- Créer le fichier s'il n'existe pas.

### Étape 4 : Rapport à l'utilisateur

Afficher :
```
✓ Installés :
  - ~/.claude/agents/Explore.md (ou chemin projet)
  - ~/.claude/agents/chores.md
  - ~/.claude/CLAUDE.md (section ## Models ajoutée)

Prochaines étapes :
  1. Redémarre Claude Code (ou tape /agents) pour charger les nouveaux agents.
  2. Vérifie avec /agents : Explore et chores doivent afficher "Haiku".
  3. La session principale reste sur Opus. Seul le travail délégué passe sur Haiku.
```

---

## 📝 Notes importantes

### Sur l'économie réelle

Anthropic ne publie pas combien chaque modèle "pèse" dans les limites d'abonnement. Ce qu'on sait :
- Opus consomme BEAUCOUP plus que Sonnet
- Sonnet consomme BEAUCOUP plus que Haiku

**Ne jamais promettre un chiffre précis** (genre "tu économises 50%"). Dire plutôt : "ça allège significativement le compteur sur les tâches de recherche et d'édition mécanique".

### Sur l'agent `Explore` intégré

Un agent utilisateur nommé `Explore` **remplace l'intégré**. C'est le but de cette installation : s'assurer que les recherches de code passent sur Haiku au lieu du modèle de session.

### Sur les limites de l'agent `chores`

Haiku est bon pour :
- Chercher
- Lire
- Éditer une ligne

Haiku n'est PAS bon pour :
- Jugement
- Design decisions
- Refactoring
- Debug complexe

**L'agent chores s'arrête tout seul** quand une tâche dépasse ses capacités (>2 fichiers ou décision de design). Ne pas essayer de forcer.

### Sur Cursor et autres IDEs

**Ce skill est SPÉCIFIQUE à Claude Code.** Il ne marche pas dans Cursor, Windsurf, ou autre.

Si l'utilisateur n'utilise pas Claude Code → signaler que ce skill ne s'applique pas et proposer d'économiser différemment (choix de modèle plus léger côté éditeur, ou désactivation de features coûteuses).

---

## 💡 Pour un dev qui commence Claude Code

**Pourquoi ce skill vaut le coup :**
- Tu vas rapidement voir ta limite d'usage descendre
- 70% du travail en session est de la recherche/édition simple
- Opus sur une recherche de fichier, c'est comme utiliser un tank pour aller chercher du pain

**Après installation :**
- La session principale (Opus/Sonnet) reste focus sur le hard thinking
- Les sous-agents Haiku font le travail de fourmi
- Tu peux coder plus longtemps avant de toucher ta limite

**Signal que ça fonctionne :**
- Tu vois `/agents` lister Explore et chores sur Haiku
- Tes recherches de fichiers sont ultra-rapides
- Tu te sens moins coupable de faire des recherches "au cas où"
