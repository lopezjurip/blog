+++
categories = ["recsys"]
date = "2016-09-04T23:13:27-03:00"
description = ""
keywords = []
title = "Comentarios sobre Content-Based Recommendation Systems"

+++

A modo de reseña, los sistemas recomendados basados en contenido analizan la descripción de los _items_ para buscar cuáles son de interés para el usuario.

¿Cómo los representamos?

Bueno si es texto libre, podemos tomar cada palabra y pasarla por un proceso de _stemming_ para considerarlas como _features_.

Típicamente está el problema de que se pierde el contexto de las palabras. No obstante, me pregunto si los algoritmos de stemming están adaptados al rápido cambio del lenguaje. Pensemos que en Instagram los _emojis_ compiten con los caracteres alfanuméricos en cantidades.

* [The Rise of Emoji on Instagram Is Causing Language Repercussions](http://bits.blogs.nytimes.com/2015/05/01/the-rise-of-emoji-on-instagram-is-causing-language-repercussions/?_r=0)
* [Emojineering Part 1: Machine Learning for Emoji Trends](http://instagram-engineering.tumblr.com/post/117889701472/emojineering-part-1-machine-learning-for-emoji)

También está la opción de usar perfiles de usuario, vecinos cercanos, etc. Sin embargo me pareció interesante el algoritmo de _Rocchio_: creo que hoy en día con lo avanzadas que son las interfaces gráficas para el usuario, se podría sacar harto provecho a este.

Si bien el paper es largo, deja gusto a poco porque no se llega a un final concluyente. Es como un paseo por lo que hay hasta ahora (ese entonces) de los recomendadores basados en contenido.
