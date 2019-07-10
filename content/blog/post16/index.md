---
slug: immutablejs-programacion-funcional-javascript
date: 2019-05-20
title: 'Programación Funcional. ImmutableJS'
description: 'La mutabilidad es uno de los serios problemas que Javascript posee en relacion con otros lenguajes de programación. Para poder "tapar" dicho problema se han creado varias librerías que nos ayudan a evitar la mutabilidad. Immutable es sin duda una de ellas y de bastante uso en la comunidad. Creada por los ingenieros en Facebook, veremos sus ventajas, desventajas y demás.'
published: false
author: 'Cristian Echeverría'
banner: './banner.png'
---

La mutabilidad es uno de los serios problemas que Javascript posee en relacion con otros lenguajes de programación. Para poder "tapar" dicho problema se han creado varias librerías que nos ayudan a evitar la mutabilidad. [Immutable](https://immutable-js.github.io/immutable-js/) es sin duda una de ellas y de bastante uso en la comunidad. Creada por los ingenieros en Facebook, veremos sus ventajas, desventajas y demás.

Si has seguido mi serie de Programación Funcional, sabrás que la [Mutabilidad de Datos](https://cristianecheverria.com/evitando-mutaciones-de-estructuras-de-datos-programacion-funcional-javascript) es un tema a tomar en cuenta cuando trabajas con grandes aplicaciones de Javascript. En dicho artículo menciono cómo evitar la Mutabilidad en Javascript usando funcionalidades propias de ES6. Sin embargo, para aplicaciones que manejan grandes estructuras de datos, los expertos recomiendan usar librerías especializadas para evitar la mutabilidad y es por eso que hablaré un poco acerca de [Immutable](https://immutable-js.github.io/immutable-js/) y su funcionamiento, así como sus desventajas.

### Introducción

Convertir estructuras de datos comunes de Javascript en Inmutables se puede lograr vía Immutable de la siguiente manera:

- Objetos `{}` usan **Map** `Map({})`
- Arreglos `[]` usan **List** `List([])`
- También podemos usar `fromJS`

```js
import { Map, List, fromJS } from 'immutable'

// Javascript Normal
const person = {
  name: 'Will',
  pets: ['cat', 'dog'],
}

// El equivalente en Immutable:
const immutablePerson = Map({
  name: 'Will',
  pets: List(['cat', 'dog']),
})

// Ó ...
const immutablePerson = fromJS(person)
```

**fromJS** es una función que convierte data anidada en Inmutable. En el proceso de conversión crea **Maps** y **Lists**.

#### Diferencia entre Map() y fromJS()

Partiendo de la premisa anterior, miremos la diferencia entre `Map` y `fromJS`:

```js
let a = {
  address: {
    postcode: 5085,
  },
}
let d = Immutable.Map(a)
```

En este ejemplo, la el siguiente objeto es inmutable (_veremos luego, que `get` es la forma en la que obtenemos valor con Immutable_):

```js
d.get('address')
```

Su valor no puede cambiar a otro objeto. Solamente podemos crear un Nuevo objeto usando la función para ello con `Immutable.Map.set()` de ImmutableJS (_también veremos la funcionalidad de `set`_).

Sin embargo, el objeto referenciado por **d.get('address')** en este caso, **{postcode:5085}** es un objeto estándar de JavaScript. **Y es mutable.** Un código como el siguiente, puede cambiar el valor de **postcode**:

```js
d.get('address').postcode = 6000
```

Si verificamos el valor de **d** una vez más, veremos que su valor ha cambiado.

```js
console.log(JSON.stringify(d))
// imprime {"address":{"postcode":6000}}
```

El motivo es que en ImmutableJS las estructuras de datos como `List` y `Map` aplican la funcionalidad de inmutabilidad a solamente miembros del nivel-1 de anidación con List/Map.

Por lo tanto, si tu tienes objetos dentro de arreglos ó arreglos dentro de objetos y deseas que también sean inmutables, tu opción es usar `Immutable.fromJS`

```js
let a = { address: { postcode: 5085 } }
let b = Immutable.fromJS(a)
b.get('address').postcode = 6000
console.log(JSON.stringify(b))
//Outputs {"address":{"postcode":5085}}
```

### Convertir de regreso de Inmutable a JavaScript normal

Es muy sencillo convertir tus datos de inmutable a JavaScript normal. Solamente debemos llamar el método **.toJS()** en nuestro objeto Inmutable.

```js
import { Map } from 'immutable'

const immutablePerson = Map({ name: 'Will' })
const person = immutablePerson.toJS()

console.log(person)
// imprime { name: 'Will' };
```

### Motivos para empezar a usar Immutable

Veremos a continuación un listado de razones muy buenas por las cuales se recomienda usar Immutable en tus proyectos

**1. Obtener un valor anidado de un objeto sin verificar que exista**

En JavaScript normal:

```js
const data = { my: { nested: { name: 'Will' } } }
const goodName = data.my.nested.name
console.log(goodName)
// imprime Will, como esperábamos

const badName = data.my.lovely.name
// imprime error: 'Cannot read name of undefined...'
// imprime un enorme mensaje de error en consola
```

Y ahora con Immutable:

```js
const data = fromJS({ my: { nested: { name: 'Will' } } })
const goodName = data.getIn(['my', 'nested', 'name'])
console.log(goodName)
// imprime Will

const badName = data.getIn(['my', 'lovely', 'name'])
console.log(badName)
// imprime undefined
// No muestra un gran mensaje de error, solamente undefined
```

No necesitas revisar por valores no definidos a lo largo de toda la cadena de la estructura de datos como se haría en JavaScript normal:

```js
if (data && data.my && data.my.nested && data.my.nested.name) { ...
```

**2. Encadenar manipulaciones**

Primero en JavaScript normal:

```js
const pets = ['cat', 'dog']
pets.push('goldfish')
pets.push('tortoise')
console.log(pets)
// imprime ['cat', 'dog', 'goldfish', 'tortoise'];
```

Ahora en Immutable:

```js
const pets = List(['cat', 'dog'])
const finalPets = pets.push('goldfish').push('tortoise')
console.log(pets.toJS())
// imprime ['cat', 'dog'];
console.log(finalPets.toJS())
// imprime ['cat', 'dog', 'goldfish', 'tortoise'];
```

**3. Immutable data**

Se llama Immutable después de todo, así que ése es su principal motivo.

Supongamos que creamos un objeto inicial y lo volvemos Inmutable, luego asignamos el objeto inicial a un segundo objeto y lo modificamos.

```js
const data = fromJS({ name: 'Will' })
const newNameData = data.set('name', 'Susie')
// Usando set() es cómo mutamos valores

console.log(data.get('name'))
// imprime 'Will'
// El objeto inicial no cambió

console.log(newNameData.get('name'))
// imprime 'Susie'
// El nuevoObjeto sí que cambió
```

### Algunos problemas al usar Immutable.JS

Pues no todo es color de rosa respecto a ImmutableJS, a continuación veremos algunos problemas y por ende algunas recomendaciones de cuándo usar dicha librería.

Fuentes:

- [Immutable.js is intimidating. Here’s how to get started.](https://medium.freecodecamp.org/immutable-js-is-intimidating-heres-how-to-get-started-2db1770466d6)
- [What is the difference between ImmutableJS Map() and fromJS()?](https://stackoverflow.com/questions/33312922/what-is-the-difference-between-immutablejs-map-and-fromjs)
