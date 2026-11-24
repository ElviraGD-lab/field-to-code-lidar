---
title: "Lección 1.2. Cómo demostrar que podemos fiarnos de un número"
---

## Dónde lo dejamos

En la lección anterior escribimos el pliego de condiciones de `canopy-growth`. Decidimos que la herramienta da un mapa de la velocidad de crecimiento del bosque en cuadrículas de 20–30 m, y que cada número lleva su intervalo. Decidimos también mostrar aparte dónde podemos fiarnos de las cifras y dónde no.

Pero enseguida surge una pregunta. Supongamos que la herramienta nos dice: «aquí el bosque crece 0,3 m al año». ¿Por qué deberíamos creerlo? ¿Y por qué debería creerlo quien vaya a planificar la poda y la tala con nuestro mapa?

## Qué queremos saber

**En esta lección vemos cómo comprobar un número que da nuestra herramienta.**

Primero pensemos cómo se hace en topografía. Nunca entregamos un levantamiento solo porque el aparato haya marcado algo. Lo comprobamos:
- cerramos la poligonal y miramos el error de cierre;
- comprobamos el aparato en una base de calibración;
- medimos puntos de control que ya conocemos;
- a veces otro topógrafo recalcula nuestra libreta de campo.

Así que nuestra hipótesis es esta: con el crecimiento del bosque hay que hacer lo mismo. Una sola comprobación no basta. Hacen falta varias distintas, y cada una caza su propio error.

Y dejamos escrita desde ya la regla principal. **Una comprobación tiene que poder fallar.** Si una comprobación pasa siempre, le demos lo que le demos, no comprueba nada. Por eso, para cada comprobación nos preguntaremos: ¿y cómo puede no pasar?

Para la parte 1 del proyecto tenemos ocho comprobaciones. Vamos con ellas una a una.

## Comprobación 1. La respuesta conocida

**Primero comprobamos el propio método con un problema cuya respuesta conocemos de antemano.**

Es como la base de calibración para un distanciómetro. Conocemos la distancia exacta entre dos señales y miramos si el aparato marca lo mismo.

Con el bosque lo hacemos así:
1. Tomamos el vuelo más reciente y más denso, el de 2025.
2. «Subimos» nosotros mismos los árboles una cantidad conocida. Por ejemplo, 1 m. Ahora sabemos exactamente qué crecimiento tiene que salir.
3. Quitamos parte de los puntos para que el vuelo quede tan poco denso como uno antiguo.
4. Lanzamos nuestro cálculo y miramos si encuentra ese mismo metro con el error que hemos declarado.

**Cómo puede fallar:** el cálculo no encuentra 1 m, sino, pongamos, 0,6 m. Eso quiere decir que el error está en el propio método. No seguimos adelante hasta encontrarlo.

## Comprobación 2. El ruido: cuánto «crecimiento» dan los errores por sí mismos

**Necesitamos saber qué diferencia muestran dos vuelos aunque el bosque casi no haya crecido.**

Sobre el puerto hay dos vuelos con solo un año de diferencia, más o menos: el asturiano de 2024 y el leonés de 2025. Donde se solapan, el bosque apenas ha cambiado en un año. Así que casi toda la diferencia entre ellos son errores del vuelo, no crecimiento.

Por ejemplo, en terreno abierto y llano ya comparamos los modelos del terreno de estos dos vuelos. La diferencia típica allí ronda los 9 cm[^1]. En el bosque será mayor.

Ese número es nuestro ruido. Un crecimiento menor que el ruido no podemos distinguirlo, siendo honestos, de un error. Por eso ese crecimiento no lo publicamos como número, sino como límite superior: «menos de tantos metros al año».

**Cómo puede fallar:** en sentido estricto, esto no es una comprobación, sino una medida. Pero de ella depende todo lo demás. Si el ruido resulta grande, el crecimiento lento sencillamente no lo veremos, y lo diremos con honestidad.

## Comprobación 3. Las superficies estables

**Hay cosas que no crecen: carreteras, tejados, roca. En ellas, la diferencia entre vuelos tiene que ser casi cero.**

Es como un clavo de nivelación. Si entre dos nivelaciones la cota del clavo se ha «movido», la culpa no es del clavo, sino de nuestro itinerario.

Comparamos los vuelos en esos lugares. Si allí la diferencia no es cero, es que uno de los vuelos está desplazado entero en altura. Entonces hay que quitar ese desplazamiento antes de calcular el crecimiento del bosque.

**Cómo puede fallar:** en las carreteras y los tejados la diferencia sigue siendo apreciable incluso después de la corrección. Entonces marcamos ese vuelo y no nos fiamos de él a ciegas.

## Comprobación 4. Las dos vertientes del puerto

**El puerto lo volaron dos programas distintos: Asturias y León. Son dos series de vuelos independientes.**

Donde se solapan, tienen que dar el mismo crecimiento, teniendo en cuenta los intervalos. Es como dos poligonales independientes que llegan al mismo vértice: si cierran, nos quedamos más tranquilos.

**Cómo puede fallar:** las series dan crecimientos distintos y la diferencia supera los intervalos. Entonces buscamos la causa en la ficha de pasadas. Por ejemplo, puede que una pasada se volara en otoño, cuando las hojas ya estaban cayendo. Ese análisis también lo publicamos.

## Comprobación 5. Los datos de campo

**El inventario forestal mide los árboles sobre el terreno. Sus cifras sirven para comprobar el orden de magnitud.**

Por ejemplo, según el inventario, un haya media en Asturias crece unos 0,1 m al año[^2]. Si nuestra herramienta dice que un haya de por aquí crece 1 m al año, es motivo para ponernos en guardia.

Aquí hay que hacer una precisión. El inventario mide el árbol medio, y nosotros medimos la parte superior del dosel. No es lo mismo. Por eso no comparamos el número exacto, sino el orden de magnitud: si estamos en el mismo mundo.

**Cómo puede fallar:** nuestro crecimiento se aleja del de campo más de lo que permiten los intervalos. Entonces publicamos las dos cifras y explicamos la diferencia.

## Comprobación 6. La aplicación a ciegas

**Lanzamos el mismo código, con los mismos ajustes, en otras zonas donde no hemos ajustado nada.**

Son Galicia y una zona de clima mediterráneo. Si el método solo funciona en Pajares, es que lo hemos ajustado sin darnos cuenta a un único lugar.

**Cómo puede fallar:** en la zona nueva, el resultado no pasa las comprobaciones. Si la culpa es de un error del programa, lo corregimos y volvemos a calcular también Pajares. Si el método simplemente no funciona en esas condiciones, escribimos dónde está el límite de su aplicación.

## Comprobación 7. La repetibilidad

**Cualquiera tiene que poder lanzar el cálculo desde cero y obtener exactamente el mismo resultado.**

Es como la libreta que recalcula otro topógrafo y le salen las mismas coordenadas.

Para comparar los resultados usamos la suma de comprobación de un archivo. Es una cadena corta que el programa calcula a partir del contenido del archivo, como una huella dactilar. Si en el archivo cambia aunque sea una cifra, cambia también la huella.

**Cómo puede fallar:** al lanzarlo de nuevo, la huella es otra. Eso quiere decir que hay algo que no hemos dejado anotado: otra versión de un programa, una selección aleatoria sin semilla fija, un paso a mano. Es un error de ingeniería, y hay que corregirlo.

## Comprobación 8. El umbral antes del cálculo

**La tolerancia la dejamos escrita antes de medir, no después.**

En topografía, el error de cierre admisible se toma de antemano de la norma. Nadie calcula la poligonal y luego se inventa una tolerancia que le encaje.

Nosotros, igual. Los umbrales de todas las comprobaciones los escribimos en el archivo `config/acceptance.yaml` del repositorio `canopy-growth`. Y hacemos el commit con el umbral antes de que aparezca el resultado. El propio historial de git muestra qué fue antes: el umbral o el número.

Veamos ese archivo ahora mismo. Abrimos en GitHub el repositorio `canopy-growth` y, dentro, el archivo `config/acceptance.yaml`. No hay casi nada:

```yaml
thresholds: {}
```

`thresholds` son los «umbrales». Las llaves `{}` son una lista vacía. Todavía no hay umbrales, porque aún no hemos dado ningún paso del cálculo. El primer umbral de verdad aparecerá antes del paso de descarga de datos.

```{figure} ../../images/ch01-02-acceptance-empty.png
:alt: El archivo config/acceptance.yaml en GitHub, con la regla en los comentarios y thresholds vacío
:width: 100%

El archivo `config/acceptance.yaml` del repositorio `canopy-growth` en GitHub. Las líneas con `#` son comentarios con la regla: el umbral se escribe antes del paso que comprueba y no se cambia después de ver el resultado. Captura de pantalla, 24 de noviembre de 2026.
```

**Cómo puede fallar:** el commit con el umbral aparece después del resultado, o el umbral se cambia después de ver el número. En ese caso, la comprobación no se da por superada.

## Qué ha salido

Juntemos las ocho comprobaciones en una tabla.

| Comprobación | Qué comparamos | Cuándo falla | Qué hacemos |
|---|---|---|---|
| 1. Respuesta conocida | el crecimiento encontrado y el que hemos añadido nosotros | el método no encuentra el crecimiento conocido | buscamos el error del método y no seguimos |
| 2. Ruido | dos vuelos con un año de diferencia, más o menos | el ruido es grande | publicamos el crecimiento lento como límite superior |
| 3. Superficies estables | carreteras, tejados y roca en los dos vuelos | allí queda diferencia | corregimos el desplazamiento y marcamos el vuelo |
| 4. Dos vertientes del puerto | el crecimiento según la serie asturiana y la leonesa | las series no coinciden | analizamos las pasadas y publicamos el análisis |
| 5. Datos de campo | nuestro crecimiento y el inventario forestal | la diferencia supera los intervalos | publicamos las dos cifras |
| 6. Aplicación a ciegas | Galicia y la zona mediterránea | no pasan las comprobaciones | corregimos el error o escribimos el límite del método |
| 7. Repetibilidad | las huellas de los archivos al lanzar desde cero | las huellas son distintas | buscamos el paso que no anotamos |
| 8. Umbral antes del cálculo | la fecha del commit con el umbral y la del resultado | el umbral apareció después | la comprobación no cuenta |

**Cada comprobación caza su propio error, y cada una puede fallar. Solo así podemos entregar el número a la gente.**

## ✎ Tus datos: qué hace falta para las comprobaciones en tu zona

1. **Dos vuelos con poca diferencia de tiempo.** Hacen falta para estimar el ruido. Si no los hay, estimamos el ruido con las superficies estables y con el solape de las pasadas.
2. **Superficies estables.** Carreteras, tejados o roca dentro de la zona.
3. **Datos de campo sobre el crecimiento.** El inventario forestal o mediciones propias de árboles.
4. **Una segunda zona para la aplicación a ciegas.** Parecida o, al contrario, muy distinta, donde no hayamos ajustado nada.

## Resumen

### Conceptos nuevos

- **Comprobación que puede fallar:** una comprobación que no pasa cuando los datos o el método son malos. Ejemplo: el método no encuentra el crecimiento que hemos añadido nosotros.
- **Respuesta conocida:** un problema cuya respuesta conocemos de antemano. Ejemplo: «subimos» los árboles 1 m y miramos si el método encuentra ese metro.
- **Ruido:** la diferencia entre vuelos que dan los errores por sí mismos, sin crecimiento. Ejemplo: unos 9 cm en terreno abierto entre los vuelos de 2024 y 2025.
- **Superficie estable:** algo que no crece ni cambia. Ejemplo: una carretera, un tejado, la roca.
- **Suma de comprobación:** una cadena corta que sirve de huella de un archivo. Ejemplo: una sola cifra cambiada en el archivo cambia la huella.
- **Umbral antes del cálculo:** una tolerancia escrita antes del resultado. Ejemplo: el commit con el umbral en `acceptance.yaml` está en el historial antes que el commit con el resultado.

## Preguntas para un ingeniero

**¿Por qué no sirve de nada una comprobación que pasa siempre?**

:::{dropdown} Respuesta
Porque no distingue nada. Un mal método y unos malos datos también la pasan, así que no nos dice si podemos fiarnos del número.
:::

**¿Para qué queremos dos vuelos con solo un año de diferencia, si en un año el crecimiento apenas se ve?**

:::{dropdown} Respuesta
Justo por eso. Como el bosque casi no ha crecido, casi toda la diferencia entre ellos son errores del vuelo. Así conocemos el ruido y sabemos qué crecimiento podemos llegar a ver.
:::

**¿Por qué comparamos nuestro crecimiento con el inventario forestal solo en orden de magnitud?**

:::{dropdown} Respuesta
El inventario mide el árbol medio desde el suelo, y nosotros, la parte superior del dosel desde arriba. Son cosas distintas, así que no podemos esperar que coincidan exactamente, pero el orden de magnitud tiene que ser el mismo.
:::

(es-ex-1-2)=
## Ejercicios

1. **Recordamos.** ¿Cómo se llama la comprobación en la que añadimos nosotros mismos un crecimiento conocido a los árboles y miramos si el método lo encuentra?

2. **Corregimos el error.** «El umbral de la comprobación lo escribimos después del cálculo, cuando ya vemos qué resultado ha salido».

3. **De la lección 1.1.** ¿Por qué calculamos el crecimiento en cuadrículas de 20–30 m y no por árboles sueltos?

[Soluciones de estos ejercicios](#es-sol-1-2)

---

## Notas y fuentes

Fuentes consultadas el 24 de noviembre de 2026.

[^1]: [Cálculo del proyecto Pajares: modelos del terreno de 1 m a partir de los vuelos PNOA de 2024 (Asturias) y 2025 (Castilla y León), terreno abierto y llano; mediana de la diferencia −0,025 m, diferencia típica (NMAD) 0,089 m. Datos: «PNOA cedido por © Instituto Geográfico Nacional»](https://centrodedescargas.cnig.es).
[^2]: [IFN4, Asturias, tabla 926: crecimiento medio anual en altura por especies; haya, 0,11 m al año](https://www.asturias.es/documents/4911212/4919385/20240117-IFN4-indice-tablas-mapas.pdf/ee7edde7-e40f-7331-a482-58a1b76ba5ee).
