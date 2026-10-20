# Mode Rat : économiser les tokens sans sacrifier le résultat

> Instructions pour tout agent IA (Claude Code, Codex, Cursor, Gemini CLI, etc.).
> À placer dans le fichier d'instructions de l'agent (AGENTS.md, CLAUDE.md, .cursorrules…) ou à coller en début de session.

## Activation

Ce mode s'active uniquement quand l'utilisateur tape `/rat` ou écrit « mode rat ». Il reste actif pour la tâche demandée, puis l'agent revient à son fonctionnement normal.

## Règle d'or

**La qualité passe d'abord.** Économise partout où cela ne dégrade pas le résultat, jamais au prix d'une tâche inachevée ou d'un livrable moins bon. Dans le doute, choisis la qualité.

## 1. Cadrage

- Si des points sont flous, pose **toutes tes questions en une seule fois** au début, pour éviter les allers-retours.
- Si la demande est claire, ne demande rien et commence directement.

## 2. Pendant le travail

- **Aucune narration entre les étapes** : pas de « je vais maintenant… », pas de commentaire sur chaque résultat d'outil ou de commande.
- Recherches et lecture de fichiers : **pas de limite imposée**. Cherche et lis autant que la qualité l'exige.
- Modifications de fichiers ou de code : **modifications ciblées** (uniquement les lignes concernées, en diff ou en édition partielle). Réécris le fichier entier seulement si environ la moitié ou plus change.
- Ne recopie pas dans ta réponse un contenu déjà écrit dans un fichier.

## 3. Délégation à des modèles moins chers (si disponible)

Si tu as accès à OpenRouter ou à un autre moyen d'appeler des modèles moins coûteux, délègue une sous-tâche **quand c'est rentable** : sortie volumineuse, peu critique, que tu n'as pas besoin de relire en détail (traduction en masse, tri, classification, reformulation, brouillons, variantes).

Ne délègue pas : le raisonnement central, les décisions, ce qui exige la meilleure qualité.

Procédure :
1. Choisis le modèle : **gratuits d'abord** (identifiants OpenRouter en `:free`) s'ils suffisent ; sinon le modèle payant le moins cher capable de faire la tâche.
2. Envoie un prompt court et précis, en demandant une **sortie compacte et structurée** (JSON, liste, texte sans préambule).
3. Pour les gros volumes, traite par lots et écris les résultats dans un fichier plutôt que de tout relire dans ton contexte.
4. Contrôle un échantillon de la sortie. Si la qualité est insuffisante, passe à un modèle plus capable.
5. Pas de plafond de dépense, mais reste raisonnable.

Sans accès à d'autres modèles, ignore cette section.

## 4. Vérification

Vérification **légère et ciblée** sur les points à risque : chiffres, calculs, code qui doit tourner (tests ou exécution rapide), sorties des modèles délégués. Pas de relecture intégrale.

## 5. Livraison

- Style **court mais lisible** : phrases complètes, uniquement l'essentiel. Pas d'introduction, pas de récapitulatif des étapes.
- Donne le livrable, puis, si une délégation a eu lieu, **une seule ligne** avec son coût, par exemple : « Coût OpenRouter : 0,012 $ (modèle X) ».
