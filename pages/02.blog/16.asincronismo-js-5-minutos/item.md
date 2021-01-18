---
title: 'Entendiendo el Asincronismo en JavaScript en 5 minutos'
media_order: 'callbackHell.jpeg,rickmortyapi.jpg,sincronoVsAsincrono.png'
published: true
metadata:
    '"Keywords"': '"JS","Promises","Asincronismo","Callback","Async/Await", "Sincrono Asincrono JavaScript"'
taxonomy:
    category:
        - JS
        - Frontend
    tag:
        - JavaScript
---

![Entendiendo el Asincronismo en JavaScript en 5 minutos](portada.gif?classes=center-block)

Cuando comenzamos a programar usualmente implementamos tareas de forma síncrona, donde una tarea se ejecuta detrás de otra de manera secuencial, de modo que el flujo del programa es sencillo y resuelve problemas elementales. Pero, más adelante necesitaremos implementar programas más complejos, donde existirán tareas asíncronas ejecutándose hasta que ocurra un determinado suceso y finalizando en un tiempo desconocido. En efecto, tareas síncronas como asíncronas conviven en nuestros programas por lo que conocer y aplicar correctamente estos conceptos es importante.

Para entender el asincronismo en JavaScript, previamente debemos tener en claro los conceptos siguientes.

## Síncrono, Asíncrono, Paralelismo y Concurrente

Imaginemos que hacemos una cola para ingresar al cine, este es un proceso **síncrono**, mientras hacemos la cola no podemos adelantarnos, tampoco podemos salir de la fila a hacer otra actividad como comprar palomitas o perderíamos nuestro lugar, por lo que debemos esperar nuestro turno hasta que las personas delante nuestro ingresen.

Sin embargo, sucederá algo diferente en un restaurante donde llamamos a un mesero para que atienda nuestra orden, este es un proceso **asíncrono**, los cocineros prepararan nuestros platillos y serán entregados en un futuro inmediato. Sin embargo, al esperar podemos seguir realizando acciones como charlar y beber de modo que esperar la comida no representa una actividad bloqueante.

![Sincrono vs Asincrono](sincronoVsAsincrono.png?classes=center-block)

Por otro lado, necesitaremos ejecutar múltiples tareas todo el tiempo, dependiendo del lenguaje o tecnologías que dispongamos este podría ser paralelo o concurrente, esto último dependerá de la cantidad de medios de ejecución.  

* Paralelismo: cuando dos o más tareas se ejecutan literalmente a la vez en el mismo instante de tiempo.
* Concurrente: cuando dos o más tareas progresan simultáneamente. 

Para comprender mejor lo anterior imaginemos que estamos en el cine, queremos comprar un boleto para ver una película minutos antes de que inicie, pero se tienen dos colas para comprarlos. Existirá **concurrencia** si hay una sola boletería que atiende a las dos colas de personas bajo un criterio determinado. Existiría **paralelismo** si hay dos boleterías que atienden a las dos colas de personas de manera simultánea.

## JavaScript Asíncrono y Concurrente.

**JavaScript es un lenguaje de programación asíncrono, concurrente, no bloqueante bajo un solo hilo de ejecución**. Este concepto tan conocido indica que JavaScript además de ejecutar una secuencia de instrucciones de manera ordenada es capaz de ejecutar tareas que no obtienen un resultado inmediatamente, pudiendo devolver el control al programa sin bloquearlo y todo aquello con un único medio de ejecución.

Con lo señalado anteriormente nos surgen muchas interrogantes, pero básicamente ¿cómo puede un lenguaje de un solo subproceso ser no bloqueante, concurrente y asincrónico? Esto es gracias a la manera en que fue diseñado e implementado el JavaScript Runtime a través del Event Loop, Call Stack, CallBack Queue y Task Queue. En un post anterior ya escribimos al respecto [The Javascript Runtime](https://gonzalohk.xyz/blog/el-javascript-runtime), sería muy importante leerlo.

Ahora bien, entendamos más sobre tareas asíncronas en JavaScript para ello podemos citar algunos ejemplos:
 
* Ejecución de tareas programadas a futuro mediante un setTimeout
* Lectura a archivos
* Petición de recursos a servidores
* Obtención de data mediante una API
* Interacción con el navegador
* Comunicación con los sensores del dispositivo

Tomemos en cuenta que todas estas tareas son acciones que iniciamos ahora, pero finalizan después por lo que se consideran asíncronas. 

JavaScript proporciona tres maneras de implementar asincronismo utilizando Callbacks, Promises y Async/Await.

## Callbacks

Un Callback es una **función que se pasa a otra función como un argumento o parámetro**, que luego se invoca dentro de la función externa para completar algún tipo de rutina o acción. La función que admite una función como parámetro se denominan High-order Function. 
```js
function add(a,b){
    return a + b;
}
function div(a,b){
    return a / b;
}

//high-order function and callback
function calc(a,b,callback){
    return callback(a,b);
}

console.log(calc(1,6,add));
console.log(calc(8,4,div));
```

Esta práctica es más habitual de lo que imaginamos, otro ejemplo puede ser realizar alguna acción luego de presionar un botón.
```js
document.getElementById("btn1").addEventListener("click", function() {
  console.log("has pulsado el botón 1");
});
```
Una gran ventaja de los Callbacks es la compatibilidad con todos los navegadores y una sintaxis relativamente sencilla.  Por otro lado, se tiende a anidar demasiados Callbacks debido al crecimiento natural de nuestros proyectos, provocando un Callback Hell o pirámide que producen código ilegible difícil de mantener. Una buena práctica recomienda no tener más de tres niveles anidados.

![Callback Hell](callbackHell.jpeg?classes=center-block)


## Promises

Una promesa se considera **un objeto especial JavaScript que representa la terminación o el fracaso de una operación asíncrona**. Para tener el concepto algo más claro imaginemos a una promesa como una especie de Karma donde si tú haces algo, en consecuencia obtendrás algo, ahora o en un futuro. Es decir, con las promesas ejecutamos código asíncrono capaz de obtener una respuesta exitosa o fallida en ese instante o en un futuro.

Para la creación de promesas se necesita del constructor denominado Promise que recibe una función Ejecutora. Dicha función recibe dos argumentos Resolve y Reject que nos permiten indicar si la promesa se resolvió o rechazó. 

Así mismo, cuando una promesa es disparada esta ingresa a un estado el cual puede ser:

* Pending: estado inicial, no cumplida y no rechazada.
* Fulfilled: significa que la operación se completó satisfactoriamente.
* Rejected: significa que la operación falló.

Una promesa inicialmente tiene el estado Pending, si se llama a Resolve la promesa pasa a estar Fulfilled, pero si se llama a Reject esta pasa a estar como Rejected. Cuando una promesa es rechazada obtenemos un error que nos indica la razón. En tal sentido, si una promesa se resuelve entonces se ejecuta la función que pasamos al método .then, si la promesa es rechazada entonces se ejecuta la función que pasamos a .catch, de esta forma podemos controlar el flujo de datos.

* Then: en caso de éxito.
* Catch: en caso de fracaso.

Pero veamos lo antes señalado con un pequeño ejemplo.
```js
const somethingWillHappen = () =>{
    return new Promise((resolve,reject) =>{
        if(true) {
            resolve('ok!');
        } else{
            reject('whoops!');
        }
    });
};

somethingWillHappen()
    .then(response => console.log(response))
    .catch(err => console.error(err));
```
Ahora bien, las promesas tienen una sintaxis fácil de comprender incluso en situaciones complejas a diferencia de los Callbacks y mejor aun permitiendo hacer el manejo de errores. Sin embargo, las promesas fueron introducidas con el estándar ES2015 o ES6 lo que significa que no es compatible con todos los navegadores por lo que se necesita de un transpilador para garantizar la compatibilidad de nuestros proyectos.

## Async Await

Es una especificación introducida en ES2017 o ES8 que permite escribir promesas más fácilmente, permitiendo el tratamiento de procesos asíncronos usando las palabras reservadas async/await. En efecto, se considera [Syntax Sugar](https://en.wikipedia.org/wiki/Syntactic_sugar) debido a que en el fondo no es más que una **implementación de Promises que permite escribir código asíncrono en JavaScript como si se tratara de simple código síncrono**.

La palabra clave async se coloca previamente a una función, para definirla como una función asíncrona o async function. Cualquier función definida de este modo puede utilizarse junto a la palabra clave await para manejarla. Es decir, await permite esperar a que se resuelva la promesa, mientras otras tareas puedan seguir realizándose en el programa.
```js
function resolveAfter2Seconds() {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve('resolved');
    }, 2000);
  });
}

async function asyncCall() {
  console.log('calling');
  const result = await resolveAfter2Seconds();
  console.log(result);
}

//Main
console.log('another instruction 1');
asyncCall();
console.log('another instruction 2');
```
Async/Await tiene la gran ventaja de tener una sintaxis simple que además permite el uso de try/catch para el manejo de errores. No obstante, al ser una implementación relativamente reciente necesita de algún transpilador para ser usado en nuestros proyectos.

### Ejemplo - Rick And Morty API

![Rick And Morty API](rickmortyapi.jpg?classes=center-block)

Veamos un ejemplo un poco más real donde queremos consumir un API, de modo tal que tomaremos como ejemplo [https://rickandmortyapi.com/](https://rickandmortyapi.com/), la cual es muy popular para realizar pruebas. Esta API tiene mucha información referente a la serie de Rick And Morty con respecto a personajes, dimensiones y locaciones. 

Por ello, nos planteamos tres ejercicios que requieren consultar la API, donde posiblemente se deban anidar las peticiones.

1. Obtener la lista total de personajes de Rick And Morty, para mostrar el número total de personajes 
2. Obtener la información del primer personaje, para mostrar su nombre 
3. Consultar la ubicación del personaje, para mostrar la dimensión a la cual pertenece

Se hará uso de tres Endpoints para obtener la información requerida, estos son:  


* Obtener todos los personajes
GET https://rickandmortyapi.com/api/character
* Obtener un personaje
GET https://rickandmortyapi.com/api/character/{id}
* Obtener ubicación
GET https://rickandmortyapi.com/api/location/{id}

Para consumir la API mencionada se requiere de peticiones HTTP/HTTPS asíncronas, por lo que utilizaremos XMLHttpRequest (XHR) que es un objeto especial de JavaScript que permite hacer peticiones de forma nativa. Actualmente el estándar es fetch y existen muchas otras librerías para este mismo objetivo, pero XMLHttpRequest será suficiente para nuestros ejercicios. Pues veamos cómo sería la implementación y las diferencias que existen. 

#### Implementación con Callbacks
```js
let XMLHttpRequest = require('xmlhttprequest').XMLHttpRequest;
function fetchData(url_api, callback){
  let xhttp = new XMLHttpRequest();                       // Connection
  xhttp.open('GET', url_api, true);                       // HttpVerb, url, sync
  xhttp.onreadystatechange = (event) => {                 // Call validation
    if(xhttp.readyState === 4){                           // Verify completed request
      if(xhttp.status === 200){                           // verify status code 200
         callback(null, JSON.parse(xhttp.responseText));  // Execute callback and send error and response parameters
      } else {
        let error = new Error('Error: ' + url_api);
        return callback(error, null);                     // Send error
      }
    }
  }
  xhttp.send();
}
```
```js
let API = 'https://rickandmortyapi.com/api/character/';

fetchData(API, (error1, data1) => {
  if(error1) return console.error(error1);  
  fetchData(API + data1.results[0].id, (error2, data2) => {
    if(error2) return console.error(error2);
    fetchData(data2.origin.url, (error3, data3) => {
      if(error3) return console.error(error3);     

      console.log(data1.info.count);
      console.log(data2.name);
      console.log(data3.dimension);          
    });
  });
});
```
#### Implementación con Promises
```js
let XMLHttpRequest = require('xmlhttprequest').XMLHttpRequest;
const fetchData = (url_api) => {
    return new Promise((resolve, reject) => {
        const xhttp = new XMLHttpRequest();                 // Connection
        xhttp.open('GET', url_api, true);                   // HttpVerb, url, sync
        xhttp.onreadystatechange = (() => {                 // Call validation
          if(xhttp.readyState === 4){                       // Verify Completed Request
                (xhttp.status === 200)                      // Verify Status Code 200
                ? resolve(JSON.parse(xhttp.responseText))   // Send Response
                : reject( new Error('Error', url_api))      // Send Error      
          }
        });
        xhttp.send();
    });
}
```
```js
let API = 'https://rickandmortyapi.com/api/character/';
fetchData(API)
    .then(data=>{
        console.log(data.info.count);
        return fetchData(`${API}${data.results[0].id}`)        
    })
    .then(data=>{
        console.log(data.name);
        return fetchData(data.origin.url)
    })
    .then(data=>{
        console.log(data.dimension);
    })
    .catch(err => console.error(err));
```
#### Implementación con Async/Await
```js
let XMLHttpRequest = require('xmlhttprequest').XMLHttpRequest;
const fetchData = (url_api) => {
    return new Promise((resolve, reject) => {
        const xhttp = new XMLHttpRequest();                 // Connection
        xhttp.open('GET', url_api, true);                   // HttpVerb, url, sync
        xhttp.onreadystatechange = (() => {                 // Call validation
          if(xhttp.readyState === 4){                       // Verify Completed Request
                (xhttp.status === 200)                      // Verify Status Code 200
                ? resolve(JSON.parse(xhttp.responseText))   // Send Response
                : reject( new Error('Error', url_api))      // Send Error      
          }
        });
        xhttp.send();
    });
}
```
```js
const API = 'https://rickandmortyapi.com/api/character/';
const rickAsyncData = async (url_api) =>{
    try{
        const data = await fetchData(url_api);
        const character = await fetchData(`${API}/${data.results[0].id}`);
        const origin = await fetchData(character.origin.url);
        console.log(data.info.count);
        console.log(character.name);
        console.log(origin.dimension);  
    } catch(err){
        console.error(err);
    }
}
rickAsyncData(API);
```

JavaScript es un lenguaje sorprendente que permite hacer el manejo de tareas asíncronas con un único hilo de manera concurrente mediante Callbacks, Promises y Async/Await.  Estas son herramientas poderosas, no creo que una sea mejor que la otra o solo se deba utilizar una de ellas debido a que cada una tiene ventajas y desventajas. Es decir, deben ser utilizadas en casos particulares a los que mejor se adecuen siempre con el fin de mejorar la legibilidad y fiabilidad del código cuando ejecutamos tareas asíncronas.

