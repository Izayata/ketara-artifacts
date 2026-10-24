# Ketara — Folyamatok és képernyők v0.8

*Kísérőanyag a PRD v1.1-hez. Minden döntés onnan származik.*
*Utolsó frissítés: 2026-10-24*

---

## Kiindulás

Öt dolog rögzíti ezt a tervet:

1. **Az étkezési cél állandó** (napi sáv ÷ étkezésszám, minden étkezésre azonos
   zsír–fehérje aránnyal). Nincs étkezésválasztó.
2. **A megoldó a referenciatányérhoz keresi a legközelebbi megoldást** (relatív
   eltérésben); ha nincs korábbi használat, az alapanyag egyedi felülírásához, végül a
   kategória alapértékéhez.
3. **A ⚠ állapot gyakoribb lesz, mint a ✓.** Rögzítéssel különösen. Ez tehát nem
   hibaképernyő, hanem a termék fő állapota.

4. **A grammok nem ismétlődnek, csak a készlet.** A megoldó minden étkezésnél lefut.
   A **Tányér a főképernyő**; a Tányérjaim nem előzmény, hanem **kiindulópont-tár**.
   Egy mentett tányér megnyitása nem olvasás, hanem újraszámolás.

5. **Nyilvános weboldal, fiókkal** (PRD 6.6). Belépés e-mailes linkkel vagy kóddal, egyszer
   eszközönként; utána a konyhában a fiók észrevétlen: hálózat nélkül is működik, a
   szinkron a háttérben fut. A fiókbeállítások a Keret alján vannak, nem külön képernyőn.

---

## 1. Folyamat — Új tányér összeállítása

Ez a termék fő útja. Ott áll a konyhában, előtte van, ami van.

```mermaid
flowchart TD
    A[Tányér, üres] --> B[+ Alapanyag]
    B --> C{Benne van a listában?}
    C -- nem --> D[Saját alapanyag felvitele]
    D --> E
    C -- igen --> E[Alapanyag a tányéron]
    E --> F{Van legalább 3?}
    F -- nem --> G["Még egy / két alapanyag kell"] --> B
    F -- igen --> H[Megoldó fut]
    H --> I{Sávon belül?}
    I -- igen --> J["✓ adagok kiírva"]
    I -- nem --> K["⚠ melyik makró, mennyivel, merre"]
    K --> L{Van rögzítés?}
    L -- igen --> M["Feloldás felajánlva,<br/>az eredmény előzetesével"]
    M --> N{Segít a feloldás?}
    N -- igen --> H
    N -- nem --> O["Kimondja: ebből nem jön ki"]
    L -- "nem, kevés" --> O2["Megnevezi a segítő kategóriát:<br/>Kevés a zsír — Zsiradék"]
    O2 --> P2[Választó a kategórián nyílik] --> H
    L -- "nem, sok" --> O3["Megnevezi a legtöbbet adó alapanyagot:<br/>Sok a CH — cseréld a brokkolit"]
    O3 --> P3[Csere-választó azon a soron] --> H
    O --> P[Alapanyag hozzáadása vagy cseréje]
    P --> H
    J --> R{Mit választ?}
    K --> R
    R -- "Tányér mentése" --> S[Név megadása] --> T
    R -- "Kész" --> T["Kész: a grammok lesznek<br/>a legutóbb használt mennyiségek"]
```

**A `Kész` ✓ és ⚠ esetén is ott van, és nem opcionális.** Ő dönti el, mit eszik meg, nem a
szoftver. Ha elfogadja a 6,6 g túllépést, az app nem áll az útjába — csak egyszer,
világosan megmondja, mi a helyzet. A `Kész` név nélküli: tányért nem ment, csak megjegyzi a
grammokat, hogy legközelebb azokból induljon (PRD 6.5). A korábbi `Mentés így` gomb ebbe
olvadt.

---

## 2. Folyamat — Ismétlődő étkezés

```mermaid
flowchart TD
    A[Tányérjaim] --> B[Tányér kiválasztása]
    B --> C["Alapanyagok betöltve,<br/>a mentett grammok = referencia"]
    C --> D[Megoldó fut a mai célra]
    D --> E{Sávon belül?}
    E -- igen --> F["✓ adagok"]
    E -- nem --> G["⚠ + helyreállítás"]
    F --> H{Igazít a valósághoz?}
    G --> H
    H -- "rögzít: ennyi csirkét akarok" --> D
    H -- "cserél: brokkoli helyett zöldbab" --> D
    H -- nem --> I{"Eltér az összetétel vagy<br/>bármelyik gramm a tárolttól?"}
    I -- igen --> J["Frissítés vagy mentés új néven"]
    I -- nem --> K["Kész"]
    J --> K
```

**A megoldó itt is lefut.** A mentett tányér nem kész válasz — a grammok soha nem
ismétlődnek pontosan, mert a valóság minden nap más. A tárolt mennyiségek csak azt mondják
meg a megoldónak, milyen összetételt szeret. A tányér rögzítés nélkül nyílik: a tegnapi
rögzítés tegnapi döntés volt.

Ez egyben megoldja a korábbi „elavult mentett tányér" veszélyt is: mivel minden megnyitás
újraszámol, egy javított keret (elgépelés) vagy módosított tápérték automatikusan átüt a
régi tányérokon. Külön ellenőrzés nem kell.

---

## 3. Folyamat — Első indítás

```mermaid
flowchart TD
    A0[Első megnyitás] --> A1["Belépés: e-mail-cím<br/>alul süti-sáv"]
    A1 --> A2["Nézd meg a leveled"]
    A2 --> A3["Koppintás a linkre,<br/>vagy a kód beírása"]
    A3 --> A4{"Van hozzájárulás<br/>az aktuális szövegre?"}
    A4 -- nem --> A5["Hozzájárulás: figyelmeztetés +<br/>adatkezelési hozzájárulás + 16 év"]
    A5 --> A6
    A4 -- igen --> A6{Van már keret?}
    A6 -- "igen, új eszköz" --> J0["Tányér — a fiók adataival"]
    A6 -- nem --> B[Keret beállítása]
    B --> C[Napi sávok: CH, fehérje, zsír, kcal]
    C --> D[Étkezések száma]
    D --> E[Zsír–fehérje arány]
    E --> F{Lehetséges a keret?}
    F -- nem --> F2["Megnevezi a hibás sort, nem menti"] --> C
    F -- igen --> F3{Gyanús érték?}
    F3 -- igen --> G["Egyszer visszakérdez, nem javít magától"]
    G -- "igen, így van" --> H
    G -- "elgépeltem" --> C
    F3 -- nem --> H
    H["Számított étkezési cél megmutatása"]
    H --> I{Ez stimmel?}
    I -- nem --> C
    I -- igen --> J[Tányér, üres]
```

**Belépés.** Egy mező, egy gomb. Jelszó nincs. Az e-mailben egy link és egy 6 jegyű kód
jön; mindkettő egyszer használható, és lejár. A link azt a böngészőt lépteti be, amelyben
megnyílik. Ha máshol nyílt meg (laptop, a levelezőalkalmazás saját böngészője), a kódot a
várakozó képernyőn lehet beírni. Ha lejárt vagy már felhasználták, a képernyő megmondja, és
egy gombbal újat küld. Egy eszközön a belépés 1 évig megmarad, és minden használattal
megújul: a konyhában gyakorlatilag soha nem kell újra belépni.

**Hozzájárulás.** Akkor jelenik meg, ha a fiókhoz nincs rögzített hozzájárulás az aktuális
szövegverzióhoz — nem az számít, hogy új-e a fiók. Ha valaki a hozzájárulás előtt bezárja
az oldalt, a következő belépéskor újra ez jön. Ha a jogi szöveg változik, újra megjelenik.
Három dolog egy képernyőn: a figyelmeztetés („A Ketara számol, nem tanácsot ad."), a
kifejezett hozzájárulás az étrendi adatok kezeléséhez (PRD 6.6), az adatvédelmi
tájékoztató linkjével, és a nyilatkozat, hogy elmúlt 16 éves. Hozzájárulás nélkül nincs
tovább, és addig étrendi adat sehova nem kerül.

**Süti-sáv.** Az első megnyitáskor alul, a Belépés fölött. Két egyenrangú gomb:
`Elfogadom` és `Csak a szükségesek`. A választás a Keret alján bármikor módosítható. Az
analitika csak elfogadás után indul (PRD 6.8).

A `H` lépés a legfontosabb az egész beállításban: **mutasd meg neki a kiszámolt étkezési
célt, mielőtt továbbenged.** Négy éve fejből tudja, mennyi jön ki egy étkezésre. Ha a mi
számunk más, azonnal látja, és rögtön kiderül, hogy félreértettük a papírt.

---

## 4. Képernyőlista

| # | Képernyő | Egy mondatban |
|---|---|---|
| 1 | **Tányér** | Itt történik minden: alapanyagok, adagok, eredmény, egy képernyőn. |
| 2 | **Alapanyag-választó** | Alsó lap: keresés, kategóriák, legutóbbiak. |
| 3 | **Saját alapanyag** | Név, kategória, 100 g tápérték. |
| 4 | **Tányérjaim** | Kiindulópontok: mentett alapanyag-kombinációk, megnyitáskor újraszámolva. |
| 5 | **Keret** | A dietetikus számai, a belőlük számolt étkezési cél, alul a fiók. |

Öt képernyő. Ha ennél több lesz, valamit rosszul csinálunk.

A **Belépés** és a **Hozzájárulás** nem számít bele: eszközönként egyszer látszanak, a
konyhai használat része nem. A fiókbeállítások a Keret alján vannak (6.4), mert az is
ritkán nyitott képernyő.

---

## 5. Tányér — a fő képernyő állapotai

### 5.1 Üres

```
┌─ Tányér ─────────────────────────┐
│ Étkezési cél                  ⚙ │
│ CH 5–10 · Fe 20–25 · Zs 45–70   │
│ ──────────────────────────────── │
│                                  │
│   Még nincs alapanyag.           │
│   Válassz legalább hármat,       │
│   és kiszámolom az adagokat.     │
│                                  │
│                                  │
│ [ + Alapanyag                  ] │
│ [ Tányérjaim                   ] │
└──────────────────────────────────┘
```

### 5.2 Alulhatározott (1–2 alapanyag)

```
┌─ Tányér ─────────────────────────┐
│ CH 5–10 · Fe 20–25 · Zs 45–70   │
│ ──────────────────────────────── │
│ Tojás                   — g      │
│ Brokkoli                — g      │
│                                  │
│ [ + Alapanyag ]                  │
│ ──────────────────────────────── │
│ Még egy alapanyag kell a         │
│ számoláshoz.                     │
└──────────────────────────────────┘
```

A `— g` fontos: **a sorok már ott vannak, csak a szám hiányzik.** Így látszik, hogy nem
elromlott valami, hanem még nincs kész.

### 5.3 Sávon belül

```
┌─ Tányér ─────────────────────────┐
│ CH 5–10 · Fe 20–25 · Zs 45–70   │
│ ──────────────────────────────── │
│ Tojás       137 g (nyersen)   📌 │
│ Brokkoli    100 g (nyersen)   📌 │
│ Olaj         31 g (nyersen)   📌 │
│                                  │
│ [ + Alapanyag ]                  │
│ ──────────────────────────────── │
│ ✓ Minden érték a sávon belül.    │
│                                  │
│ [ Tányér mentése               ] │
│ [ Kész                         ] │
└──────────────────────────────────┘
```

Ha a Keretben van kcal-sáv, és a makrók szerinti megoldás abból kilóg, a ✓ alatt egy sor
jelenik meg: *„Kalória: 40 kcal-lal a sáv fölött."* Az állapot ✓ marad.

**Mentett tányérból indulva** a mentés helyén két gomb áll, ha az eredmény eltér a
tároltaktól:

```
│ ✓ Minden érték a sávon belül.    │
│                                  │
│ [ Csütörtöki csirkés frissítése ]│
│ [ Mentés új néven              ] │
│ [ Kész                         ] │
```

A két mentőgomb akkor jelenik meg, ha **a sorok halmaza** (csere, hozzáadás, törlés) **vagy
bármelyik kiírt gramm** eltér a tárolttól; a frissítés az összetételt is felülírja. Ha nem
tér el, nincs mentőgomb, csak a `Kész`. A horgony így csak akkor mozdul, ha ő mozdítja. Nem
sodródik el csendben, és nem is ragad be véglegesen egy elavult összetételbe.

**`Kész`** mindig ott van: „ezt eszem". Név nélküli, tányért nem ment; a kiírt grammok
lesznek az alapanyagok „legutóbb használt" mennyiségei (a 0 g-os sor kivételével). Utána rövid
visszajelzés: *„Megjegyeztem. Legközelebb ebből indulok."*

A 📌 minden soron ott van, halványan — ez a rögzítés gombja. Nem külön menüben,
nem hosszú nyomásra: **látható és egy koppintás.** Ez a funkció, amit ő maga kért.

**Soron belüli műveletek** (koppintás a néven): `Rögzítés` · `Csere` · `Törlés`.
A csere ugyanannak a kategóriának a tagjait kínálja, és **leveszi a rögzítést** —
180 g brokkolit rögzíteni nem ugyanaz, mint 180 g zöldbabot.

```
┌─ Csere: Brokkoli ────────── ✕ ──┐
│ Zöldség                          │
│  Zöldbab · Karfiol · Cukkini     │
│  Spenót · Paprika · Sárgarépa    │
│ ──────────────────────────────── │
│ [ Másik kategóriából…          ] │
└──────────────────────────────────┘
```

A **`Másik kategóriából…`** nem csere, hanem törlés + hozzáadás egy mozdulatban: a sor (a
rögzítésével együtt) kikerül, és a választó minden kategóriával megnyílik. Az új alapanyag a
saját referenciájáról indul, nem a régi grammjáról.

### 5.4 Legközelebbi elérhető (⚠) — rögzítés az ok

```
┌─ Tányér ─────────────────────────┐
│ CH 5–10 · Fe 20–25 · Zs 45–70   │
│ ──────────────────────────────── │
│ Tojás       240 g (nyersen)   📍 │
│ Brokkoli     48 g (nyersen)   📌 │
│ Olaj         39 g (nyersen)   📌 │
│                                  │
│ [ + Alapanyag ]                  │
│ ──────────────────────────────── │
│ ⚠ Fehérje: 6,6 g-mal a sáv       │
│ fölött.                          │
│                                  │
│ A rögzített 240 g tojás önmagában│
│ 30,2 g fehérjét hoz. A sáv teteje│
│ 25 g.                            │
│                                  │
│ Ha feloldod a tojást:            │
│ 137 g tojás · 100 g brokkoli ·   │
│ 31 g olaj (nyersen) → minden     │
│ érték a sávon belül.             │
│                                  │
│ [ Tojás feloldása ]  [+ Alapanyag]│
│ [ Tányér mentése               ] │
│ [ Kész                         ] │
└──────────────────────────────────┘
```

Három dolog, ami itt szándékos:

- **A 📍 (rögzített) más, mint a 📌 (rögzíthető).** Ránézésre látszik, mi fix.
- **Az előzetes megmondja, hová vezet a feloldás** — itt a sávba; ha az sem elég, azt
  mondja ki. Enélkül a felhasználó vaktában
  nyomkod, és pontosan azt csinálja, amit ma is: próbálgat 10 percig.
- **A `Kész` ott van.** Nem rejtjük el, nem kérdezünk rá kétszer.
- **A brokkoli 48 g-ra csökken.** A megoldó ⚠ esetén előbb a kilógást csökkenti: a
  brokkoli is hoz fehérjét, ezért addig megy le, amíg a CH-sáv alja engedi. Az olaj annyi,
  amennyit az arány kér.

### 5.5 Minden alapanyag rögzítve

```
│ Tojás       160 g (nyersen)   📍 │
│ Brokkoli    150 g (nyersen)   📍 │
│ Olaj         20 g (nyersen)   📍 │
│ ──────────────────────────────── │
│ Nincs mit kiszámolni — minden    │
│ adag rögzítve.                   │
│ CH 11,6 g (sáv fölött)           │
│ Fe 24,4 g ✓                      │
│ Zs 36,6 g (sáv alatt)            │
│ Arány 1,5 : 1 (minimum 2 : 1)    │
│                                  │
│ Ha feloldod az olajat:           │
│ 33 g olaj (nyersen) → CH 1,6 g-  │
│ mal a sáv fölött.                │
│                                  │
│ [ Olaj feloldása ]               │
│ [ Rögzítések feloldása         ] │
```

Ez nem hibaállapot: **kiértékelés.** „Ezt akarom enni — jó lesz?" Legitim kérdés, és a
termék válaszol rá.

**Melyik feloldást ajánlja:** a megoldó egyenként kipróbálja mindhárom feloldást. Egyik sem
hoz sávba (a tojás vagy a brokkoli feloldása a CH-t nem viszi le eléggé, vagy a zsír marad
alatta), az olajé csökkenti a legjobban a kilógást (csak a CH marad 1,6 g-mal fölötte).
Ezért kerül mellé a „Rögzítések feloldása" is — ez minden rögzítést felold, az eredményét
ugyanígy előre mutatva, és csak akkor jelenik meg, ha egyetlen feloldás sem elég. Ha valamelyik egyedül sávba hozna, csak az az egy gomb állna ott.

---

## 6. A többi képernyő

### 6.1 Alapanyag-választó (alsó lap)

```
┌─ Alapanyag ──────────────── ✕ ──┐
│ [🔍 keresés                    ] │
│ ──────────────────────────────── │
│ Legutóbb használt                │
│  Csirkemell · Tojás · Brokkoli   │
│  Cukkini · Olívaolaj · Vaj       │
│ ──────────────────────────────── │
│ Fehérjeforrás      >             │
│ Gabonaköret        >             │
│ Zöldség            >             │
│ Zsiradék           >             │
│ Gyümölcs           >             │
│ Tejtermék          >             │
│ ──────────────────────────────── │
│ [ + Saját alapanyag            ] │
└──────────────────────────────────┘
```

**Állapotok:** üres kereső (legutóbbiak + kategóriák) · találatok · nincs találat.

Nincs találat esetén: *„Nincs ilyen a listában. Felviszed sajátként?"* — és a beírt szöveg
átkerül a névmezőbe. Ne kelljen újra begépelni.

### 6.2 Saját alapanyag

```
┌─ Saját alapanyag ──────── ✕ ────┐
│ Név                              │
│ [ Anyu túrója                  ] │
│                                  │
│ Kategória                        │
│ [ Tejtermék                  ▾ ] │
│                                  │
│ 100 grammban, nyersen            │
│ Szénhidrát  [  4 ] g             │
│ Fehérje     [ 13 ] g             │
│ Zsír        [  1 ] g             │
│                                  │
│ [ Mentés                       ] │
└──────────────────────────────────┘
```

**A kategória kötelező, és ez nem adminisztráció:** ebből jön az alapérték, amihez a
megoldó horgonyoz az első használatnál. Ha nincs kategória, az első javaslat vaktában
születik.

**Ellenőrzés:** CH + fehérje + zsír ≤ 100 g. Ha nem, a szöveg: *„100 grammban ennyi nem
fér el. Nézd meg a csomagolást."* Nem „érvénytelen bevitel".

**Szerkesztés és törlés.** A saját alapanyag a választóban a „…" menüből szerkeszthető
(ugyanez a lap, kitöltve) és törölhető. Ha mentett tányér használja, törlés előtt alsó lap:
*„Az Anyu túrója ezekben a tányérokban szerepel: Reggeli túrós. Törléskor kikerül
belőlük."* — `Törlöm` / `Mégse`. Az így üressé váló tányér is törlődik.

### 6.3 Tányérjaim

```
┌─ Tányérjaim ─────────────────────┐
│ Csütörtöki csirkés               │
│ csirkemell · cukkini · vaj       │
│                                  │
│ Reggeli túrós                    │
│ túró · dió · tejszín             │
│                                  │
│ Tojásos vacsora                  │
│ tojás · brokkoli · olaj          │
│                                  │
│ [ + Új tányér                  ] │
└──────────────────────────────────┘
```

**Üres állapot — az első nap:**
*„Még nincs mentett tányérod. Számolj ki egyet, és mentsd el — legközelebb innen indulhatsz."*

Ez az üres állapot fontos: **megmondja, mi lesz a jutalom.** A termék az első héten
gyűjt, utána fizet.

**Megnyitáskor:** a tányér nem kész számokkal nyílik meg, hanem betölti az alapanyagokat,
és **újraszámol**. A mentett grammok a horgony, nem a válasz. Ha a felhasználó ezt nem
látja, azt fogja hinni, hogy elromlott valami — ezért a fejlécben egy sor:
*„{Tányér neve} — a mai célra újraszámolva."* A tányér rögzítés nélkül nyílik.

**Átnevezés és törlés** a sor „…" menüjéből. A tányérnév egyedi: foglalt névnél a
névmező alatt *„Már van ilyen nevű tányér."*, és a mentés addig nem aktív.

### 6.4 Keret

```
┌─ Keret ──────────────────────────┐
│ Napi sávok                       │
│ Szénhidrát  [ 15]–[ 30] g        │
│ Fehérje     [ 60]–[ 75] g        │
│ Zsír        [135]–[210] g        │
│ Kalória     [    ]–[    ] kcal   │
│                                  │
│ Étkezések száma      [ 3 ]       │
│ Zsír : fehérje, min. [ 2 ] : 1   │
│ ──────────────────────────────── │
│ Egy étkezésre ez jön ki:         │
│ CH 5–10 · Fe 20–25 · Zs 45–70    │
│                                  │
│ [ Mentés                       ] │
│ ──────────────────────────────── │
│ A Ketara számol, nem tanácsot ad.│
│ Nem helyettesíti a dietetikust.  │
│ ──────────────────────────────── │
│ Fiók                             │
│ anna@pelda.hu                    │
│ Minden eszközödön szinkronban.   │
│ Sütik: csak a szükségesek     >  │
│ Adatvédelmi tájékoztató       >  │
│ Adataim letöltése                │
│ Kijelentkezés                    │
│ Kijelentkezés minden eszközről   │
│ Fiók törlése                     │
│ A hozzájárulás visszavonása a    │
│ fiók törlésével történik.        │
└──────────────────────────────────┘
```

**Fiók blokk.** A Keret alatt, a Mentés gomb után, hogy a beállítás útjában ne legyen. A
szinkron állapota egy sor: „Minden eszközödön szinkronban." vagy, ha hálózat nélkül van
el nem küldött változás: „Nincs kapcsolat. A változásaid megvannak, és elküldöm őket,
amint lesz." A Tányéron a szinkron nem jelenik meg: a konyhában nem kell tudni róla.

**Adataim letöltése:** a fiók teljes tartalma egy JSON fájlban (PRD 6.6).

**Fiók törlése** a fiókhoz kötött egyetlen megerősítő kérdés visszavonás helyett, mert nem
vonható vissza: alsó lap — a 7. fejezet „Fiók törlése" szövege — `Végleg törlöm` / `Mégse`.
Utána minden eszköz kijelentkezik; egy offline eszköz a következő kapcsolódáskor törli a
helyi adatot, és nem tölt fel semmit. A hozzájárulás visszavonása ugyanez az út.

Az alsó blokk a lényeg. **Nem beviteli mező, hanem visszajelzés** — ő négy éve tudja
fejből, mennyi jön ki egy étkezésre, és itt azonnal látja, hogy jól értettük-e a papírt.

Az arány mezőben csak a zsír oldala írható, alapértéke 2. Minden szöveg ezt az értéket
írja ki.

**Lehetetlen keret:** ha egy alsó határ nagyobb a felsőnél, vagy a zsírsáv teteje kisebb,
mint {min} × a fehérjesáv alja, a termék nem menti, és megnevezi a sort: *„A zsírsáv teteje
kisebb, mint amit az arány a fehérjéhez kér. Így egyetlen tányér sem jönne ki."*

**Gyanús érték:** a termék nem javítja csendben, és nem is tiltja meg. Visszakérdez
egyszer — alacsony értéknél: *„Ez alacsonyabb, mint amit dietetikusok általában adnak.
Biztosan így van a papíron?"*; szűk sávnál: *„Ez a sáv nagyon szűk, kevés tányér fog
beleférni. Biztosan így van a papíron?"* — és ha igen, elfogadja. A küszöbök (a kcal-sáv
felső határa 1200 — csak ha van kcal-sáv —, a fehérjesáv felső határa 50 g, a sávszélesség a
felső határ 10%-a) helykitöltők, a dietetikus erősíti meg őket.
Nem a mi dolgunk felülbírálni a dietetikust; a mi dolgunk észrevenni az elgépelést.

---

### 6.5 Belépés

```
┌──────────────────────────────────┐
│ Ketara                           │
│                                  │
│ Megmondja, hány grammot vegyél   │
│ abból, ami előtted van.          │
│                                  │
│ E-mail-cím                       │
│ [ anna@pelda.hu                ] │
│                                  │
│ [ Belépési link küldése        ] │
│                                  │
│ Jelszó nincs: e-mailben küldök   │
│ egy linket és egy kódot.         │
└──────────────────────────────────┘
```

**Elküldés után** ugyanez a képernyő: „Elküldtem a linket és a kódot ide: anna@pelda.hu."
+ egy 6 jegyű kódmező („Ha a link máshol nyílt meg, írd be ide a kódot.") + `Újraküldés` ·
`Másik e-mail-cím`.

### 6.6 Hozzájárulás

```
┌──────────────────────────────────┐
│ Mielőtt elkezded                 │
│                                  │
│ A Ketara számol, nem tanácsot ad.│
│ Nem helyettesíti a dietetikust:  │
│ a számokat ő adja, te írod be.   │
│                                  │
│ ☐ Hozzájárulok, hogy a Ketara    │
│   tárolja az e-mail-címemet és az│
│   étrendi adataimat (keret, saját│
│   alapanyagok, tányérok, legutóbb│
│   használt mennyiségek).         │
│   Adatvédelmi tájékoztató >      │
│ ☐ Elmúltam 16 éves.              │
│                                  │
│ [ Tovább                       ] │
└──────────────────────────────────┘
```

A `Tovább` mindkét pipa nélkül nem aktív. A hozzájárulás szövegverzióval és időponttal
mentődik.

### 6.7 Süti-sáv

```
│ ──────────────────────────────── │
│ Sütiket használok, hogy lássam,  │
│ mi működik. Étrendi adat nem     │
│ kerül bele.                      │
│ [ Elfogadom ]                    │
│ [ Csak a szükségesek ]           │
└──────────────────────────────────┘
```

## 7. Szövegek egy helyen

| Hol | Szöveg |
|---|---|
| Tányér, üres | Még nincs alapanyag. Válassz legalább hármat, és kiszámolom az adagokat. |
| Tányér, 2 alapanyag | Még egy alapanyag kell a számoláshoz. |
| Tányér, 1 alapanyag | Még két alapanyag kell a számoláshoz. |
| ✓ | Minden érték a sávon belül. |
| ✓ + kcal | Kalória: {n} kcal-lal a sáv {fölött / alatt}. |
| ⚠ általános | {Makró}: {n,n} g-mal a sáv {fölött / alatt}. |
| ⚠ rögzítés az ok, felső él | A rögzített {n} g {alapanyag} önmagában {m,m} g {makrót} hoz. A sáv teteje {k} g. |
| ⚠ rögzítés az ok, alsó él | A rögzített {n} g {alapanyag} mellett a {makró} legfeljebb {m,m} g lehet. A sáv alja {k} g. |
| ⚠ feloldás-előzetes, sikeres | Ha feloldod a {alapanyagot}: {adagok} → minden érték a sávon belül. |
| ⚠ feloldás-előzetes, részleges | Ha feloldod a {alapanyagot}: {adagok} → {makró} {n,n} g-mal a sáv {fölött / alatt}. |
| ⚠ arány | Zsír–fehérje arány: {x,x} : 1, a minimum {min} : 1. |
| ⚠ 0 g | A {alapanyag} 0 g-ra szorult — nélküle jönne ki. Cseréld vagy vedd ki. |
| ⚠ kerekítés az ok | Egész grammokra kerekítve nem fér bele a sávba. |
| ⚠ nincs rögzítés, kevés | Kevés a {makró} — adj hozzá a {kategória} közül. |
| ⚠ nincs rögzítés, sok | Sok a {makró} — cseréld a {alapanyagot} egy másik {kategória}-félére. |
| ⚠ zsákutca | Ebből a {n} alapanyagból nem jön ki. |
| Több rögzítés, egyik feloldás sem elég | Rögzítések feloldása |
| Minden rögzítve | Nincs mit kiszámolni — minden adag rögzítve. |
| Nincs keresési találat | Nincs ilyen a listában. Felviszed sajátként? |
| Érvénytelen tápérték | 100 grammban ennyi nem fér el. Nézd meg a csomagolást. |
| Tányérjaim, üres | Még nincs mentett tányérod. Számolj ki egyet, és mentsd el — legközelebb innen indulhatsz. |
| Mentett tányér megnyitva | {Tányér neve} — a mai célra újraszámolva. |
| Frissítés gomb | {Tányér neve} frissítése |
| Frissítés után | Frissítve. Legközelebb innen indul. |
| Kész gomb | Kész |
| Kész után | Megjegyeztem. Legközelebb ebből indulok. |
| Foglalt tányérnév | Már van ilyen nevű tányér. |
| Saját alapanyag törlése, tányérban használt | Az {alapanyag} ezekben a tányérokban szerepel: {tányérok}. Törléskor kikerül belőlük. |
| Csere rögzített soron | A rögzítés lekerült, mert az alapanyag változott. |
| Keret, gyanúsan alacsony | Ez alacsonyabb, mint amit dietetikusok általában adnak. Biztosan így van a papíron? |
| Keret, gyanúsan szűk | Ez a sáv nagyon szűk, kevés tányér fog beleférni. Biztosan így van a papíron? |
| Keret, lehetetlen (alsó > felső) | Az alsó határ nagyobb a felsőnél. Nézd meg a papírt. |
| Keret, lehetetlen (arány) | A zsírsáv teteje kisebb, mint amit az arány a fehérjéhez kér. Így egyetlen tányér sem jönne ki. |
| Belépés, ígéret | Megmondja, hány grammot vegyél abból, ami előtted van. |
| Belépés, magyarázat | Jelszó nincs: e-mailben küldök egy linket és egy kódot. |
| Link elküldve | Elküldtem a linket és a kódot ide: {e-mail}. |
| Kódmező | Ha a link máshol nyílt meg, írd be ide a kódot. |
| Link vagy kód lejárt / felhasznált | Ez a link vagy kód már nem érvényes. Küldök egy újat. |
| Hibás kód | Ez a kód nem jó. Nézd meg újra a leveled. |
| Figyelmeztetés | A Ketara számol, nem tanácsot ad. Nem helyettesíti a dietetikust. |
| Hozzájárulás | Hozzájárulok, hogy a Ketara tárolja az e-mail-címemet és az étrendi adataimat (keret, saját alapanyagok, tányérok, legutóbb használt mennyiségek). |
| Korhatár | Elmúltam 16 éves. |
| Süti-sáv | Sütiket használok, hogy lássam, mi működik. Étrendi adat nem kerül bele. |
| Szinkron rendben | Minden eszközödön szinkronban. |
| Szinkron, nincs kapcsolat | Nincs kapcsolat. A változásaid megvannak, és elküldöm őket, amint lesz. |
| Fiók törlése | Minden adatod törlődik: az e-mail-címed, a keret, a saját alapanyagaid, a tányérjaid és a legutóbb használt mennyiségeid, minden eszközödön. Ez nem vonható vissza. |
| Hozzájárulás visszavonása | A hozzájárulás visszavonása a fiók törlésével történik. |
| Kijelentkezés mindenhol | Kijelentkezés minden eszközről |
| Adatletöltés | Adataim letöltése |
| Inaktív fiók, értesítő e-mail | Két éve nem léptél be a Ketarába. Ha 30 napon belül nem lépsz be, a fiókod és minden adatod törlődik. |


Minden szöveg tegező, kijelentő, és **megmondja a következő lépést.** Nincs „Hiba
történt", nincs „Érvénytelen".

---

## 8. Amit ez a terv még nem old meg

- **Napközbeni eltérés** — ha egy étkezésnél kilépett a sávból, a következőnél ezt nem
  tudja beszámítani. Tudatos: a napkövetés out of scope.
- **Az adatvédelmi tájékoztató és a figyelmeztetés végleges szövege** — a fenti mondatok
  helykitöltők, amíg szakértő át nem nézi őket (PRD 8.1/2).
