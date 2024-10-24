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



