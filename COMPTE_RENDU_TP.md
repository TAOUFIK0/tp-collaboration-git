# Compte rendu du TP : collaborer sur un dépôt Git

**Date :** 20 octobre 2026  
**Auteur / pseudo GitHub :** TAOUFIK0  
**Dépôt :** [TAOUFIK0/tp-collaboration-git](https://github.com/TAOUFIK0/tp-collaboration-git)  
**Environnement :** Git Bash sous Windows  
**Plateforme :** GitHub

> **Note importante :** les éléments qui dépendent de l'exécution réelle du TP (noms des étudiants, branches de fonctionnalité, commits, Pull Requests fusionnées, conflit et tag) doivent être vérifiés et complétés avec les informations et captures réellement obtenues.

## 1. Introduction

Ce TP a pour objectif de simuler un travail d'équipe avec Git et GitHub. Le travail consiste à créer un dépôt, protéger sa branche principale, développer sur des branches séparées, provoquer puis résoudre un conflit de fusion, et livrer une version taguée `v1.0.0`.

### Objectifs

- Créer un dépôt Git local et le publier sur GitHub.
- Protéger la branche `main` avec une Pull Request obligatoire.
- Interdire les push directs sur `main`.
- Travailler avec des branches de fonctionnalité `feature/*`.
- Créer, lire et résoudre un conflit de fusion.
- Nettoyer les branches et créer le tag `v1.0.0`.

### Informations générales

| Élément | Valeur |
|---|---|
| Étudiant A | TAOUFIK RAHIOUI |
| Étudiant B | MOKHLISS HIMMICHE  |
| Dépôt | https://github.com/TAOUFIK0/tp-collaboration-git |
| Branche principale | `main` |
| Date | 20 octobre 2026 |
| Pull Request finale | `[À compléter avec le lien réel]` |



## 2. Préparer le dépôt

L'étudiant A crée le projet local, l'initialise avec Git, puis le connecte au dépôt GitHub.

```bash
mkdir tp-collaboration-git
cd tp-collaboration-git
git init

echo "# TP Collaboration Git" > README.md
echo "node_modules/" > .gitignore
echo "*.log" >> .gitignore

git status
git add .
git commit -m "Initialisation du projet"

git remote add origin https://github.com/TAOUFIK0/tp-collaboration-git.git
git remote -v
git branch -M main
git push -u origin main
```

Le dépôt doit contenir un `README.md` et un `.gitignore` excluant `node_modules/` et les fichiers `*.log`.

## 3. Protéger la branche `main`

Dans GitHub, ouvrir **Settings → Rules → Rulesets** ou **Settings → Branches**, puis créer une règle pour la branche principale.

Configuration attendue :

- Nom du ruleset : `protection-main`.
- Branche ciblée : `main` ou **Include default branches**.
- Statut : **Active**.
- **Require a pull request before merging** activé.
- Nombre d'approbations requises : `1`.
- Push direct interdit par l'obligation de passer par une Pull Request.
- Option **Block force pushes** activée si elle est proposée.

L'étudiant B doit également être ajouté comme collaborateur si nécessaire.



## 4. Développer à deux

### Cloner le dépôt et créer les branches

Étudiant A :

```bash
git clone https://github.com/TAOUFIK0/tp-collaboration-git.git
cd tp-collaboration-git
git checkout -b feature/etudiant-a
```

Étudiant B :

```bash
git clone https://github.com/TAOUFIK0/tp-collaboration-git.git
cd tp-collaboration-git
git checkout main
git pull origin main
git checkout -b feature/etudiant-b
```

### Ajouter les fonctionnalités

Étudiant A :

```bash
echo "Fonctionnalité A" > fonctionnalite-a.txt
git add fonctionnalite-a.txt
git commit -m "feat: ajout de la fonctionnalité A"
git push -u origin feature/etudiant-a
```

Étudiant B :

```bash
echo "Fonctionnalité B" > fonctionnalite-b.txt
git add fonctionnalite-b.txt
git commit -m "feat: ajout de la fonctionnalité B"
git push -u origin feature/etudiant-b
```



## 5. Créer et résoudre un conflit

Pour provoquer un conflit, les deux étudiants modifient la même ligne du même fichier `README.md`.

### 5.1 Modifier le README sur les deux branches

Sur `feature/etudiant-a` :

```bash
git checkout feature/etudiant-a
# Modifier la ligne d'accueil avec le texte de l'étudiant A
git add README.md
git commit -m "docs: modif README par A"
git push
```

Sur `feature/etudiant-b` :

```bash
git checkout feature/etudiant-b
# Modifier la même ligne avec le texte de l'étudiant B
git add README.md
git commit -m "docs: modif README par B"
git push
```

### 5.2 Fusionner la Pull Request de A

Créer une Pull Request `feature/etudiant-a` → `main`. Après l'approbation requise, fusionner la Pull Request.



### 5.3 Faire apparaître le conflit sur la branche B

```bash
git checkout main
git pull origin main
git checkout feature/etudiant-b
git merge main
git status
```

Un conflit peut apparaître dans `README.md` avec des marqueurs similaires à ceux-ci :

```text
<<<<<<< HEAD
Bienvenue dans le projet de l'étudiant B.
=======
Bienvenue dans le projet de l'étudiant A.
>>>>>>> main
```



### 5.4 Résoudre le conflit

Conserver une version combinant les contributions, puis supprimer les marqueurs de conflit :

```text
Bienvenue dans notre projet Git réalisé en collaboration
par les étudiants A et B.
```

Valider et publier la résolution :

```bash
git add README.md
git commit -m "fix: résolution du conflit dans README"
git push origin feature/etudiant-b
```



### 5.5 Fusionner la Pull Request de B

Créer une Pull Request `feature/etudiant-b` → `main`, puis la faire approuver et fusionner.



## 6. Livrer la version finale

### 6.1 Supprimer les branches de fonctionnalité

Après la fusion des Pull Requests :

```bash
git checkout main
git pull origin main

git branch -d feature/etudiant-a
git branch -d feature/etudiant-b

git push origin --delete feature/etudiant-a
git push origin --delete feature/etudiant-b

git branch
git branch -r
```

### 6.2 Créer et envoyer le tag `v1.0.0`

```bash
git tag -a v1.0.0 -m "Version 1.0.0"
git tag
git push origin v1.0.0
# ou : git push origin --tags
```



### 6.3 Vérifier l'historique

```bash
git log --oneline --graph --all
```

L'historique attendu doit montrer l'initialisation, les contributions des deux branches, les fusions des Pull Requests, la résolution du conflit et le tag `v1.0.0`.



## 7. Difficultés, bilan et compétences acquises

### Difficultés rencontrées

Pendant ce TP, nous avons rencontré plusieurs difficultés :

- Nous ne trouvions pas l'option **Branches** dans les paramètres GitHub.  
  Nous avons finalement utilisé **Settings → Rules → Rulesets** pour protéger la branche `main`.

- La branche `main` devait être protégée contre les push directs.  
  Nous avons créé un ruleset actif avec l'option **Require a pull request before merging** et une approbation obligatoire.

- Un conflit est apparu lorsque les deux étudiants ont modifié la même ligne du fichier `README.md`.  
  Nous avons utilisé `git status` pour identifier le fichier concerné, puis nous avons supprimé les marqueurs de conflit :
  `<<<<<<<`, `=======` et `>>>>>>>`.

- Nous avons dû récupérer les dernières modifications avec les commandes suivantes :

```bash
git checkout main
git pull origin main
git checkout feature/etudiant-b
git merge main



### Compétences acquises

| Commande | Rôle |
|---|---|
| `git clone <URL>` | Récupérer le dépôt |
| `git checkout -b feature/nom` | Créer une branche de travail |
| `git add . && git commit -m "..."` | Enregistrer un changement |
| `git push -u origin feature/nom` | Publier une branche |
| `git checkout main && git pull origin main` | Récupérer les changements |
| `git merge main` | Intégrer `main` dans sa branche |
| `git add . && git commit -m "fix: ..."` | Valider une résolution de conflit |
| `git tag -a v1.0.0 -m "..."` | Marquer une version livrée |

## Conclusion

Ce TP permet de pratiquer un flux de travail collaboratif complet avec Git et GitHub : branches de fonctionnalité, Pull Requests sur une branche protégée, résolution d'un conflit et livraison d'une version taguée.

**Livrable final :** `[À compléter avec le lien vers la Pull Request finale fusionnée et à vérifier avec le tag réel v1.0.0.]`
