
## Install react
`npm i -S react`
`npm i -S react-dom`

```js
import React from 'react';
import ReactDOM from 'react-dom';

const LikeButton = (props) => {
  return <h1>{props.name}</h1>;
}
ReactDOM.render(<LikeButton 
  name="Hello from React!" />
  , htmlEl);
```

