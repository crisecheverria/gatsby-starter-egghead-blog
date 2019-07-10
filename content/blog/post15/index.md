---
slug: funciones-puras-programacion-funcional-javascript
date: 2019-05-17
title: 'Programación Funcional. Funciones Puras'
description: 'Las Funciones Puras son otro elemento clave en el paradigma de la Programación Funcional, sobre todo debido a que las Funciones Puras son aquellas en las que no existen "side-effects".'
published: true
author: 'Cristian Echeverría'
banner: './banner.jpg'
---

Las Funciones Puras son otro elemento clave en el paradigma de la Programación Funcional, sobre todo debido a que las Funciones Puras son aquellas en las que no existen 'side-effects'.

### Funciones Puras

> Una Función Pura es una función en la cual, dado el mismo input, siempre retornará el mismo output y que no posee ningún [side-effect](https://cristianecheverria.com/evitando-mutaciones-de-estructuras-de-datos-programacion-funcional-javascript).

```js
// función pura
function add(a, b) {
  return a + b
}
```

Esta función es pura. No depende de ningún cambio fuera de su propio state (data), ningún elemento externo le afecta y no importa cuantas veces le pases los mismos argumentos siempre retornara la misma respuesta.

```js
// función impura
let minimum = 21
function checkAge(age) {
  return age >= minimum
}
```

Esta es una función impura, ya que su resultado depende de un elemento externo, la variable `minimum`. Si `minimum` cambia de valor, estamos fritos!.

```js
// función pura
function checkAge(age) {
  let minimum = 21
  return age >= minimum
}
```

Si movemos la variable `minimum` dentro de la misma función la convertimos en una Función Pura.

```js
// función impura
function printAge(age) {
  console.log(age)
}
```

Otro ejemplo de una función impura, debido a que usa `console.log()` el cual es un elemento externo de la función y que no fué recibido como parámetro.

Las Funciones Puras no tienen "side-effects", algunos puntos a considerar:

- Acceso al state (data) del sistema fuera de la función
- Mutación de objetos pasados como argumentos
- Hacer llamados HTTP
- Obtener el ingreso de datos del usuario

Todos ellos son considerados side-effects.

### Mutaciones Controladas

Debes estar al tanto de que existen [Métodos que Mutan Arreglos y Objetos en Javascript](https://developer.mozilla.org/es/docs/Web/JavaScript/Referencia/Objetos_globales/Array/prototype#Mutator_methods), los cuales cambiaran los objetos, un ejemplo es la comparación entre `splice` y `slice`.

```js
// función impura, arr será modificado(mutado) y nunca más será el mismo
function firstThree(arr) {
  return arr.splice(0, 3)
}

// función pura, slice retorna un nuevo arreglo
function firstThree(arr) {
  return arr.slice(0, 3)
}
```

### Beneficios de las Funciones Puras

- Más faciles de testear ya que poseen una sola responsabilidad misma entrada > misma salida.
- Resultado es cacheable ya que con la misma entrada siempre retorna la misma salida.
- Autodocumentadas ya que sus dependencias son explícitas.
- No tienes que preocuparte por side-effects.

### Funciones Esenciales

Todos los desarrolladores debemos hacer un esfuerzo para aprender las funciones fundamentales que nos ayudan a abstraer patrones comunes de la programación hacia un código más declarativo que imperativo.

Aquí una lista esencial de dichas funciones que todo desarrollador de JavaScript debería manejarlas.

Arreglos

- [forEach](https://developer.mozilla.org/es/docs/Web/JavaScript/Referencia/Objetos_globales/Array/forEach)
- [map](https://developer.mozilla.org/es/docs/Web/JavaScript/Referencia/Objetos_globales/Array/map)
- [filter](https://developer.mozilla.org/es/docs/Web/JavaScript/Referencia/Objetos_globales/Array/filter)
- [reduce](https://developer.mozilla.org/es/docs/Web/JavaScript/Referencia/Objetos_globales/Array/reduce) (ésta me hace falta sin dudas)

Funciones

- debounce
- compose
- partial
- [curry](https://cristianecheverria.com/funciones-curry-programacion-funcional-javascript)

### Menos es Más

Miremos un ejemplo de algunas mejoras que podemos hacer a nuestro código implementando Programación Funcional

```js
// código inicial
let items = ['a', 'b', 'c']
let upperCaseItems = () => {
  let arr = []
  for (let i = 0, ii = items.length; i < ii; i++) {
    let item = items[i]
    arr.push(item.toUpperCase())
  }
  items = arr
}

// pura
let upperCaseItems = items => {
  let arr = []
  for (let i = 0, ii = items.length; i < ii; i++) {
    let item = items[i]
    arr.push(item.toUpperCase())
  }
  return arr
}

// forEach
let upperCaseItems = items => {
  let arr = []
  items.forEach(item => {
    arr.push(item.toUpperCase())
  })

  return arr
}

// map
let upperCaseItems = items => items.map(item => item.upperCase())

// reducir las funcioens a su forma simple
let upperCase = item => item.toUpperCase()
let upperCaseItems = items => items.map(upperCase)
```

Fuente: [An Introduction to Reasonably Pure Functional Programming](https://www.sitepoint.com/an-introduction-to-reasonably-pure-functional-programming/)
