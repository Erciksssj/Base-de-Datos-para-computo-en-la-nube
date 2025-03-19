# Modelo de datos en MongoDB

- Modelos   
    - Embiebidos
    - Referenciados
- Modelos Embebidos

**Uno a Uno**

```json
db.departamentos.insertMany(
[{
    _id:1,
    nombre:"Tecnologias de informacion",
    responsable:{
        nombre:"Monico",
        apellidos:"Martinez Perez"
    },
    descripcion:"ejemplo de departamento"
    },
    {
    _id:2,
    nombre:"Contabilidad",
    responsable:{
        nombre:"Raul",
        apellidos:"Lopez Hernadez"
    },
    descripcion:"ejemplo de departamento"
    }
]
)
```
- Modelos Referenciados

**Uno a Uno**
```json
db.localidades.insertMany(
[
    {
        _id:"BA",
        ciudad:"Buenos Aires",
        pais:"Argentina",
        poblacion:"16 millones",
        turismo:["edificios","tango","gastronomia","museo","perques"],
        direccion:"Avenida Tortolos",
        cod_departamento:1
    },
    {
        _id:"SA",
        ciudad:"Santiago",
        pais:"Chile",
        poblacion:"20 millones",
        turismo:["iglesias","vino","gastronomia","museos"],
        direccion:"Calle soy la vaca del corral",
        cod_departamento:2
    }
]
)
```
```json
db.departamentos.aggregate(
[
    {
        $lookup:
        {
            from:"localidades",
            localField:"_id",
            foreignField:"cod_departamento",
            as:"localidades"
        }
    }
])
```
```json
db.categoria.insertMany(
[
    {
        _id:1,
        nombre:"categoria1"
    },
    {
         _id:2,
        nombre:"categoria2"
    }
]
)
```
```json
db.producto.insertMany(
[
    {
        _id:1,
        nombre:"maiz",
        precio:10,
        existencia:10,
        categoria:1
    },
    {
        _id:2,
        nombre:"frijol",
        precio:20,
        existencia:20,
        categoria:2
    }
]
)
```
```json
db.categoria.aggregate(
[
    {
        $lookup:
        {
            from:"producto",
            localField:"_id",
            foreignField:"categoria",
            as:"categoria1"
        }
    },
    {
    $project:
      {
        _id: 0,
        nombre: 1,
        "categoria.precio": 1
      }
    }
])