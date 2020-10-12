---
title: 'Como configurar e implementar Mobile Test Automation (Parte 2)'
media_order: 'browserstack.png,browserstack-appautomate.png,package-deviceFactory.png,runtest-automaton.png,browserStack-video.png,javaAndroid-running-summary.png,step1.png,step2.png,automation-mobile-test.jpg,step3.png,testRunningVideo.gif,javaAndroid-running-finishi.png,javaAndroid-running.png,stats.png,automation-testing.png'
published: true
date: '11-10-2020 15:50'
publish_date: '11-10-2020 15:50'
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

![Automation Mobile Test](automation-testing.png?classes=center-block)

La pruebas automatizadas necesitan ser ejecutadas con diversos dispositivos, esto debido a que la ejecución en ciertos modelos no suele ser igual a pesar de que tengan la misma versión del sistema operativo, de la misma forma sucede cuando los fabricantes son distintos. Por otro lado, el reproducir bugs muchas veces requiere de un dispositivo específico respecto a un modelo, fabricante o sistema operativo lo cual suele ser un problema recurrente.

En tal sentido, muchas veces no se tienen los recursos suficientes para tener todos los dispositivos e incluso teniéndolos estos representan una carga de trabajo adicional en respecto al mantenimiento, instalación y configuración que pudieran requerir. En efecto, existen varias soluciones siendo browserstack una de las más importantes hoy en día donde no solo proporcionan dispositivos, vienen con gran valor agregado al registrar toda la información referente a logs y uso recursos.
 
## BrowserStack
Es una **solución basada en cloud que funciona como plataforma de testeo** que permite a los desarrolladores testear sus sitios web y sus aplicaciones móviles a través de navegadores web, sistemas operativos y dispositivos móviles. 

* [https://www.browserstack.com/](https://www.browserstack.com/)

![BrowserStack](browserstack.png?classes=center-block)

Para ello, tienen cuatro productos principales.
* **Live** permite lanzar un dispositivo y probar realizar pruebas en un navegador de forma manual. 
* **Automate** para ejecutar tests automatizados implementados con selenium
* **App Live** permite subir una aplicación móvil nativa e instalarla y probarla en un dispositivo físico.
* **App Automate** nos ayuda a ejecutar tests automatizados implementados con herramientas como Appium, Espresso, XCUI basados en Selenium.

En efecto, los usuarios pueden escoger entre más de 1.200 dispositivos móviles, navegadores, sistemas operativos para soportar miles de tests manuales y automáticos de forma concurrente. El servicio de suscripción fue creado por Ritesh Arora y Nakul Aggarwal en 2011 en Mumbai - India y desde entonces ha obtenido una amplia aceptación para testear la capa de presentación y el rendimiento de un sitio web.

Para nuestras pruebas bastará con registrarse y validar el mismo. Posteriormente, una vez hecha esta acción ya podemos hacer uso de los productos que ofrece, si bien este es un servicio de paga inicialmente se tiene 100 minutos gratis que nos serán suficientes.

#### App Automate
Para nuestras pruebas haremos uso de App Automate, para iniciar podemos hacer clic en el menu correspondiente o ir directamente al siguiente enlace.
* [Getting started](https://app-automate.browserstack.com/dashboard/v2/getting-started)

Nótese que se está escogiendo APPIUM junto a JAVA. 

![BrowserStack - AppAutomate](browserstack-appautomate.png?classes=center-block)


**Paso 1** 
Browserstack nos proporciona documentación para implementar nuestros test con ciertas librerías y test engines. Así mismo, nos da una plantilla con una estructura que podemos usar en nuestros proyecto. 

![Step1](step1.png?classes=center-block)

Sin embargo, ya implementamos nuestra propia estructura en un post anterior por lo que no será necesario elaborarlo nuevamente, pero siempre es útil consultar los recursos que se nos proporcionan.  

**Paso 2**
 La aplicación que será testeada necesita estar en browserstack de modo tal que debemos seleccionar el .apk para luego subirlo.

![Step2](step2.png?classes=center-block)

**Paso 3**
Posteriormente, seleccionamos el device mas propiamente el sistema operativo y el modelo. Quizá no se tengan todos los modelos, esto debido a que estamos usando aún una versión de prueba.

![Step3](step3.png?classes=center-block)

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
      caps.setCapability("browserstack.user", "gonzalo63");
      caps.setCapability("browserstack.key", "XhNJFhBfa3ARUy3CpCnt");
      
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

### Implementación

Como ya tenemos una estructura base implementada en el [anterior post](http://gonzalohk.xyz/blog/configuracion-de-un-entorno-de-testing-funcional-automatizado-mobile-p1), solo será necesario crear un nuevo driver que el nuestro FactoryDriver manejara. En una primera instancia creamos la clase _BrowserStack.java_ en el paquete _deviceFactory_ con una estructura similar a nuestros otros drivers.

![DeviceFactoryPackage](package-deviceFactory.png?classes=center-block)

Sin embargo, necesitamos una configuración diferente para ello usamos el template generado y lo adecuamos a nuestra clase _BrowserStack.java_
```java
package deviceFactory;

import java.net.URL;
import java.net.MalformedURLException;

import io.appium.java_client.AppiumDriver;
import io.appium.java_client.android.AndroidDriver;
import io.appium.java_client.android.AndroidElement;

import org.openqa.selenium.remote.DesiredCapabilities;

public class BrowserStack implements IDevice{

   public static String userName = "gonzalo63";
   public static String accessKey = "XhNJFhBfa3ARUy3CpCnt";

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
Finalmente, actualizamos nuestro _Conf.java_ solamente para indicar que queremos correr el driver de ‘browserstack’ en lugar de ‘android’. 
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

Para correr las pruebas simplemente ejecutamos en consola, si todo fue correcto este mostrará un mensaje en consola o caso contrario detallara el mensaje de error y su excepción.

```sh
gradle clean test
```


![runTests](runtest-automaton.png?classes=center-block)

Una vez ejecutado inmediatamente se realiza el build en BrowserStack,  este se visualiza en el dashboard de App Automate.

![runningTests-browserStack](javaAndroid-running.png?classes=center-block)


Una vez finalizado ya se puede ver el detalle completo sobre lo que pasó en dichas pruebas


![runningTests-browserStack-finish](javaAndroid-running-finishi.png?classes=center-block)

![runningTests-browserStack-summary](javaAndroid-running-summary.png?classes=center-block)

### Resultados
Una gran ventaja es que se tiene mucha información respecto a la ejecución, incluyendo una grabación de video con los steps proporcionados en el codigo fuente.

![BrowserStack Test Video](testRunningVideo.gif?classes=center-block)

El tema de logs es completo como se muestra en la siguiente captura teniedo Text Logs, Network Logs, Device Logs y App profiling.

![BrowserStack Textlogs](browserstack-textlogs.png?classes=center-block)

Por otro lado, si existieran problemas o bugs en los test, este se presenta detalladamente indicando cuando sucedió con una captura de pantalla y el tiempo exacto en el que pasó.

![Test Bug](browserStack-testbug.png?classes=center-block)

Así mismo, las estadísticas referentes al uso de CPU, memoria, batería y network se visualizan de forma clara siendo esta un detalle de mucho valor que en un dispositvo normal necesitaria de más herramientas.

![BrowserStack Stats](stats.png?classes=center-block)

### Repo
El código completo se encuentra en el siguiente repositorio.
* [https://github.com/gonzalohk/automation-mobile-testing-when.do](https://github.com/gonzalohk/automation-mobile-testing-when.do)


Como vemos el tema de probar diversos dispositivos es sumamente importante junto a la información que estos generan para ser usados posteriormente en reportes para su mayor entendimiento. Browserstack es una solución que cumple con estas expectativas y seria bueno probar los otros productos o adentrarnos más en la potencia de esta plataformo para el bien de nuestros proyectos. 