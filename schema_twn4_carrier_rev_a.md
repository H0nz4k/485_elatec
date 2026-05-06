# TWN4 Carrier Rev A - navrh schematu

Toto je prvni navrh elektrickeho schematu pro carrier desku k ELATEC TWN4 MultiTech 3 BLE. Neni to jeste vyrobni dokumentace, ale je to konkretni zapojeni, ktere se da prekreslit do KiCadu a dale overovat.

## Predpoklady

- Ctecka je TWN4 MultiTech 3 BLE.
- Komunikace se cteckou pujde pres `COM1_TX` / `COM1_RX`.
- Hlavni systemova komunikace bude RS-485, idealne OSDP.
- Napajeci vstup desky bude `12-48 V DC`, soucastky dimenzovat s rezervou aspon na `60 V`.
- Ctecka bude napajena z `5 V`.
- BLE varianta TWN4 pouziva interne `COM2` a `GPIO7`, proto se pro carrier nepouziji.
- Pro prvni revizi se pocita s neizolovanou RS-485. Izolovana verze muze byt Rev B.

## Blokove schema

```text
J1 VIN 12-48 V
   |
   +-- F1 pojistka / polyfuse
   +-- Q1 ochrana proti prepolovani
   +-- D1 TVS
   +-- filtr
   |
   +-- U1 buck 5 V
   |      |
   |      +-- TWN4 +5 V
   |      +-- rele 5 V
   |
   +-- U2 LDO 3.3 V
   |      |
   |      +-- RS-485 transceiver
   |      +-- I2C pull-upy / IO expander
   |
TWN4 X2
   |
   +-- COM1_TX/RX -> U3 RS-485
   +-- GPIO4 -> RS-485 DE/RE
   +-- GPIO5 -> rele driver
   +-- GPIO6 -> lock MOSFET driver
   +-- I2C -> volitelny IO expander
```

## Konektory

### J1 - napajeci vstup

```text
J1.1  VIN+   12-48 V DC
J1.2  GND
```

Doporucena svorkovnice: 2pin, roztec 3.5 mm nebo 5.08 mm podle proudu a mechaniky.

### J2 - RS-485 sbernice

```text
J2.1  RS485_A
J2.2  RS485_B
J2.3  GND / reference
J2.4  SHIELD optional
```

Poznamka: `SHIELD` nepripojovat natvrdo na logickou zem bez rozmyslu. Pro prvni revizi muze byt pres RC / jumper / chassis pad.

### J3 - rele vystup do dverni jednotky

```text
J3.1  RELAY_COM
J3.2  RELAY_NO
J3.3  RELAY_NC
```

Pouziti: bezpotencialovy impuls do existujici dverni jednotky.

### J4 - primy vystup pro zamek

```text
J4.1  LOCK+
J4.2  LOCK-
```

Navrh:

```text
LOCK+ = VIN_LOCK nebo VIN+
LOCK- = spinane pres N-MOSFET na GND
```

Pro prvni revizi doporucuji propojku:

```text
JP_LOCK_SUPPLY:
1-2 = LOCK+ napajeno z VIN+
2-3 = LOCK+ napajeno z externiho VIN_LOCK
```

### J5 - vstupy

```text
J5.1  DOOR_CONTACT
J5.2  REX_BUTTON
J5.3  TAMPER
J5.4  GND
```

Tyto vstupy je vhodne vest pres IO expander, aby se nespotrebovaly vsechny TWN4 GPIO.

### J6 - TWN4 X2 socket

Carrier deska ma mit 2x12 female socket, roztec 2.00 mm, protikus k TWN4 X2.

Relevantni piny:

```text
X2.3   I2C_SCL
X2.4   I2C_SDA
X2.10  HOSTSENSE
X2.12  GND
X2.13  UVCC / 5 V
X2.17  COM1_TX
X2.18  COM1_RX
X2.19  VCC 3.3 V reference z TWN4
X2.20  GPIO4
X2.21  GPIO5
X2.22  GPIO6
X2.23  PWRDWN-
X2.24  RESET-
```

`HOSTSENSE`:

- pro COM1 provoz musi byt low, tedy pripojeno na GND
- doporuceni: dat jumper `JP_HOSTSENSE`, aby slo volit COM1/USB rezim

```text
JP_HOSTSENSE:
1-2 = HOSTSENSE -> GND, COM1 mode
open = internal pull-up, USB mode
```

## Napajeci cast

### Vstupni ochrana

```text
J1 VIN+ -> F1 -> Q1 ideal diode / reverse polarity -> VIN_PROT
VIN_PROT -> D1 TVS -> GND
VIN_PROT -> Cbulk -> GND
```

Doporucene prvky:

- `F1`: vratna polyfuse nebo tavna pojistka podle ciloveho proudu
- `Q1`: P-MOSFET nebo ideal-diode/eFuse reseni pro ochranu proti prepolovani
- `D1`: TVS pro 48V system, napriklad rada SMBJ/SMCJ s vhodnym napetim
- `Cbulk`: elektrolyt nebo polymer, napr. 47-100 uF, napetove aspon 63 V
- `C_HF`: 100 nF + 1 uF keramika u menice

Poznamka: pro skutecny 48V/PoE svet je vhodne volit buck s maximalnim vstupem 80-100 V, ne pouze 60 V.

### 5V vetev

```text
VIN_PROT -> U1 wide-input buck -> +5V
```

Pozadavky:

- vstup aspon 60 V, lepe 80-100 V
- vystup 5 V
- proud minimalne 500 mA, lepe 1 A rezerva
- nizke EMI, dobra layout doporuceni podle datasheetu

Mozne typy:

- TI `LM5163` / `LM5164` family
- Analog Devices / Monolithic Power wide-input buck
- hotovy DC/DC modul s inputem vhodnym pro 48V systemy

`+5V` napaji:

- TWN4 pres X2.13 nebo odpovidajici 5V vstup
- rele civku, pokud bude 5V rele
- pripadne dalsi drobnou logiku

### 3.3V vetev

```text
+5V -> U2 LDO 3.3 V -> +3V3
```

`+3V3` napaji:

- RS-485 transceiver
- I2C pull-upy
- IO expander

Poznamka: TWN4 ma na X2.19 vlastni `VCC 3.3 V`. Nepouzivat ho jako hlavni zdroj pro carrier. Muze slouzit jako reference nebo pro kontrolu urovne.

## RS-485 cast

### U3 - RS-485 transceiver

Doporuceny typ:

- `THVD1450`, `MAX3485`, `SN65HVD1781` nebo podobny 3.3V/5V half-duplex RS-485 transceiver

Zapojeni:

```text
TWN4 COM1_TX -> U3 DI
TWN4 COM1_RX <- U3 RO
TWN4 GPIO4  -> U3 DE
TWN4 GPIO4  -> U3 /RE pres invertor nebo vhodne zapojeni
U3 A        -> RS485_A
U3 B        -> RS485_B
```

Jednoducha varianta:

```text
GPIO4 high = vysilani
GPIO4 low  = prijem
DE  = GPIO4
/RE = not GPIO4
```

Pokud nechceme pridavat invertor, lze zvazit:

```text
DE  = GPIO4
/RE = GPIO4
```

To ale pri vysilani vypne receiver a pri prijmu vypne driver. Chovani je pouzitelne, ale je potreba overit s firmware. Lepsi je maly invertor nebo tranzistor, aby bylo rizeni jasne.

Ochrany a sbernice:

```text
RS485_A -> TVS array -> GND
RS485_B -> TVS array -> GND
RS485_A <-> RS485_B pres 120 R, pouze pres jumper
bias pull-up / pull-down pouze pres jumper
```

Doporucene prvky:

- `D2`: ESD/TVS pro RS-485 linky, napr. `SM712`
- `RTERM`: 120 R pres `JP_TERM`
- `RBIAS_UP`, `RBIAS_DN`: napr. 680 R az 1.5 k podle navrhu sbernice, pres jumpery

## Rele vystup

### K1 - rele

```text
+5V -> K1 coil -> Q2 N-MOSFET/NPN -> GND
GPIO5 -> Rgate/Rbase -> Q2
D3 flyback pres civku
```

Kontakty:

```text
K1 COM -> J3.1
K1 NO  -> J3.2
K1 NC  -> J3.3
```

Doporuceni:

- signalove rele s civkou 5 V
- kontakty dimenzovat podle toho, jestli budou spinat jen signal, nebo i realnou zatez
- pokud ma rele spinat cizi system, drzet kontaktovou cast s rozumnymi izolačními mezerami

## Primy LOCK vystup

### Q3 - MOSFET low-side switch

```text
VIN_LOCK / VIN+ -> J4.1 LOCK+
J4.2 LOCK- -> drain Q3
source Q3 -> GND_POWER
GPIO6 -> Rgate -> gate Q3
gate -> Rpulldown -> GND
D4 / TVS / flyback ochrana pres zamek
```

Doporuceni:

- logic-level N-MOSFET
- napetova rezerva aspon 80-100 V pro 48V system
- proud podle ciloveho zamku, pro prototyp navrhnout treba 2 A
- samostatna pojistka `F_LOCK`
- siroke silove cesty
- zemni proud zamku vest oddelene od citlive logiky a spojit ve vhodnem bode

Poznamka: Pokud je vstup 48 V, primy vystup dava 48 V. Pro 12V zamek je potreba bud 12V vstup, externi 12V napajeni zamku, nebo dalsi menic 48 -> 12 V.

## Vstupy pres IO expander

Protoze TWN4 ma na X2 pro vlastni pouziti pohodlne hlavne GPIO4-6, je vhodne pouzit I2C IO expander.

### U4 - MCP23008 / MCP23017

```text
TWN4 I2C_SCL -> U4 SCL
TWN4 I2C_SDA -> U4 SDA
SCL -> 4.7 k -> +3V3
SDA -> 4.7 k -> +3V3
U4 VDD -> +3V3
U4 GND -> GND
```

Vstupy:

```text
J5 DOOR_CONTACT -> ochrana/filter -> U4 GP0
J5 REX_BUTTON   -> ochrana/filter -> U4 GP1
J5 TAMPER       -> ochrana/filter -> U4 GP2
```

Vystupy / rezervy:

```text
U4 GP3 -> LED status optional
U4 GP4 -> reserve
U4 GP5 -> reserve
U4 GP6 -> reserve
U4 GP7 -> reserve
```

Poznamka: Pokud nechceme v prvni firmware iteraci resit I2C expander, lze vstupy v Rev A neosadit nebo je vyvest pouze na test konektor.

## Signalove prirazeni TWN4 GPIO

Minimalni varianta bez IO expanderu:

```text
GPIO4 = RS485_DIR
GPIO5 = RELAY_CTRL
GPIO6 = LOCK_CTRL
```

Varianta s IO expanderem:

```text
GPIO4 = RS485_DIR
GPIO5 = RELAY_CTRL
GPIO6 = LOCK_CTRL
I2C   = DOOR / REX / TAMPER / dalsi IO
```

## Netlist - hlavni signaly

```text
VIN_IN       J1.1 -> F1 -> Q1 -> VIN_PROT
GND          J1.2, U1.GND, U2.GND, TWN4.GND, U3.GND
+5V          U1.OUT -> TWN4.X2.13, K1.COIL, U2.IN
+3V3         U2.OUT -> U3.VCC, U4.VDD, I2C pull-upy

TWN4_COM1_TX TWN4.X2.17 -> U3.DI
TWN4_COM1_RX TWN4.X2.18 <- U3.RO
RS485_DIR    TWN4.X2.20 -> U3.DE / U3.RE_CTRL
RS485_A      U3.A -> D2 -> J2.1
RS485_B      U3.B -> D2 -> J2.2

RELAY_CTRL   TWN4.X2.21 -> Q2.GATE
LOCK_CTRL    TWN4.X2.22 -> Q3.GATE

I2C_SCL      TWN4.X2.3 -> U4.SCL
I2C_SDA      TWN4.X2.4 -> U4.SDA
```

## Orientacni BOM

| Ref | Funkce | Priklad |
| --- | --- | --- |
| J1 | VIN vstup | 2pin svorkovnice |
| J2 | RS-485 | 3/4pin svorkovnice |
| J3 | rele kontakt | 3pin svorkovnice |
| J4 | lock vystup | 2pin svorkovnice |
| J6 | TWN4 X2 socket | 2x12, 2.00 mm female socket |
| F1 | vstupni pojistka | podle proudu |
| D1 | vstupni TVS | 48V system, SMBJ/SMCJ rada |
| U1 | 5V buck | wide-input 60-100 V |
| U2 | 3.3V LDO | 300 mA LDO |
| U3 | RS-485 | THVD1450 / MAX3485 / SN65HVD1781 |
| D2 | RS-485 TVS | SM712 nebo ekvivalent |
| K1 | rele | 5V signal/power relay |
| Q2 | rele driver | maly N-MOSFET nebo NPN |
| Q3 | lock driver | logic-level N-MOSFET 80-100 V |
| U4 | IO expander | MCP23008 / MCP23017 |

## Veci k overeni pred kreslenim PCB

1. Fyzicky stav X2 na konkretni ctecce: osazeny pin header, nebo jen pady?
2. Presny protikus konektoru a vyska mezi deskami.
3. Kde je BLE antena a kolik keepout prostoru potrebuje.
4. Jestli TWN4 COM1 signaly opravdu potrebuji inverzi kvuli popisu "low active TTL".
5. Maximalni proud zamku.
6. Jestli lock vystup ma spinat VIN, nebo ma byt na desce samostatny 12V menic.
7. Jestli bude prvni deska podporovat jen DC 12-48 V, nebo rovnou i PoE 802.3af/at.
8. OSDP firmware: zda bude smer RS-485 rizen z GPIO4, nebo se pouzije transceiver s auto-direction.

## Doporuceni pro Rev A

Pro prvni desku bych vyrobil neizolovanou verzi:

- X2 socket pro TWN4
- 12-48 V vstup, soucastky s rezervou
- 5V buck pro TWN4
- 3.3V logika pro transceiver
- RS-485 s TVS, terminaci a bias jumpery
- GPIO4 jako RS-485 direction
- GPIO5 rele
- GPIO6 lock MOSFET
- I2C expander pripraveny pro DOOR/REX/TAMPER, klidne jako optional osazeni

Tato revize overi mechaniku, napajeni, RS-485/OSDP a zakladni dverni vystupy.
