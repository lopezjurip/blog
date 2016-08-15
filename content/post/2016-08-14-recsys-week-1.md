+++
categories = ["recsys"]
date = "2016-08-14T09:24:52-03:00"
description = ""
keywords = []
title = "Comentarios sobre cómo Hacker News ordena las noticias"

+++

Todos los días en la mañana, en el transporte público y/o en momento de procrastinación leo [Hacker News](https://news.ycombinator.com/) para mantenerme al día.

{{% figure src="/images/hackernews-1.png"%}}

En mi ramo de [Sistemas Recomendadores](http://dparra.sitios.ing.uc.cl/classes/recsys-2016-2/) me topé con este artículo que habla sobre el algoritmo que hay por detrás de su sección principal:

**[https://medium.com/hacking-and-gonzo/how-hacker-news-ranking-algorithm-works-1d9b0cf2c08d](https://medium.com/hacking-and-gonzo/how-hacker-news-ranking-algorithm-works-1d9b0cf2c08d)**

> Se puede ver la implementación del algoritmo en esa página.

Resulta que este se aplica para cada item en particular sin importar el contexto o relación con otros. Para que se mantenga la lista viva y renovándose, involucra un decaimiento por tiempo, con un parámetro `gravedad`.

El beneficio de esto es que permite gran escalabilidad, e incluso se puede realizar el ordenamiento en el cliente por medio de _Javascript_.

Implementación (versión simple) en _Javascript ES7+_:

```js
// npm install moment lodash
import moment from 'moment';
import orderBy from 'lodash/orderBy';

// Data
const news = [{
  title: '#1'
  upvotes: 220,
  creation: moment().subtract(2, 'hours'),
}, {
  title: '#2'
  upvotes: 80,
  creation: moment().subtract(1, 'hours'),
}];

function hackernews(gravity = 1.8) {
  return function({ upvotes, creation }) {
    const hours = moment().diff(moment(creation), 'hours');
    return (upvotes - 1) / Math.pow(hours + 2, gravity);
  }
}

const rater = hackernews();
const rated = news.map(n => ({ ...n, score: rater(...n) }));
const sorted = orderBy(rated, 'score', 'desc');

console.log(sorted);
```

## Mejoras

Aunque todavía es bastante discutible si el algoritmo funciona bien, pues el tiempo es gravemente penalizado:

> Mirar imagen de arriba y ver que la noticia en segundo lugar tiene más puntos que la primera en la lista en un intervalo de tiempo similar (quizás con diferencia de minutos).

Un factor importante que se ignora, y que es el punto al que voy, son los comentarios dentro de cada item. Esto es una sutileza (como las que aparecen [acá](http://www.evanmiller.org/how-not-to-sort-by-average-rating.html)), pero detalles así alteran la percepción final del usuario.

El problema al que me refiero se puede apreciar en la siguiente imagen:

{{% figure src="/images/hackernews-2.png"%}}

* El primero en la lista tiene solo 5 comentarios.
* El segundo en la lista, a pesar de tener menos puntos, **en menos tiempo lleva más de 5 veces más comentarios que el primero.**
* También es bien dudosa la diferencia entre el 4 y el 7.

Una noticia que causa grandes discuciones debería tener un mejor *rating*.

### ¿Cómo incluir los comentarios en el algoritmo?

Cabe mencionar que hace unos meses se agregó la _feature_ que permite darle _upvote_ a los comentarios.

Se me ocurren las siguientes maneras:

#### 1) Por novedad

Podríamos asignar una puntuación adicional al algoritmo con respecto **a la novedad último comentario de la noticia**. Incluso reutilizar el algoritmo original. Debería ser necesario agregar un factor de peso al puntaje por novedad del comentario. A modo de ejemplo elegiré `0.3` como valor arbitrario:

```js
function hackernews(gravity = 1.8, factor = 0.3) {
  // lastComment should be an object with 'upvotes' and 'creation' keys
  return function algorithm({ upvotes, creation, lastComment }) {
    const hours = moment().diff(moment(creation), 'hours');
    const score = (upvotes - 1) / Math.pow(hours + 2, gravity);

    if (lastComment) {
      return score + (factor * algorithm({ ...lastComment })); // recursive call
    } else {
      return score;
    }
  }
}
```

**Problemas:** Puede que la gente abuse con el clásico **_bump_** con fin de mantener el _thread_ arriba. Incluso una noticia con pocos comentarios puede salir a flote como consecuencia de un nuevo comentario.

#### 2) Por cantidad de comentarios

Similar a lo anterior. Con un peso que añada cierto valor adicional al _score_. Sería relativamente simple de implementar y no sacrifica tanto la escalabilidad (ver [eager-loading](http://www.itorian.com/2013/06/what-is-eager-loading-and-what-is-lazy.html) o [counter-cache](http://ryan.mcgeary.org/2016/02/05/proper-counter-cache-migrations-in-rails/)).

```js
function hackernews(gravity = 1.8, factor = 0.3) {
  return function algorithm({ upvotes, creation, comments }) {
    const hours = moment().diff(moment(creation), 'hours');
    const score = (upvotes - 1) / Math.pow(hours + 2, gravity);
    return score + (factor * comments.length);
  }
}
```

**Problemas**: En el peor caso, un _bot_ podría abusar y _spammear_ 10000 comentarios por segundo con el fin de posicionar una noticia.

#### 3) Por distribución de comentarios en el tiempo

Una medida que considere cómo ocurren los comentarios dentro de un intervalo de tiempo, _premiando_ a las noticias que mantengan una _conversación_ constante puede ser un mejor indicador de una buena noticia. También dificultaría un poco a los _bots_ que quieran sacar provecho.

**Problema:** Quizás ponga en juego la escalabilidad y requeriría buscar alguna distribución que se ajuste bien.

## Conclusión

Probablemente una opción como la _2)_ sería lo más adecuado para implementar, con el fin de mantener la simpleza y la escalabilidad.

Finalmente, **me he cuestionado si realmente es una buena idea incluir comentarios a los _ratings_ por lo fácil que puede ser para _bots_ manipular los puntajes.** Esto pues una misma cuenta puede publicar cuantos comentarios quiera en un mismo item, en comparación con asignar un rating que solo se puede hacer una sola vez por item.
