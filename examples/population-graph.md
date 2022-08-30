## Population count
```js
const r = await fetch('http://api.worldbank.org/v2/countries/USA/indicators/SP.POP.TOTL?per_page=5000&format=json')
const json = await r.json();
const result = json[1]
await printTable(result);
//printJSON(result);
```

## Graph
```js
//hide
async function load(country) {
  const r = await fetch('http://api.worldbank.org/v2/countries/'+country+'/indicators/SP.POP.TOTL?per_page=5000&format=json')
  const json = await r.json();
  const result = json[1].map(year => [year.date, year.value]);
  return result;
}

const usa = await load('USA');
const china = await load('CHN');
const india = await load('IND');

const data = [{
  y: usa.map(r=>r[1]),
  x: usa.map(r=>r[0]),
  name: "USA",
  type: 'scatter'
},
{
  y: china.map(r=>r[1]),
  x: china.map(r=>r[0]),
  name: "CHINA",
  type: 'scatter'
},
{
  y: india.map(r=>r[1]),
  x: india.map(r=>r[0]),
  name: "INDIA",
  type: 'scatter'
}
];

const layout = {
  height: 400,
  width: 500
}

Plotly.newPlot(el, data, layout);
```