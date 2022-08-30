## Global Temperature anomalies
```js
const r = await fetch('https://www.ncei.noaa.gov/access/monitoring/climate-at-a-glance/global/time-series/globe/land_ocean/ytd/12/1880-2016.json')
const json = await r.json();
//printJSON(json);
const result = Object.entries(json.data).map(year => [year[0], year[1]]);

const data = [{
  y: result.map(r=>r[1]),
  x: result.map(r=>r[0]),
  type: 'scatter'
}];

const layout = {
  title: "World temperature anomalies",
  height: 400,
  width: 500
}

Plotly.newPlot(el, data, layout);
```


## US Annual Average Temperature and Anomaly 
```js
const r = await fetch('https://www.ncei.noaa.gov/access/monitoring/climate-at-a-glance/national/time-series/110-tavg-ytd-12-1895-2016.json?base_prd=true&begbaseyear=1901&endbaseyear=2000')
const json = await r.json();
//printJSON(json);
const result = Object.entries(json.data).map(year => [year[0].substring(0,4), year[1].anomaly]);
const data = [{
  y: result.map(r=>r[1]/1.8), // fahrenheit to celcius
  x: result.map(r=>r[0]),
  type: 'scatter'
}];

const layout = {
  title: "US temperature anomalies (Celcius)",
  height: 400,
  width: 500
}

Plotly.newPlot(el, data, layout);
```

## Europe average temperature anomalies
```js
const r = await fetch('https://www.ncei.noaa.gov/cag/global/time-series/europe/land/1/7/1880-2022/data.json')
const json = await r.json();
//printJSON(json);
const result = Object.entries(json.data).map(year => [year[0].substring(0,4), year[1]]);
const data = [{
  y: result.map(r=>r[1]),
  x: result.map(r=>r[0]),
  type: 'scatter'
}];

const layout = {
  title: "Europe temperature anomalies",
  height: 400,
  width: 500
}

Plotly.newPlot(el, data, layout);
```