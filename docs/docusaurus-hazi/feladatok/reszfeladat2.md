---
title: 2. feladat - API dokumentáció
sidebar_position: 2
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# 2. feladat: Dinamikus tartalom - API dokumentáció generálása OpenAPI specifikációból

Ebben a feladatban egy OpenAPI specifikáció alapján generált API dokumentációval fogod bővíteni az oldaladat a kurzuson bemutatott [`@paloaltonetworks/docusaurus-openapi-docs`](https://github.com/PaloAltoNetworks/docusaurus-openapi-docs) plugin segítségével.

:::warning[Fontos]
Ezt a részfeladatot egy **új branch-en** végezd el, amit a `main` branch-ből hozol létre (pl. `feature/api-documentation`).
:::

## Mielőtt elkezdenéd

1.  **Visszaváltás `main`-re és frissítés:**

    ```bash
    git checkout main
    git pull origin main
    ```
    :::note
    Győződj meg róla, hogy a `main` branch-ed a legfrissebb állapotban van, mielőtt új branch-et hozol létre.
    :::

1.  **Új branch létrehozása:**

    ```bash
    git checkout -b feature/api-documentation
    ```

## 2.1 OpenAPI docs plugin telepítése és konfigurálása

### Plugin és téma telepítése

Telepítsd a közösségi plugin-t és a hozzá tartozó dokumentációs témát, a projekted gyökér mappájában, terminálban futtatva:

<Tabs>
<TabItem value='npm' label='NPM'>

```bash
npm install docusaurus-plugin-openapi-docs
npm install docusaurus-theme-openapi-docs
```

</TabItem>
<TabItem value='yarn' label='Yarn'>

```bash
yarn add docusaurus-plugin-openapi-docs
yarn add docusaurus-theme-openapi-docs
```

</TabItem>
</Tabs>

### React telepítése

Telepítsd a React 18, vagy korábbi verzióját:

<Tabs>
<TabItem value='npm' label='NPM'>

```bash
npm install react@18
npm install react-dom@18
```

</TabItem>
<TabItem value='yarn' label='Yarn'>

```bash
yarn add react@18
yarn add react-dom@18
```

</TabItem>
</Tabs>

### OpenAPI specifikáció előkészítése

1.  Hozz létre egy `openapi` mappát a projekt gyökerében.
1.  Mentsd el ide a [Petstore API](https://petstore3.swagger.io/openapi.yaml) specifikációját `petstore.yaml` néven. Használhatsz más v3-as OpenAPI specifikációt is, pl. az API-s házival létrehozottat.

### Plugin konfigurálása

Módosítsd a `docusaurus.config.js` fájlt. A `presets.docs` és `plugins` tömbben add hozzá a következő konfigurációt:

```javascript title="docusaurus.config.js"
// ...
module.exports = { // Vagy export default, ha ES modult használsz
  // ...meglévő konfigurációk...
  presets: [
    [
      'classic',
      /** @type {import('@docusaurus/preset-classic').Options} */
      ({
        docs: {
          sidebarPath: './sidebars.js',
          //highlight-next-line
          docItemComponent: "@theme/ApiItem", // Az API elemek komponensei, add hozzá ezt a sort
          
        blog: { /*...*/ },
        theme: { /*...*/ }
      },
      }),
    ],
  ], 
  
  //highlight-start
  plugins: [
    [
      'docusaurus-plugin-openapi-docs',
      {
        id: 'openapi', // A plugin egyedi azonosítója
        docsPluginId: 'classic',
        config: {
          petstore: {  // Egyedi azonosító az API doksinak
            specPath: 'api/petstore-api.yaml', // Az OpenAPI fájl elérési útja
            outputDir: 'docs/petstore', // A generált Markdown fájlok helye
            sidebarOptions: {
              groupPathsBy: 'tag',  // Csoportosítás tagek alapján az oldalsávban
          // További opciók a plugin dokumentációja szerint
            },
            // Opcionális: downloadButton: true,
          },
        }
      }
    ]
  ],
  themes: ['docusaurus-theme-openapi-docs'],
  //highlight-end
  
  // ...további konfigurációk...
};
```

:::info[Segítség]
[PaloAltoNetworks/docusaurus-openapi-docs GitHub Repository](https://github.com/PaloAltoNetworks/docusaurus-openapi-docs)
:::

## 2.2 API dokumentáció generálása és elérhetővé tétele

### Markdown fájlok generálása

1. **Parancs futtatása:** Futtasd a plugin parancsát a Markdown fájlok generálásához (a parancs pontos formáját ellenőrizd a plugin dokumentációjában, általában valami hasonló):
    
    <Tabs>
    <TabItem value='npm' label='NPM'>
    
    ```bash
    npm run docusaurus gen-api-docs all
    ```

    </TabItem>
    <TabItem value='yarn' label='Yarn'>
    
    ```bash
    yarn docusaurus gen-api-docs all
    ```

    </TabItem>
    </Tabs>

1. **Kimeneti mappa ellenőrzése:** Ellenőrizd, hogy a fájlok helyesen létrejöttek-e a konfigurált kimeneti mappában (`outputDir`).

1. **`.gitignore` frissítése (ajánlott):** 

    Add hozzá a generált API dokumentáció kimeneti mappáját a `.gitignore` fájlodhoz. Ez megakadályozza, hogy a generált Markdown fájlok bekerüljenek a Git verziókövetésbe.

    ```text title=".gitignore"
    # ... egyéb bejegyzések ...

    # Generated API docs
    docs/petstore-api/
    ```

    :::tip Miért hasznos ez?
    - **Tiszta változáskövetés:** Csak az OpenAPI specifikációs fájl (pl. `petstore.yaml`) változásait kell követned. A Markdown fájlok ebből automatikusan generálódnak.
    - **Automatizálás:** A CI/CD folyamat (amit a [3. feladatban](./reszfeladat3) állítasz be) felelős lesz a Markdown fájlok friss generálásáért minden build során.
    - **Lokális fejlesztés:** Attól függetlenül, hogy a generált fájlok nincsenek a Gitben, lokálisan továbbra is legenerálhatod őket (`npm run docusaurus gen-api-docs all`), hogy lásd az előnézetet és szerkeszthesd az API dokumentációt (pl. plusz információk hozzáadásával a generált fájlokhoz). **Ha az OpenAPI specifikáció (YAML) változik, újra kell generálni a fájlokat a változások megjelenítéséhez.**
    :::

1. **Fejlesztői szerver indítása és ellenőrzés:** 

    Indítsd el a fejlesztői szervert (`npm start`), és ellenőrizd, hogy a generált API dokumentáció megjelenik-e a `docs/petstore-api/add-pet` útvonalon (vagy ahogy a [navigációban beállítod](#navigáció-beállítása)).

### Navigáció beállítása

Ahhoz, hogy a létrehozott API dokumentációt a navigációban is elérhetővé tegyük, el kell végeznünk néhány konfigurációs lépést.

1. **Oldalsáv (`sidebars.js`) módosítása:**

    Hivatkozz a generált API oldalsávra. Például, ha létrehozol egy külön oldalsávot az API dokumentációnak:

    ```javascript title="sidebars.js"
    module.exports = {
      // ...meglévő oldalsávjaid (pl. tutorialSidebar, guideSidebar)...
      myApiSidebar: require('./docs/petstore-api/sidebar'), // Hivatkozás a plugin által generált oldalsáv fájlra
    };
    ```

    :::tip
    Természetesen integrálhatod egy meglévő oldalsávba is, ha az jobban illeszkedik a struktúrádhoz. Az 1. feladatban létrehozott `tutorialSidebar` vagy egy egyedi `myDocumentationSidebar` (ha közös oldalsávot használsz) is bővíthető.
    :::

1. **Navigációs sáv (`docusaurus.config.js`) frissítése:**

    Adj hozzá egy linket a fő navigációs sávhoz, ami az API dokumentációra mutat.

    ```javascript title="docusaurus.config.js"
    // ...
    themeConfig: {
      // ...meglévő themeConfig...
      navbar: {
        title: 'My Docs Site', // Vagy a te oldalad címe
        // ...logo...
        items: [
          // ...meglévő navbar itemek (pl. Tutorials, Guides)...
          {
            type: 'docSidebar', // Ha külön oldalsávot használsz az API-hoz
            sidebarId: 'myApiSidebar', // Az API oldalsávjának ID-ja a sidebars.js-ből
            label: 'Petstore API',
            position: 'left',
          },
          // ...esetleges blog link, GitHub link...
        ],
      },
    },
    // ...
    ```

1. **Ellenőrzés:** Indítsd el a fejlesztői szervert (`npm start`), és győződj meg róla, hogy az API dokumentáció megjelenik, a linkek és az oldalsáv helyesen működnek.

## 2.3 Változások feltöltése és pull request létrehozása (merge és branch törlés nélkül)

Mentsük el ezt a munkát is.

1.  **Commit és push:**

    ```bash
    git add .
    git commit -m "Feat: Add Petstore API documentation using PaloAlto plugin"
    git push -u origin feature/api-documentation
    ```

1.  **Pull Request létrehozása:** A GitHub felületén hozz létre egy új Pull Requestet a `feature/api-documentation` branch-ből a `main` branch-be. Adj neki egyértelmű címet és leírást.

:::danger[Fontos]
Ezt a Pull Requestet is merge-lheted, de **NE TÖRÖLD A BRANCH-ET!**
:::

___

## Elvárás a 2. feladat végére

| Kritérium | Elvárt állapot |
| --------- | -------------- |
| **`feature/api-documentation` branch** | Létezik egy `feature/api-documentation` (vagy hasonló nevű) branch a GitHub repository-ban, a `main` branch-ből kiindulva. |
| **OpenAPI plugin** | A `@paloaltonetworks/docusaurus-openapi-docs` plugin telepítve és helyesen van konfigurálva a `docusaurus.config.js`-ben. |
| **API dokumentáció generálása** | Az API dokumentáció (pl. Petstore API alapján) sikeresen legenerálódott a megadott `outputDir`-be. |
| **Navigáció (API)** | Az API dokumentáció elérhető a Docusaurus oldal navigációs sávján és/vagy oldalsávján keresztül. |
| **Pull Request (API)** | Egy merge-elt (de a branch nem törölt) PR mutat a `feature/api-documentation` branch-ből a `main` branch-be. |
