# TWN4 Access System - elektricke schema Rev B

Tento dokument prevadi puvodni blokovy navrh na konkretnejsi elektricke schema pro prvni prototyp. Stale nejde o finalni vyrobni dokumentaci, ale uz jsou zde konkretni soucastky, hodnoty, nety a rozhrani.

Schema je rozdeleno na dve desky:

1. `reader-carrier`: deska u dveri s TWN4, RS-485, napajenim, rele, vystupem pro zamek a vstupy.
2. `rpi-gateway`: deska/HAT u Raspberry Pi, ktera dela centralni RS-485 rozhrani a pripadne distribuci napajeni po kabelu.

## 1. Reader carrier

### 1.1 Konektory

#### J1 - vstup napajeni

```text
J1.1  VIN_IN   12-48 V DC
J1.2  GND_IN
```

Doporuceni:

- svorkovnice 2pin, roztec 5.08 mm
- vstup navrhovat tak, aby prezil bezne 48V instalace a PoE-like pasivni napajeni
- pro skutecne 802.3af/at PoE je pozdeji potreba PoE PD front-end

#### J2 - RS-485 sbernice

```text
J2.1  RS485_A
J2.2  RS485_B
J2.3  GND_BUS
J2.4  SHIELD
```

`SHIELD` vest na samostatny pad / svorku. Pro Rev B:

```text
SHIELD -> JP_SHIELD -> chassis/mechanical GND
SHIELD -> C_SHIELD 4.7 nF / 100 V -> GND
SHIELD -> R_SHIELD 1 M -> GND
```

#### J3 - rele kontakt

```text
J3.1  RELAY_COM
J3.2  RELAY_NO
J3.3  RELAY_NC
```

Pouziti: bezpotencialovy impuls do dverni jednotky.

#### J4 - primy vystup pro zamek

```text
J4.1  LOCK+
J4.2  LOCK-
```

Rev B varianta:

```text
LOCK+ = VIN_LOCK
LOCK- = spinane pres N-MOSFET na GND_POWER
```

`VIN_LOCK` je volitelne:

```text
JP_LOCK_SUPPLY:
1-2 = VIN_LOCK z VIN_PROT
2-3 = VIN_LOCK z externi svorky J7
```

#### J5 - vstupy

```text
J5.1  DOOR_CONTACT
J5.2  REX_BUTTON
J5.3  TAMPER
J5.4  GND_IO
```

Vstupy jsou navrzene jako suche kontakty proti zemi.

#### J6 - TWN4 X2 socket

Konektor:

- 2x12
- roztec 2.00 mm
- protikus pro TWN4 X2
- kandidat: Samtec `PTF-124-01-S-D` nebo kompatibilni 2x12 female socket

Pouzite piny:

```text
X2.3   I2C_SCL
X2.4   I2C_SDA
X2.10  HOSTSENSE
X2.12  GND
X2.13  +5V_TWN4
X2.17  COM1_TX
X2.18  COM1_RX
X2.19  VCC_3V3_REF
X2.20  GPIO4_RS485_DIR
X2.21  GPIO5_RELAY
X2.22  GPIO6_LOCK
X2.23  PWRDWN_N
X2.24  RESET_N
```

`HOSTSENSE`:

```text
JP_HOSTSENSE:
1-2 osazeno: HOSTSENSE -> GND, ctecka bezi pres COM1
open: internal pull-up, USB rezim
```

Pro dverni modul bude osazeno `HOSTSENSE -> GND`.

### 1.2 Vstupni ochrana a napajeni

#### F1 - vstupni pojistka

```text
VIN_IN -> F1 -> VIN_FUSED
```

Doporuceni:

- pro logiku a maly zamek: vratna polyfuse 1.1-1.5 A hold, napeti aspon 60 V
- pro instalacni verzi radeji tavna pojistka nebo eFuse podle proudoveho rozpoctu

#### U1 - vstupni ochrana, Rev B kandidat

Pro robustni vstup je vhodny `LTC4367`:

- 2.5-60 V operating range
- ochrana proti prepeti az 100 V
- ochrana proti podpeti a reverse supply
- ridi back-to-back N-MOSFETy

Rev B doporuceni:

```text
VIN_FUSED -> LTC4367 + back-to-back MOSFETs -> VIN_PROT
```

Nastaveni prahu:

```text
UVLO: priblizne 10.5 V
OVLO: priblizne 60 V
```

Poznamka: Delice UV/OV je nutne prepocitat presne podle finalni zvolene varianty `LTC4367` a datasheetove rovnice. Pro prvni KiCad kresbu je tato cast oznacena jako `front-end protection`.

Jednodussi alternativa pro laboratorni prototyp:

```text
VIN_IN -> F1 -> Schottky/rectifier diode 100 V -> VIN_PROT
VIN_PROT -> TVS -> GND
```

Tato varianta je jednodussi, ale hreje a neni tak elegantni pri vyssich proudech.

#### D1 - vstupni TVS

Kandidat:

```text
D1 = SMBJ58A nebo ekvivalent pro 48V system
```

Poznamka:

- vhodne pro pasivni 48V napajeni, kde normalni maximum muze byt blizko 57 V
- finalni TVS zvolit az podle zvolene vstupni ochrany, aby clamp neprekracoval limity dalsich obvodu

#### U2 - buck 5 V

Kandidat:

```text
U2 = TI LM5164
```

Vlastnosti:

- vstup 6-100 V
- vystup az 1 A
- vhodne pro 12-48 V vstup s rezervou

Navrh podle TI 5V aplikace:

```text
U2 VIN   -> VIN_PROT
U2 VOUT  -> +5V
U2 GND   -> GND
```

Hodnoty:

```text
CIN1, CIN2   2.2 uF / 100 V / X7R
CIN_BULK     47 uF / 100 V electrolytic nebo polymer
L1           68 uH, Isat > 1.8 A, nizky DCR
COUT1,COUT2  22 uF / 25 V / X7R
RFB_TOP      453 k / 1 %
RFB_BOT      49.9 k / 1 %
RRON         100 k / 1 %
RA           453 k / 1 %
CA           3.3 nF
CB           56 pF
CBST         2.2 nF
```

Poznamka: Tyto hodnoty vychazi z TI prikladoveho 5V navrhu. Layout bucku musi byt kreslen podle datasheetu, jinak bude zdroj zbytecne rusit RFID/BLE cast.

#### U3 - 3.3 V regulator

Kandidat:

```text
U3 = TLV75533PDBVR nebo ekvivalent 3.3 V LDO
```

Zapojeni:

```text
+5V -> U3 IN
U3 OUT -> +3V3
U3 GND -> GND
```

Hodnoty:

```text
CIN  1 uF / 10 V
COUT 1 uF / 10 V
```

`+3V3` napaji:

- RS-485 transceiver
- MCP23008
- I2C pull-upy
- drobnou logiku

### 1.3 TWN4 napajeni

```text
+5V -> J6 X2.13 +5V_TWN4
GND -> J6 X2.12
```

Pridat:

```text
C_TWN4_LOCAL 10 uF / 10 V u J6
C_TWN4_HF    100 nF u J6
FB_TWN4      optional ferrite bead mezi +5V a +5V_TWN4
```

`VCC_3V3_REF` z X2.19 nebrat jako zdroj pro carrier. Jen testpoint / reference.

### 1.4 RS-485 reader side

#### U4 - RS-485 transceiver

Kandidat:

```text
U4 = TI THVD1450
```

Vlastnosti:

- 3.0-5.5 V supply
- half-duplex RS-485
- vhodny pro 3.3V logiku
- integrovana ESD odolnost

Zapojeni:

```text
J6 X2.17 COM1_TX -> R_TX 33 R -> U4 DI
J6 X2.18 COM1_RX <- R_RX 33 R <- U4 RO
J6 X2.20 GPIO4   -> R_DIR 100 R -> U4 DE
J6 X2.20 GPIO4   -> R_DIR 100 R -> U4 /RE
U4 VCC -> +3V3
U4 GND -> GND
```

Toto zapojeni pouziva:

```text
GPIO4 low  = prijem: driver off, receiver on
GPIO4 high = vysilani: driver on, receiver off
```

Default:

```text
R_DIR_PD = 100 k z GPIO4_RS485_DIR na GND
```

Tedy po resetu je transceiver v prijmu.

Poznamka: Pokud bude potreba poslouchat vlastni vysilani, musi se `DE` a `/RE` ridit oddelene nebo pres invertor.

#### RS-485 ochrana a terminace

```text
U4 A -> R_A_SER 10 R -> RS485_A
U4 B -> R_B_SER 10 R -> RS485_B
D2 = SM712 mezi RS485_A / RS485_B / GND
```

Terminace:

```text
RS485_A -> JP_TERM -> R_TERM 120 R -> RS485_B
```

Bias, osazovat jen pokud je modul na konci / jako master podle sbernice:

```text
R_BIAS_UP   680 R: RS485_A -> +3V3, pres jumper
R_BIAS_DOWN 680 R: RS485_B -> GND, pres jumper
```

Poznamka: Znaceni A/B se mezi vyrobci umi prohodit. Pred PCB finalizaci overit polaritu s realnym THVD1450 a RPi stranou.

### 1.5 Rele vystup

#### K1 - rele

Kandidat:

```text
K1 = Omron G5V-2 DC5 nebo ekvivalent DPDT 5V signal relay
```

Pouzije se jeden prepinaci kontakt:

```text
K1 COM -> J3.1 RELAY_COM
K1 NO  -> J3.2 RELAY_NO
K1 NC  -> J3.3 RELAY_NC
```

Driver:

```text
J6 X2.21 GPIO5_RELAY -> R_G2 100 R -> Q2 gate
Q2 = 2N7002
Q2 source -> GND
Q2 drain -> K1 coil low
K1 coil high -> +5V
D3 flyback diode paralelne k civce, katoda na +5V
R_PD2 100 k gate -> GND
```

D3:

```text
D3 = 1N4148 / BAV99 / SS14 podle civkoveho proudu
```

### 1.6 Primy lock vystup

Rev B pouziva low-side MOSFET.

```text
VIN_LOCK -> J4.1 LOCK+
J4.2 LOCK- -> Q3 drain
Q3 source -> GND_POWER
```

Kandidat MOSFET:

```text
Q3 = 100V N-MOSFET s Rds(on) specifikovanou pri Vgs = 4.5 V
priklad: Diodes Inc. DMT10H015LSS nebo podobny
```

Gate driver:

```text
J6 X2.22 GPIO6_LOCK -> U6 input
U6 = SN74AHCT1G125 nebo podobny 5V TTL-compatible buffer
U6 VCC -> +5V
U6 output -> R_G3 47 R -> Q3 gate
R_PD3 100 k -> Q3 gate to GND
```

Duvod: TWN4 GPIO je 3.3V logika. Pro 100V MOSFET je lepsi mit gate drive 5 V, pokud je Rds(on) garantovana pri 4.5 V.

Ochrana zamku:

```text
D4 flyback diode pres zamek, pokud je zamek DC civka
D5 TVS na LOCK+ / LOCK- podle napeti zamku
F_LOCK pojistka v ceste VIN_LOCK
```

Doporuceni:

```text
F_LOCK = podle ciloveho zamku, napr. 1 A nebo 2 A
D5 = TVS podle 12/24/48V lock varianty
```

Pozor: Pokud je vstup 48 V, `LOCK+` bude 48 V, pokud neni pouzit externi 12V zdroj nebo samostatny 12V menic.

### 1.7 Vstupy DOOR / REX / TAMPER

#### U5 - I2C IO expander

Kandidat:

```text
U5 = MCP23008
```

Zapojeni:

```text
J6 X2.3 I2C_SCL -> R_SCL_SER 33 R -> U5 SCL
J6 X2.4 I2C_SDA -> R_SDA_SER 33 R -> U5 SDA
R_SCL_PULL 4.7 k -> +3V3
R_SDA_PULL 4.7 k -> +3V3
U5 VDD -> +3V3
U5 VSS -> GND
U5 RESET -> R_RESET 10 k -> +3V3
U5 A0,A1,A2 -> GND
```

Adresa:

```text
MCP23008 address = 0x20
```

Vstup jednoho kontaktu:

```text
J5 input -> R_IN_SER 1 k -> U5 GPx
U5 GPx -> R_IN_PULL 10 k -> +3V3
U5 GPx -> C_IN 100 nF -> GND
J5 input -> ESD diode -> GND/+3V3 nebo maly TVS na GND
```

Mapovani:

```text
GP0 = DOOR_CONTACT
GP1 = REX_BUTTON
GP2 = TAMPER
GP3 = AUX_IN
GP4-GP7 = rezerva / LED / DIP adresa
```

Pro kabelove vstupy v realne instalaci zvazit optocleny nebo robustnejsi vstupni ochranu.

### 1.8 Diagnostika

LED:

```text
LED_PWR: +5V pres 2.2 k -> LED -> GND
LED_RS485_TX: GPIO4 nebo U4 DE pres 2.2 k -> LED -> GND
LED_RELAY: GPIO5 pres 2.2 k -> LED -> GND
LED_LOCK: U6 output pres 2.2 k -> LED -> GND
```

Testpointy:

```text
TP_VIN_PROT
TP_5V
TP_3V3
TP_GND
TP_COM1_TX
TP_COM1_RX
TP_RS485_A
TP_RS485_B
TP_GPIO4_DIR
TP_GPIO5_RELAY
TP_GPIO6_LOCK
```

## 2. RPi gateway / HAT

RPi strana ma byt jednodussi nez reader carrier. Jejich ukol je:

- prevod UART RPi na RS-485
- terminace/bias na strane masteru
- volitelna distribuce `VIN_BUS` do kabelu

### 2.1 Konektory

#### J10 - Raspberry Pi 40pin header subset

Pouzite piny:

```text
Pin 1   +3V3
Pin 2   +5V
Pin 6   GND
Pin 8   GPIO14 / TXD
Pin 10  GPIO15 / RXD
Pin 12  GPIO18 / RS485_DIR
Pin 14  GND
```

#### J11 - RS-485 bus

```text
J11.1 RS485_A
J11.2 RS485_B
J11.3 GND_BUS
J11.4 VIN_BUS optional
```

#### J12 - napajeni sbernice

```text
J12.1 VIN_BUS_IN 12-48 V
J12.2 GND_BUS
```

### 2.2 RS-485 transceiver

Kandidat:

```text
U10 = TI THVD1450
```

Zapojeni:

```text
RPi GPIO14 TXD -> R10 33 R -> U10 DI
RPi GPIO15 RXD <- R11 33 R <- U10 RO
RPi GPIO18 DIR -> R12 100 R -> U10 DE
RPi GPIO18 DIR -> R12 100 R -> U10 /RE
R13 100 k: DIR -> GND
U10 VCC -> +3V3_RPI
U10 GND -> GND
```

Bus:

```text
U10 A -> R14 10 R -> RS485_A
U10 B -> R15 10 R -> RS485_B
D10 = SM712 na A/B
JP10 + R16 120 R terminace
JP11 + R17 680 R pull-up bias
JP12 + R18 680 R pull-down bias
```

Na master strane bude typicky osazena bias sada. Terminace pouze pokud je RPi na konci sbernice.

### 2.3 VIN_BUS distribuce

Pokud RPi gateway napaji sbernici:

```text
J12 VIN_BUS_IN -> F10 -> VIN_BUS -> J11.4
J12 GND_BUS -> J11.3
D11 TVS na VIN_BUS
LED_BUS_POWER pres 10 k
```

Hodnoty:

```text
F10 podle celkoveho proudu sbernice
D11 SMBJ58A nebo podle realneho VIN_BUS
```

Poznamka: Nepouzet 5V z Raspberry Pi pro napajeni ctecek na dlouhem kabelu. Sbernice ma mit vlastni 12/24/48V zdroj.

## 3. Net naming

Hlavni nety:

```text
VIN_IN
VIN_FUSED
VIN_PROT
VIN_LOCK
+5V
+3V3
GND
GND_POWER
GND_BUS
RS485_A
RS485_B
COM1_TX
COM1_RX
GPIO4_RS485_DIR
GPIO5_RELAY
GPIO6_LOCK
I2C_SCL
I2C_SDA
DOOR_CONTACT
REX_BUTTON
TAMPER
```

## 4. Kriticke body k overeni

1. `COM1_TX` / `COM1_RX` jsou v dokumentaci popsane jako low active TTL. Pred definitivnim zapojenim overit osciloskopem, zda neni potreba inverze.
2. A/B polarita RS-485 se musi overit proti realnemu transceiveru a OSDP stacku.
3. `GPIO4` rizeni DE/RE musi byt synchronizovane s vysilanim OSDP ramcu.
4. Buck `LM5164` musi mit layout podle datasheetu, jinak muze rusit RFID/BLE.
5. BLE antenni cast TWN4 nesmi byt prekryta medi ani kovem.
6. Lock vystup se musi dimenzovat podle realneho typu zamku.
7. Pokud bude skutecne 802.3af/at PoE, musi prijit samostatna PoE PD varianta.

## 5. Minimalni Rev B BOM

Viz `bom_rev_b.csv`.
