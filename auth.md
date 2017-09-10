# Authentication
## Server Side
Install additional dependencies:
```
npm install bcrypt
npm install jsonwebtoken
```
Create an `auth.js` file in the routes folder and make sure it is properly linked to the `app.js` file:
```js
// app.js (main server file)
// other stuff here
const auth = require('./routes/auth')

// body parser and other stuff here
app.use('/auth', auth);

```
Add a JWT secret in the `.env` file:
```
TOKEN_SECRET=*secret*
```
Setup additional connections in the `auth.js` file:
```js
// routes > auth.js
// default from express generator
const express = require('express')
const router = express.Router()

// additional connections & modules
const knex = require('../db/knex')
const bcrypt = require('bcrypt')
const jwt = require('jsonwebtoken')
require('dotenv').config() // JWT secrets will be stored in the .env
```
Example SIGNUP code:
```js
// routes > auth.js
router.post('/signup', (req, res, next) => {
  // validate unique email
  knex('users').where('users.email', req.body.email)
    .then(user => {
      if (user.length === 0) {
        // first hash the password
        let saltRounds = 10
        let hash = bcrypt.hashSync(req.body.password, saltRounds)
        req.body.password = hash
        // insert user with hashed PW into database
        knex('users').insert(req.body).returning('*')
          .then(newUser => {
            let payload = newUser[0] // comes back as an array of 1
            delete payload.password // don't send the hashed PW to the front end
            let token = jwt.sign(payload, process.env.TOKEN_SECRET)
            res.json({token});
          })
      } else {
        res.json({error: 'Email already in use.'})
      }
    })
});
```
Example LOGIN code:
```js
// routes > auth.js
router.post('/login', (req, res) => {
  knex('users').where('users.email', req.body.email)
    .then(user => {
      if (user.length === 0) {
        res.json({error: 'Email or password did not match'})
      } else {
        let match = bcrypt.compareSync(req.body.password, user[0].password)
        if (match) {
          let payload = user[0]
          delete payload.password
          let token = jwt.sign(payload, process.env.TOKEN_SECRET)
	  res.json({token: token, message: 'login successful'});
        }
      }
    })
});
```

## Client Side
Example setup:
```js
// scripts > app.js
const url = 'http://localhost:3000'

$(() => {
  // other stuff
  $('form.login').submit(logIn)
  $('form.signup').submit(signUp)
})
```
Example SIGNUP code:
```js
// scripts > app.js
function signUp(event) {
  event.preventDefault()
  const username = $('input[name=signup-username').val()
  const email = $('input[name=signup-email').val()
  const password = $('input[name=signup-password').val()
  const data = {username, email, password}
  $.post(`${url}/auth/signup`, data)
    .then(response => {
      if (response.error) {
        alert(response.error)
      } else {
        localStorage.setItem('token', response.token)
        location.href = '/secrets.html'
      }
    })
}
```
Example LOGIN code:
```js
// scripts > app.js
function logIn(event) {
  event.preventDefault()
  const username = $('input[name=login-username').val()
  const password = $('input[name=login-password').val()
  const data = {username, password}
  $.post(`${url}/auth/login`, data)
    .then(response => {
      if (response.error) {
        alert(response.error)
      } else {
        localStorage.setItem('token', response.token)
        location.href = '/secrets.html'
      }
    })
}
```
# Authorization
## Client Side
Example setup:
```js
// scripts > app.js
$(() => {
  // Invoke function in document.ready
  authorizeUser()
  // other stuff
})

function authorizeUser() {
	let token = localStorage.getItem('token')
	if (token) {
		location.href = '/secrets.html'
	}
}
```
Example authorization code (once user is authenticated):
```js
// scripts > secrets.js
const decodedToken = parseJWT(localStorage.getItem('token'))
const userId = decodedToken // The user ID of the person logged in
const url = 'http://localhost:3000'

// GET, POST, PUT, DELETE requests based on userId
function parseJWT(token) { // THIS WORKS
	let base64Url = token.split('.')[1];
	let base64 = base64Url.replace('-', '+').replace('_', '/');
	return JSON.parse(window.atob(base64));
};
```
