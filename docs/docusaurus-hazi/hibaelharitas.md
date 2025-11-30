---
title: Hibaelhárítás
sidebar_position: 4
---

# Hibaelhárítás és gyakori problémák

Ez az oldal a leggyakrabban előforduló technikai problémák megoldásait tartalmazza. Ha elakadtál, vagy hibát kaptál a telepítés vagy fejlesztés során, itt valószínűleg megtalálod a megoldást.

:::tip
Kattints a probléma leírására a részletes megoldás megjelenítéséhez!
:::

<details>
<summary><strong>1. <code>node: command not found</code> vagy <code>npm: command not found</code></strong></summary>

**Probléma:** A Node.js telepítve van, de a terminál nem találja a parancsot.

**Megoldás:**
1. **Zárd be és nyisd meg újra a terminált** - A `PATH` változások csak új terminál ablakban lépnek életbe
2. **Ellenőrizd a telepítést:**

   ```bash
   # macOS/Linux
   which node

   # Windows
   where node
   ```
3. **Windows-on:** Nézd meg a [Windows hibaelhárítás](./elokeszuletek#hibaelhárítás-windows-on) szekciót a PATH beállításához
4. **macOS-en:** Ha Homebrew-val telepítetted, futtasd: `brew doctor` és kövesd az utasításokat

</details>

---

<details>
<summary><strong>2. Git push authentication failed / Permission denied</strong></summary>

**Probléma:** A GitHub nem fogadja el a jelszavadat, vagy nem tudsz push-olni.

**Megoldás:**
- **GitHub már NEM fogad el jelszót!** Használd a GitHub CLI-t:
  ```bash
  gh auth login
  ```
  Válaszd a böngészős bejelentkezést (browser auth)
- **Vagy állíts be Personal Access Token-t** (PAT):
  1. GitHub.com > Settings > Developer settings > Personal access tokens > Tokens (classic)
  2. Generate new token (classic)
  3. Válaszd ki a `repo` jogosultságot
  4. Másold ki a generált token-t
  5. Használd jelszó helyett a git push során

</details>

---

<details>
<summary><strong>3. A generált oldal üres vagy hibás / 404-es hiba</strong></summary>

**Probléma:** Az oldal felépül, de a tartalom nem jelenik meg, vagy 404-es hibát kapsz.

**Megoldás:**
1. **Ellenőrizd a `docusaurus.config.js` `baseUrl` beállítását:**
   - GitHub Pages-hez: `baseUrl: '/<REPOSITORY_NEVE>/'`
   - Helyi fejlesztéshez: `baseUrl: '/'`
2. **Töröld a cache-t és építsd újra:**
   ```bash
   npm run clear
   npm start
   ```
3. **Ellenőrizd a browser console-t:**
   - Nyomd meg `F12` a böngészőben
   - Nézd meg a Console tab-ot, van-e hiba
4. **GitHub Pages URL:** Várj 2-3 percet a deployment után, mielőtt meglátogatod az oldalt

</details>

---

<details>
<summary><strong>4. <code>docusaurus gen-api-docs</code> - command not found</strong></summary>

**Probléma:** Az API dokumentáció generálási parancs nem működik.

**Megoldás:**
1. **Ellenőrizd, hogy a plugin telepítve van-e:**
   ```bash
   npm list docusaurus-plugin-openapi-docs
   ```
2. **Ha nincs telepítve:**
   ```bash
   npm install docusaurus-plugin-openapi-docs docusaurus-theme-openapi-docs
   ```
3. **Ellenőrizd a `docusaurus.config.js` plugin konfigurációját**
4. **Próbáld full path-tal:**
   ```bash
   npx docusaurus gen-api-docs all
   ```

</details>

---

<details>
<summary><strong>5. GitHub Actions workflow failed / Build errors</strong></summary>

**Probléma:** A GitHub Actions workflow hibával leáll.

**Megoldás:**
1. **Nézd meg a log-ot:**
   - GitHub repository > Actions tab > Kattints a sikertelen workflow-ra
   - Bontsd ki a lépéseket és olvasd el a hibaüzeneteket
2. **Gyakori okok:**
   - **Node version mismatch:** Ellenőrizd, hogy a `deploy.yml`-ben a Node verzió megegyezik-e a lokálissal
   - **Missing dependencies:** A `package.json`-ben minden dependency benne van?
   - **API docs not generated:** Biztosan van `- name: Build API Docs` lépés a workflow-ban?
   - **Broken links:** A build leáll törött linkek miatt - javítsd ki őket
3. **Lokálisan is futtasd le a build-et:**
   ```bash
   npm run build
   ```
   Ha itt is hibát kapsz, azt kell először megjavítani

</details>

---

<details>
<summary><strong>6. Markdown formatting nem jelenik meg helyesen</strong></summary>

**Probléma:** A Markdown formázások (pl. táblázatok, code blockok) nem jelennek meg szépen.

**Megoldás:**
1. **Ellenőrizd a szintaxist:**
   - Táblázatoknál ügyelj a `|` karakterekre
   - Code blockoknál három backtick: \`\`\`
2. **Frontmatter helyes formátuma:**
   ```markdown
   ---
   title: Cím
   sidebar_position: 1
   ---
   ```
   Ne használj extra szóközöket vagy tabulátorokat
3. **VSCode-ban használd a Markdown preview-t:** `Ctrl+Shift+V` vagy `Cmd+Shift+V`

</details>

---

<details>
<summary><strong>7. "Cannot find module" hibák</strong></summary>

**Probléma:** Node.js nem találja a modulokat / package-eket.

**Megoldás:**
1. **Töröld a `node_modules` mappát és telepítsd újra:**
   ```bash
   rm -rf node_modules package-lock.json
   npm install
   ```
2. **Ellenőrizd a `package.json` fájlt** - minden dependency ott van?
3. **Ha továbbra sem működik, próbáld cache nélkül:**
   ```bash
   npm cache clean --force
   npm install
   ```

</details>

---

<details>
<summary><strong>8. Nem találom a GitHub Settings / Actions menüt</strong></summary>

**Probléma:** Nem találod meg a GitHub beállításokat.

**Megoldás:**
1. **Settings tab:** A repository tetején, jobbra, a "Code" mellett
2. **Actions:** Settings > Actions > General (bal oldali menü)
3. **Pages:** Settings > Pages (bal oldali menü)
4. **Jogosultságok:** Csak a repository owner vagy admin látja a Settings tab-ot

</details>

---

## 💡 További tippek

- **Commit-ájl gyakran:** Ne várj órákig egy nagy commit-tal, kisebb, gyakori commit-ok jobbak
- **Olvass el hibaüzeneteket:** Legtöbbször tartalmaznak hasznos információt
- **Google a barátod:** Angol nyelven keresd a hibaüzenetet - valószínűleg nem te vagy az első
- **Dokumentáció:** A [Docusaurus hivatalos dokumentációja](https://docusaurus.io/docs) kiváló forrás

___

:::info Nem találod a megoldást?
Ha a problémád nem szerepel ebben a listában, látogass el a [Segítség és támogatás](./segitseg) oldalra további segítségért!
:::
