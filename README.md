# Learning-React WIP
### Personnal notes

https://codepen.io/phmmeyer/pen/jOqZjXd
## First try 
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

## Intallations

You need node.js : https://nodejs.org/en/

version checking : open terminal
node -v

and also npm (node packet manager) : https://www.npmjs.com/

to download easily all files needed to build react applications

version checking : open terminal
npm -v

quite similar !

### Installing create-react-app framework
npm install -g create-react-app

if you got the cb.apply is not a function error

follow these instructions by allaura-dev on github :

C:\Users(your username)\AppData\Roaming

Delete the npm folder and if there is one npm cache folder.

Run npm clean cache —force ( — force is now required to clean cache)


### C:\react>create-react-app hello
Will create all is neccessary to start a new app

cd hello

npm start => launch a server to show our application

If the installation does not work, try reinstalling everything

# Complete React Developer in 2020 (w/ Redux, Hooks, GraphQL)

https://www.udemy.com/course/complete-react-developer-zero-to-mastery/

by Andrei Neagoie and Yihua Zhang

(I prefer this course !)

Where I learn to use componentDidMount() to fetch data from the back and many other things

The app has the following files :

index.js 
```
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);
```

App.js 
```
import React,{Component} from 'react';
import './App.css';

class App extends Component{
  constructor(){
    super();
    this.state = {
      users: []
    }
  }
  componentDidMount(){
    fetch('https:jsonplaceholder.typicode.com/users')
    .then(response =>response.json())
    .then(monsters => this.setState({monsters:monsters}))
  }
  render(){
    return (
      <div className="App">
      {
        this.state.monsters.map(users => <h1 key={monsters.id}> {monsters.name} </h1>)
      }
      </div>
    );
  }
}
export default App;
```

### props.children

I cannot make a complete summary of the course, just a few notes to remember key concepts :

The exercice is a monster gallery where monsters can be search by name
so we have 3 layers or components 
- The App
- The CardList (list of monsters)
- The Card : 1 particular monster

The App component fetches an array of random people (that we are gonna turn into monsters ;-))


in App.js, I import another component names CardList
```
  render(){
    return (
      <div className="App">
        <CardList name="Phil"> // This will appear as props.name in the CardList component
        {
          this.state.monsters.map(monsters => <h1 key={monsters.id}> {monsters.name} </h1>) 
          // this will appear as props.children in the CardList component
        }
        </CardList>
      </div>
    );
  }
```
CardList component :
```
import React from 'react';

import './card-list.styles.css';

export const CardList = (props) => {
    return (<div className='card-list'>{props.children}</div>)
};
```

Components only care about their role and should be responsible for the data they manage
so the CardList should managge the array of monsters itself :

```
// App.js is modified :
...
  render(){
    return (
      <div className="App">
        <CardList monsters={this.state.monsters}/>
      </div>
    );
  }
 ... 
 
 // card-list.component also :
 
export const CardList = props => (
    <div className='card-list'>
        {
          props.monsters.map(monster => (
            <Card key={monster.id} monster={monster}/> 
          ))
        }
    </div>
);
// I don't get at this time why the key is not in the Card.component, we'll see later

// And we've got a card.component :

import React from 'react';
import './card.component.styles.css';

export const Card = props => (
    <div className='card-container'>
        <img 
            alt='monster' 
            src={`https://robohash.org/${props.monster.id}?set=set2&size=180x180`} // Really cool API !
        />
        <h2>{props.monster.name}</h2>
        <p>{props.monster.email}</p>
    </div>
)
```
Note : setState() function has an optional 2nd callback parameter

Todo : write about synthetic events in react https://fr.reactjs.org/docs/events.html
A synthetic event is a react event pretending to be a DOM event, for react to manage its components

# Learning-React OLDER NOTES From a previous course

These are Personal notes 
I started to type while watching from Bob Ziroll Course on Scrimba : https://scrimba.com/g/greact

I think this course is not a bad one 
but there are so many different notions :
Introduction with state and props
then High order components, React State etc... Hooks

I think it's important to pause the course often and then 
build real life examples at each step to really understand
So I will mix with the Net Ninja channel https://www.youtube.com/watch?v=pKYiKbf7sP0

## Introduction

React is a js library using a Virtual DOM
Compoents looks like HTML but they are made JSX syntax which can hold js as well.

### Setup

in VS Code add the extension : ES7 React/Redux/GraphQL/React-Native snippets


### Arrow functions

It's a reminder about ES6 : (scrimba)

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
Use <React.Fragement> </React.Fragment> instead of div to avoid nesting to many divs !
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
Note : if I provide a number in a component prop I use curly braces : 
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
The whole picture
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
From a parent (for instance App()) you can pass props

example in App :
```
import React from "react"
function App() {
    return (
        <div>
            <Navbar backgroundColor="firebrick" />
            <Button backgroundColor="blue" text="Click me!"/>
        </div>
    )
}
export default App
```
But the components can also be rendered as NON self closing elements
the component wikk then have some common features like style and props
but will be like a box where you can put other things in

What the teacher emphases is the use of {props.children} to render anything that is put inside the the component tags

```
function Callout(props) {
    return (
        <div className="callout">
            {props.children}
        </div>
    )
}
```
I don't find very interesting at this time because the Callout component is only a div with a class name !

and then in the App component we use the Callout component by putting regular HTML in it

```
            <Callout>
                <h2>Don't miss out!</h2>
                <p>Unless you don't suffer from FOMO, you better make sure you fill out the email form below!</p>
            </Callout>

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
    static contextType = ThemeContext
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
#### Context Consumer
We use render props
```
import React from "react"
import ThemeContext from "./themeContext"

function Button(props) {
    return (
        <ThemeContext.Consumer>
            {theme => (
                <button className={`${theme}-theme`}>Switch Theme</button>
            )}
        </ThemeContext.Consumer>
    )    
}

export default Button

```
You can mix Context with props

```
import React from "react"
import Header from "./Header"
import Button from "./Button"
import ThemeContext from "./themeContext"

function App() {
    return (
        <div>
            <Header />
            <ThemeContext.Consumer>
                {theme => (
                    <Button theme={theme}/> 
                )}
            </ThemeContext.Consumer>
            <Button  theme="dark"/>
        </div>
    )
}
export default App
```
Make Button function better :

Button.js
```
import React from "react"
import PropTypes from "prop-types"
import ThemeContext from "./themeContext"

function Button(props) {
    return (
        <button className={`${props.theme}-theme`}>Switch Theme</button>
    )    
}
Button.defaultProps = {
    theme: "light"
}
Button.propTypes = {
    theme: PropTypes.oneOf(["light", "dark"])
}
export default Button

```
### Put the provide in it's own file
themeContext.js
```
import React, {Component} from "react"
const {Provider, Consumer} = React.createContext() // destructuring to get both properties
class ThemeContextProvider extends Component {
    render() {
        return (
            <Provider value={"dark"}>
                {this.props.children} // <= will render anything put here ex ; <App />
            </Provider>
        )
    }
}
export {ThemeContextProvider, Consumer as ThemeContextConsumer} 
// ThemeContextProvider is defined as a component 
// ThemeContextConsumer is just the Consumer property
```
index.js
```
import React from "react"
import ReactDOM from "react-dom"

import App from "./App"
import {ThemeContextProvider} from "./themeContext" // <= we just import the provide to wrap our App

ReactDOM.render(
    <ThemeContextProvider>
        <App />
    </ThemeContextProvider>, 
    document.getElementById("root")
)
```
Button.js
```
import React from "react"
import {ThemeContextConsumer} from "./themeContext" // <= we import the ThemeContextConsumer to wrap our Button and get data

function Button(props) {
    return (
        <ThemeContextConsumer>
            {theme => (
                <button className={`${theme}-theme`}>Switch Theme</button>
            )}
        </ThemeContextConsumer>
    )    
}
export default Button
```
#### I don't find it easy but it works :
themeContext.js
```
import React, {Component} from "react"
const {Provider, Consumer} = React.createContext()

/**
 * Challenge:
 * 1) Add state to hold the current theme
 * 2) Add a method for flipping the state between light and dark
 * 
 */

class ThemeContextProvider extends Component {
    state = {
        theme: "dark"
    }
    
    toggleTheme = () => {
        this.setState(prevState => {
            return {
                theme: prevState.theme === "light" ? "dark" : "light"
            }
        })
    }
    
    render() {
        return (
            <Provider value={{theme: this.state.theme, toggleTheme: this.toggleTheme}}>
                {this.props.children}
            </Provider>
        )
    }
}

export {ThemeContextProvider, Consumer as ThemeContextConsumer}
```
Button.js
```
import React from "react"
import {ThemeContextConsumer} from "./themeContext"

function Button(props) {
    return (
        <ThemeContextConsumer>
            {theme => (
                <button onClick={theme.toggleTheme} className={`${theme.theme}-theme`}>Switch Theme</button>
            )}
        </ThemeContextConsumer>
    )    
}

export default Button
```
