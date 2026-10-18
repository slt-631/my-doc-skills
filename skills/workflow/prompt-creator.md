---
name: prompt-creator
description: Expert prompt engineering pour créer des prompts efficaces pour Claude, GPT, et autres LLMs. À utiliser quand on écrit des system prompts, user prompts, few-shot examples, des prompts d'agents IA, ou qu'on veut optimiser un prompt existant pour de meilleures performances. Chaque prompt créé doit être clair, spécifique et optimisé pour le modèle cible.
---

# prompt-creator : Expert prompt engineering

## 🎯 Objectif

T'aider à créer des prompts efficaces pour n'importe quel LLM (Claude, GPT, Gemini, etc.), en appliquant les techniques pros du prompt engineering.

**Pourquoi :** Un bon prompt = la différence entre une IA qui te donne du générique et une IA qui te donne exactement ce que tu veux.

---

## 📋 Workflow en 7 étapes

### 1. Clarifier le PURPOSE

**Avant d'écrire quoi que ce soit, répondre :**

- Quel est l'objectif du prompt ?
- Quel comportement veux-tu obtenir ?
- Quel output précis ?
- Quel contexte d'usage (one-shot, workflow répété, agent) ?

**Questions à poser à l'utilisateur si pas clair :**
- "C'est pour une tâche unique ou un workflow répétitif ?"
- "Quel format d'output tu attends ?"
- "Quel ton/style ?"
- "Y a-t-il des contraintes (longueur, exclusions) ?"

---

### 2. Identifier le MODÈLE cible

**Les techniques varient selon le modèle :**

| Modèle | Spécificités |
|--------|--------------|
| **Claude (Anthropic)** | Adore les **XML tags** `<context>`, `<examples>`, `<instructions>`. Longue mémoire. Comprend bien le markdown. |
| **GPT (OpenAI)** | Préfère le **markdown structuré** (`##`, `-`). Bon avec les listes numérotées. |
| **Gemini (Google)** | Mix des deux. Bon avec les exemples concrets. |
| **Open-source (Llama, Mistral)** | Instructions directes, peu de meta. Éviter le trop abstrait. |

**Demander à l'utilisateur :** "Ce prompt est pour quel modèle ?"

---

### 3. Choisir les TECHNIQUES

**Selon la complexité :**

#### Simple (une tâche directe)
- Instructions claires
- Format d'output précis
- Un ou deux exemples

#### Medium (contexte à gérer)
- Instructions + contraintes
- 2-3 exemples (few-shot)
- Format XML/markdown structuré
- Règles d'exclusion ("NE PAS faire X")

#### Complex (reasoning, agents)
- Chain-of-thought ("réfléchis étape par étape")
- Decomposition de la tâche
- Validation steps ("vérifie que X avant de répondre")
- Edge cases explicites
- Rôle/persona fort

---

### 4. STRUCTURER le contenu

#### Pour Claude (XML)
```xml
<role>
Tu es un expert en [domaine]. Tu [spécificité].
</role>

<context>
[Contexte nécessaire : situation, données, historique]
</context>

<task>
[Ce que tu demandes de faire, précisément]
</task>

<constraints>
- [Contrainte 1]
- [Contrainte 2]
</constraints>

<examples>
<example>
<input>...</input>
<output>...</output>
</example>
</examples>

<output_format>
[Format précis attendu]
</output_format>
```

#### Pour GPT (Markdown)
```markdown
# Role
You are a [specialist] with expertise in [domain].

# Context
[Background information]

# Task
[What to do, specifically]

## Constraints
- [Constraint 1]
- [Constraint 2]

## Examples
### Example 1
**Input:** ...
**Output:** ...

## Output format
[Expected format]
```

---

### 5. Ajouter des EXEMPLES (few-shot)

**Quand c'est critique :**
- Format d'output précis
- Style/ton particulier
- Tâche avec plusieurs cas
- Classification ou extraction

**Règles d'or :**
- Au moins 2 exemples pour que le modèle comprenne le pattern
- Idéalement 3-5 exemples pour les tâches complexes
- Varier les cas (pas 3 fois le même)
- Inclure au moins un cas edge ou négatif

**Format exemple :**
```xml
<examples>
  <example>
    <input>Pizza margherita</input>
    <output>{"category": "food", "cuisine": "italian", "vegetarian": true}</output>
  </example>
  <example>
    <input>Steak frites</input>
    <output>{"category": "food", "cuisine": "french", "vegetarian": false}</output>
  </example>
</examples>
```

---

### 6. Définir le SUCCESS CRITERIA

**Un bon prompt dit explicitement :**
- Ce que l'output DOIT contenir
- Ce que l'output NE DOIT PAS contenir
- Le format exact
- La longueur attendue
- Comment valider soi-même avant de répondre

**Exemple :**
```markdown
## Success criteria

L'output est bon si :
- Il contient exactement 3 suggestions
- Chaque suggestion fait 1-2 phrases
- Il est en français
- Il n'invente pas de données non fournies

Avant de répondre, vérifie :
- [ ] 3 suggestions exactement
- [ ] Chaque suggestion < 50 mots
- [ ] Aucune donnée inventée
```

---

### 7. TESTER et ITÉRER

**Un prompt n'est jamais bon du premier coup.**

**Processus de test :**
1. Tester avec un cas simple
2. Tester avec un cas edge
3. Tester avec un cas qui doit échouer (vérifier le fallback)
4. Ajuster les instructions
5. Re-tester

**Signaux qu'un prompt est bon :**
- Output cohérent entre plusieurs runs
- Format respecté à chaque fois
- Le modèle ne "diverge" pas vers autre chose
- Les cas edge sont gérés proprement

**Signaux qu'un prompt est mauvais :**
- Outputs très variables
- Le modèle ignore les contraintes
- Il invente des choses
- Il refuse ou s'excuse trop

---

## 🎯 Templates prêts à utiliser

### Template 1 : Agent avec rôle fort

```xml
<role>
Tu es un [TITRE EXPERT]. Tu as [X ans / spécialité] en [DOMAINE].
Ton approche est [TRAITS : rigoureuse / créative / pragmatique].
</role>

<context>
L'utilisateur travaille sur [PROJET/SITUATION].
Tu as accès à [RESSOURCES/OUTILS].
</context>

<task>
Pour chaque [INPUT], tu dois :
1. [Action 1]
2. [Action 2]
3. [Action 3]
</task>

<rules>
- Toujours [règle positive]
- Jamais [règle négative]
- Si [cas edge], alors [comportement]
</rules>

<output_format>
Réponds en [FORMAT] avec :
- [Section 1]
- [Section 2]
</output_format>
```

### Template 2 : Extraction de données

```xml
<task>
Extrais les informations suivantes du texte fourni et retourne-les en JSON :

- field_1 : [description]
- field_2 : [description]
- field_3 : [description]
</task>

<examples>
<example>
<input>[Exemple de texte]</input>
<output>{"field_1": "...", "field_2": "...", "field_3": "..."}</output>
</example>
</examples>

<rules>
- Si une info n'est pas présente, mets null
- Ne devine pas, ne complète pas
- Retourne UNIQUEMENT le JSON, pas de texte autour
</rules>
```

### Template 3 : Classification

```xml
<task>
Classe le texte dans UNE des catégories suivantes :
- CATEGORY_A : [description]
- CATEGORY_B : [description]
- CATEGORY_C : [description]
- OTHER : si aucune ne colle
</task>

<examples>
<example>
<input>[Texte exemple A]</input>
<output>CATEGORY_A</output>
</example>
<example>
<input>[Texte exemple B]</input>
<output>CATEGORY_B</output>
</example>
</examples>

<output_format>
Retourne UNIQUEMENT le nom de la catégorie, en majuscules.
Pas de ponctuation, pas d'explication.
</output_format>
```

---

## 🚫 Anti-patterns à éviter

### ❌ Prompts vagues
**Mauvais :** "Fais-moi un bon résumé"
**Bon :** "Résume en 3 bullet points de 10-15 mots chacun, en français, en gardant les chiffres clés"

### ❌ Instructions contradictoires
**Mauvais :** "Sois concis mais détaille tout"
**Bon :** "Sois concis (max 100 mots). Si besoin de détail, propose un suivi."

### ❌ Trop de politesse
**Mauvais :** "Si ça ne te dérange pas, est-ce que tu pourrais peut-être essayer de..."
**Bon :** "Fais X. Si impossible, dis pourquoi."

### ❌ Pas de format spécifié
**Mauvais :** "Donne-moi des idées"
**Bon :** "Donne-moi 5 idées, formatées en liste numérotée, chacune en 1-2 phrases"

### ❌ Pas d'exemples pour les tâches format-sensibles
Si tu veux un JSON précis, DONNE UN EXEMPLE DE JSON. Le modèle ne devinera pas.

### ❌ Oublier les cas edge
**Mauvais :** "Extrais le nom de l'utilisateur"
**Bon :** "Extrais le nom de l'utilisateur. Si absent, retourne null. Si plusieurs noms, prends le premier mentionné."

---

## 📊 Checklist de qualité

Avant de livrer un prompt, vérifier :

- [ ] L'objectif est clair en 1 phrase
- [ ] Le rôle du modèle est défini
- [ ] Le format d'output est précisé
- [ ] Les contraintes sont listées (DO / DON'T)
- [ ] Au moins 2 exemples (si format critique)
- [ ] Les cas edge sont couverts
- [ ] La longueur attendue est spécifiée
- [ ] Les techniques matchent le modèle cible (XML pour Claude, markdown pour GPT)
- [ ] Testé sur au moins 3 cas différents

---

## 💡 Pour un dev qui commence avec les prompts

**Règles d'or :**

1. **Soit spécifique, pas poli.** Les IA ne sont pas vexées par la précision.
2. **Donne du contexte.** L'IA ne connaît pas ton projet.
3. **Montre, ne décris pas.** Un exemple vaut 100 mots d'instruction.
4. **Itère.** Ton premier prompt ne sera pas le meilleur.
5. **Lis l'output critique.** Si c'est pas ce que tu voulais, le prompt est responsable.

**Si tu bloques :** demande à l'IA de rédiger un prompt pour toi, puis affine-le.
