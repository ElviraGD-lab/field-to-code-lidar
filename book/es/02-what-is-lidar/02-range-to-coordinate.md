---
title: "Lección 2.2. Cómo una distancia se convierte en coordenada"
---

## Dónde lo dejamos

En la lección 2.1 aprendimos a obtener la distancia a partir del tiempo de vuelo del pulso. Pero una distancia todavía no es un punto. **Para obtener X, Y y Z, necesitamos saber desde dónde salió el haz y en qué dirección.**

## Qué queremos entender

**En esta lección calculamos a mano cómo se pasa de una distancia a la coordenada de un punto, y con qué precisión sale esa coordenada.**

Primero recordemos el trabajo de campo. Con la estación total estacionamos en un vértice de coordenadas conocidas. El aparato está orientado y conocemos la dirección. Medimos el ángulo horizontal, el ángulo cenital y la distancia, y calculamos las coordenadas del punto. Es el problema directo de la topografía.

Nuestra hipótesis: el LiDAR resuelve el mismo problema. Con una sola diferencia: la estación vuela, se balancea y gira. Si la hipótesis es correcta, podremos calcular nosotros mismos un punto de Pajares, estimar qué errores dan los aparatos y compararlo con lo que promete el PNOA.

## Paso 1. Qué hay que saber en el momento del disparo

En el campo necesitamos tres cosas: dónde está el aparato, cómo está orientado y hacia dónde apunta el anteojo. En el avión es lo mismo, solo que cada cosa la mide un aparato distinto.

1. **Dónde está el avión: GNSS.** Una antena en el techo del avión y estaciones de referencia en tierra. Tras el procesado, la posición se conoce con una precisión de unos 2 cm en planimetría y 5 cm en altimetría[^1].
2. **Cómo está inclinado el avión: la unidad de medición inercial (IMU).** Mide tres ángulos: el **alabeo** (de un ala a la otra), el **cabeceo** (morro arriba o abajo) y el **rumbo** (hacia dónde apunta el morro). El avión se balancea todo el tiempo, por eso el PNOA exige registrar estos ángulos al menos 200 veces por segundo[^4].
3. **Hacia dónde salió el haz: el ángulo del espejo del escáner.** El espejo barre el haz en perpendicular al vuelo, de izquierda a derecha y vuelta. Cada pulso sale con su propio ángulo.

Hay además dos correcciones que nos suenan del campo:

- **El brazo de palanca de la antena.** La antena está en el techo y el escáner, en el suelo del avión. La distancia entre ellos se mide una vez en tierra y se tiene en cuenta, igual que la altura del instrumento sobre el vértice.
- **La calibración de los ejes (boresight).** Los ejes de la IMU y los del escáner no coinciden del todo. Ese pequeño ángulo se determina con vuelos de calibración sobre lugares conocidos.

**Todo esto junto se llama georreferenciación directa: la coordenada del punto sale directamente de los aparatos del avión, sin puntos de apoyo en tierra para cada medición.**

## Paso 2. El caso más sencillo: un triángulo rectángulo

Empecemos por lo sencillo. El avión vuela justo hacia el norte, con las alas niveladas, sin alabeo ni cabeceo. El haz sale hacia la derecha, hacia el este, con un ángulo de 20° respecto a la vertical.

Dibujemos lo que tenemos. Es un triángulo rectángulo:

```{figure} ../../images/ch02-02-beam-triangle.svg
:alt: Triángulo rectángulo: el haz de 2128,4 m sale del avión con 20° respecto a la vertical; baja 2000 m y se desplaza 728 m hacia el este
:width: 80%

El haz y sus dos catetos. Ejemplo didáctico sobre el centro del marco de Pajares; las etiquetas, en inglés.
```

- La **hipotenusa** es el propio haz. Su longitud la ha medido el aparato: 2128,4 m.
- El **ángulo de 20°** es el que forma el haz con la vertical. Nos lo da el espejo del escáner.
- Necesitamos los **dos catetos**: cuánto se ha desplazado el haz hacia un lado y cuánto ha bajado.

Si conocemos la hipotenusa y el ángulo, cada cateto sale directamente; para eso no hace falta el teorema de Pitágoras:

- **cateto opuesto al ángulo = hipotenusa × sen del ángulo**: el desplazamiento lateral;
- **cateto contiguo al ángulo = hipotenusa × cos del ángulo**: cuánto ha bajado el haz.

Son las mismas fórmulas que con la estación total: distancia reducida = S × sen z, desnivel = S × cos z. Solo que el ángulo no lo contamos hacia arriba desde el cenit, sino hacia abajo desde la vertical.

Tomemos un ejemplo sobre el centro del marco de Pajares. El avión está en el punto X = 274 000, Y = 4 763 000, a 3300 m de altitud. El terreno bajo él está a unos 1300 m, así que volamos a unos 2000 m sobre el suelo. El aparato ha medido una distancia de 2128,4 m. Es un ejemplo didáctico, no un vuelo real. Las altitudes de aquí son sobre el nivel del mar; en qué se diferencian de las altitudes GNSS lo veremos en la lección 3.4.

Recordemos cómo son los ejes en UTM:

- **X crece hacia el este.**
- **Y crece hacia el norte.**
- **Z crece hacia arriba.**

Calculamos los catetos:

- hacia el lado: 2128,4 × sen 20° = 2128,4 × 0,342 = **728,0 m**;
- hacia abajo: 2128,4 × cos 20° = 2128,4 × 0,940 = **2000,0 m**.

Ahora sumamos los catetos a la posición del avión:

- **X** = 274 000 + 728,0 = **274 728,0**. El haz ha ido hacia el este, es decir, a lo largo del eje X, así que X ha aumentado.
- **Y** = **4 763 000**. Y no cambia: el haz no se ha desplazado ni hacia el norte ni hacia el sur.
- **Z** = 3300 − 2000,0 = **1300,0**. El haz ha bajado 2000 m.

El punto ha caído en el suelo a 1300 m de altitud, como esperábamos.

**El teorema de Pitágoras nos sirve, en cambio, como comprobación.** Si los catetos están bien calculados, juntos tienen que darnos nuestra distancia:

728² + 2000² = 529 984 + 4 000 000 = 4 529 984, y √4 529 984 ≈ 2128,4 m.

Justo lo que midió el aparato. El cálculo cuadra.

Lo mismo en Python. Primero, la posición del avión. Python se salta los guiones bajos dentro del número: `274_000` es el mismo número 274 000, solo que así se cuentan mejor los ceros. También se puede escribir `274000`.

```python
import math

# Aircraft position (example), m: easting X, northing Y, height Z
x0, y0, z0 = 274_000, 4_763_000, 3_300
```

Después, lo que ha medido el escáner: la distancia y el ángulo del haz respecto a la vertical.

```python
# Measured range, m, and scan angle from the vertical, degrees
r = 2128.4
angle = 20
```

Las funciones `sin` y `cos` de Python esperan el ángulo en radianes, no en grados. Por eso primero convertimos el ángulo:

```python
# Python's sin and cos take radians, so we convert first
a = math.radians(angle)

# The beam goes east, so only X changes; Y stays the same
x = x0 + r * math.sin(a)
z = z0 - r * math.cos(a)
print(round(x, 1), y0, round(z, 1))
```

Python mostrará `274728.0 4763000 1300.0`. La respuesta coincide con el cálculo a mano.

Y la comprobación con Pitágoras: los dos catetos tienen que devolvernos la distancia.

```python
# Check: the two legs must give back the measured range (Pythagoras)
check = math.sqrt((x - x0) ** 2 + (z0 - z) ** 2)
print(round(check, 1))
```

Python mostrará `2128.4`.

**Quién hace este cálculo en el trabajo real.** Cada punto de la nube lo calcula la empresa adjudicataria con su propio programa: tiene la trayectoria del avión y los ángulos de la IMU, que nosotros no tenemos. En los archivos del PNOA ya vienen X, Y y Z calculadas. Lo hemos calculado a mano una vez para entender de dónde salen los errores y de qué tamaño son. Nos fiamos de la documentación del vuelo, pero la comprobamos, como en el campo: la estación total tiene su certificado de calibración, y aun así hacemos mediciones de control. Para el LiDAR, esa comprobación son las superficies estables (lección 1.2).

✎ *Tus datos: la altura de vuelo sobre tu zona y el ángulo de barrido del escáner (punto 1 de la lista al final de la lección).*

## Paso 3. Qué provoca un pequeño error de ángulo

Ahora, la pregunta principal de la lección. La IMU no mide los ángulos a la perfección. En una buena unidad, el error de alabeo y de cabeceo ronda los 0,005°, y el de rumbo, los 0,010°[^1]. Parece nada. Veamos en qué se convierte sobre el terreno.

En el campo conocemos la regla: **un error de 1″ a 1 km de distancia desplaza el punto unos 5 mm.** Es la regla del ángulo pequeño: desplazamiento = distancia × ángulo en radianes.

Hagamos la conversión: 0,005° son 18″. Nuestro haz mide unos 2,13 km. Desplazamiento ≈ 2,13 × 18 × 5 mm ≈ **19 cm**.

```python
# Small-angle rule: shift = distance × angle in radians
shift = r * math.radians(0.005)
print(round(shift, 3))
```

Python mostrará `0.186`, es decir, unos 19 cm.

Hacia dónde va ese desplazamiento depende de dónde esté el haz. **Bajo el avión**, cuando el haz apunta justo hacia abajo, el error de alabeo desplaza el punto hacia un lado y la altura apenas cambia. **En el borde de la pasada**, donde el haz va inclinado, parte del desplazamiento se va a la altura: con un ángulo de 20°, son unos 6 cm en altimetría y 17 cm en planimetría.

**Cuanto más lejos del avión y cuanto mayor es el ángulo del haz, más estropea la altura un pequeño error de ángulo.**

## Paso 4. El balance de errores de nuestro punto

Juntemos todos los errores de nuestro punto (haz de 2128 m, ángulo de 20°). La precisión en distancia de los escáneres de esta clase ronda los 20 mm[^2]. Los errores son independientes, así que los combinamos igual que en el campo al estimar la precisión: los elevamos al cuadrado, los sumamos y sacamos la raíz.

| Fuente | En altimetría | En planimetría |
|---|---|---|
| posición del avión (GNSS) | 5 cm | 2 cm |
| alabeo | 6 cm | 17 cm |
| cabeceo | casi 0 | 17 cm |
| rumbo | 0 en terreno llano | 13 cm |
| distancia | 2 cm | 1 cm |
| **total** | **unos 8 cm** | **unos 28 cm** |

En altimetría, el punto sale más preciso que en planimetría. Pero eso es en terreno llano.

**En una ladera, el error en planimetría se convierte en error en altimetría.** El punto cae 28 cm a un lado, y allí el terreno ya está más alto o más bajo. En una ladera de 30°, eso son unos 16 cm en altura, y junto con los demás errores, unos 18 cm. En una ladera de 5°, solo unos 2 cm.

## Qué ha salido

| Qué | Cuánto |
|---|---|
| punto de Pajares (ejemplo didáctico) | X 274 728,0; Y 4 763 000; Z 1300,0 |
| comprobación con Pitágoras | 2128,4 m: coincide con la distancia |
| desplazamiento por un error de ángulo de 0,005° a 2,13 km | unos 19 cm |
| error en altura en terreno llano | unos 8 cm |
| error en planimetría | unos 28 cm |
| error en altura en una ladera de 30° | unos 18 cm |

Para la primera cobertura del PNOA se declara una precisión en altimetría mejor que 20 cm y en planimetría de unos 30 cm[^3]. Nuestro balance aproximado da el mismo orden de magnitud[^6].

**La hipótesis se ha confirmado: el LiDAR resuelve el problema directo de la topografía, y el error mayor no viene del distanciómetro, sino de los ángulos.**

**De aquí sale una conclusión importante para nuestro proyecto.** Pajares es montaña, con laderas muy empinadas. En una ladera, el error en planimetría del vuelo parece un cambio de altura, y podemos confundirlo con crecimiento del bosque. Por eso buscamos el desplazamiento entre vuelos en lugares llanos y estables, como decidimos en la lección 1.2.

## Si algo falla

**El punto se ha ido a un kilómetro.** Si escribimos `math.sin(20)` sin convertir a radianes, Python calcula el seno de veinte radianes. El punto acabará en X = 275 943 en lugar de 274 728: 1,2 km más allá. Si vemos un resultado así, nos preguntamos enseguida: ¿el ángulo está en grados o en radianes?

**La comprobación con Pitágoras no cuadra.** Eso quiere decir que uno de los catetos está mal calculado: lo más habitual es haber cambiado el seno por el coseno. Recordemos: el seno da el cateto opuesto al ángulo (hacia el lado); el coseno, el contiguo (hacia abajo).

## ✎ Tus datos: qué hace falta para tu cálculo

1. **La altura de vuelo y el ángulo de barrido del escáner.** Vienen en la descripción del vuelo. En las especificaciones técnicas del PNOA, por ejemplo, figura un ángulo de barrido de hasta 50° y una distancia de hasta 3000 m[^5].
2. **La precisión de la IMU.** El modelo de la unidad suele venir en el informe del vuelo, y la precisión, en la ficha técnica del fabricante.
3. **La pendiente de tu zona.** La sacamos del modelo del terreno: ahí se ve dónde el error en planimetría se convertirá en error en altura.

## Resumen

### Conceptos nuevos

- **Georreferenciación directa:** la coordenada de un punto a partir de la posición del avión (GNSS), su inclinación (IMU), el ángulo del haz y la distancia. Ejemplo: el punto de Pajares X 274 728, Z 1300.
- **Alabeo, cabeceo, rumbo:** los tres ángulos del avión: de un ala a la otra, morro arriba y abajo, hacia dónde apunta el morro. Ejemplo: un error de alabeo de 0,005°.
- **Ángulo de escaneo:** el ángulo del haz respecto a la vertical. Ejemplo: con 20°, el haz se desplaza 728 m hacia un lado.
- **Catetos a partir del ángulo:** hacia el lado = distancia × sen del ángulo; hacia abajo = distancia × cos del ángulo; Pitágoras sirve de comprobación. Ejemplo: 728² + 2000² ≈ 2128,4².
- **Regla del ángulo pequeño:** desplazamiento = distancia × ángulo en radianes. Ejemplo: 1″ a 1 km son unos 5 mm.
- **Balance de errores:** todas las fuentes de error en una tabla, combinadas por sus cuadrados. Ejemplo: 8 cm en altimetría y 28 cm en planimetría.

## Preguntas para un ingeniero

**¿Por qué el mismo error de alabeo estropea menos la altura bajo el avión que en el borde de la pasada?**

:::{dropdown} Respuesta
Bajo el avión, el haz va justo hacia abajo, y el error de ángulo solo desplaza el punto hacia un lado. En el borde, el haz va inclinado y es más largo, y parte del desplazamiento se va a la altura.
:::

**¿Por qué en una ladera empinada la altura del punto sale peor que en terreno llano, si los aparatos son los mismos?**

:::{dropdown} Respuesta
El punto cae un poco desplazado del lugar correcto, y en una ladera ahí hay otra altura. 28 cm en planimetría en una ladera de 30° dan unos 16 cm en altura.
:::

**Si la empresa adjudicataria ya ha calculado todos los puntos, ¿para qué comprobamos nosotros el vuelo?**

:::{dropdown} Respuesta
Igual que en el campo comprobamos el aparato con mediciones de control. Los errores de ángulo y los desplazamientos del vuelo no se ven en un solo punto, pero sí en los lugares estables. Sin esa comprobación, podríamos confundir un error con crecimiento del bosque.
:::

(es-ex-2-2)=
## Ejercicios

1. **Recordamos.** Además de la distancia, ¿qué hay que saber en el momento del disparo para obtener la coordenada de un punto?

2. **Corregimos el error.** En la línea `x = x0 + r * math.sin(20)` hay un error. ¿Cuál?

3. **De la lección 1.1.** Para la distribuidora, ¿en qué se diferencia la altura de los árboles de su velocidad de crecimiento?

[Soluciones de estos ejercicios](#es-sol-2-2)

---

## Notas y fuentes

Fuentes consultadas el 25 de noviembre de 2026.

[^1]: [Trimble Applanix, POS AV: resumen de la precisión tras el procesado (POS AV 510: X, Y, 0,02 m; Z, 0,05 m; alabeo y cabeceo, 0,005°; rumbo, 0,010°)](https://applanix.trimble.com/en/products/hardware/applanix-pos-av).
[^2]: [RIEGL, LMS-Q780, ficha técnica: precisión en distancia de 20 mm, ángulo de barrido de 60°](https://www.rieglusa.com/wp-content/uploads/lms-q780-datasheet.pdf).
[^3]: [IGN, «MapaLidar», JIIDE 2019: primera cobertura, 0,5 puntos/m²; precisión en altimetría mejor que 20 cm (RMSE Z) y en planimetría de 30 cm](https://idee.es/resources/presentaciones/JIIDE19/dia_24/AUDITORIO_PRINCIPAL/1.Sesion_1_Nuevos_Desarrollos/2_MapaLidar_JIIDE2019.pdf).
[^4]: [IGN/CNIG, especificaciones técnicas del vuelo LiDAR, Castilla y León SW (2015): IMU con registro de al menos 200 Hz y deriva < 0,1° por hora](https://pnoa.ign.es/resources/archivos/EspTec/2015_Especificaciones_VL_CyLSW.pdf).
[^5]: [IGN/CNIG, especificaciones técnicas del vuelo LiDAR (2016): frecuencia de al menos 45 kHz con un ángulo de barrido de 50° y distancia de hasta 3000 m](https://pnoa.ign.es/resources/archivos/EspTec/2016_%20Especificaciones_VL_Madrid.pdf).
[^6]: Cálculo de la lección: el punto, la comprobación con Pitágoras, la regla del ángulo pequeño y el balance de errores para un haz de 2128,4 m con un ángulo de 20°, en laderas de 5° y 30°.
