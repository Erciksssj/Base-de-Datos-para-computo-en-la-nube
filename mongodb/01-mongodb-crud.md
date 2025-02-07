
# Crud y consultas en MongoDB

## Crear una base de datos
Solo se crea si contiene una base de datoscolleccion

**use db1**

##Como crear una colleccion
use db1 
db.createCollection('alumnos')

##Mostrar las collecciones
show collections

##Insertar un documento
```json
db.alumnos.insertOne(
{
  nombre:'Soyla',
  apellido1:'Vaca',
  edad:32,
  ciudad:'San Miguel de las Priedras'
}
)

##Insercion de un documento mas complejo con array 
```json
db.alumnos.insertOne(
{
nombre:'Erick',
apellido:'Trejo',
apellido2:'Gerrero',
edad:15,
aficiones:[
'fut','juegos','motos'
]
}
)

## Insercion de documentos mas complejos con documentos anidados y ID

```json
db.alumnos.insertOne(
{
    nombre:'Jose Luis',
    apellido1:'Herrera',
    apellido2:'Gallardo',
    edad:41,
    estudios:[
        'INg en Sistemas Computacionales',
        'Maestria en Tecnologias de la Informacion'
    ],
experiencia:{
    lenguaje:'SQL',
    abd:'SQL Server',
    experiencia:14
}
}
)

## 
db.alumnos.insertOne(
{
    _id:3,
    nombre:'Sergio',
    apellidos:'Ramos',
    equipo:'Monterrey',
    aficiones:['Dinero','Hombres','Fiesta'],
    taleentos:{
        fultol:true,
        bananse:false
    }
}
)

## Insertar Multiples Documentos
db.alumnos.insertMany(
[
    {
        _id:12,
        nombre:'Roberto',
        apellido:'Gomez',
        edad:'23',
        descrpcion:'Es un comediante bueno'
    },
    {
        nombre:'Luis',
        apellido:'Suarez',
        edad:43,
        habilidades:[
            'Correr','dormir','Morder'
        ],
        direccioned{
            calle:'Del infierno',
            numero:666,
        },
        esposas:[
            {
                nombre:'Marisol',
                edad:20,
                pension:350,
                hijos:['Erick','Yisus']
            },
            {
                nombre:'Dorien',
                edad:46,
                pension:6500.56,
                complaciente:true
            }
        ]   
    }
]
)

##Practica
1. Base de datos, collecciones e inserts
.Nos conectamos con mongosh el mongodb
.Crear una base de datos denominada curso

```json
use curso
```
.verificar que la base de datos no existe
show dbs

.crear una colleccion denominada "facturas con el comando createCollection
y comprobar que aparece tanto la colleccion como la base de datos"