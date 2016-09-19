+++
categories = ["recsys"]
date = "2016-09-18T22:23:28-03:00"
description = ""
keywords = []
title = "Comentarios sobre Context-Aware"

+++

El _paper_ parte introducción un problema para ese entonces de los sistemas recomendadores: **se pierde el contexto y por ende, poder predictivo**. Posteriormente se hace un análisis sobre qué entendemos como _"contexto"_ y distintas formas de caracterizarlo. Este se presenta como un _"paseo"_ por las opciones que existen en esta área con casos reales e hipotéticos de su aplicación.

Dado el avance en tecnologías móviles, hoy en día el tema del contexto debe ser más relevante que para ese entonces. No solo es posible obtener más datos por culpa de la masificación de los celulares, sino que también mayor precisión.

Un tema que nunca trata es la _sparcity_ de los datos, en especial al momento de hablar de _Contextual modeling_. Este se diferencia de los otros paradigmas que solo consideran los datos contextuales al momento de filtrar antes y/o después de las recomendaciones obtenidas por metodologías ya presentes en la literatura. En el caso del _Contextual modeling_, se usa realmente los contextos como una dimensión más en el _dataset_.

También vale preguntarse qué tan separado es el tema del contexto fuera de lo que ya hemos visto en sistemas recomendadores. Pues podríamos representar el contexto como _features_, incluso dentro de _implicit feedback_ podría entrar esta área.
