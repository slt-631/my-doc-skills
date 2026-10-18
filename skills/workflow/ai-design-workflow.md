# Workflow design assisté par IA

> Règle d'or : un seul écran d'abord. Le système vient après, extrait de ce qui marche.

```
Références → Fondations (1 page) → Écran clé en code → Critique → Itération → Généraliser
```

Quand tu te perds : regarde où tu en es dans cette liste, et ne saute pas à l'étape suivante tant que la checklist n'est pas cochée.

---

## 1. Direction (30 min max)

**But :** savoir à quoi ça doit ressembler, avant de produire quoi que ce soit.

- [ ] 5 à 10 références (sites, affiches, packagings, films) qui me parlent
- [ ] 3 à 5 adjectifs d'ambiance (ex. : éditorial, chaleureux, tranché)
- [ ] 2 à 3 anti-références : ce que je ne veux surtout pas
- [ ] Demander à l'IA de décrire chaque référence (typo, grille, palette, rythme) et d'en extraire 5 principes

**Sortie :** un paragraphe de direction + 5 principes.

---

## 2. Fondations minimales (1 page, 1 h max)

**But :** donner des garde-fous à l'IA. Pas un design system, juste de quoi rester cohérent.

- [ ] Palette : 1 fond, 1 texte, 1 accent, 1-2 neutres
- [ ] Typographie : 1 à 2 polices max + échelle de tailles
- [ ] Espacements : une échelle (ex. 4 / 8 / 16 / 24 / 48 / 96)
- [ ] Rayons et ombres : 2 valeurs chacun maximum
- [ ] Règles « jamais » (ex. : pas de dégradé violet, pas d'Inter par défaut)

**Sortie :** un `DESIGN.md` (ou variables CSS) d'une page.

---

## 3. Écran clé en code

**But :** voir le rendu réel le plus vite possible.

- [ ] Choisir **un seul** écran : le plus important (hero, accueil)
- [ ] Donner à l'IA : direction + fondations + contenu réel (pas du lorem ipsum)
- [ ] Générer directement en code (HTML/React), sans passer par Figma sauf besoin d'explorer
- [ ] Demander 2-3 directions vraiment différentes si je suis indécis

**Sortie :** un écran fonctionnel dans le navigateur, même imparfait.

---

## 4. Critique

**But :** transformer « je sens que c'est faux » en problèmes nommés.

- [ ] Faire un screenshot (desktop + mobile)
- [ ] Passer la grille : hiérarchie, contraste, rythme vertical, alignements, originalité
- [ ] Lister 3 à 5 problèmes précis, pas plus
- [ ] Demander à l'IA de critiquer aussi, puis comparer avec mon avis

**Sortie :** une liste courte de corrections.

---

## 5. Itération

**But :** boucler vite, sans dériver.

- [ ] Corriger les problèmes listés, un lot à la fois
- [ ] Screenshot → critique → correction (2 à 4 boucles suffisent)
- [ ] Polish manuel uniquement sur ce qui compte : micro-détails, motion, finitions
- [ ] Stop quand je suis fier du résultat, pas quand c'est « parfait »

**Sortie :** un écran dont je suis satisfait.

---

## 6. Généraliser (seulement maintenant)

**But :** transformer ce qui a marché en système réutilisable.

- [ ] Extraire les tokens réellement utilisés dans l'écran
- [ ] Isoler les composants qui reviennent (bouton, carte, titre, section)
- [ ] Mettre à jour `DESIGN.md` avec ce qui a été appris
- [ ] Décliner les autres écrans à partir de ce système
- [ ] Garder le tout comme base pour le projet suivant

**Sortie :** un mini design system issu du réel, et une signature qui se précise.

---

## Pièges à éviter

- **Sur-préparer** : trois jours de tokens avant de voir un écran. Un premier écran moche en 1 h t'apprend plus.
- **Figma par défaut** : si je code de toute façon, j'itère dans le navigateur.
- **Valider ce qui est « joli »** : toujours savoir dire pourquoi c'est bon ou mauvais.
- **Changer d'outil à chaque projet** : fixer un workflow, l'utiliser 3 fois, puis optimiser.
- **Esthétique IA générique** : dégradés, glassmorphism, cartes partout. La direction et les anti-références existent pour ça.

## Apprendre pour juger (1 h par semaine)

Pas pour produire, pour savoir corriger : typographie, couleur et contraste, composition et grille, bases d'UX. Décomposer un bon design par semaine vaut mieux que dix heures de Figma.
