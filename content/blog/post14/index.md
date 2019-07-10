---
slug: funciones-curry-programacion-funcional-javascript
date: 2019-05-02
title: 'Programación Funcional. Funciones Curry'
description: 'Nombrado así en honor a Haskell Curry (y no a la comida). Curry es una técnica usada para transformar funciones een matemáticas y ciencias de la computación.'
published: true
author: 'Cristian Echeverría'
banner: './banner.jpeg'
---

Nombrado así en honor a Haskell Curry (y no a la comida). Curry es una técnica usada para transformar funciones en matemáticas y ciencias de la computación.

### Funciones Unarias

Primero miremos qué es una función unaria: Se refiere al número de argumentos que una función acepta. Una función unaria es la que acepta exactamente 1 argumento.

```javascript
// Función Unaria
const hello = word => console.log(`Hello ${word}`)

// Función Binaria
const greetings = (greeting, word) => console.log(`${greeting} ${word}!`)
```

### Currying

Currying es el proceso de transformar una función con múltiples argumentos en una High Order Function (visita mi artículo sobre [High Order Functions](https://cristianecheverria.com/high-order-functions-programacion-funcional-javascript)) que retorna una serie de funciones cada una aceptando únicamente 1 argumento (Unaria) y es evaluada una vez que recibe el último argumento.

```
f(n, m) --> f'(n)(m)
```

```javascript
// Función regular
function sum(a, b) {
  return a + b
}

// Currying
function sum(a) {
  return function(b) {
    return a + b
  }
}

// ES6
const sum = (a, b) => a + b
// Currying
const sum = a => b => a + b
```

Hagamos un par de console.logs

```javascript
function sum(a) {
  return function(b) {
    return a + b
  }
}

console.log(sum(3)) // [Function]
```

Como vemos, nos retorna como resultado una Función. Ahora, si cambiamos a lo siguiente:

```javascript
function sum(a) {
  return function(b) {
    return a + b
  }
}

const sumTwo = sum(2)
console.log(sumTwo(3)) // 5
console.log(sumTwo(5)) // 7
console.log(sumTwo(7)) // 9
```

Hemos transformado una función de múltiples argumentos en una serie de funciones unarias y el resultado se evalúa hasta que recibe el último argumento (`sum(2)` es el argumento _a_ y `sumTwo(3)` es el argumento _b_).

Miremos el mismo ejemplo con ES6:

```javascript
const sum = a => b => a + b

console.log(sum(2)(3)) // 5
console.log(sum(2)(5)) // 7
console.log(sum(2)(7)) // 9
```

### Ejemplos Prácticos del Uso de Currying

#### jQuery

Podemos ver un buen ejemplo de Currying en jQuery en la función "on":

```javascript
const curryOn = handler => event => $('button').on(event, handler)
const curryOnClick = handler => curryOn(handler)('click')
```

#### React

La simple función de react-redux-connect() es un ejemplo del uso de Curry:

```
export default connect(mapStateToProps)(TodoApp)
```

#### Función Curry como Higher Order Function

```javascript
// Curry como Higher Order Function
const curry = f => (...a) => (...b) => f(...a, ...b)
```

Ahora bien, imagina que tenemos un componente `<Button/>` que necesita navegar a `someURL` al darle clic.

La navegación se lleva a cabo llamando `history.push('/someURL')`, pero desafortunadamente, el objeto _history_ viene como una prop, y no está disponible de manera global en nuestra función `Navigate`. Así que podríamos escribir algo como esto:

```jsx
const Navigate = props => () => props.history.push('/someURL')

const Button = ({ ...props }) => <div onClick={Navigate(props)} />
```

Ó algo como esto:

```jsx
const Navigate = props => props.history.push('/someURL')

const Button = ({ ...props }) => <div onClick={() => Navigate(props)} />
```

Ambos casos implica definir una función anónima, la cuál sería difícil de testear.

Por lo tanto, curry es útil:

```jsx
const Navigate = props => props.history.push('/someURL')

const Button = ({ ...props }) => <div onClick={curry(Navigate)(props)} />
```

Le decimos a onClick que llame a la función curry con el componente Navigate como argumento, el cual retorna una función que toma props, y prosigue a navegar hacia `someURL`.

Fuente: [Joseph Chamochumbi](https://github.com/icyJoseph)

##### Renderizar HTML

```javascript
renderHtmlTag = tagName => content => `<${tagName}>${content}</${tagName}>`

renderDiv = renderHtmlTag('div')
renderH1 = renderHtmlTag('h1')

console.log(
  renderDiv('this is a really cool div'),
  renderH1('and this is an even cooler h1'),
)
```

Fuentes: [Frontend Weekly](https://medium.com/front-end-weekly/javascript-es6-curry-functions-with-practical-examples-6ba2ced003b1), [Bene:Studio](https://blog.benestudio.co/currying-in-javascript-es6-540d2ad09400)

### Conclusión

Las funciones Curry no son nuevas, el concepto viene de hace más de 40 años, el uso de arrow functions en Javascript nos ayuda mucho para la Programación Funcional. El Currying es un tema esencial en el camino de crecimiento en Javascript. React usa Curry casi por default, uno más de los motivos de su popularidad.
