---
title: ReactJS
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

![ReactJS](./images/react-logo.png?classes=center-block)

**React es un biblioteca javascript para construir interfaces de usuario (UI)** de código abierto que fue creada por Jordan Walke, lanzada el 2013, mantenida por Facebook y la comunidad de software libre. React se destaca por ser **declarativo y basado en componentes** de una manera muy natural.

En tal sentido, React tiene características que le permiten ser fácil de comprender, potente para construir interfaces gráficas en función a sus estados, con un flujo de datos sencillo siendo popular y recomendada. 
## Composición
Es necesario entender que un **componente en React es un elemento independiente y reutilizable** y un **elemento es el bloque más pequeño de aplicaciones react**. Esto significa que los componentes son como las funciones de JavaScript es decir aceptan entradas (props) y devuelven elementos React que describen lo que debe aparecer en la pantalla.

![ Los react components son como los legos para los programadores](./images/composicion-lego.jpg?classes=center-block)

En efecto, la composición es una de las propiedades que más caracterizan a esta biblioteca debido a que **React tiene el poder de combinar componentes para crear un nuevo componentes más complejo**.

Para tener una idea más clara podemos imaginar que necesitamos obtener el avatar (fotografia y profile URL) de mis usuarios, para ellos tengo un par de funciones que me ayudaran obtener dicha información. 
```js
function getProfilePic(username){
	return "https://photos.myserver.com/"+username
}
function getProfileLink(username){
	return "https://myserver.com/"+username
}
```
Entonces, partiendo de las anteriores funciones se puede generar una tercera que sea compuesta de ambas y pueda retornar información mejor filtrada para nuestro caso el avatar del usuario.
```js  
function getAvatarInfo(username){
	return {
		Pic: getProfilePic(username),
		link:  getProfileLink(username)
	}
}
```
Ahora que pasaria si por alguna razón nos gustaría componer UI en lugar de funciones que obtienen algún determinado valor, esta es una característica que hacen de react una biblioteca muy poderosa. Entonces podemos hacer algo más interesante con el anterior ejemplo. 
```js
function profilePic(props) {
	return (
		<img src ={"https://photo.myserver.com/"+ props.username}/>
	)
}
function profileLink(props) {
	return (
		<a href={"https://myserver.com/users/"+ props.username}>{props.username}</a>
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
Entonces, se tiene un componente llamado Avatar que es compuesto de otros dos más pequeños, es decir podemos combinar componentes para crear nuevos componentes más complejos. En efecto, **la composición se considera un principio de programación language-agnostic** que es intuitiva que puede aplicarse muy bien a la creación de componentes mediante react. 

No obstante, un gran beneficio de los react components es que existe un ecosistema muy nutrido donde se tiene a disposición componentes de terceros que pueden ser reutilizados y muchos de ellos están disponibles en NPM.

> Los react components son como los legos para los programadores.

## Flujo de datos unidireccional

Un factor fundamental que diferencia al desarrollo de aplicaciones robustas es saber cuando y como los estados cambian. 

En el caso de Jquery se tienen event handlers que son responsables de actualizar los estados en la aplicación manipulando el DOM. Esta práctica es habitual donde en un inicio no se tienen problemas debido a que no existen muchos event handlers.

![jQuery flujo de datos ideal](./images/jqueryb.png?classes=center-block)

Sin embargo, más adelante el proyecto crecerá siendo más difícil de  mantener debido a que se manejan múltiples estados desde diferente lugares en el proyecto, esto a la larga se vuelve frágil e inestable.

![jQuery flujo de datos real](./images/jquery.png?classes=center-block)

React tiene una visión distinta del manejo de los estados a diferencia de bibliotecas como Jquery. En efecto, el estado vive en el componente react y desde allí se puede controlar explícitamente cómo y cuando el estado debería cambiar o en este caso como deberia verse el UI basado en el cambio de estado.

![React flujo de datos](./images/react-ui.png?classes=center-block)

Una forma interesante de ver esto, es pensar que el UI solo es una función de su estado

![UI = fn(state)](./images/ui-fn-state.png?classes=center-block)
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
Por otra lado, **React es declarativo por lo que se debe describir _QUE_ debería hacer la UI y no _COMO_ debe hacerlo**, en otras palabras react se encargará del trabajo duro actualizando el dom haciendo uso de su poderoso dom virtual. Esto nos ayuda de sobremanera porque solo deberemos preocuparnos de cómo el estado en un componente cambia y en función a ese estado que debería hacer (verse) la UI. El mismo bloque de codigo anterior puede ser escrito con React de la siguiente manera.
```html
<Btn
  highlight={this.state.highlight}
  onToggle={this.toggleBtn}
/>
```
## Solo es JavaScript
Este punto es controversial porque está acorde al gusto de cada desarrollador, el hecho de recrear una funcionalidad que ya se tenía para usar en un determinado framework o biblioteca implica recordar o memorizar una nueva forma de hacer algo que ya podíamos hacer antes. 

i.e. Listar un array

_Vue.js_
```js
<ul id="friends">
	<li v-for="friend in friends">
    	{{ friend }}
  	</li>
</ul>
```
_Angular 2_
```js
<ul>
	<li *ngFor="#friend of friends">
		{{ friend}}</p>
	</li>
</ul>
```
_React_

Con react podemos usar solo javaScript, en este caso el método map seria suficiente.
```js
<ul>
	{friends.map((name) => (
    	<li>
      		{name}
    	</li>
  	))}
</ul>
```
En efecto, esto no siginifica que otras bibliotecas y frameworks estan mal solo que se priorizan cosas diferentes. Sin embargo, con React se tiene una ligeria diferencia positiva al no tener que memorizar nueva sintaxis o formas de hacer algo.

## Abstracción 
La abstracción resulta muchas veces difícil debido a que la naturaleza de algunos frameworks que suelen ser difíciles en el sentido de modelar, esta debe dar más versátil para el programador, sin limitantes, permitiendo implementar todo de formas más natural posible y es así como lo hace react. 

El construir componentes con React es sencillo siguiendo un logica declarativa y la abstracción tiende a ser natural, por ejemplo para construir un componente calendario se podria hacer de lo siguiente.
```html
<container>
	<Navbar/>
	<Header/>
	<DatePicker>
		<Calendar/>
	</DatePicker>
</container>
```
## Comunidad 
La comunidad de react es muy grande debido a la cantidad de proyectos en los utilizan y la gente involucrada en el medio de modo tal que se tiene suficiente información y soporte. Además de tener múltiples eventos en todo el mundo y gigantescas conferencias. También es bueno saber que Facebook invierte mucho en la continua investigación y desarrollo de esta librería que está resolviendo mucho de nuestras limitantes que se tenían con librerias clásicas.
* [http://reactjsnewsletter.com/](http://reactjsnewsletter.com/)
* [https://www.reddit.com/r/reactjs/](https://www.reddit.com/r/reactjs/)
* [https://discord.gg/BnR8s5n](https://discord.gg/BnR8s5n)
* [https://reactjs.org/community/conferences.html](https://reactjs.org/community/conferences.html)



    
