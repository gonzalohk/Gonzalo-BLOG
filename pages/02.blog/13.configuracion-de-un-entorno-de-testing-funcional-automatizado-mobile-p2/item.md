---
title: 'Como configurar e implementar Mobile Test Automation (Parte 2)'
published: true
date: '11-10-2020 11:41'
publish_date: '11-10-2020 11:41'
metadata:
    '"Keywords"': '"Automation","QA Automation","Automation Mobile","Testing mobile"'
taxonomy:
    category:
        - QA
        - 'QA Automation'
    tag:
        - 'automation testing mobile'
        - mobile
---

La pruebas automatizadas necesitan ser ejecutadas con diversos dispositivos, esto debido a que la ejecución en cierto modelos no suele ser igual a pesar de que tengan la misma versión del sistema operativo, de la misma forma sucede cuando los fabricantes son distintos. Por otro lado, el reproducir bugs muchas veces requiere de un dispositivo específico respecto a un modelo, fabricante o sistema operativo lo cual suele ser un problema recurrente.

En tal sentido, muchas veces no se tienen los recursos suficientes para tener todos los dispositivos e incluso teniéndolos estos representan una carga de trabajo adicional en respecto al mantenimiento, instalación y configuración que pudieran requerir. En efecto, existen varias soluciones siendo browserstack una de las más importantes hoy en día donde no solo proporcionan dispositivos, vienen con gran valor agregado al registrar toda la información referente a logs y uso recursos.
 
## BrowserStack
Es una solución basada en cloud que funciona como plataforma de testeo que permite a los desarrolladores testear sus sitios web y sus aplicaciones móviles a través de navegadores web, sistemas operativos y dispositivos móviles. Para ello, tienen cuatro productos principales

* Live, permite lanzar un dispositivo y probar realizar pruebas en un navegador de forma manual. 
Automate
Para ejecutar tests automatizados implementados con selenium
* App Live, permite subir una aplicación móvil nativa e instalarla y probarla en un dispositivo físico.
* App Automate, nos ayuda a ejecutar tests automatizados implementados con herramientas como Appium, Espresso, XCUI basados en Selenium.

En efecto, los usuarios pueden escoger entre más de 1.200 dispositivos móviles, navegadores, sistemas operativos para soportar miles de tests manuales y automáticos de forma concurrente. El servicio de suscripción fue creado por Ritesh Arora y Nakul Aggarwal en 2011 en Mumbai - India y desde entonces ha obtenido una amplia aceptación para testear la capa de presentación y el rendimiento de un sitio web.

Para nuestras pruebas bastará con registrarse y validar el mismo. Posteriormente, una vez hecha esta acción ya podemos hacer uso de los productos que ofrece, si bien este es un servicio de paga inicialmente se tiene 100 minutos gratis que nos serán suficientes.

#### App Automate
En nuestro caso haremos uso de App Automate, para iniciar podemos hacer clic en el menu correspondiente o ir directamente al siguiente enlace.

https://app-automate.browserstack.com/dashboard/v2/getting-started

#### Paso 1 
Browserstack nos proporciona documentación para implementar nuestros test con ciertas librerías y test engines. Así mismo, nos da una plantilla con una estructura que podemos usar en nuestros proyecto. 

Sin embargo, ya implementamos nuestra propia estructura en un post anterior por lo que no será necesario elaborarlo nuevamente, pero siempre es útil consultar los recursos que se nos proporcionan.  

#### Paso 2
 La aplicación que será testeada necesita estar en browserstack de modo tal que debemos seleccionar el .apk para luego subirlo.

#### Paso 3
Posteriormente, seleccionamos el device mas propiamente el sistema operativo y el modelo. Quizá no se tengan todos los modelos, esto debido a que estamos usando aún una versión de prueba.

Todo lo anterior hecho se resumen en un template generado, que permitirá conectarnos a BrowserStack haciendo uso del dispositivo seleccionado de manera remota.

_BrowserStackSample.java_
```java
package android;

import java.net.URL;s.util.List;
import java.util.function.Function;
import java.net.MalformedURLException;

import io.appium.java_client.MobileBy;
import io.appium.java_client.android.AndroidDriver;
import io.appium.java_client.android.AndroidElement;

import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.remote.DesiredCapabilities;
      
public class BrowserStackSample {

  public static void main(String[] args) throws MalformedURLException, InterruptedException {
    
      DesiredCapabilities caps = new DesiredCapabilities();
      
      // Set your access credentials
      caps.setCapability("browserstack.user", "gonzalo73");
      caps.setCapability("browserstack.key", "XhNJFhBfc3ARUy3CpCnt");
      
      // Set URL of the application under test
      caps.setCapability("app", "bs://29915571a1a915db18d82c84a0be27b155f21fb0");
      
      // Specify device and os_version for testing
      caps.setCapability("device", "Samsung Galaxy A51");
      caps.setCapability("os_version", "10.0");
        
      // Set other BrowserStack capabilities
      caps.setCapability("project", "First Java Project");
      caps.setCapability("build", "Java Android");
      caps.setCapability("name", "first_test");
        
      
      // Initialise the remote Webdriver using BrowserStack remote URL
      // and desired capabilities defined above
        AndroidDriver<AndroidElement> driver = new AndroidDriver<AndroidElement>(
            new URL("http://hub.browserstack.com/wd/hub"), caps);
        

      /* Write your Custom code here */
        
        
      // Invoke driver.quit() after the test is done to indicate that the test is completed.
      driver.quit();
    
    }      
  }
```

### Implementando

Como ya tenemos una estructura base aquí solo será necesario crear un nuevo driver que el nuestro FactoryDriver manejara. En una primera instancia creamos la clase BrowserStack.java en el paquete deviceFactory con una estructura similar a nuestros otros drivers.


Sin embargo, necesitamos una configuración diferente para ello usamos el template generado y lo adecuamos a nuestra clase BrowserStack.java.
```java
package deviceFactory;

import java.net.URL;
import java.net.MalformedURLException;

import io.appium.java_client.AppiumDriver;
import io.appium.java_client.android.AndroidDriver;
import io.appium.java_client.android.AndroidElement;

import org.openqa.selenium.remote.DesiredCapabilities;

public class BrowserStack implements IDevice{

   public static String userName = "gonzalo73";
   public static String accessKey = "XhNJFhBfc3ARUy3CpCnt";

   @Override
   public AppiumDriver create() throws MalformedURLException {

       DesiredCapabilities caps = new DesiredCapabilities();

       // Set your access credentials
       caps.setCapability("browserstack.user", userName);
       caps.setCapability("browserstack.key", accessKey);

       // Set URL of the application under test
       caps.setCapability("app", "bs://3a8290da7abf3de00b53b6ea09e2889236a8d280");

       // Specify device and os_version for testing
       caps.setCapability("device", "Samsung Galaxy A51");
       caps.setCapability("os_version", "10.0");

       // Set other BrowserStack capabilities
       caps.setCapability("project", "First Java Project");
       caps.setCapability("build", "Java Android");
       caps.setCapability("name", "first_test");


       // Initialise the remote Webdriver using BrowserStack remote URL
       // and desired capabilities defined above
       AppiumDriver driver = new AndroidDriver<AndroidElement>(new URL("http://hub.browserstack.com/wd/hub"), caps);

       return driver;
   }
}
```
Adicionalmente modificamos nuestro _FactoryDevice.java_.
```java
package deviceFactory;

import io.appium.java_client.MobileElement;

public class FactoryDevice {

   public static IDevice make(String type){
       IDevice device;
       switch (type){
           case "ios":
               device = new IOSDriver();
               break;
           case "browserstack":
               device = new BrowserStack();
               break;
           case "windowsphone":
               device = new WindowsPhoneDriver();
               break;
           case "android":
           default:
               device = new AndroidDriver();
               break;
       }
       return device;
   }
}
```
Finalmente, actualizamos nuestro Conf.java solamente para indicar que queremos correr el driver de ‘browserstack’ en lugar de ‘android’. 
```java
package configuration;

public class Conf {

   //DRIVER to use
   public static final String SELECTED_DRIVER = "browserstack";
...
}
```
Una gran ventaja de utilizar patrones de diseño y estructura del proyecto de forma ordenada es que los cambios efectuados más adelante son mínimos e inclusos estos aún pueden ser mejorados para que se ajusten a nuestras necesidades. 

### Ejecución 
Las siguientes pruebas fueron implementadas anteriormente, las mismas no requieren cambios adicionales.

* addNotes
* addNote
* removeNote

Para correr las pruebas simplemente ejecutamos en consola.
```sh
gradle clean test
```
Una vez ejecutado inmediatamente se realiza el build,  este se visualiza en el dashboard de App Automate.


Una vez finalizado ya se puede ver el detalle completo sobre lo que pasó en dichas pruebas


Una gran ventaja es que se tiene mucha información respecto a la ejecución, una grabación de video y estadísticas referentes al uso de cpu, memoria, batería y network.



Por otro lado, si existieran problemas o bugs en los test, este se presenta detalladamente indicando cuando sucedió, una captura de pantalla y el tiempo exacto en el que pasó


Como vemos el tema de probar diversos dispositivos es sumamente importante junto a la información que estos generan para ser usados posteriormente en reportes para su mayor entendimiento. Browserstack es una solución que cumple con estas expectativas y seria bueno probar los otros productos o adentrarnos más en la potencia de esta plataformo para el bien de nuestros proyectos. 