---
title: "Prólogo. Bosque, nieve y cables en el puerto de Pajares"
---

## Por qué Pajares

**Pajares es el gran puerto de la cordillera Cantábrica entre Asturias y León. Y uno de los lugares más difíciles para todo lo que lo atraviesa.**

Veamos lo que se reúne aquí en unos pocos kilómetros.

- **Los caminos.** El ferrocarril antiguo, inaugurado en 1884, atraviesa 63 túneles seguidos[^1]. El nuevo no se abrió hasta 2023, y el 80 % de su recorrido va en túnel[^2]. La N-630 sube al puerto desde Campomanes durante casi 20 km[^3]. Cuando un argayo corta la autopista AP-66, es la única carretera entre Asturias y León[^4].
- **El invierno.** Las quitanieves están de guardia en el puerto todo el invierno[^5]. En la estación meteorológica de Pajares-Valgrande se registraron −9,3 °C el 31 de enero de 2023, la temperatura más baja del mes en toda Asturias[^6].
- **La electricidad.** Por Asturias pasan 540 km de líneas de 400 kV[^7], y también cruzan el puerto. A los pueblos de alrededor los abastece la distribuidora Viesgo[^8].
- **El relieve y el bosque.** Una sierra, un valle profundo, otra sierra. En pocos kilómetros, el desnivel supera el kilómetro[^9]. El haya crece en las laderas de umbría y el roble en las de solana[^10], así que el bosque crece de forma distinta incluso en dos laderas vecinas.

**Todo lo que trata este proyecto, las laderas, el bosque, los cables, las carreteras y la nieve, está aquí reunido en un mismo lugar.**

## Qué pasa aquí en invierno

Veamos dos episodios de 2018. Muestran lo esencial mejor que cualquier explicación.

**Febrero de 2018.** Temporal de nieve. Se quedan sin luz unos 10 000 abonados. El 112 Asturias señala la causa: caen árboles y se desprenden laderas[^11]. La reparación va lenta porque las brigadas no pueden llegar a las averías. En las carreteras están esos mismos árboles caídos, y los puertos están cerrados por la nieve. Tras la luz se van también las comunicaciones, porque las antenas se han quedado sin electricidad[^11]. En Pajares la gente pasa la noche sin calefacción y enciende las cocinas de leña[^12].

**Octubre de 2018.** Cae una nevada fuerte cuando los árboles todavía están en hoja. La nieve se posa sobre las hojas y la copa se vuelve muy pesada. El árbol se parte o se arranca de raíz y cae sobre los cables[^13]. Al tercer día, Viesgo sigue teniendo 679 clientes sin luz, 384 de ellos en Pajares[^14].

**O sea, los mismos árboles que rompen los cables impiden llegar a repararlos.**

## No es solo Pajares

Al principio podríamos pensar que es algo propio de este sitio. Miremos otros países.

- **Estados Unidos.** La vegetación es la causa más frecuente de los cortes. En los temporales fuertes, el mayor daño lo hacen los árboles que están fuera de la calle de seguridad[^15]. En Virginia se analizaron más de 1100 casos: casi la mitad de los árboles caídos no tenía ningún defecto visible[^16]. Así que un árbol así no se encuentra a simple vista.
- **Reino Unido.** La borrasca Arwen dejó sin luz en 2021 a más de un millón de clientes. La mayoría de las averías las causaron el viento y los árboles sobre las líneas[^17].
- **Finlandia.** Los árboles caen sobre las líneas por el viento fuerte o por el peso de la nieve húmeda en las copas. En los peores casos, entre 100 000 y 400 000 clientes se quedan sin luz durante mucho tiempo[^18].

**En España, en cambio, el mecanismo depende del lugar.** En Andalucía, en el invierno de 2026, el viento rompió 252 conductores, mientras que los árboles sobre las líneas fueron 53[^19]. Allí lo que rompe la red es sobre todo el propio viento. En Asturias, en 2018, pasó otra cosa: nieve húmeda sobre árboles en hoja. Eso se parece más a Finlandia.

**Y están los incendios.** Un árbol quemado puede seguir en pie después del fuego y luego partirse con el primer viento fuerte[^20].

**Así que la pregunta es general: qué árboles junto a las líneas caerán en un temporal.**

## Qué falta

¿Por qué las distribuidoras no pueden quitar de antemano los árboles peligrosos sin más? Vemos dos motivos.

- **La norma no tiene en cuenta el temporal.** La norma dice que, si un árbol puede alcanzar el conductor al caer, hay que quitarlo[^21]. Pero no pregunta si ese árbol va a caer. En una ladera de montaña, los árboles que «pueden alcanzar» son muchísimos, y la distribuidora necesita saber por cuáles empezar.
- **No hay datos abiertos.** El Ministerio publica cuántas horas faltó la luz solo por provincias[^22]. En España no hay datos abiertos sobre qué cortes provocaron los árboles, la nieve o el viento. En Estados Unidos existe para eso el conjunto abierto EAGLE-I: cortes por condado cada 15 minutos, desde 2014[^23].

**Para saber por qué árboles empezar, hay que tener en cuenta el temporal. Y para comprobarlo hacen falta datos de temporales y cortes.**

## Una pregunta, tres partes

**La pregunta principal del proyecto Pajares es esta: dónde es más probable que el bosque junto a las líneas caiga sobre los cables en un temporal.**

Veamos qué necesitamos saber para responderla.

1. **Qué altura tienen hoy los árboles y cuál tendrán dentro de unos años.** Es la parte 1, `canopy-growth`: la velocidad de crecimiento del bosque.
2. **Dónde y cuándo los temporales, las nevadas húmedas y los incendios tumbaron árboles y dejaron a la gente sin luz.** Es la parte 2, `storm-forest-damage`: un catálogo abierto de esos episodios.
3. **Dónde esos dos conocimientos juntos dan un riesgo alto.** Es la parte 3, `powerline-treefall-risk`: un mapa de riesgo por tramos de línea.

Cada parte es útil por sí misma. La velocidad de crecimiento del bosque les sirve a los servicios forestales aunque no haya ninguna línea cerca. El catálogo de episodios le sirve al 112. Pero la parte 3 no se puede construir sin las dos primeras: sin el crecimiento no sabe cómo serán los árboles, y sin los episodios no tiene con qué comprobarse.

Las tres partes funcionan con datos nacionales gratuitos: el LiDAR del PNOA, las observaciones de AEMET y los mapas de incendios. Por eso cualquiera puede comprobar y repetir el trabajo en cualquier zona de España. Elegimos Pajares porque es el lugar más difícil: si el método funciona aquí, tiene posibilidades de funcionar también en otros sitios.

## Qué hacen ya otros

No somos los primeros en pensar en los árboles y las líneas.

- **Iberdrola con la empresa Woza.** Predicción de caída de árboles teniendo en cuenta el viento, con un piloto en Bizkaia. Los resultados no se han publicado[^24].
- **Viesgo.** Un modelo propio de crecimiento de la vegetación a partir de dos vuelos LiDAR[^25].
- **Noruega, 2026.** Predicción de caída de árboles a lo largo de las líneas con datos de dron[^26].
- **Tanase y otros, 2026.** El crecimiento en altura del bosque a partir de los vuelos del PNOA para toda la España peninsular: de media, unos 0,2 m al año[^27].

**Lo que nos distingue: abierto, comprobable, con datos nacionales, para cualquier zona, con un error contado con honestidad y junto con los temporales.**

## Por dónde empezamos

**Empezamos por el crecimiento del bosque, porque en él se apoyan las dos partes siguientes.**

Un árbol que hoy no llega al cable puede llegar dentro de unos años. España ya ha volado varias veces todo el país con LiDAR, y esos vuelos son gratuitos. Así que podemos comparar la altura del bosque en dos vuelos y saber cuánto ha crecido.

La dificultad es que los vuelos son distintos: distintos años, aparatos y estaciones, y distinto número de puntos. Por eso no nos basta con un número de «metros al año». También hay que mostrar con honestidad hasta qué punto podemos fiarnos de ese número y dónde los vuelos antiguos sencillamente no lo permiten. De eso se encarga `canopy-growth`, y ese trabajo lo hacemos juntos.

---

## Notas y fuentes

Fuentes consultadas el 19 de noviembre de 2026.

[^1]: [Wikipedia, «Pajares Ramp»](https://en.wikipedia.org/wiki/Pajares_Ramp) (en inglés).
[^2]: [La Moncloa, «Sánchez inaugura la variante de Pajares», 29 de noviembre de 2023](https://www.lamoncloa.gob.es/presidente/actividades/Paginas/2023/291123-sanchez-inauguracion-variante-pajares.aspx).
[^3]: [Climbfinder, «Puerto de Pajares from Campomanes»](https://climbfinder.com/en/climbs/puerto-de-pajares-campomanes) (en inglés).
[^4]: [Diario de León, 11 de noviembre de 2024: la AP-66, cortada por un argayo; la N-630 por Pajares, única vía](https://www.diariodeleon.es/leon/241111/1678770/nieve-amenaza-bloqueo-asturias-carretera-pajares-alerta-ap-66-cerrada-desprendimiento.html).
[^5]: [Delegación del Gobierno en Asturias, 12 de noviembre de 2024](https://mptmd.gob.es/en/portal/delegaciones_gobierno/delegaciones/asturias/actualidad/2024/11/2024_11_12).
[^6]: [AEMET, *Avance climatológico, Asturias*, enero de 2023](https://www.aemet.es/documentos/es/serviciosclimaticos/vigilancia_clima/resumenes_climat/ccaa/asturias/avance_climat_ast_ene_2023.pdf).
[^7]: [Red Eléctrica, *El sistema eléctrico en Asturias*, 2026](https://www.ree.es/sites/default/files/publication/2026/07/downloadable/folleto-asturias-digital.pdf).
[^8]: [EDP, «EDP and Macquarie complete Viesgo transaction», 2020](https://www.edp.com/en/news/edp-and-macquarie-complete-viesgo-transaction) (en inglés).
[^9]: [IGN, Modelo Digital del Terreno MDT02, Centro de Descargas del CNIG. Alturas y perfil calculados por nosotros sobre la zona de 8 × 8 km del proyecto](https://centrodedescargas.cnig.es).
[^10]: [Beato Bergua, S., Marino Alfonso, J. L. y Poblete Piedrabuena, M. Á. (2017). El paisaje vegetal y los hábitats forestales de interés comunitario en la Montaña Central Asturiana. *Cuadernos Geográficos* 56(1): 26–52](https://revistaseug.ugr.es/index.php/cuadgeo/article/view/4834/5308).
[^11]: [112 Asturias, febrero de 2018: coordinación del restablecimiento del suministro eléctrico y telefónico](https://www.112asturias.es/112-asturias-coordinara-actuaciones-para-restablecimiento-suministro-electrico-telefonico).
[^12]: [Público, «Sin luz ni calefacción en pleno temporal de nieve», febrero de 2018](https://www.publico.es/sociedad/video-luz-calefaccion-pleno-temporal-nieve.html).
[^13]: [La Voz del Trubia, 29 de octubre de 2018](https://lavozdeltrubia.es/2018/10/29/somiedo-teverga-y-parte-de-quiros-siguen-sin-luz-por-el-temporal/).
[^14]: [OKDiario, 30 de octubre de 2018](https://okdiario.com/espana/temporal-deja-sin-luz-tres-dias-mas-4-500-personas-asturias-3291859).
[^15]: [U.S. Department of Energy, *Vegetation Management Resilience Investment Guide*, 2024](https://www.energy.gov/sites/default/files/2024-11/111524_Vegetation_Management.pdf) (en inglés).
[^16]: [«An Analysis of Electric Distribution Tree-Caused Outages». *Arboriculture & Urban Forestry*, 2026](https://doi.org/10.48044/jauf.2026.016) (en inglés).
[^17]: [Ofgem, revisión de la respuesta de las redes a la borrasca Arwen](https://ofgem.gov.uk/publications/ofgem-publishes-full-report-following-six-month-review-networks-response-storm-arwen) (en inglés).
[^18]: [World Energy Council, *Severe Storms Finland 2001–2007*](https://www.worldenergy.org/assets/downloads/Severe_Storms_Finland_2001_2007_Extreme_Weather_Conditions_DEC19.pdf) (en inglés).
[^19]: [Endesa, resiliencia de la red ante fenómenos climáticos extremos, invierno de 2026](https://www.endesa.com/es/prensa/sala-de-prensa/noticias/sector-energetico/resilencia-red-fenomenos-climaticos-extremos-invierno-2026).
[^20]: [Bonneville Power Administration, «How wildfires can contribute to power outages»](https://www.bpa.gov/learn-and-participate/community-education/wildfires-and-public-safety) (en inglés).
[^21]: [ITC-LAT 07 (Real Decreto 223/2008), apartado 5.12.1](https://normatia.com/es/normativa/reglamento-lineas-alta-tension/2025/instruccion-tecnica-complementaria-itc-lat-07-lineas-aereas-con-conductores/).
[^22]: [MITECO, índices de calidad del servicio eléctrico](https://www.miteco.gob.es/es/energia/energia-electrica/electricidad/calidad-servicio/indices.html).
[^23]: [Oak Ridge National Laboratory, *EAGLE-I Power Outage Data 2014–2022*](https://doi.org/10.13139/ORNLNCCS/1975202) (en inglés).
[^24]: [Iberdrola, programa PERSEO, resiliencia de las redes eléctricas ante desastres naturales](https://www.iberdrola.com/conocenos/nuestro-modelo-innovacion/programa-internacional-startups-perseo/resiliencia-redes-electricas-ante-desastres-naturales).
[^25]: [Viesgo, «Las redes de distribución eléctrica del futuro», jornadas de la Universidad de Cantabria](https://web.unican.es/centros/minas/Documents/Cursos/Presentacion%20Jornadas%20Mineria%20y%20Energia-Viesgo%20v5.pdf).
[^26]: [Merlin, Gardiner y Solberg (2026). *Natural Hazards and Earth System Sciences* 26: 2461](https://doi.org/10.5194/nhess-26-2461-2026) (en inglés).
[^27]: [Tanase y otros (2026). *Annals of Forest Research* 69(1): 3–20](https://doi.org/10.15287/afr.2026.4341) (en inglés).
