# R1 VISUM HTML export V2.6 - skladba vozidel primo podle TSys

Aktualni verze exporteru pro vsech 69 vypoctenych scenaru. Produkcni beh nad R1 skoncil PASS pro 69 absolutnich + 68 rozdilovych = 137 HTML map; nezavisla QA potvrdila nezmenenou geometrii u vsech 137 map.

## Vozova skladba

V2.6 uz nepouziva demand segmenty pro odhad skladby. Cte primo vysledkove atributy PTV Visum podle transport systemu:

- `VolVeh_TSys(C,AP)`
- `VolVeh_TSys(C_noT,AP)`
- `VolVeh_TSys(LNV,AP)`
- `VolVeh_TSys(NV,AP)`
- `VolVeh_TSys(F_T,AP)` pouze pro QA, pokud je pritomen
- `VolVehPrT(AP)` jako autoritativni celkovy PrT soucet pro kontrolu

Definice v HTML panelu:

- **OA = C + C_noT**
- **LNV = LNV**
- **NV = NV**
- **Silnicni celkem = OA + LNV + NV**

`F_T` ani dalsi budouci PrT TSys se nepricita do OA/LNV/NV. Zůstava jen v QA. Dalsi neznamy PrT TSys se eviduje jako `UNMAPPED_PRT`, ne jako fatalni zmena mapovani.

## QA reconciliation

Pro kazdy smerovy link se kontroluje:

`VOLVEHPRT(AP) = soucet vsech citelnych PrT VolVeh_TSys(...,AP)`

Geometrie a puvodni kartogramovy `VOLVEHPRT(AP)` se pri doplneni skladby nemeni.

## Rozsah

- 69 scenaru
- 69 absolutnich HTML
- 68 rozdilovych HTML proti W_2025
- 137 HTML celkem
- nativni WKTPOLY geometrie
- pan / zoom / vyhledavani
- spravne nazvy variant a profilu S1/S2/S3
- OA / LNV / NV po kliknuti na smerovy link
- offline HTML bez externich zavislosti

## Spusteni

Pokud uz je zakladni export hotovy, pouzij:

1. `00_CHECK_EXTERNAL_PYTHON.bat`
2. `10_PROBE_VEHICLE_CLASSES.bat`
3. `11_EXPORT_VEHICLE_CLASSES.bat`
4. `12_PREPARE_HTML_MAPS.bat`
5. `13_VERIFY_HTML_MAPS.bat`
6. `14_ZIP_HTML_ONLY.bat`

Zakladni export lze vytvorit pres `01_SCAN_RESULTS.bat`, `02_RUN_EXPORT.bat`, `06_VERIFY_OUTPUTS.bat`.

Pouziva se pouze externi 64bit Python s `pywin32`. Workflow je read-only: zadny assignment, `Procedures.Execute`, `SaveVersion` ani `SetAttValue`.

## Distribuce

Aktualni overeny balik:

`dist/R1_VISUM_ALL69_V2_6_TSYS_VEHICLE_CLASSES_20261101.zip`

SHA-256:

`783019f9275f719efcb1d808f9348c3d1b693992df74ba3ab3a2f72599b8cc4b`

Predchozi V2.5 zustava v historii repozitare.
