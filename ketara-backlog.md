# Ketara — Fejlesztési backlog (alfa) v0.2

*Élő dokumentum. A pipák a haladást mutatják; egy ticket akkor kész, ha a kódja a
`main`-en van, és a CI zöld.*
*Alapja: PRD v1.4, Folyamatok és képernyők v0.11, UX-terv v0.7, Tech terv v0.4.*
*Utolsó frissítés: 2026-11-13 (E4 kész: ketara-app PR #5)*

---

## Hogyan olvasd

- **Epic (E)** — egy nagy egység, általában a Tech terv 14. munkasorrendjének egy lépése.
- **Story (S)** — egy felhasználói vagy fejlesztői szempontból értelmes eredmény, „Kész, ha"
  feltétellel. Akkor pipáld ki, ha minden ticketje kész, és a feltétel teljesül.
- **Ticket (T)** — egy PR-nyi munka. Az azonosító (pl. `T2.3.1`) mehet a branch nevébe és a
  commit üzenetbe.
- 👤 = **a te feladatod** (beállítás, döntés, adat), nem kód.
- A hivatkozások: **PRD** = `ketara-prd.md`, **F** = `ketara-folyamatok-kepernyok.md`,
  **UX** = `ketara-ux.md`, **Tech** = `ketara-tech-terv.md`.
- Kód, azonosítók, commitok angolul; felületi szöveg magyarul (Tech 2.).

## Áttekintés

| Epic | Tartalom | Függ | Tech 14. lépés |
|---|---|---|---|
| E0 | Előfeltételek és döntések 👤 | — | előtte |
| E1 | Repó-alap, CI, deploy | E0 | 0 |
| E2 | Megoldó (TS-port, tesztek) | E1 | 1 |
| E3 | Alapanyaglista és konfiguráció | E1 | 2 |
| E4 | Tárolás az eszközön | E1 | 3 |
| E5 | App-váz: állapot, útvonalak, szövegek, alapkomponensek | E1, E4 | 3–4 |
| E6 | Keret és első indítás | E5 | 4 |
| E7 | Tányér: sorok, választó, számolás, rögzítés, csere, ✓ | E2, E3, E5 | 5 |
| E8 | ⚠ panel és helyreállítás | E7 | 6 |
| E9 | Kész, mentés, Tányérjaim | E7 | 7 |
| E10 | Saját alapanyag | E7 | 7 |
| E11 | PWA, offline, iOS-tipp, biztonság | E7 | 8 |
| E12 | Alfa-kiadás és teszt | E6–E11 | 9 |

**Mérföldkövek**

- [x] **M1 — Üres oldal élesben:** E0 (a Pages-hez szükséges része) + E1 kész; a Pages URL-en
  megnyílik a „Ketara" oldal.
- [ ] **M2 — A motor kész:** E2 + E3 kész; a megoldó a prototípussal egyezik, az alapanyaglista
  ellenőrizve.
- [ ] **M3 — Első használható tányér:** E4–E7 kész; a PRD 6.4 ✓ példája a képernyőn.
- [ ] **M4 — Teljes v1-scope:** E8–E11 kész; a 2. folyamat (ismétlődő étkezés) végigmegy,
  repülőgép-módban is indul.
- [ ] **M5 — Alfa:** E12 kész; tesztelők használják.

A végleges alapanyaglista (S3.4) az M2 után **párhuzamosan** készülhet; addig az app a
prototípus listájával tesztelhető.

---

## E0 — Előfeltételek és döntések 👤

*Cél: minden, ami nem kód, de a fejlesztést vagy a kiadást blokkolja. Forrás: Tech 11., 12., 15.;
PRD 8.1.*

- [x] **S0.1 — GitHub beállítva a deployhoz** · *Kész, ha:* a `ketara-app` repóból GitHub
  Actions-szel lehet Pages-re deployolni.
  - [x] T0.1.1 👤 GitHub Pro előfizetés a `Izayata` fiókra (privát repóból csak így van Pages).
  - [x] T0.1.2 👤 `ketara-app` → Settings → Pages → *Source: GitHub Actions*.
  - [x] T0.1.3 👤 Branch-védelem a `main`-en: PR kötelező, a CI zöldje kötelező (a CI-job neve
    az E1 után ismert).
  - [ ] T0.1.4 👤 Helyi gépen: Node 26 és pnpm telepítve (`node -v` → v26.x, `pnpm -v`).
    Csak ha helyben is futtatnád az appot; a CI nélküle is működik.
- [x] **S0.2 — Saját domain eldöntve** (Tech 12., 15./1) · *Kész, ha:* tudjuk, marad-e a
  `izayata.github.io/ketara-app`, vagy saját domain jön. **A külső tesztelők előtt kell**, mert
  a tárolt adat a címhez kötött.
  - [x] T0.2.1 👤 Döntés: kell-e saját domain, és mi legyen. → **Egyelőre nem** (2026-11-13); az alfa a `izayata.github.io/ketara-app` címen fut.
  - [x] ~~T0.2.2 👤 Ha igen: domain megvásárlása, DNS-rekordok a Pages-hez.~~ Nem kell.
  - [x] ~~T0.2.3 Ha igen: `CNAME` a buildbe, Vite `base` átállítása `/`-re (Tech 9.).~~ Nem kell.
  - *Ha később mégis jön domain: a tesztelők addigi adatai a régi címen maradnak (Tech 12.).*
- [ ] **S0.3 — Dietetikusi átnézés** (PRD 8.1/1–2, Tech 15./2) · *Kész, ha:* a küszöbök, a
  figyelmeztetés szövege és a szénhidrát-értelmezés megerősítve. Nem blokkolja a fejlesztést,
  csak az alfát (E12).
  - [ ] T0.3.1 👤 A Keret-ellenőrzés küszöbei (1200 kcal, 50 g fehérje, 10%) megerősítve vagy
    lecserélve → `data/config.json`.
  - [ ] T0.3.2 👤 Az egészségügyi figyelmeztetés szövege jóváhagyva → `src/texts/hu.ts`.
  - [x] T0.3.3 👤 Nettó vagy teljes szénhidrát? → **Rosttal együtt** (2026-11-13), mert az orvos
    ezt nézi; a Tech terv v0.2 5.1 szerint.
  - [x] T0.3.5 👤 Saját alapanyag: kell-e „Rost” mező? → **Nem, csak egy emlékeztető sor** a
    szénhidrát alatt (2026-11-13; PRD 6.5, Folyamatok 6.2, T10.1.5).
  - [ ] T0.3.4 👤 A kategória-alapértékek (100 / 10 / 100 / 30 / 50 / 50 g) rendben vannak-e.
- [x] **S0.4 — Alfa-tesztelők** (PRD 8.1/3, Tech 15./3) · *Kész, ha:* megvan 3–5 ketogén
  étrendet követő tesztelő és egy időpont.
  - [x] T0.4.1 👤 Tesztelők kiválasztása és megkeresése.

---

## E1 — Repó-alap, CI, deploy

*Cél: üres, de élesben futó React-app minden minőségi kapuval. Forrás: Tech 3., 4., 11.*

- [x] **S1.1 — Projekt felállítása** · *Kész, ha:* `pnpm dev` egy „Ketara" oldalt mutat, a
  mappaszerkezet a Tech 4. szerinti.
  - [x] T1.1.1 Vite + React 19 + TypeScript (`strict`), pnpm; a Tech 4. mappaszerkezete üres
    `index.ts`-ekkel (`src/solver`, `src/domain`, `src/data`, `src/storage`, `src/texts`,
    `src/ui`, `data/`, `scripts/`, `tests/`).
  - [x] T1.1.2 ESLint + Prettier; `tsc --noEmit` script.
  - [x] T1.1.3 ESLint import-szabály a rétegekre: a `solver` semmit nem importál, a `domain` nem
    importál Reactot és böngésző-API-t, csak a `storage` nyúl a `localStorage`-hoz (Tech 3.).
  - [x] T1.1.4 Vite `base: '/ketara-app/'`; `lang="hu"` az `index.html`-ben.
  - [x] T1.1.5 README lecserélése: a félretett Cloudflare-terv helyett az alfa-terv, a
    dokumentumok aktuális verzióival (Tech 13.).
  - [x] T1.1.6 Verziók rögzítése: **Node 26** (Active LTS 2027 októberéig) a `.nvmrc`-ben és a
    `package.json` `engines.node` mezőjében (`>=26 <27`); **pnpm** pontos verzióval a
    `packageManager` mezőben. A Node 25-től a Corepack nem része a Node-nak, ezért a pnpm-et
    külön kell telepíteni (helyben `npm i -g pnpm`, a CI-ban `pnpm/action-setup`).
  - [x] T1.1.7 `pnpm-lock.yaml` commitolva (a `.gitignore` nem zárja ki); a CI
    `pnpm install --frozen-lockfile`-lal telepít, így eltérő lockfile-nál elbukik. A
    `.gitignore` Cloudflare-részének törlése (a félretett tervből maradt).
- [x] **S1.2 — Teszt-keretrendszer** · *Kész, ha:* `pnpm test` és `pnpm e2e` fut egy-egy
  mintateszttel.
  - [x] T1.2.1 Vitest + fast-check; egy mintateszt.
  - [x] T1.2.2 Playwright mobil nézettel (pl. Pixel 7 és iPhone 14 emuláció) a buildelt appon
    (`vite preview`); egy mintateszt.
- [x] **S1.3 — CI minden PR-on** · *Kész, ha:* egy hibás PR piros, egy jó zöld.
  - [x] T1.3.1 `.github/workflows/ci.yml`: Node a `.nvmrc`-ből (`actions/setup-node`), pnpm
    (`pnpm/action-setup`), `pnpm install --frozen-lockfile` → `tsc` → ESLint → (később `validate-data`) →
    Vitest → `vite build` → Playwright (Tech 11.).
  - [x] T1.3.2 pnpm-cache a CI-ban, hogy gyors maradjon.
  - [x] T1.3.3 👤 Az első CI-futás után a CI-job felvétele kötelező ellenőrzésként a `main` védelmébe (T0.1.3 második fele).
- [x] **S1.4 — Deploy a `main`-ről** · *Kész, ha:* merge után pár percen belül az új verzió a
  Pages URL-en van. (Függ: S0.1.)
  - [x] T1.4.1 `.github/workflows/deploy.yml`: build → `actions/upload-pages-artifact` →
    `actions/deploy-pages`.
  - [x] T1.4.2 Verzió és commit-hash a buildbe (`package.json` `0.x` + rövid hash, Vite
    `define`) (Tech 11.).
- [x] **S1.5 — Karbantartás** · *Kész, ha:* a Dependabot havonta nyit PR-t.
  - [x] T1.5.1 `.github/dependabot.yml` (npm + GitHub Actions, havonta).

---

## E2 — Megoldó (TS-port)

*Cél: a prototípus megoldója TypeScriptben, viselkedésváltozás nélkül, bizonyítottan.
Forrás: PRD 6.3–6.4, Tech 6.*

*Tesztszintek az E2-ben: egységtesztek modulonként (S2.2), a modulokat együtt futtató példa-,
orákulum- és tulajdonság-tesztek (S2.3–S2.5), benchmark (S2.6). Felületi (Playwright) teszt az
E2-ben nincs, mert még nincs felület; ezek az S7.7, S8.5 és S9.5.*

- [x] **S2.1 — Típusok és a tiszta interfész** · *Kész, ha:* a megoldó bemenete és kimenete
  tiszta adat, globális állapot nélkül.
  - [x] T2.1.1 Típusok: `SolverItem` (id, makrók 100 g-ra, referencia, rögzített gramm),
    `MealTarget` (sávok, arány, opcionális kcal-sáv), `SolverResult` (grammok, kiértékelés,
    státusz, okok, feloldás-javaslatok).
  - [x] T2.1.2 `MealTarget` számítása a napi keretből (napi sáv ÷ étkezésszám, pontos értékkel;
    PRD 6.1).
- [x] **S2.2 — A modulok portja** (Tech 6.1, 6.4/2) · *Kész, ha:* minden modul átkerült, és
  **minden modulnak saját egységteszt-fájlja van** (`tests/solver/unit/<modul>.test.ts`), amely
  a modult közvetlenül hívja, nem csak a `solvePlate`-en keresztül, és zöld. Minden ticket
  „Egységteszt" sora a minimum.
  - [x] T2.2.1 `simplex.ts`: kétfázisú szimplex, Bland-szabály, `≤ / ≥ / =` sorok, `1e-9` tűrés.
    *Egységteszt:* ismert optimum `≤`, `≥` és `=` sorokkal; negatív jobb oldal; nem megoldható
    és nem korlátos → `null`; redundáns egyenlőség (mesterséges változó kivezetése); Bland
    belépő és kilépő szabálya (holtversenynél rögzített csúcs; degenerált példán nem ciklizál);
    a bemenet nem változik.
  - [x] T2.2.2 `evaluate.ts`: makrók, kcal (4/4/9), arány, kilógás grammban egy grammkiosztásra.
    *Egységteszt:* összegek 100 g-ra, kcal 4/4/9, kilógás és hiány makrónként, a hiányzó zsír
    az arányhoz, `1e-9` alatti zaj nulla, fehérje nélkül az arány végtelen, a kcal nem dönt a
    sávról, a referencia-távolság csak a nem rögzített sorokra számol.
  - [x] T2.2.3 `stages.ts`: sávon belüli LP (minden szabad sor ≥ 1 g), kcal mint puha feltétel
    (előbb kcal-lal, aztán nélküle); a ⚠ ág a 0 g-os halmazokkal, a legkevesebbtől; a halmazok
    közül az arány → kilógás → 0 g-os sorok száma → referencia sorrend dönt.
    *Egységteszt:* sávon belüli törtgrammos tányér, minden szabad sor ≥ 1 g; kcal-mód `null`,
    ha a kcal nem teljesíthető, és a kcal-sávon belül marad, ha igen; ⚠ mód előbb az arányt,
    aztán a kilógást csökkenti; a 0 g-os halmaz sora 0 g; döntetlennél a korábbi sor marad a
    referenciánál; azonos arány és kilógás mellett a ⚠ ág a kevesebb 0 g-os sort választja, és
    az azonos méretű halmazokat is mind megnézi.
  - [x] T2.2.4 `compare.ts`: a ⚠-sorrend kulcsa (arány → kilógás → 0 g-os sorok → sávon belül:
    kcal → referencia → sorrend szerinti döntetlen; PRD 6.1, 6.3).
    *Egységteszt:* a kulcs elemeinek sorrendje és egymás előtti elsőbbsége; a kcal csak sávon
    belül számít; a 0 g-os sor a sávon belüli tányért is hátrébb sorolja; a `less` szigorú
    (döntetlennél a korábbi marad); hat tizedesre kerekítés.
  - [x] T2.2.5 `rounding.ts`: egész grammra kerekítés, ±1 g, majd ±2 g keresés; ha így sincs
    sávon belüli, ⚠ a kerekítés okával.
    *Egységteszt:* sima kerekítés megmarad, ha sávon belüli; ±1 g javít; ±2 g sem segít → nem
    sávon belüli, és ±1 g-os sávon belüli találat után nincs ±2 g-os keresés; a rögzített gramm
    nem változik, a 0 g-os sor 0 marad, a többi ≥ 1 g; ha a kerekítés gyorsított pontozót
    használ, az véletlen bemeneten ugyanazt adja, mint a sima `evaluate` + kulcs keresés.
  - [x] T2.2.6 `solvePlate.ts`: teljes futás — 3 alatt nem számol; minden rögzítve →
    kiértékelés; egyenkénti feloldás-előzetes rögzítésenként; „mindet feloldva" előzetes, ha
    egyik sem hoz sávba (PRD 6.4).
    *Egységteszt:* minden ág (3 alatt nem számol, 3-mal igen; minden rögzítve ✓ és ⚠; ✓; ⚠
    rögzítés nélkül; egy rögzítés zsákutcával; több rögzítés „mindet feloldva" előzetessel, és
    anélkül, ha egy feloldás elég); a kerekítés mint ok csak akkor, ha az LP sávon belüli volt;
    a kcal-sávot előbb próbálja; 0 g-os sorral sosem ✓; egyforma feloldásoknál a korábbi rögzítést ajánlja; két
    egyforma sornál a korábbi marad a referenciánál, és a sorok cseréje csak ezt cseréli fel;
    a rögzített gramm az előzetesekben sem változik.
  - [x] T2.2.7 `mealTarget.ts` (T2.1.2): *Egységteszt:* napi sáv ÷ étkezésszám mindkét határra,
    pontos értékkel (70 ÷ 3); kcal-sáv nélkül `null`; az arány-minimum változatlan.
  - [x] T2.2.8 *Egységtesztek ereje:* a `pnpm mutate:solver` (`scripts/mutate-solver.mjs`)
    rögzített listájának minden szándékos egysoros hibáját (jelenleg 31; pl. Bland kilépő
    szabályának elhagyása, a feloldás-döntetlen megfordítása) legalább egy egységteszt elkapja.
    Itt egységteszt csak a `tests/solver/unit/` fájlja; a példa-, orákulum- és
    tulajdonság-tesztek nem számítanak. A futás eredménysora („31/31 mutations caught …") a PR
    leírásába kerül. Új modul vagy új szabály esetén a lista is bővül; ami nem változtat a
    viselkedésen (egyenértékű hiba), nem kerül a listába, csak megjegyzésként.
- [x] **S2.3 — A PRD példái mint tesztek** (Tech 6.4/1) · *Kész, ha:* a dokumentumok minden
  számszerű példája rögzített tesztként zöld.
  - [x] T2.3.1 PRD 6.4 ✓ példa: 137 g tojás / 100 g brokkoli / 31 g olaj; CH 8,0 · Fe 20,1 ·
    Zs 45,1 · 2,2 : 1.
  - [x] T2.3.2 PRD 6.4 ⚠ példa: 240 g rögzített tojás → 48 g brokkoli / 39 g olaj, fehérje
    6,6 g-mal fölötte; az előzetes a ✓ példát adja.
  - [x] T2.3.3 A Folyamatok 5. fejezetének összes számszerű vázlata.
  - [x] T2.3.4 A PRD 7. peremesetei, amelyek számot adnak (0 g-ra szorulás, kerekítés miatti ⚠,
    kcal-megjegyzés, minden rögzítve).
- [x] **S2.4 — Orákulum-teszt** (Tech 6.4/3) · *Kész, ha:* több ezer véletlen tányéron a TS-port
  grammra pontosan egyezik a prototípussal.
  - [x] T2.4.1 A prototípus `solver.js`-ének bemásolása `tests/oracle/`-be (forrás: projekt
    mappa `ux/prototipus/src/solver.js`; a `ketara-artifacts` repóban nincs benne), változtatás
    nélkül, a forrás és a dátum megjelölésével.
  - [x] T2.4.2 Véletlen tányér-generátor: 3–8 alapanyag, véletlen keret, véletlen rögzítések,
    kcal-sávval és anélkül; rögzített seed, hogy egy hiba újrajátszható legyen.
  - [x] T2.4.3 Összevetés: grammok, státusz, ⚠-okok, feloldás-javaslat. Eltérésnél a teszt
    kiírja a bemenetet.
- [x] **S2.5 — Tulajdonság-alapú tesztek** (Tech 6.4/4) · *Kész, ha:* a fast-check tesztek
  zöldek.
  - [x] T2.5.1 ✓ esetén minden makró és az arány a sávon belül van a kiírt egész grammokon.
  - [x] T2.5.2 Sávon belül minden szabad sor ≥ 1 g; nincs negatív gramm; a rögzített gramm nem
    változik.
  - [x] T2.5.3 Determinizmus: ugyanarra a bemenetre ugyanaz a kimenet (PRD 6.5).
- [ ] **S2.6 — Teljesítmény** (Tech 6.3) · *Kész, ha:* a legrosszabb eset (8 alapanyag, több
  rögzítés, ⚠; a tippekkel együtt, amint azok elkészülnek az E8-ban) a CI-ban mérve belefér a
  keretbe. *2026-11-13-i mérés (ketara-app PR #3):* a legrosszabb eset kb. 230–300 ms, a cél
  nem teljesül, ezért a T2.6.2 szükséges (az E7-tel, amikor a felület először hívja a
  megoldót); a PR #3 600 ms-os korlátja csak regresszió-őr, nem ez a küszöb.
  - [ ] T2.6.1 Benchmark-teszt a CI-ban; cél: egy teljes újraszámolás 50 ms alatt egy
    középkategóriás telefonon (a CI-gépen ennek megfelelő, szigorúbb küszöb).
  - [ ] T2.6.2 *Csak ha a cél nem teljesül:* a megoldó Web Workerbe burkolása.

---

## E3 — Alapanyaglista és konfiguráció

*Cél: a gondozott lista és a konfiguráció JSON-ban, ellenőrizve, a buildbe fordítva.
Forrás: PRD 4.2, 6.1, 6.3; Tech 5.*

- [x] **S3.1 — Formátum és típusok** · *Kész, ha:* a TS-típusok a Tech 5.1–5.2 mezőit írják le,
  és a buildbe importálható a lista.
  - [x] T3.1.1 `src/data/types.ts`: `Ingredient` (id, name, accusative, category, kcal, protein,
    fat, carbohydrate, defaultGrams?, sample?, source, retired?), `IngredientList` (version,
    updated, items).
  - [x] T3.1.2 `data/config.json` + típus: a hat kategória (azonosító, magyar név, alapérték),
    a Keret gyanús-érték küszöbei, az arány alapértéke (2), a tányéronkénti maximum (8) (Tech 5.4).
  - [x] T3.1.3 Betöltő: a lista és a konfiguráció `import`-tal a buildbe, a kivezetett tételek
    szűrése a választóhoz.
- [x] **S3.2 — Ellenőrző szkript** (Tech 5.5) · *Kész, ha:* a `pnpm validate-data` minden
  szabályt ellenőriz, és a CI-ban fut.
  - [x] T3.2.1 Kötelező mezők, 0–100 közötti számok, `carbohydrate + protein + fat ≤ 100`.
  - [x] T3.2.2 Egyedi, `kebab-case` id-k; egyedi nevek (kis- és nagybetűtől függetlenül);
    létező kategória; kategóriánként pontosan egy nem kivezetett `sample`.
  - [x] T3.2.3 Eltűnt id tiltása: összevetés a `main` ág fájljával (id nem törölhető, csak
    `retired`).
  - [x] T3.2.4 Figyelmeztetések (nem hiba): 4/4/9-es kcal 15%-nál jobban eltér a tárolttól;
    40 alatti vagy 60 feletti tételszám; a `version` nem nőtt, pedig a tartalom változott.
  - [x] T3.2.5 Bekötés a `ci.yml`-be.
- [x] **S3.3 — Kiinduló lista a prototípusból** · *Kész, ha:* a prototípus 40 tétele JSON-ban,
  a validálás zöld (figyelmeztetés lehet).
  - [x] T3.3.1 A prototípus alapanyagainak átírása az új formátumba (id, tárgyeset, kategória,
    felülírás); a hiányzó `source.fdcId` mezők jelölése, hogy a végleges lista pótolja.
  - [x] T3.3.2 Rövid leírás a repóban (`data/README.md`): a mezők, a szabályok (id soha nem
    változik, törlés helyett `retired`, verzióemelés), hogyan futtasd a validálást.
- [ ] **S3.4 — A végleges alapanyaglista 👤** (PRD 4.2) · *Kész, ha:* 40–60 tétel, USDA FDC
  forrással, a validálás figyelmeztetés nélkül zöld, a lista PR-ral a `main`-en.
  - [ ] T3.4.1 👤 A tételek kiválasztása: keto-súlypont, mind a hat kategória lefedve.
  - [ ] T3.4.2 👤 Tápértékek a USDA FoodData Centralból, nyers értékek, 100 g-ra; `fdcId` minden
    tételnél. A `carbohydrate` = „Carbohydrate, by difference” változatlanul (rosttal
    együtt, Tech 5.1). Olyan rekordot válassz, amelyben ez megvan.
  - [ ] T3.4.3 👤 Magyar név és tárgyeset; egyedi alapérték-felülírás, ahol a kategória
    alapértéke nyilvánvalóan rossz (pl. saláta 50 g).
  - [ ] T3.4.4 👤 Kategóriánként egy mintaalapanyag (`sample`) kijelölése (a ⚠-tippekhez, PRD 6.4).

---

## E4 — Tárolás az eszközön

*Cél: minden felhasználói adat a `localStorage`-ban, verziózott sémával. Forrás: PRD 6.6,
Tech 7.*

- [x] **S4.1 — Séma és store** · *Kész, ha:* az állapot egy kulcs alatt mentődik és töltődik
  vissza, tesztekkel.
  - [x] T4.1.1 `src/storage/schema.ts`: `StoredState` (`schemaVersion`, `keret`,
    `customIngredients`, `plates`, `lastUsed`, `currentPlate`, `ui`) (Tech 7.2).
  - [x] T4.1.2 `src/storage/store.ts`: interfész (`load`, `save`, `subscribe`) és
    `localStorage`-implementáció a `ketara` kulcson; memóriás implementáció a tesztekhez.
  - [x] T4.1.3 Sérült vagy ismeretlen adat kezelése: nem omlik össze, a hibás részt eldobja, a
    többit megtartja.
- [x] **S4.2 — Migrációk** · *Kész, ha:* van migrációs lánc, és egy próba-migráció tesztje zöld.
  - [x] T4.2.1 `src/storage/migrations.ts`: `schemaVersion` szerinti sorrendes migrálás,
    visszafelé nem.
- [x] **S4.3 — Robusztusság** · *Kész, ha:* két fül, letiltott tárolás és tartós tárolás kérése
  működik.
  - [x] T4.3.1 Két nyitott fül: a `storage` esemény újratölti az állapotot; az utolsó írás nyer
    (Tech 7.3).
  - [x] T4.3.2 Ha az írás hibát dob (privát böngészés): memóriában fut tovább, és jelzi a UI-nak
    (a sáv: S11.4) (PRD 6.6, Tech 7.4).
  - [x] T4.3.3 `navigator.storage.persist()` első indításkor (Tech 7.5).
- [x] **S4.4 — Hivatkozások épsége** · *Kész, ha:* eltűnt alapanyagra hivatkozó tányér nem
  omlik össze.
  - [x] T4.4.1 Ismeretlen vagy kivezetett `ingredientId` a tányérban: a kivezetett működik tovább,
    az ismeretlen kimarad, és a tányér hiányos állapotban nyílik (PRD 6.5, Tech 7.2).

---

## E5 — App-váz

*Cél: állapotkezelés, útvonalak, szövegek, formázás és a közös UI-elemek, mielőtt a képernyők
jönnek. Forrás: Tech 3., 9., 10.; UX 2., 3., 9.*

- [ ] **S5.1 — Állapot** · *Kész, ha:* egy store kezeli az app állapotát, és minden változás
  kiíródik.
  - [ ] T5.1.1 `useReducer` + context; akciók a domain-műveletekre (Keret mentése, sor
    hozzáadása, rögzítés, csere, Kész, mentés…).
  - [ ] T5.1.2 Írás a store-ba minden változás után; a visszavonható törlés ~5 mp-ig csak
    memóriában vár (Tech 7.2).
  - [ ] T5.1.3 Újraszámolás minden tányér-változásra (megoldó hívása a reducerből vagy egy
    effectből, egy helyen).
- [ ] **S5.2 — Útvonalak** (Tech 9.) · *Kész, ha:* a hash-útvonalak működnek, és az Android
  vissza-gomb az alsó lapot zárja be, nem az appot.
  - [ ] T5.2.1 Hash-router (`#/`, `#/tanyerjaim`, `#/keret`, `#/sajat`, `#/sajat/:id`); döntés
    `react-router` vagy kis saját megoldás között.
  - [ ] T5.2.2 Alsó lap megnyitása history-bejegyzést tesz le; vissza → a lap bezárul.
  - [ ] T5.2.3 Első indítás: ha nincs keret, a Keret nyílik (F 3.).
- [ ] **S5.3 — Szövegek és formázás** (Tech 10.) · *Kész, ha:* minden felületi szöveg a
  `hu.ts`-ben van, és a formázók tesztjei zöldek.
  - [ ] T5.3.1 `src/texts/hu.ts`: a Folyamatok 7. táblázatának minden sora, sablonfüggvényként a
    változókkal ({Alapanyag}, {n,n}, {min} …).
  - [ ] T5.3.2 `src/domain/format.ts`: tizedesvessző (`Intl.NumberFormat('hu-HU')`), egy
    tizedes a makróknál, egész számnál tizedes nélkül a célnál („23,3", „20"); a kilógás minimum
    „0,1 g"; arány „2,2 : 1"; sáv „5–10".
  - [ ] T5.3.3 A „(nyersen)" egyetlen formája egy helyen: sorban, mondatban, felsorolás végén
    (PRD 7.).
  - [ ] T5.3.4 Ékezet- és kisbetű-független keresés, `localeCompare(…, 'hu')` rendezés.
- [ ] **S5.4 — Vizuális alap és közös komponensek** (UX 9.) · *Kész, ha:* a képernyők ezekből
  épülnek, és a prototípus kinézetét adják.
  - [ ] T5.4.1 Design-tokenek CSS-változókban a prototípusból (törtfehér, grafit, mélyzöld
    akcentus, borostyán ⚠, nincs piros); rendszerbetű, táblázatos számjegyek; méretek
    (24 / 17 / 15 / 14).
  - [ ] T5.4.2 Elrendezés: egy oszlop, asztali gépen középre igazítva; álló tájolás (UX 2.).
  - [ ] T5.4.3 `BottomSheet` komponens (fejléc, ✕, görgethető tartalom, fókuszkezelés).
  - [ ] T5.4.4 `UndoSnackbar`: 5 mp-es sáv „Visszavonás" gombbal, és rövid visszajelzések
    („Mentve: …", „Megjegyeztem. …").
  - [ ] T5.4.5 `ConfirmSheet` a három megerősítő kérdéshez (UX 8.2).
  - [ ] T5.4.6 Számmező: `inputmode="decimal"`, vessző és pont is elfogadott.
  - [ ] T5.4.7 Akadálymentesség: valódi gombok és címkék, 44 px érintési cél, a rendszer
    betűméretének követése.

---

## E6 — Keret és első indítás

*Cél: a felhasználó egyszer beírja a dietetikus számait, és látja az étkezési célt.
Forrás: PRD 6.1, 6.7; F 3., 6.4; UX 8.*

- [ ] **S6.1 — A Keret képernyő** · *Kész, ha:* a Keret kitölthető, menthető, újranyitva a
  mentett értékekkel jön.
  - [ ] T6.1.1 Mezők párban (CH, fehérje, zsír, kcal alsó–felső), étkezésszám, arány (csak a
    zsír oldala, alapérték 2); „Tovább" a következő mezőre ugrik.
  - [ ] T6.1.2 A kcal-sáv opcionális, fölötte: „Ha a papíron van."
  - [ ] T6.1.3 „Egy étkezésre ez jön ki:" blokk élőben frissül gépelés közben.
  - [ ] T6.1.4 Első indításkor fölül a bevezető mondat, a gomb `Ez stimmel, kezdjük`; később
    `Mentés`.
  - [ ] T6.1.5 Alul a figyelmeztetés egy sorban, és a verziósor: „Ketara {verzió} ·
    Alapanyaglista: {n}. változat" (Tech 11.).
- [ ] **S6.2 — Keret-ellenőrzés** (`src/domain/keret.ts`) · *Kész, ha:* a logikai hibák nem
  menthetők, a gyanús értékekre egyszer visszakérdez; tesztekkel.
  - [ ] T6.2.1 Logikai hiba: alsó > felső; zsírsáv teteje < {min} × fehérjesáv alja. A hibás sor
    alatt borostyán szöveg, mentés tiltva.
  - [ ] T6.2.2 Gyanús érték a `config.json` küszöbeivel (kcal felső < 1200, ha van; fehérje felső
    < 50 g; sávszélesség < a felső 10%-a): párbeszéd `Igen, így van` / `Javítom`, alacsony és
    szűk változattal.
- [ ] **S6.3 — Első indítás végig** · *Kész, ha:* üres böngészőben a Keret → üres Tányér út
  végigmegy (e2e teszt).
  - [ ] T6.3.1 Mentés után üres Tányér a `+ Alapanyag` és a nagy `Tányérjaim` gombbal (F 5.1).
  - [ ] T6.3.2 A fejléc cél-sávja (CH · Fe · Zs ⚙) a Keretet nyitja (UX 3.).
  - [ ] T6.3.3 Playwright: első indítás → Keret → Tányér.

---

## E7 — Tányér: sorok, választó, számolás, rögzítés, csere, ✓

*Cél: a termék fő útja a ✓ ágig. Forrás: PRD 6.2, 6.2b, 6.3; F 1., 5.1–5.3, 5.5, 6.1;
UX 4.1–4.5, 4.7, 5.*

- [ ] **S7.1 — A referencia** (`src/domain/reference.ts`) · *Kész, ha:* a referencia forrásának
  sorrendje tesztelve.
  - [ ] T7.1.1 Sorrend: mentett tányér tárolt grammja → „legutóbb használt" → egyedi felülírás →
    kategória-alapérték (PRD 6.3).
  - [ ] T7.1.2 A „legutóbb használt" frissítése: csak Kész, Tányér mentése, frissítés és Mentés új
    néven; 0 g nem írja felül; a sima újraszámolás nem.
- [ ] **S7.2 — A Tányér képernyő állapotai** · *Kész, ha:* üres, alulhatározott, sávon belüli és
  minden rögzített állapot a Folyamatok 5. szerint jelenik meg.
  - [ ] T7.2.1 Zónák: fejléc (Tányérjaim · név · cél-sáv), sorok, `+ Alapanyag`, makrósor,
    a képernyő aljához tapadó eredménypanel (UX 4.1).
  - [ ] T7.2.2 Üres állapot (F 5.1) és alulhatározott: „— g", „Még egy / két alapanyag kell a
    számoláshoz." (F 5.2).
  - [ ] T7.2.3 Sor: név, kategória, gramm + „(nyersen)", táblázatos számjegyek; 0 g borostyánnal;
    képernyőolvasó-szöveg (UX 4.2, 9.).
  - [ ] T7.2.4 Felvillanás 1 mp-ig, ha egy gramm megváltozott újraszámolás után.
  - [ ] T7.2.5 Makrósor minden állapotban: CH · Fe · Zs · arány; sávon kívül borostyán + ↑/↓
    (UX 4.4).
  - [ ] T7.2.6 ✓ eredménypanel: „Minden érték a sávon belül.", kcal-megjegyzés, ha kell; gombok
    `Tányér mentése` és `Kész` (a mentés helye az E9-ben teljes).
  - [ ] T7.2.7 Minden rögzítve: kiértékelés ✓/⚠ címsorral, „Nincs mit kiszámolni — minden adag
    rögzítve." (F 5.5, UX 4.7).
- [ ] **S7.3 — Alapanyag-választó** (alsó lap) · *Kész, ha:* több alapanyag egy menetben
  felvehető, a 8-as korlát működik.
  - [ ] T7.3.1 Legutóbb használt (max. 8 chip), a hat kategória helyben kinyíló listával.
  - [ ] T7.3.2 Keresés (nem kap automatikusan fókuszt); állapotok: üres, találatok, nincs találat
    („Felviszed sajátként?" → a beírt szöveg a névmezőbe, E10).
  - [ ] T7.3.3 Többes választás pipával, `Hozzáadás ({n})`; a tányéron lévők halványan, pipával.
  - [ ] T7.3.4 8 alapanyagnál a többi halvány, és a korlát szövege látszik (PRD 6.2).
  - [ ] T7.3.5 A választó egy adott kategórián nyitva is indítható, fejléc-sorral (a ⚠-hoz, E8).
- [ ] **S7.4 — Rögzítés** · *Kész, ha:* ikonnal és grammbeírással is rögzíthető, a megoldó köré
  számol.
  - [ ] T7.4.1 ◯ / 🔒 ikon: koppintás rögzíti a jelenlegi grammot, újabb feloldja; rögzítettnél
    félkövér, akcentusszínű gramm és „rögzítve" (UX 4.2).
  - [ ] T7.4.2 A gramm beírása soron belül, numerikus billentyűzettel; jóváhagyáskor rögzít.
- [ ] **S7.5 — Sor-műveletek és csere** · *Kész, ha:* törlés és csere visszavonható, a csere
  leveszi a rögzítést.
  - [ ] T7.5.1 Sor-műveletek lap a névre koppintva: `Rögzítés` / `Feloldás` · `Csere` · `Törlés`.
  - [ ] T7.5.2 Törlés visszavonással („Törölve: {Alapanyag}. Visszavonás").
  - [ ] T7.5.3 Csere-lap: a kategória tagjai a tányéron lévők nélkül; csere helyben, a sor
    sorrendje marad; az új alapanyag a saját referenciájáról indul (PRD 6.2b).
  - [ ] T7.5.4 Rögzített sor cseréje: „A rögzítés lekerült, mert az alapanyag változott." +
    Visszavonás.
  - [ ] T7.5.5 „Másik kategóriából…": a sor törlődik, a választó minden kategóriával nyílik.
- [ ] **S7.6 — Az aktuális tányér megmarad** (PRD 6.6) · *Kész, ha:* bezárás és újranyitás után
  ugyanaz a tányér vár, a rögzítésekkel.
  - [ ] T7.6.1 `currentPlate` mentése minden változáskor, visszatöltés induláskor.
- [ ] **S7.7 — E2E: a ✓ példa** · *Kész, ha:* a Playwright-teszt a PRD 6.4 ✓ példáját a
  képernyőn látja (137 / 100 / 31 g).
  - [ ] T7.7.1 Playwright: Keret → tojás, brokkoli, olaj → ✓ és a grammok.

---

## E8 — ⚠ panel és helyreállítás

*Cél: ⚠ esetén a termék megmondja, mi csúszik ki, miért, és melyik lépés visz vissza.
Forrás: PRD 6.4, 7.; F 1., 5.4, 7.; UX 4.6, 4.7.*

- [ ] **S8.1 — A ⚠ sorai** · *Kész, ha:* a ⚠ sorai a rögzített sorrendben, mindegyik egyszer,
  helyes szöveggel jelennek meg; tesztekkel.
  - [ ] T8.1.1 Sorrend: arány → kilógó makrók (grammban nagyobb elöl) → 0 g → kerekítés → kcal;
    az első a címsor (félkövér, borostyán).
  - [ ] T8.1.2 Ok-mondat, ha a rögzítés az ok: felső és alsó él szövege (F 7.).
  - [ ] T8.1.3 0 g-ra szorult sor és a kerekítés-ok szövege.
- [ ] **S8.2 — Feloldás és előzetes** · *Kész, ha:* a ⚠ példa (240 g rögzített tojás) a
  képernyőn a helyes előzetessel és gombbal jelenik meg.
  - [ ] T8.2.1 Egy rögzítés: „Ha feloldod: {Alapanyag}" keretezett előzetes (sikeres vagy
    részleges), gomb `{Alapanyag} feloldása` (elsődleges).
  - [ ] T8.2.2 Több rögzítés: a legjobb egyetlen feloldás felajánlása; `Rögzítések feloldása` és
    „Ha feloldod mindet:" csak akkor, ha egyik egyedi feloldás sem hoz sávba.
  - [ ] T8.2.3 „Ebből a készletből nem jön ki" eset, ha a feloldás sem elég (PRD 7.).
- [ ] **S8.3 — Tippek rögzítés nélkül** (`src/domain/hints.ts`) · *Kész, ha:* a négy ág
  (arány, kevés, sok, zsákutca) tesztelve, és a gombok a helyes lapot nyitják.
  - [ ] T8.3.1 A prototípus `app.html`-jéből a tipp-logika portja a domainbe (Tech 6.1).
  - [ ] T8.3.2 Arány sérül → `+ Zsiradék`, a választó a Zsiradékon nyílik.
  - [ ] T8.3.3 Kevés → a hat mintaalapanyag próbafuttatása, a legjobban segítő kategória;
    `+ {Kategória}` a választót azon nyitja, „A {makróhoz} ezek segítenek a legtöbbet."
  - [ ] T8.3.4 Sok → a legtöbbet adó nem rögzített alapanyag; `{Alapanyag} cseréje` a
    csere-lapot azon a soron nyitja.
  - [ ] T8.3.5 Zsákutca: „Ezekből az alapanyagokból nem jön ki.", elsődleges gomb `+ Alapanyag`.
- [ ] **S8.4 — A panel elrendezése** · *Kész, ha:* hosszú ⚠ esetén a panel belül görög, a
  gombsor alul marad; a `Kész` és a mentés helye mindig ott van (UX 4.1, 4.6).
  - [ ] T8.4.1 Panel görgetés és gombsor-hierarchia.
- [ ] **S8.5 — E2E: a ⚠ példa** · *Kész, ha:* Playwright: tojás 240 g-ra rögzítve → 48 / 39 g,
  „Fehérje: 6,6 g-mal a sáv fölött.", feloldás után a ✓ példa.
  - [ ] T8.5.1 Playwright-teszt.

---

## E9 — Kész, mentés, Tányérjaim

*Cél: az ismétlődő étkezés (F 2.) végigmegy. Forrás: PRD 6.5, 6.6; F 2., 5.3, 6.3; UX 4.5,
4.8, 7.*

- [ ] **S9.1 — Kész** · *Kész, ha:* a Kész frissíti a „legutóbb használt" értékeket, a tányér a
  képernyőn marad, a következő megnyitás üres tányérral indul.
  - [ ] T9.1.1 Kész ✓ és ⚠ esetén is; 0 g-os sor kivételével frissíti a `lastUsed`-ot;
    „Megjegyeztem. Legközelebb ebből indulok."
  - [ ] T9.1.2 A `currentPlate` törlése a következő megnyitásra (PRD 6.6).
- [ ] **S9.2 — Tányér mentése** · *Kész, ha:* a tányér egyedi néven menthető, 0 g-os sor nélkül.
  - [ ] T9.2.1 Mentés lap kitöltött, kijelölt névjavaslattal („Tojás, brokkoli, olaj").
  - [ ] T9.2.2 Foglalt név: „Már van ilyen nevű tányér.", a mentés addig nem aktív.
  - [ ] T9.2.3 ⚠ esetén a lap emlékeztet a címsorra; 0 g-os sor kimarad, és a lap kiírja.
  - [ ] T9.2.4 Mentés után „Mentve: {Név}", a tányér mentett tányérként folytatódik (fejlécben
    a neve); a `lastUsed` frissül.
- [ ] **S9.3 — Mentett tányér frissítése** (`src/domain/plates.ts`) · *Kész, ha:* a mentés helye
  a három esetben helyes (új / eltér / nem tér el); tesztekkel.
  - [ ] T9.3.1 Eltérés-vizsgálat: a sorok halmaza vagy bármelyik kiírt egész gramm.
  - [ ] T9.3.2 `{Név} frissítése` (az összetételt is felülírja; „Frissítve. Legközelebb innen
    indul.") és `Mentés új néven`; ha nincs eltérés, csak `Kész`.
- [ ] **S9.4 — Tányérjaim** · *Kész, ha:* a lista, a megnyitás, az átnevezés és a törlés
  működik.
  - [ ] T9.4.1 Lista: név + alapanyagok, legutóbb használt elöl; üres állapot szövege (F 6.3).
  - [ ] T9.4.2 Megnyitás: alapanyagok betöltve, rögzítés nélkül, újraszámolva; fejléc alatt
    „{Név} — a mai célra újraszámolva."
  - [ ] T9.4.3 Nem mentett munka esetén: „A mostani tányér nincs elmentve. Lecseréled?"
    (`Lecserélem` / `Mégse`); ugyanez a `+ Új tányér`-nál.
  - [ ] T9.4.4 „…" menü: `Átnevezés` (egyedi név) · `Törlés` visszavonással.
  - [ ] T9.4.5 Hiányos tányér (3-nál kevesebb sor) megnyitása: a hiányzó alapanyagokat kéri
    (PRD 6.5).
- [ ] **S9.5 — E2E: ismétlődő étkezés** · *Kész, ha:* Playwright: tányér összeállítása → mentés
  → új tányér → Tányérjaim → megnyitás → csere → frissítés.
  - [ ] T9.5.1 Playwright-teszt.

---

## E10 — Saját alapanyag

*Cél: ami a gondozott listából hiányzik, felvihető, szerkeszthető, törölhető.
Forrás: PRD 4.3, 6.5, 7.; F 6.1, 6.2; UX 6.*

- [ ] **S10.1 — Felvitel** · *Kész, ha:* saját alapanyag felvihető, kereshető, és a választóból
  jövet rögtön a tányérra kerül.
  - [ ] T10.1.1 Lap: név, kategória választógombsor (6 elem, kötelező), CH / fehérje / zsír
    100 g-ra, nyersen; id `u-` + `crypto.randomUUID()`.
  - [ ] T10.1.2 Élő ellenőrzés: CH + Fe + Zs ≤ 100, „100 grammban ennyi nem fér el. Nézd meg a
    csomagolást."
  - [ ] T10.1.3 Egyedi név (a gondozott és a saját tételek között is, kis- és nagybetűtől
    függetlenül).
  - [ ] T10.1.4 „Nincs találat" → a beírt szöveg a névmezőben; mentés után a tányérra kerül.
  - [ ] T10.1.5 Emlékeztető sor a szénhidrát mező alatt, mindig látszik: „Ha a csomagoláson külön sorban van a rost, add hozzá a szénhidráthoz." (F 6.2).
- [ ] **S10.2 — Szerkesztés és törlés** · *Kész, ha:* a „…" menüből szerkeszthető és törölhető,
  a tányérokra gyakorolt hatással együtt; tesztekkel.
  - [ ] T10.2.1 Szerkesztés (ugyanaz a lap, kitöltve): név, kategória, tápérték.
  - [ ] T10.2.2 Törlés, ha tányér használja: megerősítő lap a tányérok felsorolásával (`Törlöm` /
    `Mégse`); a sor kikerül a tányérokból, az üressé váló tányér törlődik, a 3 alá eső hiányos
    marad.
  - [ ] T10.2.3 Törlés az aktuális tányérról és a `lastUsed`-ból is.

---

## E11 — PWA, offline, iOS-tipp, biztonság

*Cél: telepíthető, hálózat nélkül is induló app, az adat védelmével. Forrás: PRD 6.6; UX 2.;
Tech 7.4–7.5, 8., 10.*

- [ ] **S11.1 — Telepíthetőség** · *Kész, ha:* Androidon és iOS-en a kezdőképernyőre téve
  appként nyílik.
  - [ ] T11.1.1 `manifest.webmanifest`: „Ketara", `lang: hu`, `standalone`, álló tájolás.
  - [ ] T11.1.2 Ikonok (192, 512, maskable) és Apple touch icon. 👤 Az ikon kinézetéről te
    döntesz; addig ideiglenes ikon.
- [ ] **S11.2 — Offline** · *Kész, ha:* repülőgép-módban is indul és számol (e2e teszt).
  - [ ] T11.2.1 `vite-plugin-pwa` (Workbox): a build minden fájlja előre gyorsítótárban.
  - [ ] T11.2.2 Frissítés csak a következő megnyitáskor, újratöltés és kérdés nélkül (Tech 8.2).
  - [ ] T11.2.3 Playwright: offline újranyitás.
- [ ] **S11.3 — iOS-tipp** · *Kész, ha:* csak iOS Safariban, nem telepítve jelenik meg, és bezárás
  után soha többé.
  - [ ] T11.3.1 Felismerés (iOS + Safari + `navigator.standalone !== true`), sáv a Tányér tetején,
    semleges színnel, `Értem` gombbal; `ui.iosInstallHintDismissed`.
- [ ] **S11.4 — Ha nem lehet menteni** · *Kész, ha:* privát böngészésben a sáv látszik, az app
  működik.
  - [ ] T11.4.1 Sáv: „Ebben a böngészőben nem tudok adatot menteni. Bezáráskor minden elveszik."
- [ ] **S11.5 — Biztonság és külső hívások** · *Kész, ha:* futás közben nincs hálózati kérés
  külső címre.
  - [ ] T11.5.1 Atkinson Hyperlegible saját hosztolva (a prototípus Google Fonts-a helyett) —
    vagy a UX 9. szerinti rendszerbetű; döntés az első képernyőnél. *(A Tech 4. és a UX 9. itt
    eltér: lásd lent, Nyitott pontok.)*
  - [ ] T11.5.2 `Content-Security-Policy` meta-tag `default-src 'self'`-fel.

---

## E12 — Alfa-kiadás és teszt

*Cél: valódi használat a felhasználóval, majd néhány tesztelővel. Forrás: PRD 8.1/3, 9.;
Tech 14./9.*

- [ ] **S12.1 — Kiadás előtti ellenőrzés** · *Kész, ha:* minden pont kipipálva.
  - [ ] T12.1.1 A dietetikusi pontok (S0.3) bekerültek a `config.json`-ba és a szövegekbe.
  - [ ] T12.1.2 A végleges alapanyaglista (S3.4) a `main`-en.
  - [ ] T12.1.3 Kézi próba valódi telefonon: iPhone Safari (telepítve és nem), Android Chrome;
    offline; két fül.
  - [ ] T12.1.4 Verzió `0.1.0` címkével (git tag) és rövid változásnaplóval.
- [ ] **S12.2 — Alfa a projektgazdával 👤** · *Kész, ha:* egy hétig a saját étkezéseidhez
  használtad, és a hibák ticketként fel vannak víve.
  - [ ] T12.2.1 👤 Egy hét valódi használat; észrevételek gyűjtése.
  - [ ] T12.2.2 A talált hibák ticketként ebbe a backlogba (új „E13 — Alfa-javítások" epic).
- [ ] **S12.3 — Alfa külső tesztelőkkel 👤** (PRD 9.) · *Kész, ha:* a tesztelők kipróbálták, és
  megvan a válasz a PRD 8.1/3 kérdésére (van-e igény a körön túl). Függ: S0.2 (domain), S0.4.
  - [ ] T12.3.1 👤 Rövid útmutató a tesztelőknek (link, telepítés, mire figyeljenek).
  - [ ] T12.3.2 👤 Tesztelés és visszajelzések összegyűjtése a PRD 9. feltételezései mentén.
  - [ ] T12.3.3 A PRD 8.1/3 és 9. frissítése az eredménnyel.

---

## Nyitott pontok a dokumentumok között

Ezeket a backlog írásakor vettem észre; egyik sem blokkolja az indulást, de a ticketeknél
dönteni kell róluk.

1. **Betűtípus:** a Tech terv 4. Atkinson Hyperlegible-t ír saját hosztolva, a UX 9.
   rendszerbetűt (SF Pro / Roboto). A rendszerbetű egyszerűbb és nem kell letölteni. → T11.5.1.
2. **A prototípus forrása** (`solver.js`, `app.html`) nincs a `ketara-artifacts` repóban, csak a
   projekt mappában (`ux/prototipus/src/`). Az orákulum-teszt (S2.4) és a tipp-logika portja
   (S8.3) ebből dolgozik.
3. **A Tech terv 1.** még „PRD v1.2 teljes v1-es scope"-ot ír; az aktuális a v1.4 (csak az
   iOS-tippel és a szénhidrát pontosításával bővült, a scope ugyanaz).

## Döntésnapló

| Dátum | Döntés | Indok |
|---|---|---|
| 2026-11-13 | Backlog epic → story → ticket bontásban, egy `.md` fájlban, pipálható listákkal | a felhasználó kérése |
| 2026-11-13 | Egyelőre nincs saját domain; az alfa a GitHub Pages címén fut | a felhasználó döntése; költözéskor a tesztelők adatai a régi címen maradnak |
| 2026-11-13 | Szénhidrát rosttal együtt (a korábbi „rost nélkül” aznap felülírva) | az orvos a rosttal együtti CH-t nézi |
| 2026-11-13 | Az epicek a Tech terv 14. munkasorrendjét követik, E0 a felhasználó előfeltételeivel | egy sorrend a tervben és a backlogban |
| 2026-11-13 | A T2.2.8 ellenőrzése: rögzített hibalista (`pnpm mutate:solver`), csak a `tests/solver/unit/` számít, az eredmény a PR-ba kerül | a második átnézés szerint a korábbi megfogalmazás nem volt ellenőrizhető; a felhasználó jóváhagyta |
| 2026-11-13 | Minden megoldó-modulnak saját egységteszt-fájl kell (S2.2, T2.2.1–T2.2.8) | a felhasználó kérése a ketara-app PR #3 átnézése után: a modulok addig csak a `solvePlate`-en keresztül voltak tesztelve, és két szándékos hiba átment |
