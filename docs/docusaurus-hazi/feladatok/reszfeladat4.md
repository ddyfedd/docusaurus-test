---
title: 4. feladat - Review folyamat
sidebar_position: 4
---

# 4. feladat: Együttműködés szimulálása - változások kezelése és review folyamat

Az utolsó részben egy egyszerűsített review folyamatot fogsz szimulálni, ahogy az a valós projektekben is történik. Ez magában foglalja a változtatások külön branch-en történő fejlesztését, Pull Request (PR) létrehozását, és (opcionálisan) a PR ellenőrzését és a branch protection szabályok alkalmazását.

## Mielőtt elkezdenéd

- A `main` branch-ed tartalmazza az 1., 2. és 3. részfeladatok eredményeit (azaz a strukturált "Tutorials" és "Guides" szekciókat, az integrált API dokumentációt, és a beállított CI/CD folyamatot, ami a `main` branch-re sikeresen deploy-olja az oldalt GitHub Pages-re).
- Ha beállítottad a 3. részfeladatban az opcionális `test.yml` workflow-t (ami PR-ekre fut le), az most hasznos lesz.

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

1.  **Új blogbejegyzés írása (opcionális):**
    - Hozz létre egy új Markdown fájlt a `blog` mappában (pl. `YYYY-MM-DD-my-latest-thoughts.md`).
    - Írj egy rövid blogbejegyzést a Docusaurus használatával kapcsolatos tapasztalataidról vagy bármilyen más releváns témáról. Adj neki címet és esetleg címkéket (tags) a frontmatter-ben.

1.  **Apróbb konfigurációs módosítás (opcionális):**
    - Módosíts valamit a `docusaurus.config.js`-ben, például a lábléc (`footer`) tartalmát vagy egy navbar link szövegét.

## 4.3 Változások commitálása és feltöltése

Mentsd el a munkádat.

1.  **Commit:**

    ```bash
    git add .
    git commit -m "Docs: Update content and prepare for review" 
    ```
    
    :::tip
    Használj informatív commit üzenetet!
    :::

1.  **Push:**

    ```bash
    git push -u origin feature/update-content-and-review
    ```

## 4.4 Pull Request (PR) létrehozása

Most kérj véleményezést a változtatásaidról.

1.  **PR nyitása:** A GitHub felületén hozz létre egy Pull Requestet a `feature/update-content-and-review` branch-ből a `main` branch-be.
1.  **PR leírása:** Adj egy rövid, de informatív címet és leírást a PR-nek, összefoglalva a végrehajtott változtatásokat. Hivatkozhatsz a házi feladat ezen részére.

## 4.5 Review folyamat szimulálása és branch protection (opcionális, de ajánlott)

Ez a rész segít megérteni, hogyan működnek a minőségbiztosítási kapuk a valós projektekben.

1.  **Branch Protection Rule beállítása (ha még nem tetted meg):**

    - A GitHub repository **Settings -> Branches** részében adj hozzá vagy módosítsd a "branch protection rule"-t a `main` branch-re.
    - **Kötelező beállítások a szimulációhoz:**

        - **Require a pull request before merging:** Ezt pipáld be.
            - **Require approvals:** Pipáld be, és állítsd be, hogy legalább `1` jóváhagyás szükséges legyen. (Megkérhetsz egy kurzustársat, vagy az oktatót a review-ra. Ha magadnak csinálod, akkor egy másik GitHub fiókkal, vagy ideiglenesen kikapcsolhatod ezt a konkrét PR merge-eléséhez, miután "elvileg" megtörtént a review.)
        - **Require status checks to pass before merging:** Pipáld be.
            - Ha létrehoztad a `test.yml` workflow-t (3. részfeladat), akkor annak itt meg kell jelennie mint kötelezően sikeres "status check". Válaszd ki! Ha nem hoztad létre, akkor ezt a részt most kihagyhatod, vagy létrehozhatod a `test.yml`-t és hozzáadhatod ehhez a PR-hez.

1.  **Review kérése és visszajelzés (szimulált):**

    - A PR létrehozása után a GitHub felületén kérhetsz review-t másoktól.
    - Képzeld el, hogy kapsz néhány visszajelzést (pl. "Javítsd ki az elírást a ... oldalon", "Ez a mondat nem egyértelmű").
    - Végezd el a kért (szimulált) javításokat a `feature/update-content-and-review` branch-en, commitold és pushold őket. A PR automatikusan frissülni fog az új commitokkal.

1.  **Status check ellenőrzése:**
    - Figyeld meg, hogy a PR-en a beállított status check (pl. a `Test Docusaurus Build` workflow) lefut-e. Ennek sikeresnek kell lennie a merge-eléshez.

## 4.6 Pull request merge-elése

Miután a (szimulált) review megtörtént, a kért változtatások elkészültek, és minden status check sikeres:

1.  **Merge:** A GitHub felületén merge-eld a Pull Requestet a `main` branch-be. 

    :::tip
    Használhatod a "Squash and merge" vagy "Rebase and merge" opciót is, ha ismered őket, de egy sima "Merge pull request" is tökéletes.
    :::
1.  **Branch törlése (Opcionális):** A merge után a GitHub felajánlja a `feature/update-content-and-review` branch törlését. Ez bevett gyakorlat, de a házi feladat szempontjából most **NE TÖRÖLD**, hogy az oktató láthassa a branch-en végzett munkát is.

## 4.7 Deployment ellenőrzése

A `main` branch-be történő merge után a 3. feladatban beállított `deploy.yml` workflow-nak automatikusan el kell indulnia.

1.  **Actions ellenőrzése:** Az **Actions** fülön ellenőrizd, hogy a deployment workflow sikeresen lefutott-e.
1.  **Publikált oldal ellenőrzése:** Látogass el a GitHub Pages oldaladra, és győződj meg róla, hogy a legutóbbi változtatásaid megjelentek-e.

___

## Elvárás a 4. feladat végére

| Kritérium | Elvárt állapot |
| --------- | -------------- |
| **`feature/update-content-and-review` branch** | Létezik egy `feature/update-content-and-review` (vagy hasonló nevű) branch a GitHub repository-ban, ami tartalmazza a legutóbbi módosításokat. |
| **Pull Request (Tartalomfrissítés)** | Létrehoztál egy Pull Requestet a `feature/update-content-and-review` branch-ből a `main` branch-be. |
| **Merge** | A PR (szimulált review után) sikeresen merge-elve lett a `main` branch-be. A feature branch ne legyen törölve. |
| **(Opcionális) Branch Protection** | Demonstráltad a branch protection rule-ok használatát (pl. kötelező review, kötelező status check a merge előtt). |
| **CI/CD és publikálás** | A `main` branch-be történt merge után a CI/CD folyamat sikeresen deploy-olta a frissített oldalt GitHub Pages-re. |
| **Változások Láthatósága** | A 4.2 pontban végrehajtott változtatások láthatóak az élő, publikált GitHub Pages oldalon. |
