# Learning-React
Personnal notes from Bob Ziroll

## Introduction

### Arrow functions
They allow to have no binding for functions in the constructor of class Components.
Ex : 
```
increment = () => {
    this.setState(prevState => {
        return {
            count : prevState.count + 1;
        }
    }
}
```
### Constructor is not mandatoy :
```
state = {count:0}
```
instead of 
```
constructor(){
    super()
    this.state = {count:0}
}
```

### Object destructuring :
```
const {count} = this.state // get the count variable
const {count,color} = this.state // get 2 variables
const {count:number} = this.state // get a variable number with the value of state.count
```

### React.Fragment
Use <React.Fragement> </React.Fragment> instead of div
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

#### Example with an enum
```
Card.propTypes = {
    cardColor: PropTypes.oneOf(["red", "blue"]),
    height: PropTypes.number.isRequired,
    width: PropTypes.number
    }
```

## Reusability

DRY

Bob talks about inheritance and composition

### Composition over inheritance

https://www.youtube.com/watch?v=wfMtDGfHWpA&feature=youtu.be Fun Fun Function : Mattias Petter Johansson, mpj for short

He says composition is a design pattern that is build around what things (objects) DO and inheritance around what what they are
He cleary favours composition as is is more flexible as "humans cannot predict future"

the action objects are they designed first : the contains simple methods like a mover that moves(), a singer that sings() etc...
and build composite objects that use assign to get the the action methods 

Here is his funny example :

The object receives its state from outside as a parameter
and gets its methods from other basic objects :
```
const barker = (state) => ({
  bark: () => console.log('Woof, I am ' + state.name)
})
const driver = (state) => ({
  drive: () => state.position = state.position + state.speed
})

const murderRobotDog = (name)  => {
  let state = {
    name,
    speed: 100,
    position: 0
  }
  return Object.assign(
        {},
        barker(state),
        driver(state),
        killer(state)
    )
}
const bruno =  murderRobotDog('bruno')
bruno.bark() // "Woof, I am Bruno"
```

### Children 

The Components are rendered as self closing elements ```<Navbar/> ```

Call for Action 
Here is a non re-usable component :

App.js :
```
import React from "react"
import CTA from "./CTA"

function App() {
    return (
        <div>
            <CTA />
        </div>
    )
}

export default App
```
CTA.js
```
import React from "react"

function CTA() {
    return (
        <div className="border">
            <h1>This is an important CTA</h1>
            <button>Click me now or you'll miss out!</button>
        </div>
    )
}

export default CTA
```
CTA() provides the basic structure and is reusable thru props

```
import React from "react"

function CTA(props) {
    return (
        <div className="border">
            {props.children}
        </div>
    )
}

export default CTA
```
```
import React from "react"
import CTA from "./CTA"

function App() {
    return (
        <div>
            <CTA position="right">
                <h1>This is an important CTA</h1>
                <button>Click me now or you'll miss out!</button>
            </CTA>
            
            <CTA>
                <form>
                    <input type="email" placeholder="Enter email address here"/>
                    <br />
                    <button>Submit</button>
                </form>
            </CTA>
        </div>
    )
}

export default App
```

### Higher order component

Like high order function (that take another function as parameter : ex array.filter())

It's a function that takes a component as argument and returns another component with new features

```
const changedComponent = withUpgrader(aComponent)
export default changedComponent
```
or in  line :
```
export default withUpgrader(aComponent)
```
index.html
```
<html>
    <head>
        <link rel="stylesheet" href="styles.css">
    </head>
    <body>
        <div id="root"></div>
        <script src="index.pack.js"></script>
    </body>
</html>
```
index.js
```
import React from "react"
import ReactDOM from "react-dom"
import App from "./App"

ReactDOM.render(<App />, document.getElementById("root"))
```
withPointlessHOC.js : 
```
import React from "react"

// A function that takes a component as its first argument and returns a new component that wraps the given component, providing extra capabilities to it.

export function withPointlessHOC(component) {
    const Component = component
    return function(props) {
        return (
            <Component {...props} />
        )
    }
}
```
App.js : 
```
import React from "react"
import {withPointlessHOC} from "./withPointlessHOC"

function App(props) {
    return (
        <div>Hello world!</div>
    )
}

const PointlessHOC = withPointlessHOC(App)
export default PointlessHOC
```
withExtraPropAdded.js
```
import React from "react"

export function withExtraPropAdded(component) {
    const Component = component
    return function(props) {
        return (
            <Component anotherProp="Blah blah blah" {...props} />
        )
    }
}
```
#### Exercice :

App.js
```
import React from "react"
import {withFavoriteNumber} from "./withFavoriteNumber"
function App(props) {
    return (
        <div>{props.favoriteNumber}</div>
    )
}

const PreferateNumber = withFavoriteNumber(App)
export default PreferateNumber
```
withFavoriteNumber.js
```
import React from "react"
export function withFavoriteNumber(component) {
    const Component = component
        return function(props){
            return(
                <Component favoriteNumber={7} {...props}/>
            )
        }
}
```

### Render props

Not so easy to grasp at the beginning 
instead of passing simple properties in the Component let's pass a function and le't call it render !

Example of Bob Ziroll :

App.js
```
import React from "react"
import Favorite from "./Favorite"

function App() {
    return (
        <div>
            <Favorite />
        </div>
    )
}

export default App

```
Favorite.js will user reusable Toggler.js Component
```
import React, {Component} from "react"
import Toggler from "./Toggler"

function Favorite(props) {
    return (
        <Toggler render={function(on,toggle) {
            return (
                <div>
                    <h3>Click heart to favorite</h3>
                    <h1>
                        <span 
                            onClick={toggle}
                        >
                            {on ? "❤️" : "♡"}
                        </span>
                    </h1>
                </div>
            )
        }}/>
    ) 
}

export default Favorite
```
Toggler.js
```

import React, {Component} from "react"

class Toggler extends Component {
    state = {
        on: this.props.defaultOnValue
    }
    
    toggle = () => {
        this.setState(prevState => {
            return {
                on: !prevState.on
            }
        })
    }
    
    render() {
        return (
            <div>
                {this.props.render(this.state.on,this.toggle)}
            </div>
        )
    }
}

export default Toggler
``` 
Another Component using toggler.js with arrow function for a change :
Menu.js
```
import React from "react"
import Toggler from "./Toggler.js"

function Menu(props) {
    return (
        <Toggler defaultOnValue={props.defaultOnValue} render={
            (on,toggle) => (
            <div>
                <button onClick={toggle}>{on ? "Hide" : "Show"} Menu </button>
                <nav style={{display: on ? "block" : "none"}}>
                    <h6>Signed in as Coder123</h6>
                    <p><a>Your Profile</a></p>
                    <p><a>Your Repositories</a></p>
                    <p><a>Your Stars</a></p>
                    <p><a>Your Gists</a></p>
                </nav>
            </div> 
            )
        } />
    ) 
}

export default Menu
```
#### Exercice => DataFetcher

I find these render props difficult to grasp : they are usefull to make reusable Components

DataFetcher.js

```
import React, {Component} from "react"

class DataFetcher extends Component {
    state = {
        loading: false,
        data: null
    }
    
    componentDidMount() {
        this.setState({loading: true})
        fetch(this.props.url)
            .then(res => res.json())
            .then(data => this.setState({data: data, loading: false}))
    }
    
    render() {
        return (
            this.props.children(this.state.data, this.state.loading)
        )
    }
}

export default DataFetcher

```

this.props.children(this.state.data, this.state.loading)

is a nameless function that will be defined in the calling Component (here App.js)

App.js
```
import React from "react"
import DataFetcher from "./DataFetcher"

function App() {    
    return (
        <div>
            <DataFetcher url="https://swapi.co/api/people/1">
                {(data, loading) => (
                        loading ? 
                        <h1>Loading...</h1> :
                        <p>{JSON.stringify(data)}</p>
                    )
                }}
            </DataFetcher>
        </div>
    )
}

export default App
```
#### Another exercice : add error handling

App.js
```
import React from "react"
import DataFetcher from "./DataFetcher"

function App() {    
    return (
        <div>
            <DataFetcher url="https://swapi.co/api/people/1">
                {({data, loading,error}) => (
                    error ? 
                    <h1>{error}</h1> :
                    (loading ? 
                    <h1>Loading...</h1> :
                    <p>{JSON.stringify(data)}</p>)
                )}
            </DataFetcher>
        </div>
    )
}

export default App
```

DataFetcher.js

```
import React, {Component} from "react"

class DataFetcher extends Component {
    state = {
        loading: false,
        data: null,
        error : null
    }
    
    componentDidMount() {
        this.setState({loading: true})
        fetch(this.props.url)
            .then(res => res.json())
            .then(data => this.setState({data, loading: false}))
            .catch(err => this.setState({error:err.message, loading: false}))
    }
    
    render() {
        const {data, loading,error} = this.state
        return (
            this.props.children({data, loading,error})
        )
    }
}

export default DataFetcher
```

I was able to add error handling easily :-)

### React's Tree rendering

When a component needs rendeeing then React also renders its children and the children of its children ans so on and i might be very costly.

## Optimisation
### shouldComponentUpdate()
Lifecycle method
If you set it manually you should loop on the props to determine if something was changed, use PureComponent instead of component

### pureComponent
Will render only when needed : use only Class based components 
```
import React, {PureComponent} from "react"
```

### React.memo()
A version of pure.component that ca be used with fonctional components

```
export default React.memo(GrandParent)
```
so it's a high order component (a component that takes a component as imput and returns a comonent as output)
it uses a cached version of the component and compares it the actual component props

You can provide your own areEqual fonction (that will overide the internal areEqual function)

```
function areEqual(prevProps, nextProps) {
  /*
  return true if passing nextProps to render would return
  the same result as passing prevProps to render,
  otherwise return false
  */
}

export default React.memo(GrandParent, areEqual)
```

Premature optimisation can be costly !

First implement features ans then shearch for optimisation if needed

### React Context
since v 16.3

Its not possible to pass data to siblings so if 2 siblings need data we have to put State in their parent 
and one of the parent's sibling needs to share data : up we go !

The problem arises when a faraway components needs its share of data and that we have to put state at the highest level of the App and pass props all the way down : its called props drilling !

Context provides a way to pass information to the components without using the props system

The components that need to provide information will be wrapped into a PROVIDER
and the components that need information will be wrapped in a CONSUMER

#### Creating a context
Here we create a context and wrap the entire App in it and pass data with the value property
```
import React from "react"
import ReactDOM from "react-dom"

import App from "./App"

const ThemeContext = React.createContext()
// ThemeContext.Provider & ThemeContext.Consumer

ReactDOM.render(
    <ThemeContext.Provider value={"light"}>
    <App />
    </ThemeContext.Provider>,
    document.getElementById("root")
)
```
The teacher, Bob Ziroll, put evrything in the same file for use to unsderstand better and go step by step, but then says the context should go in its own file :

themeContext.js
```
import React from "react"
const ThemeContext = React.createContext()
export default ThemeContext
```

and then import the ThemeContext in index.js
```
import React from "react"
import ReactDom from "react-dom"
import ReactContext from "./themeContext"

ReactDom.render(
<ReactContext.Provider>
<App/>
</ReactContext.Provider>,
document.getElementById("root")
)

```
Header.js
```
import React, {Component} from "react"
import ThemeContext from "./themeContext"
class Header extends Component {
    render() {
        const theme = this.context
        return (
            <header className={`${theme}-theme`}>
                <h2>Light Theme</h2>
            </header>
        )    
    }
}

export default Header
```



