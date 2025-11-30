---
title: Előkészületek
sidebar_position: 2
description: Fejlesztői környezet telepítése és beállítása
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Előkészületek: Fejlesztői környezet telepítése

A feladatok elvégzéséhez szükséged lesz egy megfelelően beállított fejlesztői környezetre. Ezen az oldalon végigvesszük a szükséges eszközöket és azok telepítését.

## Szükséges eszközök listája

A következő szoftverekre lesz szükséged:

1.  **Visual Studio Code (VSCode):** Kód szerkesztő. Az órák során ezt használtam, azonban bármilyen szerkesztőt használhattok. Ez egy könnyen tanulható szerkesztő, amiben megtalálható minden feature, amivel a projekteteket létre tudjátok hozni.
2.  **Parancssor (Terminal/CLI):** A parancsok futtatásához.
3.  **Node.js és NPM:** JavaScript futtatókörnyezet és csomagkezelő.
4.  **Yarn (Opcionális, de ajánlott):** Alternatív csomagkezelő.
5.  **Git és GitHub CLI:** Verziókezeléshez és a könnyebb interakcióhoz a GitHub-bal.
6.  **GitHub Fiók:** A projektek tárolásához és megosztásához.

## 1. Visual Studio Code telepítése

A VSCode a legnépszerűbb ingyenes kódszerkesztő webfejlesztéshez.

- [Letöltés és telepítés (Windows, Mac, Linux)](https://code.visualstudio.com/download)

:::tip Ajánlott kiegészítők (Extensions)
Érdemes telepíteni néhány kiegészítőt, amelyek megkönnyítik a munkát:

- **Markdown All in One** - Markdown szerkesztés támogatása (előnézet, billentyűparancsok)
- **Prettier** - Kód formázás automatizálása (egységes kódstílus)
- **ESLint** - JavaScript/TypeScript kód minőség ellenőrzés
- **GitLens** (opcionális) - Git history és blame információk megjelenítése

Ezek nem kötelezőek, de jelentősen javítják a fejlesztői élményt!
:::

## 2. Node.js és NPM telepítése

Ez a lépés kritikus a Docusaurus futtatásához. Javasoljuk az LTS (Long Term Support) verzió használatát.

<Tabs groupId="operating-system">
<TabItem value="macos" label="macOS" default>

Mac-en a legegyszerűbb módja a telepítésnek a **Homebrew** vagy az **nvm** (Node Version Manager) használata.

<Tabs>
<TabItem value="brew" label="Homebrew (Egyszerű)" default>

A [Homebrew](https://brew.sh/) egy népszerű csomagkezelő macOS-re, amely megkönnyíti a fejlesztői eszközök telepítését.

**Homebrew telepítése (ha még nincs telepítve):**

Nyisd meg a Terminált és futtasd:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

A telepítő végigvezet a folyamaton. Telepítés után ellenőrizd:

```bash
brew --version
```

**Node.js telepítése Homebrew-val:**

```bash
brew install node
```

</TabItem>
<TabItem value="nvm" label="NVM">

Az `nvm` (Node Version Manager) segítségével könnyen válthatsz Node verziók között, ami hasznos több projekt kezelésekor.

**NVM telepítése macOS-re:**

1.  Nyisd meg a Terminált és futtasd az nvm telepítő scriptjét:

    ```bash
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
    ```

2.  A telepítés után **zárd be és nyisd meg újra a Terminált**, vagy futtasd:

    ```bash
    source ~/.bashrc  # vagy ~/.zshrc, attól függően, melyik shell-t használod
    ```

3.  Ellenőrizd, hogy az nvm telepítve van-e:

    ```bash
    nvm --version
    ```

**Node.js telepítése NVM-mel:**

1.  Telepítsd a Node legfrissebb LTS verzióját:

    ```bash
    nvm install --lts
    ```

2.  Állítsd be alapértelmezettként:

    ```bash
    nvm use --lts
    nvm alias default 'lts/*'
    ```

:::info
További információ: [nvm hivatalos dokumentáció](https://github.com/nvm-sh/nvm#installing-and-updating)
:::

</TabItem>
</Tabs>

</TabItem>
<TabItem value="windows" label="Windows">

Windows-on gyakori probléma, hogy a `node` vagy `npm` parancsok nem érhetőek el telepítés után, mert nem kerültek be a rendszer `$PATH` környezeti változójába.

<Tabs>
<TabItem value="installer" label="Installer" default>

1.  Töltsd le az **LTS** verziót a [Node.js weboldaláról](https://nodejs.org/).
1.  Futtasd a telepítőt.

    :::caution FONTOS
    A telepítés során a "Custom Setup" ablaknál vagy a beállításoknál győződj meg róla, hogy az **"Add to PATH"** opció ki van pipálva! Enélkül a parancssor nem fogja megtalálni a `node` parancsot.
    :::

1.  Telepítés után **indítsd újra a számítógépet** (vagy legalább a parancssort), hogy a változások életbe lépjenek.

</TabItem>
<TabItem value="nvm-windows" label="nvm-windows">

Hasonlóan a Mac-es nvm-hez, ez is verziókezelést tesz lehetővé Windows-on.

**nvm-windows telepítése:**

1.  Látogass el a [coreybutler/nvm-windows releases](https://github.com/coreybutler/nvm-windows/releases) oldalára
2.  Töltsd le a legfrissebb `nvm-setup.exe` fájlt (általában a legfelső release-nél található)
3.  Futtasd a letöltött telepítőt és kövesd a telepítési lépéseket
4.  Telepítés után **nyiss meg egy új parancssort** (hogy a PATH változások életbe lépjenek)

**Node.js telepítése nvm-windows-zal:**

1.  Ellenőrizd, hogy az nvm telepítve van:

    ```bash
    nvm version
    ```

2.  Telepítsd a Node legfrissebb LTS verzióját:

    ```bash
    nvm install lts
    ```

3.  Aktiváld a telepített verziót:

    ```bash
    nvm use lts
    ```

4.  Ellenőrizd a telepítést:

    ```bash
    node -v
    npm -v
    ```

:::info
További információ: [nvm-windows dokumentáció](https://github.com/coreybutler/nvm-windows)
:::

</TabItem>
</Tabs>

### Hibaelhárítás Windows-on

Ha a `node -v` parancsra "not recognized" hibát kapsz:

<Tabs>
<TabItem value="gui" label="Grafikus felület (GUI)" default>

1.  Keress rá a Windows keresőben: "Környezeti változók szerkesztése" (Edit the system environment variables).
1.  Kattints a "Környezeti változók..." (Environment Variables) gombra.
1.  A "Rendszerváltozók" (System variables) listában keresd meg a `Path` változót és kattints a "Szerkesztés" (Edit) gombra.
1.  Ellenőrizd, hogy szerepel-e a listában a Node.js elérési útja (pl. `C:\Program Files\nodejs\`). Ha nem, add hozzá manuálisan.

</TabItem>
<TabItem value="cli" label="Parancssor (CLI)">

Feltételezve, hogy a Node.js a default helyre (`C:\Program Files\nodejs`) települt:

**1. Ellenőrizd a jelenlegi PATH-t:**

```bash
echo %PATH%
```

**2. Add hozzá a Node.js útvonalát a PATH-hoz:**

- **Felhasználói PATH módosítása (rendszergazdai jogok nélkül):**
Nyiss meg egy **normál parancssort** (cmd) vagy **PowerShellt**, majd futtasd a következő parancsot:

    ```powershell
    setx PATH "%PATH%;C:\Program Files\nodejs\"
    ```

- **Rendszerszintű PATH módosítása (rendszergazdai jogok szükségesek):**
Nyiss meg egy **rendszergazdai jogú parancssort** (cmd) vagy **PowerShellt**, majd futtasd a következő parancsot:

    ```powershell
    setx /M PATH "%PATH%;C:\Program Files\nodejs\"
    ```

:::info
A `setx` parancs a felhasználói vagy rendszer szintű környezeti változókat módosítja. A változások azonnal beíródnak, de előfordulhat, hogy csak egy **új parancssor ablak megnyitása után** válnak aktívvá.
:::

**3. Ellenőrizd újra a PATH-t:**
Nyiss meg egy **új** parancssort, és ellenőrizd, hogy az útvonal bekerült-e:
```bash
echo %PATH%
```
Ezután a `node -v` és `npm -v` parancsoknak működniük kell.


</TabItem>
</Tabs>

</TabItem>
</Tabs>


### Ellenőrzés

A telepítés után ellenőrizd, hogy a Node.js és az NPM megfelelően működik-e. Nyiss meg egy **új parancssor ablakot** (hogy a PATH változások életbe lépjenek), majd futtasd a következő parancsokat:

```bash
node -v
npm -v
```

A kimenetnek valami hasonlónak kell lennie (a verziószámok eltérhetnek):
```
v18.18.0
9.8.5
```

Ha a parancsok verziószámokat adnak vissza, akkor a Node.js és NPM megfelelően telepítve és konfigurálva van a rendszereden.

:::tip
Ha Windows-on a `node -v` parancsra "not recognized" hibát kapsz, lásd a [Windows hibaelhárítás](#hibaelhárítás-windows-on) szekciót.
:::

## 3. Yarn telepítése (Opcionális)

A Docusaurus (és a Facebook-os eszközök) gyakran preferálják a Yarn-t.

### Miért Yarn az NPM helyett?

| Szempont | NPM | Yarn |
|----------|-----|------|
| **Sebesség** | Gyors | Általában gyorsabb (párhuzamos telepítés) |
| **Determinizmus** | Jó | Kiváló (lock file garantálja ugyanazt a verziót) |
| **Használat** | Alapértelmezett | Külön telepítés szükséges |

:::note
A feladatok során **mindkét** csomagkezelővel dolgozhatsz. Az utasításokban a fülek segítségével választhatsz közöttük. Válaszd azt, amelyiket kényelmesebbnek találod!
:::

### Telepítés

```bash
npm install --global yarn
```

Ellenőrzés: `yarn --version`

## 4. Git és GitHub CLI

A verziókezeléshez és a feladatok beadásához szükség lesz a Git-re.

- **Git telepítése:** [git-scm.com/downloads](https://git-scm.com/downloads)
- **GitHub CLI telepítése:** [cli.github.com](https://cli.github.com/)

Telepítés után konfiguráld a Git-et (ha még nem tetted meg):

```bash
git config --global user.name "A Te Neved"
git config --global user.email "email@cim.ed"
```

:::info Miért kell ezt beállítani?
A `git config` parancs beállítja a nevedet és az email címedet, ami **minden commit-ba** belekerül. Ez azért fontos, mert:

- **Azonosítás:** Mások látják, hogy ki készítette a változtatásokat
- **GitHub integráció:** A GitHub ez alapján köti össze a commit-jaidat a fiókoddal
- **Együttműködés:** Csapatmunkánál elengedhetetlen, hogy tudjuk ki mit csinált

Használd azt az email címet, amit a GitHub fiókodnál is használsz!
:::

Jelentkezz be a GitHub CLI-vel:

```bash
gh auth login
```

Ez egy interaktív folyamat, amely végigvezet a bejelentkezésen. Válaszd a böngészős bejelentkezést (browser auth), ez a legegyszerűbb.

## 5. GitHub regisztráció és Repo

Ha még nincs, regisztrálj a [GitHub.com](https://github.com/)-on. A feladatok során létre kell majd hoznod egy repository-t a projektednek.

___

## ✅ Végső ellenőrző lista - Készen állsz!

Mielőtt elkezdenéd az 1. feladatot, ellenőrizd, hogy minden megvan. Pipáld be azt, amit már elvégeztél!

### Eszközök telepítése

| Eszköz | Ellenőrzési parancs | Kész |
|--------|---------------------|------|
| **Node.js** | `node -v` (eredmény: pl. `v18.18.0`) | <input type="checkbox" /> |
| **NPM** | `npm -v` (eredmény: pl. `9.8.5`) | <input type="checkbox" /> |
| **Git** | `git --version` (eredmény: pl. `git version 2.39.0`) | <input type="checkbox" /> |
| **GitHub CLI** | `gh --version` (eredmény: pl. `gh version 2.40.0`) | <input type="checkbox" /> |
| **VSCode vagy más szerkesztő** | Megnyitható és használható | <input type="checkbox" /> |
| **Yarn (opcionális)** | `yarn --version` (ha használni akarod) | <input type="checkbox" /> |

### Konfigurációk

| Beállítás | Ellenőrzés | Kész |
|-----------|------------|------|
| **Git user.name** | `git config user.name` visszaad egy nevet | <input type="checkbox" /> |
| **Git user.email** | `git config user.email` visszaad egy email címet | <input type="checkbox" /> |
| **GitHub bejelentkezés** | `gh auth status` "Logged in" üzenetet mutat | <input type="checkbox" /> |

### Hozzáférések

| Fiók/Hozzáférés | Ellenőrzés | Kész |
|-----------------|------------|------|
| **GitHub fiók** | Be tudsz jelentkezni a [github.com](https://github.com)-ra | <input type="checkbox" /> |
| **Repository létrehozási jog** | Tudsz új repository-t létrehozni (**Settings > Repositories**) | <input type="checkbox" /> |

:::tip[Ha minden pipálva van...]
**Gratulálok! 🎉 Készen állsz az 1. feladatra!**

Kezdheted a [1. feladat - Projekt alapok](./feladatok/reszfeladat1) oldalt.
:::

:::caution[Ha valami nem működik...]
Ne aggódj! Nézd meg a [Segítség és támogatás](./segitseg) oldalt, ahol találsz:
- Gyakori problémák megoldásait
- Discord csatorna elérhetőségét
- További segítséget
:::
