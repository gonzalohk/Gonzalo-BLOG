---
title: 'API REST Testing con Cucumber y Rest Assured'
media_order: 'clientapi-package.png,plugin-gherkin.png,report-package.png,resources-package.png,runnerpackage.png,stepdefiniitons.png,utils-package.png,verificar.png,gherkin2.png,apirest.png,apitestbanner.gif,DummyRestAPIExample.png,newproject.png,report-sctructure.png,cucumberreport.png,ejecucion.png'
published: true
date: '20-10-2020 19:16'
publish_date: '20-10-2020 19:16'
metadata:
    '"Keywords"': '"Automation","QA Automation","API Rest Testing","API Test Cucumber","Cucumber"'
taxonomy:
    category:
        - QA
        - 'QA Automation'
    tag:
        - Cucumber
        - 'API Rest'
        - Gherkin
---

![API REST Testing con Cucumber y Rest-Assured](apitestbanner.gif?classes=center-block)


Probar y validar servicios API Rest en Java puede ser complejo si no se usan las herramientas adecuadas más aún si estas son automatizadas. En efecto, dichas pruebas se centran en los request y responses que se generan, estos deben probar una serie de aspectos como ser:

* El código de respuesta HTTP (response code)
* Las cabeceras HTTP de la respuesta (headers)
* El contenido de la respuesta en JSON o XML (payload)

Quizá no estemos familiarizados con estos conceptos aún por lo que podemos explorarlos.

## ¿Qué es API Rest?

Un API (Application Programming Interface) es un conjunto de reglas y protocolos que permiten la comunicación entre aplicaciones software. Ahora bien, REST (Representational State Transfer) se originó en el año 2000 fue descrito en la tesis de Roy Fielding. En efecto, **API Rest es una interfaz de programación de aplicaciones que se ajusta al estilo de arquitectura REST**.

En otras palabras, API REST define un conjunto de funciones para realizar solicitudes (Request) y recibir respuestas (Responses) a través del protocolo HTTP bajo una arquitectura cliente servidor. 


![API-Rest Esquema](apirest.png?classes=center-block)


En efecto, API REST utiliza los métodos HTTP (HTTP verbs) para realizar las peticiones, cada uno tiene una función en particular.

* GET es utilizado únicamente para consultar información.
* POST usado para solicitar la creación de un nuevo registro (soporta payload).
* PUT se emplea para actualizar por completo un registro existente (soporta payload).
* PATCH similar al método PUT, permite actualizar solo un fragmento del registro y no su totalidad (soporta payload).
* DELETE se lo usa para eliminar un registro existente.
* HEAD utilizado para obtener información sobre un determinado recurso sin retornar el registro.
* OPTIONS permite describir las opciones de comunicación para el recurso de destino.

Los códigos de estado HTTP (HTTP Status code) describen de forma abreviada la respuesta obtenida en las peticiones, estos son:

* 1XX Respuestas informativas
* 2XX Peticiones correctas
* 3XX Redirecciones
* 4XX Errores del cliente
* 5XX Errores del servidor


## ¿Qué es Cucumber?
Es una **herramienta que permite ejecutar descripciones funcionales** en texto plano como pruebas de software automatizados. Cucumber implementa la metodología Behaviour Driven Development ([BDD o Desarrollo Dirigido por Comportamiento](https://es.wikipedia.org/wiki/Desarrollo_guiado_por_comportamiento)) donde primero se escriben las pruebas antes que el código fuente.

En tal sentido, el lenguaje usado por Cucumber para estas descripciones funcionales se denomina Gherkin. 

## ¿Qué es  Gherkin?
Se define como un **Lenguaje Específico de Dominio, que puede ser entendido por humanos y por ordenadores**. Para definir descripciones funcionales se necesitan de 5 palabras reservadas para construir nuestras pruebas :

* **Feature** Indica el nombre de forma clara la funcionalidad que vamos a probar. Incluye una descripción en forma de historia de usuario “Como [_rol_] quiero [_característica_] para que [_los beneficios_]”. 
* **Scenario** Describe cada escenario que vamos a probar, se deben probar distintos escenarios para una misma funcionalidad.
* **Given** Provee contexto para el escenario como el punto donde se ejecuta el test y pre-requisitos en los datos.
* **When** Especifica el conjunto de acciones que lanzan el test y la interacción del usuario que acciona la funcionalidad que deseamos probar.
* **Then** Especifica el resultado esperado en el test para hacer nuestras verificaciones.

![Ejemplo Gherkin](gherkin2.png?classes=center-block)


## ¿Qué es Rest-Assured?

[Rest-Assured](https://www.javadoc.io/doc/io.rest-assured/rest-assured/latest/io/restassured/RestAssured.html) es un Java DSL (Domain Specific Language) construido sobre la capa HTTP Builder que permite simplificar la construcción de pruebas sobre una API REST. Es decir, tiene una sintaxis DSL que sigue el formato Given – When – Then como Gherkin para implementar pruebas.

En tal sentido, se considera un framework que tiene características bastante útiles como validación Xpath, JsonPath, MIME-Types, file upload, verificación de cabeceras, respuestas, cookies y muchas otras opciones. Este fue desarrollado por Jayway.

## Implementación
Para que lo expuesto sea mejor comprendido implementemos un ejemplo para probar y verificar un API Rest público. Todo esto utilizando las herramientas mencionadas y la metodología BDD. 

#### Precondiciones 
Inicialmente descargamos e instalamos Java y Gradle como gestor de dependencias.

* [Java 11](http://jdk.java.net/java-se-ri/11)
* [Gradle 5.6.4](https://gradle.org/releases/)

Por otro lado, necesitamos de una cliente capaz de hacer peticiones al API Rest como Postman con el fin de obtener la mayor información posible. Sin embargo, es importante mencionar que Postman es mucho más que un cliente rest, pero nos podría servir para hacer el análisis previo. 

* [Postman](https://www.postman.com/)

Adicionalmente podemos usar el IDE de nuestra preferencia, pero Intellij IDEA en su versión gratuita tiene lo suficiente para llevar a cabo las pruebas. 

* [IntelliJ IDEA 2020.2.2 Community](https://www.jetbrains.com/es-es/idea/download/#section=windows)

Usaremos Cucumber con Gherkin para realizar los test, por lo que es muy importnate instalar dos plugins en nuestro IDE, que nos ayudarán a formatear código Gherkin y generar código java, estos son:

* Gherkin
* Cucumber for Java

![Plugins](plugin-gherkin.png?classes=center-block)

Así mismo, se deben adicionar variables de entorno para Java y Gradle. En el caso de Windows se vería de esta forma.

* JAVA_HOME=C:\Program Files\Java\jdk-11.0.8
* GRADLE_HOME =C:\gradle-5.6.4
* Path
	*  _C:\gradle-5.6.4\bin_

Posterior a ello, verificamos ejecutando en consola los comandos siguientes.

![Verificando Variables de entorno](verificar.png?classes=center-block)

#### API REST Pública

Para hacer nuestras pruebas, utilizaremos **restapiexample.com** la cual proporciona un API Rest público de prueba. No presenta **autenticación** porque es un API de prueba, pero en cualquier otro proyecto si lo tiene, por lo que debe ser implementada más adelante.

* [http://dummy.restapiexample.com/](http://dummy.restapiexample.com/)

En efecto, nos permitirá registrar, actualizar, listar y eliminar empleados.

* Get all employees
* Get employee by Id
* Create new employee
* Update employee by Id
* Delete employeeItem by Id

![DummyRestAPI ejemplo](DummyRestAPIExample.png?classes=center-block)

En una primera instancia exploramos y probaremos estas API con Postman. Una vez realizado esto ya se tiene más detalle de los servicios mencionados.

###### a) Listar Empleados

* URL: http://dummy.restapiexample.com/api/v1/employees
* Formato: json
* Metodo HTTP: GET
* Requiere autenticación: No
* Payload: Ninguno

GET [http://dummy.restapiexample.com/api/v1/employees](http://dummy.restapiexample.com/api/v1/employees)

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
###### b) Listar Empleado

* URL: http://dummy.restapiexample.com/api/v1/employee/{id}
* Formatos: json
* Metodo HTTP: GET
* Requiere autenticación: No
* Payload: Ninguno

GET [http://dummy.restapiexample.com/api/v1/employee/1](http://dummy.restapiexample.com/api/v1/employee/1)

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
###### c) Crear Empleado

* URL: http://dummy.restapiexample.com/api/v1/create
* Formatos: json
* Metodo HTTP: POST
* Requiere autenticación: No
* Payload: Employee Object

POST [http://dummy.restapiexample.com/api/v1/create](http://dummy.restapiexample.com/api/v1/create)

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
###### d) Actualizar Empleado

* URL: http://dummy.restapiexample.com/api/v1/update/{id}
* Formatos: json
* Metodo HTTP: PUT
* Requiere autenticación: No
* Payload: Employee Object

PUT [http://dummy.restapiexample.com/api/v1/update/21](http://dummy.restapiexample.com/api/v1/update/21)

Content:
```json
{
    "name":"Gustavo (Updated)",
    "salary":"8000",
    "age":"30"
}
```
Response:
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
###### e) Eliminar Empleado
* URL: http://dummy.restapiexample.com/api/v1/delete/{id}
* Formatos: xml, json
* Metodo HTTP: DELETE
* Requiere autenticación: No
* Parámetros: Ninguno

DELETE [http://dummy.restapiexample.com/api/v1/delete/2](http://dummy.restapiexample.com/api/v1/delete/2)

Response:
```json
{
    "status": "success",
    "message": "successfully! deleted Records"
}
```

#### Creando el proyecto

Intellij IDEA Community será suficiente para implementar y correr nuestras pruebas automatizadas, por lo que creamos un nuevo proyecto Gradle con Java. 

![Creando nuevo proyecto gradle](newproject.png?classes=center-block)

#### Dependencias y el build.gradle

Las dependencias necesarias están especificadas en el archivo _build.gradle_, también se establece la tarea que correrá los tests y que generará el reporte. 

Para ser capaces de ejecutar requests y manipular responses a una API Rest agregamos. 
* rest-assured v4.3.1
* rest-assured-common v4.3.1
* json-path v4.3.1
* json-schema-validator v4.3.1
* Json-20090211

Así mismo, escribiremos las pruebas en lenguaje Gherkin que sera ejecutado por cucumber.
* JUnit v4.12
* cucumber-java v1.2.5
* cucumber-junit v1.2.5

Por otro lado, será sumamente útil el visualizar reportes acerca de la ejecución de nuestras pruebas, por ello adicionamos.
* cucumber-reporting v5.3.1

```gradle
plugins {
   id 'java'
}

group 'org.example'
version '1.0-SNAPSHOT'

repositories {
   mavenCentral()
}

dependencies {
   testCompile group: 'junit', name: 'junit', version: '4.12'

   // CUCUMBER BDD
   testCompile 'info.cukes:cucumber-java:1.2.5'
   testCompile group: 'info.cukes', name: 'cucumber-junit', version: '1.2.5'
   compile group: 'info.cukes', name: 'cucumber-java', version: '1.2.5'
   compile group: 'net.masterthought', name: 'cucumber-reporting', version: '5.3.1'

   // REST Assured
   testCompile group: 'io.rest-assured', name: 'rest-assured', version: '4.3.1'
   compile group: 'io.rest-assured', name: 'json-path', version: '4.3.1'
   compile group: 'io.rest-assured', name: 'json-schema-validator', version: '4.3.1'
   compile group: 'io.rest-assured', name: 'rest-assured-common', version: '4.3.1'
   compile group: 'io.rest-assured', name: 'rest-assured-all', version: '4.3.1'

   // JSON
   compile 'com.googlecode.json-simple:json-simple:1.1.1'
   compile 'org.json:json:20090211'
}

configurations {
   cucumberRuntime {
       extendsFrom testImplementation
   }
}

task cucumber() {
   dependsOn assemble, compileTestJava
   doLast {
       javaexec {
           main = "cucumber.api.cli.Main"
           classpath = configurations.cucumberRuntime + sourceSets.main.output + sourceSets.test.output
           args = ['--plugin','json:build/reports/cucumber/report.json', '-f','pretty','--glue', 'runner', 'src/test/resources', '--tags']+[suite]
       }
   }
}
```
#### Creando el clientapi Package

![clientapi package](clientapi-package.png?classes=center-block)

Ahora bien, en el paquete _clientapi_ crearemos clases capaces de hacer las peticiones a un API cualquiera. Para ello, hacemos uso de clases proporcionadas por rest-assured.

Las peticiones de tipo POST, PUT, DELETE y GET tienen un comportamiento similar, pero una implementación algo diferente. En tal sentido, el patrón de diseño Factory resulta útil en esta ocasión donde el _IRequest.java_ describe el comportamiento de los request.
```java
package clientapi;

import io.restassured.response.Response;

public interface IRequest {
   Response send(String url,String payload);
}
```
Con ayuda de RestAssured la implementación de las peticiones se simplifican mucho, pero lo interesante es seguir un formato Given – When – Then. Sin embargo, para este ejemplo no explotaremos mucho esta potencialidad más que para hacer las peticiones a un Endpoint con un Payload en particular.

_Post.java_
```java
package clientapi;

import io.restassured.http.ContentType;
import io.restassured.response.Response;

import static io.restassured.RestAssured.given;

public class Post  implements IRequest{
   @Override
   public Response send(String url, String payload) {
       Response response = given()
               .contentType(ContentType.JSON)
               .body(payload)
               .when()
               .post(url);
       return response;
   }
}
```
_Get.java_
```java
package clientapi;

import io.restassured.http.ContentType;
import io.restassured.response.Response;

import static io.restassured.RestAssured.given;

public class Get implements IRequest{
   @Override
   public Response send(String url, String payload) {
       Response response = given()
               .contentType(ContentType.JSON)
               .when()
               .get(url);

       return response;
   }
}
```
_Put.java_
```java
package clientapi;

import io.restassured.http.ContentType;
import io.restassured.response.Response;

import static io.restassured.RestAssured.given;

public class Put implements IRequest{

   @Override
   public Response send(String url,String payload) {
       Response response = given()
               .contentType(ContentType.JSON)
               .when()
               .put(url);
       return response;
   }
}
```
_Delete.java_
```java
package clientapi;

import io.restassured.http.ContentType;
import io.restassured.response.Response;

import static io.restassured.RestAssured.given;

public class Delete implements IRequest {
   @Override
   public Response send(String url, String payload) {
       Response response = given()
               .contentType(ContentType.JSON)
               .when()
               .delete(url);
       return response;
   }
}
```
No obstante, estas clases tiene una implementación mínima pero pueden ser extendidas mucho en funcionalidad.

El _FactoryRequest.java_ será el encargado de fabricar nuestros request de una forma más organizada sin tener código redundante.

```java
package clientapi;

public class FactoryRequest {
   public static IRequest make(String httpVerb){
       IRequest request;

       switch (httpVerb.toUpperCase()){
           case "POST":
               request = new Post();
               break;
           case "PUT":
               request = new Put();
               break;
           case "DELETE":
               request = new Delete();
               break;
           case "GET":
           default:
               request = new Get();
       }
       return request;
   }
}
```
#### Creando el util Package

![Util package](utils-package.png?classes=center-block)

El paquete útil solo contiene la clase _JsonUtils.java_, esta tiene funciones útiles para hacer operaciones básicas en cadenas en formato JSON que utilizaremos en las pruebas.
```java
package utils;

import org.json.JSONException;
import org.json.JSONObject;

import java.util.Iterator;

public class JsonUtils {
   //Convert STRING -> JSON
   public static JSONObject convertJSON(String json) throws JSONException {
       return new JSONObject(json);
   }

   //Get a "property" value
   public static String getValueFromJSON(String json, String propertyName) throws JSONException {
       String value = convertJSON(JsonUtils.convertJSON(json).get("data").toString()).getString(propertyName);
       return  value;
   }

   //Compare expectedResult vs actualResult
   public static boolean areEqualJSON(String expectedResult, String actualResult) throws JSONException {
       boolean areEqual = true;
       //String -> JSON
       JSONObject jsonExpectedResult = JsonUtils.convertJSON(expectedResult);
       JSONObject jsonActualResult = JsonUtils.convertJSON(actualResult);
       //Iterate jsonExpectedResult Data
       Iterator<?> keys = jsonExpectedResult.keys();
       while (keys.hasNext()) {
           String key = (String) keys.next();
           String actualValue = jsonActualResult.get(key).toString();
           String expectedValue = jsonExpectedResult.get(key).toString();
           if (actualValue.startsWith("{")) {
               areEqual = areEqualJSON(expectedValue,actualValue);
           } else {
               //DO NOT compare fields with value "EXCLUDE"
               if (expectedValue.equals("EXCLUDE")) {
                   System.out.println("INFO: EXCLUDE, the attribute [" + key + "]");
               } else if (!expectedValue.equals(actualValue)) {
                   areEqual = false;
               }
               System.out.println("INFO: COMPARING, the attribute [" + key + "]:  actual value [" + actualValue + "] vs expected [" + expectedValue + "]");
           }
       }
       return areEqual;
   }
}
```

#### Creando el test feature

![Test feature](resources-package.png?classes=center-block)

El paquete resources alberga a todos nuestros archivos .feature que representa los test en lenguaje Gherkins. En tal sentido, el EmployeesAPI.feature representa las pruebas que haremos con restapiexample.com

Seguidamente, realizamos la descripción en lenguaje Gherkin, donde se describe un escenario a probar, se indica el método HTTP a usar, el Endpoint y el Payload necesario. Al finalizar cada prueba se realiza la verificacion correspondiente en el status code y el response esperado. 

###### TEST Crear Nuevo Empleado

```gherkin
 Feature: Employee CRUD feature
 As a user
 I would like to create/update/delete/get an employee
 @Regression
 Scenario Outline: create new employee
   Given I have access to http://dummy.restapiexample.com/ API
   When I send POST request to '/api/v1/create' with json
   """
   {"name":"<name>","salary":"<salary>","age":"<age>"}
   """
   Then I expect the response code 200
   And I get the property 'id' and save its value on EMPLOYEE_ID
   And I expect the response body is equal
   """
   {
     "status": "success",
     "data": {
         "name": "<name>",
         "salary": "<salary>",
         "age": "<age>",
         "id": "EXCLUDE"
     },
     "message": "Successfully! Record has been added."
   }
   """
   Examples:
     | name | salary | age |
     | jimmmy plaza | 2800 | 55 |
```
Nótese que hay propiedades en el response que son dinámicos como el ID que son difíciles de ser evaluado por lo que indicamos que son EXCLUDE y será ignorados gracias a nuestra implementación en _JsonUtils.java_

###### TEST Eliminar Empleado
```gherkin
 @Regression
 Scenario: delete employee by id
   Given I have access to http://dummy.restapiexample.com/ API
   When I send DELETE request to '/api/v1/delete/EMPLOYEE_ID' with json
   """
   """
   Then I expect the response code 200
```
###### TEST Actualizar Empleado
```gherkin
 @Regression
 Scenario: update employee data
   Given I have access to http://dummy.restapiexample.com/ API
   When I send PUT request to '/api/v1/update/10' with json
   """
   {"name":"Updated Name","salary":"10000","age":"30"}
   """
   Then I expect the response code 200
```
###### TEST Listar Empleado
```gherkin
 @Regression
 Scenario: get an employee data by id
   Given I have access to http://dummy.restapiexample.com/ API
   When I send GET request to '/api/v1/employee/10' with json
   """
   """
   Then I expect the response code 200
```
###### TEST Listar Empleados
```gherkin
 @Regression
 Scenario: get employees
   Given I have access to http://dummy.restapiexample.com/ API
   When I send GET request to '/api/v1/employees' with json
   """
   """
   Then I expect the response code 200
```
Una vez implementado esto, se deben crear los Steps Definitions que tienen el código fuente de nuestras pruebas. Los plugins que instalamos en el IDE nos ayudaran a generar código fuente.

![Generar StepDefinitions](stepdefiniitons.png?classes=center-block)

#### Creando el runner Package

![Runner package](runnerpackage.png?classes=center-block)

El paquete _runner_ se encarga de correr los test y es donde se añaden los steps definitions. En tal sentido, el _StepDefinitions.java_ es donde se implementan los tests que serán ejecutados y que están estrechamente relacionado al _EmployeesAPI.feature_

La idea de los _StepDefinitions.java_ es programar cada uno de los Given - When – Then que se escribieron en las pruebas y darles funcionalidad. Sin embargo, muchos de estos se repiten o tienen patrones muy similares que pueden ser mejor organizados. Es así que, gracias a expresiones regulares podemos simplificar y optimizar el código evitando renuncia. 

```java
package runner;

import clientapi.FactoryRequest;
import cucumber.api.java.en.And;
import cucumber.api.java.en.Given;
import cucumber.api.java.en.Then;
import cucumber.api.java.en.When;
import io.restassured.response.Response;
import org.json.JSONException;
import org.junit.Assert;
import utils.JsonUtils;

import java.util.HashMap;
import java.util.Map;
import java.util.concurrent.TimeUnit;

public class StepDefinitions {
   public final String HOST="https://dummy.restapiexample.com";
   public Response response;
   public Map<String, String> globalVariables = new HashMap<>();

   @Given("^I have access to http://dummy\\.restapiexample\\.com/ API$")
   public void iHaveAccessToHttpDummyRestapiexampleComAPI() {
   }

   @When("^I send (POST|PUT|DELETE|GET) request to '(.*)' with json$")
   public void iSendARequestWithJson(String httpVerb, String url, String jsonBody) {
       response = FactoryRequest.make(httpVerb).send(HOST+this.replaceStoredVariables(url),this.replaceStoredVariables(jsonBody));
       System.out.println("[INFO] REQUEST WAS EXECUTED " + url + " - " + response.timeIn(TimeUnit.MILLISECONDS) + " milliseconds");
       response.prettyPrint();
   }

   @Then("^I expect the response code (\\d+)$")
   public void iExpectTheResponseCode(int expectedResponseCode) {
       Assert.assertEquals("Error! Wrong response data.",expectedResponseCode, response.getStatusCode());
   }

   @And("^I expect the response body is equal$")
   public void iExpectTheResponseBodyIsEqual(String expectedJsonBody) throws JSONException {
       Assert.assertTrue("Error!, wrong response data",
               JsonUtils.areEqualJSON(expectedJsonBody,response.getBody().asString()));
   }

   @And("^I get the property '(.*)' and save its value on (.*)$")
   public void iGetThePropertyAndSaveItsValue(String property, String key) throws JSONException {
       String valueToSave = JsonUtils.getValueFromJSON(response.getBody().asString(), property);
       globalVariables.put(key, valueToSave);
       System.out.println("[INFO] Property "+ property + " ("+globalVariables.get(key)+") was stored on "+ key);
   }

   // Util
   private String replaceStoredVariables(String value){
       for (String key: this.globalVariables.keySet()){
           value= value.replace(key,this.globalVariables.get(key));
       }
       return value;
   }
}
```
El _RunnerCucumber.java_ es la clase principal de nuestras pruebas donde se acostumbre a implementar los hooks (@Before/@After), pero en estas pruebas no serán necesarias.

```java
package runner;

import cucumber.api.junit.Cucumber;
import org.junit.After;
import org.junit.Before;
import org.junit.runner.RunWith;

@RunWith(Cucumber.class)
public class RunnerCucumber {
   @Before
   public void before(){}
   @After
   public void after(){}
}
```
### Ejecutando Pruebas

Para ejecutar nuestros test, ejecutamos el siguiente comando, que tiene la tarea de correr los test automatizados para todos los escenarios que tengan el tag _@Regression_.
```sh
gradle clean cucumber -Psuite=@Regression
```

![Ejecucion Cucumber](ejecucion.png?classes=center-block)

Así mismo, también se genera el _report.json_ finalizados los test. Este puede ser utilizado como fuente de datos para usar plantillas de reportes y ver una presentación mejor organizada y limpia. Para ello, en el paquere _runner_ añadimos la clase _ReportGenerator.java_ con las siguientes instrucciones. 
```java
package report;

import net.masterthought.cucumber.Configuration;
import net.masterthought.cucumber.ReportBuilder;

import java.io.File;
import java.util.ArrayList;
import java.util.List;

public class ReportGenerator {

   public static void main(String[]args){
       //Relative path to generate report.json
       String path="build/reports/cucumber/";

       //JSON report file
       File reportOutPut= new File(path + "Generated-ReportAPI");
       List<String> jsonFiles = new ArrayList<>();
       jsonFiles.add(path+"report.json");

       //Configuration
       Configuration configuration =  new Configuration(reportOutPut,"Todo.ly - API Rest test");
       configuration.setBuildNumber("v0.1");
       configuration.addClassifications("Platform","Windows");
       configuration.addClassifications("Owner","Gonzalo");
       configuration.addClassifications("Branch","master");
       configuration.addClassifications("Type Report","Local");

       //Build
       ReportBuilder reportBuilder = new ReportBuilder(jsonFiles,configuration);
       reportBuilder.generateReports();
       System.out.println(System.getProperty("buildNumber"));
   }
}
```
Finalmente, ejecutamos de forma independiente la última clase creada para generar un reporte en formato html. Esto se verá de la siguiente manera. 

![Reporte Generado](report-sctructure.png?classes=center-block)

Ahora ya podemos visualizar los reportes desde cualquier navegador web.

![Cucumber Report](cucumberreport.png?classes=center-block)


### Repositorio

El código completo se encuentra en el siguiente repositorio.

* [https://github.com/gonzalohk/api-rest-testing-cucumber-restassured](https://github.com/gonzalohk/api-rest-testing-cucumber-restassured)


Finalmente, vemos que Rest Assured permite simplificar la construcción de los pruebas API Rest haciendo fácil manipulación de los endpoints. También aplicar una metodología BDD usando Cucumber para ejecutar descripciones funcionales con Gherkin resulta interesante, pero aplica una nueva capa en el desarrollo. Sin embargo, considero que es una buena forma de implementar API test pudiendo ser personalizados e implementados a media, a pesar que herramientas como Postman podría realizar casi lo mismo pero limitándose a su estructura.
