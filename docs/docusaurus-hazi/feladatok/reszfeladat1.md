---
title: 1. feladat - Alapok
sidebar_position: 1
---

# 1. feladat: Docusaurus projekt alapok, struktúraátalakítás és új tartalmi szekció

Ebben a feladatban létrehozod a saját Docusaurus projektedet, megismerkedsz az alapvető struktúrával, feltöltöd egy GitHub repository-ba, majd egy új branch-en komolyabb strukturális átalakításokat és bővítéseket hajtasz végre.

## 1.1 Alapvető Docusaurus projekt és GitHub feltöltés (`main` branch)

### Docusaurus telepítése és projekt inicializálása

Először is, hozzuk létre a Docusaurus projektünk alapjait.

1. **Prancssor megnyitása:** Nyiss meg egy parancssoros terminált a gépeden.

1.  **Node.js ellenőrzése:** Győződj meg róla, hogy a gépeden telepítve van a Node.js (LTS verzió ajánlott).

    ```bash
    node -v
    npm -v
    ```

    :::tip
    Ha még nincs Node.js a gépeden, itt találsz további információt a letöltésről és telepítésről: [Download Node.js](https://nodejs.org/en/download/).
    :::

1.  **Docusaurus projekt létrehozása:** Navigálj egy olyan mappába, ahová létre szeretnéd hozni az új projektet. Hozd létre a projektet a `classic` template alapján. A `my-docs-site` helyett válassz egyedi nevet. Válaszd a `JavaScript` opciót, ennek egyszerűbb átlátni a konfigurációját.

    ```bash
    npx create-docusaurus@latest my-docs-site classic
    ```

    :::note
     Ha otthonosan mozogsz a fejlesztői csomagok világában, itt más telepítési opcióval is élhetsz, pl. más package manager, konfiguráció, stb. [Docusaurus: Installation](https://docusaurus.io/docs/installation)
    :::

1.  **Navigálás és indítás:** Lépj be a projekt mappájába, és indítsd el a fejlesztői szervert.

    ```bash
    cd my-docs-site
    npm start
    ```
    Az oldalnak elérhetőnek kell lennie a böngészőben, általában `http://localhost:3000` címen. Navigálj kicsit az oldalon, ismerkedj meg az alapvető felhasználói élménnyel. Érdemes lehet a már meglévő tartalmakat átolvasni, hogy átlásd a szerkesztés alapjait.

1.  **Ismerkedés a struktúrával:** Nézd át a generált mappákat (`docs/`, `blog/`, `src/pages/`) és konfigurációs fájlokat (`docusaurus.config.js`, `sidebars.js`).
    - Módosítsd a `docusaurus.config.js` fájlban az oldal címét (`title`) és a `tagline`-t a saját projektednek megfelelően.

1. **Egyszerű szerkesztés:** Próbálj meg egy egyszerű Markdown fájlt létrehozni a `docs` mappában, és nézd meg, hogyan jelenik meg az oldalon. Figyelj a szükséges 'frontmatter' meglétére (`title`, `sidebar_position`). 

    :::tip
    **Itt a kísérletezés ideje!** Próbálj ki különböző Markdown formázásokat, hogy ráérezz, milyen lehetőségeket rejt!
    :::

:::info[Segítség]
- [Docusaurus Installation](https://docusaurus.io/docs/installation)
- [Docusaur Docs - Project Structure](https://docusaurus.io/docs/category/guides)
- [Docusaur Docs - Configuration](https://docusaurus.io/docs/configuration)
- [Docusaur Docs - Sidebar](https://docusaurus.io/docs/sidebar)
- [Docusaur Docs - Doc front matter](https://docusaurus.io/docs/create-doc#doc-front-matter)
:::

### GitHub repository létrehozása és projekt feltöltése

Most verziókezeljük a projektünket.

1.  **Új repository GitHubon:** Hozz létre egy új, publikus repository-t a GitHubon. **Ne inicializáld `README`, `.gitignore` vagy `licenc` fájllal**, ezeket a Docusaurus projekt már tartalmazza (vagy nincs rájuk szükség).

    A feltöltéshez a legegyszerűbb, ha követed a frissen létrehozott repo-dban szereplő instrukciókat. Az ott szereplő parancsokat futtasd egy terminálban.

1.  **Lokális git inicializálás:** A Docusaurus projekted mappájában:

    ```bash
    git init
    git add .
    git commit -m "Initial Docusaurus project setup"
    ```

1.  **Távoli repository hozzáadása és feltöltés:**

    ```bash
    git remote add origin <A_TE_REPOSITORY-D_URL-JE>
    git push -u origin main
    ```

:::info[Segítség]
[Adding locally hosted code to GitHub](https://docs.github.com/en/migrations/importing-source-code/using-the-command-line-to-import-source-code/adding-locally-hosted-code-to-github)
:::

## 1.2 Haladóbb szerkesztések és struktúraátalakítás (új branch-en)

Miután az alap projekt fent van a `main` branch-en, a következő lépéseket egy **új branch-en** végezd el.

### Új branch létrehozása

Nevezd el például `feature/advanced-structure`-nek:

```bash
git checkout -b feature/advanced-structure
```

### Meglévő dokumentáció átstrukturálása (`/docs/tutorials`)

Szervezzük át a kezdeti dokumentációt.

1.  **`tutorials` almappa létrehozása:** A `docs` mappán belül hozz létre egy `tutorials` almappát.

1.  **Fájlok áthelyezése:** Mozgasd át az eddigi, alapértelmezetten generált dokumentációs `.md` fájlokat (pl. `intro.md`, és a `tutorial-basics`, `tutorial-extras` mappák tartalmát, ha a `classic` template-et használtad) ebbe az új `tutorials` mappába.

1.  **Konfigurációk frissítése az átstrukturáláshoz:**

    - **`sidebars.js` módosítása:** Frissítsd az oldalsáv konfigurációját, hogy az elérési utak a `tutorials/` előtaggal kezdődjenek.

        ```javascript title="sidebars.js"
        const sidebars = {
          tutorialSidebar: [{type: 'autogenerated', dirName: 'tutorials'}],

          //...
        };
        module.exports = sidebars;
        ```

    - **`docusaurus.config.js` (navbar) Frissítése:** Ha a navigációs sávban (`themeConfig.navbar.items`) volt közvetlen link a régi dokumentációs oldalakra, frissítsd az elérési utakat (pl. `to: '/docs/tutorials/intro'`).

        ```javascript title="docusaurus.config.js"
        navbar: {
          title: 'My Site',
          logo: {
            alt: 'My Site Logo',
            src: 'img/logo.svg',
          },
          items: [
            {
              to: '/docs/tutorials/intro',
              position: 'left',
              label: 'Tutorial',
            },
            {to: '/blog', label: 'Blog', position: 'left'},
          ],
        },
        ```

        :::note
        A törött linkeket egy `build` futtatással is könnyen ellenőrizheted. Lehetséges, hogy a `themeConfig.footer` is tartalmaz törött linkeket.
        :::

1.  **Ellenőrzés:** Indítsd el a fejlesztői szervert (`npm start`), és ellenőrizd, hogy az oldal hibátlanul felépül-e, a linkek a "Tutorials" szekcióban működnek-e, és az oldalsáv helyesen jelenik-e meg.

    :::tip
    Ha a build folyamat hibát dob, ellenőrizd a configurációt, lehet valamelyik ellenőrzés (`onBrokenLinks`, `onBrokenMarkdownLinks`, `onBrokenAnchors`) értéke `throw`. Ilyen esetben állítsd át ezt eggyel megengedőbb figyelmeztetési szintre, pl. `warn`.
    :::

:::info[Segítség]
[Docusaurus Sidebar Docs](https://docusaurus.io/docs/sidebar)
:::

### Új dokumentációs szekció létrehozása (`/docs/guides`)

Bővítsük a dokumentációt egy új szekcióval.

1.  **`guides` almappa létrehozása:** A `docs` mappán belül hozz létre egy `guides` almappát.

1.  **Új oldalak és almappák létrehozása:**

    - A `guides` mappában hozz létre több Markdown oldalt (legalább 2-3 oldalt, pl. `installation-guide.md`, `configuration-tips.md`).
    - Hozz létre legalább egy almappát is a `guides`-on belül (pl. `advanced-topics`), és abba is helyezz el legalább 2 oldalt (pl. `advanced-topics/api-integration.md`).

1.  **Frontmatter gyakorlása:**
    - Használd a `sidebar_position` frontmatter attribútumot az oldalak sorrendjének beállításához.
    - Használj `title`-t, ha az oldalsávban más címet szeretnél megjeleníteni.
    - Definiálj kategóriákat `_category_.json` fájlok segítségével. Adj meg nekik `label`-t és `position`-t.
    
      Példa egy `docs/guides/advanced-topics/_category_.json` fájlra:

        ```json title="_category_.json"
        {
          "label": "Haladó Témák",
          "position": 2,
          "link": {
            "type": "generated-index",
            "description": "Itt találhatóak a haladó szintű útmutatók."
          }
        }
        ```

:::info[Segítség]
- [Docusaurus Docs - Siedebar](https://docusaurus.io/docs/sidebar)
- [Docusaurus Docs - Front Matter](https://docusaurus.io/docs/api/plugins/@docusaurus/plugin-content-docs#markdown-front-matter)
- [Sidebar - Category Generated Index](https://docusaurus.io/docs/sidebar/items#generated-index-page)
:::

### Új "Guides" szekció megjelenítése

Integráljuk az új szekciót a navigációba.

1.  **Sidebar konfigurálása a "Guides"-hoz:** A `sidebars.js` fájlban:

      ```javascript title="sidebars.js"
      const sidebars = {
        tutorialSidebar: [ /* ... a tutorials konfigurációja ... */ ],
        guideSidebar: [ // Új oldalsáv a guides-nak
          {
            type: 'autogenerated',
            dirName: 'guides',
          },
        ],
      };
      module.exports = sidebars;
      ```

1.  **Navbar frissítése:** Adj hozzá egy új linket a `docusaurus.config.js` `themeConfig.navbar.items` tömbjéhez, ami az új `guides` szekcióra mutat.

    ```javascript title="docusaurus.config.js"
    // ...
    themeConfig: {
      navbar: {
        items: [
          {
            to: '/docs/tutorials/intro' // Ez a sor opcionális, akkor használd, ha konkrét oldalra szeretnél mutatni a navbar-ban
            type: 'docSidebar',
            sidebarId: 'tutorialSidebar', // A tutorials oldalsáv ID-ja
            position: 'left',
            label: 'Tutorials',
          },
          {
            to: '/docs/guides/installation-guide', // Az új guides szekció első oldalának slug-ja, vagy elérési útja a mappa szerkezetben
            // Idézd fel az órán elhangzottak alapján, ennek a megoldásnak a sajátosságait - lehet sidebarId-vel jobb lehet ezt behivatkozni, ha sok változtatásra számítasz
            label: 'Guides',
            position: 'left',
            // Ha külön oldalsávot szeretnél neki:
            // type: 'docSidebar',
            // sidebarId: 'guideSidebar',
          },
          // ...többi navbar item
        ],
      },
      // ...
    },
    // ...
    ```

1.  **Ellenőrzés:** Indítsd el a fejlesztői szervert (`npm start`), és győződj meg róla, hogy az új **Guides** szekció és annak tartalma helyesen jelenik meg.

### Változások feltöltése és pull request létrehozása

Mentsük el a munkánkat a GitHub-ra.

1.  **Commit és push:**

    ```bash
    git add .
    git commit -m "Feat: Restructure docs, add guides section with frontmatter"
    git push -u origin feature/advanced-structure
    ```

1.  **Pull Request létrehozása:** A GitHub felületén hozz létre egy Pull Request-et a `feature/advanced-structure` branch-ből a `main` branch-be. Adj neki egyértelmű címet és rövid leírást.

:::danger[Fontos]
Ezt a Pull Requestet merge-lheted, viszont **NE TÖRÖLD A BRANCH-ET!** Ez a PR és a branch arra szolgál, hogy az oktató átnézhesse és értékelhesse az első részfeladatot.
:::

___

## Elvárás az 1. feladat végére

| Kritérium | Elvárt állapot |
| --------- |--------------- |
| **Alap Docusaurus projekt** | Egy működő, alap Docusaurus projekt létezik a `main` branch-en a GitHub repository-ban. |
| **`feature/advanced-structure` branch** | Létezik egy `feature/advanced-structure` (vagy hasonló nevű) branch a GitHub repository-ban. |
| **Dokumentáció átstrukturálása (`docs/tutorials`)** | Az eredeti dokumentáció a `docs/tutorials` mappába lett áthelyezve, az oldalsáv és a navigáció helyesen konfigurálva. |
| **Új dokumentációs szekció (`docs/guides`)** | Létrejött egy új `docs/guides` mappa több oldallal és almappákkal. |
| **Frontmatter használata** | A `docs/guides` szekcióban a `sidebar_position`, `title`, és `_category_.json` használata demonstrálva van. |
| **Navigáció (`guides` szekció)** | Az új `guides` szekció megjelenik a navigációs sávban és/vagy az oldalsáv(ak)ban, és az oldalak elérhetőek. |
| **Pull Request (struktúra)** | Egy merge-elt (de a branch nem törölt) PR mutat a `feature/advanced-structure` branch-ből a `main` branch-be. |
