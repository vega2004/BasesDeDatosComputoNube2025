# Agregaciones en MongoDB (Framework)

## Metodos para para reailizar agregaciones simples

-- distinct(): Me devuelve valores no duplicados
-- CountDocument(): Cuenta los documentos en una coleccion
-- estimatedDocumentCount(): Cuenta manera aproximada durante un periodo de tiempo

## Una Agreggation pipeline consta de una o mas etapas (stage) que procesan documentos 

1. Cada etapa realiza una operacion en los documentos de entrada, por ejemplo, una fase puede filtrar documentos, agrupar documentos, y calcular valores.

2. Los documentos que se generan en una fase, pasan a la siguiente fase.

3. Puede devolver resultados para grupos de documentos como totales, maximo, minimo , etc.

## Se utiliza la clausula "agreggate"

-- Existen una serie de operadores que se pueden utilzar para realizar operaciones. Se tienen distintos tipos: etapa, comparacion,booleanos, aritmeticos, de cadena, etc.

## Metodos simples : countDocuments, y distinct()

--Contar los documentos de la coleccion

2. Contar los documentos de la editorial Terra

db1> db.libros.countDocuments({editorial: {$eq:'Biblio'}})

3. Seleccionar o mostrar todos los libros mostrando solamente la editorial

db1> db.libros.distinct({},{_id:0, editorial:1})


4. Mostrar todoso los libros distintas editoriales

db1> db.libros.distinct("editorial")

[Documentacion de Agregaciones](https://www.mongodb.com/docs/manual/aggregation/)

## Match $match. Un pipeline basica

## Tiene una funcion de tapa

db.libros.agreggate([{$match:editorial:"Terra"}])

## $project Incluir y renombrar campos

db.libros.aggregate([
    {
        $match: { editorial: "Terra" }
    },
    {
        $project: {
            _id: 0,
            titulo: 1,
            precio: 1,
            NombreEditorial: "$editorial"
        }
    }
])

## $group.Agrupaciones

Cuantos documentos hay por cada una de las editoriales

_id: "$editorial",
"Numeero Documentos":{
    $count: {}
}

Cuantos documentos hay por cada una de las editoriales por numero de documentos de manera descendente



-- Utilizando Mongo Atlas Base de Datos sample_airnb
-- Agrupar por tipo de propiedad, mostrando el numero de propiedades y el promedio de sus precios 


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
  }
]

--Operadores $set y $unset

[
  {
    $group: {
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
        Media_total: {
          $trunc: "$Media"
        }
      }
  }
]


Operador $unset
[
  {
    $group: {
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
    $set: {
      Media_total: {
        $trunc: "$Media"
      }
    }
  },
  {
    $unset:
    
      ["Media","Media_total"]
  }
]


-- Creando nuevas colecciones utilizando el Operador $out
-- Debe ser el ultimo en la agregacion


Ejemplo con operadores de comparacion

{
  _id:0,
  price:1,
  name:1,
  
  caro:{
    $gt:["$price",300]
  },
  medio:{
    $and:[
      {$gte:["$price",100]},
      {$lte:["price",300]}
    ]
  },
  baratito:{
    $lt:["price",100]
  }
}


--- $cond. Devuelve valores segun una condicion (es parecido a un operador ternario de un lenguaje de programacion)

{
  _id: 0,
  price: 1,
  name: 1,

  caro: {
    $cond: [
      { $gt: ["$price", 300] },
      "si",
      "no"
    ]
  },

  medio: {
    $cond: [
      { 
        $and: [
          { $gte: ["$price", 100] },
          { $lte: ["$price", 300] }
        ] 
      },
      "si",
      "no"
    ]
  },

  baratito: {
    $cond: [
      { $lt: ["$price", 100] },
      "si",
      "no"
    ]
  }
}

--Views
db.createView("ganancias_libros",
"libros",
[
  {
    $match: {
      editorial: "Biblio"
    }
  },
  {
    $project: {
      _id: 0,
      titulo: 1,
      precio: 1,
      cantidad: 1,
      "Nombre de editorial": "editorial",
      "Total de ganancias": {
        $multiply: ["$precio", "$cantidad"]
      }
    }
  },
  {
    $sort: {
      "Total de ganancias": -1
    }
  }
])

db.ganancias_libros.find(
  { "Total de Ganancias": { $lte: 240 } },
  {
    titulo: 1,
    "Total de Ganancias": 1,
    _id: 0
  }
).sort({ titulo: -1 });
