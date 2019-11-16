# Learning-React
Personnal notes from Bob Ziroll

### React.Fragment

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

### Default props

#### Fallback props in case it is not provided
```
Card.defaultProps = {
    cardColor: "blue"
}
```
Note : if I provide a number in a compnent prop I use curly braces : 
```
<Card cardColor="red" width={200} />
```
Of course an object keeps its js syntax _It's confusing at the beginning but it's only logical_
```
Card.defaultProps = {
    cardColor: "blue",
    width: 100,
    height:100
}
```
The whol picture
```
// app.js
import React from "react"
import Card from "./Card"

function App() {
    return (
        <div>
            <Card cardColor="red" height={200} width={400} />
            <Card />
            <Card cardColor="green" />
        </div>
    )
}

export default App

// Card.js
import React from "react"

function Card(props) {
    const styles = {
        backgroundColor: props.cardColor,
        height: props.height,
        width: props.width
    }
    
    return (
        <div style={styles}></div>
    )
}

Card.defaultProps = {
    cardColor: "blue",
    height: 100,
    width: 100
}

export default Card

// Class component version (to have on example)
import React from "react"

class Card extends React.Component {
    render() {
        const styles = {
            backgroundColor: this.props.cardColor,
            height: this.props.height,
            width: this.props.width
        }
        
        return (
            <div style={styles}></div>
        )
    }
}

Card.defaultProps = {
    cardColor: "blue",
    height: 100,
    width: 100
}

export default Card
```

#### In case of the class component we can use this syntax :

```
class Card extends React.Component {
    static defaultProps = {
        cardColor: "blue",
        height: 100,
        width: 100
    }
    ...
 
```   
### Prop types 
#### Type checking (dev tool mainly) and required

``` 
npm install prop-types
``` 
And in the component :
``` 
import React from "react"
import PropTypes from "prop-types"
``` 
And then :
```
Card.propTypes = {
    cardColor: PropTypes.string.isRequired
}
// with no default prop then
Card.defaultProps = {
    height: 100,
    width: 100
}
```
https://reactjs.org/docs/typechecking-with-proptypes.html#proptypes
