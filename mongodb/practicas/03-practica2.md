# Practica 3. Updates y Deletes

1. Cambiar el salario del empleado Imogene Nola y se le asgina 8000

Curso> db.empleados.updateOne({nombre:"Imogene"},{$set:{salario:8000}})

Curso> db.empleados.find({nombre:"Imogene"})
[
  {
    _id: ObjectId('67c0a29464987ff409f45d53'),
    nombre: 'Imogene',
    apellidos: 'Nolan',
    correo: 'montes@protonmail.com',
    direccion: '348-6070 Libero. Avenue',
    region: 'Bicol Region',
    pais: 'Costa Rica',
    empresa: 'Amazon',
    ventas: 938,
    salario: 8000,
    departamentos: 'Payroll, Public Relations, Customer Service, Advertising'
  }
]

2. Cambiar "Belgium" por "Belgica" en los empleados 
(debe haber dos)

Curso> db.empleados.updateMany({pais:"Belgium"},{$set:{pais:"Belgica"}})

3. Incrementar el salario de todos los empleados de Google en 1000
Curso> db.empleados.updateMany({empresa:"Google"},{$inc:{salario:1000}})
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 6,
  modifiedCount: 6,
  upsertedCount: 0
}

4. Reemplazar el empleado Omar Gentry por el siguiente documento

json
{
"nombre": "Omar",
"apellidos": "Gentry",
"correo": "sin correo",
"direccion": "Sin calle",
"region": "Sin region",
"pais": "Sin pais",
"empresa": "Sin empresa",
"ventas": 0,
"salario": 0,
"departamentos": "Este empleado ha sido anulado"
}

Curso> db.empleados.replaceOne(
...     { nombre: "Omar" },
...     {
...         nombre: "Omar",
...         apellidos: "Gentry",
...         correo: "sin correo",
...         direccion: "Sin calle",
...         region: "Sin region",
...         pais: "Sin pais",
...         empresa: "Sin empresa",
...         ventas: 0,
...         salario: 0,
...         departamentos: "Este empleado ha sido anulado"
...     }
... )
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}

5. Con un find comprobar que el empleado ha sido modificado

Curso> db.empleados.find({nombre:"Omar"})
[
  {
    _id: ObjectId('67c0ab70ba3a71e9714dc7e2'),
    nombre: 'Omar',
    apellidos: 'Gentry',
    correo: 'sin correo',
    direccion: 'Sin calle',
    region: 'Sin region',
    pais: 'Sin pais',
    empresa: 'Sin empresa',
    ventas: 0,
    salario: 0,
    departamentos: 'Este empleado ha sido anulado'
  }
]

6. Borrar todos los empleados que ganen mas de 8500,
Nota: deben ser borrados 3 documentos

Curso> db.empleados.deleteMany({salario:{$gt:8500}})
{ acknowledged: true, deletedCount: 3 }

7. Visualizar con una expresion regular todos los empleados con apellidos que comiencen con "R"

Curso> db.empleados.find({apellidos:{$regex:/^R/}})
[
  {
    _id: ObjectId('67c0ab70ba3a71e9714dc7ea'),
    nombre: 'Anika',
    apellidos: 'Russo',
    correo: 'luctus@protonmail.com',
    direccion: 'P.O. Box 954, 2636 A St.',
    region: 'North Region',
    pais: 'Belgium',
    empresa: 'Borland',
    ventas: 13004,
    salario: 6600,
    departamentos: 'Asset Management, Quality Assurance, Media Relations, Research and Development'
  }
]

8. Buscar todas las regiones que contengan una "v"
Hacerlo con el operador $regex y que no distinga mayusculas y minusculas. Deben salir 2
Curso> db.empleados.find({region:{$regex:/v/i}})
[
  {
    _id: ObjectId('67c0ab70ba3a71e9714dc7e8'),
    nombre: 'Meghan',
    apellidos: 'Adams',
    correo: 'auctor.non@hotmail.edu',
    direccion: 'P.O. Box 170, 219 Dignissim St.',
    region: 'Bolívar',
    pais: 'Italy',
    empresa: 'Chami',
    ventas: 33227,
    salario: 3675,
    departamentos: 'Sales and Marketing, Human Resources, Quality Assurance, Payroll'
  },
  {
    _id: ObjectId('67c0ab70ba3a71e9714dc7ec'),
    nombre: 'Hadassah',
    apellidos: 'Whitney',
    correo: 'eget.ipsum@outlook.com',
    direccion: '4588 Donec St.',
    region: 'Central Visayas',
    pais: 'Russian Federation',
    empresa: 'Apple',
    ventas: 8183,
    salario: 5504,
    departamentos: 'Accounting, Asset Management, Human Resources, Finances'
  }
]

9. Visualizar los apellidos de los empleados ordenador por el propio apellido.

Curso> db.empleados.find({},{_id:0,nombre:0,correo:0,direccion:0,region:0,pais:0,empresa:0,ventas:0,salario:0,departamentos:0}).sort({apellidos:-1})
[
  { apellidos: 'Whitney' },
  { apellidos: 'Thornton' },
  { apellidos: 'Sykes' },
  { apellidos: 'Saunders' },
  { apellidos: 'Russo' },
  { apellidos: 'Phelps' },
  { apellidos: 'Nolan' },
  { apellidos: 'Mejia' },
  { apellidos: 'Lyons' },
  { apellidos: 'Juarez' },
  { apellidos: 'Hyde' },
  { apellidos: 'Herrera' },
  { apellidos: 'Gentry' },
  { apellidos: 'Cummings' },
  { apellidos: 'Castillo' },
  { apellidos: 'Buck' },
  { apellidos: 'Adams' }
]

10. Indicar el numero de empleados que trabajan en Google
Curso> db.empleados.find({empresa:"Google"}).size()
5

11. Borrar la coleccion empleados y la base de datos

Curso> db.empleados.drop()
true

Curso> db.dropDatabase()
{ ok: 1, dropped: 'Curso' }