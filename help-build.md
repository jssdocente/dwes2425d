# Construir documentación de ayuda

### Recursos

- [MkDocs](https://www.mkdocs.org/)
- [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/)
- [Video tutorial](https://www.youtube.com/watch?v=YGg39_zG1fk)
- [Deploying MkDocs to GitHub Pages](https://www.mkdocs.org/user-guide/deploying-your-docs/)

## Primeros pasos

Lo ideal es tener todo dentro de un contenedor Docker, para ello existe un fichero `docker-compose.yml` que se encarga de levantar un contenedor con todo lo necesario para construir la documentación.
El docker-compose se basa en la imagen `squidfunk/mkdocs-material` que es una imagen de Docker que ya tiene instalado MkDocs y el tema Material, el más popular para MkDocs.

Mkdocs escucha por defecto en el puerto 8000, pero a nivel de host se puede mapear a cualquier puerto, en el fichero `docker-compose.yml` se mapea al puerto 8005.

