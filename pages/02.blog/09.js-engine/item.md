---
title: 'El JavaScript Engine'
published: true
date: '24-08-2020 23:10'
publish_date: '24-08-2020 23:10'
taxonomy:
    category:
        - JS
        - Frontend
    tag:
        - JavaScript
        - 'JS Engine'
---

JS es una de los lenguajes más populares de los últimos tiempos debido a la gran cantidad de frameworks, librerías y nuevas características que se van implementando e impulsada por su enorme comunidad. En efecto, JS casi siempre está presente en el stack de desarrollo de aplicaciones modernas en todo tipo de proyectos a diferentes escalas y muy probablemente en nuestros propios proyectos. Sin embargo, pocas veces nos preguntamos cómo es que JS se ejecuta e interactúa con los browsers, siendo el JavaScript Engine el encargado de realizar este proceso de forma muy interesante. 

## ¿Qué es el JavaScript Engine?  

También conocido como motor JavaScript, es un intérprete informático que interpreta el código fuente en lenguaje JavaScript y ejecuta un script acorde a las instrucciones dadas. Es decir, **el motor es la parte encargada de leer y ejecutar el código fuente**.

Existe una variedad de engines, cada navegador tiende a tener su propio engine siendo los más relevantes: 
* V8 empleado en Chrome
* SpiderMonkey empleado en Mozilla 
* JavaScriptCore empleado en Safari

V8 es de los más potentes motores debido a que no solo es usado y desarrollado por Chrome también fue adoptado por NodeJS, Electron, MarkLogic, Couchbase, entre muchos otros incluyendo a Microsoft Edge.

Los JS engines tienen un funcionamiento similar, donde el navegador descarga los archivos fuente de JavaScript, luego se interpretan las instrucciones del código y se realiza un proceso en tiempo real que se denomina Just-In-Time Compiler (JIT Compiler) en dónde convierte el código fuente en algo que sí pueden entender las computadoras como lo el el machine code..

## Proceso De Compilación
Tomando como ejemplo V8,  este pasa nuestro código JS hasta poder ser ejecutado por el computador, para ello:
•	Recibe el código fuente
•	Parsea el código y produce un Abstract Syntax Tree (AST)
•	Se compila a bytecode y se ejecuta
•	Se optimiza a machine code y se reemplaza el código base

Pero veamos más a detalle este proceso en 5 pasos con ayuda de unas animaciones hechas por Lydia Hallie..

#### Paso 1
El código fuente es procesado como UTF-16 byte stream y pasado al byte stream decoder para ser procesado.
  
#### Paso 2
El byte stream decoder decodifica los bytes, para que el parser se descomponga y convierta las instrucciones de JavaScript en una lista de tokens (fichas).

El parser analiza el programa verificando que se siguen las reglas del lenguaje de programación, si encuentra errores como partes de código que no forman parte de la sintaxis del lenguaje ocurre un Syntax Error. 

#### Paso 3
En función a los tokens creados, el parser genera nodos para producir un Abstract Syntax Tree (AST), es decir las instrucciones iniciales se transforman en una representación en árbol de la estructura sintáctica de nuestro código.

Abstract Syntax Tree (AST) es una estructura de datos ampliamente usada por compiladores, debido a su propiedad de representar la estructura del código del programa en un grafo. Es decir, es el resultado del análisis de la sintaxis de un compilador.

#### Paso 4
El intérprete llamado Ignition recorre el AST generado para generar el bytecode. 

El intérprete tiene dos roles: generar bytecode e interpretar este bytecode para poder ejecutarlo. En este punto el bytecode generado por Ignition sí es ejecutable en el computador, pero aún falta un paso muy importante

#### Paso 5
El bytecode aún tiene que ser optimizado, V8 cuenta con un compilador llamado TurboFan, que se encargará de este problema el cual genera machine code altamente optimizado. 

TurboFan tiene como tarea principal tomar las instrucciones retornadas por el intérprete Ignition para compilarlo para la arquitectura de destino. Es decir, según la arquitectura de destino Turbofan elige las instrucciones adecuadas de bajo nivel para la arquitectura destino, esto  resulta en una ejecución altamente optimizada del código JavaScript.


