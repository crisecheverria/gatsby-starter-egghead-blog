---
slug: reemplazar-redux-con-react-context-hook
date: 2019-07-09
title: 'Reemplazar Redux con React Context Hook'
description: 'Aceptémoslo, Redux es una excelente solución para controlar el State de nuestra aplicación, pero no siempre es lo más recomendable debido a la serie de pasos necesarios para mantener el State.'
published: false
author: 'Cristian Echeverría'
banner: './banner.jpg'
---

Aceptémoslo, Redux es una excelente solución para controlar el State de nuestra aplicación, pero no siempre es lo más recomendable debido a la serie de pasos necesarios para mantener el State o simplemente no queremos agregar más información a nuestro Reducer, es allí donde Context API entra en escena.

### React useContext

```js
const value = useContext(MyContext)
```

El siguiente ejemplo lo desarollaremos usando **create-react-app** para facilitarnos la vida. En tu terminal escribe los siguientes comandos:

```bash
npx create-react-app context-api
cd context-api
yarn add bulma
```

Hemos creado la carpeta **context-api** y dentro de ella agregamos bulma como framework para CSS.

```js
// ./src/App.js
import React from 'react'
import './App.css'

import 'bulma/css/bulma.min.css'

export default function App() {
  return (
    <div className="container">
      <h1 className="title">Employees</h1>
    </div>
  )
}

export default App
```

Modificamos nuestro archivo App.js insertando los estilos de bulma en la Línea 5, agregamos la clase container así como un título.
