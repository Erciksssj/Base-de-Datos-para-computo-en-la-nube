# Practica1: Base de Datos, colleciones e inserts

1. Conectarnos con mongosh a MongoDB
2. Crear una base de datos llamada curso
3. Comprobar que la base de datos no exista
4. Crear una colleccion que se llame facturas y mostrarla
```json
db.createCollection('facturas')
show collections
```
5. Insertar un documento con los siguientes datos

| Codigo   | Valor   |
|-------------|-------------|
| Cod_Factura | 10 |
| Ciente | Frutas Ramirez |
| Total | 223 |

| Codigo   | Valor   |
|-------------|-------------|
| Cod_Factura | 20 |
| Ciente | Ferreteria Juan |
| Total | 140 |

```json
db.facturas.insertOne(
{
  Cod_factura:10,
  Cliente:'Frutas Ramirez',
  total:223
}
)
```
```json
db.facturas.insertOne(
{
  Cod_factura:20,
  Cliente:'Ferreteria Juan',
  total:140
}
)
```
6. Crear una nueva colleccion pero usando directamente el insertOne
insertar un documento en la colleccion productos con los siguientes datos:

| Codigo   | Valor   |
|-------------|-------------|
| Cod_Producto | 1 |
| Nombre | Tornillo x 1" |
| Precio | 2 |
| Unidades | 1500 |
```json
db.productos.insertOne(
{
  Cod_Producto:1,
  Nombre:'Tornillo x 1',
  Precio:2,
  unidadades:1500
}
)
```
7. Crear un nuevo documento de producto que contenga un array. Con los siguientes datos:

| Codigo   | Valor   |
|-------------|-------------|
| Cod_Producto | 2 |
| Nombre | Martillo |
| Precio | 20 |
| Unidades | 50 |
| Fabricantes | fab1, fab2, fab3, fab4 |
```json
db.productos.insertOne(
{
Cod_Producto:2,
Nombre:'Martillo',
Precio:20,
Unidades:50,
Fabricantes:[
'fab1','fab2','fab3','fab4'
]
}
)
```
8. Borrar la colleccion facturas y comprobar que se borro
```json
db.facturas.drop()
show collections
```
9. Insertar un documento en una coleccion denominada **Fabricantes** para probar los subdocumentos y la clave _id personalizada

| Codigo   | Valor   |
|-------------|-------------|
| id | 1 |
| Nombre | fab1 |
| Lcalidad | ciudad: buenos aires, pais:argentina, calle:Calle pez 27, cod_postal:2900|

```json
db.fabricantes.insertOne(
{
    _id:1,
    Nombre:'fab1',
    Localidad:
    {
        ciudad: 'buenos aires',
        pais:'argentina',
        calle:'Calle pez 27',
        cod_postal:2900
    }
}
)
```
10. Realizar una insercion de varios documentos en la colleccion productos

| Codigo   | Valor   |
|-------------|-------------|
| Cod_Producto | 3 |
| Nombre | Alicates |
| Precio | 10 |
| Unidades | 25 |
| Fabricantes | fab1, fab2, fab5 |

| Codigo   | Valor   |
|-------------|-------------|
| Cod_Producto | 4 |
| Nombre | Arandelas |
| Precio | 1 |
| Unidades | 500 |
| Fabricantes | fab2, fab3, fab4 |

```json
db.Productos.insertMany(
[
    {
        cod_producto: 3,
        nombre: "Alicates",
        precio: 10,
        unidades: 25,
        fabricantes: ["fab1", "fab2", "fab5"]
    },
    {
        cod_producto: 4,
        nombre: "Arandelas",
        precio: 1,
        unidades: 500,
        fabricantes: ["fab2", "fab3", "fab4"]
    }
])
```