# Ampliación: Arquitecturas Web 

## 1. Fundamentos de la Web

Cuando se habla de la web todo parece muy sencillo, pero detrás hay muchos estándares y protocolos que se ponen en marcha para poder comunicar aplicaciones entre sí. Aplicaciones que se ejecutarán generalmente en equipos diferentes. Una aplicación que se ejecutará en un sistema remoto que llamaremos servidor web y otra aplicación que se ejecutará en el sistema del cliente llamado navegador web.

El conjunto de tecnologías subyacentes en la web, basada en su mayoría en estándares abiertos, se ha convertido en el pilar fundamental de Internet. Hoy en día, el uso de estas tecnologías no se limita a la comunicación entre un navegador web y un servidor web, sino que se utiliza incluso para facilitar el intercambio de datos entre aplicaciones a través de lo que conocemos como servicio web.

El mejor ejemplo de intercambio de datos entre aplicaciones a través de tecnologías web es [REST](https://educacionadistancia.juntadeandalucia.es/formacionprofesional/pluginfile.php/56055/mod_scorm/content/0/#tce4a2d8b-c7be-4729-1b4e-f97ceec3494a), donde usando protocolos y estándares ya conocidos como HTTP y XML, se permite la comunicación entre aplicaciones. Con tecnologías como esta podríamos crear una aplicación web (e incluso una aplicación móvil) que se comunicara con otra aplicación web, como por ejemplo Twitter o Facebook.

### 1.1.- ¿Qué aspectos generales de arquitecturas web debes conocer?

La arquitectura World Wide Web (WWW) de Internet provee un modelo de programación sumamente poderoso y flexible. Las aplicaciones y los contenidos son presentados en formatos de datos estándar y son localizados por aplicaciones conocidas como "web browsers", que envían requerimientos de objetos a un servidor y éste responde con el dato codificado según un formato estándar.

Los estándares WWW especifican muchos de los mecanismos necesarios para construir un ambiente de aplicación de propósito general, por ejemplo:

- **Modelo estándar de nombres:** todos los servidores, así como el contenido de la  se denominan según un Localizador Uniforme de Recursos (Uniform Resource Locator: ).
- **Contenido:** a todos los contenidos en la  se les especifica un determinado tipo, permitiendo de esta forma que los browsers (navegadores) los interpreten correctamente.
- **Formatos de contenidos estándar:** todos los navegadores soportan un conjunto de formatos y tecnologías estándar, por ejemplo (Lenguaje de marcas de hipertexto), ECMAScript (JavaScript), HTML, CSS, etc
- **Protocolos estándar:** éstos permiten que cualquier navegador pueda comunicarse con cualquier servidor web. El más comúnmente usado en  es  (Protocolo de Transferencia de HiperTexto), que opera sobre el conjunto de protocolos  (Protocolo de Control de Transmisión / Protocolo de Internet). WWW, HTTP, TCP/IP
    
    Esta infraestructura permite a los usuarios acceder a una gran cantidad de aplicaciones y servicios de terceros. También permite a los desarrolladores crear aplicaciones y servicios para una gran comunidad de clientes.

### 1.2 El servicio web e Internet

Internet, tal y como la conocemos hoy día, utiliza para su funcionamiento la arquitectura de protocolos TCP/IP. Lo importante de TCP/IP es que permite la comunicación de equipos que están en diferentes redes a través de una arquitectura flexible.

Como supondrás, cuando accedemos a un sitio web a través de un navegador web, el navegador web inicia una o más comunicaciones con un sistema remoto y se produce un intercambio de paquetes que permite mostrar la página web. Veamos el proceso más detalladamente:

- El navegador realiza una petición de un recurso al servidor web (imagen, HTML, etc).
- El servidor web recibe la petición y la procesa. Procesar la petición en el servidor web puede significar dos cosas diferentes:
    - Servir un recurso estático, es decir, el contenido de un archivo almacenado en el disco del servidor.
    - Ejecutar una aplicación en el servidor que genere contenido dinámico.
- El navegador web recibe el contenido del servidor y es el encargado de mostrarlo al usuario final, a través de un proceso conocido como renderizado. En el proceso de renderizado el navegador web convierte los datos recibidos en información que el usuario puede ver en la pantalla de su ordenador.
- Si para renderizar y representar la página web el navegador necesita más recursos (imágenes, archivos, CSS, etc.), los volvería a pedir al navegador, siguiendo el proceso anterior.

El protocolo que permite todo ese proceso anterior es HTTP. HTTP es el protocolo que permite al navegador realizar una petición y al servidor generar una respuesta, y es importante entenderlo antes de configurar un servidor web.
![Capas Modelo Internet](../imagenes/extra/01/CapasModeloInternet.png){ width="400", align=right }



Pero si ha triunfado la web y ha llegado hasta donde está hoy día, no es solo por el protocolo HTTP, sino también por otras cosas:

- El sistema DNS, que permite traducir un nombre de máquina en una dirección IP. El sistema DNS nos permite acceder a una máquina remota a través de un nombre (como www.gnu.org o **`www.debian.org`**).
- El uso de URLs (Uniform Resource Locator). Una URL es una nomenclatura pensada para poder nombrar recursos en la red de forma sencilla, por ejemplo: http://www.gnu.org.
- Por el ingenio de muchos desarrolladores y desarrolladoras web que con HTML, CSS y JavaScript, y tecnologías del lado del servidor, consiguen hacer páginas webs increíbles.

De todos estos conceptos, el que vamos a revisar ahora es el de URL. Para entender una URL lo mejor es analizar un ejemplo de URL y desglosarla en partes: [Wikipedia](http://es.wikipedia.org/wiki/Localizador_de_recursos_uniforme)

En una URL aparecen las siguientes partes:

- **Esquema**. El esquema es donde pone http: y representa el protocolo usado para acceder al recurso. Los esquemas más usados son http:, **`https:`** (protocolo HTTPS ) y ftp: (protocolo FTP ), aunque hay otros esquemas como tel: (para hacer referencia a un número de teléfono), mailto: (para hacer referencia a un correo electrónico), y bastantes más.
- **Nombre de máquina o FQDN**. En este caso es es.wikipedia.org y hace referencia a la máquina a la que estamos accediendo.
- **Ruta hasta el recurso**. En este caso sería /wiki/Localizador_de_recursos_uniforme, y hace referencia al camino a seguir para llegar al recuso.

Si alguna URL necesita **usuario y contraseña** (por ejemplo, si usamos el esquema ftp), entonces podemos escribirlo de la siguiente forma:

- ftp://anonymous@ftp.rediris.es/mirror/Apache/: donde anonymous sería el nombre de usuario.
- ftp://anonymous:contraseña@ftp.rediris.es/mirror/Apache/: donde anonymous:contraseña son la combinación del usuario y su contraseña separados por dos puntos.

Y por último, si un protocolo se usa en un puerto diferente al puerto por defecto o estándar, podemos usar el siguiente formato de URL:

- http://www.ejemplodedominio.com:8080/mirecurso; donde 8080 es el puerto que se usaría en este caso para acceder al servicio remoto.

