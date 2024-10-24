### Instalación de wordpress, mariadb con docker compose

## Explicación de la guía paso a paso

**Índice**
- Creación del archivo docker-compose.yml
- detallar especificaciones de mariadb en el docker-compose.yml
- detallar especificaciones de wordpress en el docker-compose.yml
- lanzamiento del docker compose
- verificar que accedemos a wordpress

# 1. Creación del archivo docker-compose.yml
En el ejercicio de la Tarea 04, hacíamos todo dentro de un contenedor. Esto implicaba que si caía un servidor, todo se caía. Si hacíamos contenedores separados, tendríamos que mapear puertos de ambos y es mucho tiempo. Para hacerlo fácil, vamos a crear un docker-compose.yml

```bash
#creacion del archivo (nombre).yml
mkdir carpetasCompose
nano carpetasCompose/docker-compose.yml
```
Acabamos de crear el archivo compose.yml y vamos a editarlo con nano

# 2. detallar especificaciones de mariadb en el docker-compose.yml
Vamos a crear el servicio mariadb en el compose
```bash
services:
  db:
    image: mariadb:10.11.2
    volumes:
    - db_data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: 123456789
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress_user
      MYSQL_PASSWORD: 123456789

```

# EXPLICACIÓN 
- services: 
```bash
Esta es la sección más importante del archivo, donde defines todos los contenedores que forman parte de tu aplicación
```
- db: 
``` bash
nombre del servicio (si no ponemos uno, docker nos asigna uno por defecto)
```
- image: mariadb:10.11.2: 
```bash
Aquí se indica que el contenedor de base de datos se basará en la imagen mariadb versión 10.11.2. Docker descargará automáticamente esta imagen de Docker Hub si no la tienes en tu máquina.
```
- volumes:
  - db_data:/var/lib/mysql
```bash
volumes: Los volúmenes permiten que los datos que crea el contenedor persistan en tu máquina host, es decir, que si el contenedor se elimina o reinicia, los datos (como las tablas de la base de datos) no se pierdan.

db_data:/var/lib/mysql: Esto asocia un volumen llamado db_data con la carpeta /var/lib/mysql dentro del contenedor, que es donde MariaDB guarda sus bases de datos. Así, aunque elimines el contenedor, los datos persistirán en el volumen db_data en tu máquina host.
```
- restart: always
```bash
Esta opción indica que Docker debe reiniciar el contenedor automáticamente si se detiene o falla.
```

- environment:
```bash
MYSQL_ROOT_PASSWORD: Establece la contraseña del usuario root de MariaDB.
MYSQL_DATABASE: Crea una base de datos llamada wordpress automáticamente.
MYSQL_USER: Crea un usuario de base de datos llamado wordpress_user.
MYSQL_PASSWORD: Establece la contraseña para el usuario wordpress_user.

```

# 3. detallar especificaciones de wordpress en el docker-compose.yml

Vamos a crear el servicio de wordpress en el compose:
```bash
wordpress:
    depends_on:
      - db
    image: wordpress:latest
    ports:
      - "8000:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: wordpress_user
      WORDPRESS_DB_PASSWORD: 123456789
      WORDPRESS_DB_NAME: wordpress

volumes:
  db_data:

```

## EXPLICACIÓN  

-   wordpress:
    depends_on:
    - db
```bash
Esta línea asegura que el contenedor de WordPress no se iniciará hasta que el servicio db (MariaDB) esté en marcha. WordPress necesita conectarse a la base de datos para funcionar correctamente.

```
- image: wordpress:latest

```bash
Aquí se indica que el contenedor de WordPress se basará en la imagen wordpress, utilizando la última versión disponible en Docker Hub.
```

- ports:
 - "8000:80"
```bash
Esto hace un mapeo de puertos entre el contenedor y tu máquina host. El puerto 80 es el puerto que utiliza Apache (el servidor web dentro del contenedor de WordPress), y el puerto 8000 es el puerto en tu máquina. Esto significa que puedes acceder a WordPress en tu navegador desde http://(ip maquina):8000.
```

- restart: always
```bash
 Al igual que el servicio de db, esto asegura que el contenedor de WordPress se reinicie automáticamente si falla.
```

- environment:

```bash
WORDPRESS_DB_HOST: db: WordPress se conectará a MariaDB en el contenedor db.
WORDPRESS_DB_USER: wordpress_user: El nombre de usuario que WordPress usará para conectarse a MariaDB.
WORDPRESS_DB_PASSWORD: 123456789: La contraseña para el usuario de base de datos wordpress_user.
WORDPRESS_DB_NAME: wordpress: El nombre de la base de datos a la que se conectará WordPress (la misma que creaste en el servicio db).
```

# NO PERTENECIENTE A WORDPRESS  
- volumes:
  db_data:
```bash
Aquí defines un volumen llamado db_data. Este volumen se asocia con el contenedor de MariaDB para guardar las bases de datos de forma persistente en tu máquina.

```

# 4. lanzamiento del docker compose
- Nos posicionamos en donde se encuentra el docker compose
```bash
#En mi caso sería
cd carpetasCompose

#comando que inicia el docker compose
docker-compose up -d

```
Debería de salir algo a esto
![consolaCompose](https://github.com/user-attachments/assets/40909c9d-c419-480f-8c76-4f5bf142012d)


# 5. accedemos a wordpress
```bash
#introducimos en la barra de búsqueda
http://192.168.1.46:8000/wp-admin/install.php

#tambien vale
http://192.168.1.46:8000
```
![wordpressConfirmation](https://github.com/user-attachments/assets/552af266-f53b-4d58-8e1b-3c6a9c30ab82)


enhorabuena, has completado la guía: :smiley:



