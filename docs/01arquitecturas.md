# Arquitecturas Web

??? abstract "Duración y criterios de evaluación"

    Duración estimada: 4 sesiones

    <hr />

    Resultado de aprendizaje:

    1. Selecciona las arquitecturas y tecnologías de programación Web en entorno servidor, analizando sus capacidades y características propias.

    Criterios de evaluación:

    1. Se han caracterizado y diferenciado los modelos de ejecución de código en el servidor y en el cliente Web.
    2. Se han reconocido las ventajas que proporciona la generación dinámica de páginas Web y sus diferencias con la inclusión de sentencias de guiones en el interior de las páginas Web.
    3. Se han identificado los mecanismos de ejecución de código en los servidores Web.
    4. Se han reconocido las funcionalidades que aportan los servidores de aplicaciones y su integración con los servidores Web.
    5. Se han identificado y caracterizado los principales lenguajes y tecnologías relacionados con la programación Web en entorno servidor.
    6. Se han verificado los mecanismos de integración de los lenguajes de marcas con los lenguajes de programación en entorno servidor.
    7. Se han reconocido y evaluado las herramientas de programación en entorno servidor.

??? abstract "Objetivos del tema"

    Se trata de que concozcas todos los elementos que se pueden encontrar en una aplicación web y en la arqutectura web de un servicio, además de los tipos de aplicaciones web que existen.

    1. Entender la diferencia entre la infraestructura internet y los servicios web
    2. Conocer las diferentes partes de una arquitectura web
    3. Entender los conceptos previos de las aplicaciones web.
    4. Comprender los tipos de aplicaciones web que se pueden desarrollar.
    5. Conocer las tecnologías que se utilizan en el desarrollo de aplicaciones web y los tipos de lenguajes existentes más utilizados.
  

Las arquitecturas web definen la forma en que las páginas de un sitio web están estructuradas y enlazadas entre sí. Las aplicaciones web se basan en en modelo cliente-servidor.

## Internet y la Web (no es lo mismo)

1. Repaso (se ve en PAR - Planificación y Administración de Redes)
2. Son cosas diferentes:

    1. [Internet](https://es.wikipedia.org/wiki/Internet) (Infraestuctura)
    2. [Web](https://es.wikipedia.org/wiki/Servicio_web) (Servicios)

3. Infraestructura Internet: TCP/IP + Servicios
4. Recordar los estándares (y las organizaciones)
5. El protocolo HTTP

## Modelo Cliente / Servidor

<figure>
  <img src="imagenes/01/clienteservidor.png" />
  <figcaption>Arquitectura Cliente Servidor</figcaption>
</figure>

Uno o varios cliente acceden a un servidor. La nuevas arquitecturas sustituyen el servidor por un balanceador de carga de manera que N servidores dan respuesta a M clientes.

En las aplicaciones web, el cliente es el navegador web.

El cliente hace la petición (*request* normalmente mediante el protocolo GET mediante el puerto 80/443) y el servidor responde (*response*).

### Página web dinámica

Si la página web únicamente contiene HTML + CSS se considera una página estática. Para generar una página dinámica, donde el contenido cambia, a día de hoy tenemos dos alternativas:

* Utilizar un lenguaje de servidor que genere el contenido, ya sea mediante el acceso a una BD o servicios externos.
* Utilizar servicios REST de terceros o propios invocados desde JS.

<figure>
  <img src="imagenes/01/paginadinamica.png" />
  <figcaption>Página web dinámica</figcaption>
</figure>

Las tecnologías empleadadas (y los perfiles de desarrollo asociados) para la generación de páginas dinámicas son:

| Perfil                    | Herramienta           | Tecnología
| ---                       | ---                   | ---
| *Front-end* / cliente     | Navegador Web         | HTML + CSS + JavaScript
| *Back-end* / servidor     | Servidor Web + Servidor Aplicaciones +  BBDD   | PHP, Python, Ruby, Java / JSP, .Net / .asp

!!! tip "Perfil *Full-stack*"
    En las ofertas de trabajo cuando hacen referencia a un *Full-stack developer*, están buscando un perfil que domina tanto el *front-end* como el *back-end*.

### *Single Page Application*

A día de hoy, gran parte del desarrollo web está transicionando de una arquitectura web cliente-servidor clásica donde el cliente realiza una llamada al backend, por una arquitectura SPA donde el cliente gana mucho mayor peso y sigue una programación reactiva que accede a servicios remotos REST que realizan las operaciones (comunicandose mediante JSON).

<figure>
  <img src="imagenes/01/01spa.png" />
  <figcaption>Arquitectura tradicional vs SPA</figcaption>
</figure>

??? info "SPA En detalle"
    Si quires profundizar en este tema de las aplicaciones SPA, puedes consultar los siguientes recursos:

    - [SPA Wikipedia](https://es.wikipedia.org/wiki/Single-page_application)
    - [¿Qué es una SPA en programación?](https://keepcoding.io/blog/que-es-una-spa-en-programacion/)


## Arquitectura de 3 capas

Hay que distinguir entre capas **físicas** (*tier*) y capas **lógicas** (*layer*).

### Tier

Capa física de una arquitectura. Supone un nuevo elemento hardware separado físicamente. Las capas físicas más alejadas del cliente están más protegidas, tanto por firewalls como por VPN.

Ejemplo de arquitectura en tres capas físicas (*3 tier*):

* Servidor Web
* Servidor de Aplicaciones
* Servidor de base de datos

<figure>
  <img src="imagenes/01/tier3.png" />
  <figcaption>Arquitectura de tres capas físicas</figcaption>
</figure>

!!! warning "Cluster en tiers"
    No confundir las capas con la cantidad de servidores. Actualmente se trabaja con arquitecturas con múltiples servidores en una misma capa física mediante un cluster, para ofrecer [tolerancia a errores](https://es.wikipedia.org/wiki/Dise%C3%B1o_de_tolerancia_a_fallos) y [escalabilidad horizontal](https://www.arsys.es/blog/escalado-horizontal-vs-vertical).

### Layer

![Arquitectura de tres capas físicas](imagenes/01/layer3.png){ align=right }

En cambio, las capas lógicas (*layers*) organizan el código respecto a su funcionalidad:

* Presentación
* Negocio / Aplicación / Proceso
* Datos / Persistencia

Como se observa, cada una de las capas se puede implementar con diferentes lenguajes de programación y/o herramientas.

<figure>
  <img src="imagenes/01/tierlayer.png" />
  <figcaption>Arquitectura de tres capas físicas en tres lógicas</figcaption>
</figure>

## MVC

![MVC](imagenes/01/mvc.png){align=right & width=500}

*Model-View-Controller* o Modelo-Vista-Controlador es un modelo de arquitectura que separa los datos y la lógica de negocio respecto a la interfaz de usuario y el componente encargado de gestionar los eventos y las comunicaciones.

Al separar los componentes en elementos conceptuales permite reutilizar el código y mejorar su organización y mantenimiento. Sus elementos son:

* Modelo: representa la información y gestiona todos los accesos a ésta, tanto consultas como actualizaciones provenientes, normalmente, de una base de datos. Se accede via el controlador.
* Controlador: Responde a las acciones del usuario, y realiza peticiones al modelo para solicitar información. Tras recibir la respuesta del modelo, le envía los datos a la vista.
* Vista: Presenta al usuario de forma visual el modelo y los datos preparados por el controlador. El usuario interactura con la vista y realiza nuevas peticiones al controlador.

Lo estudiaremos en más detalle al profundizar en el uso de los frameworks PHP.

## Decisiones de diseño

* ¿Qué tamaño tiene el proyecto?
* ¿Qué lenguajes de programación conozco? ¿Vale la pena el esfuerzo de aprender uno nuevo?
* ¿Voy a usar herramientas de código abierto o herramientas propietarias? ¿Cuál es el coste de utilizar soluciones comerciales?
* ¿Voy a programar la aplicación yo solo o formaré parte de un grupo de programadores?
* ¿Cuento con algún servidor web o gestor de base de datos disponible o puedo decidir libremente utilizar el que crea necesario?
* ¿Qué tipo de licencia voy a aplicar a la aplicación que desarrolle?

## Herramientas

### Servidor Web

Software que recibe peticiones HTTP (GET, POST, DELETE, ...). Devuelve el recurso solicitado (HTML, CSS, JS, JSON, imágenes, etc...)

Un servidor web es una tecnología que aloja el código y los datos de un sitio web. Al ingresar una URL en el navegador, la URL es en realidad el identificador de dirección del servidor web. El servidor web recibe la solicitud y devuelve el contenido de la página web al navegador del usuario.

Su navegador y servidor web se comunican de la siguiente manera:

1. El navegador usa la URL para encontrar la dirección IP del servidor.
2. El navegador envía una solicitud HTTP de información.
3. El servidor web se comunica con un servidor de base de datos para encontrar los datos relevantes.
4. El servidor web devuelve contenido estático, como páginas HTML, imágenes, videos o archivos, en una respuesta HTTP al navegador.
5. A continuación, el navegador le mostrará la información.

Un sitio web que aloja contenido estático, como blogs, imágenes de encabezado o artículos, puede ejecutarse en un servidor web. Sin embargo, la mayoría de los sitios web y aplicaciones web son mucho más interactivos y requieren un servidor de aplicaciones.

??? example "En profundidad: Servidores Web"

    === "Servidores Web mas populares"

		Algunos de los servidores web más utilizados son los siguientes:

		- <ins>Servidor HTTP Apache
			
			Es un servidor web gratuito y de código abierto que se utiliza en muchos sistemas operativos, como Windows, Linux y Mac OS X. [Apache](https://www.hostinger.es/tutoriales/que-es-apache/) es el software de servidor web más antiguo y uno de los más utilizados por los propietarios de páginas web, desarrolladores y proveedores de hosting, con una [cuota de mercado](https://w3techs.com/technologies/details/ws-apache) de más del 31%.
			
		- <ins>NGINX</ins>
			
			Es un famoso software de servidor web de código abierto que inicialmente sólo funcionaba para el servicio web HTTP. Ahora también se utiliza como proxy inverso, balanceador de carga HTTP y proxy de correo electrónico.
			
			NGINX es conocido por su velocidad y su capacidad para manejar múltiples conexiones, por lo que muchos sitios web de alto tráfico utilizan sus servicios.
			
		- <ins>IIS: (Internet Information Services) de Microsoft
			
			[IIS](https://www.iis.net/) es un software de servidor web cerrado desarrollado por Microsoft y ampliamente utilizado en los sistemas operativos Windows.
			
		- <ins>Lighttpd
			
			Es un software de servidor web gratuito y de código abierto que es conocido por su velocidad y por requerir menos potencia de la CPU. [Lighttpd](https://www.lighttpd.net/) también es popular por tener una pequeña huella de memoria.
			
			En el ámbito del hosting, los distintos hosts soportan diferentes tipos de servidores web. Por ejemplo, [Hostinger soporta](https://www.hostinger.es/tecnologia) tanto Apache como NGINX, los dos servidores web líderes en el mercado.


    === "Apache vs Nginx"

        ¿ Cuál es la diferencia entre Apache y Nginx ?

        **Similaridades**

          - Ambos son servidores web de código abierto.
          - Amplia comunidad de usuarios y desarrolladores.
          - Permiten agregar funcionalidad a través de módulos.
          - Ambos permiten actuar como servidor proxy, permite pasar información a otras aplicaciones y devolver esta información al usuario (cliente).
          - Procesamiento basado en eventos para conexiones simulateneas (últimas versiones de apache).


        **Diferencias**

        Puntos | Apache | Nginx
        ---|---|---
        Configuración | Sintaxis XML | Sintaxis estilo C
        Configuración | Fichero .htaccess configuración distribuida en muchas carpetas | Bloques de configuración en un único fichero
        Contenido dinámico | Nativamente procesado con módulos, eliminando la necesidad de otras aplicaciones (PHP, Perl, Python, ...) | Requiere de procesamiento externo a través de otras aplicaciones
        Contenido estático | Menos eficiente | Más eficiente (más del doble de rápido)
        Cache y balanceo de carga | A través de módulos (conf. compleja) | Capacidades nativas (sencilla)

!!! info "Comparativa de servidores web"

    <https://w3techs.com/technologies/history_overview/web_server/ms/q>

### Servidor de Aplicaciones

Un servidor de aplicaciones amplía las capacidades de un servidor web, pues admite la generación de contenido dinámico, la lógica de la aplicación y la integración con varios recursos. Proporciona un entorno de tiempo de ejecución en el que puede ejecutar el código de la aplicación e interactuar con otros componentes de software, como los sistemas de mensajería y las bases de datos. Utiliza la lógica empresarial para transformar los datos de manera más significativa que un servidor web.

* Ofrece servicios adicionales a los de un servidor web:
    * Clustering
    * Balanceo de carga
    * Tolerancia a fallos
    * ...

* *Tomcat* (<http://tomcat.apache.org/>) es el servidor de aplicaciones *open source* y multiplataforma de referencia para una arquitectura Java.
    * Contiende un contenedor Web Java que interpreta *Servlets* y JSP.


??? example "En profundidad: Servidores de Aplicaciones"

    === "Diferencias clave"

		**Servidor Web**

		- Un servidor web es un software que se ejecuta en un servidor y que responde a las solicitudes de los clientes a través del protocolo HTTP.
		- Un servidor web sirve contenido estático, como páginas HTML, imágenes y archivos.
		- Los servidores web más populares son Apache y Nginx.
		- Los servidores web son ideales para sitios web pequeños y estáticos.

		**Servidor de Aplicaciones**

		- Un servidor de aplicaciones es un software que se ejecuta en un servidor y que responde a las solicitudes de los clientes a través de protocolos más complejos.
		- Un servidor de aplicaciones sirve contenido dinámico, como aplicaciones web y servicios web.
		- Los servidores de aplicaciones son ideales para aplicaciones web complejas y dinámicas.
		- Los servidores de aplicaciones más populares son Tomcat, JBoss y WebSphere.

	=== "Resumen"
	
		Conceptos | Servidor web | Servidor de aplicaciones
		---|---|---
		Tareas realizadas | Los servidores web ofrecen respuestas a solicitudes sencillas. | Un servidor de aplicaciones ofrece contenido más complejo de bases de datos, servicios y sistemas empresariales.
		Protocolos utilizados | Los servidores web utilizan principalmente HTTP. También admiten FTP y SMTP. | Los servidores de aplicaciones admiten muchos protocolos. 
		Tipos de contenidos | Los servidores web ofrecen contenido estático, como páginas HTML, imágenes, videos y archivos. | Los servidores de aplicaciones ofrecen contenido dinámico, como actualizaciones en tiempo real, información personalizada y atención al cliente.
		Subprocesamiento múltiple | No suele utilizar subprocesos múltiples. | Utiliza subprocesos múltiples para procesar solicitudes de forma simultánea. 
	
	=== "Funcionamiento conjunto"

		Los servidores de aplicaciones y los servidores web trabajan juntos para administrar las solicitudes de los clientes y ofrecer el contenido correcto al usuario. El servidor web siempre recibe primero una nueva solicitud. Si puede producir la información por sí mismo, lo hace y envía una respuesta HTTP. También comprueba que los datos que el usuario solicitó no estén ya en su caché.

		Si el servidor web no puede acceder al contenido que el usuario necesita, reenvía la solicitud al servidor de aplicaciones. El servidor de aplicaciones procesa los datos y utiliza la lógica empresarial para proporcionar la información correcta. A continuación, devuelve la solicitud al servidor web, que la pasa al usuario. En ciertas arquitecturas, también puede configurar los servidores de aplicaciones para que gestionen las solicitudes HTTP por sí mismos.	

		![Digrama de flujo](imagenes/01/diagramflow-webserver-webapp.png)


> 💡Tanto los servidores web como los servidores de aplicaciones se estudian en profundidad en el módulo de *"Despliegue de Aplicaciones Web"*.

### Lenguajes en el servidor

Las aplicaciones que generan las páginas web se programan en alguno de los siguientes lenguajes:

* PHP
* JavaEE: Servlets / JSP
* Python
* ASP.NET → Visual Basic .NET / C#
* Ruby
* ...

#### JavaEE

*Java Enterprise Edition* es la solución Java para el desarrollo de aplicaciones *enterprise*. Ofrece una arquitectura muy completa y compleja, escalable y tolerante a fallos. Planteada para aplicaciones para grandes sistemas.

![JavaEE](imagenes/01/javaee.png)


#### .NET

![Diagrama de .NET](imagenes/01/ApsNetCoreDiagram.png){ width="500", align=right }

* Plataforma de desarrollo de Microsoft
* Utiliza el lenguaje C# o Visual Basic .NET
* Utiliza el servidor de aplicaciones IIS
* Se basa en el framework .NET (.Net Core en la actualidad)
* Permite el desarrollo de aplicaciones web, de escritorio y móviles
* Para la web, se utiliza ASP.NET, que es un framework para el desarrollo de aplicaciones web
  * ASP.NET MVC (Modelo Vista Controlador)
  * ASP.NET Web API (servicios web)
  * ASP.NET SignalR (comunicación en tiempo real)
  * ASP.NET Blazor (Las aplicaciones web se desarrollan en C# y se ejecutan en el navegador)


#### Ruby

* Lenguaje de programación interpretado
* Desarrollado por Yukihiro Matsumoto en 1995
* Inspirado en Perl, Smalltalk, Eiffel, Ada y Lisp
* Framework Ruby on Rails
* Utilizado por GitHub, Twitter, Airbnb, Shopify, ...


#### Node.js

* Entorno de ejecución de JavaScript en el servidor
* Basado en el motor V8 de Google
* Desarrollado por Ryan Dahl en 2009
* Utilizado por Netflix, Uber, LinkedIn, PayPal, ...
* Frameworks: Express, Koa, Hapi, Sails, Meteor, NestJS, ...
* Permite el desarrollo de aplicaciones web, de escritorio y móviles

#### PHP

* Lenguaje de propósito general diseñado para el desarrollo de páginas web dinámicas
* En un principio, lenguaje no tipado.
* Actualmente en la versión 8. Se recomienda al menos utilizar una versión superior a la 7.0.
* Código embebido en el HTML
* Instrucciones entre etiquetas `<?php` y `?>`
    * Para generar codigo dentro de PHP, podemos usar la instrucción `echo`
* Multitud de librerías y frameworks:
    * Laravel, Symfony, Codeigniter, Zend

Su documentación es bastante completa: <https://www.php.net/manual/es/index.php>

El siguiente mapa mental muestra un resumen de sus elementos:

<figure>
  <img src="imagenes/01/php.jpg" />
  <figcaption>Elementos del lenguaje PHP</figcaption>
</figure>

Durante las siguientes unidades vamos a estudiar PHP en profundidad.


## Puesta en marcha

Para poder trabajar con un entorno de desarrollo local, hemos de preparar nuestro entorno de desarrollo con las herramientas comentadas. A lo largo del curso vamos a utilizar la versión 8 de PHP.

### XAMPP

XAMPP (https://www.apachefriends.org/es/index.html) es una distribución compuesta con el software necesario para desarrollar en entorno servidor. Se compone de las siguientes herramientas en base a sus siglas:

* X para el sistema operativo (de ahí que se conozca tamnbién como LAMP o WAMP).
* A para Apache.
* M para MySQL / MariaDB. También incluye phpMyAdmin para la administración de la base de datos desde un interfaz web.
* P para PHP.
* la última P para Perl.

Desde la propia página se puede descargar el archivo ejecutable para el sistema operativo de nuestro ordenador. Se recomienda leer la FAQ de cada sistema operativo con instrucciones para su puesta en marcha.

!!! note "XAMPP en Windows"
    Si vas a trabajar con tu propio ordenador, XAMPP es una solución más sencilla que Docker, sobre todo si trabajar con Windows como sistema operativo.

### Docker

Docker (<https://www.docker.com>) es un gestor de contenedores, considerando un contenedor como un método de virtualización del sistema operativo.

El uso de contenedores requiere menos recursos que una máquina virtual, por lo tanto, su lanzamiento y detención son más rápidos que las máquinas virtuales.

Así pues, *Docker* permite crear, probar e implementar aplicaciones rápidamente, a partir de una serie de plantillas que se conocen como imágenes de *Docker*.

Para ello es necesario tener instalado *Docker Desktop* (<https://www.docker.com/products/docker-desktop>) en nuestros entornos de desarrollo (el cual ya incluye en nucleo de *Docker* y la herramienta *docker compose*). En los ordenadores del aula ya está instalado. Para instalarlo en casa, en el caso de Windows, es necesario instalar previamente *WSL 2*, el cual es un subsistema de *Linux* dentro de *Windows*.

A lo largo del curso iremos creando diferentes contenedores con los servicios necesarios, de manera que cada vez sólo trabajemos con el software mínimo.

!!! caution "Versiones"
    A lo largo del curso vamos a usar PHP `8.0`. Respecto a *Docker*, para escribir los apuntes hemos utilizado la version `20.10` y la version `2.19` de *docker compose*. Finalmente, la versión de *Docker Desktop* que hemos utilizado es la `4.0`.

#### Plantilla Servidor Web + PHP

*Docker* se basa en el uso de imágenes para crear contenedores. *Docker Compose* simplifica el trabajo con múltiples contenedores, y por ello, para facilitar el arraque, nos centraremos en *Docker Compose* utilizando una plantilla que únicamente va a contener como servicios Apache/Nginx y PHP.

Para ello, vamos a rellenar el archivo `docker-compose.yaml` con:

=== "Apache y PHP"

    ``` yaml
    # Services
    services:
      # Apache + PHP
      apache_php:
        image: php:8-apache
        # Preparamos un volumen para almacenar nuestro código
        volumes:
          - ./src/:/var/www/html
        expose:
          - 80
        ports:
          - 80:80
    ```

=== "Nginx y PHP"

    ``` yaml
    # Services
    services:

      nginx:
        image: nginx:1.19
        ports:
          - 80:80
        volumes:
          - ./src:/var/www/php
          - ./.docker/nginx/conf.d:/etc/nginx/conf.d # cargamos la configuración de un fichero externo
        depends_on:
          - php   # enlazamos nginx con php

      php:
        image: php:8.0-fpm
        working_dir: /var/www/php
        volumes:
          - ./src:/var/www/php
    ```

Dentro de la carpeta que contenga dicho archivo, hemos de crear una carpeta `src` donde colocaremos nuestro código fuente. Para facilitar la puesta en marcha, tenéis la plantilla de [Apache/PHP](recursos/plantilla-AP.zip) ([versión 2](recursos/plantilla-AP2.zip) con `a2enmod rewrite`) o [Nginx/PHP](recursos/plantilla-NP.zip) disponible para su descarga.

Cuando estemos listos, lanzaremos el servicio mediante:

``` console
docker-compose up -d
```

Si queremos ver el contenido de los archivos de log del servicio utilizaremos:

``` console
docker-compose logs -f
```

Para copiar un archivo desde nuestro sistema al interior del contenedor:

``` console
docker cp ./miFichero idContenedor:/tmp
```

Y al revés, si queremos consultar un archivo contenido dentro de un contenedor, lo copiaremos a nuestro sistema:

``` console
docker cp idContenedor:/tmp/archivoAConsultar.txt ./
```

Finalmente, si queremos acceder a un terminal interactivo dentro del contenedor:

``` console
docker exec -it nombreContenedor bash
```

Otros comandos que podemos llegar a utilizar son:

* `docker ps`: Ver los contenedores que se estan ejecutando
* `docker ps -a`: Ver todos los contenedores
* `docker start nombreContenedor`: Arrancar un contenedor
* `docker images`: Ver las imágenes que tenemos descargadas

Otra forma más sencilla para lanzar de nuevo los contenedores y gestionarlos una vez creados es utilizar el interfaz gráfico que ofrece *Docker Desktop*:

<figure>
  <img src="imagenes/01/dockerdesktop.png" />
  <figcaption>Arranque de contenedor mediante Docker Desktop</figcaption>
</figure>

!!! tip "Docker stack"
    Existen diversas opciones mediante Docker que ofrecen soluciones completas y empaquetas para todo el stack de desarrollo. En posteriores sesiones utilizaremos tanto *Devilbox* (<http://devilbox.org>) como *Laradock* (<https://laradock.io>)

!!! question "Pero quiero saber cómo funciona..."
    En el módulo de *Despliegue de aplicaciones* estudiaréis en profundidad, además de Docker, Apache y otros servicios que nos servirán de ayuda para el desarrollo en entorno servidor.

### Entorno de desarrollo

En este curso vamos a emplear *Visual Studio Code* (<https://code.visualstudio.com>) como entorno de desarrollo (IDE), y más adelante también [PhpStorm](https://www.jetbrains.com/es-es/phpstorm/), que es sin duda la más usada y conocinda aunque es de pago (existe opción para estudiantes).

*VSCode* es un editor de código fuente que se complementa mediante extensiones. Para facilitar el trabajo a lo largo del curso vamos a utilizar las siguientes extensiones:

* [PHP Intelephense](https://marketplace.visualstudio.com/items?itemName=bmewburn.vscode-intelephense-client)
* [Docker](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)

En la siguiente sesión comenzaremos a utilizar *Intelephense* pero en esta sesión nos vamos a centrar en *Docker* (más adelante instalaremos nuevas extensiones).

Por ejemplo, si abrimos la extensión de *Docker*, podréis visualizar tanto los contenedores como las imágenes de vuestro sistema. Desde cada contenedor, mediante clic derecho, podemos iniciar/detener/reiniciar cada contenedor, así como ver su contenido o abrir un terminal dentro del mismo.

<figure>
  <img src="imagenes/01/vscodedocker.png" width="300"/>
  <figcaption>Opciones mediante extensión Docker en VSCode</figcaption>
</figure>

### Hola Mundo

Y como no, nuestro primer ejemplo será un *Hola Mundo* en PHP.

Si nombramos el archivo como `index.php`, al acceder a `http://localhost` automáticamente cargará el resultado:

``` html+php hl_lines="9-11"
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hola Mundo</title>
</head>
<body>
    <?php
        echo "Hola Mundo";
    ?>
</body>
</html>
```

## Referencias

* Curso de introducción a Docker, por *Sergi García Barea* : <https://sergarb1.github.io/CursoIntroduccionADocker/>
* Artículo [Arquitecturas Web y su evolución](https://www.arquitecturajava.com/arquitecturas-web-y-su-evolucion/)



## Actividades

??? question "Cuestionario en grupo"

	40 preguntas sobre Internet [Kahoot](https://create.kahoot.it/share/40-preguntas-sobre-internet/ffb5a58c-4e58-4656-826f-0f8d94304331)
	

101. Trabajo sobre servidores web
     
	 1. Busca en internet la historia de Apache y redacta un resumen de la misma (con tus palabras), varios parráfos.
	 2. Busca en internet la historia de Nginx..

102. Busca en internet cuales son los tres frameworks PHP más utilizados, y indica:

    * Nombre y URL
    * Año de creación
    * Última versión

103. Busca tres ofertas de trabajo de *desarrollo de software* en Infojobs o Manfred que citen PHP y anota:

    * Empresa + puesto + frameworks PHP + requísitos + sueldo + enlace a la oferta.

104. Tecnologías Web
	
	1. ¿ Donde puede ver las especificaciones de HTML ?
	2. ¿ Y de CSS ?
	3. ¿ Y de Javascript ?
	4. ¿ Cúal es la última versión de cada uno de ellos ?
	5. ¿ Sabrías distinguir un servidor web de un servidor de aplicaciones ?
	6. El servidor de aplicaciones, el web y el de base de datos, ¿tienen que estar en la mimsa máquina ? ¿en la misma red?
