---
title: "Cómo leer este libro"
---

## Qué hacemos

**En este libro construimos `canopy-growth`, una herramienta abierta. Mide a qué velocidad crece el bosque a partir de los vuelos LiDAR repetidos y gratuitos que España hace de todo el país.**

Para qué sirve lo vimos en el prólogo. Un árbol que hoy no llega al cable puede llegar dentro de unos años. Para saber dónde y cuándo ocurrirá, no basta con la altura del bosque: necesitamos también su velocidad de crecimiento.

Construimos la herramienta desde cero, paso a paso. Ahora mismo, en el repositorio [`canopy-growth`](https://github.com/ElviraGD-lab/canopy-growth) de GitHub solo está la base: el paquete, las pruebas y la comprobación automática. Todavía no sabe calcular nada. Todo lo demás lo haremos juntos.

## Para quién es este libro

**Para ingenieros que han trabajado en campo con un aparato y conocen su oficio, pero no saben nada de LiDAR.**

Por ejemplo, para un topógrafo que ha levantado con estación total y GNSS, ha hecho planos y ha trabajado en QGIS. Los mapas, los sistemas de coordenadas y QGIS los damos por conocidos. En cambio, el propio LiDAR, las nubes de puntos y sus programas los explicamos desde el principio, en el momento en que nos hacen falta.

La autora de este libro también está aprendiendo. Por eso aquí hay dificultades de verdad, errores de verdad y la forma en que los fuimos resolviendo.

## Qué aprendemos

Todo lo necesario para llegar al resultado del proyecto, por orden:

1. **Qué es el LiDAR.** Cómo mide distancias y cómo una distancia se convierte en un punto con coordenadas.
2. **La nube de puntos.** Cómo es el archivo, cuándo voló el avión y en qué sistema de altitudes están los puntos.
3. **Los vuelos nacionales del PNOA.** Qué ha volado ya España y cómo descargarlo y verlo.
4. **PDAL.** El principal programa abierto para trabajar con nubes de puntos.
5. **El terreno y la altura del bosque.** Cómo separar el suelo de los árboles y obtener la altura del dosel.
6. **La comparación de vuelos.** Cómo comparar vuelos de distintos años, aparatos y estaciones, y distinguir el crecimiento real de un error del vuelo.
7. **La herramienta terminada.** El paquete de Python, las pruebas, la publicación con DOI y el complemento de QGIS.

## Qué vimos en otros libros de la serie

Cada libro de la serie enseña una cosa. Lo que ya está explicado en otros libros no lo repetimos aquí, simplemente lo usamos:

- Python y PyQGIS: [libro 1, *PyQGIS from Scratch*](https://elviragd-lab.github.io/field-to-code-pyqgis/);
- imágenes de satélite, luz y relieve: [libro 2, *Remote Sensing from Scratch*](https://elviragd-lab.github.io/field-to-code-remote-sensing/);
- estadística (mediana, error, intervalos): [libro 3, *Spatial Statistics from Scratch*](https://elviragd-lab.github.io/field-to-code-statistics/).

Si alguno de estos temas no nos resulta familiar, allí está explicado desde cero.

## Qué programas hacen falta

Todos los programas son gratuitos y abiertos. Sirve un ordenador con Windows, macOS o Linux.

- **QGIS**, para ver con nuestros propios ojos las nubes de puntos y los mapas.
- **Conda**, un programa que instala Python y todas las bibliotecas necesarias en un entorno aparte. Así todos tenemos las mismas versiones y el resultado se puede repetir. La lista de bibliotecas está en el archivo `environment.yml` del repositorio `canopy-growth`.
- **Python 3.12** con estas bibliotecas: PDAL para las nubes de puntos, NumPy para los cálculos, GDAL y rasterio para los ráster, geopandas para las capas vectoriales, matplotlib para los gráficos y pytest para comprobar el código.
- **Git y GitHub**, para guardar la historia del proyecto y publicarlo.

Cómo instalar todo esto lo veremos en una lección aparte.

## Cómo es una lección

**Una lección es un paso del proyecto.** Si un paso es difícil, el capítulo tiene más lecciones.

En cada lección hay:

- **Qué queremos saber:** primero la pregunta, lo que queremos averiguar y lo que creemos que va a salir.
- **Los pasos del trabajo:** el paso, el código y la explicación de cada línea.
- **Qué ha salido:** la comprobación. ¿Coincide con lo que esperábamos? Si no coincide, vemos por qué.
- **✎ Tus datos:** notas donde podemos poner nuestra propia zona o nuestros propios datos y, al final de la lección, la lista de los datos que hacen falta.
- **📸:** el lugar donde la lección incluye una captura de pantalla.
- **Resumen:** los conceptos nuevos, qué significan y un ejemplo.
- **Preguntas para un ingeniero:** la respuesta va justo debajo de la pregunta, pero primero pensamos por nuestra cuenta.
- **Ejercicios:** tres ejercicios de memoria. Las soluciones están en la página [«Soluciones de los ejercicios»](./solutions.md), al final del libro.

## Sobre el código

**El código, sus comentarios y los rótulos de las figuras están en inglés.** Así el código se puede usar directamente y las figuras son iguales en las dos versiones del libro.

**Todo el código del proyecto está en el repositorio `canopy-growth`.** Al final de cada capítulo hay un enlace al commit, es decir, a la versión guardada del código con la que trabajó ese capítulo.

**Es mejor teclear el código a mano que copiarlo.** Así se recuerda antes. Si el código no funciona, lo comparamos con el libro carácter a carácter: casi siempre la culpa es de un paréntesis o de unas comillas.

## Sobre los errores

**No escondemos los errores.** Si el resultado no coincide con lo que esperábamos, eso también es un resultado del proyecto. Analizamos por qué ha pasado y qué hacer a continuación.

**Comprobamos cada número.** Cada paso termina con una comprobación que puede fallar. El umbral de la comprobación lo fijamos antes del cálculo, no después, para no ajustarlo al resultado.

## Si algo no funciona

Los programas se actualizan y los datos en internet a veces cambian de sitio. Si el código del libro deja de funcionar o hay un error en el texto, lo contamos en GitHub, en la sección Issues:

- sobre el libro, en el repositorio [`field-to-code-lidar`](https://github.com/ElviraGD-lab/field-to-code-lidar/issues);
- sobre la propia herramienta, en el repositorio [`canopy-growth`](https://github.com/ElviraGD-lab/canopy-growth/issues). Aquí ayuda mucho indicar los nombres de las hojas del PNOA, el comando y el texto completo del error.

## Licencias

- **El código del libro y `canopy-growth`:** Apache-2.0. Se puede usar y modificar en el propio trabajo.
- **El texto y las figuras del libro:** CC BY 4.0. Se pueden usar citando a la autora.
- **El complemento de QGIS:** GPL v2 o posterior, porque así lo exige QGIS.
- **Los datos LiDAR:** «PNOA cedido por © Instituto Geográfico Nacional», CC BY 4.0. Los datos no los subimos al repositorio: el código descarga las hojas por sí mismo.
