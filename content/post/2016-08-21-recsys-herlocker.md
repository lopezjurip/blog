+++
categories = ["recsys"]
date = "2016-08-21T23:08:03-03:00"
description = ""
keywords = []
title = "Comentarios sobre Filtrado Colaborativo de Herlocker J."

+++

El _paper_ parte con el problema de _recomendación colaborativa_ y lo divide en etapas. Luego en cada etapa toma ciertos posibles modos de abordarla y los pone en comparación. Finalmente concluye con **un flujo de trabajo sugerido**, no obstante avisa de ciertos cuidados a considerar dependiendo del contexto y el tipo de datos que tenemos.

La ventaja de un filtrado colaborativo es que no depende de la información intrínseca de un ítem, sino que se basa en el contexto de los usuarios que lo recomiendan y como me parezco a ellos. De esta forma es posible obtener recomendaciones de ítems que a ese tipo de usuarios les parece importantes.

Esto es bueno porque pueden ocurrir recomendaciones _'serendipitous'_ (_fortuitas_), que son items con contenido que yo no esperaba pero me resultaron muy preciadas.

### Sobre las semejanzas de los usuarios

Creo que la razón por la cual la _correlación de Spearman_ funciona mejor (por lo general) que la de _Pearson_, es que cuando a nosotros los humanos nos gusta mucho algo, **la relación de eso con algo que solo nos _"gusta más o menos"_ no es lineal, sino logarítmica**. Esto, en mi opinión, hace que la medida de _Spearman_ funcione mejor.

> Ver: http://www.scientificamerican.com/article/a-natural-log.

Me pareció muy interesante lo que salió a la luz cuando analizaban los vecinos más cercanos. Uno suele pensar en estas áreas de datos que mientras más información tengamos, siempre es mejor. Sin embargo, se aprecia que conviene trabajar dentro de cierto rango de vecinos. Pues muchos hacen menos precisa la recomendación y con muy pocos tenemos problemas _coverage_.

En una parte habla de que _el proceso de recomendación debe hacerse en menos de 1 segundo para miles de usuarios_, sin embargo esto contradice un poco la metodología que están usando porque ellos mismo declaran que puede conllevar a problemas de escalabilidad.

Finalmente, puesto que realizaron todo sobre el dataset de  MovieLens, sus algoritmos fueron probados para ítems del tipo _película/música_, entonces **no podemos asegurar que este flujo de trabajo sugerido funcione en otros ámbitos**. Por ejemplo, recomendar una persona en alguna aplicación de citas.
