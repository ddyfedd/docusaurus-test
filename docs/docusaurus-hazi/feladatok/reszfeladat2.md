---
title: 2. feladat - API dokumentáció
sidebar_position: 2
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# 2. feladat: Dinamikus tartalom - API dokumentáció generálása OpenAPI specifikációból

Ebben a feladatban egy OpenAPI specifikáció alapján generált API dokumentációval fogod bővíteni az oldaladat a kurzuson bemutatott [`@paloaltonetworks/docusaurus-openapi-docs`](https://github.com/PaloAltoNetworks/docusaurus-openapi-docs) plugin segítségével.

## ⏱️ Becsült időigény

| Szakasz | Időigény | Megjegyzés |
|---------|----------|------------|
| **Plugin telepítése és konfiguráció** | 15-20 perc | Első plugin telepítésnél +5-10 perc |
| **OpenAPI spec előkészítése** | 5-10 perc | Ha saját API-t használsz, több idő |
| **API dokumentáció generálása** | 10-15 perc | Navigáció beállítása, tesztelés |
| **CSS stílusok hozzáadása** | 5-10 perc | Opcionális testreszabás |
| **Összesen** | **45-60 perc** | Hibakeresés nélkül |

## 📋 Előfeltételek

Mielőtt nekikezdenél ennek a feladatnak, győződj meg róla, hogy:

| Előfeltétel | Ellenőrzés | Hol találod |
|-------------|------------|-------------|
| **1. feladat elvégezve** | Működő Docusaurus projekt a `main` branch-en | [1. feladat](./reszfeladat1) |
| **Node.js és NPM működik** | `node -v` és `npm -v` parancsok válaszolnak | [Előkészületek](../elokeszuletek#2-nodejs-és-npm-telepítése) |
| **Git és GitHub beállítva** | Commitokat tudsz készíteni és push-olni | [Előkészületek](../elokeszuletek#4-git-és-github-cli) |
| **Docusaurus szerver fut** | `npm start` paranccsal elindul az oldal | [1. feladat](./reszfeladat1#docusaurus-telepítése-és-projekt-inicializálása) |

:::tip
Ha az [1. feladat](./reszfeladat1) még nincs kész, előbb azt fejezd be!
:::

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

## 🔍 Alapfogalmak: OpenAPI és Docusaurus pluginok

Az órákon már megismerkedtél az OpenAPI specifikációval és a Docusaurus plugin rendszerrel. Mielőtt nekikezdenél a technikai lépéseknek, frissítsd fel ezeket a fogalmakat, hogy magabiztosan tudj haladni!

### Mi az az OpenAPI?

Az [**OpenAPI Specification**](https://learn.openapis.org/) (korábban Swagger Specification néven ismert) egy szabványos formátum REST API-k leírására. Ez egy JSON vagy YAML formátumú fájl, amely részletesen dokumentálja az API-t:

- **Végpontok (endpoints):** Milyen URL-eken érhetők el az API funkciók (pl. `/pets`, `/users/{id}`)
- **HTTP metódusok:** Milyen műveleteket támogat (GET, POST, PUT, DELETE, stb.)
- **Paraméterek:** Milyen inputokat vár az API (path, query, header, body paraméterek)
- **Válaszok:** Milyen struktúrában és státuszkódokkal (200, 404, 500, stb.) válaszol
- **Adatmodellek (schemas):** Milyen objektumstruktúrákat használ (pl. egy `Pet` objektumnak milyen mezői vannak)

**Miért hasznos?**
- **Automatizálás:** Egy OpenAPI specifikációból automatikusan generálható dokumentáció, kliens SDK-k, szerveroldali stub kód, és még sok más
- **Egyértelmű kommunikáció:** A fejlesztők, tesztelők és dokumentációírók ugyanazt a "forrást" használják
- **Interaktív dokumentáció:** Az eszközök (mint a Swagger UI) lehetővé teszik az API kipróbálását közvetlenül a dokumentációból

**Példa egy egyszerű OpenAPI leírásra:**

```yaml title="pelda-api.yaml"
openapi: 3.0.0
info:
  title: Pet Store API
  version: 1.0.0
paths:
  /pets:
    get:
      summary: Lista az összes kisállatról
      responses:
        '200':
          description: Sikeres válasz
```

:::info További olvasnivaló
- [OpenAPI hivatalos dokumentáció](https://swagger.io/specification/)
- [Mi az az OpenAPI? - Swagger útmutató](https://swagger.io/docs/specification/about/)
:::

### Mi az a Plugin a Docusaurus-ban?

A **Docusaurus plugin** egy kiterjesztés, amely új funkcionalitást ad hozzá a Docusaurus oldaladhoz. A pluginok lehetővé teszik, hogy:

- **Új tartalom típusokat adj hozzá:** Pl. API dokumentáció, changelog, képgaléria
- **Testreszabd a build folyamatot:** Fájlok generálása, adatok betöltése külső forrásokból
- **Integráld harmadik féltől származó eszközöket:** Pl. analytics, keresés, kommentrendszerek

**A Docusaurus plugin ökoszisztéma:**
- **Hivatalos pluginok:** A Docusaurus csapat által fejlesztett és karbantartott pluginok (pl. `@docusaurus/plugin-content-docs`, `@docusaurus/plugin-content-blog`)
- **Közösségi pluginok:** Harmadik féltől származó pluginok (mint a `@paloaltonetworks/docusaurus-openapi-docs`, amit ebben a feladatban használunk)

**Hogyan működik?**
1. Telepíted a plugin npm csomagot (`npm install plugin-name`)
2. Hozzáadod a `docusaurus.config.js` fájlban a `plugins` tömbhöz
3. Konfigurálod a plugin opcióit (pl. fájl elérési utak, viselkedés beállítások)
4. A plugin automatikusan integrálódik a build folyamatba

**Példa plugin konfiguráció:**

```javascript title='docsuaurus.config.js'
plugins: [
  [
    'plugin-name',
    {
      option1: 'value',
      option2: true
    }
  ]
]
```

:::info További olvasnivaló
- [Docusaurus Plugin áttekintés](https://docusaurus.io/docs/using-plugins)
- [Docusaurus Plugin API](https://docusaurus.io/docs/api/plugin-methods)
:::

## 2.1 OpenAPI docs plugin telepítése és konfigurálása

### Plugin és téma telepítése

Telepítsd a közösségi plugin-t és a hozzá tartozó dokumentációs témát, a projekted gyökér mappájában, terminálban futtatva:

<Tabs groupId='package-manager'>
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

:::info[Miért React 18?]
A `@paloaltonetworks/docusaurus-openapi-docs` plugin React 18-at igényel a megfelelő működéshez. Bár a Docusaurus alapértelmezetten React 18-at használ, előfordulhat, hogy a projekt inicializálásakor egy újabb verziót telepített. A plugin még nem kompatibilis a React 19-cel vagy újabb verziókkal, ezért explicit módon le kell fixálnunk a React verzióját 18-ra.

**Ha nem telepíted React 18-at:**
- Build hibákat kaphatsz a plugin használatakor
- Az API dokumentáció komponensei nem jelennek meg helyesen
- Peer dependency warning üzeneteket fogsz kapni
:::

<Tabs groupId='package-manager'>
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
1.  Mentsd el ide a [Petstore API](https://petstore3.swagger.io/openapi.yaml) specifikációját `petstore-api.yaml` néven. Használhatsz más v3-as OpenAPI specifikációt is, pl. az API-s házival létrehozottat.

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
            specPath: 'openapi/petstore-api.yaml', // Az OpenAPI fájl elérési útja
            outputDir: 'docs/petstore', // A generált Markdown fájlok helye
            sidebarOptions: {
              groupPathsBy: 'tag',  // Csoportosítás tagek alapján az oldalsávban
            },
            // További opciók a plugin dokumentációja szerint
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

### API dokumentáció stílusainak hozzáadása

Az API dokumentáció oldalsávjában megjelenő HTTP metódusok (GET, POST, PUT, DELETE, stb.) és a "schema" elemek stílusának testreszabásához, hogy azok jobban kiemelkedjenek és olvashatóbbak legyenek, a `src/css/custom.css` fájlba a következő CSS szabályokat kell hozzáadni. Ezáltal egységesebb és vizuálisan is vonzóbb lesz a dokumentáció.

Az alábbi CSS kód a metódusokhoz tartozó címkéket (badge-eket) alakítja ki, különböző háttérszínekkel az egyes HTTP metódusokhoz, valamint egy általános stílust biztosít az API metódusok és sémák navigációs linkjei számára.

:::info
Ezek a stílusok a [Docusaurus OpenAPI plugin demó oldaláról](https://docusaurus-openapi.tryingpan.dev/customization/styling) származnak, és kiváló kiindulási alapot nyújtanak a vizuálisan gazdagabb API dokumentációhoz.
:::

```css title="src/css/custom.css"
/* A fájl eredeti tartalma... */

.api-method > .menu__link,
.schema > .menu__link {
  align-items: center;
  justify-content: start;
}

.api-method > .menu__link::before,
.schema > .menu__link::before {
  width: 55px;
  height: 20px;
  font-size: 12px;
  line-height: 20px;
  text-transform: uppercase;
  font-weight: 600;
  border-radius: 0.25rem;
  border: 1px solid;
  margin-right: var(--ifm-spacing-horizontal);
  text-align: center;
  flex-shrink: 0;
  border-color: transparent;
  color: white;
}

.get > .menu__link::before {
  content: "get";
  background-color: var(--ifm-color-primary);
}

.post > .menu__link::before {
  content: "post";
  background-color: var(--openapi-code-green);
}

.delete > .menu__link::before {
  content: "del";
  background-color: var(--openapi-code-red);
}

.put > .menu__link::before {
  content: "put";
  background-color: var(--openapi-code-blue);
}

.patch > .menu__link::before {
  content: "patch";
  background-color: var(--openapi-code-orange);
}

.head > .menu__link::before {
  content: "head";
  background-color: var(--ifm-color-secondary-darkest);
}

.event > .menu__link::before {
  content: "event";
  background-color: var(--ifm-color-secondary-darkest);
}

.schema > .menu__link::before {
  content: "schema";
  background-color: var(--ifm-color-secondary-darkest);
}
```

:::info[Segítség]
- [PaloAltoNetworks/docusaurus-openapi-docs GitHub Repository](https://github.com/PaloAltoNetworks/docusaurus-openapi-docs)
- [PaloAltoNetworks/docusaurus-openapi-docs Documentation](https://docusaurus-openapi.tryingpan.dev/)
:::

## 2.2 API dokumentáció generálása és elérhetővé tétele

### Markdown fájlok generálása

1. **Parancs futtatása:** Futtasd a plugin parancsát a Markdown fájlok generálásához (a parancs pontos formáját ellenőrizd a plugin dokumentációjában, általában valami hasonló):
    
    <Tabs groupId='package-manager'>
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
    docs/petstore
    ```

    :::tip Miért hasznos ez?
    - **Tiszta változáskövetés:** Csak az OpenAPI specifikációs fájl (pl. `petstore-api.yaml`) változásait kell követned. A Markdown fájlok ebből automatikusan generálódnak.
    - **Automatizálás:** A CI/CD folyamat (amit a [3. feladatban](./reszfeladat3) állítasz be) felelős lesz a Markdown fájlok friss generálásáért minden build során.
    - **Lokális fejlesztés:** Attól függetlenül, hogy a generált fájlok nincsenek a Gitben, lokálisan továbbra is legenerálhatod őket (`npm run docusaurus gen-api-docs all`), hogy lásd az előnézetet és szerkeszthesd az API dokumentációt (pl. plusz információk hozzáadásával a generált fájlokhoz). **Ha az OpenAPI specifikáció (YAML) változik, újra kell generálni a fájlokat a változások megjelenítéséhez.**
    :::

1. **Fejlesztői szerver indítása és ellenőrzés:** 

    Indítsd el a fejlesztői szervert (`npm start`), és ellenőrizd, hogy a generált API dokumentáció megjelenik-e a `docs/petstore/add-pet` útvonalon (vagy ahogy a [navigációban beállítottad](#navigáció-beállítása)).

### Navigáció beállítása

Ahhoz, hogy a létrehozott API dokumentációt a navigációban is elérhetővé tegyük, el kell végeznünk néhány konfigurációs lépést.

1. **Oldalsáv (`sidebars.js`) módosítása:**

    Hivatkozz a generált API oldalsávra. Például, ha létrehozol egy külön oldalsávot az API dokumentációnak:

    ```javascript title="sidebars.js"
    module.exports = {
      // ...meglévő oldalsávjaid (pl. tutorialSidebar, guideSidebar)...
      // highlight-next-line
      myApiSidebar: require('./docs/petstore/sidebar'), // Hivatkozás a plugin által generált oldalsáv fájlra
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
          
          // highlight-start
          {
            type: 'docSidebar', // Ha külön oldalsávot használsz az API-hoz
            sidebarId: 'myApiSidebar', // Az API oldalsávjának ID-ja a sidebars.js-ből
            label: 'Petstore API',
            position: 'left',
          },
          //highlight-end

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

| Kritérium | Elvárt állapot | Elkészült |
| --------- | -------------- | :-------: |
| **`feature/api-documentation` branch** | Létezik egy `feature/api-documentation` (vagy hasonló nevű) branch a GitHub repository-ban, a `main` branch-ből kiindulva. | <input type="checkbox" /> |
| **OpenAPI specifikáció** | A specifikációs fájl (pl. `petstore-api.yaml`) létre lett hozva az `openapi/` mappában. | <input type="checkbox" /> |
| **OpenAPI plugin** | A `@paloaltonetworks/docusaurus-openapi-docs` plugin telepítve és helyesen van konfigurálva a `docusaurus.config.js`-ben. | <input type="checkbox" /> |
| **API dokumentáció generálása** | Az API dokumentáció (pl. Petstore API alapján) sikeresen legenerálódott a megadott `outputDir`-be. | <input type="checkbox" /> |
| **.gitignore (ajánlott)** | A generált dokumentáció kimeneti mappája hozzá van adva a `.gitignore` fájlhoz. | <input type="checkbox" /> |
| **Navigáció (API)** | Az API dokumentáció elérhető a Docusaurus oldal navigációs sávján és/vagy oldalsávján keresztül. | <input type="checkbox" /> |
| **Stílusok (CSS)** | A `src/css/custom.css` fájl tartalmazza az API metódusok (GET, POST, stb.) stílusdefinícióit. | <input type="checkbox" /> |
| **Pull Request (API)** | Egy merge-elt (de a branch nem törölt) PR mutat a `feature/api-documentation` branch-ből a `main` branch-be. | <input type="checkbox" /> |

___

## 🎯 Következő lépés

:::success
Nagyszerű munka! Az oldalad most már dinamikus, OpenAPI specifikációból generált API dokumentációval rendelkezik.
:::

A következő lépésben automatizálni fogjuk a build és deployment folyamatot, hogy minden változtatás automatikusan publikálódjon GitHub Pages-re.

**Folytatás:** [3. feladat - CI/CD folyamat beállítása](./reszfeladat3)
