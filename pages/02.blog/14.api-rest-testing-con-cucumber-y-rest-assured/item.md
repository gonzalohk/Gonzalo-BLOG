---
title: 'API REST Testing con Cucumber y Rest-Assured'
---

Probar y validar servicios API Rest en Java puede ser complejo si no se usan las herramientas adecuadas más aún si estas son automatizadas. En efecto, dichas pruebas se centran en los request y responses que se generan, estos deben probar una serie de aspectos como ser:

* El código de respuesta HTTP (response code)
* Las cabeceras HTTP de la respuesta
* El contenido de la respuesta en JSON o XML (payload)

Quizá no estemos familiarizados con estos conceptos aún por lo que podemos definirlos.

## ¿Que es API Rest?

Un API (Application Programming Interface) es un conjunto de reglas y protocolos que permiten la comunicación entre aplicaciones software. Ahora bien, REST (Representational State Transfer) se originó en el año 2000 fue descrito en la tesis de Roy Fielding. Es efecto, API Rest es una interfaz de programación de aplicaciones que se ajusta al estilo de arquitectura REST.

En otras palabras, API REST define un conjunto de funciones para realizar solicitudes (Request) y recibir respuestas (Responses) a través del protocolo HTTP bajo una arquitectura cliente servidor,. 

En efecto, API REST utiliza los métodos HTTP (HTTP verbs) para realizar las peticiones, cada uno con una función particular siendo estos:

* GET: es utilizado únicamente para consultar información.
* POST: usado para solicitar la creación de un nuevo registro (soporta payload).
* PUT: se emplea para actualizar por completo un registro existente (soporta payload)
* PATCH: similar al método PUT, permite actualizar solo un fragmento del registro y no su totalidad (soporta payload)
* DELETE: se lo usa para eliminar un registro existente
* HEAD: utilizado para obtener información sobre un determinado recurso sin retornar el registro.
* OPTIONS: permite describir las opciones de comunicación para el recurso de destino

Los códigos de estado HTTP (HTTP Status code) describen de forma abreviada la respuesta obtenida, cada uno indica un estado en particular, pero de forma general estos son:

* 1XX Respuestas informativas
* 2XX Peticiones correctas
* 3XX Redirecciones
* 4XX Errores del cliente
* 5XX Errores de servidor


## ¿Que es Cucumber?
Es una herramienta que permite ejecutar descripciones funcionales en texto plano como pruebas de software automatizados. Cucumber ayuda a implementar la metodología Behaviour Driven Development (BDD o Desarrollo Dirigido por Comportamiento) donde primero se escriben las pruebas antes que el código fuente.

En tal sentido, el lenguaje usado por Cucumber para estas descripciones funcionales se denomina Gherkin. 

## ¿Que es Gherkin?
Se define como un Lenguaje Específico de Dominio, que puede ser entendido por humanos y por ordenadores. 

Para definir descripciones funcionales se necesitan de 5 palabras reservadas para construir nuestras pruebas :

* Feature: Indica el nombre de forma clara de la funcionalidad que vamos a probar. Además se Incluye una descripción en forma de historia de usuario “Como [rol] quiero [característica] para que [los beneficios]”. 
* Scenario: Describe cada escenario que vamos a probar, se deben probar distintos escenarios para una misma funcionalidad.
* Given: Provee contexto para el escenario como el punto donde se ejecuta el test y pre requisitos en los datos.
* When: Especifica el conjunto de acciones que lanzan el test y la interacción del usuario que acciona la funcionalidad que deseamos probar.
* Then: Especifica el resultado esperado en el test.

## ¿Rest - Assured?

Rest-Assured es un Java DSL (Domain Specific Language) construido sobre la capa HTTP Builder que permite simplificar la construcción de test sobre una API REST. Es decir, tiene una sintaxis DSL que sigue el formato Given – When – Then como Gherkin.

Así mismo, se considera un framework que fue desarrollado por Jayway que tiene características bastante útiles como validación Xpath, JsonPath, MIME-Types, file upload, verificación de cabeceras, respuestas, cookies y muchas otras opciones.  

## EJEMPLO
Para que lo expuesto no se quede en teórica hagamos un ejemplo probando, verificando un API Rest público, utilizando las herramientas y metodología mencionada. 

#### Precondiciones 
Primeramente descargamos e instalamos Java y Gradle como gestor de dependencias.

* Java 11 - [Descargar](http://jdk.java.net/java-se-ri/11)
* Gradle 5.6.4 -  [Descargar](https://gradle.org/releases/)

Por otro lado, necesitamos de una cliente capaz de hacer peticiones al API Rest como Postman con el fin de obtener la mayor información posible. Sin embargo, es importante mencionar que Postman es mucho más que un cliente rest, pero nos podría servir para hacer el análisis de nuestro API a probar. 

* Postman - [Descargar](https://www.postman.com/)

Adicionalmente podemos usar el IDE de nuestra preferencia, pero Intellij IDEA en su versión gratuita tiene lo suficiente para llevar a cabo nuestras pruebas. 

* IntelliJ IDEA 2020.2.2 Community - [Descargar](https://www.jetbrains.com/es-es/idea/download/#section=windows)

Usaremos Cucumber con Gherkin para realizar los test, por lo que instalaremos en el IDE dos plugins que nos ayudarán a formatear código Gherkin y generar código java.

* Gherkin
* Cucumber for Java

Así mismo, se deben adicionar variables de entorno para Java y Gradle. Por ello, se deberá tener una configuración de la siguiente manera donde en el caso de Windows se vería de esta forma.

* JAVA_HOME=C:\Program Files\Java\jdk-11.0.8
* GRADLE_HOME =C:\gradle-5.6.4
* Path
** C:\gradle-5.6.4\bin

Posterior a ello, verificamos que podamos ejecutar en la consola los comandos siguientes.

#### dummy.restapiexample.com y su API Rest

Para hacer nuestras pruebas, utilizaremos restapiexample.com la cual proporciona un API Rest público de prueba que nos permitirá registrar, actualizar, listar y eliminar empleados.

* http://dummy.restapiexample.com/

En efecto, esta indica que los employees pueden ser manejados de la siguiente forma.

* Get all employees
* Get employee by Id
* Create new employee
* Update employee by Id
* Delete employeeItem by Id

En una primera instancia probaremos estas APIs con Postman, para ello creamos una nueva colección, seguidamente vamos añadiendo request por request incluyendo los detalles de la autenticación y creación de variables si fuera necesario. 

**a) Listar empleados**

URL: http://dummy.restapiexample.com/api/v1/employees

Formato: json

Metodo HTTP: GET

Requiere autenticación: No

Payload: Ninguno

URL: http://dummy.restapiexample.com/api/v1/employees

Response:
```json
{
"status": "success",
"data": [
	{
	"id": "1",
	"employee_name": "Tiger Nixon",
	"employee_salary": "320800",
	"employee_age": "61",
	"profile_image": ""
	},
	....
	]
}
```
**b) Listar empleado**

URL: http://dummy.restapiexample.com/api/v1/employee/{id}

Formatos: json

Metodo HTTP: GET

Requiere autenticación: No

Payload: Ninguno

URL: http://dummy.restapiexample.com/api/v1/employee/1

Response:
```json
{
    "status": "success",
    "data": {
        "id": "1",
        "employee_name": "Tiger Nixon",
        "employee_salary": "320800",
        "employee_age": "61",
        "profile_image": ""
	}
}
```

**c) Crear Empleado**

URL: http://dummy.restapiexample.com/api/v1/create

Formatos: json

Metodo HTTP: POST

Requiere autenticación: No

Payload: Se requiere un Employee Object como parámetro de entrada, pero no requiere todos los campos. 

URL: http://dummy.restapiexample.com/api/v1/create

Content:
```json
{
    "name":"Gustavo",
    "salary":"5000",
    "age":"29"
}
```
Response:
```json
{
    "status": "success",
    "data": {
        "name": "Gustavo",
        "salary": "5000",
        "age": "29",
        "id": 25
    }
}
```
**d) Actualizar Empleado**
URL: http://dummy.restapiexample.com/api/v1/update/{id}

Formatos: json

Metodo HTTP: PUT

Requiere autenticación: No

Payload: Se requiere un Employee Object como parámetro de entrada, pero no requiere todos los campos. 

URL: http://dummy.restapiexample.com/api/v1/update/21

Content:
```json
{
    "name":"Gustavo (Updated)",
    "salary":"8000",
    "age":"30"
}
```
Response
```json
{
    "status": "success",
    "data": {
        "name": "Gustavo (Updated)",
        "salary": "8000",
        "age": "30",
        "id": 25
    }
}
```
**e) Eliminar Item**
URL: http://dummy.restapiexample.com/api/v1/delete/{id}

Formatos: xml, json

Metodo HTTP: DELETE

Requiere autenticación: No

Parámetros: Ninguno

URL: http://dummy.restapiexample.com/api/v1/delete/2

Response:
```json
{
    "status": "success",
    "message": "successfully! deleted Records"
}
```

Por ello, existen variedad de herramientas siendo una buena opción es Rest-Assured que permite simplificar la construcción de los pruebas API Rest, permitiendo la fácil manipulación de los endpoints. Por otro lado, sería muy interesante usar una metodología BDD usando Cucumber para ejecutar descripciones funcionales de pruebas en texto plano en lenguaje Gherkin, de modo tal que sea entendible para cualquier persona incluso sin conocimiento técnico. Así mismo, podríamos usar JUnit como test engine base para hacer nuestras verificaciones finales.