# Ketara — UX-terv v0.4

*Kísérőanyag a PRD v1.1-hez és a Folyamatok és képernyők v0.8-hoz. A viselkedést azok
rögzítik; ez a dokumentum azt írja le, **hogyan érzi és kezeli** a felhasználó.*
*Utolsó frissítés: 2026-10-24*

A v0.1 három nyitott kérdésére a felhasználó 2026-09-27-én válaszolt; a v0.3 a PRD v1.0
fiókját és nyilvános weboldalát követi (10. fejezet). A v0.4 a PRD v1.1 átnézéséből jövő
változásokat követi: `Kész` gomb a `Mentés így` helyett, egységes „(nyersen)", belépési
kód, hozzájárulás-kapu; és a PR auditját (2026-10-24): navigáció, ⚠ panel, megerősítési
szabály, fiók blokk, `Hozzáadás (3)`.
Kattintható prototípus: `ux/prototipus/ketara-prototipus.html`.

---

## 1. Tervezési elvek

1. **Konyhában, egy kézzel, fél perc alatt.** A telefon a pulton fekszik, a másik kézben
   kés vagy tojás. Nagy célterületek (legalább 48 × 48 px), a fontos gombok a hüvelykujj
   alatt, a képernyő alsó harmadában.
2. **A szám a főszereplő.** Az adag grammja a legnagyobb szöveg a képernyőn. Minden más
   (név, „nyersen", makrók) ehhez képest másodlagos.
3. **A ⚠ nem hiba, hanem munkaállapot.** Nem piros, nem riasztó. Borostyán színű, nyugodt,
   és mindig a következő lépéssel végződik.
4. **Semmi nem mozdul magától, amit nem ő mozdított — de minden mozdulat látszik.** Ha a
   megoldó átír egy grammot, a változás egy pillanatra kiemelődik, hogy lássa, mi mozdult.
5. **Nincs várakozás.** A megoldó kis lineáris programokkal dolgozik (egy tányéron
   legfeljebb 8 alapanyag): a számolás észrevétlen. Nincs pörgő ikon, nincs „Számolás…" gomb.
6. **Öt képernyő.** Minden új igény előbb egy meglévő képernyő állapotaként próbálkozik.
   (A Belépés eszközönként, a Hozzájárulás fiókonként és szövegverziónként egyszer
   látszik; nem számítanak bele.)

---

## 2. Platform és alapkeret

- **Platformfüggetlen** (a felhasználó döntése). Csak olyan minták, amelyek iOS-en és
  Androidon is természetesek: alsó lapok, numerikus billentyűzet, bal felső vissza-gomb
  (a rendszer-visszalépés mellett). Platformspecifikus minta (iOS-kapcsoló, Android FAB)
  nincs.
- **Böngészőben fut** (nyilvános weboldal, PRD 2). Telefonon teljes képernyős; a kezdőképernyőre
  tehető, így appként nyílik.
- Álló tájolás, egy oszlop. **Számítógépen ugyanez az oszlop, középre igazítva** (a
  felhasználó döntése); külön asztali elrendezés nincs. Tablet és fekvő nézet v1-ben nincs.
- Csak magyar nyelv, tegező hangnem (PRD 10: több nyelv „dísz").
- Csak világos téma (PRD 10: sötét mód „dísz"). A színek úgy készülnek, hogy később
  sötét változat is kijöhessen belőlük.
- **Fiók, de a konyhában észrevétlen.** A belépés eszközönként egyszer történik. Utána
  minden helyben fut: a megoldó és az adatok az eszközön vannak, hálózat nélkül is
  működik, a szinkron a háttérben megy. A Tányéron nincs szinkron- vagy offline-jelzés; a
  szinkron állapota csak a Keret alján, a Fiók blokkban látszik (8. fejezet).
- **Az utolsó tányér megmarad.** Ha a felhasználó bezárja az appot főzés közben, és
  újranyitja, ugyanaz a tányér várja, ugyanazokkal a rögzítésekkel. Ez az **aktuális
  tányér** csak az eszközön van, nem szinkronizál; a `Kész` vagy egy új tányér törli (PRD
  6.6). A mentett tányér ettől függetlenül rögzítés nélkül nyílik.

---

## 3. Navigáció

```
            ┌──────────── Tányér (kezdőképernyő) ────────────┐
            │                                                │
   fejléc bal: Tányérjaim ──► Tányérjaim     fejléc jobb: cél-sáv ⚙ ──► Keret
            │                                                │
   + Alapanyag ──► Alapanyag-választó (alsó lap) ──► Saját alapanyag
   sor koppintás ──► Sor-műveletek (alsó lap) ──► Csere (alsó lap)
```

- **Nincs alsó tabsor.** Két felső szintű cél van (Tányér, Tányérjaim), a Keret ritka.
  A tabsor helyet venne el a ⚠ panel elől, amely a termék fő állapota.
- A **Tányér** mindig a gyökér. A Tányérjaim és a Keret teljes képernyős lap, vissza
  gombbal; a választó, a sor-műveletek, a csere és a mentés alsó lap.
- A fejléc bal oldalán **Tányérjaim**, jobb oldalán a **cél-sáv** (CH · Fe · Zs), amely
  koppintásra a Keretet nyitja. A ⚙ ikon csak jelzés, hogy a sáv szerkeszthető.

---

## 4. Tányér — a fő képernyő

### 4.1 Zónák, fentről lefelé

| Zóna | Tartalom | Megjegyzés |
|---|---|---|
| Fejléc | Tányérjaim · (mentett tányér neve) · cél-sáv | Mentett tányérnál alatta: „{Név} — a mai célra újraszámolva." |
| Sorok | alapanyagonként egy sor | görgethető, ha sok |
| + Alapanyag | szöveges gomb a sorok alatt | |
| Makrósor | CH · Fe · Zs · arány, a tényleges értékek | minden állapotban, lásd 4.4 |
| Eredménypanel | ✓ vagy ⚠, a szöveg és a gombok | a képernyő aljához tapad |

Az eredménypanel **mindig látszik**, a sorok görögnek fölötte. ⚠ esetén a panel magasabb;
ha a sorokkal együtt nem fér ki, a panel belső része görög, a gombsor alul marad.

### 4.2 Egy sor anatómiája

```
┌──────────────────────────────────────────────┐
│ Tojás              137 g (nyersen)      ◯    │
│ fehérjeforrás                                 │
└──────────────────────────────────────────────┘
  név (koppintás → műveletek)   gramm (koppintás → beírás)   rögzítés
```

- **Gramm:** nagy, táblázatos számjegyekkel (a számok nem ugrálnak szélességben), mellette
  ugyanabban a sorban kisebben „(nyersen)": „137 g (nyersen)", a PRD 7 egyetlen formájában.
  A második sorban csak a kategória áll. A „(nyersen)" minden mennyiség mellett ott van: a
  sorokban, az előzetesben, az ok-mondatokban és a listákban is.
- **Rögzítés jelzése — a 📌 / 📍 páros helyett** (a Folyamatok vázlatai is ezt mutatják). A két gombostű-ikon kis méretben alig
  különböztethető meg, és színtévesztő szemmel sem biztos. Helyette:
  - nem rögzített: üres kör-ikon, halványan;
  - rögzített: lakat (🔒), a gramm **félkövér** és akcentusszínű, a második sorban a
    kategória mellett „rögzítve".
  Így a különbség alakban, színben és szövegben is megvan.
- **Az ikon egy koppintás:** a jelenlegi grammot rögzíti, újabb koppintás feloldja.
- **A gramm beírása rögzít.** A számra koppintva numerikus billentyűzet nyílik a soron
  belül; a beírt érték jóváhagyáskor rögzített lesz. „Ennyi tojást akarok" (PRD 9.10) így
  egy mozdulat: koppint, beír, kész.
- **Kiemelés változáskor:** ha újraszámolás után egy gramm megváltozik, a szám háttere
  egy másodpercig halványan felvillan. Nem animálunk számlálót.
- **Alulhatározott állapotban** a gramm helyén „— g" áll (Folyamatok 5.2).
- **0 g-ra szorult sor:** a gramm „0 g", borostyán színű, és az eredménypanel a 0 g
  szöveget mutatja (Folyamatok 7).

### 4.3 Sor-műveletek

- A **névre** koppintva alsó lap: `Rögzítés` / `Feloldás` · `Csere` · `Törlés`.
- **Törlés és csere után visszavonás:** 5 másodperces sáv alul — „Törölve: Brokkoli.
  Visszavonás". Ez fontosabb, mint egy megerősítő kérdés: a konyhában gyakori a
  félrekoppintás, és a visszavonás nem lassítja a szándékos mozdulatot.
- A **Csere** lap a kategória tagjait mutatja két-három oszlopos gombokként, a tányéron már
  lévő alapanyagok nélkül; alul „Másik kategóriából…". Rögzített sor cseréjekor a visszavonás-sáv
  helyett: „A rögzítés lekerült, mert az alapanyag változott." + Visszavonás.
- Húzással törlés nincs: a sor koppintási célpontjai már így is sűrűk.

### 4.4 Makrósor

**A tényleges makróértékek ✓ állapotban is látszanak** (a felhasználó döntése). A
Folyamatok 5.3-as ✓ vázlata ezt még nem mutatja; ez a terv felülírja. A felhasználó négy
éve papíron látja a számait.

Egy tömör sor minden állapotban, a panel fölött:

```
CH 8,0 · Fe 20,1 · Zs 45,1 · 2,2 : 1
```

Sávon kívüli érték borostyán színű, mellette ↑ vagy ↓. Sávjelző csík (pl. hol áll a
sávon belül) v1-ben nincs: több helyet venne el, mint amennyit hoz.

### 4.5 Eredménypanel — ✓

```
✓ Minden érték a sávon belül.
  Kalória: 40 kcal-lal a sáv fölött.        ← csak ha van kcal-sáv és kilóg
[ Tányér mentése ]                [ Kész ]
```

- Zöld pipa, de a panel háttere semleges. A ✓ a nyugalmi állapot, nem ünneplés.
- Gombok: `Tányér mentése` (másodlagos) és `Kész` (elsődleges: ez a leggyakoribb vég).
- Mentett tányérból indulva, ha az összetétel vagy bármelyik gramm eltér: `{Név} frissítése`
  és `Mentés új néven` a `Tányér mentése` helyén. Ha nem tér el: nincs mentés-gomb, csak a
  `Kész`.

### 4.6 Eredménypanel — ⚠

Olvasási sorrend, ahogy a Folyamatok 5.4 rögzíti, vizuális hierarchiával:

1. **Címsor** (félkövér): „⚠ Fehérje: 6,6 g-mal a sáv fölött." Ha több ⚠ sor van, rögzített
   sorrendben jönnek, mindegyik egyszer (PRD 6.4): arány → a kilógó makrók (grammban nagyobb
   elöl) → 0 g → kerekítés → kcal. Az első a címsor, a többi normál súlyú alatta.
2. **Ok** (normál): „Rögzítve: Tojás 240 g (nyersen). Ez önmagában 30,2 g fehérjét hoz. A sáv
   teteje 25 g."
3. **Előzetes** — keretezett doboz, hogy látszódjon: ez egy *lehetséges* tányér, nem a mostani:
   „Ha feloldod: Tojás — 137 g tojás · 100 g brokkoli · 31 g olaj (nyersen) → minden érték
   a sávon belül."
4. **Gombok:** `Tojás feloldása` (elsődleges) · `+ Alapanyag` · a mentés helye · `Kész`
   (az utóbbiak szöveges gombok, nem rejtve, de nem is versenyeznek a feloldással). A mentés
   helye ugyanúgy viselkedik, mint ✓ esetén (4.5): új tányérnál `Tányér mentése`;
   megváltozott mentett tányérnál `{Név} frissítése` és `Mentés új néven`; változatlannál
   nincs mentőgomb. A `Kész` mindig ott van.

- Az elsődleges gomb mindig az, ami a legközelebb visz a ✓-hoz: rögzítésnél a feloldás,
  rögzítés nélkül, ha egy makró kevés, a `+ {Kategória}` (a választó azon a kategórián
  nyílik), ha sok, a `{Alapanyag} cseréje` (a csere-választó azon a soron), zsákutcánál
  a `+ Alapanyag`.
- A borostyán szín csak a címsoron és a sávon kívüli makrón jelenik meg, nem a teljes
  panelen.

### 4.7 Minden rögzítve

A Folyamatok 5.5 szerint kiértékelés, ✓ vagy ⚠ címsorral. ⚠ esetén a sorok a 4.6
sorrendjében jönnek, alatta „Nincs mit kiszámolni — minden adag rögzítve.", a makrósor
(4.4), az előzetes(ek) („Ha feloldod: …", „Ha feloldod mindet: …"), a feloldó gomb(ok),
végül a mentés helye és a `Kész`, mint 4.5-ben.

### 4.8 Mentés

- `Tányér mentése` → alsó lap egy névmezővel, **kitöltött javaslattal**: az alapanyagok
  neve („Tojás, brokkoli, olaj"), kijelölve, így egy koppintással átírható vagy elfogadható.
- Mentés után rövid visszajelzés alul („Mentve: Tojásos vacsora"), és a tányér mentett
  tányérként folytatódik (fejlécben a neve).
- `Tányér mentése` ⚠ állapotban ugyanez a lap, egy sorral a mező fölött: „Fehérje: 6,6 g-mal a
  sáv fölött." Nem kérdez rá újra (PRD 6.4), csak emlékeztet. Ha van 0 g-os sor, az nem
  kerül a tányérba, és a lap kiírja: „Kimarad a tányérból, mert 0 g: Olaj."
- Foglalt névnél a mező alatt: „Már van ilyen nevű tányér.", a mentés addig nem aktív.
- `Kész` nem nyit lapot: rövid visszajelzés alul („Megjegyeztem. Legközelebb ebből
  indulok."), és a tányér marad, ahol volt, hogy mérni lehessen róla. A következő
  megnyitáskor viszont már üres tányér vár (PRD 6.6).

---

## 5. Alapanyag-választó (alsó lap)

- **Több alapanyag egy menetben.** Egy tányérhoz legalább három kell; ha minden választás
  bezárná a lapot, háromszor kellene megnyitni. Ezért a lap nyitva marad, a választott
  tételek pipát kapnak, alul `Hozzáadás (3)` (nem „Kész": az a Tányér gombja). A tányér
  közben a lap mögött már számol. Ha a tányéron 8 alapanyag van, a többi tétel halvány, és
  egy sor mondja: „Egy tányéron legfeljebb 8 alapanyag lehet."
- **A kereső nem kap automatikusan fókuszt.** A billentyűzet eltakarná a Legutóbb használt
  sort és a kategóriákat, pedig a 40–60 tételes listában a felhasználó többnyire nem keres,
  hanem választ. Egy koppintás a keresőre elég.
- Legutóbb használt: legfeljebb 8 chip, a `Kész` és a mentések alapján (PRD 6.3).
- Kategória koppintásra helyben kinyílik (nem új lap), így a vissza-lépés nem kell.
- A tányéron már lévő alapanyag halványan, pipával látszik; koppintásra nem kerül fel
  kétszer.
- Nincs találat: „Nincs ilyen a listában. Felviszed sajátként?" + gomb, a beírt szöveg
  átkerül a névmezőbe (Folyamatok 6.1).
- ⚠-ból nyitva („Kevés a zsír") a lap a Zsiradék kategóriával kinyitva nyílik, fölötte
  egy sor: „A zsírhoz ezek segítenek a legtöbbet."

---

## 6. Saját alapanyag

- Numerikus mezők tizedesvesszővel („0,7"), a telefon numerikus billentyűzetével.
- A kategória **választógombsor** (6 elem), nem legördülő: egy koppintás, és mind látszik.
  (A Folyamatok 6.2 vázlata is így mutatja.)
- Az ellenőrzés (CH + Fe + Zs ≤ 100) a mező alatt jelenik meg, amint teljesül vagy sérül,
  nem csak mentéskor.
- Mentés után az alapanyag rögtön a tányérra kerül, ha a választóból jött.

---

## 7. Tányérjaim

- Lista: név, alatta az alapanyagok. Rendezés: legutóbb használt elöl (nem ábécé: az
  ismétlődő tányérok a tetején lesznek).
- Koppintás → a Tányér megnyílik azzal, újraszámolva. Ha a jelenlegi tányéron van nem
  mentett munka, egy sor: „A mostani tányér nincs elmentve. Lecseréled?" (Lecserélem /
  Mégse).
- Hosszú nyomás vagy „…" menü: `Átnevezés` · `Törlés` (visszavonással: „Törölve: {Név}.
  Visszavonás"). A törlés a visszavonási ablak alatt csak az eszközön van meg, és csak az
  ablak bezárulása után szinkronizál (PRD 6.6).
- Üres állapot: a Folyamatok 6.3 szövege.
- `+ Új tányér`: üres Tányér (ugyanaz a figyelmeztetés, ha van nem mentett munka).

---

## 8. Belépés, Keret és első indítás

### 8.1 Belépés és hozzájárulás

- **Belépés:** a termék ígérete egy mondatban, egy e-mail-mező (e-mail-billentyűzettel,
  automatikus kitöltéssel), egy gomb. Jelszó nincs (Folyamatok 6.5).
- Elküldés után ugyanaz a képernyő mutatja, hova ment a link és a kód, egy 6 jegyű kódmezővel
  (numerikus billentyűzet, a levélből automatikus kitöltéssel, ahol a rendszer engedi),
  `Újraküldés` és `Másik e-mail-cím` gombbal. Lejárt vagy felhasznált linknél egy sor és egy
  gomb: új link.
- **Hozzájárulás** akkor, ha a fióknak nincs rögzített hozzájárulása az aktuális
  szövegverzióra (fiókonként és szövegverziónként egyszer, nem eszközönként):
  figyelmeztetés + két jelölőnégyzet (hozzájárulás, 16 év), az adatvédelmi tájékoztató
  linkjével; a `Tovább` mindkét pipa nélkül nem aktív. Alatta, elválasztva, szöveges
  gombként `Kijelentkezés` és `Fiók törlése`, hogy aki nem fogadja el, ne ragadjon be
  (Folyamatok 6.6). Meglévő fióknál az adat addig zárolva: megvan, de nem látszik.
- **Süti-sáv** az első megnyitáskor a képernyő alján. Két egyenrangú gomb, azonos
  méretben és súlyban: `Elfogadom` · `Csak a szükségesek`. Egyik sem hangsúlyosabb, és a
  sáv bezárás nélkül nem tűnik el. Amíg nincs választás, a Belépés használható mellette.
- Új eszközön, meglévő fióknál a link után (ha van érvényes hozzájárulás) rögtön a Tányér
  jön, a fiók adataival; a Keret nem kérdeződik újra.

### 8.2 Keret

- Első indításkor (új fióknál) a Keret egyetlen képernyőn nyílik, fölötte egy mondat:
  „Írd be a dietetikusod számait. Csak egyszer kell."
- Mezők párban (alsó–felső), numerikus billentyűzettel; a „Tovább" gomb a következő mezőre
  ugrik, így végig lehet menni rajta a papír sorrendjében.
- **Az „Egy étkezésre ez jön ki" blokk élőben frissül** gépelés közben, egy tizedessel
  („23,3"; egész számnál tizedes nélkül). Ez a képernyő
  lényege (Folyamatok 3, H lépés): ő fejből tudja, mennyi jön ki, és itt azonnal látja.
- Első indításnál a gomb felirata: `Ez stimmel, kezdjük`; később: `Mentés`.
- Logikai hiba: a hibás sor alatt, borostyán színnel, mentés tiltva. Gyanús érték: mentéskor
  párbeszéd a Folyamatok 6.4 szövegével, „Igen, így van" / „Javítom".
- A kcal-sáv mezői fölött: „Ha a papíron van." — hogy üresen hagyni is természetes legyen.
- A Mentés gomb alatt a figyelmeztetés egy sorban, másodlagos szövegként: „A Ketara
  számol, nem tanácsot ad. Nem helyettesíti a dietetikust."

### 8.3 Fiók (a Keret alján)

- A figyelmeztetés alatt, elválasztó után, a Folyamatok 6.4 sorrendjében: e-mail-cím ·
  szinkron állapota · Sütik (a jelenlegi választás, koppintásra a süti-sáv két gombja alsó
  lapon) · Adatvédelmi tájékoztató · Adataim letöltése · Kijelentkezés · Kijelentkezés
  minden eszközről · Fiók törlése · alatta egy sor: „A hozzájárulás visszavonása a fiók
  törlésével történik."
- Az első indításkor a Fiók blokk nem látszik: ott a Keret egyetlen dolga a beállítás.
- **Kijelentkezés** (és Kijelentkezés minden eszközről): előbb szinkronizál, utána
  megerősítés nélkül kilép; az adat a fiókban megmarad, újra be lehet lépni. Ha nincs
  kapcsolat, és van el nem küldött változás, egyszer figyelmeztet: „{N} változás még nincs
  elküldve. Ha most kilépsz, elvesznek." — `Kilépek` / `Mégse`.
- **Fiók törlése:** alsó lap a következményekkel, `Végleg törlöm` (borostyán keretes, nem
  kitöltött) és `Mégse` (elsődleges). Törlés után a Belépés képernyő jön.
- **Mikor kérdez rá a termék?** Csak ott, ahol valami nem vonható vissza vagy elveszne:
  fiók törlése, tányérban használt saját alapanyag törlése, gyanús Keret-érték, a nem
  mentett tányér lecserélése („Lecseréled?"), kijelentkezés el nem küldött változással.
  Minden más (sor vagy tányér törlése, csere) visszavonással működik.

---

## 9. Vizuális irány

- **Hangulat:** nyugodt, papírszerű, a számokra fókuszáló. Nem fitnesz-app (PRD 2: nem a
  konditeremből érkezik), nem gamifikált. Nincs konfetti, sorozat, pontszám.
- **Színek (javaslat):** meleg törtfehér háttér, sötét grafitszöveg; egy akcentus
  (mélyzöld) a rögzítésre és az elsődleges gombra; borostyán a ⚠-re. Piros sehol, mert
  semmi nem „hiba".
- **Betű:** rendszerbetű (SF Pro / Roboto), a grammokhoz táblázatos számjegyek. Méretek:
  gramm 24, név 17, másodlagos 14, makrósor 15.
- **Számformátum:** tizedesvessző („6,6 g"), gondolatjel a sávban („5–10"), szóköz a
  kettőspont körül az aránynál („2,2 : 1").
- **Akadálymentesség:** a rendszer betűméretét követi; az állapot soha nem csak színnel
  jelenik meg (ikon + szöveg is); képernyőolvasónak a sor így hangzik: „Tojás, 137 gramm,
  nyersen, rögzítve".

---

## 10. Döntések (2026-09-27 óta)

| Kérdés | Döntés |
|---|---|
| Melyik telefonon fut? | Platformfüggetlen (2. fejezet) |
| Lássa-e a makrókat ✓ állapotban is? | Igen (4.4) |
| Kell-e kattintható prototípus a teszthez? | Igen: `ux/prototipus/ketara-prototipus.html` |
| Hol vannak a fiókbeállítások? (2026-10-08) | A Keret alján, nem külön képernyőn (8.3) |
| Hogy néz ki számítógépen? (2026-10-08) | Ugyanaz az oszlop, középre igazítva (2. fejezet) |
| Kövesse-e a prototípus a v1.1-et? (2026-10-24) | Igen, a PR #4-ben frissítve (11. fejezet) |

## 11. A prototípusról

- Egyetlen HTML-fájl, telefonon a böngészőben megnyitva teljes képernyős; asztali gépen
  telefonkeretben jelenik meg.
- **Valódi megoldót futtat** (a PRD v1.1 6.3 szabályai: relatív eltérés, döntetlennél a
  korábbi sorok, sávon belül minden szabad sor ≥ 1 g, ⚠ sorrend arány → kilógás → legkevesebb
  0 g → referencia, ±1 g majd ±2 g kerekítés, a kcal a ⚠ ágban nem dönt, feloldás-előzetes),
  így a tesztelő saját kombinációkat is kipróbálhat.
- 2026-10-24-én a v1.1-re frissítve: `Kész` a `Mentés így` helyett, „sok" esetén csere,
  „(nyersen)" a sorokban, ◯/🔒 ikonok, `Hozzáadás (3)`, legfeljebb 8 alapanyag, egy tizedes
  a célban, a kiértékelés címsorral és gombokkal, „Ha feloldod mindet" előzetes, új
  ⚠-szövegek.
- Tartalmaz egy kb. 30 tételes minta-alapanyaglistát (illusztratív tápértékek, nem a
  végleges 40–60 tételes lista) és két mentett mintatányért.
- Az állapotot a böngésző helyben megőrzi; „Prototípus alaphelyzetbe" gomb a
  Tányérjaim alján állítja vissza.
- Nem része: haptika, rendszerértesítések, valódi adattárolás, és a v0.3 fiókrésze
  (Belépés, Hozzájárulás, süti-sáv, Fiók blokk). A végfelhasználó a prototípus
  viselkedését 2026-10-08-án elfogadta; a v1.1 változásait (fent) azóta nem látta.
