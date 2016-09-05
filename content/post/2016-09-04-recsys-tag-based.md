+++
categories = ["recsys"]
date = "2016-09-04T21:18:51-03:00"
description = ""
keywords = []
title = "Comentarios sobre Collaborative Filtering for Social Tagging Systems"

+++

El _paper_ introduce los nuevos desafíos de los sistemas recomendados en base a etiquetas.

El gran cambio que producen es que los usuarios pueden dar información (poner etiquetas) que deben pasar por filtros de relevancia y calidad. **Pues tienen la libertad de poner lo que se les de la gana.**

El objetivo es probar si este tipo de sistemas permite encontrar mejores usuarios similares y por ende, mejores recomendaciones.

Dentro de las medidas que se tomaron durante la preparación, fue buena idea normalizar las _tags_ y elegir más usuarios candidatos de los previstos para no sesgar las recomendaciones al área científica (_PAWS_). En caso contrario, se hubiera trabajado con un _dataset_ muy _sparse_ y hubiera sido fácil encontrar vecinos, pues gran parte de los usuarios tendrían intereses comunes.

Quizás una variable a considerar en futuros trabajos es **incorporar la relevancia de la tag al item**. Que quiero decir con esto:

* Debería ser considerado de distinta manera que dos usuarios etiqueten un item con una etiqueta muy repetida a ese _item_, versus una etiqueta poco común para el _item_.
* El orden con que un usuario añade etiquetas a veces trae implícitamente su relevancia: las primeras suelen ser más relevantes que las últimas.
