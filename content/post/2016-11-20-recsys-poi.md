+++
title = "Comentarios sobre A Survey of POI recommendation"
description = ""
keywords = []
categories = ["recsys"]
date = "2016-11-20T21:11:12-03:00"
+++

El _paper_ nos hace un recuento de cómo se están haciendo las recomendaciones en base a puntos de interés (_POI_). Este tipo de recomendaciones se puede hacer parecer a las recomendaciones típicas que hacíamos con ítems, solo que hay que considerar otros factores como las distancias, las concentraciones de POI, factores sociales y el gran **problema de la esparcicidad**, que para estos casos es más grave que antes.

Se habla de que estos problemas pueden dividirse en las siguientes categorias:

* _Pure check-in data based POI recommendation approaches._
* _Geographical influence enhanced POI recommendation approaches._
* _Social influence enhanced POI recommendation approaches._
* _Temporal influence enhanced POI recommendation approaches._

Si bien se dan hartas explicaciones de cómo aplican y sus limitantes, no se muestras resultados empíricos.

## Demasiados factores

Yo creo que el principal problema de estos _approaches_, es que cuesta mezclarlos en un recomendador híbrido. Además sería muy costoso ir probando cuáles factores pesan más que los otros.

En este sentido, podría atacarse el problema con **Redes Neuronales**. No soy experto en el tema, pero creo que con esta cantidad de factores podemos entrenar una red y abstraernos de toda implementación de los algoritmos y heurísticas con sesgos mostradas en el _paper_. De esta forma los mismos datos podrían modelar el problema y las recomendaciones.
