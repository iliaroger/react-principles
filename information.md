# react and javascript principles

```js
const array1 = ["mick", "jery", "catlyn"];

const array2 = [...array1, "bill"];

console.log(array2);

function call(...args){
    args.forEach(el =>{
        console.log(el);
    })
}

call(2, 5, 6, 8);
```

## returning array elements with .map()

with map you can loop through the array and easily display array elements without mutating the original array.

```js
render(){
    return(
        this.props.persons.map(elements => {
            return <li>{elements.name}</li>
        })
    )
}
```

## wrapping a higher order function and passing a function to it

- the connect() function receives the mapStateToProps() function which then becomes a standalone function. imagine connect(mapStateToProps) being replaced by a function called stateProps which then will take Validation as an argument. stateProps(Validation).

```js
import {connect} from 'react-redux';

class Validation extends Component{

}

const mapStateToProps = state =>{
    return {
        name: state.name
    }
}

export default connect(mapStateToProps)(Validation)
```

## preserving the imutability of an object

- often times, you dont want to manipulate the state inside react but more creating a copy and switching the copy with the original one. the spread operator is a good method to do so but there is also the object method.

- using concat() will allow yoo not to mutate the original array but more concatinate two arrays together into a new one.

```js

state = {
    name: 'Olafson'
    id: []
}

const newState = Object.assign({}, state);

const newArray = state.id.concat({id: 'd23n892'})

export default connect(mapStateToProps)(Validation)
```

## react principles

## importing

```js
import {combine} from './combine'
import {combine as bine} from './combine'
import {* as group} from './combine'
```

## creating a react project

- npm install -g create-react-app
- npx create-react-app appname

## styling react components

## flexibel way to style components

create an if statement inside render() to check couple of statements
to easily manipulate css while the virtual dom is being rerendered

```js
const classNameArray = [];

if(this.state.name.length <= 2){
    classNameArray.push("red");
}

render(){
    return(
        <h1 className={classNameArray.join('')}>Home Section</h1>
    )
}
```

## inline css and general inline styling

normally you cant apply pseudo selectors inside jsx.
this is practically the limit of inline css inside jsx.

```js
render(){

    const styling = {
        backgroundColor: 'green',
        fontFamily: 'Helvetica Neue'
    }

}
```

but with a npm package your are allowed to use functionalities like pseude selectors inside jsx!

- install radium: npm install --save radium

```js
import Radium from 'radium';

// your class App

export default Radium(App);
```

Radium is an higher order function which will inject new functionalities into App.

inline css with radium:

```js
render(){

    const styling = {
        backgroundColor: 'green',
        fontFamily: 'Helvetica Neue',
        ':hover': {
            backgroundColor: 'purple',
            fontSize: '12px'
        }
    }

    styling[':hover'] = {
        backgroundColor: 'midnightblue'
    }

}
```

## media queries with radium

in order to use media queries you need to import StyleRoot from the radium package and
afterwards wrap your jsx inside the main class app with the StyleRoot element

```js
import Radium, {StyleRoot} from 'radium';

// your class App

render(){

    // define media queries
    const mediaQueries = {
        '@media(min-width: 500px)' :{
            backgroundColor: 'orange'
        }
    }

    return(

        <StyleRoot>
            <div>
                <h1 style = {mediaQueries}></h1>
            </div>
        </StyleRoot>
    )
}

export default Radium(App);
```

## inline styling with styled-components

install: npm i --save styled-components

```js
import styled from 'styled-components';

// your class App

render(){

    // define media queries
    const DivElement = styled.div
    `
        background-color: yellow;
        font-family: default;

        @media(min-width: 500px) {
            background-color: 'orange';
            font-size: 60px;
            width: 60%;
        }
    `
    return(

        <DivElement>
            <div>
                <h1 style = {DivElements}></h1>
            </div>
        </DivElement>
    )
}

export default App;
```

note: styled component converts the written inline css to a class and not just like in radium, to inline css.

you can also insert props into your own css style and check for certain conditions:

```js
import styled from 'styled-components';

// your class App

render(){

    // define media queries
    const DivElement = styled.div
    `
        background-color: ${props => props.nameCheck === 'Rickart' ? 'green' : 'yellow' };
        font-family: default;

        @media(min-width: 500px) {
            background-color: 'orange';
            font-size: 60px;
            width: 60%;
        }
    `
    return(

        <DivElement nameCheck = {this.persons[0].name}>
            <div>
                <h1></h1>
            </div>
        </DivElement>
    )
}

export default App;
```

## css module styling

in order to use css seperately from multiple files and still apply classes to components without
the stress and hustle to create unique class names, you can use css modules to make it happen

- css files must contain the keyword `module`. for example: main.module.css

```js
import classes from './main.module.css';

// your class App

render(){

    return(

        <DivElement>
            <div>
                <h1 style = {classes.h1}></h1>
            </div>
        </DivElement>
    )
}

export default App;
```

note: if you are using an older version of react, you need manually to setup the class module settings inside
the `webpack.config` files.

## debugging react apps

- chrome debugger tools can help find tricky logical errors inside your app (chrome dev tools -> sources tab -> create breakpoints)
- another great tool is: react developer tools from chrome (extension). install it and use it in the arrow drop down menu
whilst debugging your application

## error boundries

to prevent a full crash of your application or just prevent unexpected error messages from appearing, you can create error boundries (which are technically just components) and use them to catch errors

1. create a component

``` js
import React, {Component} from 'react';

class ErrorBoundry extends Component {
    state = {
        hasError: false,
        errorMessage: ''
    }

    componentDidCatch(error, info) =>{
        this.setState({
            hasError: true,
            errorMessage: error
        })
    }

    render(){
            if(this.state.hasError){
                return <h1>{this.state.errorMessage}</h1>
            }
            else{
                return null;
            }
    }

}
export default ErrorBoundry;
```

2.import the component and wrap it around where an error might occur

```js
import classes from './main.module.css';
import ErrorBoundry from './ErrorBoundry'

// your class App

render(){

    return(

        <ErrorBoundry>
            <div>
                <h1 style = {classes.h1}></h1>
                <button>Get Data</button>
            </div>
        </ErrorBoundry>
    )
}

export default App;
```

wrap the `ErrorBoundry` element around the area where an error might occur. in this case, clicking the button and await data might cause an unexpected error which will be caught by the error boundry component.

## components and react internals in detail

note: always use a good project structure. for example: assets(folder), components(folder), containers(folder).
containers consist of the main app.js file, the main css file and the app.test.js file.

- you should not use states in every component. try to manage states in just a couple of components or classes.
- in the past, you could use only classes for statemanagment. but with the introduction of react hooks you could also incoperate them into functions and manage state within functions. both approaches are valid. older projects (before the introduction of react hooks) couldn't be made without the class based approach

## component creation lifecycle

- during the process of creating a component, react will call functions sequentially. this process of calling said functions can be used to extend the control of component manipulation. for example, you could fetch data from your database after the componentDidMount() was invoked. this simply gives you more control over the whole process behind the scenes.

## update lifecycle

```js
import React, {Component} from 'react';

class Person extends Component {

    componentDidUpdate(prevProps, prevState, snapshot){
        console.log(prevProps);
        return false;
    }

    render(){
        return(

        )
    }

}
```

the update lifecycle will go through certain methods and then rerender the virtual dom. to create a hook and affect the update lifecycle inside a component, you need to call the lifesycle methods inside the component and apply your own custom functionalities inside them.

- componentDidUpdate() is one of the most used methods

## useEffect() for function components

```js
import React, {useEffect} from 'react';

const Person = (props)=>{

    useEffect(()=>{
        console.log('use effect method triggered');
    })

    return(

    )
}
```

- inside a function component the useEffect() is the second most important function after setState(). it is basically triggered every time after a render cycle and is the equivalent compared to componentDidMount() and componentDidUpdate() methods inside the class component that are triggered sequentially.
- to control when exactly the useEffect() is being triggered, you need to pass a second argument into the function. more specifically, you need to pass an array with the information, that will be changed. if the element inside the array is changed then a rerender will occure. if the data stays the same it will not invoke the method.
- if you want to invoke the useEffect() only once in the beginning of the cycle you need to pass an empty array as the second argument
- you can call useEffect() multiple times inside an a function component

```js
import React, {useEffect} from 'react';

const Person = (props)=>{

    useEffect(()=>{
        console.log('use effect method triggered');
    }, []) // will be invoked once in the beginning

    useEffect(()=>{
        console.log('use effect method triggered');
    }, [props.name]) // will only be invoked if the props.name will be changed

    return(

    )
}
```

in order to add functionalities after a component was removed from the dom you can simply insert a return statement inside the useEffect().

```js
import React, {useEffect} from 'react';

const Person = (props)=>{

    useEffect(()=>{
        console.log('use effect method triggered');
        return () =>{
            // add functionalities after the component was removed
        }
    }, [])

    return(

    )
}
```

in this case above, the removed component function will be only called if the second argument of useEffect() is an empty array. otherwise (if the array would have elements inside it) the clean up function would be called every update.

## react.memo() function component

react.memo() will create a snapshot of the old props and then compare them to the new props. if a change isnt detected, then the component wont be rendered.

```js
import React, {useEffect} from 'react';

const Person = (props)=>{

    useEffect(()=>{
        console.log('use effect method triggered');
        return () =>{
            // add functionalities after the component was removed
        }
    }, [])

    return(

    )
}

export default React.memo(Person)
```

## pure component inside classes

pure components will check automatically before rerendering happens if the props have changed (newProps === oldProps).
so you can omit the shouldComponentUpdate() because PureComponent will check automatically for changes.

```js
import React, {PureComponent} from 'react';

class App extends PureComponent{
    render(){
        return(
            <h1>Change Content</h1>
        )
    }
}

export default App;
```

## auxiliary wrapper for multiple adjacent html elements

create a folder called 'hoc' (higher order component) and create a file called `auxiliary.js`. aux.js is not a valid name because its reserved for windows (aux is a reserved name). the aux components are basically replacing divs. you can return multiple aux elements and seperate your code without receiving any type of error message.

```js
// auxiliary.js file
const Aux = => props.children;

export default Aux;

//app.js file

import React, {Component} from 'react';
import Aux from '../hoc/auxiliary';

class App extends Component{

    render(){
        return(
            <Aux>
                <h1></h1>
            </Aux>

            <Aux>
                <h1></h1>
            </Aux>
        )
    }

}
```

## react.fragment

react fragment is basically the same like aux but it is built inside react.

```js
import React, {Component} from 'react';
import Aux from '../hoc/auxiliary';

class App extends Component{

    render(){
        return(
            <React.Fragment>
                <h1></h1>
            </React.Fragment>

            <React.Fragment>
                <h1></h1>
            </React.Fragment>
        )
    }

}
```

## higher order component class wrapper

with a higher order component you are able to create a component that would wrap around jsx with some functionalities. a good use case would be to create a wrap component that would catch errors that happen from within the wrapper. in this case the hoc will wrap jsx and also apply some custom css to it.  

```js
// classWrapper.js
import React from 'react';

const classWrapper = props => {
    return <div classes={props.classes}>{props.children}</div>
}

export default classWrapper;

// app.js
import React, {Component} from 'react';
import styles from './styles/classStyle.css';
import WithClass from '../hoc/classWrapper';

class App extends Component{

    render(){
        return(
            <WithClass classes={styles}>
                <h1></h1>
            </WithClass>
        )
    }

}

export default App;
```

## higher order functions wrapper

in order to wrap jsx around a component and handle the logic elsewhere, you can create a different kind of hoc.

```js
// higher order function
import React, {Component} from 'react';

export default const checkingFunction = (WrappedComponent, className) =>{
    return props =>{
        <div className={props.className}>
        <WrappedComponent/>
        </div>
    }
}

// app.js
import React, {Component} from 'react';
import styles from './styles/classStyle.css';
import checkingFunction from '../hoc/checkingFunction';

class App extends Component {
    render(){
        return(
            <h1>Rendering Component</h1>
        )
    }
}

export default checkingFunction(App, styles)
```

in this example you pass the app into the higher order component (checkingFunction(App)) wich will be wrapped around the div with the class name attribute. this is how higher order component wraps other content around them. a great use case would be to create a hoc which would handle any kind of error handling.

## passing props through higher order components

```js
// higher order function
import React, {Component} from 'react';

export default const checkingFunction = (WrappedComponent, className) =>{
    return props =>{
        <div className={props.className}>
        <WrappedComponent {...props}/>
        </div>
    }
}
```

in order to forward props to the destination, you need to use the spread operator.

## setState() and how to avoid mistakes

inside the setState() you wont automatically refer with this.state to your old state. sometime your old state could be manipulated during the time when setState() was called. so in order to make sure that you refer to the old state withing setState() you need to pass an anonymous function with certain parameters.

```js
// app.js
import React, {Component} from 'react';

class App extends Component{

    this.setState((prevState, props)=>{
        return{
            persons: person,
            changeCounter: prevState.updateState
        };
    });

    render(){
        return(

        )
    }
}
```

you will return an object with the correct previous state by passing an anonymous function into setState().

## propTypes for prop type safety

propTypes is a package for react which will ensure that your props inside your classes or components will always receive the right data type.

install: npm i --save prop-types

```js
// app.js
import React, {Component} from 'react';
import PropsTypes from 'prop-types';

class App extends Component{

    state = {
        person = [{
            name: 'Olafson'
            lastName: 'Juerginson'
        },
        {
            name: 'Thor',
            lastName: 'Thunderson'
        }]
    }

    this.setState((prevState, props)=>{
        return{
            persons: person,
            changeCounter: prevState.updateState
        };
    });

    render(){
        return(

        )
    }

    App.PropTypes = {
        person: PropTypes.obj
        name: PropTypes.string
    }
}

export default App;
```

you only need to add PropTypes to the class or component of your choice and then define every prop with the desired prop type.

## create a reference for any element with the ref keyword

with an reference keyword you can create a variable inside the class that will store the reference of a given element (for example an html element).

```js
// app.js
import React, {Component} from 'react';

class App extends Component{

    constructor(){
        super(props);
        this.h1Reference = React.createRef();
    }

    componentDidMount(){
        this.h1Reference.current.focus();
    }


    render(){
        return(
            <h1 ref={this.h1Reference}>Text element</h1>
        )
    }
}

export default App;
```

in this example we created a reference variable inside the constructor and passed the real reference from the h1 element (from the render method). after that, you can use the h1 reference within the whole class.  

## ref inside function components

same thing as for classes but you only need to import the createRef hook.

```js
// higher order function
import React, {Component, useRef} from 'react';

export default const ComponentRender = () =>{

    const h1reference = useRef(null);

    useEffect(()=>{
        h1.reference.current.click();
    },[]);

    return(
        <h1 ref={h1reference}>Render Data</h1>
    )
}
```

useEffect will be called after the return statement. so if you would assign the reference after the reference creation you would receive an null pointer exception because the h1 reference wasn't assigned to the reference variable yet.

## context, passing data from component to component without

in order to have an global object or any kind of data available you need to create an context object and import it into any file you like to use it in.

- create an context component

```js
// context component
import React, {Component, useRef} from 'react';

const ContextData = React.createContext();

export default ContextData;

```

- then you need to import the context component and wrap it around the component that you like to pass the data to. the file where you pass dynamic data to somewhere is called 'Context.Provider'. You provide data to other components. its the origin of dynamic data.

```js
// provider class
import React, {Component, useRef} from 'react';
import Person from './Persons/Person'
import ContextData from '../context/ ContextData';

class App extends Component {
    state = {
        personName: 'Olaf',
        personLastName: 'Gundelson'
    }
    render(){
      return(
          <ContextData.Provider value={
              personName: 'Olaf',
              personLastName: 'Gundleson'
          }>
          {
            <Person name={this.state.personName} lastName={this.state.personLastName}></Person>
          }
          </ContextData>
      )  
    }
}
```

- now you need to import the context to a receiving component. the component that receives dynamic data is called 'Context.Consumer'.
wrap jsx with the context element function that receives the data via the context argument. use the context argument to access the data.

```js
// consumer component
import React, {Component, useRef} from 'react';
import ContextData from '../context/ ContextData';

const Person = ()=>{
    return(
        <ContextData>
        {
            context =>{
                <h1>{context.personName}</h1>
                <h1>{context.personLastName}</h1>
            }
        }
        </ContextData>
    )
}

export default Person;
```

## static property for context in classes and components

if you try to access the context object within a function inside a class you will not be able to do that. so in order to create a static variable that holds the reference of the context object you need to do the following:

```js
// context static class
import React, {Component, useRef} from 'react';
import Person from './Persons/Person'
import ContextData from '../context/ ContextData';

class App extends Component {

    //context property will be created in the background

    static contextType = ContextData;

    state = {
        personName: 'Olaf',
        personLastName: 'Gundelson'
    }
    render(){
      return(
          <ContextData.Provider value={
              personName: 'Olaf',
              personLastName: 'Gundleson'
          }>
          {
            <Person name={this.state.personName} lastName={this.state.personLastName}></Person>
          }
          </ContextData>
      )  
    }
}
```

after declaring contextType and passing the ContextData component as a reference, react will create a property called (context) for you in the background. and now you will be able to access the context object within the whole class.

```js
// function component
import React, {Component, useRef, useContext} from 'react';
import ContextData from '../context/ ContextData';

const Person = ()=>{

    const Context = useContext(ContextData);

    return(
        <h1>{Context.personName}</h1>
        <h1>{Context.personLastName}</h1>
    )
}

export default Person;
```

by using the useContext hook you can create a context variable which can be accessed within the whole function.

## reaching out to the web (http/ajax)

promise based http client for node and browser: axios.

install: npm i axios --save

- get data with axios & post data with axios:

```js
// class axios
import React from 'react';
import ContextData from '../context/ ContextData';
import axios from 'axios';

class App extends Component{

    state = {
        person: []
    }

    axios.get('https://placeholderjson.com/data')
    .then((response)=>{
        this.setState({
            person: response.data
        })
        console.log(response);
    })

    postData(){
        data = {
            name: this.state.person.name;
            lastName: this.state.person.lastName;
        }

        axios.post('https://placeholderjson.com/data',)
    }

    render(){
        return(
            <Person name={this.state.person.name} handler={this.postData}></Person>
        )
    }

}

export default App;
```

## interceptors in axios

do fully control requests and responses you can use interceptors. they are used to give you more control on authorization within the whole app and are easy to setup:

- go to the index.js file (the file where `<App>` is being rendered)

```js
// index.js
import React, {Component, useRef, useContext} from 'react';
import ContextData from '../context/ ContextData';
import axios from 'axios';

axios.interceptors.request.use(request => {
    console.log(request);
    // check the request object for authorization etc.
    return request;
}, error =>{
    return Promise.reject(error);
})

axios.interceptors.response.use(response => {
    console.log(response);
    // check the request object for authorization etc.
    return response;
}, error =>{
    return Promise.reject(error);
})
```

- you need to return request/response otherwise no get/post method can be send within the whole app.
- you need to return the error in a promise otherwise no componente with an error handling function will receive an error object.

## axios global header settings

in axios you can create header settings which will be stored and send in every get/post method. a great usecase would be to store a header setting after the user has loged in and this is authorized to traverse inside the app.

```js
// index.js
import React, {Component, useRef, useContext} from 'react';
import ContextData from '../context/ ContextData';
import axios from 'axios';

axios.defaults.headers.common['Authorization'] = 'Auth Token';

```

## routing and the browser router object

install: npm i --save react-router react-router-dom;

- import `BrowserRouter` from the react-router-dom into app.js and wrap App around it.

```js
// index.js
import React, {Component, useRef, useContext} from 'react';
import {BrowserRouter} from 'react-router-dom';

class App extends Component{
    render(){
        return(
            <BrowserRouter>
                <div className="App">
                <Content>
                </div>
            <BrowserRouter>
        )
    }
}
```

- import `Route` from the react-router-dom package.  

```js
// index.js
import React, {Component, useRef, useContext} from 'react';
import {Route} from 'react-router-dom';
import PersonComponent from './Component/Person';

class App extends Component{
    render(){
        return(
            <Route path="/" exact render={()=>{<h1> Main Menu </h1>}} />

            <Route path="/" exact component={PersonComponent}>
        )
    }
}
```

- every path or component that is wrapped around the `BrowserRouter` component has access to the prop object that has extended functionalities
- the component approach is more preferable than the render method. still you can use the render method to display small bits of information to the dom.
- path is self explanatory.
- the exact keyword is a boolean and by definition true when unassigned. the normal start path of every url is "/". by using exact, react router will only redirect the route to paths that end with "/". if exact would be set to false, then paths like "/menu/profile" would be valid because they start with a "/".
- render() will display jsx.

## link

to prevent the app from rerendering by using anchor tags you can use the link elements from react instead.

```js
// index.js
import React, {Link} from 'react';
import {Route} from 'react-router-dom';
import PersonComponent from './Component/Person';

class App extends Component{

    <ul>
        <li><Link to='/'>Home</Link></li>
        <li><Link to={{
            pathname: 'profile',
            hash: '#edit', // will jump after redirect to #edit
            search: '?quick-submit'
        }}>Home</Link></li>
    </ul>


    render(){
        return(
            <h1>Home Page</h1>
        )
    }
}
```

- with link react will prevent the redirection from reloading the page. react will handle everything internally.
- the simplest way to switch a page is to just use the to='/path' method. if you want to pass more information into the link you can use the object method instead.

## withRouter accessing props from multiple components

```js
// index.js
import React, {Link} from 'react';
import {Route, withRouter} from 'react-router-dom';
import PersonComponent from './Component/Person';

class App extends Component{

    console.log(props);

    render(){
        return(
            <h1>Home Page</h1>
        )
    }
}

export default withRouter(App);
```

- wrapp a higher order function around the component/class to access props with more properties such as 'history, match'.

## absolute and relative path

- an absolut path is simply a path that gets appended to your domain url. for example: domain.com/profile. even if you are already on domain.com/edit, the path will be appended to the domain without taking the old path into consideration.
- a relative path on the other hand is a path that takes the old path into account and appends it onto the url chain. for example: domain.com/profile/edit.
- to create an relative path you need to do the following:

```js
// index.js
class App extends Component{

    <ul>
        <li><Link to='/'>Home</Link></li>
        <li><Link to={{
            pathname: this.props.match.url + '/profile',
            hash: '#edit', // will jump after redirect to #edit
            search: '?quick-submit'
        }}>Home</Link></li>
    </ul>


    render(){
        return(
            <h1>Home Page</h1>
        )
    }
}

export default withRouter(App);
```

## navlink styling

- in order to style navlinks you need to swap the link element with the navlink element.

```js
// index.js
import React, {NavLink} from 'react';
import {Route} from 'react-router-dom';
import PersonComponent from './Component/Person';

class App extends Component{

    <ul>
        <li><NavLink to='/' exact activeClassName="activeLinkName">Home</NavLink></li>
        <li><NavLink to={{
            pathname: 'profile'
        }}>Home</NavLink></li>
    </ul>


    render(){
        return(
            <h1>Home Page</h1>
        )
    }
}
```

- normally you would find in the google chrome inspector an active class that is automatically appended to the navlink called 'active'.
- in order to create an own class for the link elements you need to create the attribute called 'activeClassName' and give it an own class name.

## switch route and /:id

in order to prevent react from analyzing and traversing through every available route, you can simply add the switch element and wrap it around the routes

```js
// index.js
import React, {NavLink, Switch} from 'react';
import {Route} from 'react-router-dom';
import PersonComponent from './Component/Person';

class App extends Component{

    <Switch>
        <Route path="/" exact component={PersonComponent}/>
        <Route path="/:id" exact component={PersonComponent}/>
    </Switch>
}

```

- by adding a colon to the path you can pass any data forward and retrieve them with props.
- important the route order (top to bottom) is the same as in node.js.

## redirect component

in order to redirect users from one route to another you need to import the `Redirect` component from react.

```js
import React from 'react';
import {Redirect} from 'react-roter-dom';

class App extends Component{
    render(){
        return(
            <Redirect from="/" to="/posts"/>
        )
    }
}

export default App;
```

## conditional redirects

```js

import React from 'react';
import {Component} from 'react-router-dom';

class App extends Component{

    state = {
        formSubmited: false
    }

    app.get(response =>{
        this.setState({
            formSubmited: true
        })
    })


    render(){

        let redirect = null;

        if(this.state.formSubmited == true){
            redirect = <Redirect to="/posts"/>;
        }

        return(
            {redirect}
            <Route exact component={Person} />
        )
    }
}

```

## handling 404 routes

- you simply have to not define a path inside a route. the last route component without a path will catch all undefined paths.

## react lazy loading

in most cases you dont want to load every component immediately from the start. most components should be loaded when a user for example clicks on a link. in order to asynchronously load the content you need to implement react lazy loading.

```js
import React, {Route, Suspense} from 'react';

const PersonComponent = React.lazy(()=>{
    import('./Person/Person');
})

class App extends Component{
    render(){
        return(
            <React.Fragment>
                <Route path="/" render={()=>{
                    <Suspense fallback={<div>loading...(insert gif)</div>}>
                        <PersonComponent>
                    </Suspense>
                }}/>
            </React.Fragment>
        )
    }
}

```

- the personComponent should be imported dynamically with the react.lazy function.
- the component that you want to display should be wrapped with the suspence component from react.
- fallback attribute is active while the content is loading.

## basename for routing

- if you want to serve the app from for example: `domain.com/my-app`, you need to tweak it inside react. react routes normally start at "/" so: domain.com/.

```js
<BrowserRouter basename="/my-app">
    <App/>
</BrowserRouter>
```

## redux

install: npm i --save redux

- redux is basically a tool for managing a single global state inside your app.
- components dispatch actions and redux will notify subscribers for any state changes.
- reducer is basically the handler for updating the state.

## setting up a redux store

```js

const redux = require('redux');

const createStore = redux.createStore;

const initialState = {
    person: 'Olaf'
}

// reducer

const rootReducer = (state = initialState, action)=>{
    if(action.type === 'name'){
        ...state,
        return state.person;
    }

    return state;
}

// store

const store = createStore(rootReducer);

// subscription

store.subscribe(()=>{
    console.log('Changed state', store.getState());
})

// dispatch

store.dispatch({type: 'name'});
store.dispatch({type: 'lastName'})

console.log(store.getState());
```

- the store will be managed by the rootReducer so you have to pass the function into createStore.
- dispatch requires a unique type for identification in the rootReducer.
- never mutate the original state. always use the spread operator to create a copy of it and then change the values.

## importing redux to your app

in order to use redux you need to do the following:

- create a folder for the store and inside it create the reducer function.
- import the setup for redux in the index.js file.

```js

import {createStore} from 'redux';

const store = createStore(reducer);

```

## setting up redux with components

- settup the index.js file
- install: npm i --save react-redux
- you pass the store to the Provider so that all components can use it within the app.

```js
import {createStore} from 'redux';
import {Provider} from 'react-redux';

const store = createStore(reducer);

ReactDom.render(
    <Provider store={store}>
        <App/>
    </Provider>
)
```

- in the component/class you need to setup 'connect' for the subscription

```js

import {connect} from 'react-redux';

class App extends Component{
    render(){
        return(
            <Person name={this.props.ctr}/>
        )
    }
}

const mapStateToProps = state =>{
    return {
        ctr: state.name
    }
}

export default connect(mapStateToProps)(App)
```

- by having defined a state inside a reducer.js file, the connect() function will pass the reference from the store to the mapStateToProps function. this will allow you to use the ctr variable inside the class or component.
- mapStateToProps is bound to the class/component where it was defined. so ctr is only available inside the App class.
- normally you cut out the data from the store that you only need inside your class/component.

## sending a dispatch in components

```js

import {connect} from 'react-redux';

class App extends Component{
    render(){
        return(
            <Person name={this.props.ctr} clicked={()=> this.props.onSendUpdate}/>
        )
    }
}

const mapStateToProps = state =>{
    return {
        ctr: state.name
    }
}

const mapDispatchToProps = dispatch =>{
    return{
        onSendUpdate: ()=> dispatch({type: 'Update'});
    }
}

export default connect(mapStateToProps, mapDispatchToProps)(App)
```

- you pass the mapDispatchToProps() function into the connect() function. by doing that you will receive the dispatch function as an argument inside the mapDispatchToProps() function.
- onSendUpdate will be available inside the props and hence you can use it in the Person component to then trigger the function after a click event.
- the reducer file should take care of the functionalities of the onSendUpdate() function. to be specific, it should filter for the 'Update' type and then create the logic behind it.

## adding payload to dispatch function

-

```js
const mapDispatchToProps = dispatch =>{
    return{
        onSendUpdate: ()=> dispatch({type: 'Update', value: 'key'});
    }
}

//reducer.js

const rootReducer = (state = initialState, action)=>{
    if(action.type === 'name'){
        ...state,
        return state.person + action.value;
    }

    return state;
}
```

## multiple reducer

- splitting reducers into seperate files wont create in the end seperate reducers. all reducer files will be merged together into one.

## testing

- jest
- enzyme (airbnb testing tools)
- testing is used primarly for component testing (if components render the right data) and reducer logic.

install: npm i --save enzyme react-test-renderer enzype-adapter-react-16

- just practice unit tests

## deploying an app

deployment steps:

- check for the pathname (`BrowserRouter, pasename="domain.com/my-app"`)
- build and optimize (`npm run build in create-react-app project`)
- always serve index.html (even in 404 cases)
- upload build artifcats to static server

deploy:

- you can deploy your app to firebase or aws.
- simply install the firebase/aws packages and select the 'build' folder from your app that firebase/aws gonna upload to the server.
- uploading your app is managed inside your ide.

## babel for webpack

install: npm i --save-dev @babel/core @babel/preset-env @babel/preset-react @babel/preset-stage-2 babel-loader
@babel/plugin-proposal-class-properties

## webpack & babel & css

basic workflow requirements:

- compile next-gen javascript features
- hadle jsx
- css autoprefixing
- support image imports
- optimize code

install: npm i --save-dev webpack webpack-dev-server

webpack needs an configuration file to understand from where it should enter the app and what files to bundle.

```js
const path = require('path');
const autoprefixer = require('autoprefixer');

module.exports = {
    mode: 'development',
    entry: './src/index.js',
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'bundle.js',
        publicPath: ''
    },
    devtool: 'cheap-module-eval-source-map',
    module: {
        rules: [
            {
                test: /\.js$/,
                loader: 'babel-loader', // npm i babel-loader
                exclude: /node-modules/
            },
            {
                test: /\.css$/,
                exclude: /node-modules/,
                use: [
                    {loader: 'style-loader'}, // npm i style-loader
                    {
                        loader: 'css-loader', // npm i css-loader
                        options: {
                            importLoader: 1,
                            modules: {
                                localIdentName: '[name]__[local]__[hash:base64:5]'
                            }
                        }
                    },
                    {
                        loader: 'postcss-loader', // npm i postcss-loader
                        options: {
                            ident: 'postcss',
                            plugins: ()=>{[autoprefixer()]} // npm i autoprefixer
                        }
                    }
                ]
            },
            {
                test: /\.(png|jpe?g|gif)$/, // either png jpe or jpeg or gif
                loader: 'url-loader?limit=8000&name=images/[name].[ext]' // limit=8000kb/8mb file size limit, [ext] is the original extension
            }
        ]
    },
    {
        plugins: [
            new HtmlWebpackPlugin({ // npm i HtmlWebpackPlugin
                template: __dirname + '/src/index.html',
                filename: 'index.html',
                inject: 'body'
            })
        ]
    }
};
```

inside the `package.json` you need to insert the following so that the autoprefixer can work:

```js
"license": "ISC",
"browserlist": "> 1%, last 2 versions"
```

- const path will import the absolute path property which can be used to point to the folder location.
- module.rules is a babel configuration. you basically telling babel to look up for any file with the .js extension.
- modules.rules.loader is the package that will use the configuration settings.
- module.rules.exclude will exclude the node-module folder. so no code inside it will be transpiled.
- browserlist refers to a specific target browser selection, that needs to be defined for babel and css.

additional install: npm i file-loader

deploy for production:

- create a copy of `webpack.config.js` and rename it to `webpack.config.prod.js`
- change `mode` to `production` and `devtool` to `none`
- in the package.json file create a new entry inside scripts called: `"build:prod": "webpack --config webpack.config.prod.js"`
- if you run npm build:prod, webpack will create a `dist` folder ready to use for production.


