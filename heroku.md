## Heroku Deploy
Install or update heroku, login & create:
```bash
brew install heroku
# OR
brew upgrade heroku
heroku login
# enter username & password
heroku create
```
Add PostgreSQL DB to Heroku:
```
heroku addons:create heroku-postgresql:hobby-dev
```
Create a .env file:
```
npm install dotenv
touch .env
```
Update .env and .gitignore files:
```bash
# .env (find URL on heroku > settings > config variables)
DATABASE_URL=<your database url here>

# .gitignore
node_modules
.env
```
Update the following script files:
```js
// app.js or index.js (main server file)
const port = process.env.PORT || ${3000}

// knexfile.js
require('dotenv').config()
  // production connection
connection: process.env.DATABASE_URL + '?ssl=true'

// db/knex.js
connection: process.env.NODE_ENV || 'development'
```
Run seeds & migrations on production DB:
```
heroku run knex migrate:latest
heroku run knex seed:run
```
Deploy & open (make sure to commit first):
```
git push heroku master
heroku open
```
Debugging:
```bash
heroku pg:psql # examine tables on production DB
heroku logs --tail # view logs
```
