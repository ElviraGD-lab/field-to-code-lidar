---
title: "Prologue: Forest, snow and wires on the Pajares Pass"
---

## Why Pajares

**Pajares is the main pass through the Cantabrian Mountains between Asturias and León. It's also one of the hardest places for anything that has to cross it.**

Let's look at what's packed into just a few kilometres here.

- **Roads and railways.** The old railway, opened in 1884, runs through 63 tunnels one after another.[^1] The new one opened only in 2023, and 80% of its route is in tunnels.[^2] The N-630 climbs almost 20 km from Campomanes up to the pass.[^3] When a landslide closes the AP-66 motorway, it's the only road between Asturias and León.[^4]
- **Winter.** Snowploughs stand by at the pass all winter.[^5] On 31 January 2023, the Pajares-Valgrande weather station recorded −9.3 °C, the lowest temperature anywhere in Asturias that month.[^6]
- **Electricity.** 540 km of 400 kV lines run through Asturias,[^7] and lines like these cross the pass too. The villages around it are supplied by the distribution company Viesgo.[^8]
- **Terrain and forest.** A ridge, a deep valley, then another ridge. Over a few kilometres, the height changes by more than a kilometre.[^9] Beech grows on the shady slopes and oak on the sunny ones,[^10] so the forest grows differently even on neighbouring slopes.

**Everything this project is about – slopes, forest, wires, roads and snow – comes together here in one place.**

## What happens here in winter

Let's look at two cases from 2018. They show what matters better than any explanation.

**February 2018.** A storm with snow. Around 10,000 customers are left without power. Asturias's 112 emergency service names the cause: trees are falling and slopes are slipping.[^11] Repairs are slow because the crews can't get to the faults. The same fallen trees are lying across the roads, and snow has closed the passes. Once the power goes, communications follow, because the telecoms stations have lost power too.[^11] In Pajares, people spend the night without heating and light their wood stoves.[^12]

**October 2018.** Heavy snow falls while the trees are still in leaf. The snow settles on the leaves, and the crown becomes very heavy. The tree snaps or is uprooted and falls onto the wires.[^13] On the third day, 679 Viesgo customers are still without power, 384 of them in Pajares.[^14]

**So the same trees both break the wires and keep the repair crews from reaching them.**

## It's not just Pajares

At first we might think this is a local quirk. Let's look at other countries.

- **USA.** Vegetation is the most common cause of outages. In severe storms, most of the damage comes from trees standing outside the right-of-way corridor.[^15] In Virginia, a review of more than 1,100 cases found that almost half of the fallen trees had no visible defect at all.[^16] So we can't spot such a tree just by looking at it.
- **United Kingdom.** Storm Arwen in 2021 left more than a million customers without power. Most of the damage was caused by wind and by trees on the lines.[^17]
- **Finland.** Trees fall onto lines in strong winds or under heavy wet snow on their crowns. In the worst cases, between 100,000 and 400,000 customers are left without power for a long time.[^18]

**In Spain, the mechanism depends on the place.** In Andalusia in winter 2026, the wind snapped 252 conductors, while there were only 53 trees on lines.[^19] There, it's mostly the wind itself that breaks the network. In Asturias in 2018 it was something else: wet snow on trees in leaf. That's closer to Finland.

**And then there are fires.** A burnt tree can stay standing after a fire, then snap in the very first strong wind.[^20]

**So the question is a general one: which trees near the lines will fall in a storm?**

## What's missing

Why can't the companies simply remove the dangerous trees in advance? We see two reasons.

- **The rule doesn't take storms into account.** The regulations say that if a tree could reach a wire as it falls, it has to be removed.[^21] But they don't ask whether that tree will actually fall. On a mountain slope there are a great many trees that 'could reach', and the company needs to know which ones to start with.
- **There's no open data.** The ministry publishes how many hours the power was off only by province.[^22] Spain has no open data on which outages were caused by trees, snow or wind. The US has an open dataset for exactly this, EAGLE-I: outages by county every 15 minutes, since 2014.[^23]

**To know which trees to start with, we need to take storms into account. And to check that, we need data on storms and outages.**

## One question, three parts

**The core question of the Pajares project is this: where is the forest along the lines most likely to fall onto the wires in a storm?**

Let's work out what we need to know to answer it.

1. **How tall the trees are now, and how tall they'll be in a few years.** That's part 1, `canopy-growth`: the growth rate of the forest.
2. **Where and when storms, wet snow and fires brought trees down and left people without power.** That's part 2, `storm-forest-damage`: an open catalogue of these events.
3. **Where these two kinds of knowledge together point to high risk.** That's part 3, `powerline-treefall-risk`: a risk map by line section.

Each part is useful on its own. Forestry services need forest growth rates even where there isn't a single line nearby. The 112 services need the event catalogue. But we can't build part 3 without the first two: without growth, it doesn't know how tall the trees will be, and without events, there's nothing to test it on.

All three parts run on free national data: PNOA LiDAR, AEMET observations and fire maps. So anyone can check the work and repeat it on any site in Spain. We chose Pajares because it's the hardest place: if the method works here, it stands a chance of working elsewhere too.

## What others are already doing

We're not the first to think about trees and power lines.

- **Iberdrola with Woza.** Forecasting tree falls with wind taken into account; a pilot in Biscay. The results haven't been published.[^24]
- **Viesgo.** Its own vegetation growth model, built from two LiDAR flights.[^25]
- **Norway, 2026.** Forecasting tree falls along lines from drone data.[^26]
- **Tanase and co-authors, 2026.** Forest height growth from PNOA surveys for the whole of mainland Spain: about 0.2 m a year on average.[^27]

**What sets us apart: open, verifiable, built on national data, for any site, with an honest error margin, and together with the storms.**

## Where we start

**We start with forest growth, because both of the next parts rest on it.**

A tree that can't reach a wire today may reach it in a few years. Spain has already surveyed the whole country with LiDAR several times, and these surveys are free. So we can compare the height of the forest in two surveys and find out how much it has grown.

The difficulty is that the surveys differ: different years, instruments and seasons, and different numbers of points. So a single figure in 'metres per year' isn't enough for us. We also need to show honestly how far we can trust that figure, and where the older surveys simply don't allow us to measure it. That's what `canopy-growth` does, and it's the work we're going to do together.

---

## Notes and sources

Sources checked on 19 November 2026.

[^1]: [Wikipedia, 'Pajares Ramp'](https://en.wikipedia.org/wiki/Pajares_Ramp).
[^2]: [La Moncloa, 'Sánchez inaugura la variante de Pajares', 29 November 2023](https://www.lamoncloa.gob.es/presidente/actividades/Paginas/2023/291123-sanchez-inauguracion-variante-pajares.aspx).
[^3]: [Climbfinder, 'Puerto de Pajares from Campomanes'](https://climbfinder.com/en/climbs/puerto-de-pajares-campomanes).
[^4]: [Diario de León, 11 November 2024: the AP-66 closed by a landslide; the N-630 over Pajares the only route](https://www.diariodeleon.es/leon/241111/1678770/nieve-amenaza-bloqueo-asturias-carretera-pajares-alerta-ap-66-cerrada-desprendimiento.html).
[^5]: [Government Delegation in Asturias, 12 November 2024](https://mptmd.gob.es/en/portal/delegaciones_gobierno/delegaciones/asturias/actualidad/2024/11/2024_11_12).
[^6]: [AEMET, *Avance climatológico, Asturias*, January 2023](https://www.aemet.es/documentos/es/serviciosclimaticos/vigilancia_clima/resumenes_climat/ccaa/asturias/avance_climat_ast_ene_2023.pdf).
[^7]: [Red Eléctrica, *El sistema eléctrico en Asturias*, 2026](https://www.ree.es/sites/default/files/publication/2026/07/downloadable/folleto-asturias-digital.pdf).
[^8]: [EDP, 'EDP and Macquarie complete Viesgo transaction', 2020](https://www.edp.com/en/news/edp-and-macquarie-complete-viesgo-transaction).
[^9]: [IGN, MDT02 digital terrain model, CNIG download centre. Heights and profile computed by us over the project's 8 × 8 km area](https://centrodedescargas.cnig.es).
[^10]: [Beato Bergua, S., Marino Alfonso, J. L. and Poblete Piedrabuena, M. Á. (2017). El paisaje vegetal y los hábitats forestales de interés comunitario en la Montaña Central Asturiana. *Cuadernos Geográficos* 56(1): 26–52](https://revistaseug.ugr.es/index.php/cuadgeo/article/view/4834/5308).
[^11]: [112 Asturias, February 2018: coordinating the restoration of power and telephone services](https://www.112asturias.es/112-asturias-coordinara-actuaciones-para-restablecimiento-suministro-electrico-telefonico).
[^12]: [Público, 'Sin luz ni calefacción en pleno temporal de nieve', February 2018](https://www.publico.es/sociedad/video-luz-calefaccion-pleno-temporal-nieve.html).
[^13]: [La Voz del Trubia, 29 October 2018](https://lavozdeltrubia.es/2018/10/29/somiedo-teverga-y-parte-de-quiros-siguen-sin-luz-por-el-temporal/).
[^14]: [OKDiario, 30 October 2018](https://okdiario.com/espana/temporal-deja-sin-luz-tres-dias-mas-4-500-personas-asturias-3291859).
[^15]: [U.S. Department of Energy, *Vegetation Management Resilience Investment Guide*, 2024](https://www.energy.gov/sites/default/files/2024-11/111524_Vegetation_Management.pdf).
[^16]: ['An Analysis of Electric Distribution Tree-Caused Outages'. *Arboriculture & Urban Forestry*, 2026](https://doi.org/10.48044/jauf.2026.016).
[^17]: [Ofgem, review into networks' response to Storm Arwen](https://ofgem.gov.uk/publications/ofgem-publishes-full-report-following-six-month-review-networks-response-storm-arwen).
[^18]: [World Energy Council, *Severe Storms Finland 2001–2007*](https://www.worldenergy.org/assets/downloads/Severe_Storms_Finland_2001_2007_Extreme_Weather_Conditions_DEC19.pdf).
[^19]: [Endesa, grid resilience to extreme weather, winter 2026](https://www.endesa.com/es/prensa/sala-de-prensa/noticias/sector-energetico/resilencia-red-fenomenos-climaticos-extremos-invierno-2026).
[^20]: [Bonneville Power Administration, 'How wildfires can contribute to power outages'](https://www.bpa.gov/learn-and-participate/community-education/wildfires-and-public-safety).
[^21]: [ITC-LAT 07 (Royal Decree 223/2008), section 5.12.1](https://normatia.com/es/normativa/reglamento-lineas-alta-tension/2025/instruccion-tecnica-complementaria-itc-lat-07-lineas-aereas-con-conductores/).
[^22]: [MITECO, electricity service quality indices](https://www.miteco.gob.es/es/energia/energia-electrica/electricidad/calidad-servicio/indices.html).
[^23]: [Oak Ridge National Laboratory, *EAGLE-I Power Outage Data 2014–2022*](https://doi.org/10.13139/ORNLNCCS/1975202).
[^24]: [Iberdrola, PERSEO programme, resilience of power grids to natural disasters](https://www.iberdrola.com/conocenos/nuestro-modelo-innovacion/programa-internacional-startups-perseo/resiliencia-redes-electricas-ante-desastres-naturales).
[^25]: [Viesgo, 'Las redes de distribución eléctrica del futuro', University of Cantabria seminar](https://web.unican.es/centros/minas/Documents/Cursos/Presentacion%20Jornadas%20Mineria%20y%20Energia-Viesgo%20v5.pdf).
[^26]: [Merlin, Gardiner and Solberg (2026). *Natural Hazards and Earth System Sciences* 26: 2461](https://doi.org/10.5194/nhess-26-2461-2026).
[^27]: [Tanase et al. (2026). *Annals of Forest Research* 69(1): 3–20](https://doi.org/10.15287/afr.2026.4341).
