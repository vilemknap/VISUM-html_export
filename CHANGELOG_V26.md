# V2.6 - direct TSys vehicle composition

- odstraneno krehke mapovani podle demand segmentu,
- OA se exportuje jako `VolVeh_TSys(C,AP) + VolVeh_TSys(C_noT,AP)`,
- LNV se exportuje z `VolVeh_TSys(LNV,AP)`,
- NV se exportuje z `VolVeh_TSys(NV,AP)`,
- F_T je pouze diagnosticky TSys a nezobrazuje se jako OA/LNV/NV,
- `VolVehPrT(AP)` se kontroluje proti souctu vsech citelnych PrT TSys,
- dalsi PrT TSys jsou zaznamenany jako `UNMAPPED_PRT`, ne jako fatalni zmena mapovani,
- `ROAD_TOTAL = OA + LNV + NV` je uzivatelsky celkem v klikacim panelu,
- puvodni kartogramovy `VOLVEHPRT(AP)` zustava beze zmeny a slouzi i jako QA autorita,
- zachovano 69 scenaru, 137 HTML, spravne nazvy variant a profily S1/S2/S3,
- zachovan external-Python-only a read-only Visum workflow,
- produkcni export a nezavisla QA: PASS pro 137/137 map.
