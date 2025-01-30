# Laravel: Pildoras fundamentales

### Rutas: organización y tipos

Desde la versión 5.5 de laravel, tenemos 4 tipos de rutas: web, api, console y channel. Cada tipo de ruta tiene un propósito específico y se organiza de una manera diferente.

Estos archivos nos permiten organizar nuestras rutas según el propósito que tienen.
- `web.php` se definnen rutas de nuestra aplicación web (aquellas que consultan nuestros usuarios desde el navegador).
- `api.php` se definen rutas de nuestra API (aquellas que consultan a través de un tercero).
- En `console.php` han de estar nuestros comandos (sí, podemos declarar nuestros propios artisan commands por si aún no lo sabes).
- Y en `channel.php` se definen las rutas de los canales de eventos.

[Laravel: Aprende a organizar las rutas de tu proyecto](https://programacionymas.com/blog/organizar-rutas-laravel)


### Cookies y sessions vs JWT

Las cookies y las sesiones son dos formas de almacenar información en el lado del cliente. Las cookies son pequeños archivos de texto que se almacenan en el navegador del usuario. Las sesiones son un mecanismo de almacenamiento de información en el servidor. Y JWT es un estándar abierto que define una forma compacta y autónoma para transmitir información entre las partes como un objeto JSON.

- [Cookies y Sessions VS JSON Web Tokens](https://programacionymas.com/blog/jwt-vs-cookies-y-sesiones)


### Subir imágenes via Ajax

Subir imágenes a través de un formulario es una tarea común en las aplicaciones web. En este tutorial, aprenderás cómo subir imágenes a través de un formulario utilizando Ajax y Laravel como Backend.

- [Cómo subir una imagen vía Ajax (usando Laravel como backend)](https://programacionymas.com/blog/subir-imagen-usando-ajax-y-laravel)


### Laravel: Crear helpers personalizados

Los helpers son funciones globales que podemos utilizar en cualquier parte de nuestra aplicación. Laravel nos permite crear nuestros propios helpers personalizados para que podamos reutilizar funciones comunes en toda nuestra aplicación.

- [Cómo definir nuestros propios helpers en Laravel (2 pasos)](https://programacionymas.com/blog/como-definir-helpers-en-laravel)


### Enviar emails con Laravel

Laravel proporciona una API limpia y simple para enviar correos electrónicos. En este tutorial, aprenderás cómo enviar correos electrónicos utilizando la clase `Mail` de Laravel.

- [Laravel: ¿Cómo enviar correos?](https://programacionymas.com/blog/como-enviar-mails-correos-desde-laravel)