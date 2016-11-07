+++
description = ""
keywords = [
]
categories = ["recsys"]
date = "2016-11-06T22:36:00-03:00"
title = "Comentarios sobre The Link-Prediction Problem for Social Networks"

+++

El _paper_ se trata sobre _link-prediction_, que es el problema de poder predecir si la generación de vinculos en una comunidad. El problema de este tipo de estudio es que **las redes sociales son muy dinámicas**, versus los problemas de _ratings_ donde ocurren cambios pocas veces al pasar el tiempo. Este tipo de problemas se tratan en base a _snapshots_ de la red modelada como un grafo para un tiempo `t` y se intenta predecir a futuro en base a sus características implícitas.

## Relaciones

Lo que si es discutible, es que trata toda interacción dentro del tiempo de igual forma o sin depender de interacciones pasadas. Podríamos decir que es _naive_ en este sentido y que el problema podría extenderse a evaluar la efectividad de los vínculos.

Por ejemplo, si llega a existir un vínculo entre dos nodos y sus futuras interacciones ocurren entre largos periodos de tiempo, quizás este vínculo es _débil_. Por otro lado, si ocurren con más frecuencia en intervalos más cortos, quizás el vínculo es más _fuerte_.

Podría modelarse le problema como un grafo donde las aristas sean el tiempo que transcurre para que dos nodos interactuen (habrían `n` aristas entre pares de nodos). Aunque habría que probar si vale la pena usar algún algoritmo relacionado con las distancias, pues en la _Figura 6 y 7_ del paper se ve que el rendimiento de algo así podría ser no bueno.
