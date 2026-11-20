---
title: "Cómo leer este libro"
---

## El proyecto `canopy-growth`

**`canopy-growth` es una herramienta abierta que mide a qué velocidad crece el bosque a partir de los vuelos LiDAR repetidos y gratuitos del PNOA.** Es la primera parte del proyecto Pajares.

**Qué problema resuelve.** Un árbol que hoy no llega al cable puede llegar dentro de unos años. Para saber dónde y cuándo el bosque junto a las líneas se volverá peligroso, no basta con conocer su altura: necesitamos también su velocidad de crecimiento. España ya ha volado varias veces todo el país con LiDAR. Con esos vuelos medimos cuánto ha crecido el bosque, y decimos con honestidad dónde los vuelos antiguos no permiten medirlo.

**Para quién.**

- Las distribuidoras, para planificar la poda y la tala junto a las líneas.
- Los servicios forestales y los investigadores, para saber cómo crece el bosque en la montaña.
- Las partes 2 y 3 del proyecto Pajares, que necesitan la altura de los árboles hoy y dentro de unos años.

**Qué ofrece.**

- Un mapa de la velocidad de crecimiento en altura del bosque, en metros al año, en cuadrículas de 20–30 m y con su intervalo.
- Una ficha de cada pasada de vuelo: si es válida, si se ha corregido o si se ha descartado, y por qué.
- Un informe de dónde podemos fiarnos de las cifras.
- Un paquete de Python y una línea de comandos, para quien programa. Y un complemento de QGIS, para quien no programa: dibujamos la zona, pulsamos un botón, y el complemento descarga por sí solo las hojas del PNOA y hace el cálculo.

**Qué no promete.**

- El crecimiento de cada árbol por separado.
- Sustituir los vuelos de las propias compañías: ellas tienen decenas de veces más puntos.
- La causa del crecimiento.
- Una predicción que vaya más allá de prolongar sin más la velocidad medida.

**Dónde está ahora.** El repositorio es público en GitHub: [ElviraGD-lab/canopy-growth](https://github.com/ElviraGD-lab/canopy-growth). La versión 0.1.0 es la base del proyecto: el paquete, las pruebas y la comprobación automática. Todavía no sabe calcular nada. Todo lo demás lo construimos en este libro, paso a paso. El proyecto tiene DOI: [10.5281/zenodo.23856842](https://doi.org/10.5281/zenodo.23856842).

## Qué aprendemos en este libro

Construimos `canopy-growth` desde cero y, por el camino, aprendemos todo lo que hace falta para ello:

- **El propio LiDAR.** Cómo mide distancias el láser, cómo una distancia se convierte en un punto con coordenadas y qué ve el LiDAR en el bosque.
- **La nube de puntos y sus archivos.** Cómo son los archivos LAS y LAZ, cómo saber cuándo voló el avión y en qué sistema de altitudes están los puntos.
- **Los vuelos nacionales del PNOA.** Qué ha volado ya España, cómo descargar una hoja y cómo verla.
- **PDAL**, la principal herramienta abierta para nubes de puntos.
- **Los modelos del terreno y de la altura del bosque.** Cómo separar el suelo de los árboles y cómo obtener la altura del dosel.
- **La comparación de vuelos de distintos años.** Cómo hacer encajar vuelos hechos con distintos aparatos, en distintos años y estaciones, y cómo distinguir el crecimiento real de los errores del vuelo.
- **La herramienta.** El paquete de Python, las pruebas, la comprobación automática, la publicación con DOI y el complemento de QGIS.

**Lo que aquí no explicamos en detalle.** Ya está en otros libros de la serie. Aquí lo usamos y mostramos cómo razonamos:

- Python y PyQGIS: [libro 1, *PyQGIS from Scratch*](https://elviragd-lab.github.io/field-to-code-pyqgis/);
- imágenes de satélite, luz y relieve: [libro 2, *Remote Sensing from Scratch*](https://elviragd-lab.github.io/field-to-code-remote-sensing/);
- estadística (mediana, error, intervalos): [libro 3, *Spatial Statistics from Scratch*](https://elviragd-lab.github.io/field-to-code-statistics/).

## Para quién es este libro

Para ingenieros y técnicos de campo que conocen su trabajo pero no conocen el LiDAR. Por ejemplo, para un topógrafo que ha trabajado con estación total y GNSS y quiere aprender a trabajar con nubes de puntos como se hace hoy en el sector.

No hace falta saber de LiDAR de antemano: cada concepto lo explicamos donde lo necesitamos por primera vez. Sí viene bien tener un nivel de Python como el del libro 1.

## Qué usamos

Todos los programas son gratuitos y abiertos.

- **QGIS**, para ver con nuestros propios ojos las nubes de puntos y los mapas. Versión 4.2 LTR, como en el libro 1. Se descarga en [qgis.org](https://qgis.org).
- **Conda**, un programa que instala Python y todas las bibliotecas necesarias en un entorno aparte. Así todos tenemos las mismas versiones y el resultado se puede repetir. El entorno está descrito en el archivo `environment.yml` del repositorio `canopy-growth`.
- **Python 3.12** y, dentro del entorno, estas bibliotecas:
  - PDAL y python-pdal, para leer, filtrar y procesar nubes de puntos;
  - NumPy, para calcular sobre millones de puntos a la vez;
  - GDAL y rasterio, para los ráster: los modelos del terreno y de la altura del bosque;
  - geopandas, para las capas vectoriales: los límites de la zona y las líneas;
  - matplotlib, para gráficos y figuras;
  - pytest, para las pruebas que comprueban el código.
- **Git y GitHub**, para guardar la historia del proyecto y publicarlo.

Cómo instalarlo todo lo vemos en la lección sobre el entorno. Necesitamos un ordenador con Windows, macOS o Linux, conexión a internet y espacio en disco para las hojas LiDAR.

## Cómo está organizado el libro

**Un capítulo es una etapa del proyecto.** Los capítulos siguen el orden en que se construye `canopy-growth`: primero el problema, luego el propio LiDAR, los archivos, PDAL, los datos, las pasadas de vuelo, el terreno, la altura del bosque, el ruido y la velocidad de crecimiento, la comprobación en campo, la aplicación a otras zonas y, por último, la herramienta terminada.

**Una lección es un paso.** Si un paso es difícil, el capítulo tiene más lecciones.

**Cómo es una lección.**

- **Los pasos del trabajo:** el paso, el código y la explicación de cada línea.
- **✎ Tus datos:** notas donde podemos poner nuestra propia zona o nuestros propios datos. Al final de la lección, una lista de qué datos hacen falta y para qué.
- **📸:** el lugar donde la lección incluye una captura de pantalla.
- **Resumen:** los conceptos nuevos, qué son y un ejemplo.
- **Preguntas de comprensión:** la respuesta va justo debajo de la pregunta. Primero pensamos por nuestra cuenta y luego comprobamos.
- **Ejercicios de memoria:** tres ejercicios cortos para afianzar lo aprendido. Las soluciones están en la página [«Soluciones de los ejercicios»](./solutions.md), al final del libro.

## Sobre el código

**El código, sus comentarios y los rótulos de las figuras están en inglés.** Así el código se puede usar directamente y las figuras son iguales en las dos versiones del libro.

**Todo el código del proyecto vive en el repositorio `canopy-growth`.** Al final de cada capítulo hay un enlace al commit, la versión guardada del código con la que trabaja ese capítulo. Así podemos abrir exactamente el código del que se habla.

**Es mejor teclear el código a mano que copiarlo.** Así las manos y la cabeza lo recuerdan antes. Si el código no funciona, lo comparamos con el libro carácter a carácter: casi siempre la culpa es de un paréntesis o de unas comillas.

## Sobre los errores

**Mostramos los errores, no los escondemos.** Si el resultado no coincide con lo que esperábamos, eso también es parte del trabajo. Analizamos por qué ha pasado y qué hacemos a continuación.

**Comprobamos cada número.** Cada paso termina con una comprobación que puede fallar. Los umbrales de la comprobación los fijamos antes del cálculo, no después.

## Licencias

- **El código del libro y `canopy-growth`:** Apache-2.0. Se puede usar, modificar y aprovechar en el propio trabajo.
- **El texto y las figuras del libro:** CC BY 4.0. Se pueden usar citando a la autora.
- **El complemento de QGIS:** GPL v2 o posterior, porque así lo exige QGIS.
- **Los datos LiDAR:** «PNOA cedido por © Instituto Geográfico Nacional», con licencia CC BY 4.0. No subimos los datos al repositorio: el código descarga las hojas por sí solo.

## Si algo no funciona

Los programas se actualizan y los datos en internet a veces cambian de sitio. Si el código del libro deja de funcionar o encuentras un error en el texto, escríbenos en GitHub, en la sección Issues:

- sobre el libro, en el repositorio [field-to-code-lidar](https://github.com/ElviraGD-lab/field-to-code-lidar/issues);
- sobre la herramienta, en el repositorio [`canopy-growth`](https://github.com/ElviraGD-lab/canopy-growth/issues). Si el error está en la herramienta, ayuda mucho indicar los nombres de las hojas del PNOA, el comando y el texto completo del error.

Así el libro y la herramienta mejoran para todos.
