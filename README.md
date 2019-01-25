<p align="center">
<b>🚧 UNDER CONSTRUCTION, DO NOT USE IN PRODUCTION IF YOU LIKE YOUR CURRENT JOB 🚧</b>
</p>

<div align="center">
  <img src="logo.png" alt="logo">
</div>

> v4validation is a tiny and elegant library that let you take back control of your data validation


## Benefits over other solutions

- Simple
- Composable
- Do you really need more?


<!-- ## Install

```
$ npm install v4validation
``` -->


## Usage

```js
import V from 'v4validation'
import validator from 'validator'
import _ from 'lodash/fp'
import database from './database'

// Define some rules. A rule is a tuple of a test function and an error message.
// let rule = [test, error]
let isRequired = [v => Boolean(v), (v, data, path) => `${path} is required`]

let isString = [v => typeof v === 'string', "This is not a string"]

let isEmail = [validator.isEmail, v => `"${v}" is not a valid email`]

let isEqualToPath = path => [(v, data) => _.pathEq(path, v, data), v => `"${v}" doesn't match ${path}'s value`]

let isUnique = [async v => !(await database.users.findOne({ email: v })), v => `"${v}" is already taken`]

// want to change a rule's error?
let myRequired = [isRequired[0], "How simple!"]

(async () => {
	let schema = {
    id: [myRequired, isString]
    email: [isRequired, isEmail, isEqualToPath('confirmation'), isUnique]
  }

  let data = {
    username: 'crjepsen',
    email: 'so.call.me@may.be',
    confirmation 'nope'
  }

  let errors = await validate(schema)(data)
})()
```
