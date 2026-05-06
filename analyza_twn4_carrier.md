# TWN4 MultiTech 3 BLE - carrier deska pro RS-485 / OSDP

## Cil

Cilem je nahradit soucasne provizorni zapojeni:

- na strane ctecky: TTL -> RS-485 modul
- na strane Raspberry Pi: RS-485 -> TTL -> USB prevodnik CP2102

novou cistou carrier deskou pro ctecku ELATEC TWN4 MultiTech 3 BLE. Deska ma ctecku mechanicky prijmout bez pajeni dratku primo na piny, napajet ji, pripojit ji na RS-485 sbernici a do budoucna umoznit i ovladani dverniho zamku, vstupy a BLE identifikaci.

## Shrnuti doporucene architektury

```text
VIN 12-48 V DC, navrhove radeji 12-60 V tolerantni
   |
   +-- ochrana vstupu
   +-- buck menic na 5 V pro TWN4
   +-- RS-485 transceiver pro COM1
   +-- rele COM/NO/NC
   +-- spinany LOCK vystup
   +-- vstupy: dverni kontakt, REX, tamper
   |
TWN4 MultiTech 3 BLE
   |
   +-- RFID / NFC / BLE credential
   +-- komunikace pres RS-485 / OSDP do RPi nebo kontroleru
```

Preferovana komunikace do systemu je RS-485 s adresnym protokolem, idealne OSDP. Wiegand a Omron davaji smysl hlavne jako vystupni formaty pro kompatibilitu se starsimi pristupovymi systemy, ne jako hlavni komunikace pro Raspberry Pi.

## Mechanicke reseni

Nejcistsi reseni je carrier / baseboard deska, do ktere se TWN4 zasune jako modul.

Podle dokumentace ma TWN4 MultiTech 3 BLE genericky konektor X2:

- 2x12 pin header
- roztec 2.00 mm
- v dokumentaci je jako priklad uveden Samtec `PTT-124-01-S-D`

Na carrier desce by mel byt odpovidajici 2x12 female socket / receptacle s rozteci 2.00 mm. Ctecka se zasune do konektoru a mechanicky se zajisti pres distancni sloupky nebo pritlacny ramecek.

Varianty:

- female socket + distancni sloupky: nejvhodnejsi pro trvale zarizeni
- pogo piny + pritlak pres sroubky: vhodne spis pro testovaci fixture nebo pokud nejsou osazene piny

Sroubky nemaji delat elektricky kontakt. Maji pouze drzet geometrii, aby konektor nebyl namahany vibracemi nebo kroucenim.

## Relevantni piny TWN4

Z konektoru X2 jsou pro prvni prototyp dulezite hlavne:

```text
X2 pin 12  GND
X2 pin 13  UVCC, USB VCC 5 V
X2 pin 17  COM1_TX, low active TTL output
X2 pin 18  COM1_RX, low active TTL input, internal pull-up
X2 pin 19  VCC 3.3 V za regulatorem
X2 pin 20  GPIO4
X2 pin 21  GPIO5
X2 pin 22  GPIO6
```

Poznamky:

- `VCC 3.3 V` z TWN4 nepouzivat jako zdroj pro vetsi externi zatez. Maximalne jako logickou referenci.
- COM1 je vhodny pro host komunikaci / RS-485 prevod.
- U BLE varianty je COM2 rezervovany pro BLE modul a GPIO7 je reset BLE modulu. Nepouzivat je pro vlastni funkce.
- GPIO0 az GPIO2 jsou v API spojene s LED funkcemi, proto je lepsi pro vlastni veci preferovat GPIO4 az GPIO6.

## GPIO moznosti

TWN4 API umoznuje GPIO pouzit jako:

- vstup s internim pull-up, pull-down nebo bez pullu
- vystup push-pull nebo open-drain
- cteni logicke urovne
- nastaveni / shozeni / prepinani vystupu
- blikani nebo jednoduchy obdelnikovy prubeh
- interrupt na nabeznou, sestupnou nebo obe hrany
- Wiegand vystup
- Omron clock/data vystup

Prakticke pouziti na carrier desce:

- GPIO pro rizeni `DE/RE` RS-485 transceiveru
- vystup pro rele nebo MOSFET lock driver
- vstup pro tamper
- vstup pro dverni kontakt
- vstup pro odchodove tlacitko REX
- lokalni signalizace

## RS-485 a OSDP

RS-485 je vhodna volba pro zlevneni kabelaze. Misto jednoho kabelu pro kazdou ctecku lze vest jednu sbernici:

```text
kontroler / RPi
   |
   |  +24/48 V, GND, RS485 A, RS485 B
   |
ctecka 1, adresa 1
ctecka 2, adresa 2
ctecka 3, adresa 3
```

Nutne zasady:

- vest jako sbernici, ne jako hvezdu
- terminace 120 R jen na obou koncich sbernice
- bias odpory pouze jednou, typicky u masteru, nebo konfigurovatelne jumperem
- pouzit krouceny par pro A/B
- pri dlouhych trasach zvazit izolovanou RS-485
- napajeni 24 V nebo 48 V je vyhodnejsi nez 12 V kvuli ubytkum na kabelu

OSDP je vhodnejsi nez Wiegand, protoze:

- bezi nad RS-485
- podporuje adresovani vice ctecek
- umi obousmernou komunikaci
- umi LED, bzucek, vstupy a vystupy
- umi zabezpeceny kanal ve vyssich rezimech
- je to standard pro pristupove systemy

Wiegand je proti tomu jednoduche jednosmerne posilani bitu bez potvrzeni, adresovani a zabezpeceni.

## RS-485 hardware

Doporucene vybaveni RS-485 casti:

- moderni RS-485 transceiver vhodny pro logiku TWN4
- TVS ochrana na linkach A/B
- jumper pro 120 R terminaci
- jumper pro bias odpory
- konektor A/B/GND
- pripadne izolovana varianta pro delsi trasy nebo rusne prostredi

Smer vysilani:

- bud ridit `DE/RE` pomoci GPIO, napr. GPIO4
- nebo pouzit transceiver s automatickym rizenim smeru

Pro prvni prototyp je jednodussi auto-direction transceiver. Pro robustni OSDP implementaci muze byt lepsi mit `DE/RE` pod plnou kontrolou firmware.

## Napajeni 12-48 V

Vstup 12-48 V DC je vhodny, ale soucastky je lepsi dimenzovat s rezervou na 60 V. Duvodem jsou spicky a realne chovani 48V systemu.

Doporucena vstupni cast:

- pojistka nebo eFuse
- ochrana proti prepolovani
- TVS dioda vhodna pro 48V systemy
- vstupni filtr
- buck menic na 5 V pro TWN4

TWN4 napajet z 5 V vetve. Zamek a silove vystupy drzet oddelene od logiky, hlavne z hlediska zemnich cest a proudovych spicek.

## PoE poznamka

Pokud jde o pasivni PoE, tedy 48 V DC primo na vodicich, lze ho brat jako bezny 48V vstup, pokud je deska na to navrzena.

Pokud jde o skutecne IEEE 802.3af/at PoE, nestaci jen pripojit 48 V. Deska potrebuje PoE PD controller nebo hotovy PoE modul, ktery se vuci switchi spravne identifikuje a vyjedna napajeni.

Doporuceni:

- prvni varianta desky: DC vstup 12-48 V, navrhove 60 V tolerantni
- pozdejsi varianta: PoE 802.3af/at s PD modulem

## Vystup pro zamek

Deska by mela umet dve veci:

1. Bezpotencialovy vystup do dverni jednotky:

```text
RELAY COM
RELAY NO
RELAY NC
```

2. Primy vystup pro zamek:

```text
LOCK+ = VIN nebo externi napajeni zamku
LOCK- = spinane pres MOSFET
```

Poznamky:

- rele COM/NO/NC je nejuniverzalnejsi pro napojeni do existujici jednotky
- MOSFET vystup je vhodny pro prime kratke pusteni napeti do zamku
- zamek muze byt 12 V, 24 V nebo 48 V podle napajeni a typu
- pokud je VIN 48 V, musi byt zamek take urceny pro 48 V, pokud deska nema samostatny menic na 12 V

Ochrany pro zamek:

- samostatna pojistka nebo proudove omezeni
- flyback dioda nebo TVS proti indukcni spicce
- MOSFET s dostatecnou rezervou napeti a proudu
- oddelene silove cesty na PCB

Fail-safe / fail-secure:

- fail-secure: bez napeti zamceno, impulsem se otevira
- fail-safe: pod napetim drzi, pri vypadku napajeni otevira

Deska by mela podporovat oba scenare alespon pres rele NO/NC. Primy lock vystup musi byt navrzen s ohledem na konkretni typ zamku.

## Vstupy

Doporucene vstupy na carrier desce:

- tamper
- dverni kontakt
- REX / odchodove tlacitko

Vstupy by mely mit:

- ochranu proti ESD a prepeti
- filtrovani zakmitu
- moznost pull-up / pull-down
- idealne svorky pro bezne instalacni kontakty

## BLE budoucnost

TWN4 MultiTech 3 BLE ma BLE modul. Dokumentace zminuje podporu BLE credentialu a API funkce jako:

- `BLEInit`
- `BLEDiscover`
- `BLEConnectToDevice`
- GATT read/write
- RSSI request
- transparentni BLE prenos

Podporovane nebo zminene credential scenare:

- Mobile Badge BLE NFC
- KleverKey
- Safetrust
- HID Mobile Access
- PKOC
- custom GATT / transparentni prenos

Doporucena architektura do budoucna:

```text
telefon / BLE credential
        |
        v
TWN4 MultiTech 3 BLE
        |
        v
OSDP pres RS-485
        |
        v
RPi / kontroler
```

Tedy BLE by nemelo byt oddelene paralelni reseni. TWN4 by nacetla BLE credential stejne jako kartu a pres OSDP/RS-485 by predala udalost kontroleru.

Pozor:

- RSSI neni bezpecnostni overeni, pouze orientacni signal o vzdalenosti
- pro otevreni dveri nepouzivat jen MAC adresu nebo beacon
- pro realne nasazeni je vhodny kryptograficky credential
- BLE antena nesmi byt zakryta medi, kovem nebo spatne navrzenou carrier deskou
- u sendvicove mechaniky nechat keepout v oblasti anteny
- iOS/Android maji omezeni behu aplikaci na pozadi

## Omron a Wiegand

Omron:

- dvoulinkovy clock/data vystup
- jedna linka je hodinovy signal, druha data
- vhodne jen pro zarizeni, ktera tento format ocekavaji
- neni to robustni adresna sbernice

Wiegand:

- dve linky DATA0 a DATA1
- bit 0 je impuls na DATA0
- bit 1 je impuls na DATA1
- velmi rozsirene ve starsich pristupovych systemech
- jednoduche, ale jednosmerne a bez zabezpeceni

Vyhody Wiegandu:

- jednoducha integrace se starsimi jednotkami
- rozsireny standard v pristupovych systemech
- snadna diagnostika logickym analyzatorem

Nevyhody Wiegandu:

- jednosmerny prenos
- bez potvrzeni doruceni
- bez adresovani
- bez sifrovani
- nevhodne pro bohatou komunikaci s RPi

Pro tento projekt je hlavni cesta RS-485 / OSDP. Wiegand a Omron jsou jen kompatibilni vystupni rezimy.

## Navrh prvniho prototypu

Prvni prototyp by mel byt radeji jednoduchy a overit zakladni predpoklady.

Minimalni prototyp:

- carrier deska pro TWN4 X2
- vstup 12-48 V DC, soucastky dimenzovat na 60 V
- buck 5 V pro TWN4
- RS-485 transceiver na COM1
- jumper terminace 120 R
- jumper bias
- svorkovnice A/B/GND/VIN
- testpointy pro COM1_TX, COM1_RX, GPIO4, 5 V, GND
- mechanicke uchyceni ctecky

Druha iterace:

- rele COM/NO/NC
- MOSFET LOCK vystup
- vstupy tamper / door / REX
- LED diagnostika
- OSDP firmware

Treti iterace:

- izolovana RS-485 varianta
- PoE 802.3af/at varianta
- BLE credential integrace

## Otevrene otazky

- Je na konkretni TWN4 fyzicky osazeny X2 header, nebo pouze pady?
- Jake jsou presne rozmery a vyska konektoru v konkretni mechanice?
- Bude hlavni kabel nest i napajeni zamku, nebo bude zamek napajen lokalne?
- Jaky proud a typ zamku se ma podporovat?
- Kolik ctecek bude typicky na jedne sbernici?
- Ma byt OSDP implementovano primo ve firmware TWN4, nebo ma carrier obsahovat dalsi MCU?
- Je cil podporovat pouze pasivni 48V PoE, nebo i skutecne IEEE 802.3af/at?

## Doporuceny smer

Nejlepsi smer je postavit z toho modul:

- TWN4 jako RFID/NFC/BLE jadro
- carrier deska jako napajeci, komunikacni a dverni elektronika
- RS-485 / OSDP jako hlavni komunikace
- 12-48 V DC vstup s rezervou na 60 V
- rele a MOSFET vystup pro flexibilni ovladani dveri
- vstupy pro dverni logiku
- BLE credential jako budouci rozsireni pres samotnou ctecku

Takovy modul by mohl vyrazne snizit cenu kabelaze a zjednodusit montaz, protoze vice ctecek muze byt na jedne adresne RS-485 sbernici.
