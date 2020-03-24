# Curso de Travis CI

## Archivo de configuración de `.travis.yml`

Seleccionar diferentes lenguajes de programación:

```yaml
language: node_js
language: ruby
language: java
language: python
language: php
language: go
```

Determinar sistema operativo a trabajar:

```yml
os: osx
```

Git Clone Depth

```yaml
git:
  # 3 niveles de git
  depth:3
```

Especificar a cuales ramas se le debe hacer build usando una safelist , o una blocklist para determinar a cuales no se le debe hacer el build.

```yaml
branches:
# blocklist
branches:
  except:
  - legacy
  - experimental

# safelist
branches:
  only:
  - master
  - stable
```

Instalar un segundo lenguaje de programación:

```yaml
language: python

before_install:
  - rvm install 2.1.5
```

```yaml
install:
  - yarn install
# - apt install curl
```

Determinar una serie de scripts a seguir:

```yaml
script:
  - yarn deploy
  - yarn test

before_script:
  - clean

after_script:
  - yarn clean
```

Determinar el cache de la aplicación:

```yaml
cache:
  directories:
    - node_modules # mantener en cache node_modules
```

Construir una matriz se puede realizar de la siguiente forma:

```yaml
jobs:
  include:
    - stage: test
      script: yarn test
      script: yarn eslint
    - stage: deploy
      script: yarn deploy
```

Para configurar el deploy:

```yaml
deploy:
  provider: heroku
  on
    repo: Diegoalesco95/platzi-store
```

Configuración de `.travis.yml` en platzi-store

```yml
language: node_js

cache:
  directories:
    - node_modules
    - ~/.npm

node_js: # Determinar la versión de node
  -'12'

git:
  depth:3

script:
  # la configuracion de travis tiene yarn
  - yarn build

deploy: #configuración de deploy
  provider: pages # github pages
  skip-clean:true
  keep-history:true
  github-token: $GITHUB_TOKEN
  local-dir: dist/
  target-branch: gh-pages
  commit_message:"Deploy del proyecto"
  on:
    branch: master

# por defecto yarn hace yarn install
```

## Test & Deploy de Platzi Store

1. Para continuar con el deploy del proyecto, se debe ir a [travis-ci](https://travis-ci.org/account/repositories) y buscar nuestro proyecto en la lista, activarlo y acceder a las configuraciones del mismo para establecer las variables de entorno (en caso de usarlas).
2. Para generar un token en GitHub se debe seguir la ruta settings / Developer settings / Personal access tokens / Generate new token.
3. Copiar el token generado.
4. Volver a la página de configuración del proyecto en Travis.
5. Ingresar los valores en los campos NAME y VALUE para crear la variable de entorno.
6. Finalmente acceder a las opciones del proyecto en Travis y hacer click en "trigger build" en donde se empezará a realizar deploy del proyecto de GitHub, generando la rama gh-pages y posteriormente si todo esta correcto, nuestro proyecto estará listo para su visualización.

## Buenas prácticas de seguridad

Buenas practicas de seguridad: En este curso utilizamos información sensible que deberíamos ocultar. Para eso Travis-cli nos ofrece el siguiente comando:

```shell
travis encrypt tu_clave
```

Para encriptar la información sensible y que solo Travis pueda entender.

Luego de ejecutar este comando, Travis genera lo siguiente

```yaml
secure:"..."
```

Luego se integra de la siguiente forma:

```yaml
# ejemplo con api key
api_key:
	secure:"..."
```

Asegurando las claves y/o información sensible.
