+++
title = "Comentarios sobre A New Approach to Evaluating Novel Recommendations"
description = ""
keywords = []
categories = ["recsys"]
date = "2016-11-14T00:59:29-03:00"

+++

El paper muestra dos métodos para evaluar qué tan novedoso son algunas recomendaciones.

La percepción de la _novedocidad_ es importante para el usuario porque da posibilidad a cierta espontaneidad y se percibe como más auténtica. La dificultad de esto es que cuesta balancearlo con la familiaridad y la relevancia de ítems a recomendar.

## User-centric

Se critica a la metodología típica del _leave-n-out_ porque solo mide precisión y no qué tan útil es la información ni qué tan valiosa es.

Para medir esto último se necesita _feedback_ (explícito o implícito).

## Item-centric

Si bien _Colaborative Filtering (CF)_ es afectado por la popularidad de los ítems, los usuarios perciben que las recomendaciones son de mejor calidad.

---

Finalmente, no se deja claro el valor o ecuación que permite calcular lo que se propuso el paper, sino que queda como a interpretación del que analice los resultados.
