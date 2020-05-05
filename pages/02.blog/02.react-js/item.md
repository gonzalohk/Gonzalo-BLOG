---
title: 'React.js desde cero'
published: true
date: '04-05-2020 19:50'
publish_date: '04-05-2020 19:50'
metadata:
    '¿Que es React?': React.js
    'Ecosistema React': 'React, babel, webpack'
taxonomy:
    category:
        - Frontend
    tag:
        - React
---

## ¿Que es React,js?
React es un biblioteca javascript para construir interfaces de usuario (UI) de código abierto que fue creada por Jordan Walke y lanzada el 2013, mantenida por Facebook y la comunidad de software libre. React se destaca por ser declarativo y basado en componentes de una manera muy natural.
Tratare de explicar dichas características que hacen de esta librería tan especial.
## Composición
Es una de las propiedades que más caracterizan a react, que es llevada a un nivel diferente. Para tener una idea más clara podemos imaginar que necesitamos obtener el avatar (fotografia y profile URL) de mis usuarios, para ellos tengo un par de funciones que me ayudaran obtener la información necesito. 
```js
function getProfilePic(username){
	return ‘’’https://photos.myserver.com/’’+username
}

function getProfileLink(username){
	return ‘’’https://myserver.com/’’+username
}
```
En tal sentido, partiendo de las anteriores funciones se puede generar una tercera que sea compuesta de ambas y pueda retornar información mejor filtrada en este caso el avatar del usuario
```js  
function getAvatarInfo(username){
	return {
		Pic: getProfilePic(username),
		link:  getProfileLink(username)
	}
}
```
Ahora que pasaria si por alguna razón nos gustaría componer UI en lugar de funciones que obtienen algún valor, esta es una característica que hacen de react una biblioteca muy poderosa. Entonces podemos hacer algo más interesante con el anterior ejemplo. 
```js
function profilePic(props) {
	return (
		<img src ={‘https://photo.myserver.com/’+ props.username}/>
	)
}
function profileLink(props) {
	return (
		<a href={‘https://myserver.com/users/’’+ props.username}>{props.username}</a>
	)
}

function Avatar(props) {
	return (
		<div>
			<ProfilePic username={props.username}/ >
			<ProfileLink username={props.username}/ >
		</div>
	)
}

<Avatar username=”gonzalohk”>
```
Entonces se tiene un componente llamado Avatar que es compuesto de otros dos más pequeños, es decir podemos combinar componentes para crear nuevos componentes más complejos. En efecto, la composición se considera un principio de programación language-agnostic que es intuitiva que puede aplicarse muy bien a la creación de componentes mediante react por ejemplo podemos un calendario podría ser compuesto de la siguiente manera:
```js
<container>
	<Navbar/>
	<Header/>
	<DatePicker>
		<Calendar/>
	</Date Picker>
</container>
```
No obstante, un gran beneficio de los react components es que existe un ecosistema muy nutrido donde se tiene a disposición componentes de terceros que pueden ser reutilizados y muchos de ellos están disponibles en NPM.


> Los react components son como los legos para los programadores.

## Flujo de datos unidireccional

Un factor fundamental que diferencia al desarrollo de aplicaciones robustas es saber cuando y como los estados cambian. En el caso de Jquery se tienen event handlers que son responsables de actualizar los estados en la aplicación manipulando el dom.	
Esta práctica es habitual donde en un inicio no se tienen problemas debido a que no existen muchos event handlers. Sin embargo, más adelante el proyecto crecerá siendo más difícil de  mantener debido a que se manejan múltiples estados desde diferente lugares en el proyecto, esto a la larga se vuelve frágil e inestable.
React tiene una visión distinta del manejo de los estados a diferencia de bibliotecas como Jquery. En efecto, el estado vive en el componente react y desde allí se puede controlar explícitamente cómo y cuando el estado debería cambiar o en este caso como deberia verse el UI basado en el cambio de estado.
Una forma interesante de ver esto, es pensar que el UI solo es una función de su estado
**UI = fn(state)**

## UI declarativo 
Este apartado es muy visible en el código fuente, siguiendo el ejemplo anterior para definir como un estado se actualiza al utilizar jQuery se necesita una programación imperativa. Es así que se puede actualizar el dom de la siguiente manera 
```js
$('btn').click(() => {
  $(this).toggleClass('highlight')
  $(this).text() === 'Add Highlight'
    ? $(this).text('Remove Highlight')
    : $(this).text('Add Highlight')
})
```
Por otra lado, React es declarativo por lo que se debe describir que debería hacer la UI y no como debe hacerlo, en otras palabras react se encargará del trabajo duro actualizando el dom haciendo uso de su poderoso dom virtual. Esto nos ayuda de sobremanera porque solo deberemos preocuparnos de cómo el estado en un componente cambia y en función a ese estado que debería hacer (verse) la UI.
## Solo es Javascript
Este punto es controversial porque está acorde al gusto de cada desarrollador, el hecho de recrear una funcionalidad que ya se tenía para usar en un determinado framework o librería implica recordar o memorizar una nueva forma de hacer algo que ya podíamos hacer antes. 
Por ejemplo el listar un array se lo hace de la siguiente manera.

Vue.js
```js
<ul id="friends">
	<li v-for="friend in friends">
    	{{ friend }}
  	</li>
</ul>
```
Angular 2 
```js
<ul>
	<li *ngFor="#friend of friends">
		{{ friend}}</p>
	</li>
</ul>
```
React 
Con react podemos usar solo java script en este caso el método map seria suficiente
```js
<ul>
	{friends.map((name) => (
    	<li>
      		{name}
    	</li>
  	))}
</ul>
```
Sin embargo, ninguno está mal solo que se priorizan cosas diferentes.
## Abstraccion 
La abstracción resulta muchas veces difícil debido a que la naturaleza de algunos frameworks suelen ser difíciles en el sentido de modelar, esta debe dar más versátil para el programador, sin limitantes, permitiendo implementar todo de formas más natural posible y es así como lo hace react. .
Comunidad 
La comunidad de react es muy grande debido a la cantidad de proyectos en los utilizan y la gente involucrada en el medio de modo tal que se tiene suficiente información y soporte. Además de tener múltiples eventos en todo el mundo y gigantescas conferencias. También es bueno saber que Facebook invierte mucho en la continua investigación y desarrollo de esta librería que está resolviendo mucho de nuestras limitantes que se tenían con librerias clasicas.
# Ecosistema de React
React como tal no es tan complejo debido a que se trata solo de biblioteca UI. No obstante, lo complicado en un inicio está en entender y configurar el ecosistema en el que funciona.
En efecto, para el desarrollo de una aplicación más real se necesita combinar NPM, Babel y Webpack en un primera instancia, pero también se necesitará establecer alguna forma de hacer el routing de la aplicación, así mismo definir el styling e incluso ir más allá y adicionar redux. Es necesario conocer y saber el por que de este ecosistema. 
## React 
Al ser solo una biblioteca para construir interfaces de usuario. Para adicionarlo en una página web sencilla se puede hacer algo como:
```
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
Sin embargo, este es solo un ejemplo rápido que no es mantenible ni mucho menos escalable. Un proyecto react real por lo general se ve de la siguiente forma 
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
Se considera un JavaScript compiler o en otras palabras un code transformer, es decir debemos imaginar que babel es una máquina que recibe codigo por una extremo y por el otro retorna un código nuevo que se ha transformado. 
El código del anterior ejemplo similar a HTML en realidad es JSX, esta es una extensión de la sintaxis de javaScript que permite describir el UI que se está programando de una forma muy natural. En efecto, al tener código JSX esta necesita de babel para funcionar debido a que los browsers no lo soportan nativamente, es decir babel necesita transformar el rect code incluyendo JSX a un viejo código javascript que nuestros browsers pueden soportar. 
Es necesario aclarar que babel no se limita solo a transformación de código JSX porque también puede convertir emacscript2015+ a código compatible a nuestros navegadores.
## Webpack 
Se denomina como un module bundler, que examinan el código fuente para verificar los import y exports para posteriormente agrupar todos estos de un forma muy inteligente en un solo archivo que el navegador pueda entender. 
Por ello, en lugar de incluir todos los scripts en un index.html lo único que se hará es incluir un único archivo bundle.js de modo tal que no se incluirán ninguna declaracion de importacion o exportacion permitiendo al navegador entenderlo sin ahogarlo en caso de ser un proyecto grande.
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
Es uno de los aspectos más controversiales que tiene el ecosistema de react debido a que no sigue un esquema  tradicional donde se tiene un styles.css que aplique estilos a todo el proyecto, es decir no se tienes hojas de estilo en cascadas tradicionales o estidos manejados con preprocesadores css.
React tiene un manejo de estilos no tradicional donde un componente debería tener sus estilos dentro del mismo de tal modo que todo queda encapsulado como se ve en el ejemplo. 
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
Redux es un patrón de arquitectura de datos que permite manejar el estado de la aplicación de una manera predecible, incluso ellos mismos se dicen Predictable state container for JavaScript Al ser un patrón puede ser aplicado a todo tipo de proyectos de diferentes frameworks y bibliotecas por lo que no  es de uso exclusivo de react 
La filosofía de react indica que cada componente debe manejar sus propios estados, describir su propio UI para finalmente componerse con otro componentes para tener una app. 
No obstante, la filosofía de redux es algo diferente donde en lugar de tener los cambios de estados en diferentes lugares o componente. Redux considera tener un único lugar donde hacer estos cambios llamado store donde además se deben establecer reglas estrictas de cómo un estado y tu store pueden cambiar.
Este es un ecosistema básico que react plantea para el desarrollo de las aplicaciones, proyectos en general pudiendo incorporarse muchas más funcionalidades y bondades según los requerimientos que tengamos. Por sí solo react solo es una biblioteca pero bajo el ecosistema que plantea iguala y posiblemente supera a toda la funcionalidad que tendría un framework convencional.   


    
