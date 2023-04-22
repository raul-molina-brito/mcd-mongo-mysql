# Ejercicio de MongoDB.

El objetivo de este ejercicio es comparar como datos similares pueden almacenarse en un almacen documental.

## Importación de sakila.

Verifique que su mongo esta corriendo:

```
docker ps
````

En caso de que no estuviera corriendo:

```
docker-compose up -d
```

Ingrese a su contenedor Mongo

```bash
docker exec -it mcd-mongo-mysql_mongo_1 bash
```

Instale las siguientes herramientas:

```bash
apt update
apt install -y curl zip
```

Descargamos la simulación de Sakila para actores.

```
curl -L https://github.com/ernestomar/mcd-mongo-mysql/raw/main/sakila.zip --output sakila.zip
````

Lo descomprimimos:

```
unzip sakila.zip
```

Restablecemos la BBDD:

```
mongorestore --db sakila -u root -p example --authenticationDatabase admin  sakila
```

Verificamos en la consola Web (8081)) si tenemos la Base de Datos Sakila.


```bash
mongosh -u root -p example
```

Nos conectamos a la BBDD

```
use sakila
```

## Ejercicio

1. Listar la cantidad de películas que se hicieron por lenguaje. (Ver: lookup, unwind, group, sort)

Resultado:
```
[
  { _id: 'Spanish', count: 135 },
  { _id: 'Chinese', count: 135 },
  { _id: 'Japanese', count: 134 },
  { _id: 'German', count: 134 },
  { _id: 'French', count: 134 },
  { _id: 'English', count: 119 },
  { _id: 'Russian', count: 107 },
  { _id: 'Italian', count: 102 }
]
```

Respuesta:
```
db.film.aggregate([
  {
    $lookup: {
      from: "language",
      localField: "language_id",
      foreignField: "_id",
      as: "language"
    }
  },
  {
    $group: {
      _id: "$language.name",
      count: { $sum: 1 }
    }
  },
])


2. Seleccionar todos los actores que participaron mas de 35 peliculas. (Ver: match, group, lookup, project)

Resutado:
```
[
  { first_name: 'Jonathan', last_name: 'Smith', film_count: 57 },
  { first_name: 'Tiffany', last_name: 'Santiago', film_count: 44 },
  { first_name: 'Andrew', last_name: 'Ramsey', film_count: 64 },
  { first_name: 'Richard', last_name: 'Clarke', film_count: 63 },
  { first_name: 'Xavier', last_name: 'Reid', film_count: 54 },
  { first_name: 'Gary', last_name: 'Mcgee', film_count: 65 },
  { first_name: 'Patricia', last_name: 'Reed', film_count: 53 },
  { first_name: 'Keith', last_name: 'Owen', film_count: 57 },
  { first_name: 'Jonathan', last_name: 'Washington', film_count: 65 },
  { first_name: 'Abigail', last_name: 'Robinson', film_count: 49 },
  { first_name: 'Brooke', last_name: 'Gomez', film_count: 55 },
  { first_name: 'Christopher', last_name: 'Ashley', film_count: 55 },
  { first_name: 'Anita', last_name: 'Sellers', film_count: 57 },
  { first_name: 'Edward', last_name: 'Kim', film_count: 63 },
  { first_name: 'Kayla', last_name: 'Lopez', film_count: 59 },
  { first_name: 'Jeanette', last_name: 'Sampson', film_count: 48 },
  { first_name: 'Cynthia', last_name: 'Morris', film_count: 51 },
  { first_name: 'David', last_name: 'Butler', film_count: 65 },
  { first_name: 'Deborah', last_name: 'Thompson', film_count: 62 },
  { first_name: 'Oscar', last_name: 'Bell', film_count: 47 }
]
```

Respuesta:

```
db.film.aggregate([
  {
    $lookup: {
      from: "film_actor",
      localField: "_id",
      foreignField: "film_id",
      as: "film_actors"
    }
  },
  {
    $unwind: "$film_actors"
  },
  {
    $lookup: {
      from: "actor",
      localField: "film_actors.actor_id",
      foreignField: "_id",
      as: "actors"
    }
  },

])
```

3. Mostrar el listado de los 10 de actores que mas peliculas realizó en la categoria `Comedy`. (Ver lookup, unwind, match, group, limit)

Resultado:
```
[
  { first_name: 'Pamela', last_name: 'Cortez', comedy_film_count: 14 },
  { first_name: 'Cynthia', last_name: 'Morris', comedy_film_count: 14 },
  { first_name: 'Michael', last_name: 'Flores', comedy_film_count: 11 },
  { first_name: 'Joseph', last_name: 'Hayes', comedy_film_count: 11 },
  { first_name: 'Jeffrey', last_name: 'Clark', comedy_film_count: 11 },
  { first_name: 'Theresa', last_name: 'Adams', comedy_film_count: 10 },
  { first_name: 'Lisa', last_name: 'Rose', comedy_film_count: 10 },
  { first_name: 'Keith', last_name: 'Owen', comedy_film_count: 10 },
  { first_name: 'David', last_name: 'Butler', comedy_film_count: 10 },
  { first_name: 'Lisa', last_name: 'Smith', comedy_film_count: 9 }
]
```


Respuesta:
```
db.film_actor.aggregate(...
```