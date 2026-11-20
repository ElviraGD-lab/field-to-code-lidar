---
title: "Lección 1.1. Quién necesita la velocidad de crecimiento del bosque y para qué"
---

## Por dónde empieza un ingeniero

Antes de escribir código o descargar datos, aclaramos qué hay que hacer exactamente y para quién. Esto ya lo conocemos de la topografía.

Pensemos en cómo llega un encargo de levantamiento topográfico. El cliente no dice «levántame algo». Dice más bien esto:

- necesito un plano de la parcela para construir una casa;
- a tal escala y con tal precisión;
- en el plano tienen que aparecer el relieve, los linderos y los servicios;
- entregado en tal formato.

Eso es el pliego de condiciones. Sin él podemos hacer un levantamiento estupendo que al cliente no le sirve: la escala no es la que necesita, la precisión tampoco, faltan elementos. El trabajo está hecho, pero no sirve para nada.

Con una herramienta pasa lo mismo. Hoy escribimos el pliego de condiciones de `canopy-growth`. Vamos a responder a cinco preguntas:

1. ¿Quién es el usuario?
2. ¿Qué pregunta tiene?
3. ¿Qué tenemos de entrada?
4. ¿Qué damos de salida?
5. ¿Qué no prometemos?

Vamos por orden.

## Paso 1. Quién es el usuario

**La herramienta tiene varios usuarios, y cada uno tiene su propia pregunta.**

- **La distribuidora**, por ejemplo Viesgo o EDP Redes, que abastecen los pueblos de Pajares. Tiene que planificar la poda y la tala junto a las líneas: dónde y cuándo cortar para que los árboles no lleguen a los conductores.
- **Los servicios forestales y los investigadores.** Necesitan saber a qué velocidad crece el bosque en la montaña: qué especies, en qué laderas, a qué altitud.
- **Nuestro propio proyecto.** Es el usuario del que más fácil resulta olvidarse. Las partes 2 y 3 de Pajares van a tomar datos de `canopy-growth`. El modelo de riesgo necesita saber qué altura tienen hoy los árboles y cuál tendrán dentro de unos años.

¿Por qué conviene enumerarlos a todos? Porque del usuario depende cómo tiene que ser el resultado. La distribuidora necesita un mapa a lo largo de las líneas. El servicio forestal, cifras por especies y laderas. El modelo de riesgo, números que pueda meter en el cálculo.

✎ **Tus datos:** los usuarios de nuestra zona (punto 1 de la lista al final de la lección).

## Paso 2. Qué pregunta tiene el usuario

Tomemos al principal usuario externo: la distribuidora. Su pregunta es esta: **¿dentro de cuántos años se volverán peligrosos los árboles junto a la línea?**

¿Por qué no basta con conocer la altura de los árboles hoy? Veámoslo con un ejemplo sencillo.

Un haya de 18 m está a 20 m del conductor. ¿Caerá sobre él? No: aunque caiga justo hacia la línea, le faltarán dos metros. Hoy no es peligrosa.

Pero el haya crece. Según el inventario forestal, un haya media en Asturias gana unos 0,1 m al año[^1]. Así que los 2 m que le faltan los ganará en unos 20 años. Dentro de 20 años, esa misma haya ya puede alcanzar el conductor.

La altura muestra lo que es peligroso hoy. La velocidad de crecimiento muestra lo que será peligroso mañana. La poda y la tala se planifican con años de antelación, así que la distribuidora necesita las dos cosas.

En la calle bajo la línea el panorama es otro. Allí se cortó el bosque y vuelve a crecer: árboles jóvenes y mucha luz. Esperamos que allí el bosque crezca bastante más deprisa que la vieja haya de la ladera. Eso es justo lo que vamos a comprobar.

✎ **Tus datos:** nuestro árbol, su altura, su distancia al conductor y la velocidad de crecimiento de su especie (punto 2).

## Paso 3. Qué tenemos de entrada

**Nuestra entrada son los vuelos LiDAR gratuitos que España hace de todo el país.** El programa se llama PNOA.

**Qué es el LiDAR, en pocas palabras.** Lo veremos en detalle en el próximo capítulo; de momento nos basta con la idea. Pensemos en una estación total sin prisma. Lanza un haz y, por el reflejo, mide la distancia hasta un punto. Ahora montemos ese aparato en un avión y hagamos que mida cientos de miles de puntos por segundo. El avión sobrevuela el bosque y obtenemos millones de puntos con coordenadas y altitud. Parte de los puntos rebota en las copas de los árboles; otra parte atraviesa el follaje y llega al suelo. Ese conjunto de puntos se llama nube de puntos.

A partir de la nube podemos obtener dos superficies: el terreno y la parte superior del bosque. La diferencia entre ellas es la altura del bosque en cada lugar. A la parte superior del bosque la llamamos dosel.

España ya ha volado todo el país con LiDAR varias veces, en años distintos. Si comparamos la altura del dosel en dos vuelos y dividimos la diferencia entre los años que hay entre ellos, obtenemos la velocidad de crecimiento. Por ejemplo: había 17 m y, 12 años después, 18,2 m. El bosque ha crecido 1,2 m en 12 años, es decir, 0,1 m al año.

✎ **Tus datos:** qué vuelos LiDAR hay sobre nuestra zona y de qué años (punto 3).

## Paso 4. Qué damos de salida

En este paso, cada decisión tiene su motivo.

- **Un mapa de la velocidad de crecimiento en cuadrículas de 20–30 m, no por árboles sueltos.** Los vuelos antiguos del PNOA tienen pocos puntos: en el primer vuelo, medio punto por metro cuadrado[^2]. Con eso no se puede ver bien una copa aislada. En cambio, en una cuadrícula de 20–30 m ya hay puntos suficientes para encontrar con seguridad el punto más alto.
- **Cada número lleva su intervalo.** Es lo mismo que la precisión en topografía. No entregamos una cota sin su error. No escribimos «0,3 m al año», sino «0,3 m al año, más o menos tanto». Cómo se calcula el intervalo lo vimos en el libro de estadística. Aquí lo importante es la idea: un número sin su error no le sirve de nada a un ingeniero.
- **La ficha de pasadas.** El avión no vuela todo el país de una vez, sino en pasadas, de ida y vuelta, como quien siega la hierba. Las pasadas se vuelan en días distintos y a veces con aparatos distintos. Para cada pasada anotamos si podemos fiarnos de ella y, si no, por qué.
- **Un informe de dónde podemos fiarnos de las cifras y dónde no.** Si en algún sitio el vuelo no permite medir el crecimiento, lo decimos tal cual. Eso también es un resultado.
- **Cómo se usa.** Un paquete de Python, para quien programa. Un complemento de QGIS, para quien no: dibujamos la zona, pulsamos un botón y obtenemos el mapa.

✎ **Tus datos:** qué precisión necesita el usuario y en qué forma le viene mejor recibir el resultado (punto 4).

## Paso 5. Qué no prometemos

Los ingenieros se saltan a menudo este paso, y hacen mal. Si no escribimos con honestidad los límites, el usuario pensará por su cuenta que la herramienta sabe hacer más, y se equivocará.

- **El crecimiento de cada árbol por separado.** Ya hemos visto por qué: en los vuelos antiguos hay muy pocos puntos.
- **Sustituir los vuelos de la propia distribuidora.** Las compañías encargan sus propios vuelos sobre las líneas, y en ellos hay decenas de veces más puntos: 15–25 por metro cuadrado[^3]. No competimos con eso. Damos lo que sus vuelos no tienen: el pasado. Los vuelos gratuitos del PNOA de hace muchos años existen para cualquier lugar.
- **La causa del crecimiento.** Medimos a qué velocidad crece el bosque, pero no decimos por qué: el clima, la especie, el suelo u otra cosa.
- **Una predicción más allá de prolongar sin más.** Podemos decir: «si el bosque sigue creciendo igual, dentro de 20 años tendrá tal altura». Pero no podemos prometer que siga creciendo igual.

✎ **Tus datos:** qué queda fuera del encargo en nuestra zona (punto 5).

## Qué ha salido

Juntémoslo todo. Este es nuestro pliego de condiciones en un párrafo:

> `canopy-growth` mide a qué velocidad crece la altura del bosque a partir de los vuelos LiDAR repetidos y gratuitos del PNOA. El resultado es un mapa de la velocidad de crecimiento en cuadrículas de 20–30 m con su error, una ficha de pasadas y un informe de dónde podemos fiarnos de las cifras. A las distribuidoras les dice dónde y cuándo se volverá peligroso el bosque junto a las líneas; a los servicios forestales, cómo crece el bosque; a las partes 2 y 3 del proyecto, la altura de los árboles hoy y dentro de unos años. La herramienta no mide árboles sueltos, no sustituye los vuelos de las compañías, no explica la causa del crecimiento y no promete que el crecimiento se mantenga.

## ✎ Tus datos: qué hace falta para el pliego de condiciones

Esta misma conversación se puede tener para cualquier zona. Para nuestro propio pliego de condiciones necesitamos saber:

1. **La zona y los usuarios.** Dónde está la zona; a quién le hace falta el resultado (una distribuidora, un servicio forestal, investigadores, nuestro propio proyecto) y para qué lo quiere cada uno.
2. **La pregunta del usuario principal y un ejemplo.** Qué quiere saber exactamente. Para el ejemplo hace falta un árbol junto a la línea: su altura, su distancia al conductor y la velocidad de crecimiento de su especie.
3. **Los datos de entrada.** Qué vuelos LiDAR repetidos hay sobre la zona y de qué años. En España, las coberturas del PNOA.
4. **La salida.** Qué precisión necesita el usuario, qué tamaño de cuadrícula conviene y en qué forma entregar el resultado: mapa, tabla o complemento.
5. **Los límites.** Qué no promete la herramienta en esa zona y por qué.

## Resumen

- **Pliego de condiciones:** el acuerdo sobre qué hacemos, para quién y qué no hacemos. Ejemplo: un encargo de levantamiento topográfico con su escala y su precisión.
- **Nube de puntos:** millones de puntos con coordenadas que el LiDAR toma desde el avión. Ejemplo: puntos en las copas de los árboles y en el suelo bajo ellos.
- **Dosel:** la parte superior del bosque, la superficie que pasa por lo alto de las copas. Ejemplo: una altura del dosel de 18 m significa que la parte superior del bosque está a 18 m sobre el suelo.
- **Velocidad de crecimiento:** cuántos metros al año aumenta la altura del dosel. Ejemplo: 1,2 m en 12 años son 0,1 m al año.
- **Ficha de pasadas:** el registro de cada pasada de vuelo, si podemos fiarnos de ella y por qué. Ejemplo: una pasada se voló sin hoja, así que la corregimos o la descartamos.

## Preguntas de comprensión

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
## Ejercicios de memoria

1. **Recordamos.** ¿Cómo se llama la parte superior del bosque, la superficie que pasa por lo alto de las copas?

2. **Corregimos el error.** «Velocidad de crecimiento = altura del dosel en el segundo vuelo dividida entre los años que hay entre los vuelos».

3. **Del prólogo.** ¿Qué mecanismo tumbó árboles sobre las líneas en Asturias en 2018 y en qué se diferencia del sur de España?

[Soluciones de estos ejercicios](#es-sol-1-1)

---

## Notas y fuentes

Fuentes consultadas el 19 de noviembre de 2026.

[^1]: [Cuarto Inventario Forestal Nacional (IFN4), Asturias: tabla 926, media del incremento anual de altura por especie; haya, 0,11 m al año](https://www.asturias.es/documents/4911212/4919385/20240117-IFN4-indice-tablas-mapas.pdf/ee7edde7-e40f-7331-a482-58a1b76ba5ee).
[^2]: [IGN y MITECO, proyecto PNOA-LiDAR, 2022: 0,5 puntos/m² en la primera cobertura](https://www.miteco.gob.es/content/dam/miteco/es/ceneam/grupos-de-trabajo-y-seminarios/patrimonio-natural/20220426_fernandez_guillermo_proyecto_pnoa_lidar_miteco_ign_tcm30-540463.pdf).
[^3]: [V Congreso de Smart Grids, 2019: en los vuelos LiDAR para líneas eléctricas se suelen pedir 15–25 puntos/m²](https://smartgridsinfo.es/comunicaciones/comunicacion-aplicacion-avances-lidar-aerotransportado-modelos-crecimiento-vegetacion-inspeccion-mantenimiento-lineas-transporte).
