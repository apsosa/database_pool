## Database Pool Conection

When your application needs to retrieve data from the database, it creates a database connection. Creating this connection involves some overhead of time and machine resources for both your application and the database. Many database libraries and ORM's will try to reuse connections when possible, so that they do not incur the overhead of establishing that DB connection over and over again. The pool is the collection of these saved, reusable connections that, in your case, Sequelize pulls from. Your configuration of:

'''
pool: {
    max: 5,
    min: 0,
    idle: 10000
  }
reflects that your pool should:
''''

Never have more than five open connections (max: 5)
At a minimum, have zero open connections/maintain no minimum number of connections (min: 0)
Remove a connection from the pool after the connection has been idle (not been used) for 10 seconds (idle: 10000)
tl;dr: Pools are a good thing that help with database and overall application performance, but if you are too aggressive with your pool configuration you may impact that overall performance negatively.