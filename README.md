# Learning-React
### Personnal notes

**React Concepts**

1. Don't touch the DOM, it's React's job.
2. Build websites like lego blocks (if possible, reusable blocks!)
3. Unidirectional data flow (from top to bottom)

**React dev job is :**

1. Decide on Components
2. Decide the State and where it lives
3. What changes when state changes (think also about shouldComponentUpdate !)
4. UI, the rest is up to you


## Intallations

EDIT : If you need several version of nodejs fro différent projects

then use nvm (Node Version Manager)

https://github.com/coreybutler/nvm-windows


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

note : when you create a new app, the create-react-app version is automaticaly updated to teh latest version which can be a problem is you are xorkin on the same app (same github repo) on several computers (beacause only the dev part is saved on github) 

goto :

https://github.com/facebook/create-react-app/blob/master/CHANGELOG.md

**example : **

```
npm install --save --save-exact react-scripts@4.0.0
//or
yarn add --exact react-scripts@4.0.0
```

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

After a while working on the project, here comes the 'this' affair 

A function declared in our Class component cannot explicitly use the this keywork to refer to the class context

```
  handleChange(e){
    this.setState({searchField:e.target.value})
  }
```
2 solutions :

Use bind to declare the function inside the constructor
```
  constructor(){
    super();
    this.state = {
      monsters: [],
      searchField:''
    };
    this.handleChange = this.handleChange.bind(this);
  }
```

or better (because simpler) use the arrow functions capability of referencing the outer this context 
```
  handleChange(e){
    this.setState({searchField:e.target.value})
  }
  // becomes :
  handleChange = (e) => {
    this.setState({searchField:e.target.value})
  }
```  
See the result in https://github.com/PhilippeMarcMeyer/monsters-rolodex

### Lesson 52 : Asynchronous state

Calls to setState are asynchronous

React handles re-rendering for us

The callback in setState function is just there for debugging reasons.

It is good practise to pass a function as the first parameter of setState call in order to be sure its the last version of the state that will be used for the update.

Make a very simple state :
```  
class App extends React.Component {
  constructor(){
    super()
    this.state = {
      meaningOfLife : 42
    }
  }
}
```  
Make a button to update state
```  
<p>
  {this.state.meaningOfLife}
</p>
<button 
  onClick={this.handleClick}
>
Update State
</button>
```  
```  
handleClick = () => {
  this.setState((prevState,prevProps) => {
    return {meaningOfLife: prevState.meaningOfLife +1}
  }
}
```  
Note : if you need to use props in the constructor, just pass props as a parameter 

```  
class App extends React.Component {
  constructor(props){
    super(props)
    this.state = {
      meaningOfLife : 42
    }
  }
}
``` 

## Life cycle methods

They are listeners that give us hooks to perform some actions

For instance componentDidMount() tells us the component is ready and that it is time to fetch some date for instance

shouldComponentUpdate(nextProps, nextState) is very important

in a component you can check if it should be rendered depending on state changes

We won't render if the props specific to that component haven't changed

``` 
shouldComponentUpdate(nextProps, nextState){
  return nextProps.mySpecificProp != this.props.mySpecificProp
}

``` 

## NEW PROJECT : eCommerce platform CRWN-CLOTHING
```
create-react-app crwn-clothing
cd crwn-clothing
yarn start
```

We are building the different components (see the repo)

and then adding SASS a css preprocessor

```
yarn add node-sass
```

Sass extension is .scss

Note to myself : learn flex (css) it looks great !

With SASS, what normal css selectors can be nested within each others

```
.main-div { 

  .other-div{

  }
}

```
equivalent of : 
```
.main-div .other-div{

}

```

This in convenient for further reading : elements don't look unrelated

and there is also & prefix not to repeat the stryle name :

```
.main-div { 

  &:first-child{

  }
}

```
## Overall structure :

In the source folder we have got few files at the root :
the rest is organised in sub folders

Files :
- Index.js + index.css
- App.js + App.css

Folders :
- assets
- components
- pages

assets : can hold elements such as images

pages : are well pages it means containers that can be routed via a menu

each different in its own folder and ech composed of 2 files : jsx ans scss

- homepage
  - homepage.component.jsx
  - homepage.styles.scss
- shop
  - shop.component.jsx
  - shop.styles.scss
- sign-in-and-sign-out
  - sign-in-and-sign-out.component.jsx
  - sign-in-and-sign-out.styles.scss

Each jsx file is either a functional stateless component or a statefull class component depending on the fact it needs or not data

components are reusable components (part of pages) that are used by pages or other components (russian doll structure)
alike pages they are also organised in folders holding 2 files : 1 jsx for templating and 1 scss for styling

To illustrate the "russian doll" system :

- homepage
  - homepage.component.jsx
    - directory
      - directory-component.jsx
       - menu-item
         - menu-item-component.jsx
         - menu-item-styles.scss
      - directory-styles.scss
  - homepage.styles.scss

The homepage is only 1 page among others although it is the default one

index.js is main :
```
import React from 'react';
import ReactDOM from 'react-dom';
import {BrowserRouter} from 'react-router-dom';

import './index.css';
import App from './App';

ReactDOM.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>,
  document.getElementById('root')
);
```

App.js comes above :
```
import React from 'react';
import { Switch, Route } from 'react-router-dom';

import './App.css';

import HomePage from './pages/homepage/homepage.component';
import ShopPage from './pages/shop/shop.component';
import Header from './components/header/header.component';

function App() {
  return (
    <div>
      <Header />
      <Switch>
        <Route exact path='/' component={HomePage} />
        <Route exact path='/shop' component={ShopPage} />
      </Switch>
    </div>
  );
}
export default App;
```
We see a Headercomponent that is shared by all pages 
```
import React from 'react';
import {Link} from 'react-router-dom';

import './header.styles.scss';
import {ReactComponent as Logo} from '../../assets/crown.svg';

const Header = () => (
    <div className='header'>
        <Link className='logo-container' to="/">
            <Logo className='logo'/>
        </Link>
        <div className='options'>
            <Link className='option' to='/shop'>
            SHOP
            </Link>
            <Link className='option' to='/shop'>
            CONTACT
            </Link>
            <Link className='option' to='/sign'>
            SIGN
            </Link>
        </div>
    </div>
)
export default Header;
```
For every jsx file, we need to decide if we prefer a functional or class component:

For instance, the homepage.component.jsx is stateless and the data comes with the directory.component.jsx which passes state to the numerous menu-item compnents

**homepage**
```
import React from 'react';

import Directory from '../../components/directory/directory.component';

import './homepage.styles.scss';

const HomePage = () => (
    <div className = 'homepage'>
        <Directory/>
    </div>
)

export default HomePage;
```
**directory**
```
import React from 'react';

import MenuItem from '../menu-item/menu-item.component';

import './directory.styles.scss';

class Directory extends React.Component {
  constructor() {
    super();
    this.state = {...};
   }
 render() {
    return (
      <div className='directory-menu'>
        {this.state.sections.map(({ id,...otherSectionProps}) => (
          <MenuItem key={id} {...otherSectionProps} />
        ))}
      </div>
    );
  }
}
export default Directory;
```




## ROUTING 

Hijacking the browser history thanks to the history API

Installing react-router-dom

```
yarn add react-router-dom

```
Now index.js looks like this :
```
import React from 'react';
import ReactDOM from 'react-dom';
import {BrowserRouter} from 'react-router-dom';

import './index.css';
import App from './App';

ReactDOM.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>,
  document.getElementById('root')
);
```

Wrapping BrowserRouter component around our app gives it new functionnalities

in App.js :
```
import React from 'react';
import {Route} from 'react-router-dom';

import './App.css';

import HomePage from './pages/homepage/homepage.component'

const HatsPage = () => (
    <div>
      <h1>HATS PAGE</h1>
    </div>
)

function App() {
  return (
    <div>
        <Route exact path='/' component={HomePage}/>
        <Route path='/hats' component={HatsPage}/>
    </div>
  );
}

export default App;
```

On the ecommerce app CRWN-CLOTHING 

App.js holds the router, wraps then directory component then wraps (looping) each menu-item component implementation

The directory gives each item a linkUrl prop that will modify the url :

```
onClick={()=> { 
  history.push(`${match.url}${linkUrl}`)
}}>
```
Where match.url is the base url and the linkUrl holds 'hats' or 'jacquets'

and so its via the url and the router that we change page.

The router is passed to the menu-item component so it can push to history and trigger the page change

it is done very easily in the menu-item component :

```
import React from 'react';
import {withRouter} from 'react-router-dom';
...
export default withRouter(MenuItem);
```

A note on using svg in react :
```
import {ReactComponent as Logo} from '../../assets/crown.svg';
```

Using links : the example of the header component :

```
import React from 'react';
import {Link} from 'react-router-dom';

import './header.styles.scss';
import {ReactComponent as Logo} from '../../assets/crown.svg';

const Header = () => (
    <div className='header'>
        <Link className='logo-container' to="/">
            <Logo className='logo'/>
        </Link>
        <div className='options'>
            <Link className='option' to='/shop'>
            SHOP
            </Link>
            <Link className='option' to='/shop'>
            CONTACT
            </Link>
        </div>
    </div>
)

export default Header;
```
