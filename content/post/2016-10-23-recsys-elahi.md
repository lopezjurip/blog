+++
title = "Comentarios sobre A survey of active learning in CF recommender systems"
description = ""
keywords = []
categories = ["recsys"]
date = "2016-10-23T21:06:15-03:00"
+++

Otro paper que hace un resumen del estado actual de los sistemas recomendadores.

## Aprendizaje Activo

Este le da énfasis a lo que es el _Active Learning_, que corresponde a la funcionalidad de un sistema recomendador que ayuda a mejorar las recomendaciones. El fin es poder de solucionar en parte el problema del _Cold Start_ y la falta de datos.

Es importante mencionar que esto ocurre Online y preguntándole al usuario sobre su gusto sobre ciertos ítems que el sistema considera importantes para mejorar las recomendaciones. Idealmente, preguntándole de manera personalizada a cada usuario y no preguntarle a todos lo mismo, con el fin de obtener resultados más _finos_.

## Estrategias

Dentro de las estrategias, me llama la atención que aparecen conceptos de _Minería de Datos_ como _**Information Gain** through Clustered Neighbours (IGCN)_ y _Decision Tree_. Hace bastante sentido, pues queremos obtener la mayor información posible del usuario (ganancia de información) con la menor cantidad de preguntas. También que podemos clasificar a un usuario con un árbol de decisión en base a _pruebas_ sobre sus gustos.

Lo que más destaco es que mencionan la posibilidad de **aplicar diversas estrategias sobre grupos distintos de usuarios**, con el fin de encontrar la mejor estrategia para ellos. Esto es casi como _"recomendarles la mejor estrategia recomendadora"_. Esto es muy aplicable hoy en día usando _A/B Testing_.

### Offline vs Online

También se discute las diferencias entre sistemas _offline_ (que se entrenan y prueban dentro de un entorno _científico_) versus los _online_ que corren en producción con usuario reales. Si bien trata el tema a favor de _online_ por ser más fiel a la realidad, todavía no veo implementaciones, ideas, sugerencias y/o recomendaciones de como llevarlos a la realidad.

## Críticas finales

En primer lugar la tabla `Table 1 – Performance comparison of active learning strategies` no deja explícito el criterio para decir que un algoritmo es _Very good_, _Good_, o _Poor_ bajo ciertas métricas.

**Finalmente me parece extraño que el artículo sea de este año (2016) y se siga hablando de técnicas y algoritmos de hace 10-15 años.** Hace mención de técnicas como [_aprendizaje reforzado_](https://en.wikipedia.org/wiki/Reinforcement_learning), pero yo esperaría más profundidad en [redes neuronales](https://es.wikipedia.org/wiki/Red_neuronal_artificial) y [tensores](https://en.wikipedia.org/wiki/TensorFlow) ya a estas alturas.
