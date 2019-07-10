---
slug: pipe-programacion-funcional-javascript
date: 2019-05-29
title: 'Programación Funcional. Pipe'
description: 'El concepto de pipe es simple - combina "n" cantidad de funciones. Es un tubo que fluye de izquierda a derecha, llamando cada una de las funciones con el resultado de la anterior.'
published: true
author: 'Cristian Echeverría'
banner: './banner.jpg'
---

El concepto de pipe es simple - combina n cantidad de funciones. Es un tubo que fluye de izquierda a derecha, llamando cada una de las funciones con el resultado de la anterior.

Vamos a escribir una función que retorna el nombre de una persona usando el objeto `person`:

```js
getName = (person) => person.name
console.log(getName({ name: 'Cristian'}))
// 'Cristian'
```

Ahora vamos a escribir una función que convierte Strings a Mayúsculas:

```js
upperCase = string => string.toUpperCase()
console.log(upperCase('Cristian'))
// 'CRISTIAN'
```

Si queremos convertir el nombre del objeto `person`, podríamos hacer lo siguiente:

```js
upperCase(getName({name: 'Cristian'}))
// 'CRISTIAN'
```

Ahora, vamos a crear una función para revertir un String:

```js
reverse = string => string.split('').reverse().join('')
reverse('Cristian')
// 'naitsirC'
```

En ese caso, ahora tendríamos algo como esto:

```js
reverse(upperCase(getName({name: 'Cristian'})))
// 'NAITSIRC'
```

Y podríamos seguir y seguir y sería un gran tamal!!

### Pipe

En lugar de estar juntando funciones una encima de otra, metamoslas en un solo tubo!

```js
pipe(getName, upperCase, reverse)({name: 'Cristian'})
```

Acá podríamos usar [ramdajs](https://ramdajs.com/docs/#pipe) para hacer uso de la función `pipe`. Pero en este caso no ocupamos importar toda una librería a nuestro poryecto y en su lugar usamos la siguiente función:

```js
pipe = (...funcs) => (...args) => funcs.reduce((acc, func, i) => (i === 0 ? func(...acc) : func(acc)), args)
```

Usando _rest_ parameters podemos hacer pipe a `n` cantidad de funciones. Cada función toma el resultado de la función anterior y todo es `reducido` a un solo valor.

Así todo lo que hicimos quedaría como:

```js
pipe(getName, upperCase, reverse)({name: 'Cristian'})
// 'NAITSIRC'
```

Fuente: [freeCodeCamp](https://www.freecodecamp.org/news/pipe-and-compose-in-javascript-5b04004ac937/)