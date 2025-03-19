
# Agregaciones en MongoDB (Framework)

## Métodos para realizar agregaciones simples
- distinct(): Devuelve valores no duplicados
- countDocuments(): Cuenta los documentos en una colección 
- estimatedDocumentCount(): Cuenta de manera aproximada durante un periodo de tiempo

## Una Aggregation pipeline consta de una o más etapas (stage) que procesan documentos

1. Cada etapa realiza una operación en los documentos de entrada. Por ejemplo, una fase puede filtrar documentos, agrupar documentos y calcular valores.
2. Los documentos que se generan en una fase pasan a la siguiente fase.
3. Puede devolver resultados para grupos de documentos como totales, máximo, mínimo, etc.

### Se utiliza la cláusula "aggregate"

- Existen una serie de operadores que se pueden utilizar para realizar operaciones. Se tienen distintos tipos: etapa, de comparación, booleanos, aritméticos, de cadena, etc.

## Métodos Simples: countDocuments() y distinct()

1. Contar los documentos de la colección libros
```json
db.libros.countDocuments()
```

2. Contar los documentos de la editorial Terra
```json
db.libros.countDocuments({editorial: {$eq:'Terra'}})
db.libros.countDocuments({editorial:'Terra'})
```

3. Mostrar todos los libros mostrando solamente la editorial

4. Mostrar todas las distintas editoriales
```json
db.libros.distinct('editorial')
```
[Documentación de Agregaciones](https://www.mongodb.com/docs/manual/aggregation/)

## $match. Una pipeline básica
## Tienen funciones de etapa
```json
db.libros.aggregate(
    [
        {
            $match:{editorial:"Terra"}
        }
    ]
)
```

## $project. Incluir y renombrar campos
```json
db.libros.aggregate(
   [
    {
        $match:{editorial:'Terra'}
    },
    {
        $project:{
            _id:0,
            titulo:1, 
            precio:1, 
            NombreEditorial:"$editorial",
            editorial:1
        }
    }
   ]  
)
```

- Generado por MongoCompass
```json
[
  {
    $match:
      {
        editorial: "Terra"
      }
  },
  {
    $project:
      {
        _id: 0,
        precio: 1,
        cantidad: 1,
        NombreEditorial: "$editorial",
        editorial: 1
      }
  }
]
```

## Crear un campo calculado con #project
```json
[
  {
    $match:
      {
        editorial: "Terra"
      }
  },
  {
    $project:
      {
        _id: 0,
        titulo: 1,
        precio: 1,
        cantidad: 1,
        "Nombre Editorial": "$editorial",
        "Total de Ganancias": {
          $multiply: ["$precio", "$cantidad"]
        }
      }
  }
]
```

## sort. Ordenaciones
```json
[
  {
    $match:
      {
        editorial: "Terra"
      }
  },
  {
    $project:
      {
        _id: 0,
        titulo: 1,
        precio: 1,
        cantidad: 1,
        "Nombre Editorial": "$editorial",
        "Total de Ganancias": {
          $multiply: ["$precio", "$cantidad"]
        }
      }
  },
  {
    $sort:
      {
        precio: 1
      }
  }
]
```

## $group. Agrupaciones

[Agrupaciones](https://www.mongodb.com/docs/manual/reference/operator/aggregation/group/)

- Cuántos documentos hay por cada una de las editoriales
```json
[
  {
    $group:
      {
        _id: "$editorial",
        "Numero Documentos": {
          $count: {}
        }
      }
  }
]
```

- Cuántos documentos hay por cada una de las editoriales por número de documentos de manera descendente
```json
[
  {
    $group:
      {
        _id: "$editorial",
        "Numero Documentos": {
          $count: {}
        }
      }
  },
  {
    $sort:
      {
        "Numero Documentos": -1
      }
  }
]
```

- Utilizando Mongo Atlas Base de Datos sample_airbnb
- Agrupar por tipo de propiedad, mostrando el número de propiedades y el promedio de sus precios
```json
{
  _id: "$property_type",
  Numero: {
    $count: {}
  },
  Media:
  {
    $avg:"$price"
  }
}
```

- Operadores $set 
```json
[
  {
    $group:
      {
        _id: "$property_type",
        Numero: {
          $count: {}
        },
        Media: {
          $avg: "$price"
        }
      }
  },
  {
    $set:
      {
        Media_Total: {
          $trunc: "$Media"
        }
      }
  }
]
```

- Operador $unset
```json
[
  {
    $group:
      {
        _id: "$property_type",
        Numero: {
          $count: {}
        },
        Media: {
          $avg: "$price"
        }
      }
  },
  {
    $set:
      {
        Media_Total: {
          $trunc: "$Media"
        }
      }
  },
  {
    $unset:
      ["Media", "Media_Total"]
  }
]
```

- Creando nuevas colecciones utilizando el operador $out
```json
[
  {
    $group:
      {
        _id: "$property_type",
        Numero: {
          $count: {}
        },
        Media: {
          $avg: "$price"
        }
      }
  },
  {
    $set:
      {
        Media_Total: {
          $trunc: "$Media"
        }
      }
  },
  {
    $unset:
      ["Media"]
  },
  {
    $out:
      {
        db: "sample_airbnb",
        coll: "media_propiedades"
      }
  }
]
```

- Ejemplos con operadores de comparación y lógicos
```json
[
  {
    $project:
      {
        _id: 0,
        price: 1,
        name: 1,
        room_type: 1,
        caro: {
          $gt: ["$price", 300]
        },
        medio: {
          $and: [
            {
              $gte: ["$price", 100]
            },
            {
              $lte: ["$price", 300]
            }
          ]
        },
        baratito: {
          $lt: ["$price", 100]
        }
      }
  }
]
```

- $cond - Devuelve valores según una condición (es parecido a un operador ternario de un lenguaje de programación)
```json
[
  {
    $project:
      {
        _id: 0,
        price: 1,
        name: 1,
        room_type: 1,
        caro: {
          $cond: [
            {
              $gt: ["$price", 300]
            },
            "Si",
            "No"
          ]
        },
        medio: {
          $cond: [
            {
              $and: [
                {
                  $gte: ["$price", 100]
                },
                {
                  $lte: ["$price", 300]
                }
              ]
            },
            "Si",
            "No"
          ]
        },
        baratito: {
          $cond: [
            {
              $lt: ["$price", 100]
            },
            "Si",
            "No"
          ]
        }
      }
  }
]
```

- Views
```json
db.createView("ganancias_libros", 
"libros",
[
  {
    $match:
      {
        editorial: "Biblio"
      }
  },
  {
    $project:
      {
        _id: 0,
        titulo: 1,
        precio: 1,
        cantidad: 1,
        "Nombre de Editorial": "$editorial",
        "Total de Ganancias": {
          $multiply: ["$precio", "$cantidad"]
        }
      }
  },
  {
    $sort:
      {
        "Total de Ganancias": -1
      }
  }
])

db["ganancias_libros"].find(
  {"Total de Ganancias":{$lte:240}}
  ,{titulo:1, 'Total de Ganancias':1}).sort({titulo:-1})
```
