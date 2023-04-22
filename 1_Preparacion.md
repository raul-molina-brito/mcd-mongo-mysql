# Ejercicio Modelo Relacional vs Modelo Documental.

## Preparando el entorno de trabajo

### Instalación de Sakila en MySQL

Para instalar la base de datos Sakila en MySQL, se pueden seguir los siguientes pasos:

1.  Descargar e instalar Docker: Si aún no se tiene instalado, se debe descargar e instalar Docker en el sistema operativo. Se puede descargar desde la página oficial de Docker.
    
2.  Descargar la imagen de MySQL: Para utilizar MySQL en un contenedor Docker, es necesario descargar la imagen de MySQL. Se puede hacer ejecutando el siguiente comando en la terminal:
    
    `docker pull mysql:8-debian`
    
    Este comando descargará la imagen de MySQL versión 8 en el sistema.
    
3.  Crear un contenedor Docker: Para crear un contenedor de MySQL, se puede utilizar el siguiente comando en la terminal:
    
    `docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=123456 -d mysql:8-debian`
    
    Este comando creará un contenedor de MySQL con el nombre "some-mysql", establecerá una contraseña para el usuario "root" y utilizará la imagen de MySQL descargada anteriormente.
    
4.  Acceder al contenedor Docker: Para acceder al contenedor de MySQL, se puede utilizar el siguiente comando en la terminal:
    
    `docker exec -it some-mysql bash`
    
    Este comando accederá al contenedor de MySQL creado anteriormente y abrirá una sesión de terminal dentro del contenedor.
    
5.  Instalar paquetes adicionales: Para instalar los paquetes necesarios para descargar la base de datos Sakila, se puede utilizar el siguiente comando en la terminal del contenedor:
    
    ```shell
    apt update
    apt install -y curl zip
    
    ```
    
    Estos comandos actualizarán los paquetes existentes y luego instalarán los paquetes "curl" y "zip".
    
6.  Descargar la base de datos Sakila: Para descargar la base de datos Sakila, se puede utilizar el siguiente comando en la terminal del contenedor:
    
    `curl -O https://downloads.mysql.com/docs/sakila-db.zip`
    
    Este comando descargará la base de datos Sakila en formato zip.
    
7.  Descomprimir la base de datos Sakila: Para descomprimir la base de datos Sakila, se puede utilizar el siguiente comando en la terminal del contenedor:
    
    `unzip sakila-db.zip`
    
    Este comando descomprimirá la base de datos Sakila en una carpeta llamada "sakila-db".
    
8.  Importar la estructura de la base de datos Sakila: Para importar la estructura de la base de datos Sakila, se puede utilizar el siguiente comando en la terminal del contenedor:
    
    `mysql -u root -p < sakila-db/sakila-schema.sql`
    
    Este comando importará la estructura de la base de datos Sakila.
    
9.  Importar los datos de la base de datos Sakila: Para importar los datos de la base de datos Sakila, se puede utilizar el siguiente comando en la terminal del contenedor:
    
    `mysql -u root -p < sakila-db/sakila-data.sql`
    
    Este comando importará los datos de la base de datos Sakila.
    
10.  Verificar que la base de datos Sakila está disponible: Para verificar que la base de datos Sakila está disponible, se puede utilizar el siguiente comando en la terminal del contenedor:
    

```shell
mysql -u root -p
mysql> show databases;
mysql> use sakila;

```

Este comando mostrará todas las bases de datos disponibles en MySQL y luego establecerá la base de datos Sakila como la base de datos actual.

Tomese 15 minutos para revisar la estructura de datos de sakila, puede encontrar la documentación en: https://dev.mysql.com/doc/sakila/en/sakila-structure.html

### Instalación de MongoDB

Si ya tienen MongoDB y MongoExpress a partir del `docker-compose.yml` no requiere hacer nada. 

Para verificar si ya esta corriendo puede ejecutar:

`docker ps`

Lo cual desplegará algo similar a:
```bash
CONTAINER ID   IMAGE            COMMAND                  CREATED        STATUS          PORTS                                       NAMES
28fb64804097   mongo            "docker-entrypoint.s…"   12 hours ago   Up 16 minutes   27017/tcp                                   mcd-mongo-mysql_mongo_1
491a8ed87264   mongo-express    "tini -- /docker-ent…"   12 hours ago   Up 16 minutes   0.0.0.0:8081->8081/tcp, :::8081->8081/tcp   mcd-mongo-mysql_mongo-express_1

```

En caso contrario debe ejecutar el siguiente comando:

`docker-compose up -d`

Este comando inicará Mongo para que usted pueda trabajar.

Para ingresar al contenedor puede ejecutar:

```bash
docker exec -it mcd-mongo-mysql_mongo_1 bash
```

Una vez dentro el contenedor debe acceder a la BBDD autenticandose (la constraseya es `example`):

```bash
mongosh -u root -p 
```

A continuación se muestran ejemplos básicos de cómo usar MongoDB a través de la línea de comandos para realizar operaciones comunes como insertar, encontrar, actualizar y eliminar documentos.
    
    a) Crear una base de datos y seleccionarla:
    ```
    use miBaseDeDatos;
    ```
    
    b) Crear una colección e insertar un documento:
    
    ```
    db.misUsuarios.insert({nombre: "Juan", edad: 28, ciudad: "Madrid"});
    
    ```
    
    c) Buscar documentos en la colección:
    
    ```
    db.misUsuarios.find();
    
    ```
    
    d) Buscar documentos con un filtro:
    
    ```
    db.misUsuarios.find({ciudad: "Madrid"});
    
    ```
    
    e) Actualizar un documento en la colección:
    
    ```
    db.misUsuarios.update({nombre: "Juan"}, {$set: {edad: 29}});
    
    ```
    
    f) Eliminar un documento de la colección:
    
    ```
    db.misUsuarios.remove({nombre: "Juan"});
    
    ```
    
    g) Eliminar todos los documentos de la colección:
    
    ```
    db.misUsuarios.remove({});
    
    ```