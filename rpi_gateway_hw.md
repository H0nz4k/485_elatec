# Raspberry Pi strana - RS-485 gateway / HAT

## Proc resit HW i na strane RPi

Ano, i na strane Raspberry Pi dava smysl udelat vlastni hardware. Soucasny retezec:

```text
RS-485 -> TTL RS-485 modul -> CP2102 USB-UART -> RPi USB
```

funguje, ale je to zbytecne poskladane z vice modulu. Pro cisty system jsou lepsi dve varianty.

## Varianta A - RPi HAT pres UART

Nejcistsi varianta pro centralni jednotku s Raspberry Pi:

```text
RPi GPIO UART
   |
   v
RS-485 transceiver
   |
   v
RS-485 A/B sbernice ke cteckam
```

Pouzite signaly RPi:

```text
GPIO14 / TXD  -> RS-485 DI
GPIO15 / RXD  <- RS-485 RO
GPIO18        -> DE/RE smer RS-485
GND           -> reference
3V3           -> logika transceiveru
```

Vyhody:

- zadny USB prevodnik navic
- mensi cena
- mensi mechanicky chaos
- primo riditelne z Linuxu
- dobre pro instalaci do jedne krabicky s RPi

Nevyhody:

- musi se spravne nastavit UART v Raspberry Pi
- Linux neni real-time, takze pro half-duplex je potreba dobre resit DE/RE
- pro OSDP je vhodne otestovat casovani

Poznamka: pokud se pouzije transceiver s automatickym rizenim smeru, je software jednodussi. Pokud se smer ridi pres GPIO, je to kontrolovatelnejsi.

## Varianta B - USB-RS485 gateway

Alternativa:

```text
RPi USB
   |
   v
USB-UART bridge
   |
   v
RS-485 transceiver
```

Mozne bridge:

- CP2102N
- CH343
- FT232R / FT231X

Vyhody:

- na Linuxu se tvari jako `/dev/ttyUSBx`
- lze pouzit i s jinym pocitacem nez RPi
- mechanicky muze byt jako externi adapter

Nevyhody:

- porad je to USB vrstva navic
- je potreba vyresit DE/RE, typicky pres RTS nebo auto-direction
- pro embedded instalaci je HAT elegantnejsi

## Doporucena RPi Rev A

Pro prvni vlastni HW bych navrhl RPi HAT:

- 40pin Raspberry Pi header
- 3.3V RS-485 transceiver
- TVS ochrana na A/B, napr. SM712
- 120R terminace pres jumper
- bias odpory pres jumper
- svorkovnice RS485 A/B/GND
- volitelne svorkovnice pro rozvod VIN_BUS 12-48 V do sbernice
- LED TX/RX/POWER
- volitelne izolovana RS-485 jako Rev B

## Napajeni na sbernici

RPi gateway muze do kabelu posilat pouze komunikaci:

```text
A
B
GND
```

nebo i napajeni pro ctecky:

```text
VIN_BUS 12-48 V
GND
A
B
```

Pokud bude po stejnem kabelu napajeni vice ctecek, je vhodne:

- preferovat 24 V nebo 48 V kvuli ubytkum
- pojistit vetev
- pocitat proud vsech modulu
- zamek radeji napajet lokalne nebo zvlast, pokud ma velky proud

## OSDP dopad

RPi bude master / control panel. Ctecky budou periferie s adresami.

```text
RPi OSDP master
   |
   +-- reader 1, address 1
   +-- reader 2, address 2
   +-- reader 3, address 3
```

Na RPi strane tedy musi byt spolehliva RS-485 fyzicka vrstva a software, ktery umi OSDP nebo vlastni adresny protokol.
