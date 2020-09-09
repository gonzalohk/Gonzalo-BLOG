---
title: 'El JavaScript Engine'
media_order: 'js1.gif,js2.gif,js3.gif,js4.gif,js5.gif,jsEngine.png,howV8Works.png'
published: true
date: '24-08-2020 23:10'
publish_date: '24-08-2020 23:10'
metadata:
    Keywords: '"JS, Javascrip Engine, V8, Ignition, Turbofun"'
taxonomy:
    category:
        - Frontend
        - JS
    tag:
        - JavaScript
        - 'JS Engine'
---

![JsEngine](jsEngine.png?classes=center-block)

JS es una de los lenguajes más populares de los últimos tiempos debido a la gran cantidad de frameworks, librerías y nuevas características que se van implementando e impulsada por su enorme comunidad. En efecto, JS casi siempre está presente en el stack de desarrollo de aplicaciones modernas en todo tipo de proyectos a diferentes escalas y muy probablemente en nuestros propios proyectos. Sin embargo, pocas veces nos preguntamos cómo es que JS se ejecuta e interactúa con los browsers, siendo el JavaScript Engine el encargado de realizar este proceso de forma muy interesante. 

## ¿Qué es el JavaScript Engine?  

También conocido como motor JavaScript, es el software que interpreta el código JavaScript y que a su vez, ejecuta un script acorde a las instrucciones dadas. Es decir, **el motor es la parte encargada de leer y ejecutar el código fuente**. Todos los navegadores web tienen un motor JavaScript.

Existe una variedad de engines, cada navegador tiende a tener su propio engine, siendo los más relevantes: 
* V8 en Chrome
* SpiderMonkey en Mozilla Firefox
* JavaScriptCore en Safari

V8 es de los más potentes motores debido a que no solo es usado y desarrollado por Chrome también fue adoptado por NodeJS, Electron, MarkLogic, Couchbase, entre muchos otros incluyendo a Microsoft Edge.

Los JS engines tienen un funcionamiento similar, donde el navegador descarga los archivos fuente de JavaScript, luego se interpretan las instrucciones del código y se realiza un proceso en tiempo real que se denomina [Just-In-Time Compiler](https://www.freecodecamp.org/news/just-in-time-compilation-explained/) (JIT Compiler) en dónde convierte el código fuente en algo que sí pueden entender las computadoras como lo es el machine code.

## ¿Cómo funciona V8?
Tomando como ejemplo V8,  este procesa nuestro código JS hasta poder ser ejecutado por el computador, para ello:
* Recibe el código fuente.
* Parsea el código y produce un Abstract Syntax Tree (AST).
* Se compila a bytecode y se ejecuta.
* Se optimiza a machine code y se reemplaza el código base.

Pero veamos más a detalle este proceso en 5 pasos con ayuda de unas animaciones muy bien hechas por [Lydia Hallie](https://twitter.com/lydiahallie).

#### Paso 1
El código fuente es enviado como UTF-16 byte stream y pasado al byte stream decoder para ser procesado.

![Carga del código fuente](js1.gif?classes=center-block)

#### Paso 2
El byte stream decoder decodifica los bytes, para que el parser descomponga y convierta las instrucciones de JavaScript en una lista de tokens (fichas).

![Parseado](js2.gif?classes=center-block)

El parser analiza el programa verificando que se siguen las reglas del lenguaje de programación, si encuentra errores como partes de código que no forman parte de la sintaxis del lenguaje ocurre un Syntax Error.

Para entender mejor la descomposicion en tokens, podemos visitar [https://esprima.org/demo/parse.html](https://esprima.org/demo/parse.html#)

#### Paso 3
En función a los tokens creados, el parser genera nodos para producir un Abstract Syntax Tree (AST), es decir las instrucciones iniciales se transforman en una representación en árbol de la estructura sintáctica de nuestro código.

![Tokens -> AST](js3.gif?classes=center-block)

Abstract Syntax Tree (AST) es una estructura de datos ampliamente usada por compiladores, debido a su propiedad de representar la estructura del código del programa en un grafo. Es decir, es el resultado del análisis de la sintaxis de un compilador.

Imaginar como estos nodos están enlazados es difícil, así es que podemos ver esto de mejor manera en [https://astexplorer.net/](https://astexplorer.net/).

#### Paso 4
El intérprete llamado [Ignition](https://v8.dev/docs/ignition) recorre el AST para generar el bytecode. 

![AST -> Bytecode](js4.gif?classes=center-block)

El intérprete tiene dos roles: generar bytecode e interpretar este bytecode para poder ejecutarlo. En este punto el bytecode generado por Ignition sí es ejecutable en el computador, pero aún falta un paso muy importante

#### Paso 5
El bytecode aún tiene que ser optimizado, V8 cuenta con un compilador llamado [TurboFan](https://v8.dev/docs/turbofan), que se encargará de este problema, el cual genera machine code altamente optimizado. 

![Bytecode -> MachineCode](js5.gif?classes=center-block)

TurboFan tiene como tarea principal tomar las instrucciones retornadas por el intérprete Ignition para compilarlo a la arquitectura destino. Es decir, según la arquitectura destino Turbofan elige las instrucciones adecuadas de bajo nivel, esto  resulta en una ejecución altamente optimizada del código JavaScript.

Para resumir el funcionamiento de un JS Engine o más propiamente V8, veamos el siguiente gráfico.

![V8](howV8Works.png?classes=center-block)

Sin duda, JS con el paso del tiempo se consolida en un lenguaje robusto capaz de ser usado en diferentes contextos de una forma muy optima a pesar de ser single-threaded o contar solo con un call stack. Así mismo, es importante señalar que JS se cataloga como un lenguaje interpretado, pero luego de analizar el como funciona nos damos cuenta que también realiza un proceso de compilación, de modo tal que obtiene lo mejor de ambos mundos para tener código altamente optimizado.

