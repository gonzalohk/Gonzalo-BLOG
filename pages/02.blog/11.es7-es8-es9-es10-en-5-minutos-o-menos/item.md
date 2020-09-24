---
title: 'ES7, ES8, ES9 y ES10 en 5 minutos'
media_order: 'banner-js.jpg,EcmaScript.gif'
date: '23-09-2020 11:20'
publish_date: '23-09-2020 11:21'
metadata:
    '"Keywords"': '"ECMAScript", "ES7","ES8","ES9","ES10", "ecma international", "ES"'
taxonomy:
    category:
        - JS
        - Frontend
    tag:
        - JavaScript
        - EcmaScript
---

![EcmaScript](EcmaScript.gif?classes=center-block)

ES6 sin duda fue un antes y después de JavaScript, tal como se señaló en un [post anterior](http://gonzalohk.xyz/blog/ecmascript6). Sin embargo, nos preguntamos qué pasó después o cómo es que se sigue hoy en día adicionando nuevos features. 

Primero debemos saber que **la especificación de JavaScript propuesta por ECMA international se actualiza cada año en el mes de junio**. En efecto, los estándares ES7, ES8, ES9, ES10, ES11 ya fueron lanzados, ahora bien muchos de los cambios no son muy sustanciales, pero resultan muy útiles al momento del desarrollo e implícitamente ya venimos usando muchos de estos en frameworks y librerías que usamos día a día.
## ES7 (Ecmascript 2016)
#### Includes
Este método determina si una matriz incluye un determinado elemento, devuelve true o false según corresponda.

```js
let numbers = [1, 2, 4, 5, 9];
console.log(numbers.includes(5));// true
console.log(numbers.includes(11));// false
```
#### Exponencial 
Este es un nuevo operador que permite calcular la potencia de un número donde se indica el número base y el exponente, similar a Math.pow(base, exponente), pero ahora no es necesario utilizarla gracias al operador ** .

```js
const base = 2;
const exponent = 16;
console.log(base ** exponent); // 65536
```

## ES8 (Ecmascript 2017)
#### async y await
La declaración de función async define una función asíncrona, la cual devuelve un objeto AsyncFunction. 

Es decir, las funciones asíncronas devuelven promesas que son declaradas con la keyword async delante de la declaración de la función, dentro de este tipo funciones se puede utilizar el keyword await e indicará que se detenga hasta que la promesa no esté resuelta. Una vez  resuelta, continuará con la ejecución normal. De esta forma  ya no son necesarios el .then ni el .catch de las promesas simplificando un poco el código y viéndose de manera más legible. 
```js
const coolSong = () => {
	return new Promise((resolve, reject) => {
		if (true) {
			return setTimeout(() => resolve('Welcome to the jungle'), 2000);
		}
		return setTimeout(() => reject('T.N.T.'), 5000);
	})
};
const coolSongAsync = async () => {
	const cool = 'The coolest song is...';
	console.log(cool);
	try {
		const song = await coolSong();
		console.log(song);
	} catch (error) {
		console.log(`Apparently something went wrong: ${error}`);
	}
};
coolSongAsync(); // The coolest song is... Welcome to the jungle
```
#### Object.entries
Es un nuevo método de la clase Object que devuelve una matriz de pares propios de una propiedad enumerable [key, value] de un objeto dado.
```js
const data = {
	name: 'Gonzalo',
	age: 28,
	rocks: 'M',
};
const entries = Object.entries(data);
console.log(entries); // [ [ 'name', 'Manuel' ], [ 'age', 24 ], [ 'rocks', true ] ]
```
#### Object.values
Es un nuevo método de la clase Object que devuelve un array que contiene solamente los valores de las propiedades enumerables de un objeto dado.
```js
const data = {
	name: 'Gonzalo',
	age: 28,
	gender: 'M',
};
const entries = Object.values(data);
console.log(entries); // [ 'Gonzalo', 28, 'M' ]
```
#### Padding de Strings
Se agregaron dos métodos de instancia String que permiten el agregar o anteponer una cadena al inicio o al final de la cadena original.  Es decir, permite rellenar la cadena actual con una cadena dada (repetida eventualmente) de modo que la cadena resultante alcance una longitud dada.

#### PadStart
En el ejemplo se antepone la cadena “X” cuantas veces sea necesario para que la cadena original tenga la longitud establecida.
```js
const string = 'hi';
const newString = string.padStart(5, 'X'); 
console.log(newString); // 'XXXhi'
```
#### PadEnd
A diferencia del anterior ejemplo, ahora se agrega la cadena “X” cuantas veces sea necesario para que la cadena original tenga la longitud establecida.
```js
const string = 'hi';
const newString = string.padEnd(5, 'X');
console.log(newString); // 'hiXXX'
```
#### Trailing commas
Para muchos es un cambio menor casi innecesario, pero permite tener comas finales después del último parámetro de una función. 

Imaginemos que necesitamos modificar una función adicionando un nuevo parámetro, esto significa adicionar una coma para agregar el nuevo parámetro de modo tal que se modificarían dos líneas de código según nuestro versionador de código. Pero si utilizamos trailing commas el cambio sólo sería agregar el parámetro con una línea de código, de tal modo que la visualización de los cambios realizados será más limpio.

## ES9 (Ecmascript2018)
#### Spread en objetos 
Se lo conoce como operador de propagación o spread operator, este permite que una expresión sea expandida en situaciones donde se esperan múltiples argumentos como llamadas a funciones o múltiples elementos en arrays. Ahora en ES9 también puede ser aplicado a objetos.  
```js 
const data = {
	name: 'Gonzalo',
	age: 28,
	gender: 'M',
}
const { name, ...rest } = data;
console.log(rest); // { age: 28, gender: 'M' }
```
#### Composición de objetos
Con el operador spread no solamente podemos agrupar elementos, también podemos componer nuevos objetos. 
```js
const data = {
	name: 'Gonzalo',
	age: 28,	
	gender: 'M'
}
const additionalData = {
	country: 'BO',
	city: 'LP'
}
const person = {
	...data,
	...additionalData,
}
console.log(person);
```
#### Finally en Promises
Es una función introducida en las promesas que se ejecuta siempre al finalizar la promesa sin importar si fue ejecutada con éxito o fue rechazada. Esta funciona de la misma forma que el always del Ajax de jQuery o el finally de Java.

```js
const coolSong = () => {
	return new Promise((resolve, reject) => {
		if (true) {
			return setTimeout(() => resolve('Welcome to the jungle'), 2000);
		}
		return setTimeout(() => reject('T.N.T.'), 5000);
	})
};
console.log('The coolest song is...');
coolSong()
	.then((song) => {
		console.log(song);
	})
	.catch((error) => {
		console.log(`Apparently something went wrong: ${error}`);
	})
	.finally(() => {
		console.log('Bye!');
	})
```
#### Regex
Se introdujeron varias mejoras y cambios al prototype de Regex una de ellas es la captura de grupos en bloques.

```js
const regex = /([0-9]{4})-([0-9]{2})-([0-9]{2})/
const match = regex.exec('2020-06-02');
const year = match[1];
const month = match[2];
const day = match[3];
console.log(year, month, day); // 2020 06 02
```
## ES10 (Ecmascript2019)
#### Flat 
Es un método que crea una nueva matriz con todos los elementos de sub-array concatenados recursivamente hasta la profundidad o nivel especificado.
```js
const messedArray = [1, 2, 3, [1, 2, 3, [1, 2, 3]], [4, 5, 6]];
const flatedArray = messedArray.flat();
const flatedArray2 = messedArray.flat(2);
console.log(flatedArray); // [ 1, 2, 3, 1, 2, 3, [ 1, 2, 3 ], 4, 5, 6 ]
console.log(flatedArray2); // [ 1, 2, 3, 1, 2, 3, 1, 2, 3, 4, 5, 6 ]
```
#### Flatmap
Tiene el mismo efecto que usar el método map() seguido inmediatamente del método flat() con una profundidad por defecto de 1. Es decir, flatMap(), mapea cada valor a un nuevo valor y el resultado es aplanado a una profundidad de 1.
```js
const messedArray = [1, 2, 3, 4, 5, 6];
const mapFlatArray = messedArray.flatMap((value) => [value * 2, value * 3]);
console.log(mapFlatArray); // [ 2, 3, 4, 6, 6, 9, 8, 12, 10, 15, 12, 18 ]
```
#### trimStart y trimEnd
Elimina los espacios en blanco al inicio o al final de la cadena.
```js
const hello = '     Hello World      '
console.log(`${hello} here is the end`); //     Hello World       here is the end
console.log(`${hello.trimStart()} here is the end`); //Hello World       here is the end
console.log(`${hello.trimEnd()} here is the end`); //     Hello World here is the end
```
#### Optional catch bind
Permite hacer bloques de try y catch sin el parámetro (Error) del catch si no queremos usarlo de modo tal que ahora es opcional.
```js
let res;
let a=1;
let b=0
try {
    res=a/b;
} catch {
	console.log("#¡Div/0!")
} // no es necesario pasar error al catch
```
#### Object.fromEntries
Hace lo inverso al Object.entries(), permite pasar de un array de arrays a un Objecto.

```js
const data = [
	['name', 'Manuel'],
	['age', 24],
	['rocks', true],
];
const object = Object.fromEntries(data);
console.log(object); // { name: 'Manuel', age: 24, rocks: true }
```
#### Symbol description
Ésta nueva propiedad permite acceder a la descripción de un símbolo sin tener que usar el método toString().
```js
const symbolDescription = 'My Symbol description';
const mySymbol = Symbol(symbolDescription);
console.log(mySymbol.description); // My symbol description
```

Como se vio en estos ejemplos, los features que se fueron adicionando luego de ES6 por sí solos no representan cambios grandes, pero juntos resultan una gran evolución a como conocimos inicialmente a JavaScript, de modo tal que se reflejan características propias y muchas otras adoptadas de otros lenguajes, a pesar que los browsers van adoptando estos estándares paulatinamente es necesario conocerlos y aplicarlos con el fin de siempre mejorar el código de nuestros proyectos. 
