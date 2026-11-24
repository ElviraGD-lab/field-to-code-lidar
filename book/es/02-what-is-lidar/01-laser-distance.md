---
title: "Lección 2.1. Cómo mide distancias un láser"
---

## Dónde lo dejamos

En el primer capítulo decidimos qué construimos y repasamos las ocho comprobaciones sin las que no podemos fiarnos de nuestro número. Ahora toca entender el propio aparato. Empecemos por la pregunta más sencilla: ¿cómo mide distancias el LiDAR?

## Qué queremos saber

**En esta lección calculamos a mano cómo un pulso láser se convierte en una distancia, y qué precisión hace falta para ello.**

Primero recordemos lo que ya sabemos. Una estación total sin prisma lanza un haz contra una pared o un árbol, recoge el reflejo y muestra la distancia. Muchas de estas estaciones la miden por el tiempo: cuánto ha tardado el haz en ir y volver.

Nuestra hipótesis: el LiDAR del avión hace lo mismo, solo que muy deprisa y muchísimas veces. Si la hipótesis es correcta, podremos calcular nosotros mismos la distancia a partir del tiempo de vuelo del pulso. Y también estimar con qué precisión tiene que medir el tiempo el aparato.

## Paso 1. A qué velocidad viaja la luz

La luz viaja en el vacío a 299 792 458 m por segundo. Es un número exacto: hoy se usa para definir el propio metro[^1].

Para nosotros es más cómodo recordarlo así: **en un nanosegundo, la luz recorre unos 30 cm.** Un nanosegundo es una milmillonésima de segundo.

A partir de aquí lo calcularemos todo con este número.

## Paso 2. Ida y vuelta

El LiDAR lanza hacia abajo un pulso de luz muy corto. El pulso llega al suelo o a una copa, se refleja y vuelve al aparato. El aparato mide el tiempo entre el disparo y el regreso.

Un detalle importante: la luz ha recorrido el camino **dos veces**, de ida y de vuelta. Por eso la distancia es la mitad del recorrido:

**distancia = velocidad de la luz × tiempo ÷ 2**

Hagamos la cuenta con un ejemplo. Supongamos que el avión vuela a unos 1000 m sobre el suelo. El pulso ha vuelto al cabo de 6,67 microsegundos. Un microsegundo es una millonésima de segundo, así que 6,67 µs = 0,00000667 s.

299 792 458 × 0,00000667 ÷ 2 ≈ 999,8 m.

Sale casi exactamente 1000 m. La hipótesis funciona.

Lo mismo podemos calcularlo en Python. Si todavía no tenemos Python instalado, estas líneas también se pueden hacer con calculadora: la instalación la veremos en una lección aparte.

Primero escribimos la velocidad de la luz. Python se salta los guiones bajos dentro del número; solo sirven para que se lea mejor:

```python
# Speed of light in vacuum, m/s (exact, SI)
c = 299_792_458
```

Ahora, el tiempo que ha medido el aparato. La notación `6.67e-6` significa 6,67 × 10⁻⁶ segundos, es decir, 6,67 microsegundos:

```python
# Time between sending the pulse and receiving the echo, s
t = 6.67e-6
```

Y la fórmula. Dividimos entre 2 porque el pulso ha ido y vuelto. `round(..., 1)` redondea a un decimal:

```python
# The pulse travels to the ground and back, so we halve the path
distance = c * t / 2
print(round(distance, 1))
```

Python mostrará `999.8`, es decir, 999,8 m. Coincide con nuestra cuenta a mano.

✎ *Tus datos: la altura de vuelo sobre tu zona y el tiempo que tardará en volver el pulso (punto 1 de la lista al final de la lección).*

## Paso 3. Con qué precisión hay que medir el tiempo

Ahora hagamos la cuenta al revés. Queremos la altura de los árboles con una precisión de unos 10 cm. ¿Qué precisión de tiempo exige eso?

Damos la vuelta a la fórmula: tiempo = 2 × distancia ÷ velocidad de la luz.

2 × 0,10 ÷ 299 792 458 ≈ 0,00000000067 s, es decir, **unos 0,67 nanosegundos**.

En Python:

```python
# Wanted distance accuracy, m
dd = 0.10
# Time accuracy needed for it, s
dt = 2 * dd / c
# The same in nanoseconds
print(dt * 1e9)
```

Python mostrará aproximadamente `0.667`.

Así que el aparato tiene que captar el momento en que vuelve el pulso con una precisión de menos de un nanosegundo. En ese tiempo, la luz recorre menos de 30 cm. Precisamente por eso el LiDAR es un aparato preciso pero muy complejo: lleva dentro un reloj rapidísimo.

## Paso 4. En el aire, la luz va un poco más despacio

**En el aire, la luz viaja algo más despacio que en el vacío.** A los topógrafos esto nos suena: antes de medir, metemos en la estación total la temperatura y la presión, y ella misma aplica la corrección atmosférica.

¿Cuánto más despacio? Lo hemos calculado con la fórmula que publica el NIST, el instituto de normalización estadounidense[^2]. Para el láser del LiDAR cerca del suelo, a 15 °C y presión normal, la luz va un 0,027 % más despacio, aproximadamente[^3].

Parece poca cosa. Pero veamos qué supone en 1000 m:

1000 m × 0,00027 ≈ 0,27 m.

Si no aplicamos la corrección, el error será de unos 27 cm por cada kilómetro. Eso es más de lo que crece un haya en dos años. Por eso, al procesar el vuelo, esta corrección se tiene siempre en cuenta. Nosotros no vamos a calcularla, pero no nos olvidamos de que existe.

## Paso 5. Cuántos pulsos por segundo

El LiDAR no mide un solo punto. Dispara pulsos una y otra vez, cientos de miles de veces por segundo. Pensemos si esto tiene un límite.

La lógica es sencilla: conviene lanzar el siguiente pulso cuando el anterior ya ha vuelto. Si no, el aparato puede confundir qué pulso le ha llegado. Desde 1000 m, el pulso tarda 6,67 µs en ir y volver. Así que en un segundo caben, más o menos:

1 ÷ 0,00000667 ≈ 150 000 pulsos.

Los aparatos modernos pueden trabajar aún más deprisa, con varios pulsos en el aire a la vez. Pero la idea se mantiene: cuanto más alto vuela el avión, más tarda el pulso y más difícil es medir a menudo.

✎ *Tus datos: cuántos pulsos por segundo son posibles desde la altura de vuelo sobre tu zona (punto 2).*

## Qué ha salido

Juntemos los números. Para un vuelo a unos 1000 m de altura:

| Qué | Cuánto |
|---|---|
| tiempo de vuelo del pulso, ida y vuelta | 6,67 µs |
| precisión de tiempo para 10 cm | unos 0,67 ns |
| error sin corrección atmosférica | unos 27 cm por km |
| pulsos por segundo, esperando a cada uno | unos 150 000 |

La hipótesis se ha confirmado. El LiDAR mide la distancia igual que una estación total sin prisma: por el tiempo de vuelo de la luz, de ida y vuelta.

**Pero una distancia todavía no es un punto.** Para obtener coordenadas hay que saber dónde estaba el avión en ese momento y hacia dónde apuntaba el haz. Ahí los errores pueden ser mayores que los del propio distanciómetro.

## ✎ Tus datos: qué hace falta para tu cálculo

1. **La altura de vuelo sobre la zona.** Se puede encontrar en la descripción del vuelo. En el PNOA viene en la documentación de los vuelos.
2. **El tiempo de vuelo del pulso y la frecuencia.** Los calculamos con las fórmulas de esta lección: tiempo = 2 × altura ÷ velocidad de la luz; pulsos por segundo ≈ 1 ÷ tiempo.

## Resumen

### Conceptos nuevos

- **Tiempo de vuelo:** el tiempo entre el disparo del pulso y el regreso del reflejo. Ejemplo: desde 1000 m de altura, unos 6,67 µs.
- **Distancia por tiempo de vuelo:** velocidad de la luz × tiempo ÷ 2. Dividimos entre 2 porque la luz va y vuelve. Ejemplo: 6,67 µs equivalen a unos 1000 m.
- **Nanosegundo:** una milmillonésima de segundo. En ese tiempo, la luz recorre unos 30 cm.
- **Corrección atmosférica:** la corrección que se aplica porque en el aire la luz va más despacio que en el vacío. Ejemplo: sin ella, el error es de unos 27 cm por km.

## Preguntas para un ingeniero

**¿Por qué dividimos entre 2 en la fórmula de la distancia?**

:::{dropdown} Respuesta
El aparato mide el tiempo que ha tardado la luz en llegar al suelo y volver. Es un recorrido de ida y vuelta, y nosotros necesitamos la distancia en un solo sentido.
:::

**¿Por qué necesita el LiDAR un reloj tan preciso?**

:::{dropdown} Respuesta
En un nanosegundo, la luz recorre unos 30 cm. Para medir alturas con una precisión de unos 10 cm, hay que captar el tiempo con una precisión de menos de un nanosegundo.
:::

**¿Por qué es importante para nuestro proyecto la pequeña diferencia de la velocidad de la luz en el aire?**

:::{dropdown} Respuesta
En 1 km da un error de unos 27 cm. Es comparable a lo que crece el bosque en varios años, así que, sin la corrección, podríamos tomar el error por crecimiento.
:::

(es-ex-2-1)=
## Ejercicios

1. **Recordamos.** ¿Cómo se calcula la distancia si conocemos el tiempo de vuelo del pulso?

2. **Corregimos el error.** En el código `distance = c * t` hay un error. ¿Cuál?

3. **De la lección 1.2.** ¿En qué lugares la diferencia entre dos vuelos tiene que ser casi cero, y para qué los comparamos?

[Soluciones de estos ejercicios](#es-sol-2-1)

---

## Notas y fuentes

Fuentes consultadas el 24 de noviembre de 2026.

[^1]: [BIPM, SI base unit: metre. Velocidad de la luz en el vacío: 299 792 458 m/s, valor exacto](https://bipm.org/en/si-base-units/metre).
[^2]: [NIST, Engineering Metrology Toolbox, Index of Refraction of Air (Stone, Zimmerman)](https://emtoolbox.nist.gov/wavelength/Documentation.asp).
[^3]: Cálculo del proyecto con la fórmula de Edlén modificada (NIST, nota 2): longitud de onda 1064 nm, 15 °C, 101 325 Pa, humedad del 50 %; índice de refracción del aire 1,000274. A 8 °C y 899 hPa (unos 1000 m sobre el nivel del mar), 1,000249.
