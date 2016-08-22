+++
categories = ["recsys"]
date = "2016-08-21T23:36:39-03:00"
description = ""
keywords = []
title = "Comentarios sobre Item-Based CF"

+++

El paper aborda el tema de la **escalabilidad** y **esparcicidad** de los _Filtros Colaborativos_. En particular el cuello de botella ocurre al buscar los vecinos más cercanos de los usuarios, que se complica más cuando empiezan a aparecer más personas. Así también el problema de la precisión cuando tienen muchos ítems pero pocos _ratings_.

El _CF_ basado en ítems parte haciendo algo similar a los esquemas basados en modelos. Esto es, calcular las similaridades entre los ítems y luego escoger los ítems más similares.

Para medir la similaridad proponen tres métodos: _Cosine-based_, _Correlation-based_ y _Adjusted-Cosine_. De los cuales el último tiene mejor rendimiento (menor _MAE_). La gracia de este último es que regula la parcialidad de los _ratings_ del usuario (ya sea _pesimista_, _optimista_, _bi-modal_, etc), versus el _Correlation-based_ que regula los _ratings_ del ítem en particular.

El gran aporte del paper es mostrar que **es posible obtener muy buen rendimiento eligiendo pocos ítems similares**.

Sin embargo, esto parece no aplicar cuando se trata de ambientes de gran interacción y generación de contenido. Pues espera que el _set_ de ítems sea relativamente estático. Por ejemplo, esto no serviría para recomendar _Tweets_ o, en un caso algo ridículo, acciones en la bolsa.

Tampoco queda claro si esto funcionaría para ítems nuevos (_cold-start_). **Quizás se podrían aplicar medidas de similaridad en base a su contenido para soportar este tipo de casos**, ya que son ítems. Podemos incluir, por ejemplo, similaridad de género musical en el caso de canciones o de precio y marca en un _e-commerce_. Una ventaja de esto versus una metodología _user-based_ es que no tenemos que hacerle preguntas al usuario para poder encontrar similitudes 😄.
