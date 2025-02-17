
# Consultas

1. Cargar el archivo empleados.json

-En local:
mongoimport --db curso --collection empleados --file empleados.json

-Doker:
mongoimport --db curso --collection empleados --file empleados.json --port 27018

2. Utilizar la base de datos curso
``` json
use curso
```
3. Buscar todos los empleados que trabajen en Google
```json
db.empleados.find(
{
    empresa:{$eq: "Google"}
}
)
```
4. Empleados que vivan en Peru
```json
db.empleados.find(
{
    pais:{$eq: "Peru"}
}
)
```
5. Empleados que ganen mas de 8,000 dollares
```json
db.empleados.find(
{
    salario:{$gt: 8000}
}
)
```
6. Empleados con ventas inferiores a 10,000
```json
db.empleados.find(
{
    ventas:{$lt: 10000}
}
)
```
7. Realizar la consulta anterior pero devolviendo una sola fila
```json
db.empleados.findOne(
{
    ventas:{$lt: 10000}
}
)
```
8. Empleados que trabajan en Google o en Yahoo con el operador $in
```json
db.empleados.find(
{
    empresa:{$in:["Google","Yahoo"]}
}
)
```
9. Empledos de Amazon que ganen mas de 8,000 dolares
```json
db.empleados.find(
{
    $and:[
        {empresa:{$eq:"Amazon"}},
        {salario:{$gt:8000}}
    ]
}
)
```
10. Empleados que trabajen en Yahoo que ganen mas de 6,000 o empleados que trabajen en Google que tengan ventas inferiores a 20,000
```json
db.empleados.find(
{
    $or:[
        {$and:[{empresa:"Yahoo"},{salario:{$gt:6000}}]},
        {$and:[{empresa:"Google"},{ventas:{$lt:20000}}]}
    ]
}
)
```
11. Visualizar el nombre, apellidos y pais de cada empleado
```json
db.empleados.find({},{nombre:1,apellidos:1,pais:1,_id:0})
```
12.Empleados que trabajen en Google o en Yahoo con el operador $or
```json
db.empleados.find({
    $or:[{empresa:"Amazon"},{ empresa:{$eq:"Yahoo"}}]
})
```