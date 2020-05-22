---
title: 'ECMAScript 6 / ES6 / ECMAScript2015 / ES2015'
---



**JavaScript (JS) es un lenguaje de programación interpretado, dialecto del estándar** [ECMA-262](https://www.ecma-international.org/publications/standards/Ecma-262.htm) donde esta es una especificación de lenguaje de programación publicada por ECMA International. 

En tal sentido,  ECMA (_European Computer Manufacturer Association_) se encarga de regular varios estándares entre ellos el lenguaje JavaScript, también conocida como ECMAScript. En efecto, existen las versiones ECMAScript 1, 2, 3 que fueron usadas en un inicio, la versión 4 fue abandonada, pero la versión 5 es aún una de las más usadas y aceptadas por la mayoría de los navegadores. Sin embargo, ECMAScript 6 (ES6) fue lanzada ya el 2015 con el nombre oficial ECMAScript2015. Esto puede ser expuesto de mejor manera en la siguiente línea de tiempo.

* 1995: JavaScript nace como LiveScript
* 1997: Se establece el estándar ECMAScript
* 1999: ES3 es lanzado y IE5 es muy popular
* 2000–2005: XMLHttpRequest  (AJAX)  gana mucha popularidad por se usado por servicios como Oddpost, Gmail y Google Maps .
* 2009: ES5 es lanzado junto al estándar JSON
* 2015: ES6/ECMAScript 2015 es lanzado con nuevos features y mayor syntactic sugar es decir el poder hacer algunas cosas de forma más sencilla.

A pesar que ES6 existe ya hace muchos años, algunos browsers no lo soporta totalmente por que muchas veces es necesario usar un JavaScript compiler o code transformer como Babel para aprovechar todos los features y facilidades que posee.
## let y const
Las variables pueden ser declaradas con let en lugar de var si no se quiere que sea accesible fuera de su ámbito o también llamado scope.  
```js
//ES5
(function() {
    console.log(x); // x no está definida aún.
    if(true) {
        var x = "hola mundo";
    }
    console.log(x);  // Imprime "hola mundo", porque "var" hace que sea global a la función;
})();
```
```js
//ES6
(function() {
    if(true) {
        let x = "hola mundo";
    }
    console.log(x);  //Da error, porque "x" ha sido definida dentro del bloque  "if"
})();
```
En el caso de const, permite declarar constantes y establecer al mismo tiempo sus valores ya que luego no pueden ser modificadas.
```js
//ES6
(function() {
    const PI;
    PI = 3.15; 
    // ERROR porque se debe de asignarse un valor en la declaración y no después
})();
(function() {
    const PI = 3.15;
    PI = 3.14159;
    // ERROR porque es sólo-lectura
})();
```
## Función Arrow
También llamadas funciones flecha, son una forma de definir funciones de una manera más limpia y clara con una sintaxis reducida y un this no vinculante. 
```js
//ES5
// función tradicional
const sum = function(a, b) {
  return (a + b);
}
```
```js
//ES6
// arrow function
//const sum = (a, b) => { return a + b};
const sum = (a, b) => (a + b);
```
Es así que, el **keyword function es reemplazado por =>**, además de tener la posibilidad de remover el return y el las llaves al tener una sola expresión.  
```js
//(param1, param2, …, paramN) => { return expresion; }
(param1, param2, …, paramN) => expresion
```
Los paréntesis al inicio son opcionales cuando sólo dispone de un argumento
```js
//(singleParam) => { sentencias }
singleParam => { sentencias }
```
Una función sin argumentos requiere paréntesis: 
```js
() => { sentencias }
```
## This
El keyword this muchas veces necesita del ámbito o contexto circundante dentro de una función tradicional  pero no tiene acceso directo al mismo por lo que existen soluciones alternativas que se pueden hacer.

En  ES3 era necesario cachear el this en una variable ya que al tener una función no se podía acceder al ámbito padre debido a que una función sólo hace referencia al contexto en el que se  encontra. 
```js
//ES3
var obj = {
    foo : function() {...},
    bar : function() {
        var that = this;
        document.addEventListener("click", function(e) {
            that.foo();
        });
    }
}
```
En el caso de ES5 esto cambió, gracias al método bind que permite indicarle que this hace referencia a un contexto y no a otro.
```js
//ES5
var obj = {
    foo : function() {...},
    bar : function() {
        document.addEventListener("click", function(e) {
            this.foo();
        }.bind(this));
    }
}
```
ES6 y la función Arrow => permiten hacer lo mismo de una forma más visual y sencilla.
```js
//ES6
var obj = {
    foo : function() {...},
    bar : function() {
        document.addEventListener("click", (e) => this.foo());
    }
}
```
El this dentro de una arrow function se define en un “ámbito léxico” esto quiere decir que se  toma el this de su entorno, por lo que ya no necesita hacer un bind() o crear una const that=this para permitirnos usar el contexto correcto.
## Clases
La manera tradicional de crear objetos, definir atributos y adicionar métodos era de la siguiente manera.
```js
//ES5
//Object Constructor
function Tarea (titulo, urgencia){
    this.titulo = titulo;
    this.urgencia= urgencia;
}
Tarea.prototype.mostrar=function(){
    return `La tarea ${this.titulo} tiene una prioridad ${this.urgencia}`;
}
const tarea1 = new Tarea('Aprender nodejs','Urgente')
const tarea2 = new Tarea('Caminar','Media')
console.log(tarea1.mostrar());
console.log(tarea2.mostrar());
```
En ES6 las clases se declaran de manera muy similar a lenguajes tradicionales como Java permitiendo crear los constructores, definir propiedades, adicionar métodos, establecer relaciones de herencia e instanciar los objetos de dichas clases. 
```js
//ES6
class Tarea {
    constructor(titulo,prioridad){
        this.titulo = titulo;
        this.prioridad = prioridad;
    }
    mostrar(){
        return(`${this.titulo} tiene una prioridad  ${this.prioridad}`)
    }
}
let tarea1 = new Tarea('Tarea JS','Alta');
let tarea2 = new Tarea('Tarea CSS','Baja');
console.log(tarea1.mostrar())
console.log(tarea2.mostrar())
```
## Template Strings
Los templates strings permiten tener cadenas multilínea sin tener que concatenarlos con **+** usando el carácter backtick (** ` **).
```js
//ES5
var saludo = "hola " +
"mundo " +
":) ";
```
```js
//ES6
var saludo = `hola
mundo
:)`;
console.log(`hola
que
ase`);
```
Adicionalmente, permite hacer interpolaciones más elegantes y prácticas
```js
//ES6
let nombre1 = "Gonzalo";
let nombre2 = "Hk";
console.log(`Hola ${nombre1} ${nombre2, bienvenido`);
```
## Destructuring 
Consiste en una forma directa de declarar y extraer los datos de un array/objeto incluso pudiendo ser múltiple.
```js
const aprendiendoJS={
    version:{
        nueva: 'es6',
        anterior: 'es5'
    },
    frameworks: ['vuejs','angularjs','laravel']
}

//ES5 
let version = aprendiendoJS.version;
let frameworks = aprendiendoJS.frameworks[1];

console.log(version);  
console.log(frameworks);  
```
```js
//ES6
let {version, frameworks}= aprendiendoJS;
console.log(version);  
console.log(frameworks[1]);  

let {nueva}= aprendiendoJS.version;
console.log(nueva);  
```
## Valores por Defecto
Los valores de los parámetros tenían que ser validados en el caso de no tener un valor y asignarles uno por defecto manualmente. 
```js
//ES5
(function(valor) {
	valor = valor || "foo";
	console.log(valor);
})()
```
Con ES6 ya es posible definir los valores por defecto de los parámetros al momento de crear una función 
```js
//ES6
(function(valor="foo") {
	console.log(valor);
})()
```
## Objetos 
La forma tradicional de crear objetos es denominada Object Literal, la misma es una colección de datos separados en pares nombre: valor separados por comas y encerrados por comillas 
```js
// Object Literal
const persona = {
    nombre: Gonzalo,
    profesion: 'Desarrollador Web',
    edad: 29
}
console.log(persona);
```
Además que se puede acceder a estos individualmente a los datos de un objeto de dos formas distintas. 
```js
console.log(persona.nombre);
console.log(persona.['nombre'])
```
En ES6 se tienen una formas más sugar syntax  de crear los objetos, que se domina Object Literal Enhancement.  
```js
//ES6
const banda = 'Damas Gratis';
const genero = 'Cumbia V';
const canciones = ['Laura', 'Loro', ''No te creas tan importante']
const damasG = {banda, genero, canciones};
console.log(damasG);
```
## Módulos
Esta característica tiende a ser similar a Python o Ruby donde es posible importar funciones de forma sencilla. En nuestro caso, se pueden importar funciones desde los propios scripts sin tener que importarlos desde el HTML como se hacía anteriormente.  
```js
//person.js
module "person" {
    export function hello(nombre) {
        return nombre;
    }
}
```
De esta forma es posible importar y usar la función hello(...) desde algún otro archivo js
```js
//app.js
import { hello } from "person";
var app = {
    foo: function() {
        hello("Carlos");
    }
}
export app;
```
También es posible exportar las funciones usando la siguiente sintaxis 
```js
export function hello(nombre) {...};
export default function(nombre) {...};
```
En efecto, desde 2015 cada año se introducen nuevas características en JavaScript y se van haciendo nuevas especificaciones para el lenguaje cada año es por ello que se habla del ES2015, ES2016, ES2017, ES2018, ES2019 y ES2020. Por ellos es común encontrar referencias a la versión que expusimos como ES6 o ECMAScript 6 o ES2015 o ECMAScript2015. 

ES6 fue el cambio más radical que tuvo JS a diferencia de las especificaciones posteriores que solo tienen cambios mínimos. Es decir, JS obtuvo mayor madurez con nuevos features además de aplicar sugar syntax a parte de su código tradicional que fue bien recibido por su comunidad. No obstante, es actualmente el estándar para el manejo de muchos frameworks modernos y la tendencia en el desarrollo web.




