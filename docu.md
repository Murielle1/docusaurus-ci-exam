# Documentation

- Initialisation du projet et configuration de Docusaurus.

Pour installer le projet et configurer Docusaurus, il faut: installer Node puis lancer cette commande: 

```java
npx create-docusaurus@latest my-website classic
```

- Configuration de l'intégration continue (CI) avec GitHub Actions.

Dans un premier temps il faut:

Créer un dossier .github/workflows

Créer un fichier github-workflow.yml

Dans ce fichier , il fallait configurer GitHub Actions pour que le site Docusaurus soit construit à chaque push vers la branche principale. Pour ce faire nous avons ajouter le code suivant pour s’assurer qu’à chaque push sur la branche main les jon s’éxécute:

```yaml
on:
  push:
    branches:
      - main
```

Ensuite nous avons installé node ainsi que les dépendances, puis build notre site qui se trouve dans le dossier my-website

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout repository
      uses: actions/checkout@v3

    - name: Set up Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '20'

    - name: Install dependencies
      working-directory: my-website
      run: npm install

    - name: Build Docusaurus site
      working-directory: my-website
      run: npm run build
```

- Déploiement du site sur GitHub Pages.

La documentation de Docusaurus dispose des informations concernant le déploiement  sur Github Pages. J’ai dans un premier temps modifier le fichier docusaurus.config.js dans lequel il faut modifier les variables: organizationName,projectName et deploymentBranch. J’ai ensuite ajouter ce code dans le fichier existant .yml

```yaml
deploy:
    name: Deploy to GitHub Pages
    needs: build
    runs-on: ubuntu-latest
        steps:
        - name: Checkout repository
          uses: actions/checkout@v3

        - name: Download build artifacts
          uses: actions/download-artifact@v3
          with:
            name: docusaurus-build

        - name: Deploy to GitHub Pages
          uses: actions/deploy-pages@v4
          with:
            publish_dir: my-website/build

        permissions:
          pages: write
          id-token: write

    environment:
      name: github-pages
```

- Ajout et exécution des tests pour le site.

- Utilisation de linters et autres outils de vérification de code.
- Description détaillée du flux de travail Git Flow utilisé.