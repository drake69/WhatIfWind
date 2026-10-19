# Caso di studio: due piccole VAWT su un anemometro da tetto

Ottobre 2026. Dati di vento: un Ecowitt GW1100A sul tetto a circa 6,5 m, statistiche orarie del recorder dal 2024-11-05 al 2026-10-19 (copertura 99,9%) più 10 giorni di campioni grezzi. Modello: WhatIfWind v0.2.1 (area spazzata x Cp x densità dell'aria x v^3, perdite di default, tetto alla potenza di targa).

**In una riga:** con il vento misurato (media 0,74 m/s, solo il 2,6% delle ore sopra i 3 m/s di media), due VAWT da marketplace con rotore 0,53 x 0,75 m produrrebbero **da 3 a 5 kWh l'anno**, circa **1 EUR l'anno**. L'acquisto non rientra mai.

## Confronto

| Turbina | Prezzo | Asse / pale | Potenza dichiarata | Rotore (D x H) | Area spazzata | Avvio / cut-in | Vento nominale | Vento max | kWh/anno (inferiore - centrale) | EUR/anno a 0,25 EUR/kWh | Rientro |
|---|---|---|---|---|---|---|---|---|---|---|---|
| [Kit VAWT "6000W" 220V (Amazon)](https://www.amazon.it/dp/B0FZFN19M6) | 328,99 EUR | verticale H, 3 | 6000 W | 0,53 x 0,75 m | 0,40 m² | 2 / 3 m/s | 10 m/s | 40 m/s | da 2,5 a 4,1 | da 0,6 a 1,0 | oltre 300 anni |
| [SMARAAD SX3-3000 (AliExpress)](https://it.aliexpress.com/item/1005010801369542.html) | non rilevato | verticale, 3 | 3000 W | 0,53 x 0,75 m | 0,40 m² | 1,5 m/s | 12 m/s | 50 m/s | da 3,0 a 4,8 | da 0,8 a 1,2 | n.d. |

kWh/anno = ultimi 12 mesi. Il prezzo dell'energia (0,25 EUR/kWh) è un'ipotesi. Un terzo annuncio (un kit "5000W" 12V) è stato escluso perché il venditore non dichiara le misure del rotore, e senza area spazzata non si può stimare nulla.

## Le potenze dichiarate sono fisicamente impossibili

Attraverso un rotore da 0,40 m², un vento di 10 m/s porta circa 243 W. Il limite di Betz (59,3%) ne lascia al massimo **144 W**; con un Cp realistico di 0,32 e le perdite, WhatIfWind stima circa **68 W**. Le targhe da "6000 W" e "3000 W" sono da 20 a 40 volte oltre il limite fisico. Per questo WhatIfWind calcola dalla geometria e usa la targa solo come tetto, che qui non viene mai raggiunto: la potenza oraria simulata più alta in due anni è 135 W.

## Regola pratica per questo sito

In questo sito una turbina realistica rende circa **10 kWh per m² di rotore all'anno** (circa 22 kWh/m² per un rotore ideale al limite di Betz e senza perdite). Montarla su un palo di 12 m invece che all'altezza del tetto aggiunge dal 45 al 75%.

| Area del rotore | Esempio | kWh/anno | EUR/anno |
|---|---|---|---|
| 0,4 m² | i due kit sopra | da 3 a 5 | circa 1 |
| 3 m² | classe 3 m di diametro | da 30 a 50 | da 8 a 12 |
| 10 m² | grande turbina domestica | da 100 a 170 | da 25 a 40 |

Le righe dopo la prima scalano con l'area e sono approssimate: cut-in e Cp cambiano da modello a modello.

## Metodo

- **Due stime per ora.** *Inferiore*: vento costante pari alla media oraria. *Centrale*: vento dentro l'ora distribuito come una Rayleigh con quella media, troncata al massimo orario. Servono entrambe perché la potenza va col cubo del vento: la sola media oraria sottostima (fattore cubico mediano intra-ora pari a 4, misurato sui campioni grezzi).
- **Verifica su 10 giorni di campioni grezzi** (uno ogni 60 s): il primo kit dà 10,3 Wh dai dati grezzi contro 2,1 Wh (inferiore) e 15,3 Wh (centrale); il secondo 21,3 contro 11,7 e 25,8. Il valore vero sta tra le due stime, più vicino alla centrale.
- **Cut-in.** Il primo kit dichiara avvio a 2 m/s e cut-in a 3 m/s: si usa 3. Il secondo dichiara solo l'avvio a 1,5 m/s, usato come cut-in, il che favorisce il venditore.
- **Cp 0,32** (default di WhatIfWind per gli H-rotor), densità dell'aria 1,225 kg/m³.
- **Quota.** L'anemometro è sul tetto a 6-7 m, dentro il range di installazione dichiarato (da 2 a 12 m), quindi non si applica correzione. A 12 m, con un profilo a legge di potenza ed esponente da 0,2 a 0,3 (zona residenziale), la velocità del vento cresce dal 13 al 20% e l'energia dal 44 al 74%: la stima centrale diventa **da 6 a 8 kWh l'anno**, con un rientro sempre oltre i 150 anni.
- **Qualità dei dati.** Due tempeste (26 e 31 marzo 2026, medie orarie da 9 a 11 m/s per oltre 6 ore consecutive) sono state verificate e tenute come eventi reali.

## Configurazione in Home Assistant

Entrambe le turbine sono state inserite dal menu opzioni di WhatIfWind con questi parametri:

| Nome | Sottotipo | Diametro | Altezza | Targa | Cp | Cut-in | Cut-out |
|---|---|---|---|---|---|---|---|
| Amazon VAWT H 6000W 220V | H-rotor | 0,53 m | 0,75 m | 6000 W | 0,32 | 3,0 m/s | 40 m/s |
| AliExpress SMARAAD SX3-3000 | H-rotor | 0,53 m | 0,75 m | 3000 W | 0,32 | 1,5 m/s | 50 m/s |
