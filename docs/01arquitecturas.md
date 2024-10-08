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


??? sucess "Aprender conceptos más que código"

     Esta la la clave principal para ser un buen programador. Aprender a programar es fácil, pero entender los conceptos y saber aplicarlos es lo que te hará destacar.

    <iframe width="1000" height="700" src="https://www.youtube.com/embed/ImOR0o-QHOQ" title="Aprende conceptos antes que código" frameborder="0" clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen>
    </iframe>


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

    - [Aplicaciones SPA vs Aplicaciones MPA](https://youtu.be/2z0FChkphvo){ target="_blank" }
    - [SPA Wikipedia](https://es.wikipedia.org/wiki/Single-page_application)
    - [¿Qué es una SPA en programación?](https://keepcoding.io/blog/que-es-una-spa-en-programacion/)


### Client Side Rendering vs Server Side Rendering

En el desarrollo de aplicaciones web, se pueden distinguir dos formas de renderizar el contenido de una página web:

* **Client Side Rendering (CSR)**: El contenido de la página se renderiza en el cliente, es decir, en el navegador del usuario. La página se carga en blanco y, a medida que se van recibiendo los datos, se va mostrando el contenido. Es la técnica más utilizada en aplicaciones web modernas, ya que permite una mayor interactividad y una mejor experiencia de usuario.
* **Server Side Rendering (SSR)**: El contenido de la página se renderiza en el servidor y se envía al cliente ya renderizado. La página se carga con todo el contenido visible, lo que permite una carga más rápida y un mejor SEO. Es la técnica más utilizada en aplicaciones web tradicionales.
* **Hybrid Rendering**: Es una combinación de las dos técnicas anteriores. La página se renderiza en el servidor y en el cliente, lo que permite una carga rápida y una mayor interactividad.
  

#### Comparativa

| Característica | Client Side Rendering | Server Side Rendering |
| --- | --- | --- |
| **Carga inicial** | Más lenta | Más rápida |
| **Interactividad** | Mayor | Menor |
| **SEO** | Menor | Mayor |
| **Tiempo de carga** | Mayor | Menor |
| **Experiencia de usuario** | Mejor | Peor |

### Tipos de aplicaciones web

| Tipo de aplicación | Descripción |
| --- | --- |
| **Estáticas** | Son páginas web que no cambian su contenido en función de la interacción del usuario. Son páginas web que no requieren de una base de datos para funcionar. |
| **Dinámicas** | Son páginas web que cambian su contenido en función de la interacción del usuario. Son páginas web que requieren de una base de datos para funcionar. |
| **Interactivas** | Son páginas web que permiten al usuario interactuar con el contenido de la página. Son páginas web que requieren de una base de datos para funcionar. |
| **Transaccionales** | Son páginas web que permiten al usuario realizar transacciones, como compras o reservas. Son páginas web que requieren de una base de datos para funcionar. |
| **Colaborativas** | Son páginas web que permiten a varios usuarios colaborar en la creación de contenido. Son páginas web que requieren de una base de datos para funcionar. |
| **Adaptativas** | Son páginas web que se adaptan a las características del dispositivo del usuario. |


??? info "Client Side Rendering vs Server Side Rendering"

    Para profundizar en este tema, puedes consultar los siguientes recursos:

    - [Client Side Rendering vs Server Side Rendering](https://youtu.be/CnavwJZAMw0)
    - [CSR vs SSR presentación](https://www.figma.com/board/oBOsrzlbRjv6bSnc2gIyOA/Next.js-Diagrams-(Community)?node-id=0-1&node-type=canvas&t=5dUlZZqs6K2YIPrg-0)


## Arquitectura de 3 capas

Hay que distinguir entre capas **físicas** (*tier*) y capas **lógicas** (*layer*).

??? info "Vídeo: N-Layers Vs N-Tiers"

    En este vídeo se explica la diferencia entre capas físicas y lógicas en una arquitectura de software.
    
    <iframe width="800" height="500" src="https://www.youtube.com/embed/r3rRwKdDMz0" title="Definición de conceptos: N-Layers Vs N-Tiers" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

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



!!! example "Actividades"

    100. 40 preguntas sobre Internet [Kahoot](https://create.kahoot.it/share/40-preguntas-sobre-internet/ffb5a58c-4e58-4656-826f-0f8d94304331)

    101. Trabajo sobre servidores web
        
           1.   Busca en internet la historia de Apache y redacta un resumen de la misma (con tus palabras), varios parráfos.
           2.   Busca en internet la historia de Nginx..

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


## **Puesta en marcha**

Para poder trabajar con un entorno de desarrollo local, hemos de preparar nuestro entorno de desarrollo con todo lo necesario, y esto no es trivial al principio.

Este es un punto muy importante, ya vamos a necesitar una serie de recursos, tanto IDEs (y sus plugins) como el propio PHP, BDs, servidores web y aplicaciones, etc. para poder trabajar en el curso. Otro punto clave es el sistema operativo, ya que no es lo mismo trabajar en Windows que en Linux o en MacOS. Los entornos más comunes son Windows y Linux (MacOS), para los que existe más herramientas y recursos.

>💡[¿Por qué necesito un entorno de desarrollo local?](hhttps://youtu.be/tvbu-cezBI8)<br>
> Este video explica la importancia de tener un entorno de desarrollo local y cúal elegir para PHP.

!!! tip "Entorno de desarrollo con Docker"
    Docker es una herramienta muy importante en el desarrollo de software (y fundamental en Web), ya que nos permite crear contenedores con los servicios necesarios para trabajar, testear diferentes entornos sin afectar al entorno local, pero por su complejidad vamos a empezar con un entorno de desarrollo más sencillo, más automatizado y donde nos centremos en el código y no en la infraestructura.

    Existen opciones ya implementadas para trabajar con Docker, como [Laradock](https://laradock.io/), [DevilBox](devilbox.org) que utilizaremos más adelante en el curso.


### XAMPP

[XAMPP](https://www.apachefriends.org/es/index.html) es una distribución compuesta con el software necesario para desarrollar en entorno servidor. Se compone de las siguientes herramientas en base a sus siglas:

* X para el sistema operativo (de ahí que se conozca tamnbién como LAMP o WAMP).
* A para Apache.
* M para MySQL / MariaDB. También incluye phpMyAdmin para la administración de la base de datos desde un interfaz web.
* P para PHP.
* la última P para Perl.

Desde la propia página se puede descargar el archivo ejecutable para el sistema operativo de nuestro ordenador. Se recomienda leer la FAQ de cada sistema operativo con instrucciones para su puesta en marcha.

🔥 **XAMP tiene ya unos años, y aunque es una solución, no es la más sencilla ni más flexible, además suele ser bastante opaca en cuanto a la configuración de los servicios, y si da problemas, no es fácil de solucionar.**


### Laragon

Laragon (https://laragon.org/) es una herramienta similar a XAMPP (solo para Windows) pero más moderna y con más opciones. Como dice su eslogan "Productiva, Portable, Rápida, Efectiva y Sorprendente!!".

Otro punto importante es que Laragon sigue siendo mantenida, mientras que XAMPP no se actualiza desde hace tiempo.

Pasos:

1. [Instalar Laragon](https://laragon.org/docs/install)
2. [Montar el entorno de desarrollo con Laragon](https://youtu.be/tvbu-cezBI8?t=198)
3. Comprobar que todo funciona correctamente.


### Entorno de desarrollo

En este curso vamos a emplear *Visual Studio Code* (<https://code.visualstudio.com>) como entorno de desarrollo (IDE), y más adelante también [PhpStorm](https://www.jetbrains.com/es-es/phpstorm/), que es sin duda la más usada y conocinda aunque es de pago (existe opción para estudiantes).

*VSCode* es un editor de código fuente que se complementa mediante extensiones. Para facilitar el trabajo a lo largo del curso vamos a utilizar las siguientes extensiones:

* [PHP Intelephense](https://marketplace.visualstudio.com/items?itemName=bmewburn.vscode-intelephense-client)

> 💡 Para PHP Intelephense es requerido desactivar la extensión propia que VSCode tiene para PHP. 
> 1. Ir a extensiones, buscar por @builtin php, Desactvivar PHP Languaje Features, dejar PHP Languaje Basis para resaltar la sintaxis.

En este [tutorial](https://www.digitalocean.com/community/tutorials/how-to-set-up-visual-studio-code-for-php-projects) (inglés) se explica cómo configurar VSCode para trabajar con PHP.

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

