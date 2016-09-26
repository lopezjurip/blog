+++
categories = ["recsys"]
date = "2016-09-25T22:39:03-03:00"
description = ""
keywords = []
title = "Comentarios sobre Implicit Feedback"

+++

El paper trata sobre filtrado colaborativo, pero en vez de usar el clásico sistema de _ratings_, se usa un sistema en base al _feedback_ que da el usuario de manera implícita. Esto es, obtener información del usuario del uso que le de al sistema o aplicación, como por ejemplo: el tiempo dedicado a escuchar una canción.

Se le da gran relevancia al rendimiento en términos de complejidad (lo que es bueno), particularmente al usar _factorizaciones matriciales_ en vez de _vecinos cercanos_.

Sobre el experimento que realizan, me parece extraño que filtren las películas que los usuarios vieron hasta su 50% o menos. Luego se habla de _la incapacidad de obtener feedback negativo_. En este sentido, el hecho de dejar un capítulo a medias por desinterés **se podría considerar como una penalización a los similares al momento de recomendar.**

En un ámbito similar, el _paper_ **_Signals in the Silence: Models of Implicit Feedback in a Recommendation System for Crowdsourcing_ _(2014)_** de _Christopher H. Lin_, _Ece Kamar_ y _Eric Horvitz_; que es posterior al examinado en este _post_, considera **la exposición de un ítem** al momento de obtener _feedback negativo_. Pues un elemento visible e ignorado nos da intuición e los intereses del usuario.
