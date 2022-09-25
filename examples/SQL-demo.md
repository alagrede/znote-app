
## Make SQL query

[Blog 💡: Use Znote as SQL client](https://blog.znote.io/2021/sql-functions/)

### Install NPM packages
Install SQL driver for your database
```bash
npm install --save mysql2
```

Install tablify to read results as table
```bash
npm install --save tablify
```

### Create SQL functions
Create these 2 functions *f(x)* below to connect to your database and tablify results
```
// custom functions
async function localSQL(sqlQuery) {
  const { Sequelize } = require('sequelize');
  const sequelize = new Sequelize("", "root", "root", {  
    dialect: "mysql"/* one of 'mysql' | 'mariadb' | 'postgres' | 'mssql' */,
    host: "localhost"
  });

  sequelize.authenticate();
  const [results, metadata] = await sequelize.query(sqlQuery);
  
  sequelize.close()
  return results;
}

async function printSQL(sqlQuery) {
  const tablify = require('tablify').tablify
  print(tablify(await localSQL(sqlQuery)));
}
```

### Make SQL query on your database
```js
await printSQL("select * from book.vente_item limit 10");
```