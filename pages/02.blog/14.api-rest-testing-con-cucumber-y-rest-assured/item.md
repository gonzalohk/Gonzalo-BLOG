---
title: 'API REST Testing con Cucumber y Rest-Assured'
media_order: 'clientapi-package.png,plugin-gherkin.png,report-package.png,resources-package.png,runnerpackage.png,stepdefiniitons.png,utils-package.png,verificar.png,gherkin2.png,apirest.png'
---

![API REST Testing con Cucumber y Rest-Assured](apitestbanner.gif?classes=center-block)


Probar y validar servicios API Rest en Java puede ser complejo si no se usan las herramientas adecuadas más aún si estas son automatizadas. En efecto, dichas pruebas se centran en los request y responses que se generan, estos deben probar una serie de aspectos como ser:

* El código de respuesta HTTP (response code)
* Las cabeceras HTTP de la respuesta
* El contenido de la respuesta en JSON o XML (payload)

Quizá no estemos familiarizados con estos conceptos aún por lo que podemos definirlos.

## ¿Qué es API Rest?

Un API (Application Programming Interface) es un conjunto de reglas y protocolos que permiten la comunicación entre aplicaciones software. Ahora bien, REST (Representational State Transfer) se originó en el año 2000 fue descrito en la tesis de Roy Fielding. En efecto, API Rest es una interfaz de programación de aplicaciones que se ajusta al estilo de arquitectura REST.

En otras palabras, API REST define un conjunto de funciones para realizar solicitudes (Request) y recibir respuestas (Responses) a través del protocolo HTTP bajo una arquitectura cliente servidor,. 


![API-Rest Esquema](apirest.png?classes=center-block)


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


## ¿Qué es Cucumber?
Es una herramienta que permite ejecutar descripciones funcionales en texto plano como pruebas de software automatizados. Cucumber ayuda a implementar la metodología Behaviour Driven Development (BDD o Desarrollo Dirigido por Comportamiento) donde primero se escriben las pruebas antes que el código fuente.

En tal sentido, el lenguaje usado por Cucumber para estas descripciones funcionales se denomina Gherkin. 

## ¿Qué es Gherkin?
Se define como un Lenguaje Específico de Dominio, que puede ser entendido por humanos y por ordenadores. 

Para definir descripciones funcionales se necesitan de 5 palabras reservadas para construir nuestras pruebas :

* Feature: Indica el nombre de forma clara de la funcionalidad que vamos a probar. Además se Incluye una descripción en forma de historia de usuario “Como [rol] quiero [característica] para que [los beneficios]”. 
* Scenario: Describe cada escenario que vamos a probar, se deben probar distintos escenarios para una misma funcionalidad.
* Given: Provee contexto para el escenario como el punto donde se ejecuta el test y pre requisitos en los datos.
* When: Especifica el conjunto de acciones que lanzan el test y la interacción del usuario que acciona la funcionalidad que deseamos probar.
* Then: Especifica el resultado esperado en el test.

![Ejemplo Gherkin](gherkin2.png?classes=center-block)

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

![Plugins](plugin-gherkin.png?classes=center-block)

Así mismo, se deben adicionar variables de entorno para Java y Gradle. Por ello, se deberá tener una configuración de la siguiente manera donde en el caso de Windows se vería de esta forma.

* JAVA_HOME=C:\Program Files\Java\jdk-11.0.8
* GRADLE_HOME =C:\gradle-5.6.4
* Path
** C:\gradle-5.6.4\bin

Posterior a ello, verificamos que podamos ejecutar en la consola los comandos siguientes.

![Verificando Variables de entorno](verificar.png?classes=center-block)

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

* URL: http://dummy.restapiexample.com/api/v1/employees
* Formato: json
* Metodo HTTP: GET
* Requiere autenticación: No
* Payload: Ninguno

GET http://dummy.restapiexample.com/api/v1/employees

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

> URL: http://dummy.restapiexample.com/api/v1/employee/{id}
> Formatos: json
> Metodo HTTP: GET
> Requiere autenticación: No
> Payload: Ninguno

GET http://dummy.restapiexample.com/api/v1/employee/1

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

> URL: http://dummy.restapiexample.com/api/v1/create
> Formatos: json
> Metodo HTTP: POST
> Requiere autenticación: No
> Payload: Employee Object

POST http://dummy.restapiexample.com/api/v1/create

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

> URL: http://dummy.restapiexample.com/api/v1/update/{id}
> Formatos: json
> Metodo HTTP: PUT
> Requiere autenticación: No
> Payload: Employee Object

PUT http://dummy.restapiexample.com/api/v1/update/21

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
**e) Eliminar Item**
> URL: http://dummy.restapiexample.com/api/v1/delete/{id}
> Formatos: xml, json
> Metodo HTTP: DELETE
> Requiere autenticación: No
> Parámetros: Ninguno

DELETE http://dummy.restapiexample.com/api/v1/delete/2

Response:
```json
{
    "status": "success",
    "message": "successfully! deleted Records"
}
```

### Creando el proyecto

Intellij IDEA Community será suficiente para implementar y correr nuestras pruebas automatizadas, por lo que creamos un nuevo proyecto Gradle con Java. 



#### a) Dependencias y el build.gradle

Las dependencias necesarias están especificadas en el archivo build.gradle además de establecer la tarea que correrá los tests y que generará el reporte.

En tal sentido, necesitamos ser capaz de ejecutar requests y manipular responses a una API Rest, para ello agregamos. 
* rest-assured v. 4.3.1
* rest-assured-common v. 4.3.1
* json-path v. 4.3.1
* json-schema-validator v. 4.3.1
* Json-20090211

Así mismo, efectuar los test de forma automatizada utilizando Gherkin utilizando cucumber requieren de las siguientes dependencias. 
* JUnit v.4.12
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
### b) Creando el clientapi Package

![clientapi package](clientapi-package.png?classes=center-block)

Ahora bien, en el paquete clientapi crearemos clases capaces de hacer las peticiones a un API cualquiera. Para ello, hacemos uso de clases proporcionadas por rest-assured.

Las peticiones de tipo POST, PUT, DELETE y GET tienen un comportamiento similar, pero una implementación algo diferente. En tal sentido, el patrón de diseño Factory resulta útil en esta ocasión donde el RequestClient.java inicializara el cliente y describe el comportamiento de los request siendo estas sus clases hijas.  
```java
package clientapi;

import io.restassured.response.Response;

public interface IRequest {
   Response send(String url,String payload);
}
```
Post.java
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
Get.java
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
Put.java
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
Delete.java
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
Finalmente el FactoryRequest.java será el encargado de fabricar nuestros request de una forma más organizada sin tener código redundante.

FactoryRequest.java
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
### c) Creando el util Package

![Util package](utils-package.png?classes=center-block)

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

### d) Creando el test feature

![Test feature](resources-package.png?classes=center-block)

El paquete resources alberga a todos nuestros archivos .feature que representa los test en lenguaje Gherkins. En tal sentido, el EmployeesAPI.feature representa las pruebas que haremos con todo.ly
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
```gherkin
 @Regression
 Scenario: delete employee by id
   Given I have access to http://dummy.restapiexample.com/ API
   When I send DELETE request to '/api/v1/delete/EMPLOYEE_ID' with json
   """
   """
   Then I expect the response code 200
```
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
```gherkin
 @Regression
 Scenario: get an employee data by id
   Given I have access to http://dummy.restapiexample.com/ API
   When I send GET request to '/api/v1/employee/10' with json
   """
   """
   Then I expect the response code 200
```
```gherkin
 @Regression
 Scenario: get employees
   Given I have access to http://dummy.restapiexample.com/ API
   When I send GET request to '/api/v1/employees' with json
   """
   """
   Then I expect the response code 200
```
### e) Creando el runner Package

![Runner package](runnerpackage.png?classes=center-block)

El paquete runner se encarga de correr los test y se añaden los steps definitions. El StepDefinitions.java es en donde se implementan los tests que serán ejecutados y que están estrechamente relacionado al EmployeesAPI.feature.
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
Runnercucumber.java
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

### Ejecutando TEST

Para ejecutar nuestros test, ejecutamos el siguiente comando desde nuestra consola el cual llama a una tarea que fue definida en el buid.gradle, esta tarea es correr los test automatizados para todos los escenarios que tengan el tag @Regression.
gradle clean cucumber -Psuite=@Regression


Así mismo, también el report.json fue generado una vez finalizados los test. Este puede ser utilizado como fuente de datos para usar plantillas de reportes y ver una presentación mejor organizada y limpia. Para ello, añadimos un ReportGenerator.java con las siguientes instrucciones. 
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
Finalmente, ejecutamos la última clase creada para generar una mejor vista del reporte en archivos .html Esto se verá de la siguiente manera. 

Ahora ya podemos visualizar los reportes desde cualquier navegador web.


## Repositorio

El código completo se encuentra en el siguiente repositorio.

* [https://github.com/gonzalohk/automation-api-testing-with-cucumber](https://github.com/gonzalohk/automation-api-testing-with-cucumber)

Como se vio en este post,  Rest-Assured que permite simplificar la construcción de los pruebas API Rest, permitiendo la fácil manipulación de los endpoints. Por otro lado, aplicar una metodología BDD usando Cucumber para ejecutar descripciones funcionales de pruebas en texto plano en lenguaje Gherkin tiene muchas ventajs. En tal sentido, una de las principales es poder ser entendido por cualquier persona incluso sin conocimiento técnico y permitiendo verificaciones finales mas sencillas.
