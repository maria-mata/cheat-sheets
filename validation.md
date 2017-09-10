## Validation Examples
### Server Side
For PUT and POST requests:
```js
// validation functions can be in a separate file or not
function validUser(user) {
  let validName = typeof user.name == 'string' && user.name.trim() != ''
  let validEmail = typeof user.email == 'string' && user.email.match(/([@])/g) != null
  let validPassword = typeof user.password == 'string' && user.password.trim() != ''

  return validName && validEmail && validPassword
}

// use validation functions in the routes, for example
router.post('/', (req, res) => {
  let post = req.body

  if (validUser(post)) {
    knex('user').insert(post)
      .returning('*')
      .then (user => {
        res.json(user)
      })
  } else {
    res.json({message: 'Invalid user input.'})
  }
})
```
For GET requests (avoid server crashes):
```js
// create a validation function
function validId(req, res, next) {
  let id = req.params.id
  if (!isNaN(id)) {
    return next()
  } else {
    res.json({message: 'Invalid ID parameter.'})
  }
}

// then insert the function as an argument to the get route (middleware)
router.get('/:id', validId, (req, res) => {
  let id = req.params.id

  knex('user').where('id', id).first()
    .then(user => {
      res.json(user)
    })
})
```

### Client Side
(coming soon...)
