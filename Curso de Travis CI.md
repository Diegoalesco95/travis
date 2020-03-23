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

````yaml
script:
  - yarn deploy
  - yarn test

before_script:
  - clean

after_script:
  - yarn clean

````

Determinar el cache de la aplicación:

```yaml
cache:
  directories:
    - node_modules # mantener en cache node_modules
```

