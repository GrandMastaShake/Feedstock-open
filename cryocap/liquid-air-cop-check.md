# Liquid-air cold for data center cooling: a COP check

Alexander Smith, independent researcher. Working note, November 2026. CC BY 4.0.
Also as a two-page [PDF](liquid-air-cop-check.pdf).

This goes with Section 4 (Embodiment Family B, CryoCap) of
[doi.org/10.5281/zenodo.21938808](https://doi.org/10.5281/zenodo.21938808). That record does not
contain this calculation. It is part of a correction being prepared for publication.

## The question

A liquid-air energy store (LAES) releases a lot of cold when it discharges. My August publication
proposed sending some of that cold to a neighbouring data center. What is that cold really worth,
as a COP, against an ordinary chiller at COP 4 to 6?

## Why it is not free

A LAES liquefies cheaply because it recycles its own discharge cold into the next charge. Every
unit of cold sent to the data center is cold the liquefier no longer gets back, so it has to be
re-made with electricity on the next charge. The cost of the cold is the share of the cold-recycle
saving it takes away.

## Cold and exergy per kg of liquid air

Liquid air treated as one component: boils at T_b = 78 K, latent heat 205 kJ/kg, gas
c_p = 1.03 kJ/kg-K, dead state T0 = 288 K.

```
cold, 78 K liquid to 288 K gas   205 + 1.03 x 210               = 421.3 kJ/kg = 0.1170 kWh_th/kg
exergy, latent                   205 x (T0/T_b - 1)              = 551.9 kJ/kg
exergy, sensible                 c_p [T0 ln(T0/T_b) - (T0-T_b)]  = 171.2 kJ/kg
exergy, total                                                    = 723.1 kJ/kg

warm tail, 150 K to 288 K        energy 142.1 kJ/kg = 0.0395 kWh_th/kg  (34 pct of the cold)
                                 exergy  51.4 kJ/kg                     (7.1 pct of the exergy)
```

So the 150 to 288 K tail carries about a third of the cold but only about 7 percent of its
refrigeration value. That tail is the part a data center can use, and it is the part the
liquefier needs least.

## Cost of the cold, and the COP it implies

Published liquefaction work is about 0.50 to 0.56 kWh_e/kg without cold recycle and 0.20 to 0.35
with it, so the recycle saving S is about 0.15 to 0.36 kWh_e/kg, centre near 0.21.

**Key assumption:** the saving is credited to the recycled cold in proportion to exergy. Then:

```
COP_tail = 0.0395 / (0.0710 x S)        COP_full = 0.1170 / S
```

| Recycle saving S (kWh_e/kg) | Tail only (150-288 K) | All the cold (78-288 K) | Chiller |
| --- | --- | --- | --- |
| 0.15 | 3.71 | 0.78 | 4 to 6 |
| 0.21 | 2.65 | 0.56 | 4 to 6 |
| 0.26 | 2.14 | 0.45 | 4 to 6 |
| 0.36 | 1.54 | 0.33 | 4 to 6 |

There is also a free tier: cold the store fails to capture and dumps anyway, roughly 15 percent of
the stream (about 0.0175 kWh_th/kg). It costs nothing and should always be taken.

## Result

Taking only the warm tail gives an effective COP of about 1.5 to 3.7, centre about 2.6. Taking all
the cold gives 0.3 to 0.8. Either way it loses to a chiller, by about 2x for the tail. The cold is
a byproduct worth using where a LAES already exists, not a reason to build one for a data center.
My August record presented the pairing as a benefit; this note is part of correcting that.

## What I would most like checked

1. Is costing the diverted cold as an exergy-proportional share of the recycle saving the right way
   to do it? A real liquefier cold box has its own pinch, so the warm tail might be worth more or
   less to it than its exergy share says. If this allocation is wrong, the whole comparison moves.
2. Is treating liquid air as a single component (one boiling point, one latent heat) good enough at
   this level?
3. Anything else that looks off. Open an [issue](../../../issues) and I'll see it.

Numbers recomputed from the constants above. My working record carried slightly different rounding
(7.24 percent, COP 1.50 to 3.61); the conclusion is the same.
