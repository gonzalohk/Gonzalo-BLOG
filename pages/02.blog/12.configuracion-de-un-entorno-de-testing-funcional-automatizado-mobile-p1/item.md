---
title: 'Como configurar y correr Mobile Automation Tests (Parte 1)'
media_order: 'preconditions.png,virtualDevice.png,adb-devices.png,devices.png,adb-install-todo.png,adb-shell.png,appium-inicio.png,appium-configuration.png,appium-running.png,MavenProject.png,configurationPackage.png,deviceFactoryPackage.png,sessionManagerPackage.png,appiumControlPackage.png,activityPackage.png,MainActivity.png,AddNote.png,MoreOptions.png,confirmation.png,test-results.png,automation-mobile-test.jpg,AutomationTestRunning.gif,testPackage.png'
published: true
date: '27-09-2020 11:17'
publish_date: '27-09-2020 11:17'
taxonomy:
    category:
        - QA
        - 'QA Automation'
    tag:
        - 'automation testing mobile'
        - mobile
---

![Como configurar y correr Mobile Automation Tests con Java + Gradle + Selenium + Appium + JUnit (Parte 1)](automation-mobile-test.jpg?classes=center-block)


Estas últimas semanas estaba adentrándome un poco más a lo que es QA, más propiamente  Automation Testing, pienso que es interesante y no muy complicado de aplicar. Por lo que me gustaría compartir lo que voy aprendiendo, si bien es una introducción sé que es una base para quien este interesado en ingresar a esta área. 

En efecto, haremos la **configuración de un entorno de testing funcional automatizado para aplicaciones móviles con Java, Gradle, Selenium, Appium y Junit**. Todo esto desde cero sin utilizar algún framework, para tener un mejor entendimiento y estructurarlo mejor según nuestras necesidades.

Para iniciar con nuestras pruebas automatizadas debemos establecer un ambiente de desarrollo inicialmente local. Para ello, este debe ser capaz de proporcionarnos comunicación con nuestros dispositivos móviles (también virtuales), adicionalmente debe hacer uso de herramientas para administrar los dispositivos y mediante un lenguaje de programación junto a librerías adicionales debería poder correr nuestros casos de prueba.

## Inicio
Primeramente descargamos e instalamos las siguientes herramientas de desarrollo, gestor de dependencias y herramienta de automatización respectivamente.

* Java 11 - [Descargar](http://jdk.java.net/java-se-ri/11) [Documentación](https://docs.oracle.com/en/java/javase/11/)
* Android Studio 4.0.1 - [Descargar](https://developer.android.com/studio?hl=en) [Documentación](https://developer.android.com/studio/intro)
* Gradle 5.6.4 -  [Descargar](https://gradle.org/releases/) [Documentación](https://docs.gradle.org/current/userguide/userguide.html)
* Appium 1.18.0-2 - [Descargar](http://appium.io/) [Documentación](http://appium.io/docs/en/about-appium/api/)

Adicionalmente podemos usar el IDE de nuestra preferencia, pero Intellij IDEA en su versión gratuita tiene lo suficiente para llevar a cabo nuestras pruebas. 

* IntelliJ IDEA 2020.2.2 Community - [Descargar](https://www.jetbrains.com/es-es/idea/download/#section=windows)

Así mismo, se deben adicionar variables de entorno a nuestro Java JDK, Android SDK, Gradle y Android tools para hacer uso del adb. En efecto, se deberá tener una configuración de la siguiente manera donde en el caso de Windows las variables de entorno se construiran de esta forma.

* JAVA_HOME= _C:\Program Files\Java\jdk-11.0.8_
* ANDROID_HOME= _C:\Users\{MI-USUARIO}\AppData\Local\Android\Sdk_
* ANDROID_SDK_ROOT= _C:\Users\{MI-USUARIO}\AppData\Local\Android\Sdk_
* GRADLE_HOME = _C:\gradle-5.6.4_
* Path
	*  _C:\gradle-5.6.4\bin_
	*  _C:\Users\{MI - USUARIO}\AppData\Local\Android\Sdk\tools_
	*  _C:\Users\{MI - USUARIO}\AppData\Local\Android\Sdk\platform-tools_

Posterior a ello, verificamos que podamos ejecutar en la consola los comandos siguientes.

![Verificando](preconditions.png?classes=center-block)

## Conectando dispositivos
Necesitaremos ejecutar nuestras pruebas en dispositivos móviles, para ello tenemos varias opciones siendo una de ellas el crear dispositivos virtuales mediante Android Studio y el AVD (_Tools -> AVD Manager -> Create Virtual Device_). Esto representa una gran ventaja porque podemos escoger entre una variedad de dispositivos o también podemos personalizar las características de los mismos junto a la versión de android que necesitemos.

![VirtualDevice](virtualDevice.png?classes=center-block)

Sin embargo, el uso de recursos de Android Studio más aún los dispositivos virtuales representan una gran carga memoria y recursos por lo que podemos también usar los dispositivos móviles reales que tengamos a la mano.  

En tal sentido, podemos hacer uso de dispositivos reales, en el caso de Android debemos habilitar las Developer Options junto al USB Debugging [aqui](https://developer.android.com/studio/debug/dev-options#enable). Una vez habilitado podemos hacer uso de una aplicación mirroring para visualizar y manejar nuestro celular desde nuestra computadora de manera más cómoda. Para ello instalamos.

* Vysor - [Descargar](https://www.vysor.io/download/)

Ahora podemos verificar mediante linea de comandos si nuestros dispositivos están funcionando correctamente luego de ser iniciados y conectados por cable a nuestro equipo respectivamente. En mi caso, tengo dos dispositivos como se muestra en la imagen uno virtual y uno real.

```sh
adb devices
```

![adb-devices](adb-devices.png?classes=center-block)


![Dispositivo real y virtual](devices.png?classes=center-block)

## Instalando When.Do (Aplicación A Testear)
Para nuestras pruebas instalaremos una aplicación para registrar notas similar a un TODO denominada When.do, para ello descargamos.

* When.Do [Descagar](https://drive.google.com/file/d/1qvl-pTmCmMaTPty-DClIkGec2ItE8KNP/view?usp=sharing)

Existen diferentes formas de instalar un .apk en nuestro dispositivo, pero ahora lo haremos desde la terminal usando el Android Debug Bridge o adb indicando el número de serie del dispositivo y la aplicación a instalar.

```sh
adb -s emulator-5554 install Todo.apk
adb -s 310012c7b2929300 install Todo.apk
```

![Instalar When.do](adb-install-todo.png?classes=center-block)

## Package y Activity de When.Do
Una vez instalada la aplicación que será testeada necesitamos identificar el package y el activity al cual responden, esto para conectarnos posteriormente. Para ello, abrimos la aplicación y la ponemos en foco para luego ingresar a la Shell de nuestro teléfono desde la consola.

```sh
adb -s 310012c7b2929300 shell
```
Seguidamente ejecutamos el siguiente comando y buscamos 'CurrentFocus' o 'inputMethodControlTarget' para nuevas versiones de Android.
```sh
dumpsys window windows | grep -E 'CurrentFocus'
```
![adb shell](adb-shell.png?classes=center-block)

El mensaje que nos muestra la consola corresponde a la aplicación que esta en foco actualmente, en nuestro caso when.do. Ahora bien ya lo suficiente para obtener el package y el activity.

* Package: _com.vrproductiveapps.whendo_
* Activity: _ui.HomeActivity_

## Conectandonos a APPIUM
Seguidamente, para conectar nuestro dispositivo móvil con Appium necesitamos al menos de 5 datos importantes que son: deviceName, plataformVersion, appPackage, appActivity y platformName. Sin embargo, ya identificamos al package junto al activity, los otros datos pueden ser obtenidos fácilmente desde la configuracion del teléfono.  En tal sentido, la configuración necesaria para establecer una conexión de mi dispositvo y Appium usando When.do es: 
```json
{
  "deviceName": "Galaxy Tab A (2016)",
  "platformVersion": "5.1.1",
  "appPackage": "com.vrproductiveapps.whendo",
  "appActivity": ".ui.HomeActivity",
  "platformName": "Android"
}
```
Ahora bien, iniciamos el Appium Server con los valores por defecto. 

![Appium](appium-inicio.png?classes=center-block)

Con ayuda del Inspector Session adicionamos las configuraciones mínimas que señalamos anteriormente, para luego iniciar la sesión.

![Appium configuracion](appium-configuration.png?classes=center-block)

Si la conexión fue exitosa veremos la siguiente pantalla donde se visualiza el dispositivo, los sources y el detalle del element seleccionado. Este último es muy importante porque nos ayudará a encontrar los locators de cada elemento en la vista para luego manipularlos como buttons, labels, text boxes, check boxes, etc.

![Appium corriendo](appium-running.png?classes=center-block)

## Creando el proyecto

Intellij IDEA Community será suficiente para implementar y correr nuestras pruebas automatizadas, por lo que creamos un nuevo proyecto Gradle con Java. 

![Gradle project](MavenProject.png?classes=center-block)


#### Dependencias y el build.gradle
Nuestro ambiente necesita ser capaz de conectarse al dispositivo móvil, efectuar los test de forma automatizados y ordenada. Para ello utilizamos las siguientes dependencias que son especificadas en el archivo build.gradle
* JUnit v.4.12
* Appium Java-client v.7.3.0
* Selenium-java v.3.141.59

```
plugins {
    id 'java'
}

group 'gohk'
version '1.0-SNAPSHOT'

repositories {
    mavenCentral()
}

dependencies {
    testCompile group: 'junit', name: 'junit', version: '4.12'
    // https://mvnrepository.com/artifact/io.appium/java-client
    compile group: 'io.appium', name: 'java-client', version: '7.3.0'
    // https://mvnrepository.com/artifact/org.seleniumhq.selenium/selenium-java
    compile group: 'org.seleniumhq.selenium', name: 'selenium-java', version: '3.141.59'
}
```
#### Creando el configure Package

![configure package](configurationPackage.png?classes=center-block)

El primer paquete que creamos se denomina configuration en este adicionamos una clase denominada _Conf.java_ donde adicionaremos constantes que necesitaremos para conectarnos a Appium tal como lo hicimos anteriormente con el appium desktop.

```java
package configuration;

public class Conf {
    public static final String DEVICE_NAME = "deviceName";
    public static final String PLATFORM_VERSION = "platformVersion";
    public static final String APP_PACKAGE = "appPackage";
    public static final String APP_ACTIVITY = "appActivity";
    public static final String PLATFORM_NAME = "platformName";

    // APPIUM Server
    public static final String APPIUM_SERVER = "http://127.0.0.1:4723/wd/hub";
    public static final int DEFAULT_IMPLICIT_WAIT = 25;
    
    // Device Information
    public static final String DEVICE_NAME_VALUE = "Galaxy Tab A (2016)";
    public static final String PLATFORM_VERSION_VALUE = "5.1.1";
    public static final String APP_PACKAGE_VALUE = "com.vrproductiveapps.whendo";
    public static final String APP_ACTIVITY_VALUE = ".ui.HomeActivity";
    public static final String PLATFORM_NAME_VALUE = "Android";
}
```
#### Creando el deviceFactory Package

![deviceFactory package](deviceFactoryPackage.png?classes=center-block)

Ahora creamos el paquete deviceFactory donde configuraremos nuestros drivers para conectarnos con nuestro dispositivo Android, iPhone o Windows Phone. 

Aca aplicaremos el patrón de diseño Factory debido a que los drivers de los distintos sistemas operativos tienen tareas similares pero con detalles de implementación diferentes. En tal sentido, creamos la interfaz _IDevice.java_ para especificar que deben hacer los drivers.
```java
package deviceFactory;

import io.appium.java_client.AppiumDriver;
import java.net.MalformedURLException;

public interface IDevice {
    AppiumDriver create() throws MalformedURLException;
}
```
En el _AndroidDriver.java_ implementamos el método create() que nos ayudará a conectaremos con Appium con ayuda de los datos de configuración que se añadieron en el anterior paquete.
```java
package deviceFactory;

import configuration.Conf;
import io.appium.java_client.AppiumDriver;
import org.openqa.selenium.remote.DesiredCapabilities;

import java.net.MalformedURLException;
import java.net.URL;
import java.util.concurrent.TimeUnit;

public class AndroidDriver implements IDevice {

    @Override
    public AppiumDriver create() throws MalformedURLException {

        DesiredCapabilities capabilities=new DesiredCapabilities();
        capabilities.setCapability(Conf.DEVICE_NAME, Conf.DEVICE_NAME_VALUE);
        capabilities.setCapability(Conf.PLATFORM_VERSION, Conf.PLATFORM_VERSION_VALUE);
        capabilities.setCapability(Conf.APP_PACKAGE, Conf.APP_PACKAGE_VALUE);
        capabilities.setCapability(Conf.APP_ACTIVITY, Conf.APP_ACTIVITY_VALUE);
        capabilities.setCapability(Conf.PLATFORM_NAME, Conf.PLATFORM_NAME_VALUE);

        AppiumDriver driver=new io.appium.java_client.android.AndroidDriver(new URL(Conf.APPIUM_SERVER),capabilities);
        driver.manage().timeouts().implicitlyWait(Conf.DEFAULT_IMPLICIT_WAIT, TimeUnit.SECONDS);
        return driver;
    }
}
```
De la misma forma posteriormente será necesario implementar el create() para IOS, Windows Phone o cualquier otro driver que queramos utilizar, pudiendo ser también alguno remoto como los que ofrece browserstack.
```java
package deviceFactory;

import io.appium.java_client.AppiumDriver;

public class IOSDriver implements IDevice{
    @Override
    public AppiumDriver create() {
        // TODO Add implementation
        return null;
    }
}
```
```java
package deviceFactory;

import io.appium.java_client.AppiumDriver;

public class WindowsPhoneDriver implements IDevice {
    @Override
    public AppiumDriver create() {
        // TODO Add implementation
        return null;
    }
}
```
De modo tal que el _FactoryDevice.java_ será el encargado de fabricar los drivers en función al parámetro que indiquemos.
```java
package deviceFactory;

public class FactoryDevice {

    public static IDevice make(String type){
        IDevice device;
        switch (type){
            case "ios":
                device = new IOSDriver();
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
#### Creando el sessionManager Package

![sessionManager package](sessionManagerPackage.png?classes=center-block)

Ahora bien, en el paquete _sessionManager_ el _Session.java_ nos ayuda a gestionar la conexión que establecemos con un driver en particular, para fines de prueba un driver Android. Al ser este un singleton evitaremos problemas comunes que se tendrían con múltiples instancias o el crear o cerrar las conexiones.

```java
package sessionManager;

import deviceFactory.FactoryDevice;
import io.appium.java_client.AppiumDriver;
import java.net.MalformedURLException;

public class Session {
    public static Session session= null;
    private AppiumDriver driver;
    private Session() throws MalformedURLException {
        driver = FactoryDevice.make("android").create();
    }

    public static Session getInstance() {
        if (session == null){
            try {
                session = new Session();
            } catch (MalformedURLException e) {
                e.printStackTrace();
            }
        }
        return session;
    }

    public void closeSession (){
        driver.quit();
        session = null;
    }

    public AppiumDriver getDriver(){
        return driver;
    }
}
```
#### Creando el appiumControl Package

![appiumControl package](appiumControlPackage.png?classes=center-block)

El paquete _appiumControl_ representa una abstracción de los elementos que pueden llegar a tener nuestras aplicaciones como botones, cajas de texto, labels, checkboxes, etc. 

Muchas de las acciones que realizan estos elementos son exactamente iguales por lo que es conveniente abstraer estas funcionalidades y características. En efecto, el establecer relaciones de herencia para evitar código redundante es importante, el _Control.java_ tiene muchas acciones que son compartidas por otros elementos, así mismo existen acciones propias de cada elemento por lo que también son implementadas en sus respectivas clases. 

Esto permite tener código más limpio y abierto a ser extendido según las necesidades de nuestra aplicación.

```java
package appiumControl;

import org.openqa.selenium.By;
import org.openqa.selenium.WebElement;
import sessionManager.Session;

public class Control {
    protected WebElement control;
    protected By locator;

    public Control(By locator){
        this.locator = locator;
    }

    public void findControl() {
        this.control = Session.getInstance().getDriver().findElement(this.locator);
    }

    public void click() {
        this.findControl();
        this.control.click();
    }

    public String getText(){
        this.findControl();
        return this.control.getText();
    }

    public boolean isDisplayed(){
        try{
            this.findControl();
            return this.control.isDisplayed();
        }catch (Exception e){
            return false;
        }
    }
}
```
```java
package appiumControl;

import org.openqa.selenium.By;
import java.net.MalformedURLException;

public class TextBox extends Control {
    public TextBox(By locator){
        super(locator);
    }
    public void write(String value) throws MalformedURLException {
        this.findControl();
        this.control.sendKeys(value);
    }
}
```
```java
package appiumControl;

import org.openqa.selenium.By;

public class CheckBox extends Control {

   public CheckBox(By locator) {
       super(locator);
   }

   public void check(){
       // TODO Add implementation
       // Workaround
       click();
   }

   public void unCheck(){
       // TODO Add implementation
   }
}
```
```java
package appiumControl;

import org.openqa.selenium.By;

public class Label extends Control {
    public Label(By locator){
        super(locator);
    }
}
```
```java
package appiumControl;

import org.openqa.selenium.By;

public class Button extends Control {
    public Button(By locator){
        super(locator);
    }
}
```
#### Creando el activity Package

![activity package](activityPackage.png?classes=center-block)

El paquete _activity_ sigue parte del patrón SinglePage donde cada clase representa a una vista y donde cada atributo es un elemento de la misma. Es decir, que cada clase definirá objetos Control del paquete _appiumControl_ para ser localizado mediante un locator y ser manipulado posteriormente. Estos locators pueden ser facilmente identificados con Appium como se vio anteriormente.

###### MainActivity.java

![MainActivity](MainActivity.png?classes=center-block)
```java
package activity;

import appiumControl.Button;
import appiumControl.CheckBox;
import appiumControl.Label;
import org.openqa.selenium.By;
import sessionManager.Session;

public class MainActivity {
    public Button searchButton;
    public Button plusButton;
    public Button moreOptionsButton;

    public MainActivity(){
        moreOptionsButton = new Button(By.xpath("//android.widget.ImageView[@content-desc=\"M\u00e1s opciones\"]"));
        plusButton = new Button(By.id("com.vrproductiveapps.whendo:id/fab"));
        searchButton = new Button(By.id("com.vrproductiveapps.whendo:id/search"));
    }

    public void checkFirstNote(){
        String locator = "(//android.widget.ImageButton[@content-desc=\"Marcar como Hecha\"])[1]";
        CheckBox fistCheckBox = new CheckBox(By.xpath(locator));
        if (fistCheckBox != null) {
            fistCheckBox.check();
        }
    }

    public boolean isNoteDisplayed(String noteTitle){
        String locator = "//android.widget.TextView[@text='"+noteTitle+"']";
        Label note = new Label(By.xpath(locator));
        return note.isDisplayed();
    }

    public int countNotes(){
        // Workaround
        String locator = "//android.widget.ImageButton[@content-desc=\"Marcar como Hecha\"]";
        int numNotes = Session.getInstance().getDriver().findElements(By.xpath(locator)).size();
        return numNotes;
    }
}
```
###### AddNoteActivity.java

![AddNoteActivity](AddNote.png?classes=center-block)
```java
package activity;

import appiumControl.Button;
import appiumControl.TextBox;
import org.openqa.selenium.By;

public class AddNoteActivity {
    public Button saveNoteButton;
    public Button saveAndNewNoteButton;
    public Button closeButton;
    public TextBox titleTextBox;
    public TextBox noteTextBox;

    public AddNoteActivity(){
        saveNoteButton = new Button(By.id("com.vrproductiveapps.whendo:id/saveItem"));
        saveAndNewNoteButton = new Button(By.id("com.vrproductiveapps.whendo:id/saveAndNew"));
        closeButton = new Button(By.xpath("//android.widget.ImageButton[@content-desc=\"Navegar hacia arriba\"]"));
        titleTextBox = new TextBox(By.id("com.vrproductiveapps.whendo:id/noteTextTitle"));
        noteTextBox = new TextBox(By.id("com.vrproductiveapps.whendo:id/noteTextNotes"));
    }
}
```
###### MoreOptionsActivity.java

![MoreOptionsActivity](MoreOptions.png?classes=center-block)
```java
package activity;

import appiumControl.Button;
import org.openqa.selenium.By;

public class MoreOptionsActivity {
    public Button deleteButton;

    public MoreOptionsActivity() {
        deleteButton = new Button(By.xpath("//android.widget.TextView[@text='Eliminar Completadas']"));
    }
}
```
###### ConfirmationActivity.java

![confirmationActivity](confirmation.png?classes=center-block)
```java
package activity;

import appiumControl.Button;
import org.openqa.selenium.By;

public class ConfirmationActivity {
    public Button deleteButton;

    public ConfirmationActivity() {
        deleteButton = new Button(By.id("android:id/button1"));
    }
}
```
#### Creando el test Package

![test package](testPackage.png?classes=center-block)

Finalmente, creamos el paquete _test_ donde implementamos el _WhenDoTest.java_ donde definimos tres test que serán evaluados con ayuda de JUnit.

* addNote
* addNotes
* removeNote

En efecto, con ayuda de los activities y controles que fueron añadidos anteriormente, la implementación de los test automatizados resulta en código sencillo, limpio y muy legible donde que cualquier miembro del equipo es capaz de leer y entender los mismos. Siguiendo un orden particular donde se especifica donde el elemento realiza una acción (Activity + element + action) esto se refleja en los steps y la verificación final.
```java
package test;

import activity.AddNoteActivity;
import activity.ConfirmationActivity;
import activity.MainActivity;
import activity.MoreOptionsActivity;
import org.junit.Assert;
import org.junit.Test;

import java.net.MalformedURLException;

public class WhenDoTest {
    MainActivity mainActivity = new MainActivity();
    AddNoteActivity addNoteActivity = new AddNoteActivity();
    MoreOptionsActivity moreOptionsActivity = new MoreOptionsActivity();
    ConfirmationActivity confirmationActivity = new ConfirmationActivity();

    @Test
    public void addNote() throws MalformedURLException {
        //Steps
        mainActivity.plusButton.click();
        addNoteActivity.titleTextBox.write("title test");
        addNoteActivity.noteTextBox.write("Note test");
        addNoteActivity.saveNoteButton.click();

        //Verification
        Assert.assertTrue("New note was not created", mainActivity.isNoteDisplayed("title test"));
    }

    @Test
    public void addNotes() throws MalformedURLException {
        //Steps
        mainActivity.plusButton.click();
        addNoteActivity.titleTextBox.write("title test 1");
        addNoteActivity.noteTextBox.write("Note test 1");
        addNoteActivity.saveAndNewNoteButton.click();

        addNoteActivity.titleTextBox.write("title test 2");
        addNoteActivity.noteTextBox.write("Note test 2");
        addNoteActivity.saveNoteButton.click();

        //Verification
        Assert.assertTrue("First new note was not created", mainActivity.isNoteDisplayed("title test 1"));
        Assert.assertTrue("Second new note was not created", mainActivity.isNoteDisplayed("title test 2"));
    }

    @Test
    public void removeNote() {
        //Steps
        int numNotes = mainActivity.countNotes();
        mainActivity.checkFirstNote();
        mainActivity.moreOptionsButton.click();
        moreOptionsActivity.deleteButton.click();
        confirmationActivity.deleteButton.click();

        //Verification
        Assert.assertEquals("Error, Note was not deleted", numNotes -1, mainActivity.countNotes());
    }
}
```
## Ejecutando Tests
Por último nos aseguramos que el Appium server esta corriendo para luego ejecutar el _WhenDoTest.java_ y verificar que los tests sean exitosos.

![Ejecutando Tests](AutomationTestRunning.gif?classes=center-block)

![Resultados](test-results.png?classes=center-block)

## Repositorio
El código completo se encuentra en el siguiente repositorio.
* [https://github.com/gonzalohk/automation-mobile-testing-when.do](https://github.com/gonzalohk/automation-mobile-testing-when.do)


Hasta aquí ya tenemos un ambiente para testear nuestras aplicaciones móviles aprovechando patrones de diseño y buenas prácticas que pueden ser mejoras u adecuadas a otras necesidades. Por otro lado, existen frameworks que establecen marcos de trabajo más complejos, pero muchas veces no se acoplan a requerimientos que pudiéramos tener. En efecto, Automation Testing es una área que tiene mucho que explotar y al mismo tiempo tiene mucha madurez en los procesos que lleva a cabo, más adelante intentare compartir mas de lo que voy aprendiendo.