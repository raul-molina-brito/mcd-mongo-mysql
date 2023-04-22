# mcd-mongo-mysql
Ejemplo de diferencias entre mongodb y MySQL

1. Luego de crear la bbdd.

Todos los libros que escribio el autor apellidado Orwell.

SELECT b.* 
FROM 
    book b 
    JOIN book_author ba USING(book_id)  
    JOIN author a  USING (author_id) 
WHERE 
    UPPER(a.last_name) = upper('oRWell');

--- Primeros pasos en mongo:

test> use miDB;
switched to db miDB

miDB> db.alumnos.insert( { "nombre": "Juan", "apellido": "Perez", "fecha_nacimiento": 956365176 } )

Ahora insertamos:

{
  "nombre": "Juan",
  "apellido": "Perez",
  "fecha_nacimiento": 956365176,
  "materias": [
    {
      "name": "Calculo 1",
      "profesor": "Jaime Perez"
    },
    {
      "name": "Estadistica 1",
      "profesor": "Marco Gonzales"
    }
  ]
}

db.alumnos_collection.insert( { "nombre": "Juan", "apellido": "Perez", "fecha_nacimiento": 956365176, "materias": [ { "name": "Calculo 1","profesor": "Jaime Perez" }, {"name": "Estadistica 1", "profesor": "Marco Gonzales" }] } )

alumnos> db.alumnos_collection.find();

== Simular la estructura relacional

db.authors.insertMany([
  { _id: ObjectId(), first_name: "J.D.", last_name: "Salinger", birth_date: ISODate("1919-01-01") },
  { _id: ObjectId(), first_name: "Harper", last_name: "Lee", birth_date: ISODate("1926-04-28") },
  { _id: ObjectId(), first_name: "George", last_name: "Orwell", birth_date: ISODate("1903-06-25") },
  { _id: ObjectId(), first_name: "F. Scott", last_name: "Fitzgerald", birth_date: ISODate("1896-09-24") },
  { _id: ObjectId(), first_name: "Gabriel", last_name: "García Márquez", birth_date: ISODate("1927-03-06") },
  { _id: ObjectId(), first_name: "Toni", last_name: "Morrison", birth_date: ISODate("1931-02-18") },
]);

db.books.insertMany([
  {
    _id: ObjectId(),
    name: "The Catcher in the Rye",
    release_date: ISODate("1951-07-16"),
    quantity: 5,
    price: 10.99,
    authors: [
      { author_id: ObjectId(), first_name: "J.D.", last_name: "Salinger", birth_date: ISODate("1919-01-01") },
      { author_id: ObjectId(), first_name: "Harper", last_name: "Lee", birth_date: ISODate("1926-04-28") }
    ]
  }
]);

db.books.insertMany([
  {
    _id: ObjectId(),
    name: "Lo que el viento se llevo",
    release_date: ISODate("1951-07-16"),
    quantity: 5,
    price: 10.99,
    authors: [
      ObjectId("644335a458af7834dc8785a4"), ObjectId("644335a458af7834dc8785a5")
    ]
  }
]);
--
SELECT a.* 
FROM author a 
JOIN book_author ba USING(author_id)  
JOIN book b  USING (book_id)
WHERE UPPER(b.name) = upper('1984');


SELECT b.* 
FROM 
    book b 
    JOIN book_author ba USING(book_id)  
    JOIN author a  USING (author_id) 
WHERE 
    UPPER(a.last_name) = upper('Perez');

UPDATE author SET last_name = 'PEREZ' WHERE author_id = 3

-- Primera opción denormalizar pero si el nombre del libro cambia en la colección books.
-- El cambio no se vera reflado aqui.

db.authors.insertOne(
  { _id: ObjectId(), first_name: "Franz.", last_name: "Tamayo", 
  birth_date: ISODate("1919-01-01"),
  books: {
    _id: ObjectId(),
    name: "The Catcher in the Rye",
    release_date: ISODate("1951-07-16"),
    quantity: 5,
    price: 10.99,
  } }
);


db.authors.insertOne(
  { _id: ObjectId(), first_name: "Victor", last_name: "Viscarra", 
  birth_date: ISODate("1919-01-01"),
  books: [ObjectId("5349b4ddd2781d08c09890f3"), ObjectId("5349b4ddd2781d01239890f3") ] 
  }
);

Libros:
    10, Calculo 1:
        - 3
        - 4
    11, Estadistica 1:
        - 3
        - 5
        - 6
    12, Algebra:
        -3

Autores:
    3, Juan Perez:
        - 10
        - 11
    4, Maria Gomez:
        - 10
        - 20
        - 22