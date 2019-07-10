---
slug: evitando-mutaciones-de-estructuras-de-datos-programacion-funcional-javascript
date: 2019-04-29
title: 'Programación Funcional. Evitando Mutaciones en Estructuras de Datos'
description: 'Sigo con mi serie de Programación Funcional. En esta ocasión es acerca de cómo evitar las Mutaciones en las estructuras de datos. Primero veremos las consecuencias de tener estructuras de datos mutables y cómo evitarlas.'
published: true
author: 'Cristian Echeverría'
banner: './banner.jpg'
---

Sigo con mi serie de Programación Funcional. En esta ocasión es acerca de cómo evitar las Mutaciones en las estructuras de datos. Primero veremos las consecuencias de tener estructuras de datos mutables y cómo evitarlas.

### Un poco acerca de Inmutabilidad en JavaScript

Cuando asignamos/creamos variables de tipos primitivos en Javascript (string, numero, booleanos) se asigna el valor a dicha variable, pero en el caso de objetos o arreglos se hace una referencia o apuntador en memoria. Acá es donde pueden originarse múltiples complicaciones respecto a la mutabilidad de las estructuras de datos. 

Es por eso que la Inmutabilidad es uno de los conceptos claves en la Programación Funcional, en la que mediante la Inmutabilidad se evitan *side effects*. Tomemos el caso del siguiente ejemplo:

```javascript
const array1 = ['a', 'b', 'c']
const array2 = array1

array2.push('d')
console.log(array1) // [ 'a', 'b', 'c', 'd' ]
```

En este caso, hemos creado el _array1_ y hemos hecho referencia del array1 al _array2_, cuando modificamos el _array2_ vía `array2.push('d')` no solamente modificamos el array2, sino que también modificamos el _array1_ y se puede ver claramente al imprimir en la consola el _array1_ el cual dá como resultado `[ 'a', 'b', 'c', 'd' ]`. 

Y ésto puede conducir a múltiples problemas ya que al modificar un arreglo o una función indirectamente estamos afectando otra parte de nuestra aplicación sin darnos cuenta, lo cual genera como resultado bugs difíciles de identificar, ya que debemos ir hasta la raíz del problema.

### Las mutaciones crean side effects

Un _side effect_ es cuando el código tiene un impacto fuera de su alcance (scope), recuerden que otro de los principios de la Programación Funcional son las funciones puras y las mutaciones evitan el principio de *Single Responsibility Rule*

Las mutaciones hacen imposible llevar un registro del _state_ de nuestra aplicación. Es por eso que en Redux siempre se busca evitar las mutaciones del state de la aplicación. 

> Las mutaciones son apuntadores en memoria. La Inmutabilidad es todos sobre valores, no apuntadores. Y es por eso que siempre se puede confiar en ella.

### Aplicar Inmutabilidad en Javascript

En los objetos y arreglos de javascript la Inmutabilidad no viene por default, pero podemos lograrlo. Uno de los métodos al que tenemos acceso es *Object.freeze*

```javascript
const hero = { name: 'Ethan', power: 'sleep everywhere' }
Object.freeze(hero)
hero.power = 'Eat 24/7'
console.log(hero) // { name: 'Ethan', power: 'sleep everywhere' }
```

### Spread Operator

Usando Spread Operator podemos crear nuevas instancias de objetos/arreglos. Es sin duda un método bastante cool ya que podemos ver claramente visible qué elementos son nuevos y cuáles son reusados.

```javascript
const heroCris = { power: 'sleep everywhere', name: 'Cristian' }
const heroEthan  = { ...heroCris, name: 'Ethan' }
console.log(heroEthan) // { power: 'sleep everywhere', name: 'Ethan' }

// arreglos
const frontend = ['HTML', 'CSS', 'JS']
const frontendReact = [...frontend, 'ReactJs']
console.log(frontendReact) // [ 'HTML', 'CSS', 'JS', 'ReactJs' ]
```
### Map y Filter

El uso de `map` en lugar de `forEach` asi como el uso de `filter` te ayudan a mantener la Inmutabilidad de tu codigo, esto debido a que cuando usas map/filter creas un arreglo nuevo, dejando el arreglo original intacto.

### Inmutable.js

También es posible usar otras estructuras de datos de terceros como [Inmutable.js](https://immutable-js.github.io/immutable-js/)

Primero, instala immutable usando npm.

```
npm install immutable
```

Luego requerimos la función que ocupamos een cualquier módulo.

```javascript
const { Map } = require('immutable');
const map1 = Map({ a: 1, b: 2, c: 3 });
const map2 = map1.set('b', 50);
map1.get('b') + " vs. " + map2.get('b'); // 2 vs. 50
```

Lo que recomiendan los expertos es usar Immutable.js o cualquier otra librería cuando tienes grandes estructuras de datos, de lo contrario usar el Spread Operator es lo óptimo.

Fuente: [Daily JS](https://medium.com/dailyjs/the-state-of-immutability-169d2cd11310)

### Conclusión

La inmutabilidad no viene por default en Javascript, sin embargo podemos usar librerias de Javascript, Spread Operator y librerías de tercero para lograr dicho objetivo. Todo depende del valor que tú encuentres a trabajar con Valores y no meramente con apuntadores en memoria. La inmutabilidad sin duda es una gran ayuda para mantener confianza del _state_ de nuestra aplicación en todo momento y para evitar bugs, veo los principios y reglas de Inmutabilidad en frameworks como Redux cuando trabajas con los Reducers, ahora me parece más claro dicho principio que antes.
