+++
categories = ["recsys"]
date = "2016-09-10T21:22:52-03:00"
description = ""
keywords = []
title = "Comentarios sobre Matrix Factorization Techniques"

+++

El _paper_ trata sobre los modelos de filtrado colaborativo donde parte mencionando los métodos en _base a vecinos_, junto con los problemas asociados a este.

Luego se habla de los métodos en base a _factores latentes_. Aquí es donde entra las técnicas de factorización de matrices. Algunas ventajas de estos métodos son:

* Se permite usar feedback implícito. Esto datos sobre acciones que realizan los usuarios pero que no las declaran consiente y explícitamente al sistema. Como las acciones de este estilo son más frecuentes que los actos de _ratear_ un elemento, la matriz de información que tenemos deja de ser tan _sparse_ 👌

* Escala mejor y es paralelizable (con _"alternating least squares"_ y bajo ciertas condiciones).

Un punto rescatable del texto, es que tratan el tema de la **temporalidad**. Pues los gustos van cambiando y pueden ocurrir eventos externos que alteren la interacción típica de los usuarios a los ítems. Dejan esta opción de manera bastante general, cosa que uno pueda llegar e implementar las funciones en función al tiempo `t`.

Algo que quizás no se toca en el tema del _feedback implícito_ es el **problema de visibilidad**. Esto es: puede que algunos ítems tengan muy poca interacción, pero no por el hecho de que no sean interesantes, sino que están _"ocultos"_ o son de difícil acceso. Se necesitaría una especie de retroalimentación sobre la exposición de estos elementos y la interacción a estos, para incluirlo dentro de la ecuación que calcula los _ratings_.
