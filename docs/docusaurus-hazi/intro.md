---
title: Bevezető a Docusaurus házihoz
sidebar_position: 1
---

# Docusaurus projekt felépítése: Hozd létre saját dokumentációs oldalad!

Ez a kurzus összegző és egyben utolsó házi feladata. Ennek a feladatnak a célja, hogy a Docusaurus-központú, illetve gyakorlati órákon tanultakat összegezd és ténylegesen is kipróbáld. 

Egy saját, működő Docusaurus dokumentációs oldalt fogsz létrehozni, verziókezelni Git-tel, API dokumentációval bővíteni, és egy egyszerű CI/CD folyamaton keresztül publikálni GitHub Actions segítségével.

:::tip[Fontos]
A feladat célja nem egy tökéletes, minden részletében kidolgozott dokumentáció létrehozása, hanem az alapvető funkciók megismerése és magabiztos használatának elsajátítása.

Bátran kísérletezz, nézz utána dolgoknak a hivatalos dokumentációkban, és fedezd fel a Docusaurusban rejlő lehetőségeket!
:::

## 🎯 Mit fogsz tanulni ebben a feladatban?

Ennek a házi feladatnak az elvégzése után képes leszel:

- **Docusaurus projekt létrehozására és konfigurálására** - Nulláról egy működő dokumentációs oldal felépítése
- **Markdown tartalom strukturálására** - Frontmatter használata, kategóriák, oldalsávak kezelése
- **Git és GitHub workflow alkalmazására** - Branch-ek, commit-ok, pull reques-ek kezelése
- **API dokumentáció integrálására** - OpenAPI specifikáció alapján automatikus dokumentáció generálása
- **CI/CD pipeline beállítására** - GitHub Actions használata automatikus build és deployment-hez
- **Dokumentáció publikálására** - GitHub Pages használata ingyenes hosting-ra
- **Review folyamat gyakorlására** - Pull request review szimuláció, branch protection

Ezek gyakorlati készségek, amelyeket valós projektekben is alkalmazhatsz!

## 📋 Előfeltételek - Mire lesz szükséged?

### Technikai tudás

A feladat elvégzéséhez szükséges alapvető ismeretek:

| Téma | Szükséges szint | Megjegyzés |
|------|----------------|------------|
| **Terminál/parancssor használat** | Alapszint | Parancsok futtatása, mappák között navigálás |
| **Git alapok** | Alapszint | `commit`, `push`, `pull`, `branch` fogalmak ismerete |
| **Markdown** | Alapszint | Fejlécek, listák, linkek, kódblokkok formázása |
| **GitHub használat** | Alapszint | Repository létrehozás, beállítások módosítása |
| **Angol nyelv** | Olvasási szint | Dokumentációk olvasása, hibakeresés |

:::note
Ha valamelyik témában nem érzed magad biztonságban, az sem probléma! A feladat során gyakorolhatod őket, és segítséget kérhetsz a [Segítség és támogatás](./segitseg) oldalon leírtak szerint.
:::

### Eszközök és hozzáférések

Mielőtt elkezded a feladatot, gondoskodj róla, hogy az alábbiak megvannak:

| Eszköz/Hozzáférés | Ellenőrzés | Hol találod |
|-------------------|------------|-------------|
| **Node.js és NPM** | <input type="checkbox" /> Telepítve és működik | [Előkészületek](./elokeszuletek#2-nodejs-és-npm-telepítése) |
| **Git** | <input type="checkbox" /> Telepítve és konfigurálva | [Előkészületek](./elokeszuletek#4-git-és-github-cli) |
| **GitHub fiók** | <input type="checkbox" /> Létrehozva és aktív | [Előkészületek](./elokeszuletek#5-github-regisztráció-és-repo) |
| **Kódszerkesztő (VSCode)** | <input type="checkbox" /> Telepítve | [Előkészületek](./elokeszuletek#1-visual-studio-code-telepítése) |

:::tip
Ha még nincs minden megoldva, kezdd az [Előkészületek](./elokeszuletek) oldallal, ahol lépésről-lépésre végigvezetünk mindenen!
:::

## A feladat felépítése

A házi feladat négy fő részből áll, melyek szorosan kapcsolódnak a 8-11. órák anyagához. Javasoljuk, hogy az adott óra után rögtön végezd el a kapcsolódó részt, de természetesen egyben is nekifuthatsz, bár úgy több időt vehet igénybe egyszerre.

1.  **[Projekt alapok, struktúra és új tartalom](./feladatok/reszfeladat1.md)**
    - Kapcsolódó óra: 8. óra - Modern dokumentációs keretrendszerek, Docusaurus alapok
1.  **[Dinamikus tartalom - API dokumentáció](./feladatok/reszfeladat2.md)**
    - Kapcsolódó óra: 9. óra - Docusaurus: API-dokumentáció, pluginok
1.  **[Automatizálás - CI/CD folyamat](./feladatok/reszfeladat3.md)**
    - Kapcsolódó óra: 10. óra - Verziókövetés és CI/CD a dokumentációban
1.  **[Együttműködés szimulálása - Review](./feladatok/reszfeladat4.md)**
    - Kapcsolódó óra: 11. óra - Minőségbiztosítás, review és karbantartás

## ⏱️ Becsült időigény

Az egyes részfeladatok várható elvégzési ideje:

| Részfeladat | Becsült idő | Megjegyzés |
|-------------|-------------|------------|
| **[1. Projekt alapok](./feladatok/reszfeladat1)** | 60-90 perc | + 30-45 perc, ha még nincs környezet telepítve |
| **[2. API dokumentáció](./feladatok/reszfeladat2)** | 45-60 perc | Plugin telepítés és konfiguráció |
| **[3. CI/CD folyamat](./feladatok/reszfeladat3)** | 30-45 perc | Workflow beállítás és tesztelés |
| **[4. Review folyamat](./feladatok/reszfeladat4)** | 30-45 perc | PR létrehozás és branch protection |
| **Összesen** | **3-4 óra** | Hibakeresés nélkül, tapasztalt felhasználónak |

:::note
Kezdőknek vagy hibakeresés esetén ez több időt is igénybe vehet. Ne aggódj, ha lassabban haladsz - ez teljesen normális!
:::

## 📊 Értékelés és pontszámok

A feladat értékelése a létrehozott GitHub repository és a publikált GitHub Pages oldal alapján történik. **A házi feladat összesen 70 pontot ér.**

### Pontok lebontása részfeladatonként

| Részfeladat | Pontszám | Fő értékelési szempontok |
|-------------|----------|-------------------------|
| **1. Projekt alapok** | 20 pont | Működő Docusaurus projekt, strukturált dokumentáció, GitHub repo, PR létrehozva |
| **2. API dokumentáció** | 20 pont | OpenAPI plugin működik, API docs generálva és elérhető, CSS stílusok |
| **3. CI/CD folyamat** | 20 pont | GitHub Actions workflow működik, sikeres deployment, GitHub Pages elérhető |
| **4. Review folyamat** | 10 pont | PR létrehozva és merge-elve, review folyamat demonstrálva, változások publikálva |

### Részletes értékelési szempontok

A fő értékelési szempontok a következők lesznek:

- ✅ A Docusaurus projekt sikeres létrehozása és alapvető működése
- ✅ A struktúraátalakítás és az új tartalmi szekciók helyes létrehozása, konfigurálása
- ✅ Az API dokumentáció sikeres integrálása és elérhetősége
- ✅ A CI/CD folyamat helyes beállítása és működése (sikeres build és deploy)
- ✅ A verziókezelési és review folyamat lépéseinek demonstrálása
- ✅ A publikált GitHub Pages oldal működése és elérhetősége

:::tip Tipp a maximális pontszámhoz
- Ellenőrizd minden feladat végén az "Elvárások" táblázatot
- Teszteld a publikált oldalt böngészőben
- Nézd át a GitHub Actions log-okat, hogy nincsenek-e hibák
- Kérd meg egy kurzustársadat, hogy nézze át a PR-edet
:::

___

Sok sikert a feladathoz! 🎉