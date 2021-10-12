## Trabajo Práctico 5 - Herramientas de construcción de software


### 4- Desarrollo:

#### 1- Instalar Java JDK si no dispone del mismo. 
  - Java 8 es suficiente, pero puede utilizar cualquier versión.
  - Utilizar el instalador que corresponda a su sistema operativo 
  - http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
  - Agregar la variable de entorno JAVA_HOME
    - En Windows temporalmente se puede configurar
    ```bash
      SET JAVA_HOME=C:\Program Files\Java\jdk1.8.0_221
    ```
    - O permanentemente entrando a **Variables de Entorno** (Winkey + Pausa -> Opciones Avanzadas de Sistema -> Variables de Entorno)
  - Otros sistemas operativos:
    - https://www3.ntu.edu.sg/home/ehchua/programming/howto/JDK_Howto.html
    - https://www.digitalocean.com/community/tutorials/how-to-install-java-with-apt-on-ubuntu-18-04


#### 2- Instalar Maven
- Instalar maven desde https://maven.apache.org/download.cgi (última versión disponible 3.6.1)
- Descomprimir en una carpeta, por ejemplo C:\tools
- Agregar el siguiente directorio a la variable de entorno PATH, asumiendo que los binarios de ant están en C:\tools\apache-maven-3.6.1\bin

  ```bash   
    SET PATH=%PATH%;C:\tools\apache-maven-3.6.1\bin
  ```  
- Se puede modificar permanentemente la variable PATH entrando a (Winkey + Pausa -> Opciones Avanzadas de Sistema -> Variables de Entorno)
- En Linux/Mac se puede agregar la siguiente entrada al archivo ~/.bash_profile

  ```bash
  export PATH=/opt/apache-maven-3.6.1/bin:$PATH
  ```

#### 3- Introducción a Maven
- Qué es Maven?

Maven es una herramienta de software que se utiliza para la gestion de proyectos Java. Facilita todo el proceso del build (compilar y crear ejecutables), ademas podemos hacer tests, deploys, entre otras cosas.

- Qué es el archivo POM?
 
El archivo POM es un fichero de configuriacion de Maven del proyecto, en donde se especifican que modulos compoenen el proyecto, o que librerias utiliza el software que estamos desarrollando. Cuenta con lo siguiente:
    1. modelVersion
    2. groupId
    3. artifactId
    4. versionId
- Repositorios Local, Central y Remotos http://maven.apache.org/guides/introduction/introduction-to-repositories.html
- Entender Ciclos de vida de build
  - default: controla el despliegue del proyecto
  - clean: controla la limpieza del proyecot
  - site: controla la creacion del sistio de documentacion del proyecto
  - Referencia: http://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#Build_Lifecycle_Basics
- Comprender las fases de un ciclo de vida, por ejemplo, default:

| Fase de build | Descripción                                                                                                                            |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------|
| validate      | valida si el proyecto está correcto y toda la información está disponible                                                             |
| compile       | compila el código fuente del proyecto                                                                                 |
| test          | prueba el código fuente compilado utilizando un marco de prueba de unidad adecuado. Estas pruebas no deberían requerir que el código se empaquete o implemente |
| package       | toma el código compilado y lo empaqueta en su formato distribuible, como un JAR.                                                     |
| verify        | ejecuta cualquier verificación de los resultados de las pruebas de integración para garantizar que se cumplan los criterios de calidad                                                      |
| install       | instal1 el paquete en el repositorio local, para usarlo como dependencia en otros proyectos localmente                                       |
| deploy        | hecho en el entorno de compilación, copia el paquete final en el repositorio remoto para compartirlo con otros desarrolladores y proyectos.      |

- Copiar el siguiente contenido a un archivo, por ejemplo ./trabajo-practico-02/maven/vacio/pom.xml

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>ar.edu.ucc</groupId>
    <artifactId>proyecto-01</artifactId>
    <version>0.1-SNAPSHOT</version>
</project>
```

- Ejecutar el siguiente comando en el directorio donde se encuentra el archivo pom.xml
```
mvn clean install
```

- Sacar conclusiones del resultado

En la carpeta en donde esta el pom.xml se creo una nueva carpeta llamada target la cual contiene otra carpeta que se llama maven-archiver y un jar, la carpeta maven-archiver tiene un documento que se llama pom.properties en donde figura un snapshot que muestra las propiedades que especificamos en el pom.xml.

Con el ```mvn clean install``` le decimos a Maven que inicie la fase clean en cada modulo antes de ejecutar la fase install para cada modulo, es decir, que se asegura de limpiar todo lo creado en compilaciones anteriores, por eso se crea el snapshot.jar que nos dice cual es la version actual del proyecto


#### 4- Maven Continuación

- Generar un proyecto con una estructura inicial:

```bash
mvn archetype:generate -DgroupId=ar.edu.ucc -DartifactId=ejemplo -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

- Analizar la estructura de directorios generada:

```
.
└── ejemplo
    ├── pom.xml
    └── src
        ├── main
        │   └── java
        │       └── ar
        │           └── edu
        │               └── ucc
        │                   └── App.java
        └── test
            └── java
                └── ar
                    └── edu
                        └── ucc
                            └── AppTest.java

12 directories, 3 files
```

- Compilar el proyecto

```bash
mvn clean package
```

- Analizar la salida del comando anterior y luego ejecutar el programa

Se crea un snapshot del proyecto en donde se detecta si se hicieron cambios sobre el codigo y luego se lo compila para ver si funciona adecuadamente. Se le realizan una serie de run tests y si todos salen exitosos se dice que el build fue exitoso y se guarda la informacion del build en un snapshot.jar con el dia y la hora en la que se realizó.

```
java -cp target/ejemplo-1.0-SNAPSHOT.jar ar.edu.ucc.App
```

![imagen](https://user-images.githubusercontent.com/48757979/136879557-b227b7a6-a792-4be1-8892-2f46beb548ad.png)


#### 6- Manejo de dependencias

- Crear un nuevo proyecto con artifactId **ejemplo-uber-jar**

- Modificar el código de App.java para agregar utilizar una librería de logging:

```java
package ar.edu.ucc;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

/**
 * Hello world!
 *
 */
public class App 
{
    public static void main( String[] args )
    {
        Logger log = LoggerFactory.getLogger(App.class);
        log.info("Hola Mundo!");
    }
}
```

- Compilar el código e identificar el problema.

  El build del codigo falla debido a que Maven no cuenta con los detalles de configuracion y la informacion necesaria para poder construir el proyecto, esto se soluciona agregando el pom.xlm

- Agregar la dependencia necesaria al pom.xml

```xml
    <dependency>
      <groupId>ch.qos.logback</groupId>
      <artifactId>logback-classic</artifactId>
      <version>1.2.1</version>
    </dependency>
```

- Verificar si se genera el archivo jar y ejecutarlo

```bash
java -cp target\ejemplo-uber-jar-1.0-SNAPSHOT.jar ar.edu.ucc.App
```

```
micaela@micaela-GL553VD:~/Escritorio/ejemplo-uber-jar$ java -cp target/ejemplo-uber-jar-1.0-SNAPSHOT.jar ar.edu.ucc.App
Hello World!
```

- Sacar conclusiones y analizar posibles soluciones

- Implementar la opción de uber-jar: https://maven.apache.org/plugins/maven-shade-plugin/

```xml
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-shade-plugin</artifactId>
        <version>2.0</version>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>shade</goal>
            </goals>
            <configuration>
              <finalName>${project.artifactId}</finalName>
              <transformers>
                <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                  <mainClass>ar.edu.ucc.App</mainClass>
                </transformer>
              </transformers>
              <minimizeJar>false</minimizeJar>
            </configuration>
          </execution>
        </executions>
      </plugin>
    </plugins>
  </build>
```
- Volver a generar la salida y probar ejecutando

```bash
java -jar target\ejemplo-uber-jar.jar
```

#### 7- Utilizar una IDE
  - Importar el proyecto anterior en Eclipse o Intellij como maven project:
    - Si no dispone de Eclipse puede obtenerlo desde este link http://www.eclipse.org/downloads/packages/release/2018-09/r/eclipse-ide-java-ee-developers
    - Para importar, ir al menú Archivo -> Importar -> Maven -> Proyecto Maven Existente:
![alt text](./import-existing-maven.png)
    - Seleccionar el directorio donde se encuentra el pom.xml que se generó en el punto anterior. Luego continuar:
![alt text](./path-to-pom.png)

Decidi utilizar la IDE Eclipse para realizar las pruebas:

![imagen](https://user-images.githubusercontent.com/48757979/136991652-5b2e33e3-d4d2-47cf-9a83-39d3818a747d.png)


  - Familiarizarse con la interfaz grafica
    - Ejecutar la aplicación
    - Depurar la aplicación
    - Correr unit tests y coverage
    - Ejecutar los goals de maven
    - Encontrar donde se puede cambiar la configuración de Maven.
    - etc.

#### 8- Ejemplo con nodejs

- Instalar Nodejs: https://nodejs.org/en/

- Instalar el componente para generar aplicaciones Express

```bash
npm install express-generator -g
```

- Crear una nueva aplicación
```bash
express --view=ejs hola-mundo
```

- Ejecutar la aplicación

```bash
cd hola-mundo
npm install
npm start
```

- La aplicación web estará disponible en http://localhost:3000

![imagen](https://user-images.githubusercontent.com/48757979/136879850-0d0744f0-8beb-488f-842d-11575a5d88b0.png)


- Analizar el manejo de paquetes y dependencias realizado por npm.

  npm maneja los paquetes de la siguiente forma: Recibe paqueted de los autores de paquetes npm y los distribuye a los usuarios de paquetes npm. Si utilizamos el comando ``` npm install ``` se le entrega al cliente el paquete especificado que se encuentra en el "respositorio" npm, y con ```npm publish``` podemos mandar paquetes para que sean guardados.


#### 9- Ejemplo con python
- Instalar dependencias (Ejemplo Ubuntu) varía según el OS:
```
sudo apt install build-essential python3-dev
pip3 install cookiecutter
```
- Correr el scaffold
```bash
$ cookiecutter https://github.com/candidtim/cookiecutter-flask-minimal.git
application_name [Your Application]: test
package_name [yourapplication]: test
$
```
- Ejecutar la aplicación
```bash
cd test
make run
```
- Acceder a la aplicación en: http://localhost:5000/

![imagen](https://user-images.githubusercontent.com/48757979/136877344-dd515d00-06d1-4f43-8e28-490dccfeb01b.png)

- Explicar que hace una tool como cookiecutter, make y pip.

Cookiecutter: Nos permite crear proyectos utilizando templates. Es decir, que podemos crear nuevos archivos que ya tengan el contenido necesario para nuestro proyecto, sin la necesidad de que empecemos todo de cero. 

Make: es una herramienta que nos permite construir de forma inmediata los programas ejecutables y las librerias. Esto lo hace leyendo los archivos llmados Makefiles que contienen todas las especificaciones sobre lo que se necesita y como crear el proyecto.

Pip: me permite instalar y administrar librerias adicionales y dependencias que no son parte de la libreria estandar de python. 

#### 10- Build tools para otros lenguajes
- Hacer una lista de herramientas de build (una o varias) para distintos lenguajes, por ejemplo (Rust -> cargo)
- Elegir al menos 10 lenguajes de la lista de top 20 o top 50 de tiobe: https://www.tiobe.com/tiobe-index/

 - C++ --> Cmake
 - Python --> Kivy /Pip
 - Java --> Maven
 - PHP --> Composer
 - JavaScript --> Webpack, Grunt
 - Assambly language --> Assambler
 - Groovy --> Gradle
 - Go --> Task
 - Frotran --> Meson
 -  Ruby --> Rake
