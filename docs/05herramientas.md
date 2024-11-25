# Herramientas

??? abstract "Duración y criterios de evaluación"

    Duración estimada: 16 sesiones

    <hr />

    | Resultado de aprendizaje  | Criterios de evaluación   |
    | ------                    | -----                     |
    | 4.- Desarrolla aplicaciones Web embebidas en lenguajes de marcas analizando e incorporando funcionalidades según especificaciones.        | g) Se han utilizado herramientas y entornos para facilitar la programación, prueba y depuración del código.   |

En esta unidad vamos a estudiar algunas de las herramientas más utilizadas en PHP.

## Composer

<figure style="float: right;">
    <img src="imagenes/05/logo-composer.png" width="200">
    <figcaption>Logo Composer</figcaption>
</figure>

Herramienta por excelencia en PHP para la gestión de librerías y dependencias, de manera que instala y las actualiza asegurando que todo el equipo de desarrollo tiene el mismo entorno y versiones. Además, ofrece *autoloading* de nuestro código, de manera que no tengamos que hacerlo nosotros "a mano".

Está escrito en PHP, y podéis consultar toda su documentación en [https://getcomposer.org/](https://getcomposer.org/).

Utiliza [*Packagist*]( https://packagist.org/) como repositorio de librerías.

Funcionalmente, es similar a Maven (Java) / npm (JS).

### Instalación

- Si estamos usando XAMPP, hemos de instalar *Composer* en el propio sistema operativo. Se recomienda seguir las [instrucciones oficiales](https://getcomposer.org/doc/00-intro.md) según el sistema operativo a emplear.

- si usamos *Docker*, necesitamos modificar la configuración de nuestro contenedor. En nuestro caso, hemos decidido modificar el archivo `Dockerfile` y añadir el siguiente comando:

    ``` docker
    COPY --from=composer:2.0 /usr/bin/composer /usr/local/bin/composer
    ```

    Es importante que dentro del contenedor comprobemos que tenemos la v2:

    ``` bash
    composer -V
    ```

- Si usamos *Laravel Herd* ya viene instalado por defecto.

!!! tip "Ver capítulo: Composer y PSR-4"
    En este [capítulo](https://drive.google.com/file/d/1dADn_w8VDr4NOvUav7P0Npj_nJYCIoTz/view?usp=sharing) del curso "Crear aplicación Web" se explica el concepto de Composer y cómo utilizarlo para el autoload de clases.

### Primeros pasos

Cuando creemos un proyecto por primera vez, hemos de inicializar el repositorio. Para ello, ejecutaremos el comando `composer init` donde:

* Configuramos el nombre del paquete, descripción, autor (nombre <email>), tipo de paquete (project), etc...
* Definimos las dependencias del proyecto (`require`) y las de desarrollo (`require-dev`) de manera interactiva.
    * En las de desarrollo se indica aquellas que no se instalarán en el entorno de producción, por ejemplo, las librerías de pruebas.

Tras su configuración, se creará automáticamente el archivo `composer.json` con los datos introducidos y descarga las librerías en la carpeta `vendor`. La instalación de las librerías siempre se realiza de manera local para cada proyecto.

``` json
{
    "name": "dwes/log",
    "description": "Pruebas con Monolog",
    "type": "project",
    "require": {
        "monolog/monolog": "^2.1"
    },
    "license": "MIT",
    "authors": [
        {
            "name": "Aitor Medrano",
            "email": "a.medrano@edu.gva.es"
        }
    ]
}
```

A la hora de indicar cada librería introduciremos:

* el nombre de la librería, compuesta tanto por el creador o "vendor", como por el nombre del proyecto. Ejemplos: `monolog/monolog` o `laravel/installer`.
* la versión de cada librería. Tenemos diversas opciones para indicarla:

    * Directamente: 1.4.2
    * Con comodines: 1.*
    * A partir de: >= 2.0.3
    * Sin rotura de cambios: ^1.3.2 // >=1.3.2 <2.0.0

### Actualizar librerías

Podemos definir las dependencias via el archivo `composer.json` o mediante comandos con el formato `composer require vendor/package:version`. Por ejemplo, si queremos añadir phpUnit como librería de desarrollo, haremos:

``` bash
composer require phpunit/phpunit –dev
```

Tras añadir nuevas librerías, hemos de actualizar nuestro proyecto:

``` bash
composer update
```

Si creamos el archivo `composer.json` nosotros directamente sin inicializar el repositorio, hemos de instalar las dependencias:

``` bash
composer install
```

Al hacer este paso (tanto instalar como actualizar), como ya hemos comentado, se descargan las librerías en dentro de la carpeta `vendor`. Es muy importante añadir esta carpeta al archivo `.gitignore` para no subirlas a GitHub.

Además se crea el archivo `composer.lock`, que almacena la versión exacta que se ha instalado de cada librería (este archivo no se toca).

### `autoload.php`

*Composer* crea de forma automática en `vendor/autoload.php` el código para incluir de forma automática todas las librerías que tengamos configuradas en `composer.json`.

Para utilizarlo, en la cabecera de nuestro archivos pondremos:

``` php
<?php
require 'vendor/autoload.php';
```

En nuestro caso, de momento sólo lo podremos en los archivos donde probamos las clases

Si queremos que Composer también se encargue de cargar de forma automática nuestras clases de dominio, dentro del archivo `composer.json`, definiremos la propiedad `autoload`:

``` json
"autoload": {
    "psr-4": {"Dwes\\": "app/Dwes"}
},
```

Posteriormente, hemos de volver a generar el *autoload* de *Composer* mediante la opción `dump-autoload` (o `du`):

``` bash
composer dump-autoload
```

## Monolog

Vamos a probar *Composer* añadiendo la librería de [*Monolog*](https://github.com/Seldaek/monolog) a nuestro proyecto. Se trata de un librería para la gestión de logs de nuestras aplicaciones, soportando diferentes niveles (info, warning, etc...), salidas (ficheros, sockets, BBDD, Web Services, email, etc) y formatos (texto plano, HTML, JSON, etc...).

Para ello, incluiremos la librería en nuestro proyecto con:

``` bash
composer require monolog/monolog
```

Monolog 2 requiere al menos PHP 7.2, cumple con el estandar de logging PSR-3, y es la librería empleada por *Laravel* y *Symfony* para la gestión de logs.

!!! info "Cuando usar un log"
    * Seguir las acciones/movimientos de los usuarios
    * Registrar las transacciones
    * Rastrear los errores de usuario
    * Fallos/avisos a nivel de sistema
    * Interpretar y coleccionar datos para posterior investigación de patrones

### Niveles

A continuación mostramos los diferentes niveles de menos a más restrictivo:

* *debug -100*: Información detallada con propósitos de debug. No usar en entornos de producción.
* *info - 200*: Eventos interesantes como el inicio de sesión de usuarios.
* *notice - 250*: Eventos normales pero significativos.
* *warning - 300*: Ocurrencias excepcionales que no llegan a ser error.
* *error - 400*: Errores de ejecución que permiten continuar con la ejecución de la aplicación pero que deben ser monitorizados.
* *critical - 500*: Situaciones importantes donde se generan excepciones no esperadas o no hay disponible un componente.
* *alert - 550*: Se deben tomar medidas inmediatamente.
Caída completa de la web, base de datos no disponible, etc... Además, se suelen enviar mensajes por email.
* *emergency - 600*: Es el error más grave e indica que todo el sistema está inutilizable.

### Hola Monolog

Por ejemplo, en el archivo `pruebaLog.php` que colocaríamos en el raíz, primero incluimos el *autoload*, importamos las clases a utilizar para finalmente usar los métodos de *Monolog*:

``` php
<?php
include __DIR__ ."/vendor/autoload.php";

use Monolog\Logger;
use Monolog\Handler\StreamHandler;

$log = new Logger("MiLogger");
$log->pushHandler(new StreamHandler("logs/milog.log", Logger::DEBUG));

$log->debug("Esto es un mensaje de DEBUG");
$log->info("Esto es un mensaje de INFO");
$log->warning("Esto es un mensaje de WARNING");
$log->error("Esto es un mensaje de ERROR");
$log->critical("Esto es un mensaje de CRITICAL");
$log->alert("Esto es un mensaje de ALERT");
```

En todos los métodos de registro de mensajes (`debug`, `info`, ...), además del propio mensaje, le podemos pasar información como el contenido de alguna variable, usuario de la aplicación, etc.. como segundo parámetro dentro de un array, el cual se conoce como **array de contexto**. Es conveniente hacerlo mediante un array asociativo para facilitar la lectura del log.

``` php
<?php
$log->warning("Producto no encontrado", [$producto]);
$log->warning("Producto no encontrado", ["datos" => $producto]);
```

### Funcionamiento

Cada instancia `Logger` tiene un nombre de canal y una pila de manejadores (*handler*).
Cada mensaje que mandamos al log atraviesa la pila de manejadores, y cada uno decide si debe registrar la información, y si se da el caso, finalizar la propagación.
Por ejemplo, un `StreamHandler` en el fondo de la pila que lo escriba todo en disco, y en el tope añade un `MailHandler` que envíe un mail sólo cuando haya un error.

### Manejadores

Cada manejador tambien tiene un formateador (`Formatter`). Si no se indica ninguno, se le asigna uno por defecto. El último manejador insertado será el primero en ejecutarse.
Luego se *van ejecutando* conforme a la pila.

Los manejadores más utilizados son:

* `StreamHandler(ruta, nivel)`
* `RotatingFileHandler(ruta, maxFiles, nivel)`
* `NativeMailerHandler(para, asunto, desde, nivel)`
* `FirePHPHandler(nivel)`

Si queremos que los mensajes de la aplicación salgan por el log del servidor, 
en nuestro caso el archivo `error.log` de *Apache* utilizaremos como ruta la salida de error:

``` php
<?php
// error.log
$log->pushHandler(new StreamHandler("php://stderr", Logger::DEBUG));
//Consola
$log->pushHandler(new StreamHandler("php://stdout", Logger::DEBUG));
```

!!! tip "FirePHP"
    Por ejemplo, mediante `FirePHPHandler`, podemos utilizar `FirePHP`, la cual es una herramienta para hacer debug en la consola de *Firefox*.
    Tras instalar la extensión en Firefox, habilitar las opciones y configurar el *Handler*, podemos ver los mensajes coloreados con sus datos:

    ``` php
    <?php
    $log = new Logger("MiFirePHPLogger");
    $log->pushHandler(new FirePHPHandler(Logger::INFO));

    $datos = ["real" => "Bruce Wayne", "personaje" => "Batman"];
    $log->debug("Esto es un mensaje de DEBUG", $datos);
    $log->info("Esto es un mensaje de INFO", $datos);
    $log->warning("Esto es un mensaje de WARNING", $datos);
    // ...
    ```

    <figure style="align: center;">
        <img src="imagenes/05/firePhp.png">
        <figcaption>Ejemplo de uso de FirePHP</figcaption>
    </figure>

### Canales

Se les asigna al crear el `Logger`. En grandes aplicaciones, se crea un canal por cada subsistema: ventas, contabilidad, almacén.
No es una buena práctica usar el nombre de la clase como canal, esto se hace con un *procesador*.

Para su uso, es recomiendo asignar el log a una propiedad privada a Logger, y posteriormente, en el constructor de la clase, asignar el canal, manejadores y formato.

``` php
<?php
$this->log = new Logger("MiApp");
$this->log->pushHandler(new StreamHandler("logs/milog.log", Logger::DEBUG));
$this->log->pushHandler(new FirePHPHandler(Logger::DEBUG));
```

Y dentro de los métodos para escribir en el log:

``` php
<?php
$this->log->warning("Producto no encontrado", [$producto]);
```

### Procesadores

Los procesadores permiten añadir información a los mensajes. Para ello, se apilan después de cada manejador mediante el método `pushProcessor($procesador)`.

Algunos procesadores conocidos son `IntrospectionProcessor` (muestran la linea, fichero, clase y metodo desde el que se invoca el log), `WebProcessor` (añade la URI, método e IP) o `GitProcessor` (añade la rama y el commit).

=== "PHP"

    ``` php
    <?php
    $log = new Logger("MiLogger");
    $log->pushHandler(new RotatingFileHandler("logs/milog.log", 0, Logger::DEBUG));
    $log->pushProcessor(new IntrospectionProcessor());
    $log->pushHandler(new StreamHandler("php://stderr", Logger::WARNING));
    // no usa Introspection pq lo hemos apilado después, le asigno otro
    $log->pushProcessor(new WebProcessor());
    ```

=== "Consola en formato texto"

    ``` log
    [2020-11-26T13:35:31.076138+01:00] MiLogger.DEBUG: Esto es un mensaje de DEBUG [] {"file":"C:\\xampp\\htdocs\\log\\procesador.php","line":12,"class":null,"function":null}
    [2020-11-26T13:35:31.078344+01:00] MiLogger.INFO: Esto es un mensaje de INFO [] {"file":"C:\\xampp\\htdocs\\log\\procesador.php","line":13,"class":null,"function":null}
    ```

### Formateadores

Se asocian a los manejadores con `setFormatter`. Los formateadores más utilizados son `LineFormatter`, `HtmlFormatter` o `JsonFormatter`.

=== "PHP"

    ``` php
    <?php
    $log = new Logger("MiLogger");
    $rfh = new RotatingFileHandler("logs/milog.log", Logger::DEBUG);
    $rfh->setFormatter(new JsonFormatter());
    $log->pushHandler($rfh);
    ```

=== "Consola en JSON"

    ``` json
    {"message":"Esto es un mensaje de DEBUG","context":{},"level":100,"level_name":"DEBUG","channel":"MiLogger","datetime":"2020-11-27T15:36:52.747211+01:00","extra":{}}
    {"message":"Esto es un mensaje de INFO","context":{},"level":200,"level_name":"INFO","channel":"MiLogger","datetime":"2020-11-27T15:36:52.747538+01:00","extra":{}}
    ```

!!! tip "Más información"
    Más información sobre manejadores, formateadores y procesadores en <https://github.com/Seldaek/monolog/blob/master/doc/02-handlers-formatters-processors.md>

### Uso de Factorías

En vez de instanciar un log en cada clase, es conveniente crear una factoría (por ejemplo, siguiendo la idea del patrón de diseño [*Factory Method*](https://refactoring.guru/es/design-patterns/factory-method)).

Para el siguiente ejemplo, vamos a suponer que creamos la factoría en el *namespace* `Dwes\Ejemplos\Util`.

``` php
<?php
namespace Dwes\Ejemplos\Util

use Monolog\Logger;
use Monolog\Handler\StreamHandler;

class LogFactory {

    public static function getLogger(string $canal = "miApp") : Logger {
        $log = new Logger($canal);
        $log->pushHandler(new StreamHandler("logs/miApp.log", Logger::DEBUG));

        return $log;
    }
}
```

Si en vez de devolver un `Monolog\Logger` utilizamos el interfaz de PSR, si en el futuro cambiamos la implementación del log, no tendremos que modificar nuestro codigo. Así pues, la factoría ahora devolverá `Psr\Log\LoggerInterface`:

``` php
<?php
namespace Dwes\Ejemplos\Util

use Monolog\Handler\StreamHandler;
use Monolog\Logger;
use Psr\Log\LoggerInterface;

class LogFactory {

    public static function getLogger(string $canal = "miApp") : LoggerInterface {
        $log = new Logger($canal);
        $log->pushHandler(new StreamHandler(__DIR__ . "/log/miApp.log", Logger::DEBUG));

        return $log;
    }
}
```

Finalmente, para utilizar la factoría, sólo cambiamos el código que teníamos en el constructor de las clases que usan el log, quedando algo asi:

``` php
<?php

namespace Dwes\Ejemplos\Model;

use Dwes\Ejemplos\Util\LogFactory;
use Monolog\Logger;

class Cliente {

    private $codigo; 

    private Logger $log;

    function __construct($codigo){ 
        $this->codigo=$codigo; 

        $this->log = LogFactory::getLogger();
    }

    /// ... resto del código
}
```

**Rotando ficheros de log**

El principal problema con logging en un archivo es que, con el tiempo, el archivo puede volverse demasiado grande y complicado de administrar, además de ocupar una gran cantidad de almacenamiento. Este problema se resuelve tradicionalmente mediante la rotación de archivos.

Monolog también ofrece una solución integrada para rotar registros, aunque no está pensada para su uso en entornos de producción. 

En el siguiente ejemplo, se ha utilizado un `RotatingFileHandler` que rotará el archivo de log si el fichero es más antiguo de 30 días. También es posible especificar otros criterios de rotación, para ello consulta la documentación de Monolog.

```php	
<?php

. . .
use Monolog\Handler\RotatingFileHandler;

$rotating_handler = new RotatingFileHandler(__DIR__ . "/log/debug.log", 30, Level::Debug);
$logger->pushHandler($rotating_handler);

$logger->info("This file has been executed.");
?>
```

**Captura y logando excepciones**

En PHP las excepciones son controladoras por `try, catch, finally`. Si queremos logar una excepción, podemos hacerlo de la siguiente manera:

```php
$logger = new Logger("exceptions");

$stream_handler = new StreamHandler(__DIR__ . "/log/exception.log", Level::Debug);
$stream_handler->setFormatter(new JsonFormatter());

$logger->pushHandler($stream_handler);

try {
    $username = readline("Choose your username: ");
    if (strlen($username) < 6) {
        throw new Exception("The username $username is too short.");
    }
} catch (exception $e) {
    // Log el mensaje de la excepción
    $logger->error($e->getMessage());
    //Si queremos logar todo el objeto de la excepción
    $logger->error($e);
    //También es posible combinar las 2 opciones, logar el mensaje y el objeto de la excepción.
    // array('exception' => $e) es un contexto adicional que se añade al log.
    $logger->error($e->getMessage(), array('exception' => $e));
}
```

**Tratando con excepciones no capturadas**

Si una excepción no es capturada por un bloque `try, catch`, la excepción "burbujeará" hasta el script principal y si no existe un `block` en este nivel, PHP terminará la ejecución del script inmediatamente con un error "Fatal".

Podemos manejar este tipo de errores utilizando un manejador de excepciones personalizado. Para ello, debemos implementar la interfaz `Throwable` y capturar la excepción en el método `handle`:

```php
<?php

use Monolog\Logger;
use Monolog\Handler\StreamHandler;

class ExceptionHandler implements Throwable
{
    public function handle(Throwable $e)
    {
        $logger = new Logger("exceptions");
        $stream_handler = new StreamHandler(__DIR__ . "/log/exception.log", Level::Debug);
        $stream_handler->setFormatter(new JsonFormatter());
        $logger->pushHandler($stream_handler);
        $logger->error($e->getMessage());
    }
}

// Registramos el manejador de excepciones, e indicamos que el método handle será el encargado de gestionar las excepciones.
set_exception_handler([new ExceptionHandler(), 'handle']);
```

Ahora el `ExceptionHandler` se encargará de gestionar todas las excepciones no capturadas en el script. Cuando se produzca una excepción, el método `handle` se encargará de logar el mensaje de la excepción en el archivo `exception.log`, y PHP no mostrará una excepción fatal pero *Sí* terminará el script. 

Para el caso de una Web, la `Request` se terminará y debemos ser nosotros los que tratemos la respuesta al cliente.


**Enviar logs a múltiples destinos**

Monolog permite enviar logs a múltiples destinos, normalmente a través del tipo de severidad, podemos indicar un destino u otro. En el siguiente ejemplo, se envían los logs de nivel `DEBUG` a un fichero, los de nivel `NOTICE` a otro fichero y los de nivel `ALERT` a una base de datos.

```php
<?php

require __DIR__ . "/vendor/autoload.php";
require "./DBHandler.php";

use Monolog\Level;
use Monolog\Logger;
use Monolog\Formatter\JsonFormatter;
use Monolog\Handler\StreamHandler;
use Monolog\Handler\RotatingFileHandler;

// New Logger instance
$logger = new Logger("my_logger");
$formatter = new JsonFormatter();

// Create new handler
$rotating_handler = new RotatingFileHandler(__DIR__ . "/log/debug.log", 30, Level::Debug);
$stream_handler = new StreamHandler(__DIR__ . "/log/notice.log", Level::Notice);
$db_handler = new DBHandler(new PDO('sqlite:alert.sqlite'), Level::Alert);

$stream_handler->setFormatter($formatter);
$db_handler->setFormatter($formatter);
$rotating_handler->setFormatter($formatter);

// Push the handler to the log channel
$logger->pushHandler($stream_handler);
$logger->pushHandler($rotating_handler);
$logger->pushHandler($db_handler);

// Log the message
$logger->info("This file has been executed.");
$logger->error("An error occurred.");
$logger->critical("This application is in critical condition!!");
$logger->emergency("This is an EMERGENCY!!!");
?>
```

Aunque esta aproximación está bien, en un sistema medianamente complejo puede ser muy complicado tener que monitorizar tantos lugares. En estos casos, es recomendable utilizar un sistema de monitorización de logs, como *BetterStack*, *Loggly*, *Papertrail* o *Logstash*.


En este ejemplo, vamos a utilizar la plataforma *BetterStack* para enviar los logs de nuestra aplicación. Para ello, sigue los siguientes pasos:

1. Instalar la librería cliente de BetterStack en tu proyecto:
    ```bash
    composer require logtail/monolog-logtail
    ```
2. Configurar el logger de Monolog con BetterStack.
   ```php
    require "../vendor/autoload.php";

    use Monolog\Logger;
    use Logtail\Monolog\LogtailHandlerBuilder;

    $logger = new Logger("example-app");
    //Reempalzar $SOURCE_TOKEN por el token de tu proyecto en BetterStack
    $handler = LogtailHandlerBuilder::withSourceToken("$SOURCE_TOKEN")
    ->build();
    $logger->pushHandler($handler);
    ```	
3. Utilizar el logger de Monolog como de costumbre.
   ```php
    $logger->info("This file has been executed.");
    $logger->error("An error occurred.");
    $logger->critical("This application is in critical condition!!");
    $logger->emergency("This is an EMERGENCY!!!");
    ```

Para obtener un `token` de BetterStack, simplemente registrar una cuenta (tienen un plan gratuito) y obteneer un token para tu proyecto desde el siguiente [enlace](https://telemetry.betterstack.com/team/0/sources?_gl=1*po1zew*_gcl_au*MjEwOTk2Mzg4My4xNzMyNTU5MzE5*_ga*MTkwMDUwNTQwNy4xNzMyNTU5MzE4*_ga_9FLKD0MQYY*MTczMjU2MjgyNC4yLjEuMTczMjU2MzIwNC4wLjAuMA..).

!!! tip "Ver capítulo: Configurando un logger con Monolog"
    En este [capítulo](https://drive.google.com/file/d/1dADn_w8VDr4NOvUav7P0Npj_nJYCIoTz/view?usp=sharing) del curso "Crear aplicación Web" vemos como configurar un logger en nuestra aplicación y como enviar los logs a un servidor de logs.




## Documentación con *phpDocumentor*

[phpDocumentor](https://www.phpdoc.org/) es la herramienta *de facto* para documentar el código PHP. Es similar en propósito y funcionamiento a *Javadoc*.

Así pues, es un herramienta que facilita la documentación del código PHP, creando un sitio web con el API de la aplicación.

Se basa en el uso de anotaciones sobre los docblocks. Para ponerlo en marcha, en nuestro caso nos decantaremos por utilizar la imagen que ya existe de Docker.

### Instalación como binario

Otra opción es seguir los pasos que recomienda la [documentación oficial](https://www.phpdoc.org) para instalarlo como un ejecutable, que son descargar el archivo `phpDocumentor.phar` y darles permisos de ejecución:

``` bash
wget https://phpdoc.org/phpDocumentor.phar
chmod +x phpDocumentor.phar
mv phpDocumentor.phar /usr/local/bin/phpdoc
phpdoc --version
```

Una vez instalado, desde el raíz del proyecto, suponiendo que tenemos nuestro código dentro de `app` y que queremos la documentación dentro de `docs/api` ejecutamos:

``` bash
phpdoc -d ./app -t docs/api
```

### Uso en Docker

En el caso de usar *Docker*, usaremos el siguiente comando para ejecutarlo (crea el contenedor, ejecuta el comando que le pidamos, y automáticamente lo borra):

``` bash
docker run --rm -v "$(pwd)":/data phpdoc/phpdoc:3
```

A dicho comando, le adjuntaremos los diferentes parámetros que admite phpDocumentor, por ejemplo:

``` bash
# Muestra la versión
docker run --rm -v "$(pwd)":/data phpdoc/phpdoc:3 --version
# Mediante -d se indica el origen a parsear
# Mediante -t se indica el destino donde generar la documentación
docker run --rm -v "$(pwd)":/data phpdoc/phpdoc:3 -d ./src/app -t ./docs/api
```

### DocBlock

Un *docblock* es el bloque de código que se coloca encima de un recurso. Su formato es:

``` php
<?php
/**
* *Sumario*, una sola línea
*
* *Descripción* que puede utilizar varias líneas
* y que ofrece detalles del elemento o referencias
* para ampliar la información
*
* @param string $miArgumento con una *descripción* del argumento
* que puede usar varias líneas.
*
* @return void
*/
function miFuncion(tipo $miArgumento)
{
}
```

### Documentando el código

En todos los elementos, ademas del sumario y/o descripción, pondremos:

* En las clases:
    * `@author` nombre <email>
    * `@package` ruta del namespace
* En las propiedades:
    * `@var` tipo descripción
* En los métodos:
    * `@param` tipo $nombre descripción
    * `@throws` ClaseException descripción
    * `@return` tipo descripción

Veámoslo con un ejemplo. Supongamos que tenemos una clase que representa un cliente:

``` php
<?php
/**
* Clase que representa un cliente
* 
* El cliente se encarga de almacenar los soportes que tiene alquilado,
* de manera que podemos alquilar y devolver productos mediante las operaciones
* homónimas.
* 
* @package Dwes\Videoclub\Model
* @author Aitor Medrano <a.medrano@edu.gva.es>
*/
class Cliente {

    public string $nombre; 
    private string $numero;

    /**
    * Colección de soportes alquilados
    * @var array<Soporte> 
    */
    private $soportesAlquilados[];

    /**
    * Comprueba si el soporte recibido ya lo tiene alquilado el cliente
    * @param Soporte $soporte Soporte a comprobar
    * @return bool true si lo tiene alquilado
    */
    public function tieneAlquilado(Soporte $soporte) : bool { 
        // ...
    }
```

Si generamos la documentación y abrimos con un navegador el archivo `docs/api/index.html` podremos navegar hasta la clase `Cliente:

<figure style="align: center;">
    <img src="imagenes/05/phpdoc.png">
    <figcaption>phpDocumentor de Cliente</figcaption>
</figure>

## *Web Scraping*

Consiste en navegar a una página web y extraer información automáticamente, a partir del código HTML generado, y organizar la información pública disponible en Internet.

Esta práctica requiere el uso de una librería que facilite la descarga de la información deseada imitando la interacción de un navegador web. Este "robot" puede acceder a varias páginas simultáneamente.

!!! question "¿Es legal?"
    Si el sitio web indica que tiene el contenido protegido por derechos de autor o en las normas de acceso via usuario/contraseña nos avisa de su prohibición, estaríamos incurriendo en un delito.
    Es recomendable estudiar el archivo `robots.txt` que se encuentra en el raíz de cada sitio web.
    Más información en el artículo [El manual completo para el web scraping legal y ético en 2021](https://ichi.pro/es/el-manual-completo-para-el-web-scraping-legal-y-etico-en-2021-69178542830388)

### Goutte

[Goutte](https://github.com/FriendsOfPHP/Goutte) es un sencillo cliente HTTP para PHP creado específicamente para hacer web scraping. Lo desarrolló el mismo autor del framework *Symfony* y ofrece un API sencilla para extraer datos de las respuestas HTML/XML de los sitios web.

FIXME: Revisar https://godofredo.ninja/web-scraping-con-php-utilizando-goutte/

Los componentes principales que abstrae *Goutte* sobre *Symfony* son:

* `BrowserKit`: simula el comportamiento de un navegador web.
* `CssSelector`: traduce consultas CSS en consultas XPath.
* `DomCrawler`: facilita el uso del DOM y XPath.

Para poder utilizar *Goutte* en nuestro proyecto, ejecutaremos el siguiente comando en el terminal:

``` bash
composer require fabpot/goutte
```

### Goutte con selectores CSS

A continuación vamos a hacer un ejemplo muy sencillo utilizando los selectores CSS, extrayendo información de la web `https://books.toscrape.com/`, la cual es una página para pruebas que no rechazará nuestras peticiones.

Tras crear un cliente con *Goutte*, hemos de realizar un petición a una URL. Con la respuesta obtenida, podemos utilizar el método `filter` para indicarle la ruta CSS que queremos recorrer e iterar sobre los resultados mediante una función anónima. Una vez estamos dentro de un determinado nodo, el método `text()` nos devolverá el contenido del propio nodo.

En concreto, vamos a meter en un array asociativo el título y el precio de todos los libros de la categoría *Classics*.

``` php
<?php
require '../vendor/autoload.php';

$httpClient = new \Goutte\Client();
$response = $httpClient->request('GET', 'https://books.toscrape.com/catalogue/category/books/classics_6/index.html');
// colocamos los precios en un array
$precios = [];
$response->filter('.row li article div.product_price p.price_color')->each(
    // le pasamos $precios por referencia para poder editarla dentro del closure
    function ($node) use (&$precios) {
        $precios[] = $node->text();
    }
);

// colocamos el nombre y el precio en un array asociativo
$contadorPrecios = 0;
$libros = [];
$response->filter('.row li article h3 a')->each(
    function ($node) use ($precios, &$contadorPrecios, &$libros) {
        $libros[$node->text()] = $precios[$contadorPrecios];
        $contadorPrecios++;
    }
);
```

### Crawler

Un caso muy común es obtener la información de una página que tiene los resultados paginados, de manera que vayamos recorriendo los enlaces y accediendo a cada uno de los resultados.

En este caso vamos a coger todos los precios de los libros de fantasía, y vamos a sumarlos:

``` php
<?php
require '../vendor/autoload.php';

use Goutte\Client;
use Symfony\Component\HttpClient\HttpClient;

$client = new Client(HttpClient::create(['timeout' => 60]));
$crawler = $client->request('GET', 'https://books.toscrape.com/catalogue/category/books/fantasy_19/index.html');

$salir = false;

$precios = [];
while (!$salir) {
    $crawler->filter('.row li article div.product_price p.price_color')->each(
        function ($node) use (&$precios) {
            $texto = $node->text();
            $cantidad = substr($texto, 2); // Le quitamos las libras ¿2 posiciones?
            $precios[] = floatval($cantidad);
        }
    );

    $enlace = $crawler->selectLink('next');
    if ($enlace->count() != 0) {
        // el enlace next existe
        $sigPag = $crawler->selectLink('next')->link();
        $crawler = $client->click($sigPag); // hacemos click
    } else {
        // ya no hay enlace next
        $salir = true;
    }
}

$precioTotal = array_sum($precios);
echo $precioTotal;
```

## Pruebas con PHPUnit

El curso pasado, dentro del módulo de *Entornos de Desarrollo*, estudiamos la importancia de la realización de pruebas, así como las pruebas unitarias mediante [JUnit](https://junit.org/junit5/).

<figure style="float: right;">
    <img src="imagenes/05/tdd.png" width="300">
    <figcaption>Test Driven Development</figcaption>
</figure>

A día de hoy es de gran importancia seguir una buena metodología de pruebas, siendo el desarrollo dirigido por las pruebas (*Test Driven Development* / TDD) uno de los enfoques más empleados, el cual consiste en:

1. Escribir el test, y como no hay código implementado, la prueba falle (rojo).
2. Escribir el código de aplicación para que la prueba funcione (verde).
3. Refactorizar el código de la aplicación con la ayuda de la prueba para comprobar que no rompemos nada (refactor).

En el caso de PHP, la herramienta que se utiliza es *PHPUnit* (<https://phpunit.de/>), que como su nombre indica, está basada en JUnit. La versión actual es la 9.0

Se recomienda consultar su documentación en <https://phpunit.readthedocs.io/es/latest/index.html>.

### Puesta en marcha

Vamos a colocar todas las pruebas en una carpeta `tests` en el raíz de nuestro proyecto.

En el archivo `composer.json`, añadimos:

``` json
"require-dev": {
    "phpunit/phpunit": "^9"
},
"scripts": {
    "test": "phpunit --testdox --colors tests"
}
```

Si quisiéramos añadir la librería desde un comando del terminal, también podríamos ejecutar:

``` bash
composer require --dev phpunit/phpunit ^9
```

!!! tip "Librerías de desarrollo"
    Las librerías que se colocan en `require-dev` son las de desarrollo y *testing*, de manera que no se instalarán en un entorno de producción.

Como hemos creado un *script*, podemos lanzar las pruebas mediante:

``` bash
composer test
```

Vasmos a realizar nuestra primera prueba:

``` php
<?php
use PHPUnit\Framework\TestCase;

class PilaTest extends TestCase
{
    public function testPushAndPop()
    {
        $pila = [];
        $this->assertSame(0, count($pila));

        array_push($pila, 'batman');
        $this->assertSame('batman', $pila[count($pila)-1]);
        $this->assertSame(1, count($pila));

        $this->assertSame('batman', array_pop($pila));
        $this->assertSame(0, count($pila));
    }
}
```

Tenemos diferentes formas de ejecutar una prueba:

``` bash
./vendor/bin/phpunit tests/PilaTest.php
./vendor/bin/phpunit tests
./vendor/bin/phpunit --testdox tests
./vendor/bin/phpunit --testdox --colors tests
```

### Diseñando pruebas

Tal como hemos visto en el ejemplo, la clase de prueba debe heredar de `TestCase`, y el nombre de la clase debe acabar en `Test`, de ahí que hayamos llamado la clase de prueba como `PilaTest`.

Una prueba implica un método de prueba (público) por cada funcionalidad a probar. Cada un de los métodos se les asocia un caso de prueba.

Los métodos deben nombrarse con el prefijo `test`, por ejemplo, `testPushAndPop`. Es muy importante que el nombre sea muy claro y descriptivo del propósito de la prueba. (*camelCase*).

En los casos de prueba prepararemos varias aserciones para toda la casuística: rangos de valores, tipos de datos, excepciones, etc...

### Aserciones

Las aserciones permiten comprobar el resultado de los métodos que queremos probar. Las aserciones esperan que el predicado siempre sea verdadero.

PHPUnit ofrece las siguiente aserciones:

* `assertTrue` / `assertFalse`: Comprueba que la condición dada sea evaluada como true / false
* `assertEquals` / `assertSame`: Comprueba que dos variables sean iguales
* `assertNotEquals` / `assertNotSame`: Comprueba que dos variables NO sean iguales
    * `Same` → comprueba los tipos. Si no coinciden los tipos y los valores, la aserción fallará
    * `Equals` → sin comprobación estricta
* `assertArrayHasKey` / `assertArrayNotHasKey`: Comprueba que un array posea un *key* determinado / o NO lo posea
* `assertArraySubset`: Comprueba que un array posea otro array como subset del mismo
* `assertAttributeContains` / `assertAttributeNotContains`: Comprueba que un atributo de una clase contenga una variable determinada / o NO contenga una variable determinada
* `assertAttributeEquals`: Comprueba que un atributo de una clase sea igual a una variable determinada.

### Comparando la salida

Si los métodos a probar generan contenido mediante `echo` o una instrucción similar, disponemos de las siguiente expectativas:

* `expectOutputString(salidaEsperada)`
* `expectOutputRegex(expresionRegularEsperada)`

Las expectativas difieren de las aserciones que informan del resultado que se espera antes de invocar al método. Tras definir la expectativa, se invoca al método que realiza el `echo`/`print`.

``` php
<?php
namespace Dwes\Videoclub\Model;

use PHPUnit\Framework\TestCase;
use Dwes\Videoclub\Model\CintaVideo;

class CintaVideoTest extends TestCase {
    public function testConstructor()
    {
        $cinta = new CintaVideo("Los cazafantasmas", 23, 3.5, 107); 
        $this->assertSame( $cinta->getNumero(), 23);
    }

    public function testMuestraResumen()
    {
        $cinta = new CintaVideo("Los cazafantasmas", 23, 3.5, 107);
        $resultado = "<br>Película en VHS:";
        $resultado .= "<br>Los cazafantasmas<br>3.5 (IVA no incluido)";
        $resultado .= "<br>Duración: 107 minutos";
        // definimos la expectativa
        $this->expectOutputString($resultado);
        // invocamos al método que hará echo
        $cinta->muestraResumen();
    }
}
```

### Proveedores de datos

Cuando tenemos pruebas que solo cambian respecto a los datos de entrada y de salida, es útil utilizar proveedores de datos.

Se declaran en el docblock mediante `@dataProvider nombreMetodo`, donde se indica el nombre de un método público que devuelve un array de arrays, donde cada elemento es un caso de prueba.

La clase de prueba recibe como parámetros los datos a probar y el resultado de la prueba como último parámetro.

El siguiente ejemplo comprueba con diferentes datos el funcionamiento de `muestraResumen`:

``` php
<?php
/**
 * @dataProvider cintasProvider
 */
public function testMuestraResumenConProvider($titulo, $id, $precio, $duracion, $esperado)
{
    $cinta = new CintaVideo($titulo, $id, $precio, $duracion);
    $this->expectOutputString($esperado);
    $cinta->muestraResumen();
}

public function cintasProvider() {
    return [
        "cazafantasmas" => ["Los cazafantasmas", 23, 3.5, 107, "<br>Película en VHS:<br>Los cazafantasmas<br>3.5 €(IVA no incluido)<br>Duración: 107 minutos"],
        "superman" => ["Superman", 24, 3, 188, "<br>Película en VHS:<br>Superman<br>3 € (IVA no incluido)<br>Duración: 188 minutos"],
    ];
}
```

### Probando excepciones

Las pruebas además de comprobar que las clases funcionan como se espera,  han de cubrir todos los casos posibles. Así pues, debemos poder hacer pruebas que esperen que se lance una excepción (y que el mensaje contenga cierta información):

Para ello, se utilizan las siguiente expectativas:

* `expectException(Excepcion::class)`
* `expectExceptionCode(codigoExcepcion)`
* `expectExceptionMessage(mensaje)`

Del mismo modo que antes, primero se pone la expectativa, y luego se provoca que se lance la excepción:

``` php
<?php
public function testAlquilarCupoLleno() {
    $soporte1 = new CintaVideo("Los cazafantasmas", 23, 3.5, 107); 
    $soporte2 = new Juego("The Last of Us Part II", 26, 49.99, "PS4", 1, 1);
    $soporte3 = new Dvd("Origen", 24, 15, "es,en,fr", "16:9"); 
    $soporte4 = new Dvd("El Imperio Contraataca", 4, 3, "es,en","16:9"); 

    $cliente1 = new Cliente("Bruce Wayne", 23); 
    $cliente1->alquilar($soporte1); 
    $cliente1->alquilar($soporte2); 
    $cliente1->alquilar($soporte3); 

    $this->expectException(CupoSuperadoException::class);
    $cliente1->alquilar($soporte4); 
}
```

### Cobertura de código

La cobertura de pruebas indica la cantidad de código que las pruebas cubren, siendo recomendable que cubran entre el 95 y el 100%.

Una de las métricas asociadas a los informes de cobertura es el CRAP (Análisis y Predicciones sobre el Riesgo en Cambios), el cual mide la cantidad de esfuerzo, dolor y tiempo requerido para mantener una porción de código. Esta métrica debe mantenerse con un valor inferior a 5.

!!! warning "Requisito xdebug"
    Aunque ya viene instalado dentro de PHPUnit, para que funcione la cobertura del código, es necesario que el código PHP se ejecute con XDEBUG, y e indicarle a Apache que así es (colocando en el archivo de configuración `php.ini`la directiva `xdebug.mode=coverage`).

Añadimos en `composer.json` un nuevo *script*:

``` json
"coverage": "phpunit --coverage-html coverage --coverage-filter app tests"
```

Y posteriormente ejecutamos

``` bash
composer coverage
```

Por ejemplo, si accedemos a la clase `CintaVideo` con la prueba que habíamos realizado anteriormente, podemos observar la cobertura que tiene al 100% y que su CRAP es 2.

<figure style="align: center;">
    <img src="imagenes/05/coverage.png">
    <figcaption>Informe de cobertura de la clase CintaVideo</figcaption>
</figure>

!!! warning "Temas pendientes"
    * Dependencia entre casos de prueba con el atributo `@depends`
    * Completamente configurable mediante el archivo `phpxml.xml`: <https://phpunit.readthedocs.io/es/latest/configuration.html>
    * Preparando las pruebas con `setUpBeforeClass()` y `tearDownAfterClass()`
    * Objetos y pruebas *Mock* (dobles) con `createMock()`

## Referencias

* [Tutorial de Composer](https://desarrolloweb.com/manuales/tutorial-composer.html)
* [Web Scraping with PHP – How to Crawl Web Pages Using Open Source Tools](https://www.freecodecamp.org/news/web-scraping-with-php-crawl-web-pages/)
* [PHP Monolog](https://zetcode.com/php/monolog/)
* [Unit Testing con PHPUnit — Parte 1](https://medium.com/@emilianozublena/unit-testing-con-phpunit-parte-1-148c6d73e822), de Emiliano Zublena.
