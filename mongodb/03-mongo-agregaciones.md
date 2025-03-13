# Agregaciones en MongoDB (Framework)

## Metodos para realizar agregaciones simples
- distinct(): Devuelve vlaores no duplicados
- countDocument(): Cuanta los documentos en una coleccion
- estimateDocumentCount(): Cuanta de manera aproximada durante un periodo de tiempo

## Una Aggregation pipeline consta de una o mas etapas o (stage) que prosesan documentos

1. Cada etapa realiza una operacion en los documentos de entrada. Por ejemplo, una fase pude filtrar documentos,agrupar documentos y calcular valores.

2. Los documentos que se generan en una fase pasan a la siguente fase.

3. Puede devover resultados para grupos de documentos como totales, maximo,minimo,etc.

### Se utiliza la clausula "aggreagte"

- Existen una serie de operadores que se pueden utilizar para realizar operaciones. Se tienen distentos tipos de estapa, comparacion, booleanos, aritmeticos, de cadena, etc.

## Metodos simples: countDocument() y distinct()

1. contar los documentos de la colleccion
```json
db.libros.countDocuments()
```
2. Contar los docuemtos de la editorial Terra
```json
db.libros.countDocuments({editorial:{$eq:"Terra"}})
db.libros.countDocuments({editorial:"Terra"})
```
3. Mostrar todos los libros mostrando solamente la editorial.
```json
db.libros.find({},{_id:0,editorial:1})
```
4. Mostrar todos las distintas editoriales.
```json
db.libros.distinct("editorial")
```
[Documentacion de agregaciones](https://www.mongodb.com/docs/manual/aggregation/)

## $match. Un pipeline basica
## Tiene una funcion de tapa
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
        $match:{editorial:"Terra"}
    },
    {
        $project:{
            _id:0,
            titulo:1,
            precio:1,
            NombreAditorial:"$editorial",
            editorial:1
        }
    }
]
)
```
- Generado por mongo compas
```json
[
  {
    $match:
      /**
       * query: The query in MQL.
       */
      {
        editorial: "Terra"
      }
  },
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
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
# Crear campo calculado sencillo con #project
```json
[
  {
    $match:
      /**
       * query: The query in MQL.
       */
      {
        editorial: "Terra"
      }
  },
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
      {
        _id: 0,
        titulo: 1,
        precio: 1,
        cantidad: 1,
        "Nombre ditorial": "$editorial",
        "Total de ganancias": {
          $multiply: ["$precio", "$cantidad"]
        }
      }
  }
]
```
# sort . Ordenaciones
```json
[
  {
    $match:
      /**
       * query: The query in MQL.
       */
      {
        editorial: "Terra"
      }
  },
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
      {
        _id: 0,
        titulo: 1,
        precio: 1,
        cantidad: 1,
        "Nombre ditorial": "$editorial",
        "Total de ganancias": {
          $multiply: ["$precio", "$cantidad"]
        }
      }
  },
  {
    $sort:
      /**
       * Provide any number of field/order pairs.
       */
      {
        precio: -1
      }
  }
]
```
## $group. Agrupaciones
[Agrupaciones](https://www.mongodb.com/docs/manual/reference/operator/aggregation/group/)

-- Cuantos documentos hay por cada una de las editoriales
```json
[
  {
    $group:
      {
        _id: "$editorial",
        "Numero documentos": {
          $count: {}
        }
      }
  }
]
```
-- cuantos documentos hay por cada una de las editoriales por numero de documentos de manera decendente
```json
[
  {
    $group:
      {
        _id: "$editorial",
        "Numero documentos": {
          $count: {}
        }
      }
  },
  {
    $sort:
      {
        "Numero documentos": -1
      }
  }
]
```
-Agrupar por tipo de propiedad mostrando el numero de propiedades y el promedio de sus precios
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
  }
]
```
# Operador $set y $unset

- Operador $set
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
        Media_total: {
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
        Media_total: {
          $trunc: "$Media"
        }
      }
  },
  {
    $unset:
      ["Media", "Media_total"]
  }
]
```

- Creandoo nuevas colecciones utilizando el operador $out
- Nota: debe ser el ultimi de la agragación
- Ejemplos con operadores de comparacion y logicos
```json
[
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
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
- $cond -> Devuelve valores según una condición (es parecido a un operador ternario de un lenguje de programación)
```json
[
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
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
            "NO"
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
        barato: {
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
db.createView("ganancias_lobros","libros",
  [
  {
    $match:
      /**
       * query: The query in MQL.
       */
      {
        editorial: "Biblio"
      }
  },
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
      {
        _id: 0,
        titulo: 1,
        precio: 1,
        cantidad: 1,
        "Nombre editorial": "$editorial",
        "Total de ganancias": {
          $multiply: ["$precio", "$cantidad"]
        }
      }
  },
  {
    $sort:
      /**
       * Provide any number of field/order pairs.
       */
      {
        "Total de ganancias": -1
      }
  }
]
)
```
```json
db.ganancias_Libros.find(
{
  'Total ganacias':{$lte:240}},
   {titulo:1, 'Total ganancias':1}).sort({titulo:-1}
   )
```