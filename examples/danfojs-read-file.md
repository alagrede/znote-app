# DanfoJS read local file
The browser version of **DanfoJS** is loaded by default.


## Save a remote file locally
```js
// Get Url
const r = await fetch('https://raw.githubusercontent.com/plotly/datasets/master/finance-charts-apple.csv')
const content = await r.text();
// Write ti file
_fs.writeFileSync('/Users/alagrede/Desktop/finance-charts-apple.csv', content);
print("Done!")
```

## Read local file
The browser version only support *http://* urls.
To read local file, use Node FS API like below:
```js
const data = _fs.readFileSync('/Users/alagrede/Desktop/finance-charts-apple.csv', 'utf8');
//print(data)
const blob = new Blob([data], {
  type: "application/text",
});

const df = await dfd.readCSV(blob);
print(df);
```

## Use DanfoJS node package
You can also use the Node version of DanfoJS then run your code directly with your local Node installation:

`npm i -S danfo-node`
```js
//exec :/usr/local/opt/node@14/bin/node
const dfd = require("danfojs-node")
let df = await dfd.readCSV('file:///Users/alagrede/Desktop/finance-charts-apple.csv')
print(df)
```

