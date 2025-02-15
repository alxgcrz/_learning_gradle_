# Gradle

> :warning: **DOCUMENTO EN DESARROLLO** :warning:

## Introducción

Gradle automatiza la construcción, prueba y implementación de software a partir de información en scripts de construcción.

Un proyecto Gradle es una pieza de software que se puede construir, como una aplicación o una biblioteca.

Las construcciones de proyectos únicos incluyen un solo proyecto llamado **proyecto raíz** o **_root project_**. Las construcciones multiproyectos incluyen un proyecto raíz y cualquier número de subproyectos.

Los scripts de construcción le indican a Gradle los pasos necesarios para construir el proyecto. Cada proyecto puede incluir uno o varios scripts de construcción.

La **gestión de la dependencias** es una técnica automatizada para declarar y resolver los recursos externos requeridos por un proyecto. Cada proyecto típicamente incluye una serie de dependencias externas que Gradle resolverá durante la construcción.

Las **tareas** o **_tasks_** son una unidad de trabajo básica, como compilar código o ejecutar su prueba. Cada proyecto contiene una o más tareas definidas dentro de un script de compilación o un plugin.

Gradle tiene soporte para proyectos en Android, Java, Kotlin Multiplatform, Groovy, Scala, Javascript, y C/C++.

## Instalación

Gradle requiere que esté instalado **Java Development Kit (JVM)** versión 8 o superior.

Es recomendable que esté definida la variable **JAVA_HOME** en las variables del sistema. Utilizamos esta variable para añadir al path el directorio _'bin'_ de Java con la forma `%JAVA_HOME%\bin;`

```sh
# CMD
$ echo %JAVA_HOME%
$ echo %PATH%

# PowerShell
$ $env:java_home
$ Get-Item Env:JAVA_HOME
```

Comprobamos si Java está instalado correctamente ejecutando `java -version` en el terminal.

Una vez tenemos Java instalado correctamente se instala Gradle en el sistema. En el caso de Windows, se descarga el fichero .zip de Gradle y se descomprime en la ubicación deseada.

Al igual que Java, se añade la variable **GRADLE_HOME** a las variables del sistema para que apunte al directorio donde se ha descomprimido. Se utiliza esta variable para añadir al path el directorio _'bin'_ de Gradle con la forma `%GRADLE_HOME%\bin;`.

Comprobamos si se ha instalado correctamente ejecutando `gradle -v` en un terminal.

Por defecto, el directorio raíz del usuario se encuentra en '~/.gradle o C:\\Users\\USERNAME\\.gradle' y almacena las propiedades de configuración globales, los scripts de inicialización, ficheros de caché y ficheros de registro.

Se puede establecer con la variable de entorno **GRADLE_USER_HOME**.

- [Más información](https://docs.gradle.org/current/userguide/installation.html)

## Inicialización de un proyecto

La forma de inicializar un proyecto con Gradle o añadir Gradle a un proyecto existente es mediante la tarea **init** de Gradle. Soporta la creación de nuevas construcciones de Gradle de varios tipos, así como la conversión de las construcciones existentes de Apache Maven a Gradle.

```sh
# Ejemplo de comando para inicializar un proyecto
$ gradle init \
  --type java-application \
  --dsl kotlin \
  --test-framework junit-jupiter \
  --package my.project \
  --project-name my-project  \
  --no-split-project  \
  --java-version 17
```

La forma más simple y recomendada de usar la tarea es ejecutar `gradle init` desde una consola interactiva. Gradle irá mostrando diferentes opciones y el usuario deberá ir seleccionando aquellas opciones que más se ajusten al proyecto:

```sh
# Modo interactivo
$ gradle init
```

Hay varias opciones de línea de comandos disponibles para la tarea de inicialización. Para mostrar las opciones disponibles se utiliza la tarea **"help"** de Gradle (disponible también para todas las tareas):

```sh
# Mostrar ayuda de la tarea 'init'
gradle help --task init
```

Esta tarea sirve para diferentes [tipos de compilación de proyectos](https://docs.gradle.org/current/userguide/build_init_plugin.html). Estos se enumeran a continuación:

- [pom](https://docs.gradle.org/current/userguide/build_init_plugin.html#sec:pom_maven_conversion) - Converts an existing Apache Maven build to Gradle
- [basic](https://docs.gradle.org/current/userguide/build_init_plugin.html#sec:basic) - A basic, empty, Gradle build
- [java-application](https://docs.gradle.org/current/userguide/build_init_plugin.html#sec:java_application) - A command-line application implemented in Java
- [java-gradle-plugin](https://docs.gradle.org/current/userguide/build_init_plugin.html#sec:java_gradle_plugin) - A Gradle plugin implemented in Java
- [java-library](https://docs.gradle.org/current/userguide/build_init_plugin.html#sec:java_library) - A Java library
- [kotlin-application](https://docs.gradle.org/current/userguide/build_init_plugin.html#sec:kotlin_application) - A command-line application implemented in Kotlin/JVM
- [kotlin-gradle-plugin](https://docs.gradle.org/current/userguide/build_init_plugin.html#sec:kotlin_gradle_plugin) - A Gradle plugin implemented in Kotlin/JVM
- [kotlin-library](https://docs.gradle.org/current/userguide/build_init_plugin.html#sec:kotlin_library) - A Kotlin/JVM library
- [groovy-application](https://docs.gradle.org/current/userguide/build_init_plugin.html#sec:groovy_application) - A command-line application implemented in Groovy
- [groovy-gradle-plugin](https://docs.gradle.org/current/userguide/build_init_plugin.html#sec:groovy_gradle_plugin) - A Gradle plugin implemented in Groovy
- [groovy-library](https://docs.gradle.org/current/userguide/build_init_plugin.html#sec:groovy_library) - A Groovy library
- [scala-application](https://docs.gradle.org/current/userguide/build_init_plugin.html#sec:scala_application) - A Scala application
- [scala-library](https://docs.gradle.org/current/userguide/build_init_plugin.html#sec:scala_library) - A Scala library
- [cpp-application](https://docs.gradle.org/current/userguide/build_init_plugin.html#sec:cpp_application) - A command-line application implemented in C++
- [cpp-library](https://docs.gradle.org/current/userguide/build_init_plugin.html#sec:cpp_library) - A C++ library

## Gestión de proyectos múltiples

Gradle es capaz de gestionar tanto proyectos monolíticos, es decir, proyectos con un único proyecto raíz como proyectos compuestos por varios subproyectos o submódulos.

En estos casos, el fichero `settings.gradle` se ubica en la raíz del proyecto mientras que cada subproyecto se ubica en su propia carpeta y cuenta con su propio fichero `build.gradle`.

Para visualizar la lista de proyectos en formato árbol, Gradle dispone de la tarea `projects`:

```sh
gradle projects
```

Debido a que cada subproyecto puede tener su propia lista de tareas, hay dos formas de invocar tareas en proyectos compuestos.

Una forma es invocar las tareas por su **nombre simple**. Si estamos en el directorio raíz, al invocar una tarea por nombre invocaremos todas las tareas de los subproyectos que tengan ese nombre. Si estamos ubicados en un subproyecto, únicamente se invocan las tareas de ese subproyecto (y de otros subproyectos que puedan depender de éste) que tengan ese nombre.

Otra forma es invocar las tareas por su **nombre calificado** o _qualified name_. Este nombre se compone de `:{subproyecto}:{tarea}`.

> :warning: El primer `':'` no es un separador com tal sino que se corresponde con el **nombre del proyecto raíz**.

En proyectos que se componen de varios subproyectos, es normal que alguno de los subproyectos tenga una dependencia con otro de los subproyectos.

Para construir un [proyecto con subproyectos](https://docs.gradle.org/current/userguide/intro_multi_project_builds.html) tenemos varias tareas:

```sh
# La tarea 'build' compila, prueba y valida un único proyecto
$ gradle build

# Monta y prueba este proyecto y todos los proyectos que dependen de él.
$ gradle :subproject:buildDependents

# Ensambla y prueba este proyecto y todos los proyectos de los que depende.
$ gradle :subproject:buildNeeded
```

## Ciclo de vida de construcción

TODO

## Fichero de configuración

El archivo de configuración `settings.gradle` en Groovy o `settings.gradle.kts` en Kotlin es el punto de entrada de cada proyecto Gradle.

El objetivo principal del archivo de configuración es añadir subproyectos a su compilación:

- Para proyectos únicos, el fichero de configuración es **opcional**.

- Para proyectos múltiples, el fichero de configuración es **obligatorio** y declara todos los supbroyectos.

Este fichero se encuentra en la raíz del proyecto y acepta los lenguajes [Groovy DSL](https://docs.gradle.org/current/dsl/index.html) y [Kotlin DSL](https://docs.gradle.org/current/kotlin-dsl/index.html).

Un ejemplo de fichero de configuración con tres subproyectos:

```groovy
rootProject.name = 'root-project'   

include('sub-project-a')            
include('sub-project-b')
include('sub-project-c')
```

- [Más información](https://docs.gradle.org/current/userguide/writing_settings_files.html)

## Fichero de construcción

TODO

## Gestión de dependencias

TODO

## Gestión de plugins

TODO

## Tareas

## Gradle Wrapper Reference

La forma recomendada de ejecutar cualquier construcción de Gradle es con la ayuda del **Wrapper** de Gradle.

El **Wrapper** es un script que invoca una versión declarada de Gradle, descargándolo en la carpeta del usuario si es necesario.

En los casos en que hay diversos desarrolladores trabajando en un mismo proyecto, si se utiliza el comando `gradle`, cada desarrollador realizará la compilación del proyecto con su propia versión de Gradle que tenga instalada en su sistema.

En cambio, al usar el **Wrapper** mediante el comando `.\gradlew` se estandariza la versión utilizada, ya que al estar incluido en el control de versiones todos los desarrolladores utilizarán la misma versión de Gradle.

Para añadir el **Wrapper** a un proyecto es necesario que esté instalado Gradle. Sin embargo, una vez añadido a la raíz del proyecto, cualquier otro desarrollador que se descargue el proyecto podrá utilizar este **Wrapper** sin necesidad de tener instalado Gradle en su equipo.

### Adding the Gradle Wrapper

Para añadir el **Wrapper** a un proyecto (si aún no se ha añadido) se requiere que haya una versión de Gradle instalada en la máquina y por tanto que sea accesible vía línea de comandos. Nos situaremos en la raíz del proyecto y ejecutaremos la tarea correspondiente:

```sh
gradle wrapper
```

Esta tarea genera una carpeta con los ficheros `gradle-wrapper.jar` y `gradle-wrapper.properties`. El fichero de propiedades contendrá la URL de la versión de Gradle en uso, además de otra información.

> :warning: Es recomendable añadir estos ficheros al **control de versiones** para compartirlos con el resto de desarrolladores del proyecto.

### Upgrading the Gradle Wrapper

Para actualizar la versión del **Wrapper** se puede modificar la versión directamente en el fichero `gradle-wrapper.properties` aunque la forma recomendada es utilizar la tarea correspondiente:

```sh
# Actualiza el Wrapper a la última versión
$ ./gradlew wrapper --gradle-version latest

# Actualizar una versión específica
$ ./gradlew wrapper --gradle-version 8.4

# Comprobar la versión
$ ./gradlew --version
```

- [Más información](https://docs.gradle.org/current/userguide/gradle_wrapper.html)

## Command-Line Interface Reference

La interfaz de línea de comandos es el método principal de interactuar con Gradle.

Se recomienda el uso del **Wrapper** de Gradle por las ventajas que aporta haciendo uso de `./gradlew` en Linux/macOS o `gradlew.bat` en Windows dentro de la raíz del proyecto. Sin embargo, podemos usar la versión de Gradle instalada en el sistema desde cualquier ubicación con:

- `gradle [taskName...] [--option-name...]`

Las opciones se permiten tanto delante como detrás de la tarea:

- `gradle [--option-name...] [taskName...]`

En el caso de especificar múltiples tareas, se deben separar con un espacio:

- `gradle [taskName1 taskName2...] [--option-name...]`

Ejemplos de comandos:

```sh
# Mostrar la versión de Gradle
$ gradle --version

# Mostrar la ayuda
$ gradle --help

# Mostrar la ayuda de una tarea
$ gradle help --task {tarea}

# Mostrar la lista de tareas desde la raíz del proyecto
$ gradle tasks # normal
$ ./gradlew tasks # usando el wrapper

# Mostrar la lista detallada de tareas
$ gradle tasks --all # normal
$ ./gradlew tasks --all # usando el wrapper
```

- [Más información](https://docs.gradle.org/8.5/userguide/command_line_interface.html)

---

## Enlaces

### Gradle

- 🔸 [Gradle is an open-source build automation tool focused on flexibility and performance](https://gradle.org/)
- 👓 <https://github.com/ksoichiro/awesome-gradle>

### Gradle - Learning

- <https://www.baeldung.com/gradle-guide>
- <https://gradlehero.com/>
- <https://www.john-cd.com/cheatsheets/Java/Gradle/>

## Licencia

[![Licencia de Creative Commons](https://i.creativecommons.org/l/by-sa/4.0/80x15.png)](http://creativecommons.org/licenses/by-sa/4.0/)
Esta obra está bajo una [licencia de Creative Commons Reconocimiento-Compartir Igual 4.0 Internacional](http://creativecommons.org/licenses/by-sa/4.0/).
