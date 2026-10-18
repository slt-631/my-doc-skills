---
name: apex
description: Workflow structuré d'implémentation suivant la méthodologie APEX (Analyze-Plan-Execute-eXamine). Permet de développer une feature de bout en bout de manière méthodique avec validation à chaque étape. À utiliser quand on implémente une nouvelle feature, qu'on corrige un bug important, ou qu'on fait un changement qui bénéficie d'un workflow structuré plutôt qu'un code rapide.
---

# APEX : Workflow d'implémentation méthodique

## 🎯 Objectif

Transformer une tâche de développement en workflow structuré en 4 étapes claires :

- **A**nalyze → comprendre
- **P**lan → planifier
- **E**xecute → implémenter
- **e**X\*\*amine → vérifier

**Pourquoi :** Éviter de coder "au hasard" et obtenir un résultat fiable, testé et validé.

---

## 📋 Les 4 étapes OBLIGATOIRES

### 1. ANALYZE (Analyser)

**Avant de toucher au code, comprendre le contexte.**

**Actions à faire :**

- Lire les fichiers concernés (controllers, entités, templates, services)
- Identifier les dépendances (quels fichiers appellent quoi)
- Comprendre le pattern existant du projet
- Identifier les critères d'acceptation (qu'est-ce qui définit "feature terminée" ?)
- Noter les points d'ambiguïté

**Output attendu :**

**Règle d'or :** Si quelque chose n'est pas clair, DEMANDER avant de coder.

---

### 2. PLAN (Planifier)

**Créer une stratégie file-by-file avant de coder.**

**Actions à faire :**

- Lister tous les fichiers à créer ou modifier
- Pour chaque fichier, décrire ce qu'il va contenir
- Identifier l'ordre d'implémentation (dépendances)
- Mapper chaque fichier à un critère d'acceptation
- Prévoir les tests manuels à effectuer

**Output attendu :**

**Règle d'or :** Si le plan touche >5 fichiers, c'est peut-être trop gros. Découper.

---

### 3. EXECUTE (Exécuter)

**Implémenter en suivant le plan, un fichier à la fois.**

**Actions à faire :**

- Suivre l'ordre défini dans le plan
- Un commit par étape logique (pas tout en bloc)
- Si un problème non prévu apparaît → NOTER et continuer (sauf blocant)
- Ne PAS faire de refactoring parasite (reste focus sur la tâche)

**Règles d'or :**

- ❌ Pas de "// TODO" dans le code livré
- ❌ Pas de code commenté qui traîne
- ❌ Pas de "pendant que j'y suis, je refactore X"
- ✅ Garder le scope initial

**Output attendu après exécution :**

---

### 4. EXAMINE (Examiner)

**Vérifier que tout fonctionne et respecte les critères.**

**Actions à faire :**

**A. Validation technique :**

- `php bin/console cache:clear` (pas d'erreur)
- `php bin/console doctrine:schema:validate` (si entités modifiées)
- `php bin/console lint:twig templates/` (si templates modifiés)

**B. Validation fonctionnelle :**

- Lancer le serveur : `symfony server:start`
- Tester CHAQUE critère d'acceptation du plan
- Vérifier les cas d'erreur

**C. Review critique (self-check) :**

- Le code respecte-t-il les conventions Symfony ?
- Y a-t-il des problèmes de sécurité évidents ?
- Les messages d'erreur sont-ils clairs ?
- Est-ce que ça gère les cas edge (empty, null, erreurs réseau) ?

**D. Nettoyage final :**

- Supprimer tout code mort
- Supprimer les `dump()` et `var_dump()`
- Vérifier qu'aucun secret n'est en dur

**Output attendu :**

---

## 🎯 Workflow résumé

---

## 📏 Règles universelles

### À FAIRE ✅

- Lire le code existant AVANT de modifier
- Noter chaque critère d'acceptation
- Un commit par étape logique
- Tester manuellement après chaque phase

### À NE PAS FAIRE ❌

- Pas de "je commence à coder puis je verrai"
- Pas de refactoring parasite (reste focus)
- Pas de "// TODO" dans le code livré
- Pas de "je skip la validation, ça marche sûrement"

---

## 💡 Quand utiliser APEX

### ✅ Utiliser APEX pour :

- Nouvelle feature (login, dashboard, etc.)
- Refonte d'une partie du code
- Bug complexe qui touche plusieurs fichiers
- Intégration d'une nouvelle lib (Doctrine, Symfony Bundle)
- Setup d'une partie importante du projet

### ❌ Ne PAS utiliser APEX pour :

- Changement cosmétique (couleur, texte)
- Correction d'une typo
- Ajout d'un champ unique
- Debug rapide d'une erreur évidente

**Règle :** Si ça prend >15 min, APEX vaut le coup.

---

## 🎓 Pour toi (débutant Symfony)

**APEX t'aide à :**

- Ne pas te perdre dans un gros projet
- Développer méthodiquement
- Apprendre en structurant ta pensée
- Éviter les erreurs classiques du "je code puis je verrai"

**Au début, c'est long. Après 5-10 utilisations, ça devient naturel.** 💪
