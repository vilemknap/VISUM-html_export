# R1 kartogramy V2.5 - vsech 69 scenaru + OA / LNV / NV

Tato verze je urcena pro aktualni stav projektu, kde jsou podle screenshotu z 1. 11. 2026
vypoctene vsechny scenare 1-69.

## Co vytvori

- 69 absolutnich interaktivnich HTML map - jedna pro kazdy scenar.
- 68 rozdilovych interaktivnich HTML map proti W_2025.
- Celkem 137 HTML map.
- Po kliknuti na smerovy link se zobrazi Celkem, OA, LNV a NV.
- V rozdilove mape se zobrazi pro kazdou tridu W_2025, varianta a rozdil varianta - W_2025.
- Pan, zoom, vyhledavani linku, ridke ciselne popisky a nativni WKTPOLY geometrie zustavaji zachovane.
- Konecny sdileci ZIP obsahuje pouze samostatne HTML soubory.

Jednotky jsou vozidla za analyticke obdobi AP, nikoli automaticky RPDI.
Oba smery linku zustavaji oddelene a nesectou se.

## Scenare

Zpracovavaji se scenare 1-69 vcetne:

- W_2022, W_2025, W_2040, W_2050, W_2060, W_2070,
- vsech 15 variant pro roky 2040, 2050, 2060 a 2070,
- scenaru 52-54 Fialova a zelena varianta - S1/S2/S3 - rok 2040.

S1 = C 24,5/120 v plnem profilu
S2 = C 24,5/120 v polovicnim profilu
S3 = C 11,5/90

Autoritou identity je cislo scenare. To je dulezite hlavne pro scenare s nestandardnim kodem.

## Python

Pouziva se pouze EXTERNI 64bitovy Python s pywin32. Python dodany s Visumem je blokovan.
Predvyplnena cesta k projektu a output_root je v CONFIGURE_PATHS.json.

## Postup

### A. Zakladni export geometrie a celkovych intenzit

1. `00_CHECK_EXTERNAL_PYTHON.bat`
2. `01_SCAN_RESULTS.bat`
3. Otevrit `C:\Users\vilem_knap\R1_VISUM_DIFF_RESULTS\scenarios.csv` a zkontrolovat, ze vsech 69 radku ma nalezeny result.ver.
4. `02_RUN_EXPORT.bat`
5. `06_VERIFY_OUTPUTS.bat`

Uspech zakladniho exportu:

- `RUN_ALL_69_ABS_DIFF_V25_BASE\00_STATUS.json` ma `status = PASS`
- `versions_exported = 69`
- `absolute_cartograms_created = 69`
- `cartograms_created = 68`
- `total_cartograms_created = 137`
- `errors = []`

Zakladni export pouze nacita ulozene result.ver. Nespousti assignment a neuklada zadny model.

### B. Export skladby OA / LNV / NV

6. `10_PROBE_VEHICLE_CLASSES.bat`
   - otevre pouze W_2025,
   - zjisti skutecne demand segmenty,
   - vybere jednoznacne mapovani OA/LNV/NV,
   - overi soucet proti VOLVEHPRT(AP).

7. `11_EXPORT_VEHICLE_CLASSES.bat`
   - postupne nacte vsech 69 result.ver,
   - vyexportuje skladbu vozidel,
   - pri opakovanem spusteni pouzije overenou cache.

Uspech:
`RUN_ALL_69_V25_CLASSES_EXPORT\00_CLASSES_STATUS.json` ma `status = PASS`, `selected = 69`, `exported = 69`, `errors = []`.

### C. HTML mapy se skladbou vozidel

8. `12_PREPARE_HTML_MAPS.bat`
9. `13_VERIFY_HTML_MAPS.bat`

Hotove mapy:
`C:\Users\vilem_knap\R1_VISUM_DIFF_RESULTS\RUN_ALL_69_V25_HTML_VEHICLE_CLASSES\INDEX.html`

Ocekavany stav:

- 69 absolutnich HTML,
- 68 rozdilovych HTML,
- 137 map celkem,
- geometrie a puvodni celkove intenzity beze zmen,
- OA + LNV + NV (+ explicitni OTHER, pokud by byl nastaven) = Celkem.

### D. Maly balik pro odeslani

10. `14_ZIP_HTML_ONLY.bat`

Vznikne:
`C:\Users\vilem_knap\R1_VISUM_DIFF_RESULTS\R1_KARTOGRAMY_HTML_OA_LNV_NV_ALL_69.zip`

Archiv obsahuje pouze 137 samostatnych HTML souboru. Zadny CSV, GPKG, SVG, Python, log ani INDEX.html.
Kazdy HTML je offline a obsahuje geometrii i hodnoty uvnitr souboru.

## Vozova skladba

Nastroj neodhaduje OA jako zbytek celku. Nejprve overi skutecne demand segmenty ve W_2025.
Zname podporovane profily jsou:

- `R1_STANDARD_6`: OA = CW + CW_noT + CS + CS_noT + CL + CF; LNV = LNV; NV = NV.
- `R1_NATIVE_AND_EXOG_12`: OA obsahuje stejne sestice v NATIVE_ a EXOG_EE_ vetvi; LNV = LNV; NV = NV.
- `R1_C_AND_CNOT`: OA = C + C_noT pouze pokud jde o registrovane demand segmenty.

Pouzije se pouze takovy profil, ktery na kazdem smerovem linku splni soucet proti celku.
Chybejici atribut se nikdy nenahrazuje nulou.

## Bezpecnost modelu

Skripty nepouzivaji SaveVersion, SetAttValue ani Procedures.Execute.
Result.ver se nacitaji read-only do samostatne COM instance `Visum.Visum.260`.
Kontrolni SHA256 puvodnich result.ver brani spojeni skladby vozidel s jinym behem site.

## Testovani dodavky

Soucasti baliku je TEST_REPORT.json.
Na syntetickych datech probehl kompletni retezec 69 result.ver -> 137 zakladnich map -> 69 exportu skladby -> 137 obohacenych HTML -> HTML-only ZIP.
Skutecny Windows COM beh nad produkcnimi result.ver samozrejme musi probehnout na pocitaci s PTV Visum 2026.
