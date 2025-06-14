# TP : Introduction à GitHub Actions (CI/CD simplifié)


---

## Prérequis

- Compte GitHub avec un dépôt nommé par exemple `tp-github-actions`
- Git installé en local
- Éditeur de texte (VSCode, IntelliJ, etc.)



## Étape 1 — Création du dépôt

1. Va sur GitHub, crée un nouveau dépôt :
   - Nom : `tp-github-actions`
   - Visibilité : Public ou privé
   - Coche **"Add a README file"**

2. Clone le dépôt en local :

```bash
git clone https://github.com/ton-utilisateur/tp-github-actions.git
cd tp-github-actions
```

---

##  Étape 2 — Création de l’arborescence du workflow

1. Dans le dépôt local, crée les dossiers suivants :

```bash
mkdir -p .github/workflows
```

2. Crée un fichier `main.yml` dans ce dossier :

```bash
touch .github/workflows/main.yml
```

---

##  Étape 3 — Écrire le fichier `ci.yml`

Colle le contenu suivant dans `.github/workflows/ci.yml` :


name: CI simple

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout du code
        uses: actions/checkout@v4

      - name: Étape de test (simulée)
        run: echo "Tests OK"

      - name: Étape de déploiement conditionnelle
        if: ${{ hashFiles('deploy.flag') != '' }}
        run: echo "Fichier deploy.flag détecté"


> **Explication :**
> - `on: push` → déclenche l’action à chaque push sur `main`
> - `hashFiles('deploy.flag') != ''` → vérifie que le fichier existe

---

## Étape 4 — Tester le workflow

1. Crée un fichier de test :

```bash
echo "Ceci est un test" > test.txt
```

2. Commit et push :

```bash
git add .
git commit -m "Ajout du fichier de workflow"
git push origin main
```

3. Sur GitHub :
   - Va dans ton dépôt > onglet **Actions**
   - Tu verras le workflow s’exécuter automatiquement
   - Clique dessus pour voir chaque étape

---

## Étape 5 — Déclencher le déploiement conditionnel

1. Crée un fichier `deploy.flag` :

```bash
touch deploy.flag
```

2. Commit et push :

```bash
git add deploy.flag
git commit -m "Ajout du fichier deploy.flag"
git push origin main
```

3. Retourne dans l’onglet **Actions** → nouvelle exécution
   - Tu devrais voir apparaître la ligne :  
     `Fichier deploy.flag détecté, déploiement simulé...`

---