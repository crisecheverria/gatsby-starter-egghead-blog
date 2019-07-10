---
slug: herramientas-usadas-en-mi-blog
date: 2019-03-13
title: 'Herramientas usadas en mi blog'
description: 'Algunas personas me han preguntado que herramientas he usado para mi blog y cuanto me cuesta mes a mes mantener el sitio activo. Pues en este blog te cuento lo que he usado y por el cual pago 0.00 mensual por el. Observación: Este es un articulo técnico.'
published: true
author: 'Cristian Echeverría'
banner: './banner.jpg'
---

Algunas personas me han preguntado que herramientas he usado para mi blog y cuanto me cuesta mes a mes mantener el sitio activo. Pues en este blog te cuento lo que he usado y por el cual pago 0.00 mensual por el. Observación: Este es un articulo técnico.

## Gatsby

[Gatsby](https://www.gatsbyjs.org/) es un marco de código abierto y gratuito basado en React que ayuda a los desarrolladores a crear aplicaciones y sitios web increíblemente rápidos.

Pues me decidí por Gatsby debido a que ha tenido bastante auge y crecimiento en los últimos meses y por la idea de que usa React y mi deseo de aprender, aunque sea un poco de GraphQL. Aparte quería algo que fuese "sencillo" para poder mantener y actualizar mi blog personal, ya que mi limitado tiempo es clave y no deseo estar mucho tiempo configurando herramientas para hacerlo andar, pero, sin duda mi principal motivo ha sido el aprendizaje.

#### Markdown

En este caso cada uno de los artículos de mi blog son archivos `Markdown`, lo cual es bastante fácil al momento de publicar artículos.

#### Plugins

Gatsby te permite agregar `plugins` para agregar mas funcionalidad a tu sitio, en mi caso tuve que agregar varios plugins como `gatsby-plugin-styled-components` para Styled Components, `gatsby-plugin-google-analytics` para Google Analytics, y para las imagenes tuve que agregar 3 mas; `gatsby-plugin-sharp`, `gatsby-transformer-remark`, `gatsby-remark-images`.

## Netlify

[Netlify](https://www.netlify.com/) te permite: Construir, implementar y administrar proyectos web modernos. Un flujo de trabajo todo en uno que combina implementación global, integración continua y HTTPS automático.

Netlify te permite conectar tu repositorio ya sea de GitHUb, GitLab o Bitbucket, eliges la rama de tu repositorio que deseas publicar y Netlify se encarga del resto.

Aparte, Netlify te permite agregar un dominio personalizado y te agregan un SSL de forma gratuita. Puedes encontrar instrucciones en Gatsby para poder publicar tu blog.

## GitHub

En mi caso me he decidido por usar GitHub para alojar los archivos de mi blog, ahora que GitHub te permite tener repositorios privados sin costo alguno lo cual es genial.

## Recursos sobre Gatsby

Esta es mi primera vez usando Gatsby y para ello utilicé un excelente recurso que encontré en egghead.io, acá el enlace [Build a Blog with React and Markdown using Gatsby](https://egghead.io/courses/build-a-blog-with-react-and-markdown-using-gatsby). Este otro se ve bien también (aunque no lo he seguido) [Build a Blog using Gatsby & React](https://codeburst.io/build-a-blog-using-gatsby-js-react-8561bfe8fc91).

Y por ultimo, les dejo este otro video tutorial de el bueno de Brad Traversy [GatsbyJs Crash Course](https://www.youtube.com/watch?v=6YhqQ2ZW1sc)

## Conclusión

Al final dependerá de cuales son tus objetivos respecto a tu blog o sitio personal, en mi caso por ahora me va de maravilla y al final si que ocupa algo de conocimiento respecto a React y GraphQL pero los recursos son amplios y bastante explicativos.
