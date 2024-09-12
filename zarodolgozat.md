BUDAPESTI KOMPLEX SZAKKÉPZÉSI CENTRUM 

WEISS MANFRÉD 
TECHNIKUM, SZAKKÉPZŐ ISKOLA ÉS KOLLÉGIUM 

ZÁRÓDOLGOZAT 

INTELLIGENS OTTHON 

Témavezető:  Készítette: 

Kovács László Bálint  Holló Csaba 

Szoftverfejlesztő szak  

Budapest 2021 

<a name="_page1_x68.00_y70.92"></a>Köszönetnyilvánítás 

Ezúton  is  szeretném  megköszönni  tanáraimnak,  osztályfőnökömnek  kitartó  munkájukat, segítőkészségüket, mert nélkülük ez a záródolgozat nem készülhetett volna el. 

Külön  hálával  tartozom  Kovács  László  Bálint  tanár  úrnak,  aki  a  tanórán  kívül  is  a rendelkezésemre állt, hogy megválaszolja kérdéseimet. 

<a name="_page2_x68.00_y70.92"></a>Tartalomjegyzék 

[Köszönetnyilvánítás ................................................................................................................... 2 ](#_page1_x68.00_y70.92)[Tartalomjegyzék ......................................................................................................................... 3 ](#_page2_x68.00_y70.92)[Bevezetés .................................................................................................................................... 6 ](#_page5_x68.00_y70.92)[Implementációs leírás ................................................................................................................. 7 ](#_page6_x68.00_y70.92)[FELHASZNÁLÓI DOKUMENTÁCIÓ .................................................................................... 8 ](#_page7_x68.00_y70.92)[Bevezetés ................................................................................................................................ 8 ](#_page7_x68.00_y110.92)[Szimuláció .............................................................................................................................. 8 ](#_page7_x68.00_y345.92)[Irányító panel...................................................................................................................... 8 ](#_page7_x68.00_y443.92)[Kísérleti panel .................................................................................................................... 9 ](#_page8_x68.00_y70.92)[Alrendszerek......................................................................................................................... 10 ](#_page9_x68.00_y70.92)[Platform ................................................................................................................................ 10 ](#_page9_x68.00_y264.92)[Otthonunk aktuális jellemzői ............................................................................................... 10 ](#_page9_x68.00_y381.92)[Szobák hőmérsékletének kijelzése ................................................................................... 10 ](#_page9_x68.00_y538.92)[Külső hőmérséklet kijelzése ............................................................................................. 11 ](#_page10_x68.00_y472.92)[Szobák hőmérsékletének beállítása .................................................................................. 12 ](#_page11_x68.00_y483.92)[Fűtés működésének ellenőrzése ....................................................................................... 12 ](#_page11_x68.00_y612.92)[Belépés ......................................................................................................................... 13 ](#_page12_x68.00_y70.92)[Beállított hőmérséklet láthatósága ............................................................................... 13 ](#_page12_x68.00_y359.92)[Hőfokbeállítás .............................................................................................................. 15](#_page14_x68.00_y70.92)

[Fűtésvezérlés ................................................................................................................ 16](#_page15_x68.00_y70.92)

[Kilépés .......................................................................................................................... 16 ](#_page15_x68.00_y505.92)[Szobák lámpáinak állapotai .......................................................................................... 16 ](#_page15_x68.00_y607.92)[FEJLESZTŐI DOKUMENTÁCIÓ .......................................................................................... 18 ](#_page17_x68.00_y70.92)[Hardver ................................................................................................................................. 18 ](#_page17_x68.00_y378.92)[Mikrovezérlő .................................................................................................................... 18 ](#_page17_x68.00_y416.92)[Prototípus fejlesztő panel ................................................................................................. 19](#_page18_x68.00_y242.92)

[Az I2C busz elemei ...................................................................................................... 19 ](#_page18_x68.00_y352.92)[N75 hőmérséklet érzékelő ........................................................................................ 19](#_page18_x68.00_y410.92)

[Távoli 8 bites I / O bővítő ........................................................................................ 19 ](#_page18_x68.00_y492.92)[OLED kijelző ........................................................................................................... 19](#_page18_x68.00_y620.92)

[I2C kommunikáció ........................................................................................................... 20 ](#_page19_x68.00_y523.92)[I2C buszon lévő eszközök ............................................................................................ 21 ](#_page20_x68.00_y70.92)[Szoftver ................................................................................................................................ 21 ](#_page20_x68.00_y345.92)[TCP/IP kommunikáció ..................................................................................................... 21 ](#_page20_x68.00_y383.92)[Felhasznált függvények ................................................................................................ 21 ](#_page20_x68.00_y497.92)[Intelligens otthon vezérlés ................................................................................................ 22](#_page21_x68.00_y98.92)

[Feladatai ....................................................................................................................... 22 ](#_page21_x68.00_y171.92)[Hőmérséklet szenzoroktól érkező adatok konvertálása ........................................... 22 ](#_page21_x68.00_y229.92)[A beállított és mért hőfok alapján a fűtés vezérlése ................................................. 24 ](#_page23_x68.00_y70.92)[I2c kommunikáció vezérlése, kommunikációs hiba detektálása .............................. 25 ](#_page24_x68.00_y118.92)[N75 hőmérséklet szenzor mért hőmérséklet lekérdezés....................................... 26](#_page25_x68.00_y70.92)

[N75 hőmérséklet szenzor lekérdezés folyamatának részletezése......................... 28 ](#_page27_x68.00_y70.92)[8 bites I/O bővítő írása, olvasása .............................................................................. 31](#_page30_x68.00_y70.92)

[I/O bővítő olvasás függvény ................................................................................ 31 ](#_page30_x68.00_y92.92)[I/O bővítő írás függvény ...................................................................................... 33](#_page32_x68.00_y70.92)

[OLED kijelző vezérlése ........................................................................................... 34 ](#_page33_x68.00_y541.92)[Webszerver inicializálása, futtatása ......................................................................... 35 ](#_page34_x68.00_y554.92)[Weblap ............................................................................................................................. 36 ](#_page35_x68.00_y143.92)[HTML, CSS ................................................................................................................. 36](#_page35_x68.00_y257.92)

- [Kép beillesztése .............................................................................................. 37](#_page36_x68.00_y97.92)
- [Abszolút pozicionálás ..................................................................................... 37](#_page36_x68.00_y177.92)
- [Belső hőfok kijelzés........................................................................................ 37](#_page36_x68.00_y332.92)
- [helyiség hőfok kijelzés tulajdonságai ............................................................. 38](#_page37_x68.00_y70.92)
- [Külső hőfok kijelzés ....................................................................................... 38](#_page37_x68.00_y352.92)
- [Külső hőmérséklet kijelzés jellemzőinek beállítása ....................................... 38](#_page37_x68.00_y494.92)

[Java script ..................................................................................................................... 39](#_page38_x68.00_y70.92)

- [Hőfok adatok frissítése ................................................................................... 39](#_page38_x68.00_y185.92)
- [Lámpák (Ledek) állapotainak frissítése .......................................................... 39](#_page38_x68.00_y339.92)
- [I2C busz kommunikációs hiba kijelzése ........................................................ 39](#_page38_x68.00_y430.92)

[Összegzés ................................................................................................................................. 40 ](#_page39_x68.00_y70.92)[Webográfia ............................................................................................................................... 41](#_page40_x68.00_y70.92)

<a name="_page5_x68.00_y70.92"></a>Bevezetés 

Az intelligens otthon olyan hardver és szoftver megoldások együttese, amelyek segítségével automatikusan, azaz emberi beavatkozás nélkül, vagy csak minimális emberi beavatkozással képes  ellátni  olyan  feladatokat,  melyek  az  emberek  mindennapi  életét  kényelmesebbé, biztonságosabbá,  takarékosabbá  teszik.  Megkülönböztetünk  központi,  elosztott,  valamint összetett intelligenciájú rendszereket. Ezen záródolgozat egy központi intelligenciájú rendszer valósít meg, melyben egy mikrovezérlő dolgozza fel a hozzá beérkező jeleket és vezérli az otthon eszközeit (fűtés-hűtés, világítás…). A technológia fejlődésével egyre nagyobb tudású és egyre  olcsóbb  okos  otthonvezérlések  készülnek,  melyek  egyre  több  háztartásban  válnak elérhetővé, ezért is választottam záródolgozatom témájául az intelligens otthon vezérlést. 

<a name="_page6_x68.00_y70.92"></a>Implementációs leírás 

Windows 10: A Windows 10 operációs rendszert használtam záródolgozatom elkészítésnél, mert ez az operációs rendszer kompatibilis az alábbiakkal: 

- MPLAB X IDE v5.45-vel, mely a PIC mikrovezérlő programok készítéséhez ideális fejlesztőkörnyezet, magába foglal 
  - egy  szövegszerkesztőt,  mely  segítségével  lehet  beírni  és  formázni  a forrásprogramot 
  - C32 fordítóprogramot (C nyelven megírt programot fordítja le gépi kódra, mely betöltésre kerül a mikrovezérlőbe) 
  - és a hibák felderítéséhez egy hibakeresőt. 
- ICD4 áramköri programozó és hibakereső eszközzel: a lefordított program áttöltéséhez, 
- Notepad++: a HTML, CSS, Java script forráskódok megírásához. 

<a name="_page7_x68.00_y70.92"></a>FELHASZNÁLÓI DOKUMENTÁCIÓ <a name="_page7_x68.00_y110.92"></a>Bevezetés 

Az  intelligens  otthon  segítségével  az  épület  villamos,  gépészeti,  informatikai,  és  egyéb rendszereinek  felügyelete  és  vezérlése,  valamint  az  intelligens  megoldások  segítségével  a következő alrendszerek válnak elérhetővé: 

- fűtés – hűtés 
- biztonságtechnika 
- világítás 
- öntözőrendszer 
- redőny, reluxa, kapu 
- szauna és egyéb kényelmi berendezések 

<a name="_page7_x68.00_y345.92"></a>Szimuláció 

A záródolgozatomban egy szimulációs intelligens otthonvezérlést mutatok be, mely két fő egységből áll: 

<a name="_page7_x68.00_y443.92"></a>Irányító panelből: az irányító panelen található a mikrovezérlő, mely az egész ház vezérlését ellátja. Fogadja a külön fajta érzékelők jeleit és ezek alapján eldönti, hogy milyen parancsokat adjon ki a beavatkozó egységeknek (pl. padlófűtés be-kikapcsolása). Internet böngészőn keresztül értesül a felhasználó parancsairól, mely internet segítségével történhet bárhonnan. 

![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.001.png)

1. *ábra* 

   *Irányító panel*

<a name="_page8_x68.00_y70.92"></a>Kísérleti  panelből:  a  kísérleti  panelen  találhatók  meg  az  érzékelő  (hőmérsékletérzékelő), beavatkozó elemek (fűtésvezérlést szimuláló zöld színű kis lámpák) és érintőkapcsolók (lámpa – helyiségvilágítás ki-bekapcsolás), melyek egy valós kiépítésben a ház különböző pontjain vannak elhelyezve. Ezek az elemek látják el adatokkal a mikrovezérlőt. 

![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.002.jpeg)

2. *ábra* 

*Kísérleti panel* 

<a name="_page9_x68.00_y70.92"></a>Alrendszerek

Záródolgozatomban a következő Alrendszerek lettek megvalósítva: 

- 4 szobahőmérséklet kijelzése 
- külső hőmérséklet kijelzése 
- a szobák egyenkénti hőmérséklet beállítása 
- fűtésvezérlés (a fejlesztő panelon kicsi zöld lámpák(ledek) mutatják a fűtés állapotát) 
- szobák világításának vezérlése, állapot ellenőrzése 

<a name="_page9_x68.00_y264.92"></a>Platform 

Az intelligens otthon működéséhez program telepítésére nincs szükség. A program bármely modern  internetes  böngészővel  használható  számítógépen,  tableten,  okos  telefonon.  Az alkalmazást a böngésző címsorába a[ http://iotthon ](http://iotthon/)beírásával érhetjük el. 

<a name="_page9_x68.00_y381.92"></a>Otthonunk aktuális jellemzői 

A[ http://iotthon ](http://iotthon/)beírását követően láthatóvá válnak otthonunk aktuális jellemzői: 

- a szobák hőmérsékletei 
- a külső hőmérséklet 
- a szobák beállított (kívánt) hőmérsékletei 
- a szobák lámpáinak állapotai 

<a name="_page9_x68.00_y538.92"></a>Szobák hőmérsékletének kijelzése 

A szobahőmérséklet mérésének ellenőrzéséhez meg kell érinteni a megfelelő hőfokérzékelő (ha a weblapon 2. helyiség hőmérsékletét szeretnénk ellenőrizni, akkor a 2. helyiség hőmérséklet érzékelőjét  kell  megérinteni)  fekete  tetejét,  mely  érzékeli  a  testünk  melegét.  A  weblapon látható, hogy a hőfok emelkedik, ha elengedjük a hőfokérzékelő tetejét, a hőfok csökken. 

![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.003.jpeg)

3. *ábra* 

*Intelligens otthon -[ http://iotthon* ](http://iotthon/)*

<a name="_page10_x68.00_y472.92"></a>Külső hőmérséklet kijelzése 

A külső-hőmérsékletérzékelő ténylegesen kiépítésre került, mint egy valós intelligens otthon vezérlésnél  és  mutatja  a  külső  környezeti  hőmérsékletet.  Amennyiben  a  külső  érzékelő meghibásodik, vagy valamilyen kommunikációs probléma adódik (pl. vezetékszakadás), akkor a weblapon a hibás érzékelőhöz tartozó kijelzés háttere zöld színről pirosra változik, a kijelzett érték helyett pedig kérdőjelek láthatóak. 

![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.004.jpeg)

4. *ábra* 

*Külső hőmérséklet kijelzése* 

<a name="_page11_x68.00_y483.92"></a>Szobák hőmérsékletének beállítása 

A helyiségeknek nemcsak a hőmérsékletét tudjuk mérni, hanem lehetőség van a helyiségek hőmérséklet beállítására is. Jelen vezérlés csak fűtés vezérlésére van kidolgozva, de minimális programozással  képes  lenne  a  helyiségek  hűtésének  a  vezérlésére  is  (ebben  az  esetben természetesen a háznak rendelkezni kell klímaberendezéssel is). 

<a name="_page11_x68.00_y612.92"></a>Fűtés működésének ellenőrzése 

A szobák fűtése működésének ellenőrzéséhez a beállított hőfokot állítsuk a mért hőfok fölé  3-4  oC-kal.  Ez  úgy  tudjuk  megtenni,  hogy  a  weblapon  kiszemelt  helyiség  hőmérséklet kijelzésére kattintunk az egérrel. Ezek után egy felhasználói név és jelszó bekérő ablak ugrik fel.  

<a name="_page12_x68.00_y70.92"></a>*Belépés* 

Ezzel elérjük, hogy csak a jogosult személyek tudják a helyiségek beállított hőmérsékletét módosítani. Példánkban a felhasználói név és a jelszó is az „admin”. 

![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.005.png)

5. *ábra* 

*Felhasználói név és jelszó bekérő ablak* 

<a name="_page12_x68.00_y359.92"></a>*Beállított hőmérséklet láthatósága* 

Miután  beírtuk  a  felhasználói  nevet  és  a  jelszót,  a  bejelentkezés  gomb  megnyomásával visszajutunk  az  előző  weblapra,  de  most  már  ha  a  hőfok  kijelzésre  kattintunk  a  felugró ablakban, be tudjuk írni a kiválasztott helyiség kívánt hőfokát. A beállított kívánt hőmérséklet láthatóvá válik, ha a weblapon a kiválasztott helyiség hőmérséklet kijelzésére húzzuk az egeret. 

![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.006.jpeg)

6. *ábra* 

*A beállított kívánt hőmérséklet láthatóvá válik* 

<a name="_page14_x68.00_y70.92"></a>*Hőfokbeállítás* 

![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.007.jpeg)

7. *ábra* 

*Hőfokbeállítás* 

Tehát ha a kiválasztott helyiség kívánt hőfokát pl. 3 0C-al magasabbra állítjuk, mint a helyiség mért hőfoka, akkor a vezérlés kiadja a fűtésnek (gázkazán, padlófűtés, hűtő-fűtő klíma) hogy kezdjen el fűteni. A kísérleti panelen ezt a kiválasztott helyiséghez tartozó kicsi zöld lámpa jelzi. A kiválasztott helyiség érzékelőjét megérintjük ezzel azt szimulálva, hogy elindult a fűtés és melegszik a helyiség hőmérséklete. Ha a hőmérséklet eléri ill. meghaladja a kívánt hőfokot a fűtés leáll, ezt a kiválasztott helyiség fűtés zöld színű lámpájának kikapcsolása jelzi. Ha most elengedjük az érzékelőt, akkor a hőmérséklet elkezd csökkenni. Ha a hőmérséklet a kívánt hőfok alá esik 2  0C-al, akkor a fűtés ismét bekapcsol (világitani kezd a hozzá tartozó zöld lámpa). Látható, hogy a fűtés ki és bekapcsolási hőmérséklete között 2 C van. Ezt hiszterézisnek nevezzük.  Ez  azért  kell,  mert  a  hőmérsékletmérés  értéke  ingadozik,  és  ha  nem  lenne  a hiszterézis beépítve a hőfokszabályozásba, akkor ez az ingadozás egy sűrű ki-be kapcsolgatást eredményezne  a  fűtés  vezérlésénél,  mely  károsíthatja  a  beavatkozó  ezközeinket  (kazán, hőszivattyú). 

<a name="_page15_x68.00_y70.92"></a>*Fűtésvezérlés* (a fejlesztő panelon kicsi zöld lámpák (ledek) mutatják a fűtés állapotát) 

Mind a négy helyiséghez tartozik egy-egy fűtés vezérlésére szolgáló kimenet. Ez a tényleges megvalósításnál legtöbbször relé vagy mágneskapcsoló szokott lenni. De itt, a szimulációban, ha a zöld lámpa világit, az azt jelenti, hogy a fűtőegység be van kapcsolva, ha nem világít, akkor a fűtőegység kikapcsolt állapotban van. 

![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.008.jpeg)

8. *ábra* 

   *Fűtésvezérlés* 

<a name="_page15_x68.00_y505.92"></a>*Kilépés* 

Kilépéshez be kell zárni a böngészőt, és ha újból állítani akarjuk a hőmérsékletet, ismét meg kell adni a bejelentkezési adatokat. 

<a name="_page15_x68.00_y607.92"></a>*Szobák lámpáinak állapotai* 

Intelligens otthon segítségével lekérdezhetőek a helyiségekben lévő lámpák állapotai (világít, nem világít. A szimuláció miatt szükséges a lámpák állapotának változtatására. Ezekhez nyújt segítséget  a  kísérleti  panelon  található  4db  érintőkapcsoló.  Ezek  a  kapcsolók  a  szobák villanykapcsolóit hivatottak szimulálni. Mind a 4 helyiséghez tartozik egy-egy érintőkapcsoló. 

![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.009.png)

9. *ábra* 

*Szobák lámpáinak állapotai* 

Az érintőkapcsolók is kijelzik a lámpák fel-le kapcsolt állapotát. Az érintőkapcsolók érintésével a lámpa fel, ill. lekapcsolódik. A weblapon zöldre vált a lámpát szimbolizáló kör, ha a lámpa fel van kapcsolva. A kísérleti panelon az érintőkapcsoló pirosan világít, ha a hozzá tartozó helyiség lámpa fel van kapcsolva. 

![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.010.jpeg)

10. *ábra* 

*Szobák lámpáinak állapotai* 

<a name="_page17_x68.00_y70.92"></a>FEJLESZTŐI DOKUMENTÁCIÓ 

A záródolgozatom két részből áll: 

- Hardver:  az  intelligens  otthon  megvalósításához  mindig  kell  egy  valamilyen programozható  eszköz,  egy  számítógép,  PLC,  mikrovezérlő.  Én  a  Mikrochip  által gyártott PIC mikrovezérlő választásával készítettem el a feladatot. A hardver áll egy PIC32 Ethernet Starter kittből és egy prototípus fejlesztő panelből, amely segítségével gyorsan össze lehet állítani az áramköröket. A Starter kitten található a PIC 32 bites mikrovezérlő,  webszerver,  Ethernet  port  és  a  működéshez  szükséges  elektronikai elemek. A fejlesztő panelen kaptak helyet a hőmérséklet érzékelők, érintőkapcsolók, OLED kijelző, valamint a fűtés állapotát mutató ledek. 
- Szoftver:  a  mikrovezérlő  nem  működhet  program  nélkül.  Munkám  során  több programnyelvet is érintettem. A mikrovezérlőt C nyelven lehet programozni. A web szerver elkészítéséhez szükséges volt a HTML, CSS, Java nyelvek használata. 

<a name="_page17_x68.00_y378.92"></a>Hardver 

<a name="_page17_x68.00_y416.92"></a>Mikrovezérlő 

Az  általam  választott  mikrovezérlőt  jellemzően  beágyazott  rendszerekben  egy  célfeladat elvégzésére  használják.  Ezek  a  mikrovezérlők  egyazon  Chip-en  elhelyezett mikroprocesszorból,  adat  és  program  memóriából,  perifériából  és  egyéb  kiegészítő áramkörökből állnak. Ezen áramkörök összesége olyan kompakt vezérlőmodult alkot, melynek működéséhez csak néhány külső alkatrész szükséges. Másik nagy előnyük a mai technikai szinttel tömeggyártásban nagyon olcsón előállíthatók. 

A PIC32 Ethernet Starter kitt webes alkalmazások fejlesztéséhez nyújt segítséget. A panelen található Ethernet vezérlő, 3 db nyomógomb, 3db led, ami szabadon programozható, valamint egy bővítő kártya segítségével ki van vezetve egy csatlakozóra a mikrovezérlő kivezetései.  

![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.011.png)

11. *ábra* 

<a name="_page18_x68.00_y242.92"></a>Prototípus fejlesztő panel 

Ezen a panelen lettek elhelyezve periféria elmei. Ezen elemek mindegyike I2C buszon tartja a kapcsolatot  a  mikrovezérlővel.  A  buszon  lévő  elemeket  mind  egyedi  címet  kapnak,  ezek felhasználásával szólítja meg azokat ciklikusan a mikrovezérlő. 

<a name="_page18_x68.00_y352.92"></a>*Az I2C busz elemei*  

<a name="_page18_x68.00_y410.92"></a>N75 hőmérséklet érzékelő 

I2C buszon lekérdezhető digitális hőmérséklet szenzor. -40 0C és +150 0C tartományban mér 1 0C pontosággal. 

<a name="_page18_x68.00_y492.92"></a>Távoli 8 bites I / O bővítő 

I2C  buszon  kommunikáló,  8  db  egyenként  bemenetként  vagy  kimenetként programozható I/O bővítő. Alsó négy bit bemenetként van beállítva és négy érintő nyomógomb van rákötve, a felső négy bit kimenetként van beállítva és négy led-et működtet (fűtés vezérlés). 

<a name="_page18_x68.00_y620.92"></a>OLED kijelző 

i2C buszon vezérelhető grafikus kijelző, melyen a DHCP szervertől kapott IP cím olvasható. 

![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.012.jpeg)

12. *ábra* 

<a name="_page19_x68.00_y523.92"></a>I2C kommunikáció 

Kétvezetékes  szinkron  adatátviteli  rendszer,  a  Philips  által  kidolgozott  áramkörök összekapcsolására. Két adatvonal közül az egyik az órajel (SCL), a másik vonalon az adatok mennek (SDA). A mostani alkalmazásnál egy master és több slave van a buszon. Mindig a master indítja a kommunikációt, a slave-k figyelik a buszt. Ha a slave a saját címét látja a buszon, csak akkor válaszol. A master a mikrovezérlő a slave-k pedig a perifériák (hőfok szenzor, OLED kijelző…). A mester ciklikusan (1s) szólítja meg a slave-eket címük szerint. 

<a name="_page20_x68.00_y70.92"></a>*I2C buszon lévő eszközök* 

![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.013.jpeg)

13. *ábra* 

<a name="_page20_x68.00_y345.92"></a>Szoftver 

<a name="_page20_x68.00_y383.92"></a>TCP/IP kommunikáció 

TCP/IP kommunikációhoz a Microchip által elkészített szabadon felhasználható TCP/IP Stack v5.42.08 verzió függvényeit használtam fel. Ezek a függvények megvalósítják az OSI modell rétegeit. 

<a name="_page20_x68.00_y497.92"></a>*Felhasznált függvények* 

- BYTE HTTPCheckAuth(BYTE\* cUser, BYTE\* cPass) 
- HTTP\_IO\_RESULT HTTPExecuteGet(void) 
- void HTTPPrint\_led(WORD num) 
- void HTTPPrint\_hofok1(void) 
- void HTTPPrint\_hofok2(void) 
- void HTTPPrint\_hofok3(void) 
- void HTTPPrint\_hofok4(void) 
- void HTTPPrint\_BeallHo1(void) 
- void HTTPPrint\_BeallHo2(void) 
- void HTTPPrint\_BeallHo3(void) 
- void HTTPPrint\_BeallHo4(void) 

<a name="_page21_x68.00_y98.92"></a>Intelligens otthon vezérlés 

Ezen programrész végzi a tényleges működést.  

<a name="_page21_x68.00_y171.92"></a>*Feladatai* 

<a name="_page21_x68.00_y229.92"></a>Hőmérséklet szenzoroktól érkező adatok konvertálása 

A hőmérsékletet a következő formátumban kapja meg a mikrovezérlő: 



||**7. bit** |**6. bit** |**5. bit** |**4. bit** |**3. bit** |**2. bit** |**1. bit** |**0. bit** |
| :- | - | - | - | - | - | - | - | - |
|MSB |Sign |<p>6 </p><p>2 °C </p>|<p>5 </p><p>2 °C </p>|<p>4 </p><p>2 °C </p>|23 °C |<p>2 </p><p>2 °C </p>|21 °C |20 °C |
|LSB |<p>-1</p><p>2 °C </p>|0 |0 |0 |0 |0 |0 |0 |

*7. ábra* 

A legnagyobb helyiértékű bájton (MSB) a hőmérséklet egész része található, amiből a 7. bit az előjel. A legkisebb helyiértékű bájt (LSB) 7. bitje tartalmazza a nem egész részt. 

float convertTemp(UINT8 aa, UINT8 bb) ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.014.png)

{ 

`    `float temp; 

`    `if (aa>128)            // negatív szám 

`    `{ 

`        `temp=((255-(float)aa)\*-1); 

`        `if (bb==128)         // 0.5 fok kivonása 

`        `{ 

`            `temp-=0.5; 

`        `} 

`    `} 

`    `else                   // pozitív szám 

`    `{ 

`        `temp=(float)aa; 

`        `if (bb==128)         // 0.5 fok hozzáadása         { 

`            `temp+=0.5; 

`        `} 

`    `} 

`    `return(temp); 

} 

A fenti függvény 2 bemenő UINT8 típusú (8 bites előjel nélküli integer) paramétere és egy float típusú eredménnyel tér vissza. Létrehozunk egy float típusú „temp” nevű változót. A program ellenőrzi az „aa” paraméter alapján, hogy a mért hőmérséklet negatív-e.  

Ha negatív, akkor az értéket konvertáljuk float típusúvá, kivonjuk 255-ből és megszorozzuk - 1-el. A kapott értéket a „temp” változóba tároljuk. Ha a „bb” paraméter 7. bitje magas, akkor a temp értékéből kivonunk 0.5-öt. 

Ha az „aa” értéka pozitív, akkor konvertáljuk float típusúvá és eltároljuk a „temp” változóba. Ha a „bb” paraméter 7. bitje magas, akkor a „temp” értékéhez hozzáadunk 0.5-öt. 

A „temp” változó értékét átadjuk a függvény visszatérési értékének. 

<a name="_page23_x68.00_y70.92"></a>A beállított és mért hőfok alapján a fűtés vezérlése 

// 1. helyiség fűtés vezérlés ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.015.png)

`            `if (inhofok1<=BeallHo1-2.0) 

`                `AdatIO\_out=AdatIO\_out & 0x7F; // Fűtés bekapcsolás             if (inhofok1>=BeallHo1) 

`                `AdatIO\_out=AdatIO\_out | 0x80; // Fűtés kikapcsolás 

`            `// 2. helyiség fűtés vezérlés 

`            `if (inhofok2<=BeallHo2-2.0) 

`                `AdatIO\_out=AdatIO\_out & 0xBF; // Fűtés bekapcsolás             if (inhofok2>=BeallHo2) 

`                `AdatIO\_out=AdatIO\_out | 0x40; // Fűtés kikapcsolás 

`            `// 3. helyiség fűtés vezérlés 

`            `if (inhofok3<=BeallHo3-2.0) 

`                `AdatIO\_out=AdatIO\_out & 0xDF; // Fűtés bekapcsolás             if (inhofok3>=BeallHo3) 

`                `AdatIO\_out=AdatIO\_out | 0x20; // Fűtés kikapcsolás 

`            `// 4. helyiség fűtés vezérlés 

`            `if (inhofok4<=BeallHo4-2.0) 

`                `AdatIO\_out=AdatIO\_out & 0xEF; // Fűtés bekapcsolás             if (inhofok4>=BeallHo4) 

`                `AdatIO\_out=AdatIO\_out | 0x10; // Fűtés kikapcsolás 

`            `// Adat kiküldésa I2C buszon 

`             `I2C2\_PCF8574\_W(0x21,AdatIO\_out); 

A következő programrész beállított és mért hőmérsékletek alapján vezérli a fűtést.  

A fűtés akkor kapcsol be, ha mért hőmérséklet alacsonyabb vagy egyenlő mint a beállított hőfokból -2.0-vel csökkentett érték. A -2 a hiszterézis, így a bekapcsolás és kikapcsolási érték között 2.0 C lesz. A hiszterézis megakadályozza a kimenet túl gyakori kapcsolgatását. 

Ha a mért hőmérséklet érték egyenlő vagy nagyobb, mint a beállított hőmérséklet a fűtés kikapcsol. 

Minden szobához tartozik egy fűtés bekapcsolás és fűtés kikapcsolás „IF” utasítás. A szobák fűtés vezérléséhez „AdatIO\_out” nevű változó felső 4 bitje tartozik. Ha az adott helyiség bitje 1-ben van a fűtés bekapcsol, ha a bit 0 értékű a fűtés kikapcsol. 

- 1. helyiség: 7. bit 
- 2. helyiség: 6. bit 
- 3. helyiség: 5. bit 
- 4. helyiség: 4. bit 

` `A programrész utolsó sorában „AdatIO\_out” nevű változó értéke el lesz küldve I2C buszon a 0x21 címen lévő 8bites I/O bővítőnek. 

<a name="_page24_x68.00_y118.92"></a>I2c kommunikáció vezérlése, kommunikációs hiba detektálása 

inhofok1=I2C2\_N75(0x49);  //0x49; 0b1001001; inhofok2=I2C2\_N75(0x4A);  //0x4A; 0b1001010; inhofok3=I2C2\_N75(0x4B);  //0x4B; 0b1001011; inhofok4=I2C2\_N75(0x4C);  //0x4C; 0b1001100; ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.016.png)

Ezen a főprogramból kivett részletben történik a 4 db hőmérséklet érzékelő lekérdezése I2C buszon keresztül. A függvények visszatérő értékei tartalmazzák a hőfok érzékelők által mért hőmérséklet konvertált értékeit. Kommunikációs hiba esetén az adott hőmérséklet érzékelő visszatérési értéke 255.0 lesz. 

<a name="_page25_x68.00_y70.92"></a>N75 hőmérséklet szenzor mért hőmérséklet lekérdezésének függvénye: 

float I2C2\_N75(UINT8 Address) { ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.017.png)

`    `UINT8 Pointer = 0x00; 

`    `UINT8 I2Cadat1; 

`    `UINT8 I2Cadat2; 

`    `int Timeout; 

`    `if (I2CBusIsIdle(I2C2)) 

`    `{     

`        `I2CStart(I2C2); 

`        `while(I2C2CONbits.SEN); 

`        `//Hőmérséklet sensor címének kiküldése, Irás 

`        `I2CSendByte(I2C2,(Address << 1)|I2C\_WRITE); 

`        `while(I2C2STATbits.TRSTAT); 

`        `if (I2CByteWasAcknowledged(I2C2)) 

`        `{ 

`            `//Hőmérséklet sensor Pointer kiküldése, Irás 

`            `I2CSendByte(I2C2, Pointer|I2C\_WRITE); 

`            `while(I2C2STATbits.TRSTAT); 

`            `if (I2CByteWasAcknowledged(I2C2)) 

`            `{ 

`                `//Restart 

`                `I2CRepeatStart(I2C2); 

`                `while(I2C2CONbits.RSEN); 

`                `//Hőmérséklet sensor címének kiküldése, Olvasás 

`                `I2CSendByte(I2C2,(Address << 1)|I2C\_READ); 

`                `while(I2C2STATbits.TRSTAT); 

`                `if (I2CByteWasAcknowledged(I2C2)) 

`                `{ 

`                    `//1. adat byte vétele 

`                    `I2CReceiverEnable(I2C2, TRUE); 

`                    `Timeout = 0; 

`                    `while(!I2CReceivedDataIsAvailable(I2C2) & Timeout<5000)                     { 

`                        `Timeout++; 

`                    `} 

`                    `I2Cadat1 = I2CGetByte(I2C2); 

`                    `//ACK 

`                    `I2CAcknowledgeByte(I2C2,true); 

`                    `Timeout = 0; 

`                    `while(I2C2CONbits.ACKEN & Timeout<5000) 

`                    `{ 

`                        `Timeout++; 

`                    `} 

`                    `//2. adat byte vétele 

`                    `I2CReceiverEnable(I2C2, TRUE); 

`                    `Timeout = 0; 

`                    `while(!I2CReceivedDataIsAvailable(I2C2) & Timeout<5000)                     { 

`                        `Timeout++; 

`      `}    ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.018.png)

`                    `I2Cadat2 = I2CGetByte(I2C2); 

`                    `//NOACK 

`                    `I2CAcknowledgeByte(I2C2,false); 

`                    `Timeout = 0; 

`                    `while(I2C2CONbits.ACKEN & Timeout<5000)                     { 

`                        `Timeout++; 

`                    `}          

`                    `Delay10us(20); 

`                    `// send a STOP: 

`                    `I2CStop(I2C2); 

`                    `while(I2C2CONbits.PEN); 

`                    `return(convertTemp(I2Cadat1, I2Cadat2));                  }   

`                `else 

`                `{ 

`                    `Delay10us(20); 

`                    `I2CStop(I2C2); 

`                    `while(I2C2CONbits.PEN); 

`                    `return(255); 

`                `} 

`            `} 

`            `else 

`            `{ 

`                `Delay10us(20); 

`                `I2CStop(I2C2); 

`                `while(I2C2CONbits.PEN);                 return(255); 

`            `}         } 

`        `else         { 

`            `Delay10us(20); 

`            `I2CStop(I2C2); 

`            `while(I2C2CONbits.PEN);             return(255); 

`        `} 

`    `} 

`    `else 

`    `{ 

`        `return(255); 

`    `}    

} 

<a name="_page27_x68.00_y70.92"></a>N75 hőmérséklet szenzor lekérdezés folyamatának részletezése: 

- Startbit kiküldése a buszra, várakozás a művelet befejezéséig: 

I2CStart(I2C2); while(I2C2CONbits.SEN); ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.019.png)

- Szenzor címének kiküldése a buszra, írásmód, várakozás a művelet befejezéséig: 

I2CSendByte(I2C2,(Address << 1)|I2C\_WRITE); while(I2C2STATbits.TRSTAT); ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.020.png)

- Szenzor  belső  pointerének  kiküldése  a  buszra,  írásmód,  várakozás  a  művelet befejezéséig: 

I2CSendByte(I2C2, Pointer|I2C\_WRITE); while(I2C2STATbits.TRSTAT); ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.021.png)

- Restart bit kiküldése a buszra, várakozás a művelet befejezéséig: 

I2CRepeatStart(I2C2); while(I2C2CONbits.RSEN); ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.022.png)

- Szenzor címének kiküldése a buszra, olvasásmód, várakozás a művelet befejezéséig: 

I2CSendByte(I2C2,(Address << 1)|I2C\_READ); while(I2C2STATbits.TRSTAT); ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.023.png)

- Vételi üzemmódba kapcsolás: I2CReceiverEnable(I2C2, TRUE); ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.024.png)
- Várakozás az MSB bájt vételére, beírása az „I2Cadat1” változóba, ha nem érkezik meg késleltetés után kilépés a várakozásból: 

Timeout = 0; ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.025.png)

while(!I2CReceivedDataIsAvailable(I2C2) & Timeout<5000) { 

Timeout++; 

` `} 

`                    `I2Cadat1 = I2CGetByte(I2C2); 

- ACK  bit  küldése  a  buszra,  várakozás  a  slave  nyugtázására,  ha  nem  érkezik  meg késleltetés után kilépés a várakozásból: 

`       `I2CAcknowledgeByte(I2C2,true); ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.026.png)

`                    `Timeout = 0; 

`                    `while(I2C2CONbits.ACKEN & Timeout<5000)                     { 

`                        `Timeout++; 

`                    `} 

- Várakozás az LSB bájt vételére, beírása az „I2Cadat2” változóba, ha nem érkezik meg késleltetés után kilépés a várakozásból: 

`     `Timeout = 0; ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.027.png)

`                    `while(!I2CReceivedDataIsAvailable(I2C2) & Timeout<5000)                     { 

`                        `Timeout++; 

`                    `}    

`                    `I2Cadat2 = I2CGetByte(I2C2); 

- NACK bit küldése a buszra, várakozás a slave nyugtázására, ha nem érkezik meg késleltetés után kilépés a várakozásból 

`     `I2CAcknowledgeByte(I2C2,false); ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.028.png)

`                    `Timeout = 0; 

`                    `while(I2C2CONbits.ACKEN & Timeout<5000)                     { 

`                        `Timeout++; 

`                    `} 

- Stop bit küldése a buszra, várakozás a művelet befejezésére 

I2CStop(I2C2); while(I2C2CONbits.PEN); ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.029.png)

<a name="_page30_x68.00_y70.92"></a>8 bites I/O bővítő írása, olvasása 

<a name="_page30_x68.00_y92.92"></a>I/O bővítő olvasás függvény** forráskódja: 

int I2C2\_PCF8574\_R(UINT8 Address) { ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.030.png)

`    `UINT8 Pointer = 0x00; 

`    `UINT8 I2Cadat; 

`    `int Timeout; 

`    `if (I2CBusIsIdle(I2C2)) 

`    `{     

`        `I2CStart(I2C2); 

`        `while(I2C2CONbits.SEN); 

`        `//Eszköz címének küldése 

`        `I2CSendByte(I2C2,(Address << 1)|I2C\_READ); 

`        `while(I2C2STATbits.TRSTAT); 

`        `if (I2CByteWasAcknowledged(I2C2)) 

`        `{ 

`            `//Vételi üzemmód 

`            `I2CReceiverEnable(I2C2, TRUE); 

`            `Timeout = 0; 

`            `//Adatbyte vétele    

`            `while(!I2CReceivedDataIsAvailable(I2C2) & Timeout<5000)             { 

`                `Timeout++; 

`            `} 

`            `I2Cadat = I2CGetByte(I2C2); 

`            `Delay10us(20); 

`            `//Stop bit küldése 

`            `I2CStop(I2C2); 

`            `while(I2C2CONbits.PEN);             return(I2Cadat); 

`        `}   

`        `else 

`        `{ 

`            `Delay10us(20); 

`            `I2CStop(I2C2); 

`            `while(I2C2CONbits.PEN);             //Hiba 

`            `return(256); 

`        `} 

`    `} 

`    `else 

`    `{ 

`        `//Hiba 

`        `return(256);     }    

} 

I/O bővítő olvasás függvény forráskódjának részletezése: 

- Startbit kiküldése a buszra, várakozás a művelet befejezéséig: 

I2CStart(I2C2); while(I2C2CONbits.SEN); ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.031.png)

- Szenzor címének kiküldése a buszra, olvasásmód, várakozás a művelet befejezéséig: 

I2CSendByte(I2C2,(Address << 1)|I2C\_READ); while(I2C2STATbits.TRSTAT); ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.032.png)

- Vételi üzemmódba kapcsolás: I2CReceiverEnable(I2C2, TRUE); ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.033.png)
- Várakozás az adatbájt vételére, beírása az „I2Cadat” változóba, ha nem érkezik meg késleltetés után kilépés a várakozásból: 

`            `Timeout = 0; ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.034.png)

`            `while(!I2CReceivedDataIsAvailable(I2C2) & Timeout<5000)             { 

`                `Timeout++; 

`            `} 

`            `I2Cadat = I2CGetByte(I2C2); 

- Stop bit küldése a buszra, várakozás a művelet befejezésére 

I2CStop(I2C2); while(I2C2CONbits.PEN); ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.035.png)

<a name="_page32_x68.00_y70.92"></a>I/O bővítő írás függvény** forráskódja:** 

int I2C2\_PCF8574\_W(UINT8 Address, UINT8 adat) { ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.036.png)

`    `UINT8 Pointer = 0x00; 

`    `UINT8 I2Cadat; 

`    `int Timeout; 

`    `if (I2CBusIsIdle(I2C2)) 

`    `{     

`        `I2CStart(I2C2); 

`        `while(I2C2CONbits.SEN); 

`        `//Eszköz címének küldése 

`        `I2CSendByte(I2C2,(Address << 1)|I2C\_WRITE); 

`        `while(I2C2STATbits.TRSTAT); 

`        `if (I2CByteWasAcknowledged(I2C2)) 

`        `{ 

`            `//adatbyte küldése 

`            `I2CSendByte(I2C2,adat); 

`            `Timeout = 0; 

`            `while(!I2CReceivedDataIsAvailable(I2C2) & Timeout<5000)             { 

`                `Timeout++; 

`            `} 

`            `Delay10us(20); 

`            `// Stop bit küldése 

`            `I2CStop(I2C2); 

`            `while(I2C2CONbits.PEN);             return(I2Cadat); 

`        `}   

`        `else 

`        `{ 

`            `Delay10us(20); 

`            `// Stop bit küldése 

`            `I2CStop(I2C2); 

`            `while(I2C2CONbits.PEN);             //Hiba 

`            `return(256); 

`        `} 

`    `} 

`    `else 

`    `{ 

`        `//Hiba 

`        `return(256);     }    

} 

I/O bővítő írás függvény forráskódjának részletezése: 

- Startbit kiküldése a buszra, várakozás a művelet befejezéséig: 

I2CStart(I2C2); while(I2C2CONbits.SEN); ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.037.png)

- Szenzor címének kiküldése a buszra, írásmód, várakozás a művelet befejezéséig: 

I2CSendByte(I2C2,(Address << 1)|I2C\_WRITE); while(I2C2STATbits.TRSTAT); ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.038.png)

- Adatbyte kiküldése, várakozás a slave nyugtázására, ha nem érkezik meg késleltetés után kilépés a várakozásból: 

`            `I2CSendByte(I2C2,adat); ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.039.png)

`            `Timeout = 0; 

`            `while(!I2CReceivedDataIsAvailable(I2C2) & Timeout<5000)             { 

`                `Timeout++; 

`            `} 

- Stop bit küldése a buszra, várakozás a művelet befejezésére 

I2CStop(I2C2); while(I2C2CONbits.PEN); ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.040.png)

<a name="_page33_x68.00_y541.92"></a>OLED kijelző vezérlése 

Az OLED kijelző a mostani feladatban egy funkciója van: a DHCP szervertől kapott IP cím megjelenítése. E nélkül nem tudnánk az intelligens otthon elérhetőségét. Lehetőség van statikus IP cím használatára is, ebben az esetben rá kell kapcsolódni az adott helyiség Ethernet hálózatra, az IP Cím ütközések elkerülése érdekében célszerű a dinamikus IP cím használata. 

Természetesen a kijelzőn bármilyen adat megjeleníthető, ami a mikrovezérlő birtokában van. (pl. hibaüzenetek, állapotok, hőmérsékletek…) 

Oled kijelzőt egy SSD1306 típusú grafikus chip kezeli. Kijelző vezérléséhez a Chip saját függvényeit és fontkészletét használtam fel. 

- Kijelző inicializálása, 1s-os késleltetés: 

OLED\_init2(); DelayMs(1000); ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.041.png)

- Képernyő törlése: 

OLED\_clear\_screen(); ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.042.png)

- Kurzor pozicionálása. A kiírás ebben a pozícióban (oszlop, sor) történik: OLED\_gotoxy(50,0); ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.043.png)
- String kiíratás: 

LED\_Write\_String("DHCP"); ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.044.png)

- Szám (integer) kiíratás. Szám kiíratás függvénynél bemenő paraméterként meg kell adni a kiírás pozícióját a kijelzőn, valamint a kiírandó számot, vagy változót. Esetünkben az „AppConfig.MyIPAddr.v[]” tömb tartalmazza a DHCP szervertől kapott IP címet. 

OLED\_print\_int(10,2,AppConfig.MyIPAddr.v[0]); OLED\_print\_int(35,2,AppConfig.MyIPAddr.v[1]); OLED\_print\_int(60,2,AppConfig.MyIPAddr.v[2]); OLED\_print\_int(85,2,AppConfig.MyIPAddr.v[3]); ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.045.png)

<a name="_page34_x68.00_y554.92"></a>Webszerver inicializálása, futtatása 

Itt amint már említettem a Microchip által elkészített függvényeket használtam fel. OSI modell elkészítése  és  magyarázata  meghaladná  e  záródolgozat  határait,  és  nem  is  az  Ethernetes kommunikáció elemzése volt a cél. Ezért használtam a Microchip által elkészített TCP/IP Stack v5.42.08 verzió függvényeit. 

1. webszerver inicializálás: 

StackInit(); ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.046.png)

2. webszerver stack meghívása 

StackTask(); ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.047.png)

<a name="_page35_x68.00_y143.92"></a>Weblap 

Itt egy viszonylag egyszerű weblap lett elkészítve a mikrovezérlő korlátos memóriája okán. Segédprogram  segítségével  a  weblapból  egy  image  fájl  készül,  mely  a  C-ben  megírt forrásprogrammal együtt fordítás után a mikrovezérlő memóriájába kerül. 

<a name="_page35_x68.00_y257.92"></a>*HTML, CSS* 

<!DOCTYPE html> <html> ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.048.png)

<head> 

<meta lang="hu" /> 

<meta charset="utf8" /> 

<link rel="stylesheet" type="text/css" href="okoso.css" /> <title> Intelligens Otthon </title> 

<script src="/mchp.js" type="text/javascript"></script> 

`              `</head> <body> 

<h1>Intelligens Otthon</h1> 

<div id="kep"> 

<img  id="haz" src="hatter.jpg "  /> 

<div id="pozk"><span id="hok">~hofokk~</span> °C</div> 

<a id="Btnho1" href="protect/pindex.htm" title=~BeallHo1~°C><span id="ho1">~hofok1~</span> °C</a> <a id="Btnho2" href="protect/pindex.htm" title=~BeallHo2~°C><span id="ho2">~hofok2~</span> °C</a> <a id="Btnho3" href="protect/pindex.htm" title=~BeallHo3~°C><span id="ho3">~hofok3~</span> °C</a> <a id="Btnho4" href="protect/pindex.htm" title=~BeallHo4~°C><span id="ho4">~hofok4~</span> °C</a> 

<span class="leds"> 

<a id="led0">&bull;</a> <a id="led1">&bull;</a> <a id="led2">&bull;</a> <a id="led3">&bull;</a> 

</span> 

</div> 

Html, CSS részletezése: 

- Kép<a name="_page36_x68.00_y97.92"></a> beillesztése 

<img  id="haz" src="hatter.jpg "  /> ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.049.png)

- Abszolút<a name="_page36_x68.00_y177.92"></a> pozicionálás, a kép szélessége 800 pixel, z-index:-1 hogy a hőfok kijelzéseket ne takarja el a ház képe. 

#haz{ ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.050.png)

position: absolute; width: 800px; z-index: -1; 

} 

- Belső<a name="_page36_x68.00_y332.92"></a> hőfok kijelzés 

Itt látható egy hiperhivatkozás, ami egy felhasználói név és jelszó beírás után egy védett oldalra visz. Ahol elérhetővé válik a helység hőmérséklet módosítása. Ha az egeret rávisszük valamelyik belső helyiség hőmérséklet kijelzésére láthatóvá válik a beállított hőmérséklet. A programrészletnél maradva az 1. helyiség beállított hőmérsékletét a ”~BeallHo1~” változó, a mért hőmérsékletet a ”~hofok1~” változó tartalmazza.  

<a id="Btnho1" href="protect/pindex.htm" title=~BeallHo1~°C><span id="ho1">~hofok1~</span> °C</a> ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.051.png)

- helyiség<a name="_page37_x68.00_y70.92"></a> hőfok kijelzés tulajdonságai 

A következő sorok a 1. helyiség hőfok kijelzés tulajdonságait mutatják: 

#Btnho1{ ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.052.png)

position: absolute; top: 150px; 

left: 180px; border-style: solid; border-width: 1px; border-radius: 5px; border-color: #000; padding: 2px; text-decoration:none; font-size: 12pt; 

} 

- Külső<a name="_page37_x68.00_y352.92"></a> hőfok kijelzés 

Nincs  hiperhivatkozás  mint  a  belső  hőfok  kijelzésnél  mivel  a  külső  környezeti hőmérsékletet nem tudjuk állítani csak mérni. 

<div id="pozk"><span id="hok">~hofokk~</span> °C</div> ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.053.png)

- Külső<a name="_page37_x68.00_y494.92"></a> hőmérséklet kijelzés jellemzőinek beállítása 

  A következő sorok állítják be a külső hőmérséklet kijelzés jellemzőit. 

#pozk { ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.054.png)

position: absolute; top: 190px; 

left: 750px; border-style: solid; border-width: 1px; border-radius: 5px; border-color: #000; padding: 2px; font-size: 12pt; 

} 

<a name="_page38_x68.00_y70.92"></a>*Java script* 

Java script segítségével történik a weboldalon lévő adatok dinamikus frissítése. A weboldalon az adatok 1 másodpercenként frissülnek.  

- Hőfok<a name="_page38_x68.00_y185.92"></a> adatok frissítése: 

function updateStatus(xmlData) { ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.055.png)

document.getElementById('ho1').innerHTML = getXMLValue(xmlData, 'ho1'); document.getElementById('ho2').innerHTML = getXMLValue(xmlData, 'ho2'); document.getElementById('ho3').innerHTML = getXMLValue(xmlData, 'ho3'); document.getElementById('ho4').innerHTML = getXMLValue(xmlData, 'ho4'); document.getElementById('hok').innerHTML = getXMLValue(xmlData, 'hok'); 

- Lámpák<a name="_page38_x68.00_y339.92"></a> (Ledek) állapotainak frissítése: 

for(i = 0; i < 4; i++) ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.056.png)

document.getElementById('led' + i).style.color = (getXMLValue(xmlData, 'led' + i) == '1') ? '#090' : '#ddd'; 

- I2C<a name="_page38_x68.00_y430.92"></a> busz kommunikációs hiba kijelzése: 

Ha valamelyik hőmérséklet érzékelővel megszakad a kapcsolat a kijelzés háttere zöldről pirosra vált, a kijelzett érték helyén kérdőjelek lesznek. A java script kód abból értesül, hogy kommunikációs  hiba van, hogy a mikrovezérlőtől kapott hőmérséklet értéke: 255.0. 

// Hő1 kommunikációs hiba ![](Aspose.Words.faa7fd56-d923-494e-9a59-624b22540f06.057.png)

if (getXMLValue(xmlData, 'ho1') == "255.0") 

{ 

document.getElementById("Btnho1").style.backgroundColor = "red"; document.getElementById('ho1').innerHTML = "??"; 

} 

else 

{ 

document.getElementById("Btnho1").style.backgroundColor = "#0f0"; } 

<a name="_page39_x68.00_y70.92"></a>Összegzés 

Egy  olyan  alaprendszert  –  kiemelten  a  világítás,  fűtés,  külső-belső  hőmérséklet  mérés  – készítettem, mely a későbbiekben továbbfejleszthető az újabb igényeknek eleget téve több elektronikus eszköz (riasztórendszer, öntözőrendszer ...) vezérlésével. 

<a name="_page40_x68.00_y70.92"></a>Webográfia 

Microchip's TCP/IP Stacks: [https://www.microchip.com/SWLibraryWeb/product.aspx?product=TCPIPSTACK ](https://www.microchip.com/SWLibraryWeb/product.aspx?product=TCPIPSTACK)
41 
