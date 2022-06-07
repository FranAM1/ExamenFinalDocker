# ExamenFinalDocker

## Introduccion
Como proyecto final hemos hecho una aplicación web que consiste en una parte de frontend, backend y base de datos. A continuación haré un despliegue de docker en local de esta aplicación utitilizando la rama *test2* del [repositorio](https://github.com/MarcBelenFran/PokemonProject).

## Configuración del archivo docker-compose.
En el repositorio ya tenemos el docker-compose para nuestro proyecto. Que se divide en la configuración de mysql, tomcat y phpmyadmin.

```
version: '3.3'
services:
  db:
    image: mysql:8.0
    volumes:
      - ./test:/var/lib/mysql
      - ./BD:/docker-entrypoint-initdb.d
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: pokedexdb
      MYSQL_USER: prueba
      MYSQL_PASSWORD: 123456
    ports:
      - 3307:3306
    cap_add:
      - SYS_NICE
```
Comenzando con la primera parte del docker-compose, tenemos la version que se utilizará del docker-compose (3.3) y a partir de ahi se definen los servicios mencionados anteriormente. 
En primer lugar esta la configuracion de mysql, que en nuestro caso utilizamos un 8.0, ya que versiones anteriores nos daban problemas. 
A partir de ahi definimos dos volumenes, ```./test:/var/lib/mysql``` ruta donde se guardara toda la configuracion y datos de la base de datos, y ```./BD:/docker-entrypoint-initdb.d``` ruta donde está guardado el script de creación de la base de datos.
En *environment* se encuentras las diferentes configuraciones de los datos de la base de datos, entre los que se encuentran la base de datos a utilizar, el usuario y la contraseña de la sesion que se utilizará.
Por último seria la configuración de los puertos a utilizar, en mi caso he usado el puerto 3307 ya que tengo un mysql en local que me coge el puerto 3306.
