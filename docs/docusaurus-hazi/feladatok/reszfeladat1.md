---
title: 1. feladat - Alapok
sidebar_position: 1
---

# 1. feladat: Docusaurus projekt alapok, struktúraátalakítás és új tartalmi szekció

Ebben a feladatban létrehozod a saját Docusaurus projektedet, megismerkedsz a projekt alapvető struktúrájával, feltöltöd egy GitHub repository-ba, majd egy új branch-en komolyabb strukturális átalakításokat és bővítéseket hajtasz végre.

## ⏱️ Becsült időigény

| Szakasz | Időigény | Megjegyzés |
|---------|----------|------------|
| **Docusaurus telepítés és inicializálás** | 15-20 perc | Első alkalommal +10-15 perc |
| **GitHub repo létrehozása és feltöltés** | 10-15 perc | Git ismeretek függvényében |
| **Struktúraátalakítás és új szekció** | 30-45 perc | Több oldal létrehozása, konfiguráció |
| **Összesen** | **60-90 perc** | + 30-45 perc, ha még nincs környezet telepítve |

## 📋 Előfeltételek

Mielőtt nekikezdenél ennek a feladatnak, győződj meg róla, hogy:

| Előfeltétel | Ellenőrzés | Hol találod |
|-------------|------------|-------------|
| **Node.js és NPM telepítve** | `node -v` és `npm -v` parancsok működnek | [Előkészületek](../elokeszuletek#2-nodejs-és-npm-telepítése) |
| **Git telepítve és konfigurálva** | `git --version` és `git config user.name` működik | [Előkészületek](../elokeszuletek#4-git-és-github-cli) |
| **GitHub fiók létrehozva** | Be tudsz jelentkezni a github.com-ra | [Előkészületek](../elokeszuletek#5-github-regisztráció-és-repo) |
| **Kódszerkesztő telepítve** | VSCode vagy más szerkesztő elérhető | [Előkészületek](../elokeszuletek#1-visual-studio-code-telepítése) |

:::tip
Ha valamelyik előfeltétel hiányzik, látogass el az [Előkészületek](../elokeszuletek) oldalra!
:::

## 1.1 Alapvető Docusaurus projekt és GitHub feltöltés (`main` branch)

### Docusaurus telepítése és projekt inicializálása

Először is, hozzuk létre a Docusaurus projektünk alapjait.

1. **Parancssor megnyitása:** Nyiss meg egy parancssoros terminált a gépeden.

1.  **Node.js ellenőrzése:** Győződj meg róla, hogy a gépeden telepítve van a Node.js (LTS verzió ajánlott).

    ```bash
    node -v
    npm -v
    ```

    :::tip
    Ha még nincs Node.js a gépeden, itt találsz további információt a letöltésről és telepítésről: [Előkészületek: Node.js és NPM telepítése](../elokeszuletek#2-nodejs-és-npm-telepítése).
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
1.  Módosítsd a `docusaurus.config.js` fájlban az oldal címét (`title`) és a `tagline`-t a saját projektednek megfelelően.

1. **Egyszerű szerkesztés:** Próbálj meg egy egyszerű Markdown fájlt létrehozni a `docs` mappában, és nézd meg, hogyan jelenik meg az oldalon.

    **Mi az a frontmatter?**

    A frontmatter egy YAML formátumú metaadat blokk, amely minden Markdown dokumentum elején található. Három kötőjel (`---`) közé kerül, és különböző beállításokat tartalmaz az oldalhoz.

    Példa egy minimális frontmatter-re:
    ```markdown
    ---
    title: Az oldal címe
    sidebar_position: 1
    ---

    # Az oldal tartalma itt kezdődik
    ```

    **Legfontosabb frontmatter mezők:**
    - `title`: Az oldal címe, ami az oldalsávban és a böngésző címsorában jelenik meg
    - `sidebar_position`: Az oldal pozíciója az oldalsávban (pl. 1, 2, 3...)
    - `description`: Rövid leírás az oldalról (SEO-hoz hasznos)
    - `sidebar_label`: Ha más címet szeretnél az oldalsávban, mint a `title`

    :::tip
    **Itt a kísérletezés ideje!** Próbálj ki különböző Markdown formázásokat és frontmatter beállításokat, hogy ráérezz, milyen lehetőségeket rejt!
    :::

:::info[Segítség]
- [Docusaurus Installation](https://docusaurus.io/docs/installation)
- [Docusaur Docs - Project Structure](https://docusaurus.io/docs/category/guides)
- [Docusaur Docs - Configuration](https://docusaurus.io/docs/configuration)
- [Docusaur Docs - Sidebar](https://docusaurus.io/docs/sidebar)
- [Docusaur Docs - Doc front matter](https://docusaurus.io/docs/create-doc#doc-front-matter)
:::

### Git alapfogalmak rövid áttekintése

Mielőtt nekikezdenél a verziókezelésnek, itt egy gyors összefoglaló a legfontosabb Git fogalmakról:

| Fogalom | Mit jelent? | Mikor használod? |
|---------|-------------|------------------|
| **Repository (repo)** | A projekt verziókezelt mappája, amely tartalmazza az összes fájlt és a változások történetét | Projekt kezdésekor létrehozod (`git init`) |
| **Commit** | Egy "pillanatkép" a projektedről egy adott időpontban. Tartalmazza a változásokat és egy leíró üzenetet | Amikor elmentesz egy logikai egységnyi változást (`git commit`) |
| **Branch** | A fejlesztés egy független ága. Lehetővé teszi, hogy párhuzamosan dolgozz különböző funkciókon | Új funkció vagy javítás kezdésekor (`git checkout -b`) |
| **Checkout** | Branch váltás vagy fájlok visszaállítása egy korábbi verzióra | Branch-ek közötti váltáskor (`git checkout <branch-név>`) vagy új branch létrehozásakor (`git checkout -b <branch-név>`) |
| **Main/Master** | Az alapértelmezett, "főág" branch, általában az éles kód | Ez a stabil, publikálásra kész verzió |
| **Push** | Feltölti a lokális commit-jaidat a távoli repository-ba (pl. GitHub-ra) | Amikor meg akarod osztani a változásaidat (`git push`) |
| **Pull** | Letölti a távoli repository változásait a lokális gépedre | Mások változásainak beszerzésekor (`git pull`) |
| **Remote** | Egy távoli repository (pl. GitHub-on), ahova feltöltöd a kódodat | Kapcsolat létrehozásakor (`git remote add origin`) |
| **Pull Request (PR)** | Kérelem a változások beolvasztására egy branch-ből a másikba | Code review és együttműködés esetén |

:::tip[Bővebben a Git-ről]
Ha még nem vagy teljesen otthon a Git használatában, nézd meg ezt a remek bevezető útmutatót: [Git Handbook](https://guides.github.com/introduction/git-handbook/)
:::

### GitHub repository létrehozása és projekt feltöltése

Most verziókezeljük a projektünket.

1.  **Új repository GitHubon:** Hozz létre egy új, publikus repository-t a GitHubon. **Ne inicializáld `README`, `.gitignore` vagy `licenc` fájllal**, ezeket a Docusaurus projekt már tartalmazza (vagy nincs rájuk szükség).

    A feltöltéshez a legegyszerűbb, ha követed a frissen létrehozott repo-dban szereplő instrukciókat. Az ott szereplő parancsokat futtasd egy terminálban.

1.  **Lokális git inicializálás:** A Docusaurus projekted mappájában:

    ```bash
    # Létrehoz egy új git repository-t a mappában
    git init
    
    # Hozzáadja az összes fájlt a staging area-hoz
    git add .
    
    # Létrehozza az első commit-ot
    git commit -m "Initial Docusaurus project setup"
    
    # Átnevezi az aktuális branch-et main-re (ha még nem az lenne)
    git branch -M main
    ```

1.  **Távoli repository hozzáadása és feltöltés:**

    ```bash
    # Összeköti a lokális repository-t a távoli GitHub repository-val
    git remote add origin <A_TE_REPOSITORY-D_URL-JE>
    
    # Feltölti a main branch tartalmát és beállítja az upstream-et
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
          // highlight-next-line
          tutorialSidebar: [{type: 'autogenerated', dirName: 'tutorials'}], 
          // Az eredeti ez volt: tutorialSidebar: [{type: 'autogenerated', dirName: '.'}],

          //...
        };
        module.exports = sidebars;
        ```

    - **`docusaurus.config.js` (navbar) frissítése (opcionális):** Ha a navigációs sávban (`themeConfig.navbar.items`) volt közvetlen link a régi dokumentációs oldalakra, frissítsd az elérési utakat (pl. `to: '/docs/tutorials/intro'`).

        ```javascript title="docusaurus.config.js"
        navbar: {
          title: 'My Site',
          logo: {
            alt: 'My Site Logo',
            src: 'img/logo.svg',
          },
          items: [
            // Hagyhatod ezt az eredeti definíció szerint is, hiszen a `tutorialSidebar` ID alapján megtalálja a navigációs elemet a program. Ezt akkor érdemes változtatni, ha közvetlenül linkelsz a `to` paraméterrel.
            // highlight-start
            {
              to: '/docs/tutorials/intro',
              position: 'left',
              label: 'Tutorial',
            },
            // highlight-end
            {to: '/blog', label: 'Blog', position: 'left'},
          ],
        },
        ```

        :::note
        A törött linkeket egy `build` futtatással is könnyen ellenőrizheted. Lehetséges, hogy a `themeConfig.footer` is tartalmaz törött linkeket.
        :::

1.  **Ellenőrzés:** Indítsd el a fejlesztői szervert (`npm start`), és ellenőrizd, hogy az oldal hibátlanul felépül-e, a linkek a "Tutorials" szekcióban működnek-e, és az oldalsáv helyesen jelenik-e meg.

    :::tip
    Ha a build folyamat hibát dob, ellenőrizd a konfigurációt, lehet valamelyik ellenőrzés (`onBrokenLinks`, `onBrokenMarkdownLinks`, `onBrokenAnchors`) értéke `throw`. Ilyen esetben állítsd át ezt eggyel megengedőbb figyelmeztetési szintre, pl. `warn`.
    :::

:::info[Segítség]
[Docusaurus Sidebar Docs](https://docusaurus.io/docs/sidebar)
:::

### Új dokumentációs szekció létrehozása (`/docs/guides`)

<details>

<summary>

#### Hogyan generálódik az oldalsáv? {#sidebar-generalas}

</summary>

Amikor a `sidebars.js`-ben egy oldalsávban `type: 'autogenerated'` szerepel, a Docusaurus a `docs/<dirName>` mappa struktúrájából építi fel az oldalsávot:

- Minden `.md/.mdx` fájl egy "oldal" az oldalsávban.
- Minden almappa egy "kategória", amit a mappán belüli `_category_.json` metaadatai (pl. `label`, `position`, `link`) tudnak finomhangolni.

Ugyanazon a szinten a fájlok (`sidebar_position`) és a mappák/kategóriák (`_category_.json` `position`) **egy közös listába rendeződnek**.

Ha két elem ugyanazt a pozíciót kapja ugyanazon a szinten, a Docusaurus nem hibázik, hanem a név/útvonal alapján dönt a sorrendről — ez gyakran "véletlenszerűnek" tűnik, ha csak a pozíciókra számítottál.

Példa (egy szint: `docs/guides/`):

```text
docs/guides/
├─ installation-guide.md      (sidebar_position: 1)
├─ configuration-tips.md      (sidebar_position: 2)
└─ advanced-topics/           (_category_.json position: 3)
```

Ütközés példa (kerüld):

```text
docs/guides/
├─ configuration-tips.md      (sidebar_position: 2)
└─ advanced-topics/           (_category_.json position: 2)
```

</details>

Bővítsük a dokumentációt egy új szekcióval.

1.  **`guides` almappa létrehozása:** A `docs` mappán belül hozz létre egy `guides` almappát.

1.  **Új oldalak és almappák létrehozása:**

    - A `guides` mappában hozz létre több Markdown oldalt (legalább 2-3 oldalt, pl. `installation-guide.md`, `configuration-tips.md`).
    - Hozz létre legalább egy almappát is a `guides`-on belül (pl. `advanced-topics`), és abba is helyezz el legalább 2 oldalt (pl. `advanced-topics/api-integration.md`).

1.  **Frontmatter és sidebar gyakorlása:**
    - Használd a `sidebar_position` frontmatter attribútumot az oldalak sorrendjének beállításához.
    - Használj `title`-t, ha az oldalsávban más címet szeretnél megjeleníteni.

      Példa a frontmatter-re, ezt minden Markdown fájlhoz add hozzá a legelején:

      ```markdown title="new-page.md"
      ---
      title: Új oldal
      sidebar_position: 1
      ---
      ```

    - Definiálj kategóriákat `_category_.json` fájlok segítségével. Adj meg nekik `label`-t és `position`-t.

      :::tip
      A `position` és a `sidebar_position` ugyanazon a szinten "összeér" (fájlok + mappák együtt rendeződnek), ezért figyelj, hogy ne legyen ütközés. Részletek: [Hogyan generálódik az oldalsáv?](#sidebar-generalas)
      :::

      Példa egy `docs/guides/advanced-topics/_category_.json` fájlra:

        ```json title="_category_.json"
        {
          "label": "Haladó Témák",
          "position": 3,
          "link": {
            "type": "generated-index",
            "description": "Itt találhatóak a haladó szintű útmutatók."
          }
        }
        ```

        :::note
        A `link` objektummal a Docusaurus automatikusan generál egy index oldalt a kategória számára. 
        
        Ez az oldal áttekintést nyújt a kategóriában található dokumentumokról, és a `description` mezőben megadott szöveget is megjeleníti. 
        
        Ezáltal a felhasználók könnyebben navigálhatnak és átfogó képet kaphatnak a kategória tartalmáról anélkül, hogy az összes oldalt külön-külön meg kellene nyitniuk.
        :::

:::info[Segítség]
- [Docusaurus Docs - Sidebar](https://docusaurus.io/docs/sidebar)
- [Docusaurus Docs - Front Matter](https://docusaurus.io/docs/api/plugins/@docusaurus/plugin-content-docs#markdown-front-matter)
- [Sidebar - Category Generated Index](https://docusaurus.io/docs/sidebar/items#generated-index-page)
:::

### Új "Guides" szekció megjelenítése

Integráljuk az új szekciót a navigációba.

1.  **Oldalsáv konfigurálása a "Guides"-hoz:** A `sidebars.js` fájlban:

    ```javascript title="sidebars.js"
    const sidebars = {
      tutorialSidebar: [ /* ... a tutorials konfigurációja ... */ ],
      // highlight-start
      guideSidebar: [ // Új oldalsáv a guides-nak
        {
          type: 'autogenerated',
          dirName: 'guides',
        },
      ],
      // highlight-end
    };
    module.exports = sidebars;
    ```

1.  **Navigációs sáv frissítése:** Adj hozzá egy új linket a `docusaurus.config.js` `themeConfig.navbar.items` tömbjéhez, ami az új `guides` szekcióra mutat.

    ```javascript title="docusaurus.config.js"
    // ...
    themeConfig: {
      navbar: {
        items: [
          {
            type: 'docSidebar',
            sidebarId: 'tutorialSidebar', // A tutorials oldalsáv ID-ja
            position: 'left',
            label: 'Tutorial',
          },
          // highlight-start
          {
            type: 'docSidebar',
            sidebarId: 'guideSidebar',
            position: 'left',
            label: 'Guides',
          },
          // highlight-end
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

| Kritérium | Elvárt állapot | Kész |
| --------- |--------------- | :-------: |
| **Alap Docusaurus projekt** | Egy működő, alap Docusaurus projekt létezik a `main` branch-en a GitHub repository-ban. | <input type="checkbox" /> |
| **`feature/advanced-structure` branch** | Létezik egy `feature/advanced-structure` (vagy hasonló nevű) branch a GitHub repository-ban. | <input type="checkbox" /> |
| **Dokumentáció átstrukturálása (`docs/tutorials`)** | Az eredeti dokumentáció a `docs/tutorials` mappába lett áthelyezve, az oldalsáv és a navigáció helyesen konfigurálva. | <input type="checkbox" /> |
| **Új dokumentációs szekció (`docs/guides`)** | Létrejött egy új `docs/guides` mappa több oldallal és almappákkal. | <input type="checkbox" /> |
| **Frontmatter használata** | A `docs/guides` szekcióban a `sidebar_position`, `title`, és `_category_.json` használata demonstrálva van. | <input type="checkbox" /> |
| **Navigáció (`guides` szekció)** | Az új `guides` szekció megjelenik a navigációs sávban és/vagy az oldalsáv(ak)ban, és az oldalak elérhetőek. | <input type="checkbox" /> |
| **Pull Request (struktúra)** | Egy merge-elt (de a branch nem törölt) PR mutat a `feature/advanced-structure` branch-ből a `main` branch-be. | <input type="checkbox" /> |

___

## 🎯 Következő lépés

:::success
Gratulálunk! Sikeresen létrehoztad a Docusaurus projektedet, strukturáltad a dokumentációt, és feltöltötted GitHub-ra.
:::

Most, hogy az alapok megvannak, ideje dinamikus tartalommal bővíteni az oldalt.

**Folytatás:** [2. feladat - API dokumentáció integrálása](./reszfeladat2)
