---
title: 2. feladat - API dokumentáció
sidebar_position: 2
---

# 2. Részfeladat: Dinamikus tartalom - API dokumentáció generálása OpenAPI specifikációból

Ebben a részben egy OpenAPI specifikáció alapján generált API dokumentációval fogod bővíteni az oldaladat a kurzuson bemutatott `@paloaltonetworks/docusaurus-openapi-docs` plugin segítségével.

:::warning[Fontos]
Ezt a részfeladatot egy **új branch-en** végezd el, amit a `main` branch-ből hozol létre (pl. `feature/api-documentation`).
:::

## Mielőtt Elkezdenéd

1.  **Visszaváltás `main`-re és frissítés:**
    ```bash
    git checkout main
    git pull origin main
    ```
    *(Győződj meg róla, hogy a `main` branch-ed a legfrissebb állapotban van, mielőtt új branch-et hozol létre.)*

2.  **Új branch létrehozása:**
    ```bash
    git checkout -b feature/api-documentation
    ```

## 2.1 OpenAPI Docs Plugin (Palo Alto Networks) Telepítése és Konfigurálása

### Plugin Telepítése

Telepítsd a közösségi plugint, a projekted gyökér mappájában, terminálban futtatva:

```bash
npm install @paloaltonetworks/docusaurus-openapi-docs
```

### OpenAPI Specifikáció Előkészítése

1.  Hozz létre egy `openapi` mappát a projekt gyökerében.
1.  Mentsd el ide a [Petstore API](https://petstore3.swagger.io/openapi.yaml) specifikációját `petstore.yaml` néven. Használhatsz más v3-as OpenAPI specifikációt is.

### Plugin Konfigurálása

Módosítsd a `docusaurus.config.js` fájlt. A `plugins` tömbben add hozzá a következő konfigurációt:

```javascript
// docusaurus.config.js
// ...
module.exports = { // Vagy export default, ha ES modult használsz
  // ...meglévő konfigurációk...
  plugins: [
    // ...esetleges egyéb pluginek...
    [
      '@paloaltonetworks/docusaurus-openapi-docs',
      {
        id: 'petstoreapi', // Egyedi azonosító az API doksinak
        specPath: 'openapi/petstore.yaml', // Az OpenAPI fájl elérési útja
        outputDir: 'docs/petstore-api', // A generált Markdown fájlok helye
        sidebarOptions: {
          groupPathsBy: 'tag', // Csoportosítás tagek alapján az oldalsávban
          // További opciók a plugin dokumentációja szerint
        },
        // Opcionális: downloadButton: true,
      },
    ],
  ],
  // ...további konfigurációk...
};
```

*Segítség:* [PaloAltoNetworks/docusaurus-openapi-docs GitHub Repository](https://github.com/PaloAltoNetworks/docusaurus-openapi-docs)

## 2.2 API Dokumentáció Generálása és Elérhetővé Tétele

### Markdown Fájlok Generálása

Futtasd a plugin parancsát a Markdown fájlok generálásához (a parancs pontos formáját ellenőrizd a plugin dokumentációjában, általában valami hasonló):

```bash
npm run docusaurus gen-api-docs all
# Vagy: npx docusaurus gen-api-docs all
# Vagy: yarn docusaurus gen-api-docs all
```

Indítsd el a fejlesztői szervert (`npm start`), és ellenőrizd, hogy a fájlok létrejöttek-e a `docs/petstore-api` mappában.

### Navigáció Beállítása

1.  **Oldalsáv (`sidebars.js`) módosítása:**

    Hivatkozz a generált API oldalsávra. Például, ha létrehozol egy külön oldalsávot az API dokumentációnak:

    ```javascript
    // sidebars.js
    module.exports = {
      // ...meglévő oldalsávjaid (pl. tutorialSidebar, guideSidebar)...
      myApiSidebar: [ // Új oldalsáv az API-nak
        {
          type: 'category',
          label: 'Petstore API', // Az oldalsávban megjelenő címke
          link: {
            type: 'generated-index', // Automatikusan generált index oldal a kategóriához
            title: 'Petstore API Áttekintés',
            slug: '/category/petstore-api-docs' // URL slug; győződj meg róla, hogy egyedi
          },
          items: require('./docs/petstore-api/sidebar.js'), // Hivatkozás a plugin által generált oldalsáv fájlra
        },
      ],
    };
    ```

    Természetesen integrálhatod egy meglévő oldalsávba is, ha az jobban illeszkedik a struktúrádhoz. Az 1. részfeladatban létrehozott `tutorialSidebar` vagy `myDocumentationSidebar` (ha közös oldalsávot használsz) is bővíthető.

1.  **Navigációs sáv (`docusaurus.config.js`) frissítése:**

    Adj hozzá egy linket a fő navigációs sávhoz, ami az API dokumentációra mutat.

    ```javascript
    // docusaurus.config.js
    // ...
    themeConfig: {
      // ...meglévő themeConfig...
      navbar: {
        title: 'My Docs Site', // Vagy a te oldalad címe
        // ...logo...
        items: [
          // ...meglévő navbar itemek (pl. Tutorials, Guides)...
          {
            to: '/docs/petstore-api'
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

    Ha nem külön oldalsávot használsz, hanem pl. a `/docs/petstore-api/` (vagy a generált index oldal) slugjára szeretnél linkelni, akkor a `to` attribútumot használd:

    ```javascript
    // Példa 'to' használatára, ha nincs külön API oldalsáv a navbarban:
    // {
    //   to: '/docs/petstore-api/', // Vagy a generált kezdőoldal slugja
    //   label: 'Petstore API',
    //   position: 'left',
    // },
    ```


### Ellenőrzés

Indítsd el a fejlesztői szervert (`npm start`), és győződj meg róla, hogy az API dokumentáció megjelenik, a linkek és az oldalsáv helyesen működnek.

## Változások Feltöltése és Pull Request Létrehozása (Merge és Branch Törlés Nélkül)

Mentsük el ezt a munkát is.

1.  **Commit és push:**
    ```bash
    git add .
    git commit -m "Feat: Add Petstore API documentation using PaloAlto plugin"
    git push -u origin feature/api-documentation
    ```

2.  **Pull Request létrehozása:** A GitHub felületén hozz létre egy új Pull Requestet a `feature/api-documentation` branch-ből a `main` branch-be. Adj neki egyértelmű címet és leírást.

:::warning
Ezt a Pull Requestet is merge-lheted, de **NE TÖRÖLD A BRANCH-ET!**
:::

___

## Elvárás a 2. Részfeladat Végére

* A `main` branch-ből létrehozott `feature/api-documentation` (vagy hasonló nevű) branch létezik a GitHub repository-ban.
* Ez a branch tartalmazza a telepített és konfigurált `@paloaltonetworks/docusaurus-openapi-docs` plugint.
* A Docusaurus oldal kiegészült egy (pl. Petstore) API dokumentációval, ami elérhető a navigációs sávon és az oldalsávon keresztül.
* Egy Pull Request mutat a `feature/api-documentation` branch-ből a `main` branch-be, ami merge-elve lett.
