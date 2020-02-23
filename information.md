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







