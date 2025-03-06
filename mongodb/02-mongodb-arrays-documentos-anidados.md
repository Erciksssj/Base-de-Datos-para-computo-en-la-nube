# Busquedad de Arrays y ducumentos anidados
1. Buscar dentro de un documento anidado

```json
db.Ciudades.find({ alcalde: {nombre:'Crystal', apellidos: 'Long', edad:76}})

db.Ciudades.find({'alcalde.nombre': 'Crystal'})
```

2. Seleccionar las ciudades donde los alcaldes tengan mas de 70 años
```json
db.Ciudades.find({ 'alcalde.edad':{$gt:70}})
```
3. Realizar la consulta anterior usando una proyeccion
```json
db.Ciudades.find(
{
     'alcalde.edad':{$gt:70}
},
{
    _id:0,nombre:1,'alcalde.edad':1
})
```
4. Seleccionar las ciudades donde el alcalde tenga 76 años o 41
```json
db.ciudades.find({
    $or:[
        {"alcalde.edad":76},
        {"alcalde.edad":41}
        ]
})
```
## Consultas con Arrays

1. Crear una colleccion cazas
```json
db.createCollection("Cazas")
```
2. Cargar los documentos de la coleccion cazas,de la data cazas
## Ejemplos de consultas con Arrays
1. Buscar los documentos que tengan USA como paises
```json
db.cazas.find({paises:"usa"})
```
2. Buscar los documentos donde el elemento del array dimensiones sea mayor a 5
```json
db.cazas.find({dimensiones:{$gt:5}}).count()
```
3. Buscar los documentos donde las dimensiones del avion por ancho que es la posición 1, sea superior a 14
```json
db.cazas.find({
    "dimensiones.1":{$gt:14}
    },
    {
        _id:0,modelo:1,dimensiones:1
    })
```
### Array. Operador $all
1. Buscar los aviones que estan sirviendo en egipto y en israel
```json
db.cazas.find(
    {
        paises:
        {
            $all:["egipto","israel"]
            }
    },{_id:0,modelo:1,paises:1}
)
```
```json
db.cazas.find(
{
    $and:[
        {paises:"egipto"},
        {paises:"israel"}
    ]
}
)
```
2. Seleccionar los cazas que sirven en inglaterra y españa
```json
db.cazas.find(
{
    $and:[
        {paises:"inglaterra"},
        {paises:"españa"}
    ]
})
```
```json
db.cazas.find(
{
    paises:{$all:["inglaterra","españa"]}
})
```
### Arrays Operador $elementMatch -> Permite hacer busquedas dentro de arrys, esta busca la lista de condiciones que estan dentro del operador
1. Buscar que aviones tinen uso dentro de inglaterra
```json
db.cazas.find(
{
        paises:{$elemMatch:{$eq:"inglaterra"}}
    
})
```
2. Buscar los aviones donde las dimensiones sean menores a  20 o mayores a 15
```json
db.cazas.find(
{
        dimensiones:{$elemMatch:{$lt:20,$gt:15}}
    
})
```
### Buscar en arrays de documentos anidados
1. Utilizar la coleccion ciudades
2. Buscar los consejeros de nombre Jeri Flowers de edad 78
```json
db.ciudades.find(
{
    consejeros:{identificador:1,nombre:"Jeri Flowers",edad:78}
})
```
3. Buscar en consejeros donde tengan una edad mayor a 70
```json
db.ciudades.find(
{
    "consejeros.edad":{$gt:70}
})
```
4.Buscar en consejeros en la posision 2que esten entre 70 y 80 años 
```json
db.ciudades.find(
{
    "consejeros.2.edad":{$gt:70,$lt:80}
},
{
    _id:0,"consejeros.nombre":1,"consejeros.edad":1
})
```
5.Buscar los consejeros de la posision 0 que sean mayores a 70
```json
db.ciudades.find(
{
    "consejeros.0.edad":{$gt:70}
},
{
    _id:0,"consejeros.nombre":1,"alcalde.edad":1
})
```
6. Buscar consejeros que sean mayores a 70
```json
db.ciudades.find(
{
    "consejeros.edad":{$gt:70}
},
{
    _id:0,"consejeros.nombre":1,"consejeros.edad":1
})
```