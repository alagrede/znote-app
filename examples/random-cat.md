## Random Cat PNG
```js
const r = await fetch('https://cataas.com/cat', {cache: "no-store"})
const bytes = await r.arrayBuffer();

// could be a custom function
const blob = new Blob([bytes], { type: "image/png" });
const imageUrl = URL.createObjectURL(blob);
const img = createElement("img");
img.src = imageUrl;
htmlEl.innerHTML = ""; 
htmlEl.append(img);
```

## Random Cat GIF
```js
const r = await fetch('https://cataas.com/cat/gif', {cache: "no-store"})
const bytes = await r.arrayBuffer();
await printAsImage(bytes);
```


```
//custom function
async function printAsImage(bytes) {
  const blob = new Blob([bytes], { type: "image/png" });
  const imageUrl = URL.createObjectURL(blob);
  const img = createElement("img");
  img.src = imageUrl;
  htmlEl.innerHTML = ""; 
  htmlEl.append(img);
}
```