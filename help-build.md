# Construir documentación de ayuda

### Recursos

- [MkDocs](https://www.mkdocs.org/)
- [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/)
- [Video tutorial](https://www.youtube.com/watch?v=YGg39_zG1fk)
- [Deploying MkDocs to GitHub Pages](https://www.mkdocs.org/user-guide/deploying-your-docs/)
- [Creación y despliegue de documentación con MkDocs](https://josejuansanchez.org/iaw/practica-mkdocs/index.html)

## Primeros pasos

Lo ideal es tener todo dentro de un contenedor Docker, para ello existe un fichero `docker-compose.yml` que se encarga de levantar un contenedor con todo lo necesario para construir la documentación.
El docker-compose se basa en la imagen `squidfunk/mkdocs-material` que es una imagen de Docker que ya tiene instalado MkDocs y el tema Material, el más popular para MkDocs.

Mkdocs escucha por defecto en el puerto 8000, pero a nivel de host se puede mapear a cualquier puerto, en el fichero `docker-compose.yml` se mapea al puerto 8005.


## Conectar a docker

Para conectar al contenedor de Docker, como es un docker-compose, se puede hacer con el siguiente comando:

```bash
docker-compose up -d  # Levanta el contenedor en segundo plano
docker-compose exec mkdocs sh  # mkdocs es el nombre del servicio dentro del docker-compose
```

## Despliegue

### Despliegue a través de Mkdocs

Para desplegar la documentación en GitHub Pages, se puede hacer de la siguiente manera:

```bash
mkdocs gh-deploy
```

### Despliegue a través de GitHub Actions

Se puede configurar un flujo de trabajo en GitHub Actions para que se despliegue automáticamente la documentación en GitHub Pages cada vez que se haga un push a la rama `main`.

Para ello, se puede crear un fichero `.github/workflows/build-push-mkdocs.yml` con el siguiente contenido:

```yaml
name: build-push-mkdocs

# Eventos que desescandenan el workflow
on:
  push:
    branches: ["master"]

  workflow_dispatch:

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:

  # Job para crear la documentación de mkdocs
  build:
    # Indicamos que este job se ejecutará en una máquina virtual con la última versión de ubuntu
    runs-on: ubuntu-latest
    
    # Definimos los pasos de este job
    steps:
      - name: Clone repository
        uses: actions/checkout@v4

      - name: Install Python3
        uses: actions/setup-python@v4
        with:
          python-version: 3.x

      - name: Install Mkdocs
        run: |
          pip install mkdocs
          pip install mkdocs-material 

      - name: Build MkDocs
        run: |
          mkdocs build

      - name: Push the documentation in a branch
        uses: s0/git-publish-subdir-action@develop
        env:
          ACTIONS_ALLOW_USE_UNSECURE_NODE_VERSION: true
          REPO: self
          BRANCH: gh-pages # The branch name where you want to push the assets
          FOLDER: site # The directory where your assets are generated
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }} # GitHub will automatically add this - you don't need to bother getting a token
          MESSAGE: "Build: ({sha}) {msg}" # The commit message
```
El último paso que tendremos que realizar será configurar los permisos que tendrá el GITHUB_TOKEN cuando se ejecute el workflow en este repositorio.

Para configurar el repositorio seleccionamos: Settings -> Actions -> General.

Buscamos la sección `Workflow permissions` y seleccionamos la opción Read and write permissions.
