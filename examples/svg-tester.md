# SVG tester
```html ubuntu
<svg viewBox="-70 -70 140 140">

  <defs>
    <path id="b" d="M 23,-20 A32,32 0,0,0 -23,-20 L -40,-30 A42,42 0,0,1 -14,-47 A17,17 0,0,0 14,-47 A42,42 0,0,1 40,-30 Z"/>
    <circle id="h" cx="0" cy="-57" r="12"/>
  </defs>

  <g transform="translate(5,5)" opacity="0.125">
    <use xlink:href="#h" transform="rotate(30)"/>
    <use xlink:href="#h" transform="rotate(150)"/>
    <use xlink:href="#b" transform="rotate(30)"/>
    <use xlink:href="#h" transform="rotate(-90)"/>
    <use xlink:href="#b" transform="rotate(150)"/>
    <use xlink:href="#b" transform="rotate(-90)"/>
  </g>

  
  <use xlink:href="#h" fill="#d00" transform="rotate(30)"/>
  <use xlink:href="#b" fill="#f40" transform="rotate(30)"/>
  <use xlink:href="#h" fill="#f80" transform="rotate(150)"/>
  <use xlink:href="#b" fill="#d00" transform="rotate(150)"/>
  <use xlink:href="#h" fill="#f40" transform="rotate(-90)"/>
  <use xlink:href="#b" fill="#f80" transform="rotate(-90)"/>


</svg>
```

```html android
<svg viewBox="0 0 96 105">
  <g fill="#97C024" 
    stroke="#97C024" 
    stroke-linejoin="round" 
    stroke-linecap="round">
    <!-- body -->
    <path d="M14,40v24M81,40v24M38,68v24M57,68v24M28,42v31h39v-31z" stroke-width="12"/>
    <!-- ears -->
    <path d="M32,5l5,10M64,5l-6,10 " stroke-width="2"/>
  </g>

  <!-- head -->
  <path d="M22,35h51v10h-51zM22,33c0-31,51-31,51,0" fill="#97C024"/>
  <!-- eyes -->
  <g fill="#FFF">
    <circle cx="36" cy="22" r="2"/>
    <circle cx="59" cy="22" r="2"/>
  </g>
</svg>
```

```html rubyforge
<svg viewBox="0 0 100 100">
  <path d="M16,22l40,-14l33,28l-10,42l-40,14l-32-28z" stroke="#000" stroke-width="2" fill="#e82323"/>
  <path d="M19,25l36,-12l28,24 c-12,49 -45,-15 -62,32 l-10-6z" fill="#edc6c6"/>
</svg>
```

```js
print(loadBlock('rubyforge')) // try: ubuntu/android/rubyforge
```