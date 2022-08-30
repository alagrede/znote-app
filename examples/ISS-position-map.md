## ISS position

```js
const r = await fetch('http://api.open-notify.org/iss-now.json')
const json = await r.json();

var data = [{
    type: 'scattergeo',
    lon: [json.iss_position.longitude],
    lat: [json.iss_position.latitude],
    marker: {size: 13,},
    name: 'ISS',
}];

var layout = {
    geo: {
        scope: 'world',
        resolution: 50,        
        showland: true,
        countrycolor: '#d3d3d3'
    }
};

Plotly.newPlot(el, data, layout);
```