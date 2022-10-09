# Express client/server demo
Example of node backend with express and client with fetch

## Installation
Install NPM packages
`npm i -S express`
`npm i -S body-parser`

# Server
```js
//exec: /usr/local/opt/node@14/bin/node
const express = require('express')
const bodyParser = require('body-parser');
const app = express()

app.get('/', function (req, res) {
  res.send('Hello World')
})
// POST HTML Form
app.post('/subscribe', 
    express.urlencoded({extended: true}), 
    async (request, response) => {
  print(`email ${request.body.email}`)
  response.sendStatus(200);
})

// POST JSON data
app.post('/activate', bodyParser.json(), async (request, response) => {
  printJSON(request.body);
  response.sendStatus(200);
})

app.listen(4000)
```


# Client
## Post form with JS
```js
const data = new URLSearchParams();
data.append('email', 'name@domain.com');

const result = await fetch("http://localhost:4000/subscribe", {
        body: data,
        method: "post"
});
print(await result.text())
```

## Post form with HTML
```html form
    <form action="http://localhost:4000/contact" method="POST">
        Email: <input type="text" name="email" id="email" required>
        <button type="submit">Submit</button>
    </form>
```

```js
const form = loadBlock('form')
print(form)
```

## POST JSON
```js
const data = {name: "Anthony"}
const result = await fetch("http://localhost:4000/activate", {
        body: JSON.stringify(data),
        method: "post",
        headers: {'Content-Type': 'application/json'}
});
print(await result.text())
```