---
title: 'API REST Testing con Postman'
media_order: 'apitestbanner.gif,01-create-account.png,02-create-workspace.png,03-swagger.png,04-pets.png,05-workspace.png,06-create-collection.png,07-addRequest.png,08-saveRequest.png,09-addingRequest.png,10-saving-var.png,11-testingandsavevariables.png,12-savevariables.png,13-get.png,14-get-test.png,15-delete.png,15-delete-test.png,16-varglobal-1.png,16-varglobal-2.png,16-varglobal-3.png,16-variablesglobales.png,17-generate-doc.png,17-generate-saveResponse.png,18-docupublished.png,20-export.png,20-export-2.png,20-export-3.png,20-export-globals.png,apirest.png,19-runner-run.png,19-runner-summary.png,19-runner-config.png,21-running-newman.png'
date: '18-11-2020 14:41'
publish_date: '18-11-2020 14:41'
metadata:
    '"Keywords"': '"Automation","QA Automation","API Rest Testing","API Test Postman","Postman"'
taxonomy:
    category:
        - QA
        - 'QA Automation'
    tag:
        - 'API Rest'
        - Postman
---

![API REST Testing con Postman](apitestbanner.gif?classes=center-block)

## ¿Qué es POSTMAN ?


Es una **herramienta que permite realizar peticiones HTTP a cualquier API**. Es decir, permite probar, consumir y depurar **REST, SOAP y GraphQL.**  

Postman es una herramienta útil en el desarrollo permitiendo consumir y comprobar el correcto funcionamiento de los servicios que se llevan implementado. En tal sentido, más allá de las funcionalidades mencionadas, este permite organizar las peticiones en colecciones, automatizar pruebas, generar documentación e incluso crear mocks de APIs. 

Sin embargo, Postman tiene una versión de pago con funcionalidades como la monitorización, trabajo sincronizado en equipo, mayor límite de vistas a la documentación, etc que pueden ser muy bien explotadas. Describamos algunas de sus características:

* **Colecciones** 
Postman permite guardar/agrupar un conjunto de solicitudes (request), que se denominan Collections. Estas son carpetas en distintos niveles que organizan nuestras peticiones HTTP.

* **Variables de entorno y globales** 
Las variables nos ayudarán a guardar, recordar un valor para evitar escribir el mismo texto/ruta de forma repetitiva. Por lo que puede ser usado en los request y pruebas posteriores. Así mismo, estas pueden tener un ámbito global o de entorno. 

* **Documentación de APIs** 
Postman genera documentación de forma automática, utilizando la información de las peticiones y las descripciones introducidas. Esta puede ser pública o privada, teniendo limitantes en la versión gratuita. 

* **Automatización de pruebas**
Postman permite ejecutar y validar un conjunto de pruebas de forma automatizada utilizando un Collection Runner. Una vez ejecutado, se mostrará el resultado resumen, indicando el tiempo de respuesta y el estado HTTP devuelto por el servidor. Incluso pueden adicionarse add-ons como Newman, para ejecutar los test en línea de comandos permitiendo trabajar con Jenkins, Travis y Docker.

## ¿Qué es API Rest?

![REST](apirest.png?classes=center-block)

API REST define un **conjunto de funciones para realizar solicitudes (Request) y recibir respuestas (Responses)** a través del protocolo **HTTP** bajo una arquitectura **cliente servidor.**

En efecto, API REST utiliza los métodos HTTP (HTTP verbs) para realizar las peticiones, cada uno tiene una función en particular.
 
* GET solicitar datos o algún recurso.
* HEAD traer headers para obtener información sobre un determinado recurso 
* POST enviar datos a un recurso para la creación.
* PUT reemplazar por completo un recurso.
* PATCH reemplazar parcialmente un recurso.
* DELETE eliminar un recurso.

Los códigos de estado HTTP (HTTP Status Code) describen de forma abreviada la respuesta obtenida en las peticiones. 

* 1XX Respuestas informativas
* 2XX Peticiones correctas
* 3XX Redirecciones
* 4XX Errores del cliente
* 5XX Errores del servidor

### Instalación

Inicialmente Postman fue ideado como una extensión Chrome que aún sigue disponible pero ya está deprecada debido a que cuenta con versiones nativas para Windows, Linux y MacOS. En efecto, debemos descargarlo e instalarlo desde: 

* [Postman Instalador](https://www.postman.com/downloads/)

Postman es gratuito, pero ofrece planes adicionales como Postman Pro con más ancho de banda para las pruebas y Postman Enterprise que puede integrar la herramienta en los sistemas de SSO de nuestra empresa, ambos permiten mayor número de visualizaciones de la documentación publicada de nuestros proyectos. 

![Postman Crear Cuenta](01-create-account.png?classes=center-block)

Luego de instalarlo, puede usarse únicamente de forma local, pero se recomienda crear una cuenta de usuario para gestionar nuestros request, sincronizarlos y guardarlos en la nube para usarlos más adelante. Así mismo, se pueden crear en equipos de trabajo que permitirán a varios usuarios ver/editar colecciones, request en un mismo workspace compartido.

### PetStore Swagger
Para realizar las pruebas, utilizaremos un API público ejemplo proporcionado por Swagger que se denomina [petstore.swagger.io](https://petstore.swagger.io/). Este no presenta autenticación aunque puede ser configurada si fuera necesario.

![Swagger PetStore](03-swagger.png?classes=center-block)


![Swagger PetStore PetAPI](04-pets.png?classes=center-block)


En efecto, nos permitirá registrar, actualizar, listar, eliminar usuarios y mascotas. Por otro lado, también permite gestionar las órdenes de compra de mascotas. Para las pruebas nos enfocaremos en las mascotas y particularmente en tres de sus servicios.

* Adicionar nueva mascota (POST /pet)
* Obtener mascota por Id (GET /pet/{petId})
* Eliminar mascota por Id (DELETE /pet/{petId})

Antes de implementar e iniciar cualquier prueba es fundamental explorar el API proporcionado con el fin de tener la mayor información posible.

###### ADICIONAR NUEVA MASCOTA

* URL: http://petstore.swagger.io/v2/pet
* Formato: json
* Método HTTP: POST
* Requiere autenticación: No
* Payload: Objeto Mascota
```sh
POST https://petstore.swagger.io/v2/pet
```
Payload 
```json
{
  "id": 0,
  "category": {
    "id": 0,
    "name": "string"
  },
  "name": "doggie",
  "photoUrls": [
    "string"
  ],
  "tags": [
    {
      "id": 0,
      "name": "string"
    }
  ],
  "status": "available"
}
```
Response
```js
{
    "id": 9222968140491081190,
    "category": {
        "id": 0,
        "name": "string"
    },
    "name": "doggie",
    "photoUrls": [
        "string"
    ],
    "tags": [
        {
            "id": 0,
            "name": "string"
        }
    ],
    "status": "available"
}
```

###### OBTENER MASCOTA POR ID

* URL: https://petstore.swagger.io/v2/pet/{petId}
* Formatos: json
* Método HTTP: GET
* Requiere autenticación: No
* Payload: Ninguno
```sh
GET https://petstore.swagger.io/v2/pet/9222968140491081005
```
Response
```json
{
    "id": 9222968140491081005,
    "category": {
        "id": 0,
        "name": "string"
    },
    "name": "doggie",
    "photoUrls": [
        "string"
    ],
    "tags": [
        {
            "id": 0,
            "name": "string"
        }
    ],
    "status": "available"
}
```
###### ELIMINAR MASCOTA POR ID

* URL: https://petstore.swagger.io/v2/pet/{petId}
* Formatos: json
* Método HTTP: DELETE
* Requiere autenticación: No
* Payload: Ninguno
```sh
DELETE https://petstore.swagger.io/v2/pet/9222968140491081004
```
Response
```json
{
    "code": 200,
    "type": "unknown",
    "message": "9222968140491081004"
}
```
### Implementando Pruebas

Ahora que se conoce la API a ser evaluada, iniciamos Postman seleccionando el workspace de nuestra preferencia donde crearemos las colecciones y peticiones. En este ejemplo, se utilizará el workspace compartido del equipo para que varios usuarios puedan trabajar en el mismo proyecto.

![Team Workspace](02-create-workspace.png?classes=center-block)

Creamos una nueva colección que agrupara todas las peticiones necesarias para realizar nuestras pruebas. 

![Crear Collection](06-create-collection.png?classes=center-block)

PETSTORE-Collection agrupará las peticiones mencionadas anteriormente, nos dirigimos a la opción Add Requests.

![Adicionar Request](07-addRequest.png?classes=center-block)

##### Request - ADD PET
Para crear el nuevo request, introducimos el nombre que tendrá junto a una descripción del mismo. La descripción es muy importante porque mientras más detallada/completa sea mejor será la documentación generada.

![POST Request ADD PET](08-saveRequest.png?classes=center-block)

Posteriormente, se agrega la URL de la petición, indicamos el formato que tiene nuestro payload (en nuestro caso raw - JSON) y adicionamos el body (Pet Object). Una vez llenados correctamente estos campos podemos enviar la petición, en la parte inferior se puede ver el response generado junto a detalles del mismo como el StatusCode, tiempo de ejecución y tamaño.

![POST Request ADD PET - creacion](09-addingRequest.png?classes=center-block)

Adicionalmente realizamos las pruebas pertinentes, comprobando el StatusCode obtenido, el tiempo de respuesta y evaluando valores del response. Es decir, se implementarán tres pruebas:

* Status code is 200
* Response time is less than 200ms
* Body matches string

```js
let responseBigIntsChangedToStrings = pm.response.text().replace(/([^"\w-])([-0-9.]{8,100})([^"\w-])/g, '$1\"$2\"$3');
let jsonResponse = [...responseBigIntsChangedToStrings.matchAll(/{.*}/g)];
let jsonData = JSON.parse(jsonResponse);

// Saving environment variables
pm.environment.set("idPetToTest", jsonData.id);
pm.environment.set("namePetToTest", jsonData.name);

// Testing ADD PET
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});
pm.test("Response time is less than 200ms", function () {
    pm.expect(pm.response.responseTime).to.be.below(200);
});
pm.test("Body matches string", function () {
    pm.expect(jsonData.name).to.eql("chiki");
    pm.expect(jsonData.status).to.eql("available");
    pm.expect(jsonData.category.name).to.eql("cánidos");
});
```
En tal sentido, en la pestaña Tests se adiciona el código implementado con anterioridad. Este fue realizado en JS, pero Postman permite realizar el mismo en una variedad de lenguajes según la preferencia de cada uno, el resultado será el mismo.

![ADD Pet variables y pruebas](11-testingandsavevariables.png?classes=center-block)

Es importante notar que almacenamos dos variables de entorno que será usadas en las peticiones posteriores debido a que estas son dinámicas y el utilizar datos estáticos no servirían para automatizar dichas pruebas. 

* idPetToTest
* namePetToTest

También se hace un pequeño artificio para convertir enteros gigantes a cadena y no existan problemas posteriores.

![ADD Pet Variables y pruebas 2](12-savevariables.png?classes=center-block)

##### Request - GET PET
Para crear un nuevo request indicamos el nombre e ingresamos una descripción. Luego adicionamos la URL, se debe notar que ahora se hace uso de variables de entorno como {{idPetToTest}} que pueden ser concatenados fácilmente no solo al Endpoint también puede adicionarse al body y al código de las pruebas. Todo ello con el objetivo de hacer pruebas más robustas y mantenibles.

![GET Pet Request](13-get.png?classes=center-block)

En este caso se validará
* Status code is 200
* Response time is less than 200ms
* Body matches string

```js
let jsonData = pm.response.json();

// Testing GET PET
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});
pm.test("Response time is less than 200ms", function () {
    pm.expect(pm.response.responseTime).to.be.below(200);
});
pm.test("Body matches string", function () {
    pm.expect(pm.response.text()).to.include(pm.variables.get("namePetToTest"));
});
```
En esta ocasión no fue necesario almacenar variables por lo que hacen las pruebas directamente.

![GET Pet Test](14-get-test.png?classes=center-block)

##### Request - DELETE PET
Creamos el último request, donde detallamos la URL de forma similar al anterior request pero con un Https Verb diferente denominado DELETE.

![DELETE Pet Request](15-delete.png?classes=center-block)

En este caso se validará.
* Status code is 200
* Response time is less than 200ms

```js
// Testing DELETE PET
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});
pm.test("Response time is less than 200ms", function () {
    pm.expect(pm.response.responseTime).to.be.below(200);
});
```
Como se puede visualizar, la prueba funciona correctamente, pero en caso de esta fallar esta muestra un error en la parte inferior y en la consola de Postman. En nuestro caso, el request tarda más de 200ms por lo que no cumple el assert y representa un test fallido.

![DELETE Pet Test](15-delete-test.png?classes=center-block)

#### Variables Globales
Las variables globales son útiles cuando no queremos repetir código con valores constantes. La URL del API es el mismo para todos los casos por lo que es conveniente convertirla a una variable global como se muestra a continuación. 

![](16-variablesglobales.png?classes=center-block)

En tan sentido, cambiaremos las URL de nuestras pruebas de la siguiente manera.

* ADD PET : {{PETSTORE_API_URL}}
* GET PET : {{PETSTORE_API_URL}}/{{idPetToTest}}
* DELETE PET : {{PETSTORE_API_URL}}/{{idPetToTest}}

#### Generando Documentación 
La generación de documentación depende fuertemente de las descripciones/nombres que se pusieron al crear las colecciones, request y los responses ejemplo. En efecto, debemos asegurarnos guardar los responses obtenidos luego de enviar las peticiones, para que sean adicionados en la documentación.

![](17-generate-saveResponse.png?classes=center-block)

Para generar la documentación nos dirigimos a la colección y la opción Publish Docs. Luego de seleccionar el template y otros estilos visuales el documento podrá ser publicado.

![](17-generate-doc.png?classes=center-block)

En tal sentido, la documentación generada en el ejemplo implementado se encuentra en la siguiente dirección de manera pública en internet.

* [Mi documentación generada](https://documenter.getpostman.com/view/13536986/TVes6m5R)


![](18-docupublished.png?classes=center-block)

#### Ejecutando Pruebas
Luego de dirigirnos al Collection Runner de Postman, seleccionamos la colección y los request que serán evaluados. También indicamos el environtment, numero de iteraciones y delay de las mismas. Para luego hacer clic en Run PETSTORE Collection.

![](19-runner-config.png?classes=center-block)

Seguidamente, se corren las pruebas y se ven los resultados en tiempo real indicando si la prueba fue exitosa, el tiempo de ejecución y otros detalles citados de forma detallada.

![](19-runner-run.png?classes=center-block)

Así mismo, se puede visualizar un cuadro resumen de la ejecución.

![](19-runner-summary.png?classes=center-block)

#### Ejecutando Pruebas con Newman
La ejecución de prueba en la terminal se realiza con [Newman](https://www.npmjs.com/package/newman) e incluso esto puede ser integrado fácilmente a [Jenkins](https://www.jenkins.io/) o [Travis](https://travis-ci.org/) o [Docker](https://www.docker.com/). Por lo tanto, podria ser escalado a un ambiente más automatizado y real de desarrollo.

Para instalarlo simplemente se requiere de [NodeJS](https://nodejs.org/en/). Newman se instala ejecutando:
```sh
npm install -g newman
```
Ahora bien, es necesarios el archivo .JSON de la colección, por lo que debe ser exportado.

![](20-export.png?classes=center-block)

Adicionalmente, también las variables de entorno deben ser exportadas.

![](20-export-globals.png?classes=center-block)

Finalmente, ejecutamos Newman indicando los archivos exportados de la colección y las variables de entorno.

```sh
newman run PETSTORE-Collection.postman_collection.json -e TEST.postman_environment.json
```

![](21-running-newman.png?classes=center-block)

#### Código Fuente
* [Petstore-postman_collection.json](https://drive.google.com/file/d/1ndJNaV032zFDxTcA39d3O0E9QZ5WguT3/view?usp=sharing)
* [Test-postman_environtment.json](https://drive.google.com/file/d/1qpC6rVvUJ20pNICwJLyoyMkT1oVqhDXt/view?usp=sharing)


En este texto, notamos que Postman es una de las herramientas más populares e utilizadas para las pruebas de API, se diferencia de las demás por tener colecciones, colaboración en equipo, integración continua, generación de documentación, etc. Así mismo, crear una cuenta para hacer la gestión en la nube de nuestras pruebas es una ventaja importante. A pesar de que las pruebas se hicieron con una cuenta gratuita resulta sorprendente la cantidad de acciones que se pueden realizar. 


Considero que hacer uso de Postman representaría una gran ventaja en nuestros proyectos sean estos pequeños o grandes por el valor agregado que suma en nuestras implementaciones por lo que creo que es útil abordar más sobre el tema y darle una oportunidad. 