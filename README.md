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

