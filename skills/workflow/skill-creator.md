---
name: skill-creator
description: Créer, structurer et optimiser des agent skills (SKILL.md) pour Claude Code, Cursor, Codex, ou tout autre agent compatible. À utiliser quand on veut transformer un workflow répétitif en skill réutilisable, quand on veut documenter une méthodologie pour qu'une IA la suive, ou qu'on veut créer son propre skill from scratch à partir d'une idée.
---

# skill-creator : Créer tes propres skills

## 🎯 Objectif

T'aider à créer des **SKILL.md** propres, bien structurés, et efficaces — des skills que n'importe quel agent IA peut charger et suivre.

**Pourquoi :** Les skills que tu crées toi-même sont :
- 100% safe (tu sais ce qu'il y a dedans)
- Adaptés à TON contexte (projet, stack, niveau)
- Faciles à maintenir et faire évoluer
- Partageables (si tu veux)

---

## 📋 Qu'est-ce qu'un bon skill ?

### Un skill, c'est :
- Un fichier `SKILL.md` (markdown)
- Avec un **frontmatter YAML** (name + description)
- Qui donne des instructions claires à une IA
- Pour une tâche ou un workflow précis

### Un bon skill :
- ✅ Fait UNE chose, mais la fait bien
- ✅ A une description qui aide l'IA à savoir QUAND l'utiliser
- ✅ Est écrit comme si tu briefais un nouveau membre d'équipe
- ✅ Donne des exemples concrets
- ✅ Pose des règles claires (DO / DON'T)
- ✅ Est réutilisable sur plusieurs projets similaires

### Un mauvais skill :
- ❌ Fait 10 choses différentes
- ❌ A une description vague ("un skill utile")
- ❌ Est juste une liste de commandes sans contexte
- ❌ Oublie les cas edge
- ❌ Est trop spécifique (ne marche que pour UN projet)

---

## 🏗️ Structure obligatoire

### Le frontmatter YAML

**En haut de chaque SKILL.md :**

```yaml
---
name: nom-du-skill
description: Description courte (1-3 phrases) qui dit CE QUE le skill fait ET QUAND l'utiliser. L'IA lit ça pour décider si elle doit charger le skill.
---
```

**Règles pour le `name` :**
- `kebab-case` (minuscules, tirets)
- Court, descriptif
- Pas de mots génériques comme "helper", "util", "tool"
- Ex : `apex`, `fix-grammar`, `grill-me`

**Règles pour la `description` :**
- 1 à 3 phrases
- Commence par un verbe d'action ("Crée...", "Corrige...", "Génère...")
- Inclut les **triggers** : "À utiliser quand l'utilisateur dit X, veut Y, ou fait Z"
- Exemples de triggers : "quand l'utilisateur dit 'audit', veut optimiser, mentionne X"

**Pourquoi la description est critique :**
- C'est elle qui détermine QUAND l'IA invoque le skill
- Trop vague = jamais invoqué
- Trop précise = invoqué seulement dans un cas rare
- Trop long = l'IA ne lit pas tout

---

## 📏 Les 7 sections d'un skill solide

### 1. Titre + Objectif (TL;DR)
```markdown
# [nom] : [sous-titre court]

## 🎯 Objectif

[1 paragraphe : ce que fait le skill et pourquoi c'est utile]
```

### 2. Workflow / Processus
```markdown
## 📋 Workflow

### Étape 1 : [Action]
[Détails]

### Étape 2 : [Action]
[Détails]
```

### 3. Règles (DO / DON'T)
```markdown
## 📏 Règles

### À FAIRE ✅
- [règle 1]
- [règle 2]

### À NE PAS FAIRE ❌
- [règle 1]
- [règle 2]
```

### 4. Exemples concrets
```markdown
## 🎯 Exemples

### Exemple 1 : [Cas d'usage]
**Avant :**
[code / situation]

**Après :**
[code / situation]
```

### 5. Cas edge
```markdown
## 🚫 Cas edge

- Si [situation], alors [comportement]
- Si [situation 2], alors [comportement 2]
```

### 6. Quand utiliser (et quand NE PAS utiliser)
```markdown
## 💡 Quand utiliser

### ✅ Utiliser pour :
- [cas 1]
- [cas 2]

### ❌ Ne PAS utiliser pour :
- [anti-cas 1]
- [anti-cas 2]
```

### 7. Notes pour l'utilisateur (optionnel)
```markdown
## 🎓 Pour un [profil de l'utilisateur]

[Conseils spécifiques au niveau/contexte]
```

---

## 🎨 Exemple complet (minimal)

```markdown
---
name: commit-message
description: Génère un message de commit git conventionnel (type(scope): description) à partir d'un diff git. À utiliser quand l'utilisateur veut committer et demande un message, dit "commit this", ou mentionne qu'il veut respecter conventional commits.
---

# commit-message : Messages de commit conventionnels

## 🎯 Objectif

Transformer un diff git en un message de commit propre, suivant le standard conventional commits.

## 📋 Workflow

### Étape 1 : Lire le diff
Lancer `git diff --staged` et analyser les changements.

### Étape 2 : Identifier le type
- `feat:` nouvelle feature
- `fix:` correction de bug
- `docs:` documentation
- `style:` formatting (pas de changement de logique)
- `refactor:` refactoring (ni feature ni fix)
- `test:` ajout/modif de tests
- `chore:` tâches de maintenance

### Étape 3 : Identifier le scope (optionnel)
Le scope est la zone touchée : `auth`, `api`, `ui`, etc.

### Étape 4 : Rédiger la description
- Impératif présent ("add", "fix", pas "added", "fixing")
- Pas de majuscule au début
- Pas de point final
- Max 72 caractères

## 📏 Règles

### À FAIRE ✅
- Toujours vérifier `git diff --staged` d'abord
- Un commit = une raison
- Message en anglais (standard)

### À NE PAS FAIRE ❌
- Pas de message vague ("update", "fix stuff")
- Pas de scope inventé
- Pas de description de plus de 72 caractères

## 🎯 Exemples

### Exemple 1 : Nouvelle feature
**Diff :** Ajout du système de login par email
**Commit :** `feat(auth): add email login flow`

### Exemple 2 : Correction
**Diff :** Fix d'un bug sur la pagination
**Commit :** `fix(pagination): handle empty result set`

### Exemple 3 : Documentation
**Diff :** Ajout d'un README section
**Commit :** `docs(readme): add installation section`

## 💡 Quand utiliser

### ✅ Utiliser pour :
- Chaque commit git sérieux
- Projets avec conventional commits
- Repos partagés

### ❌ Ne PAS utiliser pour :
- WIP commits locaux (si tu squash après)
- Projets avec un autre standard (demander à l'utilisateur)
```

---

## 🔧 Processus de création d'un skill

### Étape 1 : Clarifier l'intention

**Demander à l'utilisateur :**
1. Quel workflow veux-tu automatiser ?
2. À quelle fréquence tu le fais ?
3. Quelles sont les étapes exactes ?
4. Qu'est-ce qui foire habituellement ?
5. Quels sont les cas edge que tu as rencontrés ?

### Étape 2 : Nommer et décrire

**Choisir :**
- Un `name` en kebab-case, court et descriptif
- Une `description` qui dit QUE + QUAND, avec des triggers

**Tester la description :**
Si tu lis juste la description, est-ce que tu sais :
- Ce que fait le skill ?
- Quand l'IA doit l'invoquer ?

Si non, retravailler.

### Étape 3 : Structurer

**Appliquer les 7 sections (ou un sous-ensemble si petit skill).**

**Règle :** si une section est vide ou n'ajoute rien, la skip.

### Étape 4 : Donner des exemples RÉELS

**Pas d'exemples bidons.** Prends des cas que l'utilisateur a vraiment vécus.

**Un exemple vaut 10 instructions.**

### Étape 5 : Challenger

**Demander :**
- Est-ce qu'un dev qui n'a jamais fait ce workflow comprend ?
- Est-ce que les règles couvrent les cas edge ?
- Est-ce que les exemples sont parlants ?
- Est-ce que la description déclenchera le skill au bon moment ?

### Étape 6 : Tester le skill

**En conditions réelles :**
- Charge le skill dans ton agent
- Donne-lui une tâche qui devrait déclencher le skill
- Observe si l'IA suit les instructions
- Ajuste ce qui ne marche pas

---

## 📁 Où placer les skills

### Pour Claude Code (global)
```
~/.claude/skills/[nom-du-skill]/SKILL.md
```

### Pour Claude Code (projet)
```
.claude/skills/[nom-du-skill]/SKILL.md
```

### Pour Cursor / IDE
```
.skills/[nom-du-skill].md
```
(puis référencé dans `.cursorrules`)

### Pour un workflow partagé
```
~/skills-perso/[catégorie]/[nom-du-skill].md
```

**Conseil :** Dans un projet, organiser par catégorie :
```
.skills/
├── universal/       # Toujours actif
├── workflow/        # Workflows (APEX, grill-me)
├── writing/         # Grammaire, prompts
├── design-code/     # Skills design pour coder
└── meta/            # Skills qui créent/gèrent d'autres skills
```

---

## 🚫 Erreurs classiques à éviter

### ❌ Faire un skill "fourre-tout"
Un skill qui fait 5 choses différentes = un skill que l'IA ne saura pas quand utiliser.
**Solution :** Découper en 5 petits skills.

### ❌ Oublier les triggers dans la description
"Un skill pour le dev" → l'IA ne saura jamais quand l'invoquer.
**Solution :** "À utiliser quand l'utilisateur dit X, veut Y, ou mentionne Z."

### ❌ Trop de meta-blabla
Des sections "Introduction", "Philosophy", "Why this skill exists" sur 200 lignes.
**Solution :** Aller droit au but. L'IA (et l'utilisateur) veulent savoir COMMENT faire, pas POURQUOI tu as créé le skill.

### ❌ Zero exemple
L'IA interprétera à sa sauce.
**Solution :** Toujours au moins 1-2 exemples concrets.

### ❌ Trop générique
Un skill qui marche pour tout = un skill qui marche pour rien.
**Solution :** Optimiser pour un contexte précis. Faire une variante si besoin.

### ❌ Pas de limite de scope
"Ce skill fait aussi X, Y, Z au passage"
**Solution :** Scope strict. Si le skill déborde, créer un autre skill.

---

## 📊 Checklist finale avant de livrer un skill

- [ ] Le `name` est en kebab-case, court, descriptif
- [ ] La `description` dit QUE + QUAND, avec triggers
- [ ] L'objectif est clair en 1 paragraphe
- [ ] Il y a au moins 1-2 exemples concrets
- [ ] Les règles DO / DON'T sont listées
- [ ] Les cas edge sont couverts
- [ ] Le "quand utiliser / ne pas utiliser" est clair
- [ ] Le skill fait UNE chose bien (pas 10)
- [ ] Testé sur au moins 2 cas différents
- [ ] Placé au bon endroit (`.claude/skills/` ou `.skills/`)

---

## 💡 Pour un dev qui débute avec les skills

**Commence petit :**
- Ton premier skill devrait faire UNE action simple
- Exemple : "generate-commit-message", "fix-twig-syntax"

**Évolue :**
- Après 5-10 skills, tu sauras structurer plus vite
- Tu commenceras à voir des patterns réutilisables

**Partage :**
- Si un skill marche bien, versionne-le dans Git
- Tu peux le partager avec d'autres devs
- Tu peux l'améliorer au fil du temps

**Ne surcharge pas :**
- Mieux vaut 10 bons skills que 50 skills moyens
- Si tu ne te souviens plus pourquoi tu as créé un skill, c'est qu'il faut le supprimer
