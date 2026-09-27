# Ketara — PRD v0.9

*Élő dokumentum. Helyben frissül, ahogy a döntések megszületnek.*
*Kísérőanyag: `ketara-folyamatok-kepernyok.md` — folyamatok, képernyők, szövegek.*
*Utolsó frissítés: 2026-09-27*

---

## 1. Probléma

Egy ember dietetikustól kapott, írásos szabály szerint étkezik — **ketogén étrendet**.
A papír megadja a napi
kcal-, szénhidrát-, fehérje- és zsírsávot, az étkezések számát, valamint az
étkezésenkénti zsír–fehérje arányt. A szabály **négy éve változatlan**.

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

**Kifejezetten nem:** a makrót követő testépítő; az edző és a rábízott kliensek; aki
étrendet *kap* kész grammokkal (annak nincs mit számolni).

## 3. Ígéret

> Megmondja, hány grammot vegyél az előtted lévő alapanyagokból, hogy a dietetikusod
> sávján belül maradj — 10–15 perc keresgélés helyett fél perc alatt.

## 4. Scope — v1

1. **Keret beállítása** — egyszeri művelet: napi CH-, fehérje- és zsírsáv, opcionális
   kcal-sáv (alsó és felső határ), a **napi étkezések száma**, valamint az
   étkezésenkénti zsír–fehérje arány. A keret rögzített; a képernyő csak elgépelés javítására szerkeszthető.
2. **Alapanyaglista** — v1-ben kb. 40–60 kézzel gondozott alapanyag, 100 g-ra vetített
   tápértékkel, **nyers súlyra**. Összetétele **keto-súlypontú**: zsiradékok, húsok,
   tojás, sajtok, alacsony szénhidráttartalmú zöldségek; a gabonaköret és a gyümölcs
   kategória is megmarad. Kevés tétel, szűk kör — ez könnyíti az összeállítást. Nem
   korlátozódik magyar alapanyagokra: a lista alapanyag szerint épül, nem konyha szerint.
   Minden sor **kategóriába** tartozik, és opcionálisan saját alapérték-felülírást kaphat.
3. **Saját alapanyag felvitele** — név + **kategória** + 100 g tápérték, nyers súlyra.
   A kategória kötelező: ebből jön az alapérték. A gondozott lista biztosan hiányos lesz.
4. **Tányér összeállítása** — legalább 3 alapanyag kiválasztása.
5. **Rögzítés** — egy vagy több alapanyag grammjának fixálása. A megoldó a maradékot
   számolja ki köré.
6. **Alapanyag cseréje** — egy sor lecserélése másikra, azonos kategórián belül
   („nincs brokkoli, van zöldbab"). Önálló mozdulat, nem törlés+hozzáadás.
7. **Megoldás** — a sávon belüli megoldások közül a **referenciatányérhoz legközelebbi**
   (relatív eltérésben mérve, 6.3), egész grammra kerekítve.
8. **Két kimeneti állapot** — sávon belül, illetve legközelebbi elérhető + a kilógás
   mértéke + a helyreállítás útja.
9. **Tányér mentése** saját néven, a grammokkal együtt.
10. **Mentett tányér előhívása** és újraszámolása.
11. **Tányér frissítése** — mentett tányérból indulva a horgony felülírható az új
    arányokkal, egy gombbal.

## 5. Out of scope — és miért

| Kimarad | Miért |
|---|---|
| Napi naplózás, étkezésnyilvántartás | Étkezésenként dolgozunk. A napkövetés külön termék, és ott a KalóriaBázis 150 000 tétellel és félmillió felhasználóval van jelen. |
| Vonalkód, Open Food Facts integráció | A gondozott lista lefedi a főzős életet. Az OFF adatait önkéntesek viszik fel, pontosságra nincs garancia — dietetikusi sávnál ez érzékeny. |
| Célkalkulátor (BMR/TDEE) | A számokat a dietetikus adja. Fél terméknyi munka megspórolva. |
| Edző / kliens szerepek, több felhasználó | Másik termék. |
| Főtt–nyers súlyátváltás, receptek | **Véglegesen kimarad**, nem csak v1-ből: minden számolás nyers alapanyaggal történik. |
| Mikrotápanyagok, rost, só, vitaminok | A papíron nincsenek. |
| Felhasználói fiók, felhőszinkron | Egy telefon, egy ember. |

## 6. Viselkedés

### 6.1 Keret beállítása
**A felhasználó:** beírja a napi sávokat, az étkezések számát és az étkezésenkénti
zsír–fehérje arányt.
**A termék:** eltárolja, és kiszámolja az **étkezési célt**. Nem kérdezi újra.

**Szabályok:**
- Étkezési cél = napi sáv ÷ étkezések száma, mindkét határra.
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
  - *Gyanús érték* (visszakérdez, igen után elfogadja): napi kcal 1200 alatt, napi fehérje
    50 g alatt, vagy bármely sáv szélessége kisebb az alsó határa 10%-ánál. **Ezek
    helykitöltő számok**, konfigurációs értékként tárolva; a dietetikus megerősíti vagy
    lecseréli őket az indulás előtt, alkalmazásfrissítés nélkül.

### 6.2 Tányér összeállítása
**A felhasználó:** kiválaszt legalább 3 alapanyagot. Bármelyikhez megadhat rögzített
grammot.
**A termék:** minden változtatás után újraszámol.
**Szabály:** 3 alapanyag alatt nem számolunk — három makrósávhoz legalább három
mennyiség kell, különben a sávok jellemzően nem teljesíthetők egyszerre. A rögzített
alapanyagok is beleszámítanak a háromba.
Ha minden alapanyag rögzített, a termék nem old meg, csak kiértékel: megmondja, hol tart
a sávokhoz képest.

### 6.2b Alapanyag cseréje
**A felhasználó:** egy soron a cserét választja.
**A termék:** felkínálja ugyanannak a kategóriának a tagjait, majd helyben lecseréli és
újraszámol.
**Szabály:** a csere **leveszi a rögzítést** az adott sorról. 180 g brokkolit rögzíteni nem
ugyanaz, mint 180 g zöldbabot. Az új alapanyag referenciája a saját legutóbbi használata,
ennek hiányában az egyedi felülírása, végül a kategória alapértéke (6.3).

### 6.3 A megoldó választási szabálya
**A szabály:** a sávon belüli megoldások közül azt választja, amelyik a
**referenciatányérhoz** legközelebb esik.

**A távolság mértéke: relatív eltérés.** Minden nem rögzített alapanyagra
|adag − referencia| ÷ referencia, és ezek **összege** (nem négyzetösszege). Így 3 g
változás 30 g olajon ugyanannyit számít, mint 20 g változás 200 g tojáson, és jellemzően
csak egy-két alapanyag mozdul, a többi a megszokott mennyiségén marad. Ez lineáris
program: gyors, pontos, és ugyanarra a bemenetre mindig ugyanazt adja.

**Ha nincs sávon belüli megoldás** (⚠), a megoldó ebben a sorrendben dönt:
1. a zsír–fehérje arány minimuma — kemény feltétel, amíg teljesíthető;
2. a sávokból való **teljes kilógás grammban** a lehető legkisebb (CH, fehérje, zsír
   kilógásának összege);
3. ezek közül a referenciához legközelebbi (relatív eltérés, mint fent).

A referencia forrása, ebben a sorrendben:
1. ha mentett tányérból indult: a **tányérban tárolt** mennyiség — a tányér egy megőrzött
   arány, és ez az egyetlen dolog, amit hordoz;
2. egyébként amennyit a felhasználó **legutóbb** használt ebből az alapanyagból. A
   „legutóbb használt" **mentéskor** frissül (Tányér mentése, frissítése, Mentés így,
   Mentés új néven); a sima újraszámolás nem írja felül, így a referencia nem sodródik;
3. ha nincs ilyen, az alapanyag **egyedi felülírása**, végül a **kategória alapértéke**.

**Kategória-alapértékek** (kiinduló számok a keto sávhoz, a lista összeállításakor
véglegesítendők): fehérjeforrás 100 g · gabonaköret 10 g nyersen · zöldség 100 g ·
zsiradék 30 g · gyümölcs 50 g · tejtermék 50 g. Ahol a kategória alapértéke nyilvánvalóan
rossz (saláta 100 g helyett 50 g), az alapanyag **egyedi felülírást** kap.

Az alapérték csak horgony: az első használat után a felhasználó saját mennyisége veszi át
a helyét. Ezért elég, ha irányban jó.

**Kerekítés:** a megoldó törtgrammal számol, a kiírás egész gramm. A **kiírt, kerekített
tányér** a mérvadó: erre számolja a makrókat és a ✓/⚠ állapotot. Ha a kerekítés kiléptetne
egy sávból vagy az arányból, a megoldó a szomszédos egész grammok (±1 g) közül a legjobbat
választja, ugyanazzal a sorrenddel, mint fent. A makróértékek és a kilógás **egy
tizedesjeggyel** jelennek meg („6,6 g"), az arány is („2,2 : 1").

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
   [ Tányér mentése ]
```

**Legközelebbi elérhető** — a felhasználó rögzítette a tojást 240 g-ra (négy tojás)
(CH 5,0 · Fe 31,6 · Zs 63,2 · arány 2,0 : 1). A megoldó először a kilógást csökkenti: a
brokkolit addig viszi le, amíg a CH-sáv alja engedi (48 g), az olajat annyira emeli, amennyit
az arány kér (39 g):
```
⚠  240 g tojás 📍 · 48 g brokkoli · 39 g olaj (nyersen)
   Fehérje: 6,6 g-mal a sáv fölött.
   A rögzített tojás önmagában 30,2 g fehérjét hoz; a sáv teteje 25 g.

   Ha feloldod a tojást:
   137 g tojás · 100 g brokkoli · 31 g olaj (nyersen) → minden érték a sávon belül.

   [ Tojás feloldása ]        [ + Alapanyag ]
   [ Mentés így ]
```

**Szabályok a ⚠ állapotra:**
- Mindig megnevezi, **melyik makró, mennyivel, melyik irányba** csúszik ki. Ha az arány
  sérül, azt is: „Zsír–fehérje arány: 1,5 : 1, a minimum {min} : 1."
- Ha a kilógás oka egy rögzítés, ezt kimondja. A felhasználó különben a rossz elemet
  próbálja javítani.
- A feloldó gomb **megnevezi az alapanyagot** („Tojás feloldása"), nem generikus.
- **Több rögzítésnél** a megoldó egyenként kipróbálja minden rögzítés feloldását (rögzítésenként
  egy kis futás). Azt az egyet ajánlja fel, amelyiknek a feloldása sávba hoz; ha több is,
  amelyik a referenciához közelebbi tányért ad; ha egyik sem, amelyik a legtöbbet csökkent a
  kilógáson. A „Rögzítések feloldása" gomb csak akkor jelenik meg mellette, ha **egyetlen
  feloldás sem** hoz sávba.
- A feloldás **eredményét előre mutatja**, kattintás előtt — akkor is, ha a feloldás sem
  oldja meg. A termék akkor jó, ha megmondja, melyik lépés vezet valahova.
- A feloldás nem törli az alapanyagot: csak visszaadja a grammját a megoldónak.
- Ha nincs rögzítés, a feloldó gomb nem létezik.
- A **Mentés így** mindig ott van: a felhasználó dönti el, mit eszik meg. A termék egyszer,
  világosan megmondja a kilógást, és nem kérdez rá kétszer.

### 6.5 Mentés
**Saját alapanyag:** név + kategória + 100 g tápérték, nyers súlyra. Bekerül a listába,
kereshető.

**Tányér:** saját név + az alapanyagok a kiszámolt grammokkal. A tárolt grammok **nem kész
válasz, hanem horgony**: a grammok minden étkezésnél újra kiszámolódnak, mert a valóság
(mennyi hús van itthon, van-e brokkoli) minden alkalommal más. A tányér azt őrzi meg,
milyen *arányban* szereti ezeket együtt.

**Tányér frissítése:** ha mentett tányérból indult és **bármelyik kiírt (egész) gramm**
eltér a tárolttól, a mentés helyén két lehetőség áll: *„{Név} frissítése"* és *„Mentés új
néven"*. Tűréshatár nem kell: a megoldó determinisztikus, ugyanarra a bemenetre ugyanazt
adja, így eltérés csak valódi okból lehet (javított keret, módosított tápérték, rögzítés,
csere). A horgony így csak szándékosan mozdul el — nem sodródik csendben, és nem is ragad be
véglegesen.

## 7. Peremesetek

| Eset | Mit tesz a termék |
|---|---|
| 3-nál kevesebb alapanyag | Nem számol. Megmondja, hány alapanyag hiányzik még (egy vagy kettő), nem hibaüzenettel. |
| Nincs sávon belüli megoldás | Legközelebbi elérhető + melyik makró mennyivel, melyik irányba csúszik ki + a helyreállítás útja. Ha nincs rögzítés, a helyreállítás a kilógó makróhoz legtöbbet segítő **kategóriát** nevezi meg („Kevés a zsír — adj hozzá a Zsiradék közül."), és a hozzáadás gomb azon a kategórián nyitja meg a választót. |
| A zsír kevesebb, mint a fehérje {min}-szerese | ⚠: megnevezi az arányt és a minimumot („Zsír–fehérje arány: 1,5 : 1, a minimum 2 : 1"). |
| A rögzített mennyiség önmagában kilépteti a sávból | Külön üzenet: a rögzítés a hibás elem. Felajánlja a feloldást, az eredmény előzetesével. |
| A feloldás sem elég | Kimondja, hogy ebből a készletből nem jön ki. Nem küldi a felhasználót vakon próbálgatni. |
| A megoldás negatív mennyiséget adna | Nem mutatunk mínuszt. A padló 0 g. Ha egy alapanyag 0 g-ra szorul, az ⚠ akkor is, ha minden makró a sávon belül van. |
| Minden alapanyag rögzítve | Nem old meg, kiértékel: hol tart a sávokhoz képest. |
| Saját alapanyag értelmetlen tápértékkel | Ellenőrzés: 100 g-ban a CH+fehérje+zsír nem haladhatja meg a 100 g-ot. |
| Lehetetlen keret (alsó > felső, vagy a zsírsáv teteje < {min} × fehérjesáv alja) | Nem menti. Megnevezi a hibás sort. |
| Gyanúsan alacsony vagy szűk keret (6.1 helykitöltő küszöbei) | A termék nem hajtja végre szó nélkül. Egyszer visszakérdez; igen után elfogadja. |
| A makrók sávon belül, a kcal nem | ✓ marad, alatta egy sor: „Kalória: {n} kcal-lal a sáv {fölött / alatt}." A megoldó előbb kcal-ban is megfelelő megoldást keres (6.1). |
| Első indítás | Nincs keret, nincs tányér, nincs előzmény. Az üres állapot egyetlen dolgot kínál: állítsd be a keretet. |
| Nincs hálózat | Minden helyben működik. A konyhában ez nem luxus. |
| A horgony elavult | A felhasználó frissítheti a tányért egy gombbal. A termék nem dönti el helyette, hogy melyik arány az érvényes. |
| Csere rögzített soron | A rögzítés lekerül, a termék ezt kiírja: „A rögzítés lekerült, mert az alapanyag változott." |
| Nyers–főtt félreértés | **Minden** kiírt mennyiség mellett ott áll: „90 g csirkemell (nyersen)". Alapanyagonkénti „főzve változik" jelölés nincs: minden számolás nyers súllyal történik. |

## 8. Nyitott kérdések

1. **A Keret-ellenőrzés küszöbei** (napi 1200 kcal, napi 50 g fehérje, 10%-os sávszélesség)
   helykitöltők. *Hogyan derül ki:* a dietetikus megerősíti vagy lecseréli őket az
   indulás előtt.

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

## 9. Feltételezés-nyilvántartás

| # | Feltételezés | Miből ered | Bizalom | Legolcsóbb teszt |
|---|---|---|---|---|
| 9.1 | A dietetikusi jelölésmód általánosan felismerhető | a projektgazda állítja, egy forrásból — **v0.6-ban lejjebb vitte, hogy az étrend ketogén: a jelölés a ketón belül általános, nem minden dietetikus papírján** | **alacsony** | nézz meg egy nem ketós étrendet ugyanettől a dietetikustól |
| 9.2 | A felhasználónak vannak stabil, megszokott **arányai** (a grammok nem ismétlődnek, az arányok igen) | **a felhasználó megerősítette**: a készlet ismétlődik, a grammok nem | magas | — |
| 9.3 | A papíros számolás fájdalmas | **megmérve: 10–15 perc étkezésenként** | **magas** | — |
| 9.4 | Minden számolás nyers alapanyaggal | **a felhasználó mondta** | magas | — |
| 9.5 | Fél perc alatt használhatónak kell lennie | tervezői következtetés, nem felhasználói adat | közepes | figyeld, mikor teszi le a telefont |
| 9.6 | Saját alapanyagot és saját tányért is menteni akar | **a felhasználó mondta** | magas | — |
| 9.7 | Rögzített adag köré akarja számoltatni a többit | **a felhasználó mondta** | magas | — |
| 9.8 | A makrók **egyenletesen** oszlanak el az étkezések között | **a felhasználó megerősítette** (napi makró ÷ étkezésszám) | magas | — |
| 9.9 | v1-ben 40–60 alapanyag elég (a méret eldöntött, az elégségesség nem) | becslés | közepes | első felhasználói teszt: hányszor nyúl a „Saját alapanyag" felé az első héten |
| 9.10 | A rögzített mennyiség ízlés szerinti választás („ennyi tojást akarok"), nem a hűtőben lévő darab súlya | **a felhasználó megerősítette** | magas | — |
| 9.11 | A mentés nála **szándékos gesztus**, nem „hátha jó lesz" — ezért érdemes a tárolt arányt horgonynak tekinteni | a 2026-08-29-i A opció (a mentett tányér grammjai a referencia) választásából | közepes | nézd meg, hány tányért ment el az első héten, és hányat használ újra |

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

**Dísz** — jó, de nem szükséges:
- Tányérok rendezése mappákba, kedvencek
- Előzmények, statisztika, grafikonok
- Sötét mód, témák
- Alapanyag-fotók
- Megosztás, export
- Több nyelv

## 11. Szószedet

| Fogalom | Jelentés |
|---|---|
| **keret** | a dietetikus által adott napi cél, sávokkal, étkezésszámmal és zsír–fehérje aránnyal |
| **sáv** | egy makró alsó és felső határa |
| **étkezési cél** | a napi sáv étkezésszámmal osztott része |
| **alapanyag** | amiből számolunk; 100 g-ra vetített tápértékkel, nyers súlyra |
| **adag** | egy alapanyag kiszámolt mennyisége grammban |
| **tányér** | alapanyagok mentett kombinációja, saját névvel; a tárolt grammok **horgonyok, nem kész válaszok** — minden használatnál újraszámolódnak |
| **csere** | egy alapanyag helyettesítése másikkal azonos kategórián belül; leveszi a rögzítést |
| **rögzítés** | a felhasználó által fixált adag, amit a megoldó nem mozdíthat |
| **feloldás** | a rögzítés visszavonása: az adag újra szabaddá válik a megoldónak |
| **referenciatányér** | a megszokott mennyiségek, amikhez a megoldó a legközelebbi megoldást keresi (relatív eltérésben mérve) |
| **alapérték** | a referencia, amikor még nincs korábbi használat |
| **legközelebbi elérhető** | a kimenet, ha a sáv nem érhető el: a legkisebb teljes kilógású tányér; mindig a kilógás mértékével együtt |

---

## Döntésnapló

| Dátum | Döntés | Indok |
|---|---|---|
| 2026-08-23 | **A opció: Tányér-megoldó** — étkezésenként dolgozunk, nem napi rendszerben | a döntés pillanata a főzés előtti perc |
| 2026-08-23 | **Referenciatányér** a megoldó választási szabálya (az „egész grammhoz legközelebbi" elvetve) | az egész gramm nem szűr; a mentett tányér viszont horgony |
| 2026-08-23 | **Kézzel gondozott alapanyaglista**, OFF-integráció kimarad | 40–60 tétel kell neki, nem 150 000 |
| 2026-08-23 | **Keret = egyszeri beállítás**, nem ismétlődő képernyő | a szabály 4 éve változatlan |
| 2026-08-23 | **Rögzítés** bekerül v1-be | a felhasználó kérte, szó szerint |
| 2026-08-29 | **Étkezési cél = napi sáv ÷ étkezésszám** | az étkezésszám adott, a makrók aszerint oszlanak el |
| 2026-08-29 | **Mindig nyers súly**; a főtt–nyers átváltás véglegesen kimarad | a felhasználó így számol |
| 2026-08-29 | Az alapanyaglista **nem korlátozódik magyar alapanyagokra** | az étel bárhonnan származhat |
| 2026-08-29 | A ⚠ állapot **megnevezi a rögzítést mint okot**, és a feloldás eredményét előre mutatja | ma is az alapanyagon vagy az arányon változtat; a termék mondja meg, melyik vezet valahova |
| 2026-08-29 | **B: kategória-alapérték** (6 szám, egyedi felülírással) | a horgony az első használat után felülíródik, a pontossága elpazarolt munka |
| 2026-08-29 | **Nincs étkezésválasztó képernyő** | a zsír–fehérje arány minden étkezésre azonos, tehát minden étkezés célja ugyanaz |
| 2026-08-29 | **A Tányér a főképernyő**, a mentett tányér kiindulópont | a grammok nem ismétlődnek, csak a készlet — a megoldó minden étkezésnél fut |
| 2026-08-29 | **Alapanyag cseréje** bekerül v1-be, önálló mozdulatként | „nincs brokkoli, van zöldbab" — a felhasználó mondta |
| 2026-08-29 | **A: a mentett tányér grammjai a referencia**, nem az alapanyag legutóbbi használata | különben a mentés funkció kiürül; az arány az egyetlen, amit a tányér hordoz |
| 2026-08-29 | **Tányér frissítése** gomb — a horgony szándékosan mozdítható | ez az A opció elfogadott árának a javítása |
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

## Javítások

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
