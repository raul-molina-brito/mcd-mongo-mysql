# Practica Modelo Relacional.

La siguiente practica evalua su conocimiento de consultas con Modelo Relacional.

## Como enviar sus respuesta.

1. Debe crear este archivo en su `fork` con el mismo nombre.
2. Debe copiar el contenido.
3. Debe comenzar a resolver las consultas.
4. Debe guardar el progreso en cada paso, puede cambiar la respuesta las veces que quiera, pero debe guardar el progreso cada vez. (Muy importante).

## Guardar progreso.

Para guardar su progreso cada vez que termine un ejercicio, o cambie una consulta. Ejecuta lo siguiente en un nuevo terminal:

```bash
git add .
git commit -m "Enviando progreso"
git push
```

## Ejercicios:

1. Listar la cantidad de tiendas por ciudad.

Salida:
```
+---------+------------+-------------+
| city_id | city       | store_count |
+---------+------------+-------------+
|     300 | Lethbridge |           1 |
|     576 | Woodridge  |           1 |
+---------+------------+-------------+
```

Respuesta:
```sql
-- Su respuesta aqui:

SELECT  c.city_id,c.city, count(c.city_id) as store_count
FROM  store s, address a, city c, film f
WHERE s.address_id=a.address_id
and  a.city_id=c.city_id
group by  c.city_id,c.city;
```

2. Listar la cantidad de películas que se hicieron por lenguaje.

Salida:
```
+-------------+----------+------------+
| language_id | language | film_count |
+-------------+----------+------------+
|           1 | English  |       1000 |
|           2 | Italian  |          0 |
|           3 | Japanese |          0 |
|           4 | Mandarin |          0 |
|           5 | French   |          0 |
|           6 | German   |          0 |
+-------------+----------+------------+
```

Respuesta:
```sql
-- Su respuesta aqui:

SELECT la.language_id, la.name as language, count(la.language_id) as cantidad
FROM film f, language la
where  
f.language_id = la.language_id
group by la.language_id, la.name ;
```

3.  Seleccionar todos los actores que participaron mas de 35 peliculas.

Salida:

```
+----------+------------+-----------+------------+
| actor_id | first_name | last_name | film_count |
+----------+------------+-----------+------------+
|       23 | SANDRA     | KILMER    |         37 |
|       81 | SCARLETT   | DAMON     |         36 |
|      102 | WALTER     | TORN      |         41 |
|      107 | GINA       | DEGENERES |         42 |
|      181 | MATTHEW    | CARREY    |         39 |
|      198 | MARY       | KEITEL    |         40 |
+----------+------------+-----------+------------+
6 rows in set (0.01 sec)
```

Respuesta:
```sql
-- Su respuesta aqui:

SELECT  a.actor_id, a.first_name , a.last_name , count(a.actor_id) as film_count
from film f,  film_actor fa, actor a
WHERE f.film_id=fa.film_id
and fa.actor_id=a.actor_id
group by a.actor_id, a.first_name , a.last_name
having count(a.actor_id) > 35;
```

4. Mostrar el listado de los 10 de actores que mas peliculas realizó en la categoria `Comedy`.

Salida:
```
+----------+------------+-----------+-------------------+
| actor_id | first_name | last_name | comedy_film_count |
+----------+------------+-----------+-------------------+
|      196 | BELA       | WALKEN    |                 6 |
|      143 | RIVER      | DEAN      |                 5 |
|      149 | RUSSELL    | TEMPLE    |                 5 |
|       82 | WOODY      | JOLIE     |                 4 |
|      127 | KEVIN      | GARLAND   |                 4 |
|       58 | CHRISTIAN  | AKROYD    |                 4 |
|       24 | CAMERON    | STREEP    |                 4 |
|      129 | DARYL      | CRAWFORD  |                 4 |
|       83 | BEN        | WILLIS    |                 4 |
|       37 | VAL        | BOLGER    |                 4 |
+----------+------------+-----------+-------------------+
```

Respuesta:
```sql
-- Su respuesta aqui:

SELECT  a.actor_id, a.first_name , a.last_name , count(a.actor_id) as comedy_film_count
from film f,  film_actor fa, actor a, film_category fc, category c
WHERE f.film_id=fa.film_id
and fa.actor_id=a.actor_id
and f.film_id =fc.film_id
and fc.category_id=c.category_id
and c.name='Comedy'
group by a.actor_id, a.first_name , a.last_name
order by comedy_film_count desc limit 10;

```

5. Obtener la lista de actores que NO han participado en ninguna película de categoría "Comedy":

Salida:
```
+----------+------------+-------------+
| actor_id | first_name | last_name   |
+----------+------------+-------------+
|        3 | ED         | CHASE       |
|        6 | BETTE      | NICHOLSON   |
|        7 | GRACE      | MOSTEL      |
|        8 | MATTHEW    | JOHANSSON   |
|        9 | JOE        | SWANK       |
... Se corta por longitud
|      189 | CUBA       | BIRCH       |
|      190 | AUDREY     | BAILEY      |
|      195 | JAYNE      | SILVERSTONE |
|      200 | THORA      | TEMPLE      |
+----------+------------+-------------+

53 filas total
```
Respuesta:
```sql
-- Su respuesta aqui:

SELECT  a.actor_id, a.first_name , a.last_name 
from film f,  film_actor fa, actor a
WHERE f.film_id=fa.film_id
and fa.actor_id=a.actor_id
and a.actor_id not in( SELECT  a1.actor_id
from film f1,  film_actor fa1, actor a1, film_category fc1, category c1
WHERE f1.film_id=fa1.film_id
and fa1.actor_id=a1.actor_id
and f1.film_id =fc1.film_id
and fc1.category_id=c1.category_id
and c1.name='Comedy'
group by a1.actor_id
) 
group by a.actor_id, a.first_name , a.last_name

```
