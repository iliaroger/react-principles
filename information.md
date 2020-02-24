# javascript topics 
## spread and rest operator

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

# react principles

## importing 

```js
import {combine} from './combine'
import {combine as bine} from './combine'
import {* as group} from './combine'
```

## creating a react project

- npm install -g create-react-app
- npx create-react-app appname


# styling react components

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

# debugging react apps

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

2. import the component and wrap it around where an error might occur 

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

# components and react internals in detail 

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
    <div classes={props.classes}>{props.children}</div>
}

export default classWrapper;

// app.js
import React, {Component} from 'react';
import styles from './styles/classStyle.css'
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

```







