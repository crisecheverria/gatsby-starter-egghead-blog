---
slug: high-order-functions-programacion-funcional-javascript
date: 2019-04-27
title: 'Programación Funcional con JavaScript. High Order Functions'
description: 'Este es el inicio de una serie de artículos referentes a la Programación Funcional con JavaScript. En esta ocasión escribo un poco de High Order Function. Su definición y algunos ejemplos prácticos. Que empiece el show!'
published: true
author: 'Cristian Echeverría'
banner: './banner.jpeg'
---

Este es el inicio de una serie de artículos referentes a la Programación Funcional con JavaScript. En esta ocasión escribo un poco de High Order Function. Su definición y algunos ejemplos prácticos. Que empiece el show!

Llevo 1 semana exacta desde que inicié una nueva aventura laboral dentro de la misma empresa. En este caso un cliente de renombre, Volvo. Estoy colaborando como Frontend Developer para la empresa dentro de Volvo que se encarga de las Soluciones de Software para sus clientes.

Platicando con uno de sus Frontend Architect me mencionaba el uso de la Programación Funcional. Así que decidí repasar algunos conceptos muy importantes en dicho estilo de programación y para practicar qué mejor que ir documentando mi aprendizaje y experiencia en una serie de artículos relacionadas a este tema.

La idea es publicar al menos 1 artículo semanal (posiblemente más de uno), para así poder recurrir a estas publicaciones cuando me sea necesario, me sirve como mi biblioteca personal.

Aclaración: La mayoría de los ejemplos que pretendo usar provienen de otras fuentes y por ende al final daré los créditos respectivos.

### Programación Funcional

La Programación Funcional es un paradigma, una forma de pensar y estructurar tu código. Es una alternativa a la Programación Orientada a Objetos. Generalmente se utiliza para optimizar la reusabilidad del código, fácil lectura y comprensión y fácil de testear. Las herramientas utilizadas son funciones, formas de componerlas a favor del comportamiento con modelo complejos, evitar mutabilidad del _state_ así como _side effects_.

#### Algunos usos de la Programación Funcional

La programación funcional (PF) se está volviendo una tendencia sobre todo en el siempre cambiante ecosistema de Javascript y el aumento en su adopción es clara. Muchas de las nuevas librerías y frameworks de moda están fuertemente influenciados por la Programación Funcional:

- React - Con sus componentes reusables y funciones puras.
- Redux - Por la manera en la que te fuerza a evitar mutaciones del _state_.
- Reason - Por su alcance funcional y su inmutabilidad.
- Elm - Un lenguaje funcional que compila a Javascript
- Underscore.js, lodash, Ramda - Todas librerías que tienen PF dentro.

Hay mucha oportunidad de que tú ya hayas usado estas herramientas y sino sin duda alguna pronto las usarás.

Fuente: [DailyJS](https://medium.com/dailyjs/functional-js-1-introduction-7908bfe5ef8d)

> Así que siéntate bien, vé por tu taza de café y bienvenido a mi viaje acerca del aprendizaje de Programación Funcional mediante Javascript.

### High Order Functions

Se definen como aquellas funciones que cumplen al menos 1 de los siguientes puntos:

1. Aceptan una función como argumento.
2. Retornan una nueva función.

Miremos un ejemplo:

```javascript
const withCount = fn => {
  let count = 0

  return (...args) => {
    console.log(`Call count: ${++count}`)
    return fn(...args)
  }

  /* (...args) Funcionalidad de ES6 conocida como Spread Operator, 
    así pasamos a la función retornada cuales sean 
    los parámetros definidos. */
}
```

Hemos definido una función _withCount_ la cual recibe como parámetro otra función (que aún no hemos definido). Dicha funcuón a su vez retorna una segunda función (la que recibió como argumento), imprimiendo un contador en consola junto con el resultado de la función.

Seguido de eso, definimos la función que pasaremos como argumento de la High Order Function definida al inicio.

```javascript
const add = (x, y) => x + y
```

Asignamos el resultado del llamado a la función `withCount` pasándole la función `add`, el resultado lo almacenamos dentro de una constante:

```javascript
const countedAdd = withCount(add)
```

El código final es el siguiente:

```javascript
const withCount = fn => {
  let count = 0

  return (...args) => {
    console.log(`Call count: ${++count}`)
    return fn(...args)
  }
}

const add = (x, y) => x + y
const countedAdd = withCount(add)

console.log(countedAdd(1, 2))
console.log(countedAdd(2, 2))
console.log(countedAdd(3, 2))

/* Imprime:
Call count: 1
3
Call count: 2
4
Call count: 3
5
*/
```

Fuente: [Just Enough Functional Programming in Javascript](https://egghead.io/lessons/javascript-modify-functions-with-higher-order-functions-in-javascript)

### Ejemplo 2. Crear otra función

En el siguiente caso veremos otro ejemplo, en este caso uno mas sencillo que el anterior, sin embargo, decidí abrir el artículo con el anterior ya que es más descriptivo.

En este ejemplo veremos el caso de cómo una función crea una nueva función.

```javascript
const greaterThan = n => m => m > n

let greaterThan10 = greaterThan(10)

console.log(greaterThan10(11)) // true
console.log(greaterThan10(10)) // false
```

Declaramos la función _greaterThan_ quien es la que realiza el análisis, en este caso una comparación entre 2 valores. Al momento de asignar dicha función a la variable _greateThan10_ estamos creando la segunda función.

Fuente: [eloquentjavascript.net](https://eloquentjavascript.net/05_higher_order.html)

Ahora miremos otro ejemplo:

```js
function add(x, y) {
  return x + y
}

// High Order Function.
// addReference es la función add(x, y)
function makeAdder(x, addReference) {
  return function(y) {
    return addReference(x, y)
  }
}

// Creamos una serie de funciones
// Cada una llama a la High Order Function
// Pasando diferentes parámetros y la función add
const addFive = makeAdder(5, add)
const addTen = makeAdder(10, add)
const addTwenty = makeAdder(20, add)

// Estos parámetros son el parámetro "y" de la HOC
addFive(10) // 15
addTen(10) // 20
addTwenty(10) // 30

/* 
Higher-Order Function:
     1- Es una función
     2- Toma como argumento una Callback Function
     3- Retorna una nueva función
     4- La función retornada puede invocar a la función que tomó como argumento
*/

function higherOrderFunction(callback) {
  return function() {
    return callback()
  }
}
```

Fuente: [TylerMcginnis.com](https://tylermcginnis.com/react-higher-order-components/)

### Ejemplo 3. Modificar funciones

Seguimos con un ejemplo de funciones que modifican otras funciones.

```javascript
const noisy = fn => {
  return (...args) => {
    console.log(`Calling with ${args}`)
    let result = fn(...args)
    console.log(`Called with ${args}, returned ${result}`)
    return result
  }
}

console.log(noisy(Math.min)(3, 2, 1))

/*
respuesta:
Calling with 3,2,1
Called with 3,2,1, returned 1
1
*/
```

A la función _noisy_ le pasamos una función de la librería Math de Javascript seguido de los parámetros para dicha función.

Fuente: [eloquentjavascript.net](https://eloquentjavascript.net/05_higher_order.html)

### Ejemplo 4. Filtrado de Arreglos.

Supongamos tenemos el siguiente arreglo de objetos

```javascript
const users = [
  {
    id: 1,
    name: 'Cris',
    age: '33',
    city: 'Gothenburg',
  },
  {
    id: 2,
    name: 'John',
    age: '28',
    city: 'Raleigh',
  },
  {
    id: 3,
    name: 'Dude',
    age: '28',
    city: 'San Diego',
  },
]
```

Para este caso filtraremos _users_ en base a la edad

```javascript
const filter = (array, fn) => {
  let passed = []
  for (let iterator of array) {
    if (fn(iterator)) {
      passed.push(iterator)
    }
  }

  return passed
}

console.log(filter(users, user => user.age > 28))
```

Si se cumple la estructura condicional if (la cual es la funcion que se envía como segundo argumento `user => user.age > 28`) entonces se registra el _user_ al arreglo _passed_

Fuente: [eloquentjavascript.net](https://eloquentjavascript.net/05_higher_order.html)

### Ejemplo 5: High Order Components con React

El siguiente ejemplo, como el resto de ejemplos de mi serie de Programación Funcional los he tomado prestados de diversos sitios, y éste ha sido uno de los ejemplos más claros que he visto sobre High Order Components en React.

Pueden encontrarlo en el siguiente link [TylerMcginnis.com](https://tylermcginnis.com/react-higher-order-components/)

### Conclusión

JavaScript sin duda es bastante flexible, podemos declarar variables con valores, con funciones, podemos declarar funciones como propiedades de objetos y más mucho más. Este es solamente el primer artículo sobre Functional Programming.
