# Desafio MongoDB

El objetivo de este desafio es familiarizarnos con el CLI de mongo shell, donde practicamos los comandos basicos para realizar un CRUD

## Consigna

Utilizando mongo Shell, crear una base de datos llamada ecommerce que contenga dos colecciones: mensajes y productos.

crear base de datos:

```bash
use ecommerse
```

crear collecciones:

```bash
db.createCollection('messages')
db.createCollection('products')
```

Agregar 10 documentos con valores distintos a las colecciones mensajes y productos. El formato de los documentos debe estar en correspondencia con el que venimos utilizando en el entregable con base de datos MariaDB.

```bash
db.products.insertMany([
{
"title": "mochila",
"price": 2860,
"thumbnail": "https://cdn3.iconfinder.com/data/icons/education-209/64/bag-pack-container-school-512.png",
"description": "mochila ideal para el colegio"
},
{
"title": "regla",
"price": 900,
"thumbnail": "https://cdn3.iconfinder.com/data/icons/education-209/64/ruler-triangle-stationary-school-512.png",
"description": "regla de 30 cm para Algebra y Geometria"
},
{
"title": "lapiz",
"price": 120,
"thumbnail": "https://cdn3.iconfinder.com/data/icons/education-209/64/pencil-pen-stationery-school-512.png",
"description": "lapiz grafo fino"
},
{
"title": "calculadora",
"price": 3350,
"thumbnail": "https://cdn3.iconfinder.com/data/icons/education-209/64/calculator-math-tool-school-512.png",
"description": "calculadora cientifica"
},
{
"title": "microscopio",
"price": 4990,
"thumbnail": "https://cdn3.iconfinder.com/data/icons/education-209/64/microscope-lab-science-school-512.png",
"description": "microscopio sencillo inicial"
},
{
"title": "tubo de ensallo",
"price": 4320,
"thumbnail": "https://cdn3.iconfinder.com/data/icons/education-209/64/tube-lab-science-school-512.png",
"description": "tubo cristal templado"
},
{
"title": "cuaderno",
"price": 1700,
"thumbnail": "https://cdn3.iconfinder.com/data/icons/education-209/64/book-note-paper-school-512.png",
"description": "cuaderno 96 hojas"
},
{
"title": "gorro graduacion",
"price": 1280,
"thumbnail": "https://cdn3.iconfinder.com/data/icons/education-209/64/graduation-square-academic-cap-school-512.png",
"description": "gorro graduacion standar"
},
{
"title": "globo terraqueo",
"price": 2300,
"thumbnail": "https://cdn3.iconfinder.com/data/icons/education-209/64/globe-earth-geograhy-planet-school-512.png",
"description": "globo terraqueo 60cm diametro"
},
{
"title": "acuarela",
"price": 580,
"thumbnail": "https://cdn3.iconfinder.com/data/icons/education-209/64/paint-color-pallete-brush-academy-512.png",
"description": "acuarela de 4 colores"
}
])
```

```bash
  db.messages.insertMany([
{
"email": "martin@gmail.com",
"message": "hola que tal"
},
{
"email": "marcos@gmail.com",
"message": "todo bien vos?"
},
{
"email": "martin@gmail.com",
"message": "me alegro aca probando cosas"
},
{
"email": "martin@gmail.com",
"message": "que tal el dia?"
},
{
"email": "marcos@gmail.com",
"message": "que bueno me alegro, en la misma"
},
{
"email": "marcos@gmail.com",
"message": "pronto para mirar el mundial"
},
{
"email": "martin@gmail.com",
"message": "quedamos afuera, ahora vamos por algun sudaca"
},
{
"email": "marcos@gmail.com",
"message": "si jugamos mal los dos primeros"
},
{
"email": "marcos@gmail.com",
"message": "mala suerte"
},
{
"email": "martin@gmail.com",
"message": "ahora a llorar al cuartito"
}
])
```

Listar todos los documentos en cada colección.

```bash
db.products.find().pretty()
```

```bash
db.messages.find().pretty()
```

Mostrar la cantidad de documentos almacenados en cada una de ellas.

```bash
db.products.countDocuments()
```

```bash
db.messages.countDocuments()
```

### Realizar un CRUD sobre la colección de productos

a) Agregar un producto más en la colección de productos

```bash
  db.products.insertOne({
  "title": "reloj",
  "price": 1990,
  "thumbnail": "https://cdn3.iconfinder.com/data/icons/education-209/64/clock-stopwatch-timer-time-256.png",
  "description": "reloj despertador con 3 alarmas programables"
  })
```

b) Realizar una consulta por nombre de producto específico:

i) Listar los productos con precio menor a 1000 pesos.

```bash
db.products.find({"price": {$lt: 1000}})
```

ii) Listar los productos con precio entre los 1000 a 3000 pesos.

```bash
db.products.find({$and: [{"price": {$gt: 1000}}, {"price": {$lt: 3000}}]})
```

iii) Listar los productos con precio mayor a 3000 pesos.

```bash
db.products.find({"price": {$gt: 3000}})
```

iv) Realizar una consulta que traiga sólo el nombre del tercer producto más barato.

```bash
db.products.find().limit(1).skip(2).sort({"price": 1})
```

c) Hacer una actualización sobre todos los productos, agregando el campo stock a todos ellos con un valor de 100.

```bash
db.products.updateMany({}, {$set: {"stock": 100}})
```

d) Cambiar el stock a cero de los productos con precios mayores a 4000 pesos.

```bash
db.products.updateMany({"price": {$gt: 4000}}, {$set: {"stock": 0}})
```

e) Borrar los productos con precio menor a 1000 pesos

```bash
db.products.deleteMany({"price": {$lt: 1000}})
```

Crear un usuario 'pepe' clave: 'asd456' que sólo pueda leer la base de datos ecommerce. Verificar que pepe no pueda cambiar la información.

```bash
db.createUser({"user": "pepe", "pwd": "asd456", "roles": [{"role": "read", "db": "ecommerce"}]})
```

Para realizar la prueba del usuario "pepe" debemos ejecutar el servidor en modo autenticacion, se debe usar el siguiente comando:

```bash
mongod --auth
```

y en el mongo shell ingresamos con las credenciales

```bash
mongosh -u pepe -p asd456
```

## Guardar base de datos o hacer backup

Para poder hacer un backup de la base de datos debemos ejecutar el siguiente comando, tener en cuenta que se guardara en la carpeta que estamos parados, y el servidor mongod debe estar corriendo, en este caso mi base de datos es ecommerce

```bash
mongodump --db ecommerce
```

## Importar o Restaurar la base de datos

El servidor mongod debe estar corriendo antes de ejecutar el comando

```bash
mongorestore --db ecommerce url:baseDeDatos
```

Adicional a esto se debe tener instalado las MongoDB DataBase Tools y configuradas en las variables de entorno

Ya que a partir de la version de MongoDB 4.4 se instalan por separado como la Shell

Dejo link de documentacion: [database-tools](https://www.mongodb.com/docs/database-tools/installation/installation-windows/)
