# Ketara — PRD v1.1

*Élő dokumentum. Helyben frissül, ahogy a döntések megszületnek.*
*Kísérőanyag: `ketara-folyamatok-kepernyok.md` — folyamatok, képernyők, szövegek.*
*Utolsó frissítés: 2026-10-24*

---

## 1. Probléma

Egy ember dietetikustól kapott, írásos szabály szerint étkezik — **ketogén étrendet**.
A papír megadja a napi
szénhidrát-, fehérje- és zsírsávot (esetenként kcal-sávot is), az étkezések számát,
valamint az étkezésenkénti zsír–fehérje arányt. A szabály **négy éve változatlan**.

A papír azonban nem mondja meg, **hány grammot vegyen abból, ami éppen előtte van.**
Ezt minden étkezés előtt ki kell számolni — ma papíron, kézzel, **étkezésenként
10–15 percben**. Négy éve, naponta többször.

A 10–15 perc nem szorzásból jön: egy szorzás húsz másodperc. Az idő nagy része
**keresés** — kombinációk próbálgatása, amíg a tányér belefér a sávba. A termék valódi
munkája tehát a keresés, nem a számolás.

## 2. Felhasználó

**Elsődleges:** felnőtt, aki négy éve követi ugyanazt a dietetikusi szabályt, otthon főz,
szűk és ismétlődő alapanyagkörből, és minden étkezés előtt papíron számol.
Nem sportoló, nem fitneszes, nem a konditeremből érkezik.

**Másodlagos:** v1-ben nincs. Szándékosan.

**Elérés:** a Ketara **nyilvános weboldal**, böngészőből használható, **ingyenes** és
**magyar nyelvű**. Bárki regisztrálhat, de a termék továbbra is a fenti elsődleges
felhasználóra készül: egy ember, a saját dietetikusi papírjával.

**Kifejezetten nem:** a makrót követő testépítő; az edző és a rábízott kliensek; aki
étrendet *kap* kész grammokkal (annak nincs mit számolni).

## 3. Ígéret

> Megmondja, hány grammot vegyél az előtted lévő alapanyagokból, hogy a dietetikusod
> sávján belül maradj — 10–15 perc keresgélés helyett fél perc alatt.

## 4. Scope — v1

1. **Keret beállítása** — egyszeri művelet: napi CH-, fehérje- és zsírsáv, opcionális
   kcal-sáv (alsó és felső határ), a **napi étkezések száma**, valamint az
   étkezésenkénti zsír–fehérje arány. A keret ritkán változik. A Keret képernyő
   szerkeszthető, de a termék nem kínálja ismétlődő feladatként.
2. **Alapanyaglista** — v1-ben kb. 40–60 kézzel gondozott alapanyag, 100 g-ra vetített
   tápértékkel, **nyers súlyra**. A tápértékek forrása az **USDA FoodData Central**
   (közkincs, nyers értékek); a neveket magyarra fordítjuk. Összetétele **keto-súlypontú**: zsiradékok, húsok,
   tojás, sajtok, alacsony szénhidráttartalmú zöldségek; a gabonaköret és a gyümölcs
   kategória is megmarad. Kevés tétel, szűk kör — ez könnyíti az összeállítást. Nem
   korlátozódik magyar alapanyagokra: a lista alapanyag szerint épül, nem konyha szerint.
   Minden sor a hat **kategória** egyikébe tartozik (6.3), és opcionálisan egyedi
   (gondozói) alapérték-felülírást kaphat. Ezt a lista gondozója állítja be, a
   felhasználó nem.
3. **Saját alapanyag felvitele, szerkesztése, törlése** — név + **kategória** + 100 g
   tápérték, nyers súlyra. A kategória kötelező: ebből jön az alapérték. A gondozott lista
   biztosan hiányos lesz.
4. **Tányér összeállítása** — legalább 3 alapanyag kiválasztása.
5. **Rögzítés** — egy vagy több alapanyag grammjának fixálása. A megoldó a maradékot
   számolja ki köré.
6. **Alapanyag cseréje** — egy sor lecserélése másikra, azonos kategórián belül
   („nincs brokkoli, van zöldbab"). Önálló mozdulat, nem törlés+hozzáadás. (A csere-
   választó „Másik kategóriából…" lehetősége viszont törlés + hozzáadás, 6.2b.)
7. **Megoldás** — a sávon belüli megoldások közül a **referenciatányérhoz legközelebbi**
   (relatív eltérésben mérve, 6.3), egész grammra kerekítve.
8. **Két kimeneti állapot** — sávon belül, illetve legközelebbi elérhető + a kilógás
   mértéke + a helyreállítás útja.
9. **Kész** — a felhasználó jelzi, hogy ezt eszi meg; a kiírt grammok lesznek a
   „legutóbb használt" mennyiségek. Név nem kell, tányért nem hoz létre (6.5).
10. **Tányér mentése** egyedi néven, a grammokkal együtt; átnevezése és törlése.
11. **Mentett tányér előhívása** és újraszámolása.
12. **Tányér frissítése** — mentett tányérból indulva a horgony felülírható az új
    összetétellel és grammokkal, egy gombbal.
13. **Fiók és szinkron** — belépés e-mailben kapott linkkel vagy kóddal, jelszó nélkül.
    A fiók tartalma (6.6) minden eszközön ugyanaz. Adatok letöltése, kijelentkezés
    minden eszközről, fióktörlés.
14. **Jogi szövegek** — egészségügyi figyelmeztetés, adatvédelmi tájékoztató, süti-
    hozzájárulás (6.7).
15. **Használati mérés** — anonim webanalitika, a süti-hozzájárulás után (6.8).

## 5. Out of scope — és miért

| Kimarad | Miért |
|---|---|
| Napi naplózás, étkezésnyilvántartás | Étkezésenként dolgozunk. A napkövetés külön termék, és ott a KalóriaBázis 150 000 tétellel és félmillió felhasználóval van jelen. |
| Vonalkód, Open Food Facts integráció | A gondozott lista lefedi a főzős életet. Az OFF adatait önkéntesek viszik fel, pontosságra nincs garancia — dietetikusi sávnál ez érzékeny. |
| Célkalkulátor (BMR/TDEE) | A számokat a dietetikus adja. Fél terméknyi munka megspórolva. |
| Edző / kliens szerepek, közös vagy megosztott fiók | Másik termék. Minden fiók egy emberé. |
| Főtt–nyers súlyátváltás, receptek | **Véglegesen kimarad**, nem csak v1-ből: minden számolás nyers alapanyaggal történik. |
| Mikrotápanyagok, rost, só, vitaminok | A papíron nincsenek. |
| Jelszavas belépés, Google- / Apple-belépés | Az e-mailes link a legkevesebb tárolt titok és a legkevesebb függés. |
| Fizetés, előfizetés | A termék ingyenes. |
| Alkalmazásbolti (iOS / Android) kiadás | Böngészőből fut, nincs mit telepíteni. |
| Más nyelvek | Indulásnál csak magyar; a szövegek később fordíthatók (10. Dísz). |

## 6. Viselkedés

### 6.1 Keret beállítása
**A felhasználó:** beírja a napi sávokat, az étkezések számát és az étkezésenkénti
zsír–fehérje arányt.
**A termék:** eltárolja, és kiszámolja az **étkezési célt**. Nem kérdezi újra.

**Szabályok:**
- Étkezési cél = napi sáv ÷ étkezések száma, mindkét határra. A kijelzés egy
  tizedesjegyű (70 ÷ 3 → „23,3"; egész számnál tizedes nélkül, „20"); a megoldó a pontos
  értékkel számol.
- Minden makróhoz alsó és felső határ tartozik — a szénhidráthoz is: a papíron a CH
  is tartományként szerepel, így az alullövés is ⚠.
- A kcal-sáv **opcionális**: a felhasználó tudja, szerepel-e a papíron. Ha megadja, **puha
  feltétel** (fehérje és CH 4 kcal/g, zsír 9 kcal/g): a megoldó a sávon belüli megoldások
  közül azt keresi, amelyik kcal-ban is belefér. Ha ilyen nincs, a makrók szerinti legjobb
  megoldást adja; az eredmény **✓ marad**, alatta egy sor kcal-megjegyzéssel. Ha nem adja
  meg, az ellenőrzés elmarad.
- Az étkezések számát a felhasználó adja meg a Keretben; a terméknek nem kell előre
  tudnia.
- Minden étkezés célja ugyanaz: a napi sáv egyenletesen oszlik el, és a zsír–fehérje
  arány is minden étkezésre azonos. Az app soha nem kérdezi, hányadik étkezés jön.
  Étkezésválasztó képernyő nincs.
- A zsír–fehérje arány minimumát ({min}) a felhasználó adja meg a Keretben; **alapértéke
  2 : 1** (a jelenlegi papíron legalább 2 g zsír / 1 g fehérje). Minden szabály és szöveg a
  megadott értékkel számol, nem beégetett 2-vel.
- Az arány **kemény alsó korlát**: sávon belüli megoldás csak az, ahol a zsír legalább a
  fehérje {min}-szerese. Több zsír megengedett, a zsírsáv tetejéig. Ha az arány nem
  teljesül, az ⚠.
- **Keret-ellenőrzés mentéskor**, két szinten:
  - *Logikai hiba* (mindig): alsó határ nagyobb a felsőnél, vagy a zsírsáv teteje kisebb,
    mint {min} × a fehérjesáv alja — ilyenkor egyetlen tányér sem lehet sávon belül. A
    termék megnevezi a hibás sort, és nem menti el.
  - *Gyanús érték* (visszakérdez, igen után elfogadja): a napi kcal-sáv felső határa 1200
    alatt (csak ha van kcal-sáv), a napi fehérjesáv felső határa 50 g alatt, vagy bármely
    sáv szélessége (felső − alsó) kisebb a felső határa 10%-ánál. **Ezek
    helykitöltő számok**, konfigurációs értékként tárolva; a dietetikus megerősíti vagy
    lecseréli őket az indulás előtt, alkalmazásfrissítés nélkül.

### 6.2 Tányér összeállítása
**A felhasználó:** kiválaszt legalább 3 alapanyagot. Bármelyikhez megadhat rögzített
grammot.
**A termék:** minden változtatás után újraszámol.
**Szabály:** 3 alapanyag alatt nem számolunk — három makrósávhoz jellemzően három
mozgó mennyiség kell. A rögzített alapanyagok mégis beleszámítanak a háromba (a
felhasználó döntése, 2026-09-27): ha a rögzítések miatt kevés mennyiség mozdulhat, és
így nincs sávon belüli tányér, azt a ⚠ kezeli.
Ha minden alapanyag rögzített, a termék nem old meg, csak kiértékel: megmondja, hol tart
a sávokhoz képest. A kiértékelés is ✓ vagy ⚠ címsort kap, és ugyanúgy ott van alatta a
Kész és a mentés helye (6.5).
**Felső korlát:** egy tányéron legfeljebb **8 alapanyag** lehet. A választó a nyolcadik
után nem enged többet: *„Egy tányéron legfeljebb 8 alapanyag lehet."*

### 6.2b Alapanyag cseréje
**A felhasználó:** egy soron a cserét választja.
**A termék:** felkínálja ugyanannak a kategóriának a tagjait (a tányéron már szereplő
alapanyagok nélkül), majd helyben lecseréli és újraszámol.
**Szabály:** a csere **leveszi a rögzítést** az adott sorról. 180 g brokkolit rögzíteni nem
ugyanaz, mint 180 g zöldbabot. Az új alapanyag referenciája a saját legutóbbi használata,
ennek hiányában az egyedi felülírása, végül a kategória alapértéke (6.3).

**Másik kategóriából…** A csere-választó alján. Ez nem csere: a sor törlődik (a
rögzítésével együtt), és a választó minden kategóriával megnyílik. Az új alapanyag a saját
referenciájáról indul, nem a régi grammjáról.

### 6.3 A megoldó választási szabálya
**A szabály:** a sávon belüli megoldások közül azt választja, amelyik a
**referenciatányérhoz** legközelebb esik.

**A távolság mértéke: relatív eltérés.** Minden nem rögzített alapanyagra
|adag − referencia| ÷ referencia, és ezek **összege** (nem négyzetösszege). Így 3 g
változás 30 g olajon ugyanannyit számít, mint 20 g változás 200 g tojáson, és jellemzően
csak egy-két alapanyag mozdul, a többi a megszokott mennyiségén marad. Ez lineáris
program: gyors, pontos, és ugyanarra a bemenetre mindig ugyanazt adja.

**Döntetlen:** ha több megoldás egyformán közel van, az a győztes, amelyikben a **korábban
felvett** sorok maradnak közelebb a referenciához (a tányér sorrendje szerint, az első sor
előre). Így az eredmény nem a számolás belső sorrendjén múlik, és a mentett tányér nem tér
el ok nélkül.

**Megengedett mennyiségek.** Sávon belüli megoldásnak csak az számít, ahol minden nem
rögzített alapanyag **legalább 1 g**. 0 g csak a ⚠ ágban jelenhet meg, ha másképp nem
közelíthető a sáv. A ⚠ ágban is minden nem rögzített alapanyag **vagy 0 g, vagy legalább
1 g**. Ehhez a megoldó a 0 g-ra tett sorok halmazait a legkevesebbtől kezdve sorra veszi,
halmazonként egy **kis lineáris programmal** (legfeljebb 8 sornál ez is észrevétlen).

**Ha nincs sávon belüli megoldás** (⚠), a megoldó ebben a sorrendben dönt:
1. a zsír–fehérje arány minimuma — kemény feltétel, amíg teljesíthető; ha nem
   teljesíthető, a **hiányzó zsír grammban** ({min} × fehérje − zsír) a lehető legkisebb;
2. a sávokból való **teljes kilógás grammban** a lehető legkisebb (CH, fehérje, zsír
   kilógásának összege);
3. a lehető **legkevesebb 0 g-ra szoruló** alapanyag;
4. ezek közül a referenciához legközelebbi (relatív eltérés, mint fent).

A kcal-sáv a ⚠ ágban nem dönt: ha a kcal is kilóg, az csak a megjegyzés-sorban jelenik
meg (6.1).

A referencia forrása, ebben a sorrendben:
1. ha mentett tányérból indult: a **tányérban tárolt** mennyiség — a tányér egy megőrzött
   **összetétel** (mely alapanyagok, hány grammal), és ez az egyetlen dolog, amit hordoz.
   A referencia a tárolt **abszolút** gramm: ha a felhasználó egy alapanyagot a tárolttól
   eltérő grammra rögzít, a többi alapanyag referenciája nem skálázódik vele. Tárolt gramm
   soha nem 0, mert 0 g-os sor nem kerül tányérba (6.5);
2. egyébként amennyit a felhasználó **legutóbb** használt ebből az alapanyagból. A
   „legutóbb használt" a **Kész**, a Tányér mentése, a {Név} frissítése és a Mentés új
   néven gombra frissül; a sima újraszámolás nem írja felül, így a referencia nem sodródik.
   0 g-os sor nem írja felül a „legutóbb használt" értéket;
3. ha nincs ilyen, az alapanyag **egyedi felülírása**, végül a **kategória alapértéke**.

**Kategóriák** (zárt lista, hat darab; minden alapanyag pontosan egybe tartozik, és a csere
ezen belül működik): fehérjeforrás (hús, hal, tojás) · gabonaköret · zöldség · zsiradék
(olajok, vaj, zsír, olajos magvak, diófélék) · gyümölcs · tejtermék (sajtok, túró, tejföl,
tejszín).

**Kategória-alapértékek** (kiinduló számok a keto sávhoz, a lista összeállításakor
véglegesítendők): fehérjeforrás 100 g · gabonaköret 10 g nyersen · zöldség 100 g ·
zsiradék 30 g · gyümölcs 50 g · tejtermék 50 g. Ahol a kategória alapértéke nyilvánvalóan
rossz (saláta 100 g helyett 50 g), az alapanyag **egyedi felülírást** kap.

Az alapérték csak horgony: az első Kész vagy mentés után a felhasználó saját mennyisége
veszi át a helyét. Ezért elég, ha irányban jó.

**Kerekítés:** a megoldó törtgrammal számol, a kiírás egész gramm. A **kiírt, kerekített
tányér** a mérvadó: erre számolja a makrókat és a ✓/⚠ állapotot. Ha a kerekítés kiléptetne
egy sávból vagy az arányból, a megoldó a szomszédos egész grammok (±1 g) kombinációi közül
a legjobbat választja, ugyanazzal a sorrenddel, mint fent. Ha ezek között nincs sávon
belüli, ±2 g-ig keres; ha így sincs, az eredmény ⚠ a fenti sorrend szerint, és a szöveg a
kerekítést nevezi meg okként. A makróértékek és a kilógás **egy
tizedesjeggyel** jelennek meg („6,6 g"), az arány is („2,2 : 1"). Kilógás soha nem
látszik „0,0 g"-nak: a legkisebb kiírt kilógás 0,1 g.

### 6.4 A két kimenet

Példa (illusztratív számok, nem a valós keret). Keret egy étkezésre:
**CH 5–10 g · fehérje 20–25 g · zsír 45–70 g · zsír ≥ 2 × fehérje.**
Tápértékek 100 g-ban, nyersen: tojás CH 0,7 · Fe 12,6 · Zs 10 · brokkoli CH 7 · Fe 2,8 ·
Zs 0,4 · olaj Zs 100. Alapanyagok: tojás (fehérjeforrás), brokkoli (zöldség), olaj
(zsiradék). Nincs előzmény, a referencia a kategória-alapérték: **100 g tojás · 100 g
brokkoli · 30 g olaj**.

**Sávon belül** (CH 8,0 · Fe 20,1 · Zs 45,1 · arány 2,2 : 1). Csak a tojás mozdul
érdemben (+37 %), a brokkoli marad, az olaj +1 g:
```
✓  137 g tojás · 100 g brokkoli · 31 g olaj (nyersen)
   Minden érték a sávon belül.
   [ Tányér mentése ]         [ Kész ]
```

**Legközelebbi elérhető** — a felhasználó rögzítette a tojást 240 g-ra. Eredmény:
CH 5,0 · Fe 31,6 · Zs 63,2 · arány 2,0 : 1. A megoldó először a kilógást csökkenti: a
brokkolit addig viszi le, amíg a CH-sáv alja engedi (48 g), az olajat annyira emeli, amennyit
az arány kér (39 g):
```
⚠  240 g tojás 🔒 · 48 g brokkoli · 39 g olaj (nyersen)
   Fehérje: 6,6 g-mal a sáv fölött.
   Rögzítve: Tojás 240 g (nyersen). Ez önmagában 30,2 g fehérjét hoz; a sáv teteje 25 g.

   Ha feloldod: Tojás
   137 g tojás · 100 g brokkoli · 31 g olaj (nyersen) → minden érték a sávon belül.

   [ Tojás feloldása ]        [ + Alapanyag ]
   [ Tányér mentése ]         [ Kész ]
```

**Szabályok a ⚠ állapotra:**
- Mindig megnevezi, **melyik makró, mennyivel, melyik irányba** csúszik ki. Ha az arány
  sérül, azt is: „Zsír–fehérje arány: 1,5 : 1, a minimum {min} : 1."
- **A ⚠ sorainak rögzített sorrendje**, mindegyik legfeljebb egyszer: arány → a kilógó
  makrók, a grammban nagyobb eltérés elöl → 0 g-ra szorult sor → kerekítés → kcal. Az első
  sor a címsor. Utána jön az ok (pl. a rögzítés), az előzetes, végül a gombok.
- Ha a kilógás oka egy rögzítés, ezt kimondja. A felhasználó különben a rossz elemet
  próbálja javítani.
- A feloldó gomb **megnevezi az alapanyagot** („Tojás feloldása"), nem generikus.
- **Több rögzítésnél** a megoldó egyenként kipróbálja minden rögzítés feloldását (rögzítésenként
  egy kis futás). Azt az egyet ajánlja fel, amelyiknek a feloldása sávba hoz; ha több is,
  amelyik a referenciához közelebbi tányért ad; ha egyik sem, amelyiknek az eredménye a 6.3
  ⚠-sorrendje szerint a legjobb (előbb az arány, aztán a kilógás grammban). A „Rögzítések feloldása" gomb csak akkor jelenik meg mellette, ha **egyetlen
  feloldás sem** hoz sávba. Ez **minden** rögzítést felold, és az eredményét ugyanígy előre
  mutatja.
- A feloldás **eredményét előre mutatja**, kattintás előtt — akkor is, ha a feloldás sem
  oldja meg. A termék akkor jó, ha megmondja, melyik lépés vezet valahova.
- A feloldás nem törli az alapanyagot: csak visszaadja a grammját a megoldónak.
- Ha nincs rögzítés, a feloldó gomb nem létezik. Helyette a helyreállítás iránya számít,
  ebben a sorrendben:
  - ha az **arány** sérül: zsiradék hozzáadását javasolja (a választó a Zsiradékon nyílik);
  - különben a **grammban legnagyobb eltérés** dönti el az irányt, akkor is, ha egy másik
    makró az ellenkező irányba csúszik ki;
  - ha az a makró **kevés**: a termék azt a kategóriát nevezi meg, amelynek **mintaalapanyaga**
    hozzáadva (próbafuttatásban) a legjobban csökkenti a kilógást, és a hozzáadás gomb azon
    a kategórián nyitja meg a választót. Kategóriánként egy mintaalapanyag van, a gondozó
    jelöli ki; így hat próbafutás elég, a teljes listát nem kell végigpróbálni;
  - ha az a makró **sok**: azt a nem rögzített alapanyagot nevezi meg, amely a legtöbbet
    adja belőle, és kategórián belüli cserét javasol, a csere-választót azon a soron nyitva;
  - ha egyik kategória mintaalapanyaga sem csökkenti a kilógást: a zsákutca szövege (7.).
- A **Kész** mindig ott van, ✓ és ⚠ esetén is: a felhasználó dönti el, mit eszik meg. A
  termék egyszer, világosan megmondja a kilógást, és nem kérdez rá kétszer. (A korábbi
  „Mentés így" gomb a Kész-be olvadt.)
- Ha a ⚠ oka a kerekítés (6.3), a szöveg ezt mondja ki.

### 6.5 Mentés
**Saját alapanyag:** név + kategória + 100 g tápérték, nyers súlyra. Bekerül a listába,
kereshető. Szerkeszthető (név, kategória, tápérték) és törölhető. Ha a törölt alapanyag
mentett tányérokban szerepel, a megerősítő kérdés felsorolja őket; törléskor a sor kikerül
belőlük. Az így üressé váló tányér törlődik; a 3-nál kevesebb sorral maradó tányér megmarad,
és hiányos állapotban nyílik (a hiányzó alapanyagokat kéri).

**Kész:** név nélküli mozdulat, ✓ és ⚠ esetén is: a felhasználó ezt eszi meg. A kiírt
grammok lesznek az alapanyagok „legutóbb használt" mennyiségei (a 0 g-os sor kivételével).
Tányért nem hoz létre és nem módosít.

**Tányér:** egyedi név + az alapanyagok a kiszámolt grammokkal. Foglalt névnél a termék
szól („Már van ilyen nevű tányér."). **0 g-os sor nem kerül a tányérba**; a mentés lapja ezt
kiírja: *„Kimarad a tányérból, mert 0 g: {Alapanyag}."* A tárolt grammok **nem kész válasz, hanem horgony**: a
grammok minden étkezésnél újra kiszámolódnak, mert a valóság (mennyi hús van itthon, van-e
brokkoli) minden alkalommal más. A tányér azt őrzi meg, milyen **összetételben** szereti
ezeket együtt. A rögzítéseket **nem** tárolja: a rögzítés aznapi, ízlés szerinti választás
(9.10), ezért a tányér mindig rögzítés nélkül nyílik. A tányér átnevezhető és törölhető.

**A mentés helye** a kimenet alatt: új tányérnál *„Tányér mentése"*; mentett tányérból
indulva, ha eltér a tárolttól, *„{Név} frissítése"* és *„Mentés új néven"*; ha nem tér el,
nincs mentőgomb. A **Kész** mindig ott van mellette.

**Tányér frissítése:** ha mentett tányérból indult, és **a sorok halmaza** (csere,
hozzáadás, törlés) **vagy bármelyik kiírt (egész) gramm** eltér a tárolttól, a mentés helyén
két lehetőség áll: *„{Név} frissítése"* és *„Mentés új néven"*. A frissítés az összetételt
is felülírja. Tűréshatár nem kell: a megoldó determinisztikus, ugyanarra a bemenetre ugyanazt
adja, így eltérés csak valódi okból lehet (javított keret, módosított tápérték, rögzítés,
csere). A horgony így csak szándékosan mozdul el — nem sodródik csendben, és nem is ragad be
véglegesen.

### 6.6 Fiók és szinkron
**A felhasználó:** megadja az e-mail-címét, és a kapott linkre koppint, vagy beírja a
kapott kódot.
**A termék:** belépteti; a fiók adatai minden eszközén megjelennek.

**Szabályok:**
- **Belépés nélkül nem használható.** Az első indítás sorrendje: belépés → hozzájárulás →
  Keret → üres Tányér (a `+ Alapanyag` és a `Tányérjaim` gombbal).
- A belépési e-mail egy **linket** és egy **6 jegyű kódot** tartalmaz. A link azt a
  böngészőt lépteti be, amelyben megnyitják. Ha máshol nyílt meg (másik eszköz, a
  levelezőalkalmazás beépített böngészője), a kódot a kérő eszköz várakozó képernyőjén
  lehet beírni. A link és a kód **egy belépési token**: bármelyik felhasználása mindkettőt
  érvényteleníti. **15 percig** érvényes; **5 hibás kód** után érvénytelen, új kell.
- Egy eszközön a belépés **1 évig** megmarad, és minden használattal megújul.
- **A fiók tartalma** (kanonikus lista; minden szöveg — hozzájárulás, adatvédelmi
  tájékoztató, fióktörlés, szószedet — ezt használja): az e-mail-cím, a keret, a saját
  alapanyagok, a mentett tányérok, a „legutóbb használt" mennyiségek és a hozzájárulás
  (szövegverzió, időpont). A gondozott alapanyaglista közös, nem a fiók része.
- **Csak az eszközön:** az **aktuális tányér** (a sorai, grammjai és rögzítései). Nem
  szinkronizál: ha a felhasználó főzés közben bezárja az appot, újranyitáskor ugyanaz a
  tányér várja, a rögzítésekkel együtt. A **Kész** vagy egy új tányér törli (a Kész után a
  tányér a képernyőn marad, hogy mérni lehessen róla, de a következő megnyitás már üres
  tányérral indul). A letöltésben szerepel, és a fiókkal együtt törlődik.
- **Hálózat nélkül is működik**: a megoldó és az adatok az eszközön vannak, a változás a
  következő kapcsolatnál szinkronizál. A konyhában ez nem luxus.
- **Szinkron-tétel** a keret, egy saját alapanyag, egy tányér, illetve egy alapanyag
  „legutóbb használt" értéke. Ha ugyanazt a tételt két eszközön is módosították, a változás
  **eszközoldali időpontja** szerint a későbbi marad (egyezésnél az, amelyik előbb ért a
  szerverre). A **törlés mindig győz**: a törölt tételt egy másik eszközön végzett módosítás
  sem hozza vissza. Ha egy tányér sora törölt alapanyagra mutat, összefésüléskor a sor
  kimarad (mint a 6.5 törlésénél).
- **Visszavonható törlés** (tányér, sor): a törlés a visszavonási ablak (~5 mp) alatt csak
  az eszközön van meg, és csak az ablak bezárulása után szinkronizál.
- **Kijelentkezés:** előbb szinkronizál. Ha nincs kapcsolat, és van el nem küldött
  változás, egyszer figyelmeztet: *„{N} változás még nincs elküldve. Ha most kilépsz,
  elvesznek."* — `Kilépek` / `Mégse`. A **Kijelentkezés minden eszközről** (a Fiók
  blokkban) ugyanígy viselkedik az aktuális eszközön.
- **Adataim letöltése**: a fiók teljes tartalma egy JSON fájlban (GDPR: hozzáférés és
  adathordozhatóság).
- A felhasználó **törölheti a fiókját**; ekkor minden adata törlődik a szerverről, és
  minden eszközén kijelentkezik. Egy offline eszköz a következő kapcsolódáskor **nem tölt
  fel semmit**: törli a helyi adatot, és a belépésre áll vissza.
- Az étrendi adat egészségügyi adatnak minősülhet (GDPR), ezért a felhasználó
  **kifejezetten hozzájárul** a kezeléséhez, és nyilatkozik, hogy **elmúlt 16 éves**. A
  hozzájárulás-képernyő akkor jelenik meg, ha a fiókhoz **nincs rögzített hozzájárulás az
  aktuális szövegverzióhoz** — nem csak új fióknál. A hozzájárulás **fiókonként és
  szövegverziónként** egyszer kell, nem eszközönként. Ha a jogi szöveg változik, újra
  megjelenik. Új fióknál hozzájárulás előtt étrendi adat nem keletkezik. Meglévő fióknál
  (új szövegverzió) az adat hozzájárulásig **zárolva**: megmarad, de nem látszik és nem
  szinkronizál. A hozzájárulás-képernyőn ott a **Kijelentkezés** és a **Fiók törlése** is,
  hogy aki nem fogadja el, ne ragadjon be.
- **A hozzájárulás visszavonása a fiók törlése.** A Fiók blokk ezt egy mondatban kimondja;
  külön visszavonási út nincs.
- **Megőrzés:** 24 hónap **használat nélkül** (használat: az app megnyitása belépve vagy
  egy szinkron; a belépés 1 évig megmarad, ezért a belépés nem jó mérce) → e-mailes
  értesítés (*„Két éve nem használtad a Ketarát."*) → ha 30 napon belül nem használja, a
  fiók törlődik, ugyanúgy, mint kézi törlésnél.

### 6.7 Jogi szövegek
- **Egészségügyi figyelmeztetés:** a Ketara számol, nem tanácsot ad; nem helyettesíti a
  dietetikust. A Hozzájárulás képernyőn (hosszú változat) és a Keret képernyőn (rövid
  változat) látható.
- **Adatvédelmi tájékoztató:** mit tárolunk (a 6.6 listája), hol, meddig (24 hónap
  használat nélkül + 30 nap), hogyan tölthető le és törölhető, és hogy az anonim analitika
  (6.8) a fióktörlés hatályán kívül esik.
- **Süti-hozzájárulás:** az analitika csak elfogadás után indul (6.8).

### 6.8 Használati mérés
- **Anonim** webanalitika (pl. Google Analytics), **csak süti-hozzájárulás után**.
  Elutasítás esetén a termék ugyanúgy működik.
- Az analitika **nem kap felhasználói azonosítót**, ezért a fióktörlés nem érinti; ezt az
  adatvédelmi tájékoztató kimondja.
- Az analitikába **nem kerül** étrendi adat: sem a keret, sem a grammok, sem az
  alapanyagok tápértéke. Az URL-ek és az oldalcímek **nem tartalmaznak felhasználói
  szöveget** (tányér- vagy alapanyagnevet). Csak az alábbi események és időtartamok.
- A mért események a feltételezések tesztjei (9.), és csak ezek: a Keret kitöltésének
  megkezdése és befejezése (9.1), tányér összeállításának ideje (9.5), „Saját alapanyag"
  felvitele (9.9), tányér mentése és mentett tányér újrahasználata (9.11).

## 7. Peremesetek

| Eset | Mit tesz a termék |
|---|---|
| 3-nál kevesebb alapanyag | Nem számol. Megmondja, hány alapanyag hiányzik még (egy vagy kettő), nem hibaüzenettel. |
| Nincs sávon belüli megoldás | Legközelebbi elérhető + melyik makró mennyivel, melyik irányba csúszik ki + a helyreállítás útja. |
| Nincs sávon belüli megoldás, nincs rögzítés, egy makró kevés | A legtöbbet segítő **kategóriát** nevezi meg („Kevés a zsír — adj hozzá a Zsiradék közül."), és a hozzáadás gomb azon a kategórián nyitja meg a választót (6.4). |
| Nincs sávon belüli megoldás, nincs rögzítés, egy makró sok | A kilógó makróból legtöbbet adó alapanyagot nevezi meg, és kategórián belüli cserét javasol („Sok a szénhidrát — Brokkoli cseréje segít."). |
| Nincs sávon belüli megoldás, nincs rögzítés, az arány sérül, vagy egy makró kevés és egy másik sok | Előbb az arány (zsiradék hozzáadása), különben a grammban nagyobb eltérés dönti el az irányt (6.4). |
| A zsír kevesebb, mint a fehérje {min}-szerese | ⚠: megnevezi az arányt és a minimumot („Zsír–fehérje arány: 1,5 : 1, a minimum {min} : 1"). |
| A rögzített mennyiség önmagában kilépteti a sávból | Külön üzenet: a rögzítés a hibás elem. Felajánlja a feloldást, az eredmény előzetesével. |
| A feloldás sem elég | Kimondja, hogy ebből a készletből nem jön ki. Nem küldi a felhasználót vakon próbálgatni. |
| A megoldás negatív mennyiséget adna | Nem mutatunk mínuszt. Sávon belüli megoldásban minden nem rögzített alapanyag legalább 1 g. 0 g csak akkor jelenik meg, ha másképp nem közelíthető a sáv; az ⚠ akkor is, ha minden makró a sávon belül van (6.3): „0 g-ra szorult: {Alapanyag}. Ebben a tányérban nem fér el — cseréld vagy vedd ki." |
| A kerekítés kiléptet, ±2 g sem segít | ⚠, a szöveg a kerekítést nevezi meg okként (6.3). |
| Minden alapanyag rögzítve | Nem old meg, kiértékel: hol tart a sávokhoz képest, ✓/⚠ címsorral, a Kész és a mentés helye alatta. |
| A 9. alapanyag | Nem kerül fel: „Egy tányéron legfeljebb 8 alapanyag lehet." |
| Saját alapanyag értelmetlen tápértékkel | Ellenőrzés: 100 g-ban a CH+fehérje+zsír nem haladhatja meg a 100 g-ot. |
| Lehetetlen keret (alsó > felső, vagy a zsírsáv teteje < {min} × fehérjesáv alja) | Nem menti. Megnevezi a hibás sort. |
| Gyanúsan alacsony vagy szűk keret (6.1 helykitöltő küszöbei) | A termék nem hajtja végre szó nélkül. Egyszer visszakérdez; igen után elfogadja. |
| A makrók sávon belül, a kcal nem | ✓ marad, alatta egy sor: „Kalória: {n} kcal-lal a sáv {fölött / alatt}." A megoldó előbb kcal-ban is megfelelő megoldást keres (6.1). |
| Első indítás | Belépés e-mailes linkkel vagy kóddal → hozzájárulás → Keret → üres Tányér, a `+ Alapanyag` és a `Tányérjaim` gombbal. Nincs tányér, nincs előzmény. |
| Nincs hálózat | Minden helyben működik; a változás a következő kapcsolatnál szinkronizál. |
| Lejárt vagy már használt belépési link | Megmondja, és egy gombbal újat küld. |
| A belépési link másik eszközön vagy beépített böngészőben nyílt meg | A link azt a böngészőt lépteti be; a kérő eszközön az e-mailben kapott 6 jegyű kód beírható. |
| Belépett, de a hozzájárulás előtt bezárta az oldalt | A következő megnyitáskor újra a hozzájárulás jön; addig nincs tárolt étrendi adat. |
| Új jogi szövegverzió, meglévő fiók | A hozzájárulás-képernyő jön; az adat addig zárolva (megvan, de nem látszik, nem szinkronizál). A képernyőn Kijelentkezés és Fiók törlése is van. |
| Kijelentkezés el nem küldött változással, hálózat nélkül | Egyszer figyelmeztet: „{N} változás még nincs elküldve. Ha most kilépsz, elvesznek." — Kilépek / Mégse. |
| Tányérsor törölt alapanyagra mutat (másik eszközön törölték) | Összefésüléskor a sor kimarad (6.6). |
| Ugyanaz a tétel két eszközön módosítva | A változás eszközoldali időpontja szerint a későbbi marad; törlés és módosítás ütközésekor a törlés (6.6). |
| Fióktörlés, miközben egy másik eszköz offline | Kapcsolódáskor az eszköz nem tölt fel semmit, törli a helyi adatot, és a belépésre áll vissza. |
| Saját alapanyag törlése, amit mentett tányér használ | A megerősítés felsorolja a tányérokat; törléskor a sor kikerül belőlük. Az üressé váló tányér törlődik; a 3-nál kevesebb sorú megmarad, és hiányos állapotban nyílik. |
| ⚠ tányér mentése 0 g-os sorral | A 0 g-os sor nem kerül a tányérba; a mentés lapja kiírja (6.5). |
| Foglalt tányérnév | „Már van ilyen nevű tányér." Nem menti, amíg más nevet nem kap. |
| Új eszköz, első belépés | Letölti a fiók adatait; a Keret nem kérdeződik újra. |
| Süti-hozzájárulás elutasítva | Minden működik, csak a mérés marad el. |
| A horgony elavult | A felhasználó frissítheti a tányért egy gombbal. A termék nem dönti el helyette, hogy melyik összetétel az érvényes. |
| Csere rögzített soron | A rögzítés lekerül, a termék ezt kiírja: „A rögzítés lekerült, mert az alapanyag változott." |
| Nyers–főtt félreértés | **Minden** kiírt mennyiség mellett ott áll a „(nyersen)", egyetlen formában: sorban „Csirkemell 90 g (nyersen)", mondatban és felsorolásban „90 g csirkemell (nyersen)", egy felsorolás végén egyszer. Az ok-mondatokban is („Rögzítve: Tojás 240 g (nyersen)."). Alapanyagonkénti „főzve változik" jelölés nincs: minden számolás nyers súllyal történik. A nyers súly héj, csont és tisztítási hulladék nélkül értendő. |

## 8. Nyitott kérdések

### 8.1 Nyitott

1. **A Keret-ellenőrzés küszöbei** (napi 1200 kcal, napi 50 g fehérje, 10%-os sávszélesség)
   helykitöltők. *Hogyan derül ki:* a dietetikus megerősíti vagy lecseréli őket az
   indulás előtt.

2. **A jogi szövegek tartalma** (figyelmeztetés, adatvédelmi tájékoztató, egészségügyi
   adatként való kezelés, 16 éves korhatár). *Hogyan derül ki:* adatvédelmi szakértő átnézi az indulás előtt.

### 8.2 Lezárva

**Lezárva v0.2-ben:** a napi sáv étkezésekre osztása · nyers vagy főtt súly (mindig nyers)
· mit csinál ma, amikor nem jön ki · mennyi ideig tart ma (10–15 perc étkezésenként).

**Lezárva v0.3-ban:** a zsír–fehérje arány minden étkezésre azonos → nincs étkezésválasztó
· az alapérték kategóriaszintű.

**Lezárva v0.4-ben:** a grammok nem ismétlődnek, csak a készlet → a megoldó minden
étkezésnél fut, a Tányér a főképernyő, a mentett tányér kiindulópont · a makrók
egyenletesen oszlanak el (napi makró ÷ étkezésszám) · az alapanyag cseréje v1-es funkció.

**Lezárva v0.5-ben:** mentett tányérnál a tárolt grammok a referencia; a horgony
szándékos frissítéssel mozdítható.

**Lezárva v0.7-ben:** a szénhidrát kezelése a meglévő sávos matekkal rendben van — a
felhasználóval átbeszélve, nem kell változtatni rajta.

**Lezárva v0.8-ban:** a CH a papíron tartomány (alsó és felső határ) · az arány
legalább 2 g zsír / 1 g fehérje, kemény alsó korlát · a keret rögzített, nem félévente változik · v1-ben 40–60
alapanyag · a minimum 3 alapanyagba a rögzítettek is beleszámítanak · a gabonaköret és
a gyümölcs kategória megmarad · a 0 g-ra szoruló alapanyag ⚠, sávon belül is · a
rögzítés ízlés szerinti választás („ennyi tojást akarok"), nem a hűtőben lévő darab súlya,
így a Tányér képernyő marad · az étkezések számát és a kcal-sávot a felhasználó adja meg,
a kcal-sáv opcionális · a kategória-alapértékek a keto sávhoz csökkentve.

**Lezárva v0.9-ben:** a referenciától való távolság relatív eltérés (összeg) · ⚠ esetén a
sorrend: arány → legkisebb kilógás → legközelebbi · több rögzítésnél a legjobb egyetlen
feloldást ajánlja, a „Rögzítések feloldása" csak ha egyik sem elég · a kcal puha feltétel,
kilógásnál ✓ + megjegyzés · a „legutóbb használt" mentéskor frissül · a zsír–fehérje
minimum szerkeszthető, alapértéke 2 : 1 · a Keret-ellenőrzés logikai szintje végleges, a
küszöbök helykitöltők · rögzítés nélküli ⚠-nél a legtöbbet segítő kategóriát nevezi meg · minden mennyiség mellett „(nyersen)" · a ✓/⚠ a kiírt egész grammokra számol (±1 g igazítással), a makrók egy tizedessel · a mentett tányér akkor tér el, ha bármelyik kiírt gramm eltér · a
40–60 alapanyag eldöntött; hogy elég-e, az első felhasználói teszt mutatja meg.

**Lezárva v1.0-ban:** a végfelhasználó elfogadta a prototípus működését · nyilvános
weboldal, böngészőből · fiók és szinkron, belépés e-mailes linkkel · tápérték-forrás:
USDA FoodData Central · csak magyar nyelv · webanalitika süti-hozzájárulással ·
ingyenes.

**Lezárva v1.1-ben:** a „legutóbb használt" a Kész gombra is frissül; a „Mentés így" a
Kész-be olvadt · sávon belül minden szabad alapanyag ≥ 1 g, a 0 g csak ⚠ tartalék, és nem
lesz referencia · a tányér összetételt hordoz, abszolút grammal, skálázás nélkül; a
rögzítést nem tárolja · tányér átnevezése, törlése, egyedi név; saját alapanyag
szerkesztése és törlése · a kategóriák zárt listája · a „Másik kategóriából…" törlés +
hozzáadás · ⚠ sorrend: arány → kilógás → 0 g-os sorok → referencia; a kcal a ⚠ ágban nem
dönt · rögzítés nélküli „sok" esetén csere · ±2 g-os kerekítési keresés · belépés linkkel
vagy kóddal, 1 éves munkamenet · szinkron-tétel, eszközoldali idő, a törlés győz ·
fióktörlés minden eszközön · hozzájárulás-kapu verzióval; visszavonás = fióktörlés; 16
éves korhatár · adatletöltés teherhordó · 24 hónapos megőrzés · anonim analitika.
Az auditból (2026-10-24): megőrzés használat, nem belépés szerint · 0 g-os sor nem kerül
tányérba · zárolt adat és kilépési út a hozzájárulás-képernyőn · a ⚠ ágban is 0 g vagy
≥ 1 g, 0 g-os halmazonként kis LP · döntetlennél a korábbi sorok · rögzítés nélküli ⚠:
előbb arány, aztán a legnagyobb eltérés, kategóriánként egy mintaalapanyag · a ⚠ sorainak
rögzített sorrendje · a kiértékelésnek is van címsora és Kész gombja · a 3-nál kevesebb
sorú tányér megmarad · törölt alapanyagra mutató sor összefésüléskor kimarad · az aktuális
tányér csak az eszközön · kijelentkezés előtt szinkron, offline figyelmeztetés ·
visszavonható törlés csak az ablak után szinkronizál · egy tizedes a célban · belépési
token 15 perc, 5 próba · legfeljebb 8 alapanyag tányéronként · a prototípus v1.1-re
frissítve.

## 9. Feltételezés-nyilvántartás

| # | Feltételezés | Miből ered | Bizalom | Legolcsóbb teszt |
|---|---|---|---|---|
| 9.1 | A dietetikusi jelölésmód általánosan felismerhető | a projektgazda állítja, egy forrásból — **lejjebb vitte, hogy az étrend ketogén: a jelölés a ketón belül általános, nem minden dietetikus papírján**. **v1.0: a nyilvános weboldal miatt ez már nem csak elméleti kérdés** | **alacsony** | nézz meg egy nem ketós étrendet ugyanettől a dietetikustól; az indulás után: hány regisztráló hagyja félbe a Keret kitöltését |
| 9.2 | A felhasználónak vannak stabil, megszokott **összetételei** (a grammok nem ismétlődnek, az összetételek igen) | **a felhasználó megerősítette**: a készlet ismétlődik, a grammok nem | magas | — |
| 9.3 | A papíros számolás fájdalmas | **megmérve: 10–15 perc étkezésenként** | **magas** | — |
| 9.4 | Minden számolás nyers alapanyaggal | **a felhasználó mondta** | magas | — |
| 9.5 | Fél perc alatt használhatónak kell lennie | tervezői következtetés, nem felhasználói adat | közepes | analitika: a tányér összeállításának ideje (6.8) |
| 9.6 | Saját alapanyagot és saját tányért is menteni akar | **a felhasználó mondta** | magas | — |
| 9.7 | Rögzített adag köré akarja számoltatni a többit | **a felhasználó mondta** | magas | — |
| 9.8 | A makrók **egyenletesen** oszlanak el az étkezések között | **a felhasználó megerősítette** (napi makró ÷ étkezésszám) | magas | — |
| 9.9 | v1-ben 40–60 alapanyag elég (a méret eldöntött, az elégségesség nem) | becslés | közepes | analitika: hányszor nyúl a „Saját alapanyag" felé az első héten (6.8) |
| 9.10 | A rögzített mennyiség ízlés szerinti választás („ennyi tojást akarok"), nem a hűtőben lévő darab súlya | **a felhasználó megerősítette** | magas | — |
| 9.11 | A mentés nála **szándékos gesztus**, nem „hátha jó lesz" — ezért érdemes a tárolt összetételt horgonynak tekinteni | a 08-29/A opció (a mentett tányér grammjai a referencia) választásából | közepes | analitika: hány tányért ment el az első héten, és hányat használ újra (6.8) |

## 10. Vágásvonal

**Teherhordó** — ha kiveszed, az ígéret törik:
- Keret beállítása (sávok + étkezésszám + arány) — enélkül nincs mihez számolni
- Alapanyaglista tápértékekkel, nyers súlyra — enélkül nincs miből
- Saját alapanyag felvitele — a lista biztosan hiányos lesz, és az első hiány megöli a bizalmat
- Tányér összeállítása 3+ alapanyagból — ez a bemenet
- Megoldó a referenciatányér-szabállyal — ez a termék
- A ⚠ állapot a kilógás megnevezésével és a helyreállítás előzetesével — a hibás eset gyakoribb lesz, mint a jó
- Rögzítés — a felhasználó kérte, és ez illeszkedik ahhoz, ahogy valójában eszik
- Tányér mentése — **a megoldó horgonya**. (Javítás v0.4-ben: korábban „gyorsítás és
  horgony" szerepelt itt; mivel a grammok minden étkezésnél újraszámolódnak, a gyorsítás
  mellékes, a horgony az egyetlen valódi indok.)
- Alapanyag cseréje — „nincs brokkoli, van zöldbab" gyakori művelet, a felhasználó mondta
- Fiók és szinkron — a mentett tányér a horgony; ha a telefonnal elveszik, az ígéret is
- Jogi szövegek — nyilvános weboldalon étrendi adattal nem indulhat nélkülük
- Adataim letöltése és fióktörlés — hozzájárulásos kezelésnél GDPR-kötelezettség, nem dísz

**Dísz** — jó, de nem szükséges:
- Tányérok rendezése mappákba, kedvencek
- Előzmények, statisztika, grafikonok
- Sötét mód, témák
- Alapanyag-fotók
- Megosztás
- Használati mérés (6.8) — a feltételezések tesztjéhez kell, de az ígéret nélküle is teljesül
- Több nyelv

## 11. Szószedet

| Fogalom | Jelentés |
|---|---|
| **keret** | a dietetikus által adott napi cél, sávokkal, étkezésszámmal és zsír–fehérje aránnyal |
| **sáv** | egy makró alsó és felső határa |
| **étkezési cél** | a napi sáv étkezésszámmal osztott része |
| **alapanyag** | amiből számolunk; 100 g-ra vetített tápértékkel, nyers súlyra |
| **adag** | egy alapanyag kiszámolt mennyisége grammban |
| **tányér** | alapanyagok mentett kombinációja, egyedi névvel; a tárolt grammok **horgonyok, nem kész válaszok** — minden használatnál újraszámolódnak |
| **csere** | egy alapanyag helyettesítése másikkal azonos kategórián belül; leveszi a rögzítést. A „Másik kategóriából…" nem csere, hanem törlés + hozzáadás |
| **arány** | mindig a zsír–fehérje arány; a minimuma {min} : 1 |
| **összetétel** | egy tányér alapanyagai a tárolt grammjaikkal; a megoldó horgonya |
| **Kész** | név nélküli mozdulat: „ezt eszem"; frissíti a „legutóbb használt" mennyiségeket |
| **legutóbb használt** | egy alapanyag utolsó Kész vagy mentés szerinti grammja; a referencia, ha nem mentett tányérból indult |
| **rögzítés** | a felhasználó által fixált adag, amit a megoldó nem mozdíthat |
| **feloldás** | a rögzítés visszavonása: az adag újra szabaddá válik a megoldónak |
| **referenciatányér** | a megszokott mennyiségek, amikhez a megoldó a legközelebbi megoldást keresi (relatív eltérésben mérve) |
| **alapérték** | a referencia, amikor még nincs korábbi használat: az egyedi (gondozói) felülírás vagy a kategória alapértéke |
| **fiók** | egy ember adatai (a 6.6 kanonikus listája), e-mailes linkkel vagy kóddal elérve, minden eszközén ugyanazok |
| **legközelebbi elérhető** | a kimenet, ha a sáv nem érhető el: a 6.3 ⚠-sorrendje szerinti legjobb tányér (előbb az arány, aztán a legkisebb teljes kilógás); mindig a kilógás mértékével együtt |
| **aktuális tányér** | az épp szerkesztett tányér a rögzítéseivel; csak az eszközön él, a Kész vagy egy új tányér törli |

---

## Döntésnapló

| Dátum | Döntés | Indok |
|---|---|---|
| 2026-08-23 | **08-23/A: Tányér-megoldó** — étkezésenként dolgozunk, nem napi rendszerben | a döntés pillanata a főzés előtti perc |
| 2026-08-23 | **Referenciatányér** a megoldó választási szabálya (az „egész grammhoz legközelebbi" elvetve) | az egész gramm nem szűr; a mentett tányér viszont horgony |
| 2026-08-23 | **Kézzel gondozott alapanyaglista**, OFF-integráció kimarad | kis, gondozott lista kell neki, nem 150 000 tétel (a méretet lásd 2026-09-27) |
| 2026-08-23 | **Keret = egyszeri beállítás**, nem ismétlődő képernyő | a szabály 4 éve változatlan |
| 2026-08-23 | **Rögzítés** bekerül v1-be | a felhasználó kérte, szó szerint |
| 2026-08-29 | **Étkezési cél = napi sáv ÷ étkezésszám** | az étkezésszám adott, a makrók aszerint oszlanak el |
| 2026-08-29 | **Mindig nyers súly**; a főtt–nyers átváltás véglegesen kimarad | a felhasználó így számol |
| 2026-08-29 | Az alapanyaglista **nem korlátozódik magyar alapanyagokra** | az étel bárhonnan származhat |
| 2026-08-29 | A ⚠ állapot **megnevezi a rögzítést mint okot**, és a feloldás eredményét előre mutatja | ma is az alapanyagon vagy az arányon változtat; a termék mondja meg, melyik vezet valahova |
| 2026-08-29 | **08-29/B: kategória-alapérték** (6 szám, egyedi felülírással) | a horgony az első használat után felülíródik, a pontossága elpazarolt munka |
| 2026-08-29 | **Nincs étkezésválasztó képernyő** | a zsír–fehérje arány minden étkezésre azonos, tehát minden étkezés célja ugyanaz |
| 2026-08-29 | **A Tányér a főképernyő**, a mentett tányér kiindulópont | a grammok nem ismétlődnek, csak a készlet — a megoldó minden étkezésnél fut |
| 2026-08-29 | **Alapanyag cseréje** bekerül v1-be, önálló mozdulatként | „nincs brokkoli, van zöldbab" — a felhasználó mondta |
| 2026-08-29 | **08-29/A: a mentett tányér grammjai a referencia**, nem az alapanyag legutóbbi használata | különben a mentés funkció kiürül; az összetétel az egyetlen, amit a tányér hordoz |
| 2026-08-29 | **Tányér frissítése** gomb — a horgony szándékosan mozdítható | ez a 08-29/A opció elfogadott árának a javítása |
| 2026-09-27 | Az étrend **ketogén** — ez magyarázza az étkezésenkénti zsír–fehérje arányt | a felhasználó étrendje keto; az arány a ketogén étrend központi mechanikája |
| 2026-09-27 | **A termék neve: Ketara** (keto + Tar, ami egy név) | kitalált szó, ami semmivel nem ütközött ki; magyarul és angolul azonosan olvasható. Vállalt ár: a név a ketóhoz köti a terméket, miközben a megoldó bármelyik dietetikusi papírhoz működik |
| 2026-09-27 | **A keret rögzített**, nem félévente változik | a felhasználó megerősítette |
| 2026-09-27 | **v1-ben 40–60 alapanyag** | a felhasználó megerősítette |
| 2026-09-27 | **A CH tartomány**, nem csak felső korlát | a felhasználó megerősítette: a papíron így szerepel |
| 2026-09-27 | **Zsír–fehérje arány: legalább 2 g zsír / 1 g fehérje**, kemény alsó korlát; több zsír megengedett | a felhasználó a klienssel egyeztette |
| 2026-09-27 | **A minimum 3 alapanyagba a rögzítettek is beleszámítanak** | a felhasználó döntése |
| 2026-09-27 | **A gabonaköret és a gyümölcs kategória megmarad** a keto listában | a felhasználó döntése |
| 2026-09-27 | **A 0 g-ra szoruló alapanyag ⚠**, akkor is, ha minden makró sávon belül van | a felhasználó megerősítette |
| 2026-09-27 | **A rögzítés ízlés szerinti választás**, nem a hűtőben lévő mennyiség; a Tányér képernyő nem rendeződik át | a felhasználó megerősítette |
| 2026-09-27 | **Az étkezésszám és a kcal-sáv a felhasználó bevitele**; a kcal-sáv opcionális | a felhasználó tudja, mi van a papíron |
| 2026-09-27 | **A kategória-alapértékek a keto sávhoz csökkentve** | a régi értékek egyedül is túllépték egy keto étkezés CH-sávját |
| 2026-09-27 | **A távolság relatív eltérés** (az eltérések összege, nem négyzetösszege) | igazságos a kis tételekhez, jellemzően csak egy-két alapanyag mozdul, és lineáris programként gyors és determinisztikus |
| 2026-09-27 | **⚠ esetén előbb a kilógás minimuma**, utána a referencia | a ⚠ a legközelebbi *elérhető* tányért ígéri |
| 2026-09-27 | **Több rögzítésnél a legjobb egyetlen feloldás** a gomb; „Rögzítések feloldása" csak ha egyik sem elég | egy világos lépés a ⚠ képernyőn, néhány extra futás áráért |
| 2026-09-27 | **A kcal puha feltétel**: ha lehet, belefér; ha nem, ✓ + megjegyzés | az opcionális mező ne buktassa a dietetikus makrópapírját |
| 2026-09-27 | **„Legutóbb használt" mentéskor frissül** | meglévő mozdulat, csak a megtartott tányér számít, a referencia nem sodródik |
| 2026-09-27 | **A zsír–fehérje minimum szerkeszthető**, alapértéke 2 : 1 | nem kerül többe, és más papírt is lefed |
| 2026-09-27 | **Keret-ellenőrzés: logikai szint + helykitöltő küszöbök** konfigurációban | a fejlesztés ma indulhat, a klinikai számok a dietetikusnál maradnak |
| 2026-09-27 | **A mentett tányér eltér, ha bármelyik kiírt gramm eltér** | a legegyszerűbb szabály, és a determinisztikus megoldó mellett nincs zaj |
| 2026-09-27 | **A nem keto mintatányérok cseréje keto példákra** | a rizses minta épp azt a tányért mutatta, amit a termék lehetetlennek mond |
| 2026-09-27 | **Rögzítés nélküli ⚠: a kategóriát nevezi meg**, a választó azon nyílik | a megoldó tudja, melyik makró hiányzik; a teljes lista végigpróbálása nem kell |
| 2026-09-27 | **Minden mennyiség mellett „(nyersen)"** | nem kell alapanyagonkénti jelölés, és minden számolás úgyis nyers |
| 2026-09-27 | **A ✓/⚠ a kiírt grammokra számol**, a makrók egy tizedessel | amit lát, az egyezik az állapottal; nincs „0 g-mal fölötte" |
| 2026-09-27 | **A 40–60 alapanyag eldöntött**; az elégségességet az első teszt méri | a méret döntés, az elégségesség csak használatból derül ki |
| 2026-10-08 | **A prototípus működését a végfelhasználó elfogadta** | a 6.1–6.5 viselkedése a prototípus szerint végleges (a 6.6–6.8 aznap került be, a prototípus nem tartalmazza). A v1.1 a 6.3–6.5-öt módosította; a prototípus 2026-10-24-én követte (lásd ott) |
| 2026-10-08 | **Nyilvános weboldal**, böngészőből | a felhasználó döntése; nincs alkalmazásbolt |
| 2026-10-08 | **Fiók és szinkron** — a v0.9-es „nincs fiók" kizárás visszavonva | a felhasználó döntése; az adat nem veszhet el a telefonnal |
| 2026-10-08 | **Belépés e-mailes linkkel**, jelszó nélkül | a legkevesebb építeni- és védenivaló, mindenkinek van e-mail-címe |
| 2026-10-08 | **Tápértékek: USDA FoodData Central** | közkincs, szabadon közölhető, nyers értékek; a neveket fordítjuk |
| 2026-10-08 | **Csak magyar nyelv** induláskor | minden szöveg kész magyarul; később bővíthető |
| 2026-10-08 | **Teljes webanalitika**, süti-hozzájárulással; étrendi adat nem kerül bele | a felhasználó döntése; a 9.5, 9.9, 9.11 feltételezések így mérhetők |
| 2026-10-08 | **Ingyenes** | v1-ben nincs fizetési munka |
| 2026-10-24 | **Kész gomb**: a „legutóbb használt" erre is frissül; a „Mentés így" beleolvad | aki ment nélkül főz, annál is tanuljon a megoldó, a referencia mégse sodródjon |
| 2026-10-24 | **Sávon belül minden szabad alapanyag ≥ 1 g**; 0 g csak ⚠ tartalék, nem lesz referencia | ne rejtsen el ✓ tányért, és ne osszon nullával |
| 2026-10-24 | **Fióktörlés minden eszközt töröl**; szinkronban a törlés győz | a „minden adata törlődik" ígéret így igaz |
| 2026-10-24 | **Hozzájárulás-kapu verzióval; visszavonás = fióktörlés** | e-mailes linknél a fiók a hozzájárulás előtt létrejön; külön helyi mód nincs |
| 2026-10-24 | **A tányér összetételt hordoz, abszolút grammal**; rögzítéskor nincs skálázás; a rögzítés nem mentődik | ez a megoldó és a prototípus viselkedése; a rögzítés aznapi döntés |
| 2026-10-24 | **Saját alapanyag törlése a tányérokból is kiveszi a sort** | egy lépés, és a megerősítés megmutatja, mi változik |
| 2026-10-24 | **A kcal a ⚠ ágban nem dönt** | ✓ és ⚠ esetén ugyanúgy viselkedjen |
| 2026-10-24 | **Rögzítés nélküli „sok": csere a legtöbbet adó alapanyagra** | a kísérőanyag szövege már ezt mondta; a tányér felismerhető marad |
| 2026-10-24 | **„Másik kategóriából…" marad**, törlés + hozzáadásként | valós igény, a csere szabályai nem változnak |
| 2026-10-24 | **Belépés linkkel vagy 6 jegyű kóddal** | telefonon a link gyakran rossz böngészőben nyílik |
| 2026-10-24 | **Anonim analitika**, felhasználói azonosító nélkül | a tesztekhez számok kellenek, nem személyek |
| 2026-10-24 | **Megőrzés: 24 hónap használat nélkül → értesítés → 30 nap → törlés** | védhető határ, és nem lepi meg, aki hosszabb szünetet tart; a használat (megnyitás, szinkron) számít, mert a belépés egy évig megmarad |
| 2026-10-24 | **Egy „(nyersen)" formátum mindenhol**, a sorokban is | egységes; a prototípus sorait igazítani kell |
| 2026-10-24 | **0 g-os sor nem kerül a tányérba** | a tárolt gramm a referencia; 0 nem lehet az |
| 2026-10-24 | **Hozzájárulás-képernyő kilépési úttal**; új szövegverziónál a meglévő adat zárolva | aki nem fogadja el, ne ragadjon be, és az adata se tűnjön el szó nélkül |
| 2026-10-24 | **A ⚠ ágban is 0 g vagy ≥ 1 g**; a 0 g-os halmazok sorra véve, halmazonként kis LP | a „legkevesebb 0 g" lépés így pontosan kiszámolható |
| 2026-10-24 | **Döntetlennél a korábban felvett sorok maradnak a referenciánál** | determinisztikus, és a felhasználó sorrendjét követi |
| 2026-10-24 | **Rögzítés nélküli ⚠: arány → legnagyobb eltérés; kategóriánként egy mintaalapanyag** | minden esetre van szabály, és hat próbafutás elég |
| 2026-10-24 | **A ⚠ sorai rögzített sorrendben**, egyszer-egyszer | a legfontosabb van elöl, és két képernyő nem mondhat mást |
| 2026-10-24 | **Az aktuális tányér csak az eszközön**; a rögzítések újranyitáskor megmaradnak | főzés közben bezárt app ne veszítse el a munkát; másik eszközre nem kell |
| 2026-10-24 | **Kijelentkezés előtt szinkron**; offline, el nem küldött változásnál figyelmeztetés | a kilépés ne veszítsen el adatot szó nélkül |
| 2026-10-24 | **Belépési token: link és kód együtt, 15 perc, 5 hibás kód** | egyszerű és biztonságos |
| 2026-10-24 | **Legfeljebb 8 alapanyag egy tányéron** | a 0 g-os halmazok sorra vétele így kicsi marad; a valós tányérok ennél kisebbek |
| 2026-10-24 | **A prototípus v1.1-re frissítve**: Kész, ≥ 1 g, 0 g-os lépés, ±2 g, kcal a ⚠-n kívül, „sok" csere, döntetlen, (nyersen), ◯/🔒, legfeljebb 8 | a 10-08-i elfogadás óta változott viselkedés így kipróbálható |

## Javítások

- **v1.1 (audit):** a PR független auditja (`reviews/ketara-pr4-audit.md`, 40 pont) alapján.
  A megőrzés a belépést mérte, pedig a belépés egy évig megmarad. A ⚠ tányér 0 g-os sora
  0-s referenciát adott volna. A hozzájárulás-kapu mögött ragadt, aki nem fogadta el az új
  szöveget. A „legkevesebb 0 g" lépés nem volt egyetlen LP-vel számolható, a döntetlen nem
  volt definiálva, a rögzítés nélküli ⚠ vegyes esetekre nem adott szabályt, és a ⚠ sorainak
  nem volt sorrendje. A kiértékelésből hiányzott a címsor és a Kész. A 3-nál kevesebb sorúvá
  váló tányér sorsa, a törölt alapanyagra mutató sor szinkronja, az aktuális tányér
  tárolása, az offline kijelentkezés és a visszavonható törlés szinkronja definiálatlan
  volt. A szószedet a legközelebbi elérhetőt az arány nélkül írta le; a 6.7
  „regisztrációt" említett, amilyen képernyő nincs; az első indítás sora elavult volt; a
  sablonok névelője hibás alakot adott („a olajat"). A prototípus a v1.1 előtti
  viselkedést mutatta.

- **v1.1:** független átnézés (`reviews/ketara-prd-v1.0-cold-review.md`, 27 pont) alapján.
  A 6.3 egyszerre mondta, hogy a „legutóbb használt" csak mentéskor frissül, és hogy az első
  használat után átveszi az alapérték helyét — a Kész gomb feloldja. A 0 g, a ±1 g-os
  kerekítés kudarca, a nem teljesíthető arány mérése és a kcal helye a ⚠ sorrendben
  definiálatlan volt. A fióktörlés és az offline szinkron ütközött, a szinkron-tétel nem
  volt meghatározva. A hozzájárulás-kapu kihagyható volt, visszavonás nem volt leírva. Az
  „arány" két jelentése szétválasztva (a tányérnál „összetétel"). A mentőgombok ⚠ esetén
  ellentmondtak. Hiányzott a tányér átnevezése/törlése, a saját alapanyag
  szerkesztése/törlése, a kategóriák zárt listája, a „sok" eset rögzítés nélkül, a belépés
  másik böngészőben, a megőrzési idő, a korhatár és az adatletöltés (az export „dísz" volt).
  A 7. fejezetben beégetett „2 : 1" volt; az 1. fejezet a kcal-sávot mindig adottnak írta; a
  3-as minimum indoklása ellentmondott a rögzítettek beszámításának; a fiók tartalma négy
  helyen négyféle volt; a „saját alapérték-felülírás" kétértelmű volt; a „csak elgépelésre
  szerkeszthető" keret nem volt kikényszeríthető. A Döntésnapló 08-23-i sora utólag át volt
  írva, és két „A opció" szerepelt; a 8. fejezet nyitott kérdései szét voltak szórva; a 6.4
  ⚠ példájában a zárójel a tányér makróit a tojáséinak mutatta, és a „négy tojás" nem 240 g;
  a „(nyersen)" formátuma eltért a két dokumentumban; a mérés a vágásvonalból, a Keret-
  elhagyás eseménye a 6.8-ból hiányzott.

- **v1.0:** az 5. fejezet „Felhasználói fiók, felhőszinkron" kizárása a fiók és szinkron
  bevezetésével érvényét vesztette, törölve. Az „Edző / kliens szerepek, több
  felhasználó" sor pontosítva: több felhasználó van (nyilvános weboldal), közös fiók nincs.
  A 9.5, 9.9 és 9.11 legolcsóbb tesztje a megfigyelés helyett az analitika.

- **v0.9:** a 6.4-es példák nem következtek egyetlen rögzített szabályból sem (a
  referencia nem volt megadva; a ⚠ példa nem a legkisebb kilógást mutatta, és 42 g olaj
  is elég lett volna 43 helyett). A távolság, a ⚠ sorrend és a kerekítés most definiált, a
  példák ezek szerint újraszámolva. A rögzített tojás jele 📌-ről 📍-re javítva (a kísérőanyag
  jelölése szerint). A 75 g rizses példa keto alapanyagra cserélve. A 40–60 alapanyag
  egyszerre volt nyitott és lezárt kérdés — lezárva.
- **v0.8:** a 6.4-es ✓ példa lehetetlen volt: rizsből, tojásból és olajból a sáv nem
  érhető el (ezt a ⚠ példa maga is kimondja). A példa végigszámolt, a 2 : 1-es
  zsír–fehérje arányt tartó keto számokra cserélve (tojás, brokkoli, olaj), a
  tápértékekkel együtt. A 4. fejezet
  számozása javítva. A 6.1-es nyitott CH-jegyzet törölve (v0.7-ben lezárult). A 6.5-ből
  hiányzott a kötelező kategória. Az alapanyaglista mérete 150–250-ről 40–60-ra javítva.
  A keret nem félévente szerkeszthető, hanem rögzített.
- **v0.6:** a 9.1-es feltételezés bizalma közepesről alacsonyra került. A jelölésmód
  „általános" volta a ketogén gyakorlaton belül értendő, nem a dietetikai gyakorlat
  egészében.
- **v0.4:** a 10. fejezetben a tányér mentésének indoklása pontatlan volt („gyorsítás és
  horgony"). A gyorsítás nem indok, mert a grammok újraszámolódnak.
- **v0.2:** a v0.1 6.4-es példája számszerűen hibás volt („zsír 3 g-mal a sáv alatt",
  miközben az olaj 0 g-ra szorult — ez csak a sáv **fölött** fordulhat elő). Végigszámolt
  példára cserélve.
