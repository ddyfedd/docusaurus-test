---
title: 4. feladat - Review folyamat
sidebar_position: 4
---

import Tabs from '@theme/Tabs'
import TabItem from '@theme/TabItem'

# 4. feladat: Együttműködés szimulálása - változások kezelése és review folyamat

Az utolsó feladatban egy egyszerűsített review folyamatot fogsz szimulálni, ahogy az a valós projektekben is történik.

Ez magában foglalja a változtatások külön branch-en történő fejlesztését, Pull Request (PR) létrehozását, és (opcionálisan) a PR ellenőrzését és branch protection szabályok alkalmazását.

## ⏱️ Becsült időigény

| Szakasz | Időigény | Megjegyzés |
|---------|----------|------------|
| **Branch létrehozása és változtatások** | 10-15 perc | Tartalom szerkesztés, új blogbejegyzés |
| **Pull Request létrehozása** | 5-10 perc | PR leírás, review kérés |
| **Branch Protection beállítása** | 10-15 perc | Első alkalommal +5 perc |
| **Review szimuláció és merge** | 5-10 perc | Változtatások, status check |
| **Összesen** | **30-45 perc** | Review-tól függően |

## 📋 Előfeltételek

Mielőtt nekikezdenél ennek a feladatnak, győződj meg róla, hogy:

| Előfeltétel | Ellenőrzés | Hol találod |
|-------------|------------|-------------|
| **1-3. feladatok elvégezve** | Működő projekt CI/CD-vel a `main` branch-en | [1](./reszfeladat1), [2](./reszfeladat2), [3](./reszfeladat3). feladat |
| **GitHub Pages él** | Az oldalad elérhető a `<username>.github.io/<repo>` címen | [3. feladat](./reszfeladat3) |
| **CI/CD workflow működik** | A `deploy.yml` sikeresen lefut main push-ra | [3. feladat](./reszfeladat3) |
| **Git alapok ismerete** | Branch, commit, push, PR műveleteket ismered | [1. feladat](./reszfeladat1#🔑-git-alapfogalmak-rövid-áttekintése) |

:::tip
Ha a CI/CD még nem működik stabilan, előbb azt javítsd ki!
:::

## Mielőtt elkezdenéd


Bizonyosodj meg róla, hogy a `main` branch-ed tartalmazza az 1., 2. és 3. feladatok eredményeit (azaz a strukturált "Tutorials" és "Guides" szekciókat, az integrált API dokumentációt, és a beállított CI/CD folyamatot, ami a `main` branch-re sikeresen deploy-olja az oldalt GitHub Pages-re).

:::tip
Ha beállítottad a [3. részfeladatban](./reszfeladat3) az opcionális `test.yml` workflow-t (ami PR-ekre fut le), az most hasznos lesz.
:::

## 4.1 Új branch létrehozása a változtatásoknak

Minden új funkciót, javítást vagy nagyobb módosítást érdemes külön branch-en fejleszteni.

1.  **Visszaváltás `main`-re és frissítés:**

    ```bash
    git checkout main
    git pull origin main
    ```

1.  **Új branch létrehozása:** Hozz létre egy új branch-et a tervezett módosításaidnak, például:

    ```bash
    git checkout -b feature/update-content-and-review
    ```

## 4.2 Változtatások végrehajtása

Végezz el néhány egyszerűbb módosítást a Docusaurus oldaladon ezen az új branch-en. Például:

1.  **Meglévő tartalom szerkesztése:**

    - Nyisd meg az egyik korábban létrehozott "Guides" vagy "Tutorials" oldalt, és végezz rajta valamilyen tartalmi módosítást (pl. adj hozzá egy új bekezdést, javíts ki egy elírást, frissíts egy linket).
    - Akár szándékosan illessz be egy törött linket, hogy megfigyelhesd a build és teszt folyamataid viselkedését. Szerinted az ilyen eseteket, milyen szigorúan kéne kezelni? Ha van kedved, ehhez mérten állíts be szabályokat a workflow-idban.

      :::tip A törött linkek kezelése
      A `docusaurus.config.js` fájlban az `onBrokenLinks` és `onBrokenAnchors` opciók segítségével szabályozhatod, hogyan reagáljon a Docusaurus a törött linkekre. Ha ezeket `'throw'` értékre állítod, a build folyamat hibával leáll, ha törött linket talál. 
      
      Ez azt eredményezi, hogy a `test.yml` workflow is meghiúsul törött linkek detektálásakor, ami - a (később) beállított Branch Protection szabályoknak köszönhetően - blokkolja a Pull Request merge-elését. Így garantálható, hogy nem kerül hibás link az éles oldalra.
      :::


1.  **Új blogbejegyzés írása (opcionális):**

    - Hozz létre egy új Markdown fájlt a `blog` mappában (pl. `YYYY-MM-DD-my-latest-thoughts.md`).
    - Írj egy rövid blogbejegyzést a Docusaurus használatával kapcsolatos tapasztalataidról vagy bármilyen más releváns témáról. Adj neki címet és esetleg címkéket (tags) a frontmatter-ben.
    - Gyakorolhatod az `authors.yml` fájl létrehozását/módosítását és frontmatter-ben hivatkozását, hogy feltűntesd magad a bejegyzés szerzőjeként.

      <Tabs groupId="author-setup">
      <TabItem value="authors-yml" label="authors.yml">

      Ez a `blog/authors.yml` fájl struktúrája, ahol definiálhatod a blogbejegyzések szerzőit:

      ```yaml title="blog/authors.yml"
      # Add your own authors here
      your_name:
        name: Your Name
        title: Your Title
        url: https://your-website.com
        image_url: https://your-website.com/img/profile.jpg
      ```

      </TabItem>
      <TabItem value="blog-frontmatter" label="Blog frontmatter">

      A blogbejegyzés `frontmatter`-ében így hivatkozhatsz a definiált szerzőre:

      ```markdown title="blog/YYYY-MM-DD-my-latest-thoughts.md"
      ---
      title: My Latest Thoughts
      date: 2023-11-30
      authors: [your_name] # Az authors.yml-ben definiált szerző ID-je
      tags: [docusaurus, blog, frontend]
      ---

      Hello World! This is my first blog post.
      ```

      </TabItem>
      </Tabs>

1.  **Apróbb konfigurációs módosítás (opcionális):**
    - Módosíts valamit a `docusaurus.config.js`-ben, például a lábléc (`footer`) tartalmát vagy egy navbar link szövegét.

## 4.3 Változások commitálása és feltöltése

Mentsd el a munkádat.

1.  **Változások hozzáadása és commit:**

    ```bash
    git add .
    git commit -m "Docs: Update content and prepare for review" 
    ```

    :::tip Atomikus commitek fontossága
    Bár a `git add .` kényelmes, érdemes megfontolni a változások atomikusabb hozzáadását a staging területhez (`git add <fájlnév>`) vagy a `git add -p` (patch) parancs használatát. Ez lehetővé teszi, hogy a commitok kisebbek és fókuszáltabbak legyenek, ami megkönnyíti a Git history áttekintését és a hibakeresést.

    **Miért hasznos ez?**
    *   **Tisztább history:** Egy-egy commit egy logikai változást takar, így könnyebb megérteni, miért és hogyan történt egy módosítás.
    *   **Egyszerűbb review:** A kisebb, fókuszált commit-ok sokkal könnyebben áttekinthetők a Pull Request review során.
    *   **Célzott hibakeresés:** Ha egy hiba kerül a kódba, a `git bisect`/`git blame` sokkal hatékonyabban tudja megtalálni a hibás commit-ot, ha azok atomikusak.
    *   **Rugalmasabb változtatások:** Könnyebb egy-egy commit-ot visszaállítani (`git revert`), ha valami probléma merül fel, anélkül, hogy más, helyes változásokat is elveszítenél.
    :::

1.  **Változások feltöltése:**

    ```bash
    git push -u origin feature/update-content-and-review
    ```

## 4.4 Pull Request (PR) létrehozása

Most kérj véleményezést a változtatásaidról.

1.  **PR nyitása:** A GitHub felületén hozz létre egy Pull Requestet a `feature/update-content-and-review` branch-ből a `main` branch-be.
1.  **PR leírása:** Adj egy rövid, de informatív címet és leírást a PR-nek, összefoglalva a végrehajtott változtatásokat. Hivatkozhatsz a házi feladat ezen részére.

:::tip Haladó: PR Sablonok és Automatizáció
A konzisztens és informatív PR leírások érdekében érdemes bevezetni egy [Pull Request sablont](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/creating-a-pull-request-template-for-your-repository) (pl. `.github/pull_request_template.md`). 

Ez a sablon automatikusan megjelenik minden új PR létrehozásakor, és segíti a fejlesztőket a szükséges információk megadásában (pl. változások célja, tesztelési lépések, kapcsolódó ticket-ek).

**Automatizációs ötlet**

    Egy GitHub Actions workflow segítségével még tovább léphetsz! Készíthetsz egy olyan folyamatot, ami a PR létrehozásakor (vagy frissítésekor) automatikusan kigyűjti a megváltozott fájlok listáját (`git diff --name-only`), és hozzáfűzi azt a PR leírásához vagy egy kommenthez. Ez jelentősen megkönnyíti a reviewerek dolgát, mivel azonnal látják az érintett fájlokat anélkül, hogy a "Files changed" fülre kellene kattintaniuk.
:::


## 4.5 Review folyamat szimulálása és branch protection (opcionális, de ajánlott)

Ez a rész segít megérteni, hogyan működnek a minőségbiztosítási kapuk a valós projektekben.

:::info[Review szimuláció egyedül dolgozva]
Ha egyedül dolgozol a projekten és nincs társad, aki review-olhatná a PR-edet, van néhány lehetőséged:

**Opció 1: Kurzustárs vagy oktató kérése (ajánlott)**
- Kérd meg egy kurzustársadat vagy az oktatót, hogy nézze át a PR-edet
- Ez valódi tapasztalatot ad a code review folyamatról
- A Discord csatornán kérhetsz segítséget

**Opció 2: Második GitHub fiók (oktatási célokra)**
- Hozz létre egy második GitHub fiókot (pl. `<neved>-reviewer`)
- Add hozzá collaborator-ként a repository-hoz (**Settings > Collaborators**)
- Jelentkezz be a második fiókkal és review-old a PR-t

**Opció 3: Branch Protection nélkül (egyszerűsített)**
- Ne állíts be "Require approvals" szabályt
- Csak a "Require status checks" opciót használd
- Ez is értékes gyakorlat, még review nélkül is

**Opció 4: Self-review (tanulási célokra)**
- Nézd át saját kódodat kritikusan, mintha más írta volna
- Írj kommenteket a PR-ben arról, mit csinálnál másképp
- Dokumentáld a review gondolatmenetét
:::

1.  **Branch Protection Rule beállítása (ha még nem tetted meg):**

    - A GitHub repository **Settings > Branches** részében adj hozzá vagy módosítsd a "branch protection rule"-t a `main` branch-re.
    - **Kötelező beállítások a szimulációhoz:**

        - **Require a pull request before merging:** Ezt pipáld be.
            - **Require approvals:** Pipáld be, és állítsd be, hogy legalább `1` jóváhagyás szükséges legyen. (Lásd a fenti box-ot a review szimulációs lehetőségekről.)
        - **Require status checks to pass before merging:** Pipáld be.
            - Ha létrehoztad a `test.yml` workflow-t (3. részfeladat), akkor annak itt meg kell jelennie mint kötelezően sikeres "status check". Válaszd ki! Ha nem hoztad létre, akkor ezt a részt most kihagyhatod, vagy létrehozhatod a `test.yml`-t és hozzáadhatod ehhez a PR-hez.

              :::tip Légy kreativ!
              A szabályok és követelmények, aminek való megfelelést tesztelni szeretnéd a workflow segítségével, itt válnak igazán relevánssá. Az alapvető hozzáállásunk, a szigor mértéke, illetve a konkrét követelmények, amiknek a dokumentációt meg szeretnénk feleltetni meghatározzák, hogy ez a CI/CD fázis, hogyan zárul.

              Érdemes itt is fontosság szerint rendszerezni a különböző hibatípusokat és azok hatásait, annak érdekében, hogy hatékony megelőző stratégiákat alakítsunk ki a folyamatainkban. Mi a célja az ellenőrzésnek? Milyen minőségi eltéréseket vagyunk még képesek átengedni? Mikről akarunk (csak) tudni, mi az, ami kritikus és azonnali beavatkozást igényel, mi az, ami ráér, de tudni akarunk róla?
              :::

1.  **Review kérése és visszajelzés (szimulált):**

    - A PR létrehozása után a GitHub felületén kérhetsz review-t másoktól.
    - Képzeld el, hogy kapsz néhány visszajelzést (pl. "Javítsd ki az elírást a ... oldalon", "Ez a mondat nem egyértelmű").
    - Végezd el a kért (szimulált) javításokat a `feature/update-content-and-review` branch-en, commit-old és push-old őket. A PR automatikusan frissülni fog az új commit-okkal.

1.  **Status check ellenőrzése:**
    - Figyeld meg, hogy a PR-en a beállított status check (pl. a `Test Docusaurus Build` workflow) lefut-e. Ennek sikeresnek kell lennie a merge-eléshez.
    - Próbáld ki, mi történik, ha szándékosan hibás kódot próbálnál áttolni a status check-en.

## 4.6 Pull request merge-elése

Miután a (szimulált) review megtörtént, a kért változtatások elkészültek, és minden status check sikeres:

1.  **Merge:** A GitHub felületén merge-eld a Pull Requestet a `main` branch-be. 

    :::tip
    Használhatod a **Squash and merge** vagy **Rebase and merge** opciót is, ha ismered őket, de egy sima **Merge pull request** is tökéletes.
    :::
1.  **Branch törlése (szimulált):** A merge után a GitHub felajánlja a `feature/update-content-and-review` branch törlését. Ez bevett gyakorlat valós projektek esetén, hogy a publikus fájlrendszer letisztult maradjon. 
 
    :::danger figyelmeztetés
    **A házifeladat részeként most <ins>_NE TÖRÖLD_</ins>, hogy az oktató láthassa a branch-en végzett munkát is.**
    :::

## 4.7 Deployment ellenőrzése

A `main` branch-be történő merge után a [3. feladatban](./reszfeladat3) beállított `deploy.yml` workflow-nak automatikusan el kell indulnia.

1.  **Actions ellenőrzése:** Az **Actions** fülön ellenőrizd, hogy a deployment workflow sikeresen lefutott-e.
1.  **Publikált oldal ellenőrzése:** Látogass el a GitHub Pages oldaladra, és győződj meg róla, hogy a legutóbbi változtatásaid megjelentek-e.

___

## Elvárás a 4. feladat végére

| Kritérium | Elvárt állapot | Elkészült |
| --------- | -------------- | :-------: |
| **`feature/update-content-and-review` branch** | Létezik egy `feature/update-content-and-review` (vagy hasonló nevű) branch a GitHub repository-ban, ami tartalmazza a legutóbbi módosításokat. | <input type="checkbox" /> |
| **Pull Request (Tartalomfrissítés)** | Létrehoztál egy Pull Requestet a `feature/update-content-and-review` branch-ből a `main` branch-be. | <input type="checkbox" /> |
| **Merge** | A PR (szimulált review után) sikeresen merge-elve lett a `main` branch-be. **A feature branch ne legyen törölve.** | <input type="checkbox" /> |
| **(Opcionális) Branch Protection** | Demonstráltad a branch protection rule-ok használatát (pl. kötelező review, kötelező status check a merge előtt). | <input type="checkbox" /> |
| **CI/CD és publikálás** | A `main` branch-be történt merge után a CI/CD folyamat sikeresen deploy-olta a frissített oldalt GitHub Pages-re. | <input type="checkbox" /> |
| **Változások láthatósága** | A [4.2 pontban](#42-változtatások-végrehajtása) végrehajtott változtatások láthatóak az élő, publikált GitHub Pages oldalon. | <input type="checkbox" /> |

___

## 🎉 Gratulálunk! Befejezted a házi feladatot!

:::success
Elkészítetted a teljes Docusaurus projekt lifecycle-ját: a projektkezdéstől a branch protection-ig, az API dokumentáción át a CI/CD automatizálásig.
:::

### Mit tanultál ebben a feladatban?

- ✅ **Docusaurus projekt** létrehozása és konfigurálása
- ✅ **Markdown dokumentáció** strukturálása frontmatter-rel
- ✅ **Git workflow** alkalmazása (branch, commit, PR, merge)
- ✅ **OpenAPI dokumentáció** automatikus generálása
- ✅ **CI/CD pipeline** beállítása GitHub Actions-zel
- ✅ **GitHub Pages** deployment automatizálása
- ✅ **Code review folyamat** szimulálása branch protection-nel

### Következő lépések

Most, hogy megvan az alapod, továbblépve kipróbálhatod:

- **Több dokumentációs szekció** hozzáadása (pl. FAQ, changelog, contributing guide)
- **Több API dokumentáció** integrálása különböző szolgáltatásokhoz
- **Testreszabás:** Egyedi témák, komponensek, Docusaurus pluginok
- **Többnyelvű dokumentáció** (i18n) beállítása
- **Keresés** integrálása (Algolia DocSearch)
- **Versioning** használata (több dokumentációs verzió párhuzamosan)

:::tip[Továbbfejlesztési ötletek]
- Adj hozzá Google Analytics-et az oldaladhoz
- Integráld a Docusaurus blog funkcióját intenzívebben
- Hozz létre egyedi React komponenseket a dokumentációba
- Állíts be automatikus link ellenőrzést (broken link checker)
:::

### Hasznos linkek

- [Docusaurus hivatalos dokumentáció](https://docusaurus.io/docs)
- [Docusaurus showcase](https://docusaurus.io/showcase) - Példák más projektektől
- [Docusaurus Discord közösség](https://discord.gg/docusaurus)
- [GitHub Actions marketplace](https://github.com/marketplace?type=actions)

___

**Köszönjük, hogy elvégezted ezt a feladatot! Sok sikert a további projektjeidhez!** 🚀
