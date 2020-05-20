---
title: 'Ecosistema de React'
media_order: ecosistema-react.png
date: '11-05-2020 21:18'
publish_date: '11-05-2020 21:18'
unpublish_date: '20-05-2020 16:08'
metadata:
    'Ecosistema Reactjs': reactjs
taxonomy:
    category:
        - Frontend
    tag:
        - React
---

![Ecosistema React](ecosistema-react.png?classes=center-block)

React como tal no es tan complejo debido a que se trata solo de biblioteca UI. No obstante, lo complicado en un inicio está en entender y configurar el ecosistema en el que funciona.

En efecto, para el desarrollo de una aplicación se necesita combinar NPM, Babel y Webpack en un primera instancia, pero también se necesitará establecer alguna forma de hacer el routing de la aplicación, así mismo definir el styling e incluso ir más allá y adicionar redux. Es necesario conocer y saber el por qué de este ecosistema. 
## React 
Al ser solo una biblioteca para construir interfaces de usuario. Para adicionarlo en una página web sencilla se puede hacer algo como:
```html
<!DOCTYPE html>
<html>
<head>
  <title>React</title>
  <script crossorigin src="https://unpkg.com/react@16.7/umd/react.development.js"></script>
  <script crossorigin src="https://unpkg.com/react-dom@16.7/umd/react-dom.development.js"></script>
  <script src='https://unpkg.com/babel-standalone@6/babel.min.js'></script>
</head>
<body>
  <div id='app'></div>
  <script type='text/babel'>
    function Hello ({ name }) {
      return <h1>Hello, {name}</h1>
    }

    ReactDOM.render(
      <Hello name='Tyler' />,
      document.getElementById('app')
    )
  </script>
</body>
</html>
```
Sin embargo, este es solo un ejemplo rápido que no es mantenible ni mucho menos escalable. Un proyecto react convencional por lo general se ve de la siguiente forma. 
```js
import React from 'react'

export default function User ({ name, username }) {
	const avatarURL = `https://github.com/${username}.png?size=200`
  	return (
    	<div>
      		<h1>{name}</h1>
      			<img 
        		alt={`Avatar for ${username}`}
        		src={avatarURL}
      			/>
    		</div>
  		)
}
```
Si copiamos este código para corremos directamente en un browser se tendrán errores de sintaxis en el código JSX y en los import/export que se hallan en mi código. En tal sentido, para tener el build correcto de nuestra aplicación se necesitan de herramientas extra para ello solucionaremos el primer problema con babel y el segundo con webpack.
## Babel 
Se considera un **JavaScript compiler o en otras palabras un code transformer**, es decir debemos imaginar que babel es una máquina que recibe codigo por una extremo y por el otro retorna un código nuevo que se ha transformado. 

El código del anterior ejemplo similar a HTML en realidad es JSX, esta es una extensión de la sintaxis de javaScript que permite describir el UI que se está programando de una forma muy natural. En efecto, al tener código JSX se necesita de babel para funcionar debido a que los browsers no lo soportan nativamente, es decir babel necesita transformar el react code incluyendo JSX a un código javascript viejo que nuestros browsers pueden soportar o sean compatibles. 

Es necesario aclarar que babel no se limita solo a transformación de código JSX porque también puede convertir emacscript2015+ a código compatible a nuestros navegadores.
## Webpack 
Se denomina como un **module bundler**, que examinan el código fuente para verificar los import y exports para posteriormente agrupar todos estos de un forma muy inteligente en un solo archivo que el navegador pueda entender. 

Por ello, en lugar de incluir todos los scripts en un _index.html_ lo único que se hará es incluir un único archivo _bundle.js_ de modo tal que no se incluirán ninguna declaracion de importacion o exportacion permitiendo al navegador entenderlo sin ahogarlo en caso de ser un proyecto grande.
## Routing con React Router
React es solo una librería UI por lo que no incluye otras funcionalidades como el ruteo tal como lo hacen muchos frameworks, por lo que se la debe incluir. React Router es una de las más populares y tienen componentes específicos para trabajar con las urls como se ven el siguiente ejemplo.
```js
<Router>
  	<div>
    	<ul>
      		<li><Link to="/">Home</Link></li>
      		<li><Link to="/about">About</Link></li>
      		<li><Link to="/topics">Topics</Link></li>
    	</ul>
	    <Route exact path="/" component={Home}/>
    	<Route path="/about" component={About}/>
    	<Route path="/topics" component={Topics}/>
	</div>
</Router>
```
## Styling con Styled Components 
Es uno de los aspectos más controversiales que tiene el ecosistema de react debido a que se tiende a no seguir un esquema tradicional donde se tiene un styles.css que aplique estilos a todo el proyecto, es decir hojas de estilo en cascadas tradicionales o estidos manejados con preprocesadores css. Pero esto no quiere decir que no sea posible hacerlo de esta forma.

React tiende a tener un manejo de estilos no tradicional donde un componente debería tener sus estilos dentro del mismo de tal modo que todo queda encapsulado como se ve en el ejemplo. 
```js
const styles = {
  header: {
    fontWeight: 400,
    fontSize: 55,
    color: '#a41c1c'
  },
  avatar: {
    width: 50,
    height: 50,
    borderRadius: 25
  }
}

export default function User ({ name, username }) {
  const avatarURL = `https://github.com/${username}.png?size=200`
  return (
    <div>
      <h1 style={styles.header}>{name}</h1>
      <img
        style={styles.avatar}
        alt={`Avatar for ${username}`}
        src={avatarURL}
      />
    </div>
  )
}
```
Styled components es la libreria más popular en el ecosistema es react. 
```js
import React from 'react'
import styled from 'styled-components'

const Header = styled.h1`
  font-weight: 400;
  font-size: 55;
  color: #a41c1c;
`
const Avatar = styled.img`
  width: 50px;
  height: 50px;
  border-radius: 25px;
`
export default function User ({ name, username }) {
  const avatarURL = `https://github.com/${username}.png?size=200`
  return (
    <div>
      <Header>{name}</Header>
      <Image
        alt={`Avatar for ${username}`}
        src={avatarURL}
      />
    </div>
  )
}
```
## Redux
**Redux es un patrón de arquitectura de datos** que permite manejar el estado de la aplicación de una manera predecible, incluso ellos mismos se dicen **Predictable state container for JavaScript** al ser un patrón puede ser aplicado a todo tipo de proyectos de diferentes frameworks y bibliotecas por lo que no  es de uso exclusivo de react.

La filosofía de react indica que cada componente debe manejar sus propios estados, describir su propio UI para finalmente componerse con otro componentes para tener una app. 
No obstante, la filosofía de redux es algo diferente donde en lugar de tener los cambios de estados en diferentes lugares o componente. Redux considera tener un único lugar donde hacer estos cambios llamado store donde además se deben establecer reglas estrictas de cómo un estado y tu store pueden cambiar.

Este es un ecosistema básico que react plantea para el desarrollo de las aplicaciones, proyectos en general pudiendo incorporarse muchas más funcionalidades y bondades según los requerimientos que tengamos. Por sí solo react solo es una biblioteca pero bajo el ecosistema que plantea iguala y posiblemente supera a toda la funcionalidad que tendría un framework convencional.   