---
title: "Lección 1.1. Quién necesita la velocidad de crecimiento del bosque y para qué"
---

## Por dónde empezamos

Antes de descargar nada o de escribir código, aclaremos qué hacemos exactamente y para quién.

Esto ya lo conocemos del trabajo de campo. El cliente no dice «levántame algo». Dice, por ejemplo, esto:

- necesito un plano de la parcela para construir;
- a tal escala y con tal precisión;
- con el relieve, los linderos y los servicios;
- entregado en tal formato.

Eso es el pliego de condiciones. Sin él podemos hacer un buen levantamiento que al cliente no le sirva: la escala no es la que necesita, la precisión tampoco, faltan elementos. El trabajo está hecho, pero no sirve para nada.

Con una herramienta pasa lo mismo. En esta lección vamos a escribir el pliego de condiciones de `canopy-growth`. Para ello respondemos a cinco preguntas:

1. ¿Quién va a usar la herramienta?
2. ¿Qué quiere saber esa persona?
3. ¿Qué tenemos de entrada?
4. ¿Qué le vamos a entregar?
5. ¿Qué no prometemos?

## Quién la va a usar

**Pensemos a quién le hace falta, en realidad, la velocidad de crecimiento del bosque.** Vemos tres usuarios.

- **La distribuidora.** Por ejemplo, Viesgo o EDP Redes, que abastecen los pueblos de Pajares. Tienen que planificar dónde y cuándo hacer la poda y la tala junto a las líneas.
- **Los servicios forestales y los investigadores.** Necesitan saber a qué velocidad crece el bosque en la montaña: qué especies, en qué laderas, a qué altitud.
- **Nuestro propio proyecto.** Es fácil olvidarse de él. Las partes 2 y 3 de Pajares van a tomar datos de `canopy-growth`. El modelo de riesgo necesita saber qué altura tienen hoy los árboles y cuál tendrán.

¿Para qué enumerarlos a todos? Porque cada uno necesita un resultado un poco distinto. La distribuidora necesita un mapa a lo largo de las líneas. El servicio forestal, cifras por especies y laderas. El modelo de riesgo, números que pueda meter en el cálculo.

✎ **Tus datos:** quiénes son los usuarios en tu zona (punto 1 de la lista al final de la lección).

## Qué quiere saber la distribuidora

Tomemos al usuario principal: la distribuidora. Su pregunta es: **¿dentro de cuántos años se volverán peligrosos los árboles junto a la línea?**

¿Por qué no le basta con conocer la altura de los árboles hoy? Hagamos la cuenta con un ejemplo sencillo.

Un haya de 18 m está a 20 m del conductor. ¿Caerá sobre él? No. Aunque caiga justo hacia la línea, le faltarán 2 m. Hoy no es peligrosa.

Pero el haya crece. Según el inventario forestal, un haya media en Asturias gana unos 0,1 m al año[^1]. Los 2 m que le faltan los ganará en unos 20 años:

2 m ÷ 0,1 m al año = 20 años.

Dentro de 20 años, esa misma haya ya puede alcanzar el conductor.

**La altura muestra lo que es peligroso hoy. La velocidad de crecimiento muestra lo que será peligroso después.** La poda y la tala se planifican con años de antelación, así que la distribuidora necesita las dos cosas.

En la calle bajo la línea el panorama es otro. Allí se cortó el bosque y vuelve a crecer: los árboles son jóvenes y hay mucha luz. Esperamos que allí el bosque crezca bastante más deprisa que la vieja haya de la ladera. Eso es justo lo que vamos a comprobar en el proyecto.

✎ **Tus datos:** un árbol junto a la línea, con su altura, su distancia al conductor y la velocidad de crecimiento de su especie (punto 2).

## Qué tenemos de entrada

**De entrada tenemos los vuelos LiDAR gratuitos que España hace de todo el país.** El programa se llama PNOA.

Qué es el LiDAR lo veremos en detalle en el próximo capítulo. De momento nos basta con la idea. Pensemos en una estación total sin prisma: lanza un haz y, por el reflejo, mide la distancia hasta un punto. Ahora montemos ese aparato en un avión y hagamos que mida cientos de miles de puntos por segundo. El avión sobrevuela el bosque y obtenemos millones de puntos con coordenadas y altitud.

Parte de los puntos rebota en las copas de los árboles; otra parte atraviesa el follaje y llega al suelo. Ese conjunto de puntos se llama **nube de puntos**.

A partir de la nube podemos obtener dos superficies: el terreno y la parte superior del bosque. A la parte superior del bosque la llamamos **dosel**. La diferencia entre el dosel y el terreno es la altura del bosque en cada lugar.

España ya ha volado todo el país con LiDAR varias veces, en años distintos. Veamos cómo sacar de ahí el crecimiento. Comparamos la altura del dosel en dos vuelos:

- en el primer vuelo había 17 m;
- 12 años después, 18,2 m;
- la diferencia: 18,2 − 17 = 1,2 m;
- la dividimos entre los años: 1,2 m ÷ 12 años = 0,1 m al año.

Esa es la **velocidad de crecimiento**.

✎ **Tus datos:** qué vuelos LiDAR hay sobre tu zona y de qué años (punto 3).

## Qué le vamos a entregar al usuario

Repasemos cada decisión y por qué la tomamos.

- **Un mapa de la velocidad de crecimiento en cuadrículas de 20–30 m, no por árboles sueltos.** Los vuelos antiguos del PNOA tienen pocos puntos: en el primer vuelo, medio punto por metro cuadrado[^2]. Con eso no se puede ver bien una copa aislada. En cambio, en una cuadrícula de 20–30 m ya hay puntos suficientes para encontrar el punto más alto.
- **Cada número lleva su intervalo.** Es como la precisión en topografía: no entregamos una cota sin su error. Por eso no escribimos «0,3 m al año», sino «0,3 m al año, más o menos tanto». Cómo se calcula el intervalo está explicado en el libro de estadística. Aquí lo que nos importa es la idea: un número sin su error no le sirve de nada a un ingeniero.
- **La ficha de pasadas.** El avión no vuela todo el país de una vez, sino en pasadas, de ida y vuelta. Las pasadas se vuelan en días distintos y a veces con aparatos distintos. Para cada pasada anotamos si podemos fiarnos de ella y, si no, por qué.
- **Un informe de dónde podemos fiarnos de las cifras.** Si en algún sitio los vuelos no permiten medir el crecimiento, lo decimos tal cual. Eso también es un resultado.
- **Una forma cómoda de usarla.** Un paquete de Python, para quien programa. Un complemento de QGIS, para quien no: dibujamos la zona, pulsamos un botón y obtenemos el mapa.

✎ **Tus datos:** qué precisión necesita el usuario y en qué forma le viene mejor recibir el resultado (punto 4).

## Qué no prometemos

Este paso es fácil saltárselo, y es un error. Si no escribimos con honestidad los límites, el usuario pensará por su cuenta que la herramienta sabe hacer más, y se equivocará.

- **El crecimiento de cada árbol por separado.** En los vuelos antiguos hay muy pocos puntos; ya lo hemos visto.
- **Sustituir los vuelos de la propia distribuidora.** Las compañías encargan sus propios vuelos sobre las líneas, y en ellos hay decenas de veces más puntos: 15–25 por metro cuadrado[^3]. No competimos con eso. Damos lo que sus vuelos no tienen: el pasado. Los vuelos gratuitos del PNOA de años anteriores existen para cualquier lugar.
- **La causa del crecimiento.** Medimos a qué velocidad crece el bosque, pero no decimos por qué: el clima, la especie, el suelo u otra cosa.
- **Una predicción más allá de prolongar sin más.** Podemos decir: «si el bosque sigue creciendo igual, dentro de 20 años tendrá tal altura». Pero no podemos prometer que siga creciendo igual.

✎ **Tus datos:** qué queda fuera del encargo en tu zona (punto 5).

## Qué ha salido

Juntémoslo todo. Este es nuestro pliego de condiciones en un párrafo:

> `canopy-growth` mide a qué velocidad crece la altura del bosque a partir de los vuelos LiDAR repetidos y gratuitos del PNOA. El resultado es un mapa de la velocidad de crecimiento en cuadrículas de 20–30 m con su error, una ficha de pasadas y un informe de dónde podemos fiarnos de las cifras. A las distribuidoras les muestra dónde y cuándo se volverá peligroso el bosque junto a las líneas; a los servicios forestales, cómo crece el bosque; a las partes 2 y 3 del proyecto, la altura de los árboles hoy y dentro de unos años. La herramienta no mide árboles sueltos, no sustituye los vuelos de las compañías, no explica la causa del crecimiento y no promete que el crecimiento se mantenga.

## ✎ Tus datos: qué hace falta para tu pliego de condiciones

Un pliego así se puede escribir para cualquier zona. Para ello hace falta:

1. **La zona y los usuarios.** Dónde está la zona; a quién le hace falta el resultado y para qué lo quiere cada uno.
2. **La pregunta del usuario principal y un ejemplo.** Qué quiere saber; un árbol junto a la línea, con su altura, su distancia al conductor y la velocidad de crecimiento de su especie.
3. **Los datos de entrada.** Qué vuelos LiDAR repetidos hay sobre la zona y de qué años. En España, las coberturas del PNOA.
4. **El resultado.** Qué precisión hace falta, qué tamaño de cuadrícula conviene y en qué forma entregarlo: mapa, tabla o complemento.
5. **Los límites.** Qué no promete la herramienta en esa zona y por qué.

## Resumen

### Conceptos nuevos

- **Pliego de condiciones:** el acuerdo sobre qué hacemos, para quién y qué no hacemos. Ejemplo: un encargo de levantamiento topográfico con su escala y su precisión.
- **Nube de puntos:** millones de puntos con coordenadas que el LiDAR toma desde el avión. Ejemplo: puntos en las copas de los árboles y en el suelo bajo ellos.
- **Dosel:** la parte superior del bosque, la superficie que pasa por lo alto de las copas. Ejemplo: una altura del dosel de 18 m significa que la parte superior del bosque está a 18 m sobre el suelo.
- **Velocidad de crecimiento:** cuántos metros al año aumenta la altura del dosel. Ejemplo: 1,2 m en 12 años son 0,1 m al año.
- **Ficha de pasadas:** el registro de cada pasada de vuelo, si podemos fiarnos de ella y por qué. Ejemplo: una pasada se voló sin hoja, así que la corregimos o no la usamos.

## Preguntas para un ingeniero

**¿Por qué a la distribuidora no le basta con conocer la altura de los árboles hoy?**

:::{dropdown} Respuesta
La altura muestra el peligro de ahora, pero la poda y la tala se planifican con años de antelación. Un árbol que hoy no llega al conductor puede llegar dentro de unos años.
:::

**¿Por qué calculamos el crecimiento en cuadrículas de 20–30 m y no por árboles?**

:::{dropdown} Respuesta
En los vuelos antiguos hay demasiado pocos puntos para ver bien una copa aislada. En una cuadrícula de 20–30 m hay puntos suficientes para encontrar la parte superior del bosque.
:::

**¿Para qué escribir lo que la herramienta no promete?**

:::{dropdown} Respuesta
Si no, el usuario pensará que la herramienta sabe hacer más y sacará una conclusión equivocada. Unos límites honestos son parte del resultado.
:::

(es-ex-1-1)=
## Ejercicios

1. **Recordamos.** ¿Cómo se llama la parte superior del bosque, la superficie que pasa por lo alto de las copas?

2. **Corregimos el error.** «Velocidad de crecimiento = altura del dosel en el segundo vuelo dividida entre los años que hay entre los vuelos».

3. **Del prólogo.** ¿Qué tumbó árboles sobre las líneas en Asturias en 2018 y en qué se diferencia de Andalucía?

[Soluciones de estos ejercicios](#es-sol-1-1)

---

## Notas y fuentes

Fuentes consultadas el 20 de noviembre de 2026.

[^1]: [IFN4, Asturias, tabla 926: crecimiento medio anual en altura por especies; haya, 0,11 m al año](https://www.asturias.es/documents/4911212/4919385/20240117-IFN4-indice-tablas-mapas.pdf/ee7edde7-e40f-7331-a482-58a1b76ba5ee).
[^2]: [IGN y MITECO, proyecto PNOA-LiDAR, 2022: 0,5 puntos/m² en la primera cobertura](https://www.miteco.gob.es/content/dam/miteco/es/ceneam/grupos-de-trabajo-y-seminarios/patrimonio-natural/20220426_fernandez_guillermo_proyecto_pnoa_lidar_miteco_ign_tcm30-540463.pdf).
[^3]: [V Congreso de Smart Grids, 2019: para los vuelos sobre líneas se suelen encargar 15–25 puntos/m²](https://smartgridsinfo.es/comunicaciones/comunicacion-aplicacion-avances-lidar-aerotransportado-modelos-crecimiento-vegetacion-inspeccion-mantenimiento-lineas-transporte).
