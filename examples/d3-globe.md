# D3 World map

**Install**
- d3 - `https://d3js.org/d3.v7.min.js`
- topojson - `https://unpkg.com/topojson@3`

```js scripts=d3,topojson
//hide
const svg = d3.create("svg");
svg.attr("width", "100%")
svg.attr("height", "500px")

const projection = d3.geoNaturalEarth1();
const pathGenerator = d3.geoPath().projection(projection);

svg.append('path')
    .style("fill", "#4242e4")
    .attr('d', pathGenerator({type: 'Sphere'}));


d3.json('https://unpkg.com/world-atlas@1.1.4/world/110m.json')
.then(data => {
    const countries = topojson.feature(data, data.objects.countries);
    svg.selectAll('path').data(countries.features)
    .enter().append('path')
        .style("fill", "lightgreen")
        .style("stroke", "black")
        .style("stroke-opacity", "0.1")
        .attr('d', pathGenerator);
    print(svg.node().outerHTML)
});
```