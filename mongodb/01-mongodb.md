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

## Practica
1. Bases de datos, colecciones e inserts
    .Nos conectamos con mongosh al MongoDB
    .Crear una base de datos denominada curso

    use curso

    verificar que la base de datos no existe
    show dbs

    .Crear una coleccion denominada "facturas"
    con el comando db.createCollection("facturas")

    y comprobar que aparece tanto la coleccion como la base de datos

    show collections

    show dbs
