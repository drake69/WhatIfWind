# Case study: two small VAWTs on a rooftop anemometer

October 2026. Wind data: an Ecowitt GW1100A on a roof at about 6.5 m, hourly recorder statistics from 2024-11-05 to 2026-10-19 (99.9% coverage) plus 10 days of raw samples. Model: WhatIfWind v0.2.1 (swept area x Cp x air density x v^3, default losses, capped at the nameplate power).

**In one line:** with the measured wind (mean 0.74 m/s, only 2.6% of hours averaging above 3 m/s), two marketplace VAWTs with a 0.53 x 0.75 m rotor would produce **3 to 5 kWh per year**, about **1 EUR per year**. The purchase never pays back.

## Comparison

| Turbine | Price | Axis / blades | Declared power | Rotor (D x H) | Swept area | Start-up / cut-in | Rated wind | Max wind | kWh/year (lower to central) | EUR/year at 0.25 EUR/kWh | Payback |
|---|---|---|---|---|---|---|---|---|---|---|---|
| [VAWT kit "6000W" 220V (Amazon)](https://www.amazon.it/dp/B0FZFN19M6) | 328.99 EUR | vertical H-rotor, 3 | 6000 W | 0.53 x 0.75 m | 0.40 m² | 2 / 3 m/s | 10 m/s | 40 m/s | 2.5 to 4.1 | 0.6 to 1.0 | over 300 years |
| [SMARAAD SX3-3000 (AliExpress)](https://it.aliexpress.com/item/1005010801369542.html) | not recorded | vertical, 3 | 3000 W | 0.53 x 0.75 m | 0.40 m² | 1.5 m/s | 12 m/s | 50 m/s | 3.0 to 4.8 | 0.8 to 1.2 | n/a |

kWh/year = last 12 months. The energy price (0.25 EUR/kWh) is an assumption. A third listing (a "5000W" 12V kit) was dropped because the seller does not state the rotor size, and without the swept area no estimate is possible.

## The declared ratings are physically impossible

Through a 0.40 m² rotor, a 10 m/s wind carries about 243 W. The Betz limit (59.3%) leaves at most **144 W**; with a realistic Cp of 0.32 and losses, WhatIfWind estimates about **68 W**. The "6000 W" and "3000 W" nameplates are 20 to 40 times beyond the physical limit. That is why WhatIfWind computes from the geometry and uses the nameplate only as a cap, which is never reached here: the highest simulated hourly power in two years is 135 W.

## Rule of thumb for this site

At this site a realistic turbine yields about **10 kWh per m² of rotor per year** (about 22 kWh/m² for an ideal rotor at the Betz limit with no losses). Mounting it on a 12 m mast instead of at roof height adds 45 to 75%.

| Rotor area | Example | kWh/year | EUR/year |
|---|---|---|---|
| 0.4 m² | the two kits above | 3 to 5 | about 1 |
| 3 m² | 3 m diameter class | 30 to 50 | 8 to 12 |
| 10 m² | large domestic turbine | 100 to 170 | 25 to 40 |

Rows after the first scale with area and are approximate: cut-in and Cp differ by model.

## Method

- **Two estimates per hour.** *Lower*: the wind is constant at the hourly mean. *Central*: wind within the hour follows a Rayleigh distribution with that mean, truncated at the hourly maximum. Both are needed because power goes with the cube of wind speed: the hourly mean alone underestimates (median within-hour cube factor of 4, measured on the raw samples).
- **Check on 10 days of raw samples** (one every 60 s): the first kit gives 10.3 Wh from raw data against 2.1 Wh (lower) and 15.3 Wh (central); the second gives 21.3 against 11.7 and 25.8. The true value lies between the two, closer to the central one.
- **Cut-in.** The first kit states start-up 2 m/s and cut-in 3 m/s: 3 is used. The second states only a 1.5 m/s start-up, used as cut-in, which favours the seller.
- **Cp 0.32** (WhatIfWind default for H-rotors), air density 1.225 kg/m³.
- **Height.** The anemometer is on the roof at 6 to 7 m, inside the declared installation range (2 to 12 m), so no correction is applied. At 12 m, with a power-law profile and exponent 0.2 to 0.3 (residential area), wind speed rises 13 to 20% and energy 44 to 74%: the central estimate becomes **6 to 8 kWh per year**, still a payback of over 150 years.
- **Data quality.** Two storms (26 and 31 March 2026, 9 to 11 m/s hourly means for 6+ consecutive hours) were checked and kept as real events.

## Home Assistant configuration

Both turbines were entered from the WhatIfWind options menu with these parameters:

| Name | Subtype | Diameter | Height | Nameplate | Cp | Cut-in | Cut-out |
|---|---|---|---|---|---|---|---|
| Amazon VAWT H 6000W 220V | H-rotor | 0.53 m | 0.75 m | 6000 W | 0.32 | 3.0 m/s | 40 m/s |
| AliExpress SMARAAD SX3-3000 | H-rotor | 0.53 m | 0.75 m | 3000 W | 0.32 | 1.5 m/s | 50 m/s |
