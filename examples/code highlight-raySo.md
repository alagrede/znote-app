# Ray.so
Create beautiful code image

```js code
const array1 = [1, 4, 9, 16];
const map1 = array1.map(x => x * 2)
console.log(map1)
```

```js
const COLORS="midnight" 
const BACKGROUND="true"
const DARK_MODE="true"
const PADDING="64"
const LANGUAGE="auto"
const TITLE="Untitled-1"
const CODE=btoa(loadBlock("code"))
open(`https://ray.so/?colors=${COLORS}&background=${BACKGROUND}&darkMode=${DARK_MODE}&padding=${PADDING}&title=${TITLE}&code=${CODE}&language=${LANGUAGE}`)
```