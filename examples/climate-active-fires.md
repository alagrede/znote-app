## Active fires in the World
In this notebook we use the **last 7d of active fires**. 

**Where get data?**
You can ask a specific location in history with the links below:
[Last fires data](https://firms.modaps.eosdis.nasa.gov/active_fire/#firms-txt)
[Ask archives data](https://firms.modaps.eosdis.nasa.gov/download/create.php)

Globals **vars** and **functions** *(play me before others blocks)*
```js
//global 1
//hide
const basePath = "/Users/alagrede/Desktop/znote-public-github/examples/data/"; // change me!

// custom functions
async function listDays(start, end) {
    for(var arr=[], dt=new Date(start); 
            dt<=new Date(end); 
            dt.setDate(dt.getDate()+1)){
        arr.push(new Date(dt).toISOString().slice(0,10));
    }
    return arr;
}

async function sleep(time) {
  await new Promise(r => setTimeout(r, time));
}
```

**Download the last 7 days into a file**
```js
const r = await fetch('https://firms.modaps.eosdis.nasa.gov/data/active_fire/modis-c6.1/csv/MODIS_C6_1_Global_7d.csv')
const content = await r.text();
_fs.writeFileSync(basePath + 'modis-active-fire.csv', content);
print("Done!");
```


## Active fires on the world map
Show last **7 days** of active fires
```js
//hide
const last7d = basePath + "modis-active-fire.csv";
const content = _fs.readFileSync(last7d, 'utf8');

function show(date) {
    const latLong = content.split('\n').filter(r=>String(r.split(",")[5]).startsWith(date)).map(r => [r.split(",")[0], r.split(",")[1]]) // lat/long
    latLong.shift(); // remove header

    var data = [{
        type: 'scattergeo',
        lon: latLong.map(r=>r[1]),
        lat: latLong.map(r=>r[0]),
        marker: {size: 2, color:'red'},
    }];

    var layout = {
        geo: {
            // Africa
            //center: { lat: -16.400190, lon: 22.515316},
            //projection: {
            //    scale: 1.5
            //},
            scope: 'world', // "africa" | "asia" | "europe" | "north america" | "south america" | "usa" | "world"
            resolution: 50,
            showland: true,
            showocean: true,
            //bgcolor:"#46484A",
        },
        title: date,
        //paper_bgcolor:"#46484A",
    };

    Plotly.newPlot(el, data, layout);
}

const today = new Date(); // new Date("2022-08-24")
const lastWeek = new Date();
lastWeek.setDate(today.getDate()-7); // new Date("2022-08-18") 

var daylist = await listDays(
    lastWeek,
    today);

for(const day of daylist) {
    show(day);
    await sleep(500);  
}
```

## France
### Evolution of active fires in france from 2001 to today
```js
//hide
// All France fires
const allfires = basePath + "fire_archive_M-C61_290304.csv";
const content = _fs.readFileSync(allfires, 'utf8');
const activeFires = content.split('\n');
activeFires.shift();
const count = activeFires
    // map to month
    .map(r=>String(r.split(",")[5]).slice(0,7)) // date field like 2008-08
    // map to year
    //.map(r=>String(r.split(",")[5]).slice(0,4)) // date field like 2008
    // group by
    .reduce((total, value) => {
        total[value] = (total[value] || 0) + 1;
        return total;
    }, {});


const data = [{
  y: Object.values(count),
  x: Object.keys(count),
  type: 'scatter'
}];

const layout = {
  title: "Evolution of active fires in france",
  height: 400,
  width: 500
}

Plotly.newPlot(el, data, layout);
```



