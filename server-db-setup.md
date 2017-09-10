# Server & Database Setup with Express Generator
## Dependencies
You will need express-generator installed globally to have access to the 'express' cli command
```
npm install -g express-generator
```

## Initial Setup
* Create a GitHub repo
* Clone the repo and cd into it

Console:
```
express --view=hbs --git
npm install pg knex dotenv
knex init
createdb *database-name*
touch .env
```

In .gitignore file:
```
node_modules
.env
```

## Knex: Environment Configuration
In knexfile.js
```js
require('dotenv').config();

module.exports = {
  development: {
    client: 'pg',
    connection: 'postgres://localhost/*database-name*'
  },

  production: {
    client: 'pg',
    connection: process.env.DATABASE_URL + '?ssl=true'
  }
};
```

Console:
```
mkdir db
cd db
touch knex.js
```

In knex.js
```js
const environment = process.env.NODE_ENV || 'development';
const config = require('../knexfile.js')[environment];

module.exports = require('knex')(config);
```

## Knex: Migrations

Console:
```
knex migrate:make 01_migration_name
```

Migration file should look something like:
```js
// DO NOT use camelCase in column names, dashes or underscores OK
exports.up = function(knex, Promise) {
  return knex.schema.createTable('member', (table) => {
  table.increments();
  table.text('username').notNullable().unique();
  table.text('email').notNullable().unique();
  table.text('password').notNullable();
  table.date('date_created').notNullable().defaultTo(new Date);
  // ^ in case of postgres timezone error:
  // table.date('date_created').notNullable().defaultTo(knex.raw('now()'));
  table.boolean('is_active').notNullable().defaultTo(true);
  table.text('bio');
  })
};

exports.down = function(knex, Promise) {
  return knex.schema.dropTableIfExists('member');
};
```

Foreign Key relationships are handled as such:
```js
table.integer('memberID').references('member.id').unsigned().onDelete('cascade')
```

Run migration
```
knex migrate:latest
```

* Continue making migrations for all the tables in your database

## Knex: Seeds

Console:
```
knex seed:make 01_seed_name
```

Seed file should look something like:
```js
var bcrypt = require('bcrypt');

exports.seed = function(knex, Promise) {
  // Deletes ALL existing entries
  return knex.raw('DELETE FROM "user"; ALTER SEQUENCE user_id_seq RESTART WITH 3;')
    .then(function () {
      var users = [{
        id: 1,
        name: 'sam',
        email: 'sam@gmail.com',
        password: bcrypt.hashSync('sammyg21', 10)

      }, {
        id: 2,
        name: 'alex',
        email: 'alex@gmail.com',
        password: bcrypt.hashSync('alexmart05', 10)
      }];
      return knex('user').insert(users);
    });
};
```

Run seed
```
knex seed:run
```

* Keep creating seeds and running them until your entire database is seeded

## Require Knex

In your routes file make sure to require knex
```js
const knex = require('../db/knex');
```

Do the same in your app.js file if you plan to write knex queries there:
```js
const knex = require('../db/knex');
```

## CORS

Install CORS module
```
npm install cors
```

In app.js
```js
const cors = require('cors');

// Make sure the following line is above your routes
app.use(cors());
```

## Connecting to client
If running lite-server and nodemon at same time, change the port under `bin > www` to anything other than `3000`
