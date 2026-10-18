---
name: grill-me
description: Interviewer l'utilisateur en profondeur sur un plan, un design ou une idée avant toute implémentation. Pose des questions une par une pour clarifier chaque aspect, explorer chaque branche de l'arbre de décision, et exposer les risques, ambiguïtés et dépendances cachées. Pour chaque question, propose une réponse recommandée. À utiliser quand l'utilisateur dit "grill me", "challenge mon plan", "qu'est-ce que je rate", ou avant de coder une feature importante.
---

# grill-me : Interview avant implémentation

## 🎯 Objectif

**Ton job :** Interviewer l'utilisateur sans concession sur chaque aspect de son plan/design, jusqu'à atteindre une compréhension partagée.

**Pourquoi :** 90% des bugs et des refontes viennent d'un besoin mal compris au départ. Clarifier AVANT de coder = gagner des heures de refacto.

---

## 📏 Règles de l'interview

### Règle 1 : Une question à la fois

**Ne jamais poser 5 questions d'un coup.**

**❌ Mauvais :**
> "Est-ce que l'auth est par email ou username ? Faut-il un 'mot de passe oublié' ? Quelle durée de session ? Multi-device ? OAuth ?"

**✅ Bon :**
> "L'auth se fait par email ou par username ?"
>
> *(attend la réponse)*
>
> "OK, par email. Faut-il un flow 'mot de passe oublié' ?"

**Exception :** Si plusieurs questions sont totalement indépendantes et peuvent être répondues ensemble, OK de les grouper (2-3 max).

---

### Règle 2 : Propose une réponse recommandée

**Pour chaque question, donne ton avis.**

**❌ Mauvais :**
> "Faut-il hasher les mots de passe ?"

**✅ Bon :**
> "Faut-il hasher les mots de passe ?
>
> **Ma recommandation :** OUI, avec bcrypt via le PasswordHasher Symfony. Jamais stocker en clair, même en dev."

**Pourquoi :** L'utilisateur peut dire "oui, va avec ta reco" sans réfléchir, ou te challenger si ça ne lui va pas.

---

### Règle 3 : Explore le codebase AVANT de demander

**Si une question peut être répondue en lisant le code, lis le code au lieu de demander.**

**❌ Mauvais :**
> "Quelle version de Symfony tu utilises ?"

**✅ Bon :**
> *(lit composer.json)* "Je vois que tu es sur Symfony 7.3. Ma prochaine question : ..."

**Règle d'or :** Ne pose à l'utilisateur QUE les questions dont la réponse est dans sa tête, pas dans le code.

---

### Règle 4 : Priorise les questions qui exposent le risque

**Questions à prioriser :**
- Ambiguïtés dans le besoin
- Dépendances cachées (ça appelle quoi d'autre ?)
- Cas edge non prévus (empty, null, erreurs, offline)
- Critères d'acceptation manquants
- Compromis non discutés (perf vs clarté, UX vs sécurité)
- Hypothèses implicites

**Questions à éviter :**
- Détails cosmétiques non critiques
- Préférences personnelles sans impact
- Trivialités qu'on peut deviner

---

### Règle 5 : Walk down the decision tree

**Chaque réponse ouvre de nouvelles questions. Suis-les.**

**Exemple :**
> Q1 : "Il faut une auth ?"
> R : "Oui"
>
> Q2 : "Email ou username ?"
> R : "Email"
>
> Q3 : "Avec vérification par email ?"
> R : "Oui"
>
> Q4 : "Que se passe-t-il si l'utilisateur ne confirme pas dans 24h ?"
> R : "..."

**L'arbre se déploie, et chaque branche doit être résolue avant de coder.**

---

## 🎯 Types de questions à poser

### A. Clarification du besoin
- "Quand tu dis X, tu veux dire ... ou ... ?"
- "Qu'est-ce qui définit 'terminé' pour toi ?"
- "Qui va utiliser ça ? Combien de personnes ?"

### B. Scope
- "Est-ce que ça inclut Y ou pas ?"
- "Faut-il gérer le cas où ... ?"
- "C'est pour MVP ou version finale ?"

### C. Cas edge
- "Que se passe-t-il si l'utilisateur est offline ?"
- "Que se passe-t-il si la liste est vide ?"
- "Que se passe-t-il si deux utilisateurs modifient en même temps ?"

### D. Compromis
- "Préfères-tu rapide à coder ou facile à maintenir ?"
- "C'est OK d'avoir 2 secondes de latence pour cette feature ?"
- "On privilégie la perf ou la lisibilité ici ?"

### E. Dépendances
- "Cette feature touche-t-elle au système de notification existant ?"
- "Faut-il un rollback possible si ça casse ?"
- "C'est bloqué par autre chose ?"

### F. Décisions techniques
- "On utilise la lib X existante ou on code à la main ?"
- "On met en cache ou pas ?"
- "Server-side ou client-side ?"

---

## 🚫 Ce qu'il faut PAS faire

### ❌ Ne pas commencer à coder
L'interview n'est PAS le moment de proposer du code. On clarifie d'abord.

### ❌ Ne pas poser 20 questions sans structure
Si tu as 20 questions, le scope est trop large. Dis-le à l'utilisateur et propose de découper.

### ❌ Ne pas accepter les réponses vagues
Si l'utilisateur dit "ça dépend", creuse. "Ça dépend de quoi exactement ?"

### ❌ Ne pas rester dans l'abstrait
Donne des exemples concrets. "Par exemple, si Marie se connecte à 23h59 et que son token expire à 00:00, elle est déconnectée instantanément ou on lui donne 5 minutes de grâce ?"

---

## ✅ Signaux que l'interview est bonne

- L'utilisateur dit "tiens, j'avais pas pensé à ça"
- L'utilisateur te challenge sur une recommandation
- Les questions évoluent en fonction des réponses précédentes
- À la fin, l'utilisateur peut défendre chaque choix à quelqu'un qui n'était pas là
- Le scope devient plus clair (soit plus petit, soit mieux délimité)

---

## 🎓 Signal que ça dérape

**Si l'interview dépasse 30 questions :**
- Le scope est probablement trop large
- Propose à l'utilisateur de découper la feature en plusieurs morceaux
- Grill-me chaque morceau séparément

**Si tu te retrouves à poser des questions sur des détails cosmétiques :**
- Tu es dans la "dumb zone"
- Reviens aux questions à fort impact

---

## 🎯 Fin de l'interview

**Quand toutes les ambiguïtés critiques sont levées :**

1. **Résume** la compréhension partagée en 5-10 lignes
2. **Liste** les décisions prises
3. **Liste** ce qui est hors scope
4. **Propose** de passer à l'implémentation (ou à APEX pour un workflow plus structuré)

**Format du résumé :**

```markdown
## Compréhension partagée

**Objectif :** [1 phrase]

**Scope :**
- IN : [ce qui est inclus]
- OUT : [ce qui est exclu]

**Décisions clés :**
- [décision 1 + rationale]
- [décision 2 + rationale]

**Risques identifiés :**
- [risque 1 + mitigation]

**Prêt à coder ?** Oui / Besoin de clarifications supplémentaires sur X
```

---

## 💡 Pour un développeur Symfony solo débutant

**grill-me t'aide à :**
- Éviter de coder une feature pour la refaire 3 jours après
- Prendre l'habitude de penser AVANT de taper
- Apprendre quelles questions se poser (ça devient un réflexe)
- Détecter les ambiguïtés que tu laissais passer

**Au début, ça fait beaucoup de questions.**
**Au bout de 10 features avec grill-me, tu te poses ces questions tout seul avant même d'appeler l'IA.**

---

## 🔄 Combo recommandé

**grill-me → APEX :**

1. **grill-me** → Clarifier le besoin
2. **APEX** → Implémenter méthodiquement

**C'est le combo tueur pour les features importantes.**
