# Ketara — Tech terv (alfa) v0.2

*Élő dokumentum. Helyben frissül, ahogy a döntések megszületnek.*
*Alapja: PRD v1.3, Folyamatok és képernyők v0.10, UX-terv v0.6, prototípus v1.1.*
*Utolsó frissítés: 2026-11-13*

---

## 1. Mit épít az alfa

Az alfa a PRD v1.2 teljes v1-es scope-ja (PRD 4.), **szerver nélkül**: egy statikus
weboldal, amely a böngészőben fut, és minden adatot az eszközön tárol. Nincs fiók,
belépés, szinkron, adatbázis, analitika és export (PRD 5.).

Kinek: a projektgazda és néhány más ketogén étrendet követő tesztelő (PRD 8.1/3). Az
alfa célja kideríteni, van-e igény a terméken túl a projektgazda körén.

**Nem célja:** pénzt keresni. A PRD szerint a v1 ingyenes. Ha a termék beindul és
fizetőssé válik, az új döntés; a terv ehhez kilépési utat tart nyitva (12.).

## 2. Döntések egy helyen

| Téma | Döntés | Ki döntötte |
|---|---|---|
| Hosztolás | **GitHub Pages**, közvetlenül a privát `Izayata/ketara-app` repóból (**GitHub Pro** előfizetés kell hozzá) | felhasználó, 2026-11-13 |
| Kód | **React + TypeScript**, Vite | felhasználó |
| Repó | `Izayata/ketara-app`, **privát** marad (a termék később pénzt hozhat) | felhasználó, 2026-11-13 |
| Alapanyaglista | **JSON fájl**; kötelező mezők: id, név, kcal, fehérje, zsír, szénhidrát (+ a PRD által megkövetelt mezők, 5.) | felhasználó |
| kcal | A kcal-sáv **mindenhol 4/4/9-cel** számol (PRD 6.1 változatlan). A JSON kcal-értéke csak **ellenőrző adat** | felhasználó, 2026-11-13 |
| A lista összeállítása | A **felhasználó** állítja össze; a terv a formátumot és az ellenőrző szkriptet adja | felhasználó, 2026-11-13 |
| iOS adatvesztés | **Egyszeri, bezárható tipp** iOS Safariban: tedd a kezdőképernyőre (8.3) | felhasználó, 2026-11-13 |
| Nyelv | Tervek és felület magyarul; kód, azonosítók, commitok angolul | felhasználó, 2026-11-13 |
| Helyi tárolás | `localStorage`, verziózott séma, migrációk (7.) | terv |
| Offline | Telepíthető PWA, service worker (8.) | terv (PRD 6.6 követeli) |
| Útvonalak | Hash-alapú (`#/tanyerjaim`), mert a Pages nem tud SPA-átirányítást (9.) | terv |
| Megoldó | A prototípus `solver.js`-ének TS-portja, saját szimplex, külső LP-könyvtár nélkül (6.) | terv (a 2026-10-26-i tervből átvéve) |
| Tesztek | Vitest; a PRD példái és a prototípus mint orákulum (6.4) | terv |
| CI/CD | GitHub Actions: ellenőrzés minden PR-on, deploy a `main`-ről (11.) | terv |

A 2026-10-26-i tech tervből (Cloudflare Workers, D1, Dexie, saját belépés, titkosítás,
analitika) a scope-vágás után csak a Vite, a PWA, a megoldó portja és a CI marad meg.

## 3. Architektúra

```
┌──────────────────────── böngésző (telefon) ────────────────────────┐
│                                                                     │
│  UI (React)  ──►  állapot (reducer)  ──►  tárolás (localStorage)    │
│      │                  │                                           │
│      │                  ▼                                           │
│      │          tartomány: megoldó, szabályok, szövegek             │
│      │                  ▲                                           │
│      └──── gondozott lista + konfiguráció (JSON, a buildbe fordítva)│
│                                                                     │
│  service worker: minden fájl gyorsítótárban → offline működés       │
└─────────────────────────────────────────────────────────────────────┘
                 ▲  egyszer letöltve, utána csak frissítéskor
        GitHub Pages (statikus fájlok, HTTPS)
```

Azure-hasonlattal: olyan, mint egy **Azure Static Web App API nélkül**. Nincs Functions,
nincs Cosmos DB: a „backend" a böngésző maga. A GitHub Pages a Static Web Apps statikus
hosztolásának felel meg, a GitHub Actions a deploy pipeline.

**Rétegek, és ki mit importálhat:**

| Réteg | Mappa | Függ |
|---|---|---|
| Megoldó | `src/solver/` | semmitől (tiszta TS, nincs React, nincs böngésző-API) |
| Tartomány | `src/domain/` | megoldó, adattípusok (referencia-forrás, ⚠-tippek, mentési szabályok, Keret-ellenőrzés) |
| Adat | `src/data/` | gondozott lista és konfiguráció betöltése, típusai |
| Tárolás | `src/storage/` | tartomány típusai; csak ez a réteg nyúl a `localStorage`-hoz |
| Szövegek | `src/texts/hu.ts` | — |
| UI | `src/ui/` | minden fenti |

A megoldó és a tartomány **nem ismeri a Reactot**, így Node-ban tesztelhető, és ha egyszer
szerver vagy natív app kell, változatlanul átvihető. Ezt egy ESLint import-szabály őrzi.

## 4. Repó és eszközök

`Izayata/ketara-app` (privát), egyetlen csomag:

```
data/ingredients.json        gondozott alapanyaglista (5.)
data/config.json             kategóriák, alapértékek, Keret-küszöbök (5.4)
scripts/validate-data.ts     a két JSON ellenőrzése (5.5)
src/solver/                  simplex.ts, evaluate.ts, stages.ts, rounding.ts, solvePlate.ts
src/domain/                  reference.ts, hints.ts, keret.ts, plates.ts, format.ts
src/storage/                 schema.ts, migrations.ts, store.ts
src/texts/hu.ts              minden felületi szöveg (Folyamatok 7.)
src/ui/                      screens/, sheets/, components/
public/                      ikonok, manifest, betűtípusok
tests/                       megoldó- és tartománytesztek, PRD-példák, orákulum
.github/workflows/           ci.yml, deploy.yml
```

| Eszköz | Választás | Miért |
|---|---|---|
| Nyelv | TypeScript, `strict` | a felhasználó döntése |
| Build | Vite | gyors, statikus kimenet, a React alapértelmezett eszköze |
| UI | React 19 | a felhasználó döntése |
| Állapot | `useReducer` + context, egy store | kevés képernyő, kevés adat; nincs Redux |
| Stílus | sima CSS változókkal (a prototípus tokenjei), CSS Modules | nincs futásidejű CSS-könyvtár |
| Betűtípus | Atkinson Hyperlegible, **saját hosztolva** | a prototípus a Google Fonts-ról tölti; offline és adatvédelem miatt ez nem maradhat |
| Tesztek | Vitest, fast-check (tulajdonság-alapú), Playwright (néhány végponttól végpontig teszt) | |
| Csomagkezelő | pnpm | |
| Minőség | ESLint, Prettier, `tsc --noEmit` | |
| Függőségek | Dependabot, havonta | |

## 5. Az alapanyaglista (JSON)

### 5.1 Mezők

A felhasználó által kért hat mező mellé a PRD további mezőket követel meg:

| Mező | Típus | Kötelező | Honnan jön a kötelezettség |
|---|---|---|---|
| `id` | string, `kebab-case`, pl. `tojas` | igen | felhasználó |
| `name` | string, magyar név, pl. „Tojás" | igen | felhasználó |
| `kcal` | szám, 100 g-ra, nyersen | igen | felhasználó (csak ellenőrző adat, 5.3) |
| `protein` | szám, g / 100 g, nyersen | igen | felhasználó |
| `fat` | szám, g / 100 g, nyersen | igen | felhasználó |
| `carbohydrate` | szám, g / 100 g, nyersen, **rosttal együtt** (5.1) | igen | felhasználó |
| `category` | a hat kategória-azonosító egyike (5.4) | igen | PRD 4.2, 6.3: minden alapanyag pontosan egy kategóriába tartozik; a csere és az alapérték ebből jön |
| `accusative` | string, a név tárgyesete, pl. „tojást" | igen | a mondatok („137 g tojást", Folyamatok 7.) helyes magyarsága; a prototípus is tárolja |
| `defaultGrams` | szám, opcionális | nem | PRD 6.3: egyedi (gondozói) alapérték-felülírás, pl. saláta 50 g |
| `sample` | `true`, opcionális | kategóriánként pontosan egy | PRD 6.4: kategóriánként egy mintaalapanyag a rögzítés nélküli ⚠-tippekhez |
| `source` | `{ "db": "usda-fdc", "fdcId": 171287 }` | igen | PRD 4.2: a tápérték forrása az USDA FoodData Central, nyers értékek; így minden szám visszakereshető |
| `retired` | `true`, opcionális | nem | 5.2: kivezetett tétel, a választóban nem jelenik meg |

A szénhidrát **rosttal együtt** értendő, mert az orvos / dietetikus a rosttal együtti
szénhidrátot nézi (a felhasználó döntése, 2026-11-13). A listába ezért az USDA
**„Carbohydrate, by difference"** értéke kerül változatlanul (ez a rostot tartalmazza).

**Figyelem, saját alapanyag:** a magyar és az EU-s csomagolásokon a „Szénhidrát" sor
**rost nélkül** szerepel, a rost (ha fel van tüntetve) külön sorban. Ha a felhasználó a
csomagolásról a szénhidrátot írja be, a saját alapanyag kevesebb CH-t kap, mint a
valóság. Hogyan kezeljük: 15./4.

### 5.2 A fájl szerkezete és verziózása

```json
{
  "version": 3,
  "updated": "2026-11-20",
  "items": [
    {
      "id": "tojas",
      "name": "Tojás",
      "accusative": "tojást",
      "category": "protein",
      "kcal": 143,
      "protein": 12.6,
      "fat": 9.5,
      "carbohydrate": 0.7,
      "sample": true,
      "source": { "db": "usda-fdc", "fdcId": 171287 }
    }
  ]
}
```

*(A számok a formátumot mutatják; a végleges értékeket a felhasználó tölti ki.)*

Szabályok, mert a felhasználó tányérjai és „legutóbb használt" értékei **id szerint**
hivatkoznak a gondozott tételekre:

- Az `id` **soha nem változik és soha nem használható újra.** Átnevezés csak a `name`-et
  érinti.
- Tétel **nem törölhető** a fájlból, csak `retired: true` lesz. Kivezetett tétel a
  választóban és a cserében nem jelenik meg, de a már mentett tányérban működik tovább.
- Tápérték-javítás szabad: a következő számolás már az új értékkel fut, a mentett tányér
  ettől eltérhet, és a „{Név} frissítése" gomb megjelenik (PRD 6.5 erre fel van készítve).
- Minden változás növeli a `version` számot. A Keret alján kiírjuk: „Alapanyaglista:
  3. változat".

A fájl **a buildbe fordul** (`import`), nem futásidőben töltődik le: így a service worker
a többi fájllal együtt gyorsítótárazza, és nincs külön „még nem töltődött le" állapot.

### 5.3 A kcal-mező szerepe

A kcal-sáv mindenhol `4 × CH + 4 × fehérje + 9 × zsír` szerint számol (PRD 6.1), a
gondozott és a saját alapanyagnál egyaránt. A JSON `kcal` mezője ezért nem kerül a
megoldóba. Arra való, hogy az ellenőrző szkript **elírást fogjon**: ha a 4/4/9-es érték
15%-nál jobban eltér a tárolt kcal-tól, figyelmeztet (az USDA saját szorzói miatt
néhány százalék eltérés normális). Saját alapanyagnál nincs kcal-mező (PRD 6.5
változatlan).

### 5.4 Konfiguráció (`data/config.json`)

Ami nem alapanyag, de a gondozó vagy a dietetikus állítja:

- a hat kategória: `protein` (Fehérjeforrás), `grain` (Gabonaköret), `vegetable`
  (Zöldség), `fat` (Zsiradék), `fruit` (Gyümölcs), `dairy` (Tejtermék), mindegyik
  magyar névvel és alapértékkel (100 / 10 / 100 / 30 / 50 / 50 g, PRD 6.3);
- a Keret gyanús-érték küszöbei (1200 kcal, 50 g fehérje, 10%, PRD 6.1);
- a zsír–fehérje arány alapértéke (2 : 1) és a tányéronkénti maximum (8).

A PRD 6.1 szerint a küszöbök „alkalmazásfrissítés nélkül" cserélhetők. Statikus oldalon
ez **egy JSON-módosítás és egy automatikus deploy** (11.): a felhasználónak nem kell
semmit telepítenie, a következő megnyitáskor már az új érték él. Kódot nem kell hozzá
írni.

### 5.5 Ellenőrző szkript

`pnpm validate-data`, a CI is futtatja, és hibánál megállítja a buildet:

- minden kötelező mező megvan, a számok 0 és 100 között vannak;
- `carbohydrate + protein + fat ≤ 100` (ugyanaz a szabály, mint a saját alapanyagnál, PRD 7.);
- az `id`-k egyediek, `kebab-case`-ek, és **egy korábbi változatban szereplő id nem tűnt
  el** (a szkript a `main` ág fájljával veti össze);
- a kategória létezik; kategóriánként pontosan egy nem kivezetett `sample`;
- a nevek egyediek (kis- és nagybetűtől függetlenül);
- figyelmeztetés (nem hiba): kcal-eltérés 15% fölött; 40 alatti vagy 60 feletti tételszám.

## 6. A megoldó

### 6.1 Port

A prototípus `solver.js`-e (kétfázisú szimplex, Bland-szabály; sávon belüli LP, a kcal
mint puha feltétel, a ⚠ ág a 0 g-os halmazokkal, ±1 / ±2 g-os kerekítés, egyenkénti és
teljes feloldás) a végfelhasználó által elfogadott működés. Viselkedést **nem
változtatunk**, csak szerkezetet:

| Modul | Tartalom |
|---|---|
| `simplex.ts` | általános kétfázisú szimplex (`minimize c·x`, `≤ / ≥ / =` sorok) |
| `evaluate.ts` | makrók, kilógás, arány, kcal egy adott grammkiosztásra |
| `stages.ts` | sávon belüli LP (kcal-lal és nélküle), ⚠ ág a 0 g-os halmazokkal |
| `rounding.ts` | egész grammra kerekítés, ±1 majd ±2 g-os keresés |
| `solvePlate.ts` | a teljes futás: kevés alapanyag, minden rögzítve, feloldás-előzetesek |
| `compare.ts` | a 6.3-as ⚠-sorrend kulcsa (arány → kilógás → 0 g-os sorok → referencia → sorrend) |

A tippek (PRD 6.4: melyik kategóriát ajánlja, melyik sort cserélné) a prototípusban az
`app.html`-ben vannak; az alfában a `src/domain/hints.ts`-be kerülnek, mert ezek is
szabályok, nem megjelenítés.

A bemenet tiszta adat (`items`: id, makrók 100 g-ra, referencia, rögzítés; `keret`), a
kimenet is (grammok, kiértékelés, státusz, feloldás-javaslat). Nincs globális állapot.

### 6.2 Számábrázolás

`number` (64 bites lebegőpontos), a prototípus tűréseivel (`1e-9`). A kijelzés és a
✓/⚠ mindig a **kerekített egész grammokon** számol (PRD 6.3), így a lebegőpontos zaj nem
látszik. Egy tizedesre formázás `Intl.NumberFormat('hu-HU')`-val („6,6 g").

### 6.3 Teljesítmény

Legfeljebb 8 alapanyag, így a ⚠ ág legfeljebb 2⁸ = 256 kis LP-t futtat, a feloldás-előzetes
rögzítésenként még egy kört, a kategória-tipp hat próbafutást. Cél: **egy teljes
újraszámolás 50 ms alatt** egy középkategóriás telefonon. A CI egy benchmark-teszttel méri a
legrosszabb esetet (8 alapanyag, több rögzítés, ⚠). Ha a cél nem teljesül, a megoldó egy
**Web Workerbe** kerül (a tiszta bemenet–kimenet miatt ez egy burkoló, nem átírás).

### 6.4 Tesztek

1. **A PRD példái** mint rögzített tesztek: a 6.4-es ✓ példa (137 / 100 / 31 g) és a ⚠
   példa (240 g rögzített tojás → 48 g brokkoli / 39 g olaj, fehérje 6,6 g-mal fölötte), a
   Folyamatok minden számszerű példája, és a 7. peremesetek közül minden, ami számot ad.
2. **Orákulum-teszt:** a prototípus `solver.js`-e bekerül a tesztek közé, és több ezer
   véletlen tányéron (3–8 alapanyag, véletlen keret, rögzítések) a TS-port eredményének
   grammra pontosan egyeznie kell vele. Ez bizonyítja, hogy a port nem változtatott a
   végfelhasználó által elfogadott viselkedésen.
3. **Tulajdonságok** (fast-check): ✓ esetén minden makró és az arány a sávon belül van a
   kiírt grammokon; sávon belül minden szabad sor ≥ 1 g; nincs negatív gramm; a rögzített
   gramm nem változik; ugyanarra a bemenetre ugyanaz a kimenet (determinizmus, PRD 6.5);
   a sorok sorrendjének felcserélése csak a döntetlen-szabály szerint hat.
4. **Tartomány:** referencia-forrás sorrendje, „legutóbb használt" frissítése (Kész,
   mentés; 0 g nem írja felül), Keret-ellenőrzés, saját alapanyag törlése tányérokból,
   mentett tányér eltérés-vizsgálata.
5. **Végponttól végpontig** (Playwright, mobil nézet): első indítás → Keret → 3 alapanyag
   → ✓ → Kész → újranyitás; ⚠ rögzítéssel és feloldással; offline újranyitás.

## 7. Tárolás az eszközön

### 7.1 Választás: `localStorage`

Az adat kicsi: egy keret, néhány tucat saját alapanyag, néhány tucat tányér, legfeljebb
pár száz „legutóbb használt" érték, összesen jóval 100 KB alatt (a böngésző kb. 5 MB-ot
enged). A `localStorage` szinkron, egyszerű, és a prototípus is ezt használja.

Az IndexedDB (Dexie) a 2026-10-26-i tervben a szinkron-sor és a nagyobb adatmennyiség
miatt kellett. Szinkron nélkül ez az indok megszűnt. A tárolás egy saját interfész
(`src/storage/store.ts`) mögött van, így ha a fiók és a szinkron visszatér, csere, nem
átírás.

### 7.2 Séma

Egyetlen kulcs, `ketara`:

```ts
type StoredState = {
  schemaVersion: 1;
  keret: Keret | null;              // sávok, étkezésszám, arány, kcal-sáv
  customIngredients: CustomIngredient[]; // id: "u-" + crypto.randomUUID()
  plates: Plate[];                  // id, név, sorok: { ingredientId, grams }
  lastUsed: Record<string, number>; // ingredientId → gramm
  currentPlate: CurrentPlate | null;// sorok, grammok, rögzítések, honnan indult
  ui: { iosInstallHintDismissed: boolean };
};
```

- **Migrációk:** minden sémaváltozás új `schemaVersion` és egy migrációs függvény. Betöltéskor
  a régi adat sorban végigmegy rajtuk. Visszafelé nem migrálunk.
- **Írás:** minden állapotváltozás után, egyben. A visszavonható törlés (PRD 6.6) a memóriában
  vár ~5 mp-ig, és csak utána íródik ki.
- **Saját alapanyag id-ja** `u-`-val kezdődik, így soha nem ütközik gondozott id-val.
- **Hivatkozás eltűnt alapanyagra** (pl. kivezetett vagy hibásan törölt): a sor kimarad, a
  tányér a PRD 6.5 szerinti hiányos állapotban nyílik.

### 7.3 Két nyitott lap

Ha a felhasználó két fülön nyitja meg az appot, a `storage` esemény jelzi a másik fülnek a
változást, és az újratölti az állapotot. Az utolsó írás nyer; egy embernél ez elég.

### 7.4 Ha a tárolás nem elérhető

Privát böngészés vagy letiltott tárolás esetén az írás hibát dob. Az app ilyenkor működik
tovább a memóriában, és egy sávban kiírja: „Ebben a böngészőben nem tudok adatot menteni.
Bezáráskor minden elveszik." (Új szöveg, a Folyamatok 7.-be kerül.)

### 7.5 Tartós tárolás

Első indításkor az app kéri: `navigator.storage.persist()`. Androidon és asztalon ez
megvédi az adatot attól, hogy a böngésző helyhiány miatt törölje. iOS-en nem elég (8.3).

## 8. Offline és telepíthetőség (PWA)

### 8.1 Service worker

`vite-plugin-pwa` (Workbox): a build **minden fájlját** (HTML, JS, CSS, betűtípus, ikonok,
alapanyaglista) előre gyorsítótárba teszi. Az első betöltés után az app hálózat nélkül
indul és működik (PRD 6.6). Futás közben nincs hálózati kérés.

### 8.2 Frissítés

Új verziónál a service worker a háttérben letölti az új fájlokat, és **a következő
megnyitáskor** vált. Főzés közben nem frissít, nem kérdez rá, és nem tölti újra a
képernyőt. A tárolt adat a 7.2-es migrációkon megy át.

### 8.3 Telepítés és az iOS-tipp

`manifest.webmanifest`: név „Ketara", `lang: hu`, `display: standalone`, álló tájolás,
ikonok (192, 512, maskable). Így a kezdőképernyőről appként nyílik (UX 2.).

**Miért kell iOS-tipp:** az iOS Safari egy weboldal tárolt adatait **7 nap használat
nélkül törli**, ha az oldal nincs a kezdőképernyőre téve. A kezdőképernyőről nyitott
appnál ez nem történik meg. Export nincs (PRD 6.6), ezért egy egyhetes szünet után a
Keret, a tányérok és a saját alapanyagok elveszhetnének.

**Működés:** ha az eszköz iOS, a böngésző Safari, és az app **nem** a kezdőképernyőről
fut (`navigator.standalone !== true`), a Tányér tetején egy bezárható sáv jelenik meg.
Bezárás után nem jön vissza (`ui.iosInstallHintDismissed`). Androidon és asztalon nem
jelenik meg. Szöveg (a Folyamatok 7.-be kerül):

> Tedd a kezdőképernyőre, különben az iPhone egy hét szünet után törölheti az adataidat.
> Megosztás → Főképernyőhöz adás.
> [ Értem ]

## 9. Útvonalak és navigáció

A GitHub Pages nem tud minden útvonalat az `index.html`-re irányítani, ezért egy
`/ketara-app/tanyerjaim` közvetlen megnyitása 404 lenne. Megoldás: **hash-alapú
útvonalak**, a szerveren mindig csak az `index.html` van.

| Útvonal | Képernyő (UX 3.) |
|---|---|
| `#/` | Tányér (gyökér) |
| `#/tanyerjaim` | Tányérjaim |
| `#/keret` | Keret |
| `#/sajat` , `#/sajat/:id` | Saját alapanyag (új, szerkesztés) |

Az **alsó lapok** (választó, sor-műveletek, csere, mentés) nem útvonalak, de megnyitáskor
egy history-bejegyzést tesznek le, így az Android vissza-gombja a lapot zárja be, nem az
appot (UX 2.: rendszer-visszalépés). Router: `react-router` hash módban, vagy egy ~50
soros saját megoldás; az első képernyőnél döntjük el, a viselkedés ugyanaz.

A Vite `base` beállítása `/ketara-app/` (a Pages alútvonala). Saját domainnél `/` (12.).

## 10. Felület

- **Szövegek:** minden felületi szöveg egy helyen, `src/texts/hu.ts`, a Folyamatok 7.
  táblázatával egyező kulcsokkal. Nincs i18n-könyvtár (csak magyar, PRD 5.), de így egy
  későbbi fordítás egy fájl.
- **Magyar formázás:** tizedesvessző, `Intl.NumberFormat('hu-HU')`; a „(nyersen)"
  szabályai (PRD 7.) egy közös formázó függvényben.
- **Keresés a választóban:** ékezet- és kisbetű-független („tur" → „Túró"),
  `localeCompare(…, 'hu')` szerinti rendezés.
- **Számbevitel:** `inputmode="decimal"`, vessző és pont is elfogadott.
- **Akadálymentesség:** valódi gombok és címkék, a ✓/⚠ nem csak színnel jelezve, 44 px-es
  érintési célok.
- **Biztonság:** nincs külső szkript, betűtípus vagy hívás; `Content-Security-Policy`
  meta-tag `default-src 'self'`-fel.
- A vizuális irány, a tokenek és a komponensek a UX 9. és a prototípus szerint.

## 11. Build, CI és deploy

**Minden PR-on** (`ci.yml`): `pnpm install` → `tsc --noEmit` → ESLint → `validate-data` →
Vitest (benne az orákulum- és a benchmark-teszt) → `vite build` → Playwright a buildelt
appon. Ha bármelyik elbukik, a PR nem merge-elhető (branch-védelem a `main`-en).

**Merge után a `main`-re** (`deploy.yml`): ugyanaz a build, majd
`actions/upload-pages-artifact` + `actions/deploy-pages`. Azure-hasonlattal: mint egy
Static Web Apps deploy workflow, csak a GitHub saját Pages-környezetébe.

**Előnézet PR-onként:** a Pages ezt nem tudja (egy repó, egy oldal). Az alfában a PR-t a
CI tesztjei és egy helyi `pnpm preview` ellenőrzi. Ha később kell, a hosztváltással (12.)
jön.

**Verziók:** az app verziója a `package.json`-ban (`0.x` az alfában), a build beleírja a
commit rövid hash-ét. A Keret alján: „Ketara 0.3.0 (a1b2c3d) · Alapanyaglista: 3.
változat". Hibabejelentésnél ebből látszik, mi fut.

**Előfeltételek a felhasználótól:** GitHub Pro előfizetés; a repó beállításaiban
*Pages → Source: GitHub Actions*. A Pages oldal **nyilvános URL-en** érhető el akkor is,
ha a repó privát (privát Pages-oldal csak Enterprise csomagban van); a forráskód privát
marad, a böngészőbe kerülő, minifikált JS viszont bármely hosztnál olvasható.

## 12. Domain és kilépési út

**Az adat a domainhez kötött.** A `localStorage` az oldal címéhez (origin) tartozik. Ha
az app egyszer `izayata.github.io/ketara-app` címen fut, és később átköltözik
`ketara.hu`-ra, a felhasználók adatai a régi címen maradnak, az új cím üresen indul. Export
nincs, tehát nincs átvitel.

Ezért **javaslat:** a saját domain még a külső tesztelők előtt legyen beállítva a Pages-en
(a Pages ingyen kezel saját domaint és HTTPS-t). Onnantól a hoszt cseréje (pl. Azure Static
Web Apps vagy Cloudflare Pages) csak DNS-átállítás, a cím és vele az adat marad.

**Ha a termék pénzt hoz:** a GitHub Pages feltételei nem engedik, hogy főként kereskedelmi
célú oldal fusson rajta. Fizetős működés előtt a hosztot cserélni kell. Mivel a build
statikus fájlok halmaza, ez a `deploy.yml` cseréje és egy DNS-módosítás; az app kódja nem
változik. A fizetés maga (és ami hozzá kell: fiók, szerver) új PRD-döntés.

## 13. A termékdokumentumok változásai ebben a körben

- **PRD 6.6:** új szabály az iOS-tippről és a tartós tárolás kéréséről; döntésnapló-sor.
- **Folyamatok 7.:** az iOS-tipp és a „nem tudok menteni" szövege.
- **UX 2.:** az iOS-tipp helye és viselkedése.
- **`ketara-app` README:** még a félretett Cloudflare-tervet írja le; az első kód-PR
  ezzel a tervvel cseréli le.

## 14. Munkasorrend

| # | Lépés | Kész, ha |
|---|---|---|
| 0 | Repó-alap: Vite + React + TS, lint, Vitest, CI, deploy egy üres oldallal | a Pages URL-en megnyílik egy „Ketara" oldal |
| 1 | Megoldó-port + PRD-példák + orákulum + benchmark | minden teszt zöld, az orákulum egyezik |
| 2 | JSON-formátum, `config.json`, ellenőrző szkript, a prototípus 40 tétele átírva kiindulásnak | a CI ellenőrzi a fájlt; **a felhasználó** elkezdheti a végleges listát |
| 3 | Tárolás: séma, migrációk, store, több fül | tárolási tesztek zöldek |
| 4 | Keret képernyő + első indítás + Keret-ellenőrzés | Keret mentése és újranyitása működik |
| 5 | Tányér: választó, sorok, élő számolás, rögzítés, csere, ✓ | a 6.4 ✓ példa a képernyőn |
| 6 | ⚠ panel: sorrend, feloldás-előzetes, tippek, zsákutca | a 6.4 ⚠ példa a képernyőn |
| 7 | Kész, mentés, Tányérjaim, frissítés, saját alapanyag | a 2. folyamat (ismétlődő étkezés) végigmegy |
| 8 | PWA, offline, iOS-tipp, saját betűtípus, CSP | repülőgép-módban is indul |
| 9 | Alfa-teszt a felhasználóval, majd néhány tesztelővel (PRD 9.) | — |

A 2. lépéstől a végleges alapanyaglista párhuzamosan készülhet; az app a prototípus
listájával is tesztelhető, amíg az nincs kész.

## 15. Nyitott kérdések

1. ~~**Saját domain:** kell-e, és mi legyen?~~ Eldöntve 2026-11-13: egyelőre nincs (12.).
2. ~~**Nettó vagy teljes szénhidrát**~~ Eldöntve 2026-11-13: rosttal együtt (5.1).
3. ~~**Alfa-tesztelők:** kik és mikor?~~ 2026-11-13: a felhasználó megtalálta őket.
4. **Saját alapanyag szénhidrátja:** a csomagoláson a CH rost nélkül áll, a lista rosttal
   együtt számol (5.1). Kell-e a Saját alapanyag lapra egy „Rost" mező, amit az app hozzáad
   a CH-hoz? (Termékdöntés, a PRD 6.5-öt és a Folyamatok 6.2-t is érinti.)

## Döntésnapló

| Dátum | Döntés | Indok |
|---|---|---|
| 2026-11-13 | GitHub Pages, React + TypeScript, JSON alapanyaglista (id, név, kcal, fehérje, zsír, szénhidrát) | a felhasználó előre rögzített választása |
| 2026-11-13 | A `ketara-app` privát marad; Pages GitHub Pro-val | a termék később pénzt hozhat; a felhasználó választotta a Pro-t a „csak a build nyilvános" helyett |
| 2026-11-13 | kcal mindenhol 4/4/9, a JSON kcal ellenőrző adat | egy szabály a gondozott és a saját alapanyagra; a PRD 6.1 változatlan |
| 2026-11-13 | A listát a felhasználó állítja össze | a felhasználó döntése; a terv formátumot és ellenőrzést ad |
| 2026-11-13 | Egyszeri iOS-tipp a kezdőképernyőről | az iOS 7 napos törlése a legvalószínűbb adatvesztés, és export nincs |
| 2026-11-13 | Terv magyarul, kód angolul | a felhasználó jóváhagyta |
| 2026-11-13 | Szénhidrát rosttal együtt: USDA „by difference" változatlanul | az orvos a rosttal együtti CH-t nézi (a korábbi „rost nélkül" döntést aznap felülírta) |
| 2026-11-13 | Egyelőre nincs saját domain | a felhasználó döntése; költözéskor a tesztelők adatai a régi címen maradnak (12.) |
| 2026-11-13 | `localStorage`, hash-útvonalak, PWA, saját szimplex TS-ben, Vitest + orákulum | terv; a felhasználó a PR-ban felülbírálhatja |
