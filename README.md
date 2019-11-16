# Learning-React
Personnal notes from Bob Ziroll

#### React.Fragment

To avoir nesting div every time you include a child in a parent component use <React.Fragement> </React.Fragment> instead of div

```
import React from "react"
import Child from "./Child"

function App() {
    return (
        <React.Fragment>
            <Child />
        </React.Fragment>
    )
}
export default App
```
Be carefull : the DOM hiearachy is changed ! Everything is flatten.

#### Default props

Fallback props in case it is not provided
```
Card.defaultProps = {
    cardColor: "blue"
}
```
If I provide a number in a prop I use curly braces : <Card cardColor="red" width={200} />
