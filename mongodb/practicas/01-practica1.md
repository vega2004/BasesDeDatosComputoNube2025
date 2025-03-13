# Practica 1: Base de datos, colecciones e inserts

1.Conectarnos con mongosh a Mongodb
2.Crear una base de datos llamado curso
3.Comprobar que la base de datos no existe
4.Crear una coleccion que se llame facturas y mostrarla

``` json
db.createCollection("facturas")
show collections
```

5.Insertar un documento con los siguientes datos

| cod_facturas | Cliente         | Total |
|-------------|----------------|------|
| 010         | Frutas Ramirez  | 223  |

| cod_facturas | Cliente         | Total |
|-------------|----------------|------|
| 020        | Frutas Ferreteria Juan | 140  |


``` json
db.facturas.insertOne( {
cod_factura: 10,
      cliente: "Frutas Ramirez",
       total: 223 })
```

``` json
db.facturas.insertOne( {
cod_factura: 20,
      cliente: "Ferreteria Juan",
       total: 140 })
```

6.Crear una nueva coleccion pero usando directamente el insertOne:
insertar un documento en la coleccion productos con los siguientes datos:

|Codigo | valor|
| cod_facturas | Cliente         | Total |
|-------------|----------------|------|
| 020        | Frutas Ferreteria Juan | 140  |

``` json
db.productos.insertOne({
     cod_producto: 1,
      nombre: "tornillo x 1",
       precio: 2,
        unidades: 1500 })
```

7.Crear un nuevo documento de producto que contenga un array.
Con los siquientes datos.
``` json
db.productos.insertOne({ 
    cod_producto: 2,
     nombre: "martillo",
      precio: 20,
       unidades: 50,
        fabricantes:["fab1","fab2","fab3","fab4"] })
```
8.Borrar la coleccion facturas y comprobar que se borro

curso> db.facturas.drop()
true
curso> show collections
productos
curso>

9.Insertar un documento en una coleccion denominada fabricantes para probar los subdocumentos y
la clave _id personalizada

curso> db.fabricantes.insertOne({ _id: 1, nombre: "fab1", localidad: { ciudad: "buenos aires", pais: "argentina", calle: "Calle pez 27", cod: 2090 } })

10.Realizar una insercion de varios documentos en la coleccion productos



