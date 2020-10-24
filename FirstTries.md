

## First try on codepen

```
<script crossorigin src="https://unpkg.com/react@16/umd/react.production.min.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.production.min.js"></script>

<div id="root">
  
</div>
```
``` 
ReactDOM.render(<h1>Hello World!</h1>,document.getElementById("root"));
```
Hello World!

## Second try
```
const user = {
  firstName:'Philippe',
  lastName:'MEYER'
}

function formatName(){
  return user.firstName + ' ' +user.lastName
}
ReactDOM.render(<h1>Hello {formatName()}</h1>,document.getElementById("root"));
```
Hello Philippe MEYER
