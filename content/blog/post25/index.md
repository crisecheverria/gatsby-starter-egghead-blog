---
slug: react-redux-tutorial-parte-3
date: 2019-09-01
title: 'React Redux - Parte 3: Formularios con Redux'
description: 'Este Post es la Parte 3 del curso de React Redux. En esta ocasión veremos cómo crear un formulario para poder agregar datos usando Redux. Veremos cómo se crean Formularios Controlados con React.'
published: true
author: 'Cristian Echeverría'
banner: './banner.jpg'
---

Este Post es la Parte 3 del curso de React Redux. En esta ocasión veremos cómo crear un formulario para poder agregar datos usando Redux. Veremos cómo se crean Formularios Controlados con React.

Si deseas seguir las publicaciones anteriores sigue los siguientes links:

- [Parte1: Redux](https://cristianecheverria.com/react-redux-tutorial-parte-1)
- [Parte2: React](https://cristianecheverria.com/react-redux-tutorial-parte-2)

### Código Fuente

Si deseas ver el código fuente descarga el [repositorio de GitHub](https://github.com/crisecheverria/react-redux-tutorial)

### Instalar React Redux

```js
yarn add react-redux
```

Una vez instalado modificamos nuestro **./src/index.js**. Importamos el componente `<Provider>` de react-redux para poder insertar el State de nuestra **store** a nuestra aplicación.

```jsx
// /src/index.js
import React from 'react'
import ReactDOM from 'react-dom'
// Importamos Provider de React Redux
import { Provider } from 'react-redux'
import './index.css'
import App from './App'
import * as serviceWorker from './serviceWorker'

import store from './store'

const addExpense = val => store.dispatch({ type: 'ADD_EXPENSE', payload: val })

ReactDOM.render(
  <Provider store={store}>
    <App addExpense={addExpense} />
  </Provider>,
  document.getElementById('root'),
)

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: https://bit.ly/CRA-PWA
serviceWorker.unregister()
```

Modificamos el código anterior donde teníamos la función **render**, inyectábamos el **state** del store a nuestra aplicación y además **suscribíamos** nuestra aplicación para que **escuchara** cualquier cambio que se llevara a cabo en nuestra store.

Véase como inyectamos nuestra store a al componente `<Provider>` y además ya no pasamos el state como prop al componente `<App>`, en su lugar conectaremos el componente `<App>` con nuestra Store directamente y cualquier otro componente que necesite obtener datos de nuestra Store:

```jsx
ReactDOM.render(
  <Provider store={store}>
    <App addExpense={addExpense} />
  </Provider>,
  document.getElementById('root'),
)
```

El poder conectar cualquier componente que deseamos con el **Store** de nuestra aplicación que es donde reside el State del proyecto, es el poder de Redux. El poder tener el State en un solo lugar y poder al mismo tiempo **viajar en el tiempo** usando Redux DevTools y así poder conocer en qué punto el State del proyecto es actualizado.

### Conectarnos al State de la Store

Ahora veremos cómo conectar nuestro componente `<App>` con el State de nuestra Store de Redux.

```jsx
import React from 'react'
// Importamos el método connect de React Redux
import { connect } from 'react-redux'
import './App.css'

import 'bulma/css/bulma.min.css'

function App(props) {
  ...
}

// Modificamos el export default
const mapStateToProps = state => state
export default connect(mapStateToProps)(App)
```

Como podemos ver hemos importado **connect**. Connect nos permite poder conectar cualquier componente con nuestra Store, y así poder **recibir el State y poder ejecutar acciones**. Connect en Redux es un ejemplo del paradigma de la programación conocida como [Programación Funcional mediante funciones Curry](https://cristianecheverria.com/funciones-curry-programacion-funcional-javascript), es por eso que lleva la siguiente estructura `connect()(Componente)`.

En este caso a connect, le hemos pasado una función conocida como **mapStateToProps**, la cual recibe como parámetro el state de nuestra Store. Podríamos devolver TODO el state o solamente UNA parte del State, en este caso devolvemos/inyectamos todo el state.

```jsx
// Modificamos el export default
const mapStateToProps = state => state
export default connect(mapStateToProps)(App)
```

Al hacer el paso anterior pasando la función **mapStateToProps** a connect, tenemos acceso como vía props al state, es por eso que si corremos nuestra aplicación en el navegador deberíamos poder ver el listado inicial de Expenses que hemos definido.

![App Props](app-connect.png)

### Ejecutar Acciones

El siguiente paso será ejecutar acciones mediante react-redux. Para ello moveremos la action creator que hemos definido en nuestro index.js

```jsx
import React from 'react'
import ReactDOM from 'react-dom'
import { Provider } from 'react-redux'
import './index.css'
import App from './App'
import * as serviceWorker from './serviceWorker'

import store from './store'

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root'),
)

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: https://bit.ly/CRA-PWA
serviceWorker.unregister()
```

Como se puede observar hemos eliminado por completo la función **addExpense** y ya no la pasamos como prop al componente App.js

Seguido de eso, nos movemos hacia App.js y creamos una nueva función la cual será el segundo parámetro que recibe la función **connect** de react-redux conocida como **mapDispatchToProps**

```jsx
// ./src/App.js
import React from 'react'
import { connect } from 'react-redux'
import './App.css'

import 'bulma/css/bulma.min.css'

function App(props) {
  // Nuevo prop la función 'handleAddExpense'
  const { expensesItems, handleAddExpense } = props

  return (
    <div className="container">
      <button className="button is-primary" onClick={handleAddExpense}>
        Add Expense
      </button>
      {/* OJO: He omitido el Body de la Tabla */}
      <table className="table">...</table>
    </div>
  )
}

const mapStateToProps = state => state
// Función MapDispatchToProps
// Devuelve el nuevo prop 'handleAddExpense'
const mapDispatchToProps = dispatch => {
  const newExpense = {
    id: 4,
    amount: 12.5,
    description: 'Coca-cola',
  }
  return {
    handleAddExpense: () =>
      dispatch({ type: 'ADD_EXPENSE', payload: newExpense }),
  }
}
export default connect(
  mapStateToProps,
  mapDispatchToProps,
)(App)
```

Nótese que hemos eliminado la función anterior conocida como **handleAddExpense**, bueno de hecho la hemos reubicado de posición. Ahora la hemos definido dentro de la función **mapDispatchToProps**.

mapDispatchToProps, recibe como parámetro el método **dispatch** de redux, y en este caso hemos declarado el objeto _newExpense_ y hemos retornado la nueva función _handleAddExpense_ la cual mediante dispatch enviamos el tipo de acción y el payload `{ type: 'ADD_EXPENSE', payload: newExpense }`.

```jsx
// Función MapDispatchToProps
const mapDispatchToProps = dispatch => {
  const newExpense = {
    id: 4,
    amount: 12.5,
    description: 'Coca-cola',
  }
  return {
    handleAddExpense: () =>
      dispatch({ type: 'ADD_EXPENSE', payload: newExpense }),
  }
}
```

Luego, pasamos el segundo parámetro que recibe el método **connect** como se muestra:

```jsx
export default connect(
  mapStateToProps,
  mapDispatchToProps, // Inyectamos las acciones a nuestro App.js
)(App)
```

Ahora, la función **handleAddExpense** es parte de las props de nuestro App.js véase en la siguiente línea:

```jsx
const { expensesItems, handleAddExpense } = props
```

Tenemos dos props: `expensesItems` que viene de la Store y `handleAddExpense` que es el action creator para modificar nuestro State.

Suficiente por hoy, hasta la próxima!
