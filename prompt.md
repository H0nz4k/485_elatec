Jsi seniorní elektro inženýr, KiCad specialista, embedded hardware designer, reviewer výrobních podkladů pro JLCPCB a technický root-cause analytik.

Pracuji na elektro projektu, jehož cíl už znáš z předchozí komunikace: vytvořit výrobně použitelný prototyp PCB kolem ELATEC TWN4 čtečky / carrier desky, napájení, RS-485 komunikace, zámku, relé a vstupů. Mechanickou konstrukci teď NEŘEŠ jako samostatný návrh. Mechanické body pouze eviduj jako rizika, požadavky a TODO, pokud brání správnému návrhu PCB.

Tvým cílem je pokračovat dál bez čekání na moje potvrzení až do fáze prototypu připraveného pro výrobu, ideálně importovatelného do JLCPCB.

DŮLEŽITÉ ZÁSADY:
- Pracuj samostatně a neptej se mě na potvrzení běžných kroků.
- Pokud narazíš na nejasnost, udělej kvalifikovaný technický předpoklad, zapiš ho do reportu a pokračuj.
- Zastav se pouze v případě, že by další krok mohl nevratně poškodit projekt, smazat data, změnit main/master větev, nebo vytvořit výrobní podklady s kritickou neověřenou chybou.
- Nezasahuj přímo do aktuálně funkční větve.
- Vždy vytvoř novou git větev pro tuto práci.
- Pracuj jako senior elektro inženýr: ověřuj polaritu, proudy, ochrany, hodnoty pasivů, footprinty, výrobní limity, ERC/DRC a konzistenci schéma ↔ PCB ↔ BOM.
- Cílem není jen „něco nakreslit“, ale připravit smysluplný servisní prototyp, který půjde objednat, osadit, měřit a ladit.

ZAČNI TAKTO:
1. Prozkoumej aktuální stav repozitáře.
2. Identifikuj KiCad projekt, aktuální Rev B schéma, knihovny, footprinty, případné pracovní/vložené symboly a dosavadní poznámky.
3. Vytvoř novou větev, například:
   feature/rev-b-jlcpcb-prototype
4. Zapiš úvodní stav do reportu.
5. Pokračuj samostatně v práci.

HLAVNÍ CÍL PROTOTYPU:
Připravit Rev B elektro návrh carrier PCB pro:
- ELATEC TWN4 čtečku,
- napájení vstupem přibližně 12–48 V,
- stabilní 5 V větev pro TWN4,
- případně 3V3 větev, pokud je potřeba pro logiku,
- RS-485 komunikaci pro OSDP,
- řízení zámku,
- relé výstup,
- vstupy,
- ochrany proti přepětí, přepólování, ESD a špičkám z dlouhých kabelů,
- servisní měření a ladění prvního prototypu.

PRACOVNÍ KROKY:

1. Vyčistit Rev B schéma v KiCadu
- Převést pracovní/vložené symboly na normální KiCad/library symboly.
- Zkontrolovat a sjednotit knihovny.
- Doplnit konkrétní hodnoty odporů, kondenzátorů, cívek, diod, TVS, pojistek a dalších prvků.
- Zkontrolovat footprinty.
- Doplnit chybějící pasivy kolem buck měniče.
- Doplnit nebo ověřit vstupní ochrany.
- Doplnit nebo ověřit RS-485 terminaci, bias rezistory a jumpery.
- Doplnit blokovací kondenzátory u všech IC.
- Zkontrolovat napájecí větve a jejich názvy.
- Zkontrolovat GND návrh.
- Doplnit testpointy.
- Spustit ERC a chyby řešit nebo zdokumentovat.

2. TWN4 carrier
- Neřeš mechanickou konstrukci jako finální návrh.
- Přesto zkontroluj elektrické připojení TWN4.
- Ověř konektor X2 / COM1 / TTL / napájení / GND.
- Eviduj nutné mechanické údaje jako TODO:
  - přesná poloha pinů,
  - výška konektoru,
  - protikus Samtec 2×12 2.00 mm,
  - montážní otvory,
  - výška standoffů.
- Pokud nejsou mechanická data jistá, navrhni prototyp tak, aby šel servisně ověřit, ale jasně označ riziko.

3. Napájení
- Navrhni/ověř vstup 12–48 V.
- Ověř ochranu proti přepólování.
- Ověř pojistku/polyfuse.
- Ověř TVS na vstupu.
- Ověř buck měnič na 5 V.
- Spočítej přibližnou proudovou rezervu.
- Doporuč konkrétní hodnoty pasivů podle datasheetu měniče.
- Zkontroluj rozmístění vstupních/výstupních kondenzátorů.
- Přidej testpointy:
  - VIN,
  - +5V,
  - případně +3V3,
  - GND.

4. RS-485 / OSDP
- Na začátek počítej s OSDP přes RS-485.
- BLE neřeš v této první výrobní větvi; pouze ho uveď jako budoucí možnost.
- Ověř RS-485 transceiver.
- Ověř polaritu A/B.
- Ověř řízení DE/RE.
- Ověř možnost terminace 120 Ω jumperem/DIPem.
- Ověř bias rezistory jumperem/DIPem.
- Doporuč, jestli má být galvanické oddělení už v Rev B, nebo až Rev C.
- Pokud galvanické oddělení není nutné pro první prototyp, zdokumentuj proč.
- Přidej testpointy:
  - RS485_A,
  - RS485_B,
  - TX,
  - RX,
  - DE/RE,
  - GND.

5. Zámek / relé / výstupy
- Ověř proudové nároky zámku.
- Ověř ochranu proti indukční špičce.
- Doplnit flyback diodu / TVS / snubber podle typu výstupu.
- Přidej možnost odpojit lock část jumperem nebo servisním propojovacím bodem.
- Ověř dimenzování tranzistoru/MOSFETu/relé.
- Ověř šířky cest pro proud zámku.
- Zajisti jasně pojmenované svorky.
- Přidej měřicí body.

6. Vstupy
- Ověř vstupní ochrany na dlouhé kabely.
- Doplnit pull-up/pull-down podle logiky.
- Doplnit RC filtraci, pokud dává smysl.
- Ověř ESD/TVS ochranu.
- Jasně označ logiku vstupů.

7. PCB Rev B
- Po vyčištění schématu vytvoř nebo uprav PCB.
- První prototyp dělej servisní, ne miniaturní.
- Použij větší testpointy.
- Dej rozumné clearance mezi napájecími částmi.
- Odděl výkonovou část zámku od logiky.
- Drž buck layout podle datasheetu.
- Umísti ochrany co nejblíže konektorům.
- Umísti terminaci a bias RS-485 blízko transceiveru nebo konektoru podle smyslu návrhu.
- Zkontroluj návratové proudy.
- Přidej popisky na silkscreen:
  - VIN polarity,
  - RS485 A/B,
  - LOCK,
  - RELAY,
  - INPUTS,
  - 5V,
  - GND,
  - JUMPER terminace,
  - JUMPER bias,
  - REV označení.
- Spusť DRC a oprav chyby.

8. JLCPCB výstupy
Připrav výrobní výstupy:
- Gerber files,
- Drill files,
- BOM,
- Pick & Place / CPL,
- poznámky k osazení,
- seznam dílů vhodných pro JLCPCB/LCSC, pokud je možné,
- seznam dílů, které bude nutné osadit ručně,
- README pro výrobu,
- manufacturing checklist.

Výstupy ulož do struktury například:

/reports
  001_initial_state.md
  002_schematic_cleanup.md
  003_power_review.md
  004_rs485_review.md
  005_lock_relay_io_review.md
  006_pcb_layout_review.md
  007_jlcpcb_export_review.md
  008_final_prototype_readiness.md

/manufacturing
  /rev_b
    gerbers/
    drill/
    bom/
    pick_place/
    README_JLCPCB.md
    assembly_notes.md
    known_risks.md
    prototype_test_plan.md

/docs
  architecture.md
  electrical_assumptions.md
  revision_history.md
  testpoints.md
  bringup_plan.md

9. Reporty
Po každé větší části vytvoř report v Markdownu.
Každý report musí obsahovat:
- co bylo změněno,
- proč to bylo změněno,
- jaké předpoklady byly použity,
- co bylo ověřeno,
- co zůstává rizikem,
- co je další krok,
- odkazy na relevantní soubory,
- výsledek ERC/DRC, pokud se týká dané fáze.

10. Git commity a verze
Pracuj průběžně přes git.
Po každém logickém kroku udělej commit.
Commity piš jasně, například:
- rev-b: create production prototype branch
- rev-b: clean schematic symbols and footprints
- rev-b: add power input protection and buck passives
- rev-b: add rs485 termination bias and testpoints
- rev-b: add lock output protection and service jumpers
- rev-b: route initial pcb layout
- rev-b: fix drc and silkscreen labels
- rev-b: export jlcpcb manufacturing package
- docs: add prototype bring-up test plan

Zaveď nebo aktualizuj verzování projektu:
- Rev B jako pracovní výrobní prototyp.
- Přidej revision_history.md.
- Přidej jasné označení aktuální verze.
- Nepřepisuj staré výrobní výstupy bez archivace.

11. Kontrolní seznam před výrobou
Před finálním exportem vytvoř checklist:
- ERC bez kritických chyb.
- DRC bez kritických chyb.
- Všechny součástky mají hodnotu.
- Všechny součástky mají footprint.
- Kritické součástky mají ověřený datasheet.
- Napájecí větve jsou pojmenované a měřitelné.
- Testpointy jsou dostupné.
- RS-485 A/B je jasně označené.
- Terminace a bias jsou volitelné.
- Lock část lze servisně odpojit.
- Vstupní ochrany jsou osazené.
- BOM odpovídá PCB.
- Pick&Place odpovídá footprintům.
- Gerbery byly otevřeny a zkontrolovány v Gerber vieweru.
- Silkscreen nezasahuje do padů.
- Polarita konektorů, diod, elektrolytů a IC je jasná.
- Jsou zdokumentovaná rizika.
- Je připraven bring-up plán.

12. Bring-up plán prototypu
Vytvoř plán prvního oživení:
- vizuální kontrola PCB,
- kontrola zkratů mezi VIN/GND/5V/3V3,
- první napájení přes laboratorní zdroj s proudovým limitem,
- měření 5 V větve bez TWN4,
- měření 5 V větve s TWN4,
- kontrola RS-485 TX/RX/DE/RE,
- test OSDP komunikace,
- test vstupů,
- test relé,
- test zámku nejprve bez reálné zátěže,
- test zámku s reálnou zátěží přes proudový limit,
- záznam výsledků do reportu.

13. Co teď neřešit
- Neřeš finální mechanickou konstrukci.
- Neřeš BLE jako součást první výrobní desky.
- Neřeš design krabičky.
- Neřeš finální estetiku.
- Neřeš masovou výrobu, dokud není ověřen první servisní prototyp.

14. Výstup, který od tebe očekávám
Na konci práce chci mít:
- čisté KiCad schéma Rev B,
- PCB Rev B,
- reporty,
- git commity,
- revision history,
- výrobní složku pro JLCPCB,
- BOM,
- Pick&Place,
- Gerbery,
- Drill files,
- assembly notes,
- known risks,
- bring-up/test plán,
- jasné doporučení, jestli je prototyp připraven k objednání, nebo co přesně ještě blokuje výrobu.

Pracuj postupně, pečlivě a samostatně. Každé rozhodnutí zapisuj. Pokud něco nevíš, udělej nejlepší inženýrský předpoklad, označ ho jako ASSUMPTION a pokračuj. Cílem je dostat projekt z aktuální Rev B fáze do reálně objednatelného a měřitelného prototypu.