---
title: 3. feladat - CI/CD folyamat
sidebar_position: 3
---

# 3. Részfeladat: Automatizálás - CI/CD folyamat beállítása GitHub Actions segítségével

Ebben a részben egy egyszerű CI/CD (Continuous Integration/Continuous Deployment) folyamatot fogsz beállítani GitHub Actions segítségével, hogy a Docusaurus oldalad automatikusan build-eljen és publikálódjon GitHub Pages-re.

:::warning[Fontos]
Ezt a részfeladatot egy **új branch-en** végezd el, amit a `main` branch-ből hozol létre (pl. `feature/cicd-setup`). 
:::

## Mielőtt elkezdenéd

1.  **Visszaváltás `main`-re és új branch létrehozása:**

    ```bash
    git checkout main
    git pull origin main
    git checkout -b feature/cicd-setup
    ```
    
    :::note
    Győződj meg róla, hogy a `main` a legfrissebb (`git pull origin main`, ha szükséges)
    :::

## 3.1 Docusaurus konfigurálása GitHub Pages-hez

Módosítsd a `docusaurus.config.js` fájlt a GitHub Pages-hez való helyes publikáláshoz.

```javascript title="docusaurus.config.js"
module.exports = {
  // ...
  url: 'https://<FELHASZNALONEVED>.github.io', // Cseréld le a saját GitHub felhasználónevedre!
  baseUrl: '/<REPOSITORY_NEVE>/', // Cseréld le a repository nevére!
                                     // Ha a repository-hoz tartozó GitHub Pages neve <FELHASZNALONEVED>.github.io, akkor a baseUrl '/' legyen.
  organizationName: '<FELHASZNALONEVED>', // A GitHub felhasználóneved
  projectName: '<REPOSITORY_NEVE>', // A GitHub repository-d neve
  // ...
};
```

:::info[Segítség]
[Docusaurus Deployment - GitHub Pages](https://docusaurus.io/docs/deployment#deploying-to-github-pages)
:::

## 3.2 GitHub Actions workflow létrehozása a deploymenthez

Automatizáljuk a publikálást.

1.  **Mappák létrehozása:** A Docusaurus projekted gyökerében hozz létre egy `.github/workflows` mappát.

    ```bash
    mkdir -p .github/workflows
    ```

1.  **Workflow fájl létrehozása (`deploy.yml`):**

    A `.github/workflows` mappában hozz létre egy `deploy.yml` nevű fájlt. Másold bele a Docusaurus hivatalos dokumentációjában található GitHub Pages deployment workflow tartalmát. Ez általában a következőkhöz hasonló:

    ```yaml title=".github/workflows/deploy.yml"
    name: Deploy to GitHub Pages

    on:
      push:
        branches:
          - main
      workflow_dispatch: # Manuális indítás engedélyezése
        # Review gh actions docs if you want to further define triggers, paths, etc
        # [https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#on](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#on)

    jobs:
      build:
        name: Build Docusaurus
        runs-on: ubuntu-latest
        steps:
          - uses: actions/checkout@v4
            with:
              fetch-depth: 0
          - uses: actions/setup-node@v4
            with:
              node-version: 18
              cache: yarn

          - name: Install dependencies
            run: yarn install --frozen-lockfile
          # Adjuk hozzá ezt a lépést, hogy az API specifikáció is felépüljön.
          - name: Build API Docs
            run: yarn docusaurus gen-api-docs all
          - name: Build website
            run: yarn build

          - name: Upload Build Artifact
            uses: actions/upload-pages-artifact@v3
            with:
              path: build

      deploy:
        name: Deploy to GitHub Pages
        needs: build

        # Grant GITHUB_TOKEN the permissions required to make a Pages deployment
        permissions:
          pages: write # to deploy to Pages
          id-token: write # to verify the deployment originates from an appropriate source

        # Deploy to the github-pages environment
        environment:
          name: github-pages
          url: ${{ steps.deployment.outputs.page_url }}

        runs-on: ubuntu-latest
        steps:
          - name: Deploy to GitHub Pages
            id: deployment
            uses: actions/deploy-pages@v4
    ```

    :::caution[Figyelem]
    - Ellenőrizd a `node-version`-t, hogy megfeleljen a projektednek.
    - Ha az API dokumentáció generált fájljait nem commitolod (ajánlott nem commitolni őket, és a CI/CD-vel generáltatni), akkor a `Build API Docs` lépés szükséges. Ha commitolod őket, ez a lépés kihagyható. 
    
        A házi feladat során korábban commitoltuk őket, de a CI/CD során jobb, ha a forrásból (OpenAPI spec) generálódnak - ebben az esetben az output mappát hozzá is adhatod a `.gitignore`-hoz, hogy ne zavarjon fejlesztés közben a sok új fájl, ami a generáláskor létrejön. Döntsd el, melyik utat követed, és szükség szerint módosítsd a workflow-t. Ebben a példában benne hagytam a generálást.
    :::

1.  **Repository beállítások GitHub Actions-höz:** A GitHub repository **Settings -> Actions -> General** részében a **Workflow permissions** alatt győződj meg róla, hogy a **Read and write permissions** van kiválasztva, vagy legalábbis a GitHub Actions képes írni a `gh-pages` branch-et és kezelni a Pages beállításokat. Az `id-token: write` permission a `actions/deploy-pages@v4` újabb verzióihoz szükséges.

:::info[Segítség]
[Docusaurus Deployment - Triggering deployment with GitHub Actions](https://docusaurus.io/docs/deployment#triggering-deployment-with-github-actions)
:::

## 3.3 Változások commitálása és a workflow tesztelése

Mentsük el a CI/CD konfigurációt és teszteljük.

1.  **Commit és push:**
    - Add hozzá a `.github/workflows/deploy.yml` fájlt és a `docusaurus.config.js` módosításait a staging területhez.
    - Készíts egy commitot a `feature/cicd-setup` branch-re:

        ```bash
        git add .
        git commit -m "Feat: Configure GitHub Pages deployment and add CI/CD workflow"
        git push -u origin feature/cicd-setup
        ```

1.  **Pull request létrehozása és merge-elése a `main` branch-be:**
    - A GitHub felületén hozz létre egy Pull Requestet a `feature/cicd-setup` branch-ből a `main` branch-be. Adj neki címet és leírást.
    - **Most, ennél a PR-nél, végezd el a merge-elést a `main` branch-be.** (Feltételezzük, hogy a reviewer ezt a PR-t "jóváhagyta".)

1.  **Deployment ellenőrzése:**
    - A `main` branch-be történő merge után a `deploy.yml` workflow-nak automatikusan el kell indulnia.
    - Nyisd meg a GitHub repository-dat, és az **Actions** fülön ellenőrizd, hogy a workflow sikeresen lefutott-e.
    - Ha sikeres volt, a GitHub repository **Settings -> Pages** részében látnod kell, hogy az oldalad publikálva lett a konfigurált `gh-pages` branch-ről. Az URL-nek meg kell jelennie (pl. `https://<FELHASZNALONEVED>.github.io/<REPOSITORY_NEVE>/`).
    - Látogass el a publikált URL-re, és ellenőrizd, hogy minden megfelelően működik-e.

## 3.4 (Opcionális) Teszt workflow hozzáadása pull requestekhez

Ez a lépés segít abban, hogy a hibás kód ne kerüljön be a `main` branch-be.

1.  **Workflow fájl létrehozása (`test.yml`):**

    A `.github/workflows` mappában hozz létre egy `test.yml` nevű fájlt.

    ```yaml title=".github/workflows/test.yml"
    name: Test Docusaurus Build

    on:
      pull_request:
        branches:
          - main # Csak a main branch-be irányuló PR-ek esetén

    jobs:
      test-build:
        runs-on: ubuntu-latest
        steps:
          - name: Checkout
            uses: actions/checkout@v4
          - name: Set up Node
            uses: actions/setup-node@v4
            with:
              node-version: '18' # Vagy a projekt által használt Node.js verzió
              cache: 'npm'
          - name: Install dependencies
            run: npm ci
          - name: Generate API Docs (if needed)
            # Ha a generált API doksik nincsenek commitolva
            run: npm run docusaurus gen-api-docs all
          - name: Build
            run: npm run build
    ```

1.  **Commit és push (egy új branch-en tesztelve):**

    - Hozz létre egy új, teszt branch-et (pl. `feature/test-pr-workflow`) a `main` branch-ből.
    - Add hozzá a `test.yml` fájlt, commitold és pushold ezt az új branch-et.
    - Hozz létre egy Pull Requestet ebből a teszt branch-ből a `main` branch-be.
    - Ellenőrizd az **Actions** fülön, hogy a `Test Docusaurus Build` workflow elindult-e és sikeresen lefutott-e a PR-en.
    - Ha működik, a `test.yml` fájlt hozzáadhatod a `main` branch-hez a PR-t mergelve. (Feltételezzük, hogy a reviewer "jóváhagyta".)

___

## Elvárás a 3. feladat végére

| Kritérium | Elvárt állapot |
| --------- | -------------- |
| **GitHub Pages konfiguráció** | A `docusaurus.config.js` helyesen van beállítva a GitHub Pages publikáláshoz (`url`, `baseUrl`, `organizationName`, `projectName`). |
| **Deployment workflow (`deploy.yml`)** | Egy működő `deploy.yml` GitHub Actions workflow létezik, ami a `main` branch-re történő merge után sikeresen build-eli és deploy-olja az oldalt. |
| **Sikeres deployment** | A Docusaurus oldal (beleértve a korábbi feladatokból származó tartalmakat) sikeresen elérhető a publikus GitHub Pages URL-en. |
| **(Opcionális) Teszt workflow (`test.yml`)** | Egy `test.yml` workflow is létezik, ami Pull Request-ek esetén ellenőrzi a Docusaurus build sikerességét. |
