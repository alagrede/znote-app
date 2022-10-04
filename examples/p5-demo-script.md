
# Use P5
https://p5js.org/

In the **Package** popup add: 
name:`p5`
url:`https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.4.2/p5.min.js`

**P5** is used here in [instance mode](https://github.com/processing/p5.js/wiki/Global-and-instance-mode)


```js scripts=p5
//hide
const s = p => {

  p.setup = function() {
    const canvas = p.createCanvas(700, 410);
  };

  p.draw = function() {
    p.background(0);
    p.fill(255);
    p.rect(p.mouseX, p.mouseY, 50, 50);
  }
};

htmlEl.innerHTML = '' // reset result div before to load P5
const P5 = new p5(s, htmlEl); // htmlEl is the 
```