# Crud y consultas en MongoDB

## Crear una base de datos      

Solo se crea si contiene por lo menos una coleccion

CLS Para limpiar la pantalla
use bd1 

## Como crear una coleccion
use bd1
db.createCollection("Empleado")

## Mostrar las colecciones
show collections
Muestra empleado solamente

## Como insertar un Documento

db.Alumnos.insertOne(
  {
    "nombre": "Soyla",
    "apellido1": "vaca",
    "edad": 32,
    "ciudad": "San miguel de las Piedras"
  }
)

## SELECT * FROM Alumnos

 db.Alumnos.find({})

## Insercion de un mas complejo con array

 db.Alumnos.insertOne(
    { nombre: 'Joaquin',
     apellido: "Dorian",
      apellido2: "Guerrero",
       edad: 15, aficiones: [
         'Cerveza', 'Hueva', 'Canabis']
    }
)

## Insercion de documentos mas complejos con documentos anidados y ID
 db.Alumnos.insertOne(
... {
... nombre: 'Kevin',
... apellido1: 'Vega',
... apellido2: 'Zarza',
... edad: 20,
... aficiones: ['Musica','Leer','Bailar','Programar','Dibujar'],
... experiencia: {
... lenguaje: 'Sql',
... sbd: 'SQL Server',
... aniosExp: 14
... }
... })

## Insertar multiples documentos

db.Alumnos.insertMany([
  {
    "_id": 12,
    "nombre": "Roberto",
    "apellido": "Gomez",
    "edad": "23",
    "descripcion": "Es un comediante bueno"
  },
  {
    "nombre": "Luis",
    "apellido": "Suarez",
    "edad": 43,
    "habilidades": [
      "Correr",
      "Dormir",
      "Morder"
    ],
    "direcciones": {
      "calle": "Del infierno",
      "numero": 666
    },
    "esposas": [
      {
        "nombre": "Marisol",
        "edad": 20,
        "pension": 350,
        "hijos": ["Joaquin", "Bridget"]
      },
      {
        "nombre": "Dorien",
        "edad": 46,
        "pension": 6500.56,
        "complaciente": true
      }
    ]
  }
])

## Practica1
## Cargar Datos
[Libro.json](./data/libros.json)

## Busquedas: Condiciones simples de igualdad: Metodo find()
1. Seleccionar todos los documentos de la coleccion libros
SELECT * FROM libros;

``` json
db1> db.libros.find({})
```
2. Mostrar los documentos que sean de la editorial biblio

``` json
db1> db.libros.find({editorial:"Biblio"})
```

3. Mostrar todos los documentos que el precio sea 25
``` json
db1> db.libros.find({precio:25})
```

4. Seleccionar donde todos los documentos donde el titulo sea json para todos
``` json
db1> db.libros.find({titulo:"JSON para todos"})
```
5. [Operadores de comparacion](https://www.mongodb.com/docs/manual/reference/operator/query/)

![Operadores de Comparacion](/img/image.png)

1. Mostrar todos los documentos donde el precio sea mayor a 25
``` json
db1> db.libros.find({precio: {$gt: 25}})
```

2. Mostrar los documentos donde el precio sea 25
``` json
db1> db.libros.find({precio: {$eq: 25}})
```

3. Mostrar los documentos cuya cantidad sea menor a 5
``` json
db1> db.libros.find({cantidad: {$lt: 5}})
```
4. Mostrar los documentos que pertenezacan a la editorial biblio o planeta
``` json
db1> db.libros.find({editorial: {$in: ["Biblio","Planeta"]}})
```

5. Mostrar todos los documentos de libros que cuesten 20 o 25
``` json
db1> db.libros.find({precio: {$in: [20,25]}})
```
6. Mostrar todos los documentos de libros que no cuesten 20 o 25
``` json
db.libros.find({precio: {$nin: [20,25]}})
```
7. Mostrar el primer documento de libros que cueste 20 o 25
``` json
db1> db.libros.findOne({precio: {$in: [20,25]}})
```
## Operadores Logicos

[Operadores Logicos](https://www.mongodb.com/docs/manual/reference/operator/query/)

![Operadores Logicos](/img/OperadoresLogicos.png)

### Operador AND
Dos posibles opciones de AND

1. La simple, mediante condiciones separadas por comas
***Sintaxis***

db.coleccion.find({condicion1,condicion2}) -> Con esto asumo que es 
una ***and***

2. Usando el operador $and
***sintaxis***
db.coleccion.find({$and:[{condicion1},{condicion2}]})

## Ejercicios
1. Mostrar todos aquellos libros que cuesten mas de 25 y cuta cantidad
sea inferior a 15

***Primero de la forma simple***
``` json
db1> db.libros.find({precio: {$gt: 25}, cantidad: {$lt:15}})
```
2. Mostrar todos aquellos libros que cuesten mas de 25 y cuya cantidad sea inferior a 15 y id igual 4
``` json
db1> db.libros.find({precio: {$gt: 25}, cantidad: {$lt:15}, _id:{$eq:4}})
```

***Operador $and***
``` json
db1> db.libros.find({$and:[{precio: {$gt: 25}}, {cantidad: {$lt:15}}]})
```

``` json
db1> db.libros.find({$and:[{precio: {$gt: 25}}, {cantidad: {$lt:15}}}]})
```

### Operador AND

### Operador OR

## Mostrar todos aquellos libros que cuesten mas de 26 o cuya cantidad sea inferior a 15
``` json
db1> db.libros.find({$or:[{precio: {$gt: 25}}, {cantidad: {$lt:15}}]})
```

### AND y OR Combinadas

1. Mostrar los libros de la editorial biblio con precio mayor a 40 o libros de la editorial Planeta con precio mayor 30 

db.libros.find({$or:[{precio: {$gt: 25}}, {cantidad: {$lt:15}}]})


db.libros.find({$or:[{ $and: [{ editorial: 'Biblio' }, { precio: { $gt: 30 } }] },{ $and: [{ editorial: 'Planeta' }, { precio: { $gt: 20 } }] }]})

db.libros.find({
  $or: [
    {editorial:'Biblio', precio:{#gt:30}},
    {editorial:'Planeta', precio:{$gt:20}}
  ]
})

### Proyeccion de columnas

db.coleccion.find(filtro,columnas)

db.libros.find({},{titulo:1})

1. Seleccionar todos los documentos, mostrando el titulo y la editorial

``` json
db1> db.libros.find({},{titulo:1,editorial:1}

db1> db.libros.find({},{titulo:1,editorial:1,_id:0})
```

2. Seleccionar todos los documentos de la editorial planeta, mostrando solamente el titulo y la editorial

``` json
db1> db.libros.find({},{titulo:1,editorial:'Planeta',_id:0})
```

# Practica1 
## Operador exits (permite saber si un campo se encuentra o no en un documento)

db.libros.fin({ editorial:{exits:true}})

db.libros.insertOne(
  {
    _id: 10,
    titulo: 'Mongo en entornos graficos',
    editorial: 'Terra',
    precio: 125
  }
)

db.libros.find(
  {
    cantidad:{exits:false}
  }
)

1. Mostrar todos los documentos que no contengan el campo cantidad

db.libros.find({cantidad: {$exits:true}})


2. Operador Type (Permite preguntar si un determinado campo corresponde con un tipo)

1. Mostrar todos los documentos donde el precio sean dobles

db.libros.find({precio:{$type:1}}) equivalencia de datos double

db.libros.find({precio:{$type:16}}) equivalencia de datos int

db.libros.insertOne({
  _id:11,
  titulo: 'IA',
  editorial:'Terra',
  precio:125.4,
  cantidad:20
  })

1. Seleccionar los documentos donde la editorial sea de tipo entero.

db.libros.find({editorial:{$type:16}})
db.libros.find({editorial:{$type:"string"}})

2. Seleccionar todos los documentos donde la editorial sea string

db.libros.find({editorial:{$type:"string"}})
db.libros.find({editorial:{$type:2}})

## Practica de consultas

1. Instalar las tools de MongoDb
[DatabaseTools](https://www.mongodb.com/try/download/database-tools)

2. Cargar el json empleados (debemos estar ubicados en la carpeta donde se encuentra
el JSON empleados)

- En local:
  comando: 
    mongoimport --db curso --collection empleados --file empleados.json


## Operador exists (Permite saber si un campo si un campo se encuentra o no en un documento)

json
db.libros.find( { editorial:{ $exists:true } } )


1. Mostrar todos los documentos que no contengan el campo cantidad

json
db.libros.find(
 {
   cantidad:{$exists:false}
 }
)


## Operador Type (Permite preguntar si un determinado campo corresponde con un tipo) 

[Operador Type](https://www.mongodb.com/docs/manual/reference/operator/query/type/#mongodb-query-op.-type)

1. Mostrar todos los documentos donde el precio sean dobles

json
db.libros.find({precio:{$type:1}})
db.libros.find({precio:{$type:16}})


json
db.libros.insertOne({
  _id:11,
  titulo: 'IA',
  editorial: 'Terra', 
  precio:125.4,
  cantidad: 20
})


json
db.libros.insertMany

## Operador exists (Permite saber si un campo si un campo se encuentra o no en un documento)

json
db.libros.find( { editorial:{ $exists:true } } )


1. Mostrar todos los documentos que no contengan el campo cantidad

json
db.libros.find(
 {
   cantidad:{$exists:false}
 }
)


## Operador Type (Permite preguntar si un determinado campo corresponde con un tipo) 

[Operador Type](https://www.mongodb.com/docs/manual/reference/operator/query/type/#mongodb-query-op.-type)

1. Mostrar todos los documentos donde el precio sean dobles

json
db.libros.find({precio:{$type:1}})
db.libros.find({precio:{$type:16}})


json
db.libros.insertOne({
  _id:11,
  titulo: 'IA',
  editorial: 'Terra', 
  precio:125.4,
  cantidad: 20
})


json
db.libros.insertMany([
 {
    _id: 12,
    titulo: 'IA',
    editorial: 'Terra',
    precio: 125, 
	cantidad: 20
  },
  {
    _id: 13,
    titulo: 'Python para todos',
    editorial: 2001,
    precio: 200, 
	cantidad: 30
  }]
  )


json
db.libros.find({_id:13}

db.libros.insertOne({editorial:{$type:16}})


1. Seleccionar todos los documentos don


## Practica de consultas
1. Instalar las tools de monofdb


2. Cargar json
--En local:
  mongoimport --db curso --collection empleados --file empleados

# Modificando Documentos
## Comandos importabtes
1. updateOne -> Modififcar un solo documento
2. updateMany -> Modificar multiples documentos 
3. replaceOne -> Sustituir el contenido completo de un documento

Tiene el segundo formato:

json
db.collection.updateOne(
  {filtro},{operador: }
)


### Operador set
1. Modificar un documento
json
db.libros.updateOne({titulo:'Python para torpes'},{$set:{titulo:'Java para todos'}})


db.libros.updateOne({_id:2},{$set:{precio:56}})

2. Actualizar el precio a 100 y la cantidad a 50 para el _id: 10
json
db.libros.updateOne({_id:10},{$set:{precio:100, cantidad: 50}})


### Modificar Multiples Documentos

--Modificar todos los documentos donde el precio sea mayor a 100 a un precio de 150
json
db.libros.updateMany({precio:{$gt:100}},{$set:{precio:150}})


2. Operador $inc y $mul

--Actualizar con un incremento de 5 todos los documentos

db.libros.updateMany(
  {},
  {$inc:{precio:5}}
)

--Actualizar con multiplicacion de 2 todos los documentos donde la cantidad que sean mayores a 20

db.libros.updateMany({cantidad:{$gt:20}},{$mul:{cantidad:20}})

--Actualizar todos los documentos donde el precio sea mayor a 20 y 
--se multiplique por 2 la cantidad y el precio

3.Reemplazar Documentos

db1> db.libros.replaceOne({_id:2},{titulo:"De la tierra a la Luna",})


# Borrar Documentos 

deleteOne -> Elimnar un solo documento
deleteMany -> Elimina Multiples documentos

Eliminar el documento con id 2

db.libros.deleteOne({_id:2})

Eliminar los documentos donde la cantidad sea mayor o igual a 150

db.libros.deleteMany({cantidad:{$gte:150}})

# Expresiones Regulares

1. Buscar los libros que contengan la letra t

db.libros.find({titulo:/t/})

2. Buscar los libros que en el titulo contengan la palabra JSON

db.libros.find({titulo:/JSON/})

3. Buscar todos los documentos que en el titulo termine en tos

db.libros.find({titulo:/$tos/})

4. Todos los documentos que en el titulo comiencen con J

db.libros.find({titulo:/^J/})

# Operador $regex 

[Operador Regex](https://www.mongodb.com/docs/manual/reference/operator/query/regex/)

--Seleccionar los libros que contengan la palabra para en titulo
db.libro.find({titulo:{$regex:"para"}})

db.libros.find({titulo:{$regex:"JSON"}})

db.libros.find({titulo:{$regex:/JSON/}})

- Distinguir entre mayusculas y minusculas


db.libros.find({titulo:{$regex:/json/}}) -> No distingue entre mayusculas y minusculas

db.libros.find({titulo:{$regex:/json/, $options:"i"}})

db.libros.find({titulo:{$regex:/j/i}})

-- Seleccionar todos los libros que comiencen con j o J

db.libros.find({titulo:{$regex:/^j/i}}) 

-- Seleccionar todos los libros que terminen es

db.libros.find({titulo:{$regex:/es$/i}})

db.libros.find({titulo:{$regex:"es", $options:"i"}})

# Metodo sort (Ordernar Documentos)

1. Ordenar los libros de manera ascendente por el precio

db.libros.find({},{titulo:1,precio:1,_id:0}).sort({precio:1})

2. Ordenar los libros de manera descendete por el precio

db.libros.find({},{titulo:1,precio:1,_id:0}).sort({precio:-1})

3. Ordenar los libros de manera ascendente por la editorial y de manera 
descendente por el precio mostrando el titulo, el precio y la editorial

db.libros.find({},{titulo:1,precio:1,_id:0,editorial:1}).sort({editorial:1,precio:-1})

# Otros metodos skip, limit, size

db.libros.find({},{titulo:1, precio:1, _id:0, editorial:1}).size()

db.libro.find({titulo:{$regex:/Java/i}}).size()

-- Buscar todos los libros pero mostrando los dos primeros

db.libros.find({},{titulo:1,editorial:1,_id:0,precio:1}).limit(2)

-- Mostrar los 3 ultimos libros 

 db.libros.find({},{titulo:1,editorial:1,_id:0,precio:1}).sort({precio:-1}).limit(3)

-- Seleccionar todos los libros ordenados los titulos de forma descendete, 
saltando los dos primeros y el tamano

db.libros.find({}).sort({titulo:-1}).skip(2).size()

# Como borrar colecciones y base de Datos 

use db5

db.createCollection('ejemplo')

 db.ejemplo.insertOne({ nombre: "Chapuin"})

 db.ejemplo.drop()

 db.dropDatabase()