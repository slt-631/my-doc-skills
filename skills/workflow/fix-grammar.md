---
name: fix-grammar
description: Corrige la grammaire et l'orthographe dans un ou plusieurs fichiers en préservant le formatting (markdown, HTML, code, YAML frontmatter). Travaille en français et en anglais. À utiliser quand l'utilisateur demande de corriger la grammaire d'un fichier, d'un README, d'un article, de documentation, ou de contenu rédactionnel.
---

# fix-grammar : Correction grammaire & orthographe

## 🎯 Objectif

Corriger les erreurs de grammaire, d'orthographe et de syntaxe dans un ou plusieurs fichiers **sans toucher au formatting**, au code, ou à la structure.

**Pourquoi :** Tu peux relire ta prose sans avoir à la réécrire, et sans risque de casser le markdown ou le HTML.

---

## 📋 Workflow

### Étape 1 : Parse les arguments

**L'utilisateur peut passer :**
- Un fichier unique : `fix-grammar README.md`
- Plusieurs fichiers : `fix-grammar README.md CONTRIBUTING.md`
- Un dossier : `fix-grammar docs/`
- Un pattern : `fix-grammar "docs/*.md"`

**Si aucun fichier n'est spécifié → ARRÊTER et demander le chemin.**

### Étape 2 : Vérifier les fichiers

**Pour chaque fichier :**
- Vérifier qu'il existe
- Vérifier qu'il est lisible (pas binaire)
- Détecter la langue (français / anglais / autre)

**Si plusieurs fichiers :** Les traiter un par un (séquentiellement).

### Étape 3 : Lire et analyser

**Lire le fichier et identifier :**
- Les erreurs d'orthographe
- Les erreurs de grammaire (accords, conjugaisons)
- Les erreurs de syntaxe
- Les erreurs de ponctuation
- Les anglicismes mal utilisés (si français)
- Les faux-amis (si anglais ↔ français)
- Les répétitions évidentes

### Étape 4 : Appliquer les corrections

**Utiliser l'outil Edit (pas Write) pour ne modifier QUE les erreurs.**

**Après correction, afficher :**
```
✓ Fixed grammar in [filename]
  - X corrections appliquées
```

---

## 🛡️ Règles de PRÉSERVATION

### À NE PAS modifier :

1. **Le code dans les blocs ```
   ```php
   // Ce code ne doit JAMAIS être modifié
   $user->getName();
   ```

2. **Le code inline** entre backticks : `const foo = 'bar'`

3. **Les URLs** : `https://example.com/path?query=value`

4. **Les noms de fichiers** : `src/Controller/UserController.php`

5. **Le frontmatter YAML** (en haut du fichier)
   ```yaml
   ---
   title: Mon article
   date: 2026-01-01
   ---
   ```

6. **Les commandes shell** : `npm install`, `git commit`

7. **Les balises HTML** : `<div class="foo">`

8. **Les noms propres** : noms de personnes, marques, projets

9. **Les termes techniques** établis : "framework", "bundle", "commit"

10. **Le ton et le style** de l'utilisateur (sauf erreur manifeste)

### À modifier :

1. **Orthographe** : "recomander" → "recommander"
2. **Accords** : "les variable sont déclarées" → "les variables sont déclarées"
3. **Conjugaisons** : "il travail" → "il travaille"
4. **Ponctuation** : espaces manquants, virgules oubliées
5. **Syntaxe** : phrases mal construites
6. **Majuscules** : début de phrase, noms propres

---

## 🎯 Exemples concrets

### Exemple 1 : Markdown
**Avant :**
```markdown
# Mon projet

Ce projet utilise Symfony 7 pour géré les requête HTTP.
Il faut install les dépendance avec `composer install`.
```

**Après :**
```markdown
# Mon projet

Ce projet utilise Symfony 7 pour gérer les requêtes HTTP.
Il faut installer les dépendances avec `composer install`.
```

**Note :** La commande `composer install` est préservée.

---

### Exemple 2 : README avec frontmatter
**Avant :**
```markdown
---
title: Guide d'installation
date: 2026-01-01
---

# Installation

Pour installé le projet, cloner le repo puis lance les commande suivante:
```

**Après :**
```markdown
---
title: Guide d'installation
date: 2026-01-01
---

# Installation

Pour installer le projet, clone le repo puis lance les commandes suivantes :
```

**Note :** Le frontmatter YAML n'est pas touché. L'espace avant `:` est ajouté (règle française).

---

### Exemple 3 : Préservation de code
**Avant :**
```markdown
Utiliser la méthode `getUser()` pour recupere l'utilisateur courant.

```php
public function getUser(): User
{
    // Ce commentaire a une faute : recupere
    return $this->user;
}
```
```

**Après :**
```markdown
Utiliser la méthode `getUser()` pour récupérer l'utilisateur courant.

```php
public function getUser(): User
{
    // Ce commentaire a une faute : recupere
    return $this->user;
}
```
```

**Note :** La prose est corrigée, mais le code (y compris le commentaire fautif) n'est PAS modifié car il est dans un bloc code.

---

## 📏 Règles typographiques

### Pour le FRANÇAIS :

- Espace **avant** `:` `;` `!` `?` `»` (espace insécable idéalement)
- Pas d'espace **avant** `.` `,`
- Espace **après** `.` `,` `:` `;` `!` `?`
- Guillemets français `« ... »` (pas `"..."`)
- Apostrophe courbe `'` (pas droite `'`)
- Tiret cadratin `—` pour les incises
- Majuscules accentuées : `À`, `É`, `È` (pas `A`, `E`)

### Pour l'ANGLAIS :

- Pas d'espace avant `:` `;` `!` `?`
- Espace après la ponctuation
- Guillemets anglais `"..."` ou `'...'`
- Pas d'accents (sauf noms propres étrangers)
- Oxford comma : optionnel (respecter le style existant)

---

## 🚫 Ce qu'il faut PAS faire

### ❌ Ne pas réécrire le style de l'utilisateur
Si l'utilisateur écrit "ouais, c'est cool", NE PAS transformer en "effectivement, c'est intéressant".

### ❌ Ne pas changer le ton
Casual reste casual. Formel reste formel.

### ❌ Ne pas "améliorer" sans erreur
Si une phrase est grammaticalement correcte mais pas optimale, LAISSER TELLE QUELLE.

### ❌ Ne pas modifier le code
Les blocs ``` et le code inline sont SACRÉS.

### ❌ Ne pas inventer du contenu
Si une phrase est incomplète, demander à l'utilisateur, pas inventer la fin.

---

## 📊 Rapport final

**Après traitement, afficher :**

```markdown
## Corrections appliquées

### [file1.md]
- X corrections :
  - Ligne 5 : "recomander" → "recommander"
  - Ligne 12 : "les variable" → "les variables"
  - Ligne 23 : espace avant `:` ajouté

### [file2.md]
- Y corrections :
  - ...

### Total : [X] corrections dans [N] fichiers
```

---

## 💡 Cas d'usage pour un dev Symfony

### ✅ Utile pour :
- `README.md` du projet
- Documentation (`docs/`)
- Commentaires de PR (si c'est un fichier)
- Articles de blog
- Templates Twig avec du contenu éditorial
- Fichiers de traduction (`translations/*.yaml`)

### ⚠️ Précaution pour :
- **Fichiers de traduction** : vérifier que les clés techniques ne sont pas modifiées
- **Templates Twig** : préserver `{{ }}` et `{% %}`
- **Documentation API** : préserver les noms de méthodes et classes

### ❌ Pas utile pour :
- Fichiers de code (`.php`, `.js`, `.ts`)
- Fichiers de config (`.yaml`, `.json`)
- Fichiers binaires
