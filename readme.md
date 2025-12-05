## OBSAH
1. [Uvod / Pravidla, Priebeh kola, Ukoncenie kola atd.](#uvod--pravidla-priebeh-kola-ukoncenie-kola-atd)
2. [Technicka dokumentacia](#technicka-dokumentacia--triedy--metody-atd)
3. [Co som sa naucila, co bola vyzva ](#co-som-sa-naucila-co-bola-pre-mna-vyzva)
4. [Plany do buducnosti / Vylepsenia](#plan-s-chramst-do-buducnosti--vylepsenia)

## UVOD / Pravidla, Priebeh kola, Ukoncenie kola atd

**Chramst** je konzolova hra pre 4 hracov, kde cielom je prezit utoky zraloka. Hra existuje ako kartova hra, nebol to moj vymysel. Pravidla/hranie som musela prisposobit mojim programovacim schopnostiam a casu na vytvorenie projektu. Urcite je co vylepsovat, ale o planoch do buducnosti sa dozviete pod sekciou [Plany do buducnosti / Vylepsenia](#plan-s-chramst-do-buducnosti--vylepsenia) 

**Kde najst moj kod?**
mam tu ulozene 3 subory, relevantny je **chramst3.ipynb**
zvysne dva chramsty su viac menej zaznamy mojej strastiplnej cesty. Su tam rozne verzie od mojich zaciatkov, po testovanie, aj nejake chyby a rozne myslienkove pochody, napady a vsetky komentare co som potrebovala k prezitiu. 

### ℹ Zakladne informacie
- **pocet hracov:** 4 (Modry 🔵, Zlty 🟡, Zeleny 🟢, Cerveny 🔴)
- **karty:** Kazdy hrac zacina so siestimy kartami v hodnote od 1 az 6
- **Zivoty:** Kazdy hrac ma na zaciatku 4 Zivoty (1 zivot za kazdu koncatinu :D )
- **Pohyb v hre:** hraci sa hybu z miesta, co najdalej od zraloka, podla cisla zahrateho na karte
- **Koniec hry:** Hra konci, ked zostanu dvaja hraci

## 🎮 Pravidla hry

### Priebeh kola

**1. 🎴 Zahranie karty:** 
Hraci zacinaju s rovnakymi kartami a rovnakym poctom zivotov. Zacina Modry hrac, vyberie si jednu z kariet co ma na ruke a zahra ju. Toto zopakuju aj ostatni hrajuci hraci

**2. 🏅 Vyhodnotenie kola:** 
Karty sa zoraduju od najvyssej po najnizsiu
Hrac s najvyssou kartou ziskava 1.miesto
Ak dvaja alebo viaceri hraci zahrali rovnaku kartu tak sa z miesta nehybu, karty su "zablokovane" a zablokovani hraci si zachovavaju svoje povodne poradie

**3. 💀 Strata zivota:**
Hrac na poslednom mieste straca zivot
Ak hrac strati vsetky zivoty , vypadava z hry

**4. 🥈 Zmena poradia:**
Hrac, ktory stratil zivot sa posuva na prve miesto, a ostatnych hracov posuva o priecku nizsie
Ak nahodou vypadol jeden hrac pocas tohto kola, tak sa poradie hracov nemeni - nikto sa neposuva na prve miesto

**5. 🎴🎴🎴 Doplnenie kariet:**
Ak ma hrac menej ako 3 karty na ruke, doplni si karty zo svojho odhadzovacieho balicka, a v dalsom kole ma plny pocet zivotov



## TECHNICKA DOKUMENTACIA / Triedy , metody atd. 

### Trieda `Chramst`
Je to hlavna trieda, ktora obsahuje hlavnu logiku celej hry. 
Atributy v tejto triede:

```python
self.hraci = {
    "🔵 Modry": {
        "poradie": int,              
        "zivoty": int,               
        "karty na ruke": list,       
        "odhadzovaci balicek": list  
    },
    # ... ostatní hráči
}

self.karty_zahrane_v_kole = []  # Zoznam dvojic (karta, meno_hraca)
self.hra_bezi = True # Flag pre beh hry
```
### Metody

#### `__init__(self)`
Inicializuje novu hru - nastavuje hracov, ich zivoty a poradie

#### `zobraz_hracov(self)`
Vypise aktualny stav vsetkych hracov (poradie, zivoty, karty)
Tuto funkciu som viac pouzivala na testovanie/debugging, aby som videla ci vsetko bezi ako ma

#### `zahraj_kartu(self, meno_hraca)`
- hrac moze zahrat kartu (iba) ktoru ma na ruke


tato metoda v skratke:
- zistuje ci je vstup cislo (ak nie je, tak hraca poziada o opatovne zadanie karty)
- zistuje, ci hrac ma danu kartu na ruke (ak nie je, tak hraca poziada o opatovne zadanie karty)
- ak hrac napise do terminalu "koniec" , moze kedykolvek hru ukoncit 
- zobrazi konkretnemu hracovi karty, ktore ma na ruke (input)
- poziada o vyber karty (input)
- odstrani kartu z ruky hraca do odhadzovacieho balicku (kazdy hrac ma svoj  odhadzovaci balicek)
- ulozi zahranu kartu do `self.karty_zahrane_v_kole`

#### `vyhodnot_kolo(self)`
Vyhodnoti zahrate karty, urci nove poradie hracov:

**1. Identifikuje duplikaty**
- pouzity bol `Counter` (pre mna novinka), zisti ktore karty su duplikaty
- vytvori zoznam duplikatnych a neduplikatnych kariet

**2. Zoradenie neduplikatnych a duplikatnych kariet**
- neduplikatne karty zoraduje od najvyssej po najnizsiu a priradi im poradie
- duplikatne karty zoradi podla povodneho poradia hracov, a priradi poradie za neduplikatnymi kartami

**3. Vypise nove poradie hracov**
na zaklade vysledkov zo zoradenia hracov na zaklade duplikatnych/neduplikatnych kariet, vyprinti poradie hracov

#### `uber_zivot(self)`
odoberie zivot hracovi, ktory je na poslednom mieste

#### `posledny_na_prve(self)`
po vyhodnoteni kola a ubrati zivotov, posunie posledneho hraca na prve miesto 

#### `dopln_karty(self)`
ak maju hraci na ruke 2 karty, doplna kazdemu karty z odhadzovacieho balicka do ruky

#### `aktivni_hraci(self)`
ked zacne nove kolo, zistuje metoda, ktory hraci su este aktivni - maju viac ako 0 zivotov
vypadnutych hracov vymaze zo self.hraci (uz ich tam nepotrebujeme)

#### `hraj_kolo(self)`
Spusta kolo hry - kombinacia metod uvedenych vyssie:

1. vyprazdni `self.karty_zahrane_v_kole` 
2. zisti, ktori hraci su aktivni v danom kole
3. vypise aktualne poradie hracov
4. hraci zahraju kartu
5. Vyhodnotenie kola, urcenie poradia
6. odobratie zivota poslednemu hracovi
7. posun posledneho hraca na prve miesto
8. doplni karty hracom
9. zacina dalsie kolo od bodu 1

#### `play(self)`
Tak toto damy a pani, spusta celu hru!!! :

1. vypise pravidla ktore som v skratke dala do "printov"
2. spusta kolo po kole
3. ak zostanu uz len dvaja hraci, hra sa zastavi, a vypise meno vitaza


## CO SOM SA NAUCILA, CO BOLA PRE MNA VYZVA

no, tak tu by som sa vedela rozpisat, ale skusim to drzat strucne. Nauciala som sa vela, a vyzva bolo vsetko, kazdy jeden riadok kodu :D 

1. Velke obavy boli z `while` cyklu
2. Rozmyslat nad "problemamy" trochu inak:
- rozobrat ich na male kusky, aj ked som si myslela ze uz to rozobrane je, pocas testovania som zistila ze sa daju rozbit na este mensie a este mensie kusky. Bezne bolo ze som musela "dorabat" kusky kodu spatne, pridavat nove premenne atd. 
- aj najmensi problem moze byt ovela vacsi problem ako sa povodne ocakavalo, ale aj naopak, nieco s cim clovek si mysli ze bude bojovat, tak to nakoniec nebolo take zle
3. Treba nejak zacat. Ja som mala x pokusov zacat, az nakoniec som si povedala, ideme do `Dictionaries`. 
Uz ked som mala zadefinovanych hracov, prisla otazka ako dalej???
Na papiery som si rozne mozne metody napisala, a to som ani len netusila ze o kolko viac ich realne bude
4. Triedy: nevedela som si predstavit ako to tu cele spustim v ramci triedy
5. Vdaka tomuto projektu som zistla o existencii roznych kniznic (nie vsetky som tu aj pouzial, proste pri hladani odpovedi som nasla kopu zaujimavych veci, ktore som v projekte nevyuzila), funkciach, strankach.. 


## PLAN S "CHRAMST" DO BUDUCNOSTI / VYLEPSENIA

1. Urcite by som chcela hru rozsirit o povodne pravidla: 
- zapojit zraloka - bol by ako hrac (realny hrac/clovek si vyberie ci bude hrat za plavca alebo zraloka)
- hra je povodne pre 2-7 hracov - na zaklade poctu hracov sa menia aj jemne pravidla, hraci maju iny pocet kariet
- prve kolo: hraci by si vybrali farbu za aku chcu hrat,  a hodili by si napriklad kockou a urcili poradie v ktorom by zacinali v prvom kole, pripadne cez random by sa urcilo poradie v prvom kole

2.  moznost hrat hru ako jeden realny hrac (pripadne viaceri), ostatni hraci by boli "pocitac" alebo mozno aj AI? 
3.  dat tomu nejaky vizual, pouzit rozne kniznice aby sa to nemuselo hrat len cez terminal
4. prisposobit na mobil - mobilna appka, a potom to poslat kamosom a nutit ich sa so mnou hrat moj vytvor!
5. naucila som sa robit dokumentaciu v md formate a rozne vychytavkay ako:

- ked dame odrazku a medzeru tak mame ciernu gulicku 
- ked napiseme `` tak nam to da do ramceku slovo :
`slovo`
- mriezky nam robia nadpisy roznych velkosti:
# jedna mriezka
## dve mriezky

- da sa dat aj kus kody do bloku aby to malo spravne farby a bolo to oddelene od zvysku textu: ```python (pozri vyssie v metodach)

