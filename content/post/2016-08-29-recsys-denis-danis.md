+++
categories = ["recsys"]
date = "2016-08-28T23:17:24-03:00"
description = ""
keywords = []
title = "Comentarios sobre Sources of Knowledge and Evaluation Metrics"

+++

El paper de Denis Parra (_me pregunto quién será él_) y Shaghayegh Sahebi es una especie de handbook para empezar a entender a los sistemas recomendadores.

El fin último de un sistema de este estilo es entregarle al usuario lo que necesita sin tener que preguntárselo.

Los tres principales tipos de sistemas recomendadores son:

### Basados en reglas

Por lo general las reglas son definidas a mano y requieren que el usuario personalmente (y parcialmente) diga qué le gusta.

Esto en el mejor caso, cuando el usuario se anima a entregar su información.

### Basado en contenido

Otra alternativa son los recomendadores en base al contenido de los ítems calificados.

Sin embargo estos pueden meter al usuario en una burbuja y siempre mostrarle lo mismo. Sin mencionar lo difícil que puede ser representar estos ítems a través de _features_.

### En base a la colaboración

Consisten en buscar usuarios "semejantes" para recomendar las cosas que a ellos les gustan.

Lamentablemente estos no suelen escalar, existe el problema de la esparcicidad de los datos y los ítems nuevos pueden pasar ignorados.

Como en toda área del conocimiento, existe la opción de mezclar estas ideas en un sistema de **recomendación híbrido**.

---

## Fuentes

Por otra parte, hay que considerar las fuentes de información.

Están los _ratings_, el _feedback implícito_, las _etiquetas_ y las _redes sociales_.

Un tema que quizás se habla poco es **la invasión a la privacidad** al momento de obtener feedback implícito. Todos aceptamos la _política de privacidad_ de las aplicaciones sin cuestionarlas, no obstante esto expone mucho a la gente y su libertad.

Serán buenas fuentes de información, pero a nadie le gusta que lo estén constantemente mirando en todo lo que hace. Además, **si el usuario común supiera esto transparentemente, quizás _"actuaría"_ distinto.**
