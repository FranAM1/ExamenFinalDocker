# ExamenFinalDocker

# Introduccion
Como proyecto final hemos hecho una aplicación web que consiste en una parte de frontend, backend y base de datos. A continuación haré un despliegue de docker en local de esta aplicación utitilizando la rama *test2* del [repositorio](https://github.com/MarcBelenFran/PokemonProject).

# Configuración del archivo docker-compose.
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
Comenzando con la primera parte del docker-compose, tenemos la version que se utilizará del docker-compose (3.3) y a partir de ahi se definen los servicios mencionados anteriormente. <br>
En primer lugar esta la configuracion de mysql, que en nuestro caso utilizamos un 8.0, ya que versiones anteriores nos daban problemas. <br>
A partir de ahi definimos dos volumenes, ```./test:/var/lib/mysql``` ruta donde se guardara toda la configuracion y datos de la base de datos, y ```./BD:/docker-entrypoint-initdb.d``` ruta donde está guardado el script de creación de la base de datos. <br>
En *environment* se encuentras las diferentes configuraciones de los datos de la base de datos, entre los que se encuentran la base de datos a utilizar, el usuario y la contraseña de la sesion que se utilizará. <br>
Por último seria la configuración de los puertos a utilizar, en mi caso he usado el puerto 3307 ya que tengo un mysql en local que me coge el puerto 3306. <br>

```
phpmyadmin:
    depends_on:
      - db
    image: phpmyadmin/phpmyadmin
    ports:
      - '8081:80'
    environment:
      PMA_HOST: db
      MYSQL_ROOT_PASSWORD: root
 ```
En la configuración del phpadmin no hay mucho que añadir, principalemnte el uso del nombre de *db* haciendo referencia a la imagen de mysql que hemos configurado anteriormente y el uso del puerto 8081. <br>

```
web:
    depends_on:
      - db
    image: tomcat:10.0
    volumes:
      - ./PokemonFBM.war:/usr/local/tomcat/webapps/PokemonFBM.war
    ports:
      - '8080:8080'
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: pokedexdb
      MYSQL_USER: prueba
      MYSQL_PASSWORD: 123456
 ```
 Por último, la configuración del tomcat 10, ya que como en el caso del mysql, las versiones anteriores nos daban problemas con la paqueteria que utiliza java. <br>
 Se vuelven a utilizar los mismo datos que se habian puesto anteriormente en el mysql. Además de añadir un volumen haciendo referencia al archivo *.war* del proyecto donde ya va incluido tanto el frontend como el backend.
 
 ## Pasos para el despliegue de la aplicación.
 Para realizar el despliegué utilizaré, como he mencionado en la introducción, nuestro proyecto ya en git, en la rama test2, donde incluye el *docker-compose* y el *.war*, asi como el script correcto de la creación de la base de datos.
 ![image](https://user-images.githubusercontent.com/91600940/172450905-d1af88bb-bea5-4e59-898e-bdf45fb728f1.png) <br> <br>
 
 Para realizar el despliegue, basta con situarse en la terminal dentro de la carpeta donde se encuentran todos estos archivos y ejectuar un ```docker-compose up -d```, de esta forma comenzará a descargar las imagenes si no estaban ya instaladas y a configurarlas segun el *docker-compose*. <br>
 ![image](https://user-images.githubusercontent.com/91600940/172451505-e58712e2-1eef-4616-977d-ac37fe2e4fb0.png) <br>
 
 Una vez finalizado el docker-compose, ya estarán los contenedores de las imagenes en funcionamiento.
 ![image](https://user-images.githubusercontent.com/91600940/172452538-c4bf4b46-e968-4ea1-b3f3-599aec4278bd.png)

# Demostración del correcto despliegue.
Primer revisaré que la parte del tomcat se ha desplegado correctamente accediendo a http://localhost:8080/PokemonFBM
![image](https://user-images.githubusercontent.com/91600940/172452809-73a85f9e-5cb2-4726-b713-5ccfb08d91d4.png) <br>
Así pues, tambien revisaré todo el apartado relacionado con la base de datos entrando a http://localhost:8081/ ruta del *phpadmin* con las credenciales puestas anteriormente.
![image](https://user-images.githubusercontent.com/91600940/172453340-b705dbae-b902-417a-8278-9df2844d724e.png) <br>
Como se puede obervar toda la creacion inicial de la base de datos con sus inserts incluidos se ha realizado correctamente.
![image](https://user-images.githubusercontent.com/91600940/172453540-af845850-4ca8-4538-bdd4-a0e730b5853c.png)


## Ejemplo de registro en la página.
Registro un usuario de ejemplo con usuario *fran*, correo *fran@gmail.com* y contraseña *123456Fran*
![image](https://user-images.githubusercontent.com/91600940/172453912-2b1c85d4-0f46-42b1-bcbf-61c2513dffa8.png) <br>

Como se puede observar me deja hacer login, y se ha registrado en la base de datos correctamente.
![image](https://user-images.githubusercontent.com/91600940/172454068-8d079f65-f805-49b2-98ad-2e7085edce13.png)
![image](https://user-images.githubusercontent.com/91600940/172454108-e6d5ad9f-5cd0-49d9-b5ed-47d322d50da5.png)


# Preparación y subida de la imagen a dockerhub.

 
 


