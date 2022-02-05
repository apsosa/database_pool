## Database Pool Conection

When your application needs to retrieve data from the database, it creates a database connection. Creating this connection involves some overhead of time and machine resources for both your application and the database. Many database libraries and ORM's will try to reuse connections when possible, so that they do not incur the overhead of establishing that DB connection over and over again. The pool is the collection of these saved, reusable connections that, in your case, Sequelize pulls from. Your configuration of:

Cuando una aplicacion necesita consultar datos de una base de datos, crea una conexión a la misma. Crear dicha conexión involucra tener algun overhead de tiempo y recursos para ambos, la aplicacion y la base de datos. Muchas bibliotecas de dases de datos y ORM's siempre trataran de reusar las conexiones generadas previamente cuando sea posible, para no incurrir al overhead de establecer la conexion a la base de datos una y otra vez. La pool conexion es la coleccion de conexiones reusables que se guardan en una base de datos determinada.

### Sequelize
En el caso de sequelize obtiene el comportamiento de las poll conection del siguiente parametro:

```
pool: {
    max: 5,
    min: 0,
    idle: 10000
  }
```
reflects that your pool should:
- Never have more than five open connections (max: 5)
- At a minimum, have zero open connections/maintain no minimum number of connections (min: 0)
- Remove a connection from the pool after the connection has been idle (not been used) for 10 seconds (idle: 10000)

Pools are a good thing that help with database and overall application performance, but if you are too aggressive with your pool configuration you may impact that overall performance negatively.

With this idle configuration, if my query does more than 10 seconds to execute, is the connection closed without me getting a response?
no, idle is literally how long the connection will sit there doing nothing. what you're talking about is the pool.timeout setting. which i think defaults to 30s. after that the query times out and you'll get an error.

If you're connecting to the database from a single process, you should create only one Sequelize instance. Sequelize will set up a connection pool on initialization. This connection pool can be configured through the constructor's options parameter (using options.pool), as is shown in the following example:

```
const sequelize = new Sequelize(/* ... */, {
  // ...
  pool: {
    max: 5,
    min: 0,
    acquire: 30000,
    idle: 10000
  }
});
```
If you're connecting to the database from multiple processes, you'll have to create one instance per process, but each instance should have a maximum connection pool size of such that the total maximum size is respected. For example, if you want a max connection pool size of 90 and you have three processes, the Sequelize instance of each process should have a max connection pool size of 30.



##### Spanish

## Pool Conection de una Base de datos

Cuando una aplicacion necesita consultar datos de una base de datos, crea una conexión a la misma. Crear dicha conexión involucra tener algun overhead de tiempo y recursos para ambos, la aplicacion y la base de datos. Muchas bibliotecas de dases de datos y ORM's siempre trataran de reusar las conexiones generadas previamente cuando sea posible, para no incurrir al overhead de establecer la conexion a la base de datos una y otra vez. La pool conexion es la coleccion de conexiones reusables que se guardan en una base de datos determinada.

### Sequelize
En el caso de sequelize obtiene el comportamiento de las poll conection del siguiente parametro, el cual recibe al crear una conexion:

```
pool: {
    max: 5,
    min: 0,
    idle: 10000
  }
```

Lo cual refleja el comportamiento de las pool conection en Sequelize de la siguiente manera:
- max: 5 (Nunca tener mas de 5 conexiones abiertas)
- min: 0 (No hay un minimo posible de conexiones)
- idle: 10000 (Remueve la pool conexion despues de estar inactiva por 10 segundos)

###### Consideraciones:
- Si la base de datos se conectan a travez de muilples procesos, cada proceso va a generar una conexion a la base de datos