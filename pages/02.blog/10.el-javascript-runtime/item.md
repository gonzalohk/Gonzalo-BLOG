---
title: 'El JavaScript Runtime'
media_order: 'callstack-stackoverflow.png,callstack-working.png,JSR.png,JSR-2.png,el1.gif,el2.gif,el3.gif,el4.gif,el5.gif,JSRuntimeExampleFinal.gif'
published: true
date: '08-09-2020 12:42'
publish_date: '08-09-2020 12:42'
metadata:
    Keywords: '''JS Runtime'', ''Call Stack JavaScript'',''JavaScript'',''Callback Queue'',''WebAPI Js'',''setTimeout'',''JS Engine'''
taxonomy:
    category:
        - Frontend
        - JS
    tag:
        - JavaScript
        - 'JS Runtime'
        - 'JS Engine'
---

El cómo funciona JavaScript es muy interesante, siendo este denominado un lenguaje **single-threaded non-blocking asynchronous concurrent**, esto significa que la ejecución de un script se hace solo en un hilo, es decir solo va suceder una cosa a la vez. Sin embargo, aún así JavaScript puede ser asíncrono y no bloqueante gracias al Javascript Runtime.

# ¿Qué es el Javascript Runtime Environment?
El JavaScript Runtime proporciona características adicionales a nuestra aplicación en el tiempo de ejecución, siendo básicamente **el responsable de hacer que JavaScript sea asíncrono y no bloqueante**. Para ello, está compuesto del JavaScript Engine, Event Loop, Web APIs, Callback Queue y el Job Queue (desde ES6) donde cada uno tiene una tarea en particular.

![Javascript Runtime Environtment](JSR-2.png?classes=center-block)


## JS Engine
El motor de JavaScript es el encargado de leer, analizar, convertir y ejecutar el código fuente. Este tiene una forma particular de realizar estos procesos, podemos ver más detalles [aquí](http://gonzalohk.xyz/blog/js-engine). 

En tal sentido, el JS Engine tiene dos componentes. 
### Memory Heap
Es donde se realiza la asignación de memoria a variables, funciones, objetos, arrays, etc. 

### Call Stack
Esta es una estructura de datos de pila donde el runtime mantiene un seguimiento de las llamadas a las funciones bajo el modelo LIFO (último en entrar, primero en salir). Recordemos que Javascript es single-threaded por lo tanto solo tiene una pila de llamadas o Call Stack. 

La tarea que realiza es simple, cuando se está a punto de ejecutar una función, esta es añadida al Call Stack, si esta función llama a su vez a otra función, esta es agregada sobre la anterior y así sucesivamente. Esto puede ser mejor entendido con el siguiente ejemplo.
```js
function multiply(x, y) {
    return x * y;
}
function printSquare(x) {
    var s = multiply(x, x);
    console.log(s);
}
printSquare(5);
```
![Callstack working](callstack-working.png?classes=center-block)

El Call Stack tiene un límite por defecto de 16000, pasado este valor se produce un Stack Overflow  que indica que la pila se desbordó y no puede atender tantas funciones apiladas. Los bucles infinitos y funciones recursivas tienden a producir este tipo de errores.
```js
function foo() {    
  foo(); 
} 

foo();
```
![Stack Overflow](callstack-stackoverflow.png?classes=center-block)

Solo con el JS Engine se podría manejar la ejecución del programa, pero no podríamos hablar de asincronismo. Ahí es donde intervienen otros componentes del JS Runtime.

## Web APIs
Son APIs proveídas por el mismo navegador donde JavaScript es el encargado de facilitar su acceso. 

Por ejemplo, JS no puede manipular por sí solo HTML y CSS por lo que debe usar la API DOM que el navegador le ofrece, de la misma forma ocurre con HTTP requests, Timers, Eventos siendo estos lo más relevantes. Sin embargo, existe una amplia lista de [Web APIs](https://developer.mozilla.org/es/docs/Web/API) que podemos utilizar. 

## Event Loop
Es el componente más importante, debido a que tiene el objetivo de ayudar a que el código se ejecute en un solo hilo tanto de forma síncrona como asincrónica. Para ello, realiza dos tareas: 
- Comprobar continuamente si existen tareas pendientes en el Javascript Engine, en otras palabras verificar si hay tareas en el Call Stack.
- Mover las funciones pendientes del Callback Queue y el Job Queue al Call Stack. Si solo si el Call Stack está libre.

## Callback Queue 
Es una estructura de datos cola que almacena todas las Funciones Callback que son requeridas bajo el modelo FIFO (Primero en entrar, primero en salir) de modo tal que mantienen el orden en el que fueron agregadas para luego ser enviadas al Call Stack, posteriormente ser ejecutadas siempre y cuando el Call Stack esté vacío o sin tareas. 

## Job Queue
Desde ES2015 o ES6, fue añadido el Job Queue que también es una estructura de datos cola, es utilizada por las Promises que almacena funciones asíncronas. El Event Loop nuevamente es el encargado de mover las tareas de esta cola al Call Stack siempre y cuando esté vacío. Un detalle importante es que los elementos del Job Queue tienen mayor prioridad que el Callback Queue.

# ¿Cómo funciona el Javascript Runtime?
Ahora que ya conocemos los componentes del JS Runtime, veamos cómo es que los componentes interactúan y hace posible que nuestro código se ejecute de forma organizada y asíncrona. 

Veamos una vez más un ejemplo y animaciones hechas por [Lydia Hallie](https://twitter.com/lydiahallie). Tenemos dos funciones simples que son ejecutadas, una de ellas greet() que devuelve un saludo y respond() que también retorna un saludo luego de 1 segundo.
```js
function greet(){
console.log("Hello!")
}

function respond(){
return setTimeout(()=> {
console.log("Hey!")
}, 1000)
}

greet()
respond()
```
Veamos paso a paso qué sucede.
#### Paso 1
El código fuente es analizado y las funciones que son invocadas son enviadas al Call Stack. greet() es ejecutado inmediatamente debido a que en su interior no invoca a otras funciones mientras que respond() tiene una función dentro que también es posicionada en la pila.

![Callstack](el1.gif?classes=center-block)

#### Paso 2
Nuestro ejemplo tiene un setTimeout, este ejecuta una función luego que expire el temporizador, siendo este un timer se considera un Web API. Veamos que este se ejecuta e  inmediatamente sale del Call Stack, pero el Web API se hace cargo de esperar el temporizador.

![WebAPI](el2.gif?classes=center-block)

#### Paso 3 
Cuando el timer finaliza, la función Callback que fue definida dentro del setTimeout es movida al Callback Queue

![Callback Queue](el3.gif?classes=center-block)

#### Paso 4
El Event Loop que está siempre atento, se da cuenta que el Call Stack está vacío y existe una tarea en el Callback Queue, por tanto el Event Loop mueve la función mencionada al Call Stack.

![Callback Queue -> Call Stack](el4.gif?classes=center-block)

#### Paso 5
Inmediatamente la función dentro del Call Stack es ejecutada.

![Call Stack execution](el5.gif?classes=center-block)

Ahora veamos un ejemplo y su funcionamiento de manera completa. Este ejemplo fue presentado por Philip Roberts en la JSConf del 2014, creo que hasta hoy es el mejor recurso en internet para entender cómo funciona realmente el JS Runtime, puedes ver la charla [aquí](https://www.youtube.com/watch?v=8aGhZQkoFbQ)  

El objetivo de este ejemplo es mostrar la ejecución del código fuente utilizando un Timer y un Evento, ambos tiene Funciones CallBack que serán ejecutadas de forma asincrona. La herramienta que nos ayudará a ver graficamente como nuestro codigo se ejecuta se llama [loupe](http://latentflip.com/loupe/?code=JC5vbignYnV0dG9uJywgJ2NsaWNrJywgZnVuY3Rpb24gb25DbGljaygpIHsKICAgIHNldFRpbWVvdXQoZnVuY3Rpb24gdGltZXIoKSB7CiAgICAgICAgY29uc29sZS5sb2coJ1lvdSBjbGlja2VkIHRoZSBidXR0b24hJyk7ICAgIAogICAgfSwgMjAwMCk7Cn0pOwoKY29uc29sZS5sb2coIkhpISIpOwoKc2V0VGltZW91dChmdW5jdGlvbiB0aW1lb3V0KCkgewogICAgY29uc29sZS5sb2coIkNsaWNrIHRoZSBidXR0b24hIik7Cn0sIDUwMDApOwoKY29uc29sZS5sb2coIldlbGNvbWUgdG8gbG91cGUuIik7!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D)
```js
$.on('button', 'click', function onClick() {
    setTimeout(function timer() {
        console.log('You clicked the button!');    
    }, 2000);
});

console.log("Hi!");

setTimeout(function timeout() {
    console.log("Click the button!");
}, 5000);

console.log("Welcome to loupe.");
```
![JS Runtime Example](JSRuntimeExampleFinal.gif?classes=center-block)

En una primera instancia se observa que el código es leído y analizado de arriba a abajo, de modo tal que se ejecuta inmediatamente, pero las funciones se mueven al Call Stack.

Las función evento y el timer son responsabilidad del Web API. En el caso del onClick() que responde al evento no es ejecutada aún, pero si está a la espera que se lance el evento. Mientras la función timeout() que responde al timer, espera 5 segundos y es movida al Callback Queue para luego ser llevada al Call Stack mediante el Event Loop y posteriormente ser ejecutada.

Por otro lado, cualquier momento en el que se haga click al botón, el onClick() es movido al Callback Queue y luego pasado al Call Stack para ser ejecutado inmediatamente siempre y cuando esté vacío. 

Evidenciamos que JS funciona de una manera muy particular y cómo necesita e interactúa de forma constante con el navegador mediante las Web APIs. Así mismo, el JS Engine y el JS Runtime en su conjunto realizan algo increíble, que es la ejecución síncrona y asíncrona de código fuente solo con un hilo (monoproceso) a diferencia de otros lenguajes de programación  tradicionales. Sin embargo, ahora que ya entendemos el cómo funciona es responsabilidad de cada uno optimizar nuestro código para obtener una mejor performance y sobretodo evitar código bloqueante en tiempo de ejecución. 

