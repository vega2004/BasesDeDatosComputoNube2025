
# Práctica 1: Base de Datos, Colecciones e Inserts

## 1. Conexión con `mongosh` a MongoDB

1.1. Conectarnos a la base de datos MongoDB utilizando `mongosh`.

## 2. Crear una Base de Datos llamada `curso`

2.1. Comprobamos que la base de datos no exista previamente.

## 3. Crear una Colección llamada `facturas` y mostrarla

```json
db.createCollection('facturas')
show collections
```

## 4. Insertar un Documento en la Colección `facturas`

A continuación se insertan los siguientes documentos:

| Código    | Valor         |
|-----------|---------------|
| Cod_Factura | 10            |
| Cliente    | Frutas Ramirez |
| Total      | 223           |

| Código    | Valor          |
|-----------|----------------|
| Cod_Factura | 20             |
| Cliente    | Ferretería Juan |
| Total      | 140            |

```json 
db.facturas.insertOne({ cod_factura: 10, cliente: 'Frutas Ramirez', total: 223 })
db.facturas.insertOne({ cod_factura: 20, cliente: 'Ferretería Juan', total: 140 })
```

## 5. Crear una Nueva Colección con `insertOne`

Utilizando `insertOne`, creamos una colección llamada `productos` e insertamos un documento con los siguientes datos:

| Código         | Valor          |
|----------------|----------------|
| Cod_producto   | 1              |
| Nombre         | Tornillo x 1"  |
| Precio         | 2              |

```json
db.productos.insertOne({ cod_producto: 1, nombre: "Tornillo x 1", precio: 2, unidades: 1500 })
```

## 6. Insertar un Documento en `productos` con un Array

Insertamos un nuevo documento que contiene un array en la colección `productos`:

| Código         | Valor          |
|----------------|----------------|
| Cod_producto   | 2              |
| Nombre         | Martillo       |
| Precio         | 20             |
| Unidades       | 50             |
| Fabricantes    | fab1, fab2, fab3, fab4 |

```json
db.productos.insertOne({
  cod_producto: 2,
  nombre: 'Martillo',
  precio: 20,
  unidades: 50,
  fabricantes: ['fab1', 'fab2', 'fab3', 'fab4']
})
```

## 7. Borrar la Colección `Facturas` y Comprobar su Eliminación

```json
db.facturas.drop()
show collections
```

## 8. Insertar un Documento en la Colección `fabricantes`

En este paso, insertamos un subdocumento y utilizamos una clave `_id` personalizada en la colección `fabricantes`:

| Código    | Valor       |
|-----------|-------------|
| id        | 1           |
| Nombre    | fab1        |
| Localidad | ciudad: Buenos Aires, país: Argentina, calle: Calle Pez 27, cod_postal: 2900 |

```json
db.fabricantes.insertOne({
  _id: 1,
  nombre: 'fab1',
  localidad: {
    ciudad: 'Buenos Aires',
    pais: 'Argentina',
    calle: 'Calle Pez 27',
    cod_postal: 2900
  }
})
```

## 9. Insertar Varios Documentos en la Colección `productos`

Finalmente, insertamos múltiples documentos en la colección `productos` con los siguientes datos:

| Código         | Valor          |
|----------------|----------------|
| Cod_producto   | 3              |
| Nombre         | Alicates       |
| Precio         | 10             |
| Unidades       | 25             |
| Fabricantes    | fab1, fab2, fab5|

| Código         | Valor          |
|----------------|----------------|
| Cod_producto   | 4              |
| Nombre         | Arandela       |
| Precio         | 1              |
| Unidades       | 500            |
| Fabricantes    | fab2, fab3, fab4|



```json
db.productos.insertMany([
  { cod_producto: 3, nombre: 'Alicates', precio: 10, unidades: 25, fabricantes: ['fab1', 'fab2', 'fab5'] },
  { cod_producto: 4, nombre: 'Arandela', precio: 1, unidades: 500, fabricantes: ['fab2', 'fab3', 'fab4'] }
])
```
