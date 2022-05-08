

# The Road To Learn React

Notes started on 11/8/19

I will also make notes on the React Docs later on.

## Hi, my name is React

Single page applications have become increasingly popular. Angular, Ember, and Backbone were predecessors to React. React was released by Facebook's web dev team as a view library as the V in MVC. As a view it allows you to render viewable components. Its ecosystem lets us build single page applications. ***React is only used to build your view layer***. Its is a library of reusable components.

In React, the focus remains on the view layer until more aspects are introduced to the application. These are the building blocks for an SPA. This comes with two advantages:

1. You can learn the building blocks one at a time without having to understand them all together. a SPA framework gives you every building block from the start. This books focuses on React as the first building block.
2. All building blocks are interchangeable. This makes the ecosystem of React very dynamic. You can use the most appealing solution to suit your needs.

### File structure of a typical React App

1. Readme.md
2. node_modules - This folder contains all of the node packages that have been installed by npm. You will rarely touch this folder as it is generally manipulated by the npm command line
3. package.json - This file shows you a list of node package dependencies and other project configurations.
4. public - This folder holds all development files such as index.html. The index is displayed at localhost:3000 when developing your app. The boilerplate takes care of relating this index with all of the scripts in src
5. build - This folder is created when you build for production and holds all the production files. All of the source code from src and public are bundled and placed into the built folder.
6. manifest.json and serviceWorker.js : These files wont be used for this project

The main focus lies on the src/App.js file which is used to implement React components. Later on you can split your components into multiple files. Src/App.test.js is for all of your tests. src/index.js is an entry point to the React world. src/index.css and src/App.css are used to style your application.

***Commands to know:***

***npm start*** - Runs the application in http://localhost:3000

***npm test*** - Runs the tests

***npm run build*** - Builds the application for production

These scripts are defined in your package.json file.

## Intro to JSX

***JSX is the syntax of React.***

This is how to declare a component.

```javas
import React, { Component } from 'react';

//Declaration
class App extends Component  {...}

//Instantiation
<App />
```

After you have declared a component you can use it as an element anywhere in your app. It will produce an ***instance*** of your ***component***. It gets ***instantiated***.

The returned ***element*** is specified in the render() method. ***Components are made up of elements.*** You can instantiate the App element anywhere in your JSX with <App />.

The content in the render block may look similar to HTML but it is actually JSX. JSX allows you to mix html and javascript.

Lets work with variables in plain html:

```javascript
import React, { Component } from 'react';
import './App.css';
class App extends Component {
	render() {
	var helloWorld = 'Welcome to the Road to learn React';
	return (
		<div className="App">
			<h2>{helloWorld}</h2>
		</div>
		);
	}
}	

export default App;
```



### ES6 const and let

ES6 comes with a few more ways to declare variables: ***const*** and ***let***. You rarely use var anymore. Let can be modified and const cant.

### ReactDOM

The app component is located in your entry point to the React world: src/index.js

```javascript
ReactDOM.render(
	<App />,
	document.getElementById('root')
);
```

ReactDOM.render() uses a DOM node in your HTML to replace it with JSX. Its a way to integrate React in any foreign applications easily. You can use ReactDOM.render() multiple times across your app. You can use it to bootstrap simple JSX syntax, a React component, several components, or the entire app. In a plain React App you would only use it once to bootstrap the component tree.

ReactDOM.render() expects two arguments. The first argument is for rendering JSX. The second argument specifies the place where React applications hook into your HTML. It expects an element with id='root' found in the public/index.html file

```javascript
ReactDOM.render(
	<h1>Hello React World</h1>,
    document.getElementbyId('root')
);

//This is technically a function call
```

### Hot Module Replacement

By default, create-react-app will cause the browser to refresh every time the source is changed. However, Hot Module Replacement is a tool for reloading your application in the browser without page refresh. You can activate it by adding the following to your src/index.js file:

```javascript
if(module.hot) {
    module.hot.accept()
}
```

###  Complex Javascript in JSX

Now we can render a list of items. Use this syntax to render a list of elements:

```javas
const list = ['a;ksdj']

class App extends Component {
	render() {
		return (
			<div className="App">
			{list.map(function(item){
				return <div>{item.title}</div>
			})}
			</div>
		);
	}
}
```

By assigning a key attribute to each list element, React can identify modified items when the list changes:

```javascript
{list.map(function(item) {
    return (
    	<div key={item.objectID}>
         ... Other Html here
    );
})}
```

### ES6 Arrow Functions

```javas
function () { ... }

becomes...

() => { ... }
```

### ES6 Classes

An example of a class:

```javas
class Developer {
	constructor(firstname, lastname) {
		this.firstname = firstname;
		this.lastname = lastname;
	}
	
	getName() {
		return this.firstname + ' ' + this.lastname;
	}
}
```

A function associated with a class is called a ***method***.

The Component class is used to extend a basix es6 class to a Es6 component. Methods exposed by a React Component are its public interface. One of these methods must be overriden. Generally the render() method must be overriden.

## Basics in React

### Local Component State

Also known as internal state, local component state allows you to save, modify, and delete properties stored in your component. The constructor is called only once by React to construct initial local state. 

A class constructor:

```javascript
class App extends Component {
    constructor(props){
        // It is MANDATORY TO CALL SUPER(PROPS)!!!
        super(props);
    }
}
```

Set some state in the class:

```javascript
const list = [{title: 'toot'}]

class App extends Component {
    constructor(props) {
        super(props);
    }
    //Setting state like this is sufficient to create local state with this field
    this.state = {
        list: list
    }
}
```

### ES6 Object Initializer

There is a shorthand property syntax to initialize your objects more concisely: 

``` javascript
const name = 'Joe'
const user = {
    name: name;
}
//When the property 'name' has the same name as the field then you can do this:
const user = {
    name,
}
```

You can also initialize methods in an object more concisely:

```javascript
const userService = {
    getUserName(user) {
        return user.firstname + ' ' + user.lastname
    }
}
```

You can also use computed property names:

```javascript
const key = 'name'
const user = {
    [key]: 'Robin'
}
```

### Unidirectional Data Flow

The local state and its component are static. A good way to experience state manipulation is to engage in component interaction.

```html
render() {
	...
	{this.state.list.map(item =>
<span>
    <button onClick={() =>
        //This onDismiss function must be defined in a component
        this.onDismiss(item.objectID)
    </button>}
    </spans>)}
	...
}
```

Here is how to implement that function:

```javascript
class App extends Component {
    constructor(props) {
        super(props);
        this.state = {
            list,
        }
        this.onDismiss = this.onDismiss.bind(this);
    }
   	render(){//This is where than HTML is defined};
    //Now we implement the function onDismiss
    onDismiss(id) {
        ...
    }
}
```

This function returns a new list instead of updating the old one:

```javascript
const words = ['spray', 'toottoot']
onDismiss(id) {
    const updatedList = this.state.list.filter(function isNotID(item) {
    	return item.objectID != id;
	})
}
//This is another way to write an anonymous filter function
const isNotId = item => item.objectId !== id;
const updatedList = this.state.list.filter(isNotId);
// Finally we must update the state by calling the class method setState{}
this.setState({list: updatedList});
```

What we just did is called unidirectional data flow!

### Bindings

Binding is a necessary step because class methods don't automatically bind this to the class instance. Thus you must do it explicitly.

```javascript
class ExplainBindingsComponent extends Component {
    
    constructor(){
        super();
        // This is binding the method to the instance
        this.onClickMe = this.onClickMe.bind(this);
    }
    
    onClickMe() {
        console.log(this);
    }
    
    render() {
		return (
			<button
				onClick={this.onClickMe}
				type="button"
			>
				Click Me
				</button>
		);
	}
}
```

### Event Handler

Javascipt embedded in HTML is evaluated immediately when the browser loads the page. Therefore it is necessary to abstract a piece of functionality one level to be able to have an element call it.

```javascript
class ExplainEventHandler extends Component {
    constructor(){...}
    onClickMe(){...}
    render(){
        return(
        	<button>
            	//You must use a second tier function call like an arrow function
            	onClick = {() => this.onDismiss(item.objectID)}
            </button>
        );
    };
}
```

### Interactions With Forms and Events

```javascript
class App extends Component {
    constructor(props){
        super(props);
        this.state = {
            list,
            searchTerm: '', //This is the search term for the onSearchChange event
        }
        this.onSearchChange.bind(this);
    }
    //This is a synthetic event:
    //event is an object with a target field
    //target is an object with a value field
    onSearchChange(event){
        this.setState({searchTerm: event.target.value });
    }
    
    //This is the boolean-returning function that is used to filter the items
    function isSearched(searchTerm){
        return function (item){
            return item.title.toLowerCase().includes(searchTerm.toLowerCase());
        }
    }
    
    render(){
        return(
        	<div className="App">
            	<form>
            		<input type="text"
            		onChange={this.onSearchChange}>
            	</form>
				//This is the call that filters based on the search term
				{this.state.list.filter(isSearched(this.state.searchTerm)).map(item =>
				...
				)}
            </div>
        );
    }
    
}
```

A ***higher-order function*** is a function that returns another function that is contingent on the inputs to the original function.

### ES6 Destructuring

```javasc
const user = {
	firstName = "Jared";
	lastName = "Nelsen";
}

// ES5
var firstname = user.firstname;
var lastname = user.lastname;

console.log(firstname + " " + lastname)
//output Jared Nelsen

// ES6
const {firstname, lastname} = user;

console.log(firstname + " " + lastname);
//output Jared Nelsen
```

### Controlled Components

An uncontrolled component is one that is outside the state of React like an HTML field.

We can control these states with React:

```javascript
render(){
    const {searchTerm, list} = this.state;
    return (
    	<div>
        	<form>
        		<input>
        		  //Here we add a value tag that is set to the JS state in this Component
        		  type = "text" value = {searchTerm} onChange = {this.onSearchChange}
        		</input>
        	</form>
        </div>
    );
}
```

### Split Up Components

So far we have been working on only one monolithic component. We can and should split things up by using more components.

```javascript
render(
	const { searchTerm, list} = this.state;
	return (
		<div className = "App">
			<Search />
			<Table />
		</div>
	);
);
```

We pass the properties of the components down the tree.

```javascript
render() {
    const {searchTerm, list} = this.state;
    return (
    	<div className ="App">
        	<Search 
        		value = {searchTerm};
				onChange = {this.onSearchChange};
        	/>
			<Table
				list = {list}
				pattern = {searchTerm}
				onDismiss={this.onDismiss}	
			/>
        </div>
    );
};
```

Now we define the sub components either in the same file or in another one.

```javascript
class App extends Component {
    ...
}
    
//This is the level we define the other components at
class Search extends Component {
    render(){
        const {value, onChange} = this.props;
        return (
        	<form>
            	<input
            		type = "text"
            		value = {value}
    				onChange= {onChange}
            	>
            </form>
        );
    };
}
```

We would also define the Table component and pass the props down with it.

Notice that props is a global instance to its class. Thus, state is managed in a tree fashion.

Now that we have broken the components out it is easy to see that we can use them in different ways by passing down different parameters.

### Composable Components

The **children** prop is used to pass elements down to components.

You can pass a text string like this:

```javascript
...
//Composing App component
<Search
	value = {searchTerm}
	onChange = {this.onSearchChange}
	Search //This is the string we are passing
>
...
//In th Search Component
...
render(){
	const {value, onChange, children} = this.props;
    return (
    	<form>
        	{children}
        	<input type= "text" value= {value} onChange= {onChange}>
        </form>
    );
};
    
```

The word "Search" is now visible next to the input field. You can pass basically anything as children including other elements or element trees.

### Reusable Components

```javascript
class Button extends Component {
    render(){
        const {onClick, className, children} = this.props;
        
        return (
        	<button
            	onClick={onClick} className= {className} type = "button">
            {children}
			</button>
        );
    };
}
```

Now that we have defined a button we can use it in a component and pass functionality:

```javascript
...
return(
	...
    <Button onClick={() => onDismiss(item.objectID)}>
    	Dismiss //This child is the button text
	</Button>
    ...
);
...
```

Notice that we did not pass a classname with the children. We can get away with this if we specify default values in the object destructuring:

```javascript
const {
    onClick,
    className = '', //This is how to speficy a default value
    children
} = this.props;
```

### Component Declarations

We can improve our components using a stateless strategy. First lets define some things:

- ***Functional Stateless Components*** are components that take input and return an output. The inputs are props and the output is a component instance in plain JSX. Functional stateless components ***have no state***. They also do not have lifecycle methods like render() and constructor(). 
- ***ES6 Class Components*** extend from the React component. The extend keyword hooks all the lifecycle methods to the components. You can also store and manipulate state in ES6 classes with this.state and this.setState().
- ***React.createClass*** was used in older versions of React.

***Functional vs ES6 class components rule of thumb*** -

	- Use functional stateless components when you dont need local state or lifecycle methods.

Refactoring the Search component into a functional stateless component:

```javascript
//Notice that we are changing it to a function and sending in the props as a parameter
function Search(props) {
    const {value, onChange, children} = props; //Notice that props is used instead of this.props
 	return (
    	<form>
        {children} <input type = "text" value = {value} onChange= {onChange}>
        </form>
    );   
}
```

The best practice is to use the function signature to destructure the props:

```javascript
function Search({value, onChange, children}){
    
    //You can do something here
    
    return (
    	<form>
        	{children} <input type = "text" onChange={onChange} value={value}>
        </form>
    );
}
```

We can further simplify things by using ES6 arrow functions: (Use this if it is pure enough as to have no side effects. i.e. we dont do anything locally.)

```javascript
const Search = ({value, onChange, children}) => 
		//Note that this eliminates the return statement
	    <form>
        	{children} <input type = "text" onChange={onChange} value={value}>
        </form>
```

### Styling components

We style React components just like regular HTML with CSS: we add corresponding style tags and classNames in the HTML.

## Getting Real With APIs

### Lifecycle Methods

**Mounting** is when a component is intantiated and inserted into the DOM. The **render** method is called during the mount process and when the component updates. Every time the state or the props of a component changes the render() method is created.

***getDerivedStateFromProps()** is called before the render() method. The **componentDidMount()** after the render() method.

This is the **mounting lifecycle method call order**:

1. constructor()
2. getDerivedStateFromProps()
3. render()
4. componentDidMount()

For the **update lifecycle** when the state or the props change there are 5 lifecycle methods:

1. getDerivedStateFromProps()
2. shouldComponentUpdate()
3. render()
4. getSnapshotBeforeUpdate()
5. componentDidUpdate()

For the last lifecycle, the **unmounting lifecycle**, there is only one method

1. componentWillUnmount()

Here is a detailed explanation of all these methods:

1. ***constructor(props)*** - Called when the component gets  initialized. You can set an initial state and bind class methods during this call.
2. ***getDerivedStateFromProps(props, state)*** - Called before the render() lifecycle method. Returns an object to update the state or null to update nothing. It is a static method without access to the instance.
3. ***render()*** - A mandatory metod that returns elements as an output of the component. It should be pure, without modifying state. It gets an input as props and state and returns an element.
4. ***componentDidMount()*** - Called once when the component is mounted. **This is a perfect time to do an asynchronous request to fetch data from an API. The fetched data is stored in the local component state to display it in the render() method.
5. ***shouldComponentUpdate(nextProps, nextState)*** - Always called when the component updates due to state or props changes. Generally used in mature React Apps for optimization. You can use it to prevent the render() lifecycle method propogating down through the tree.
6. ***getSnapshotBeforeUpdate(prevProps, prevState)*** - Invoked before the most recently rendered output is rendered to the DOM. Enables a component to capture information from the DOM before it is potentially changed. componentDidUpdate() will receive and value returned by this method as a parameter.
7. ***componentDidUpdate(prevProps, prevState, snapshot)*** - Invoked immediately after update but not after initial render. You can use it to perform DOM operations or to perform more asychronous requests. If your component implements this method, the value it returns will be received as the snapshot parameter in getSnapshotBeforeUpdate().
8. ***componentWillUnmount()*** is called before you destroy your component. You can use this method to do any cleanup tasks.

In a component the render() method is required to return a component instance.

### Fetching Data

To fetch data we use the ***componentDidMount()*** method. Let's do some setup for our URL.

```javascript
const DEFAULT_QUERY = 'redux';
const PATH_BASE = 'www.somewebsite.com/api';
const PATH_SEARCH = '/search';
const PARAM_SEARCH = 'query=';

// In ES6 you can use template literals to concatenate strings
const url = `${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${DEFAULT_QUERY}`;

class App extends Component {
   constructor(props){
       super(props);
       this.state = {
           result: null,
           searchTerm: DEFAULT_QUERY
       }
       
       //Dont forget to bind the new method we are creating
       this.setSearchTopStories = this.setSearchTopStories.bind(this);
   }
    
    //This new method is how we set the state retrieved from the query
    setSearchTopStories(result) {
        this.setState({result});
    }
    
    //Here is were we do the fetching
    componentDidMount(){
        const {searchTerm} = this.state;
        
        fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}`)
		.then(response => response.json())
		.then(result => this.setSearchTopStories(result))
		.catch(error => error);
    }
}
```

The intial state now has an empty result and a default search term set in the state. componentDidMount() fetched the data after the component mounted.

The native (to the browser) fetch API is used. The URL is the argument and the response is transformed to JSON and then it is set to the state.

Now we create the render() method:

```javascript
render() {
    const {searchTerm, result} = this.state;
    
    if(!result) {return null; };
    
    return (
		<div className="page">
		...
		<Table
			list={result.hits}
			pattern={searchTerm}
			onDismiss={this.onDismiss}
		/>
	</div>
}
```

Steps that happened:

1. The component is intiatlized by the constructor and it renders for the first time
2. We prevented it from displaying anything because the local state is null
3. The componentDidMount() method runs and fetches the data from the API asynchronously
4. Once the data arrives it changes your local component state in our custom method
5. The update lifecycle runs again because we updated the state
6. The component runs the render() method again, this time with the populated result in your local component state
7. The component and the Table are rendered

### ES6 Spread Operators

```javascript
//Dont do this to set the state
this.state.result.hits = updatedHits;
```

React embraces immutable data structures so **never mutate an object**. ***Instead, generate a new object.***

To generate a new object from an old one we use ES6's *Object.assign()*. It takes a target object as the first argument and all following arguments are source objects, which are merged into the target object. Source objects will override previously merged objects with the same property names.

```javascript
const updatedHits = { hits: updatedHits }; //Create the object to set the state to
//Send all of the state into a new object
const updatedResult = Object.assign({}, this.state.result, updatedHits); 
```

A ***spread operator*** is a simpler way to copy every value from an array or object to another array or object. It is signified with the '...' operator.

An array spread operator:

```javascript
const userList = ['jared', 'Brad']
const additionalUser = 'Jordan';
const allUsers = [ ...userList, additionalUser]
// Now allUsers is (note that this is a completely new array)
['Jared', 'Brad', 'Jordan']
```

This is the same for objects:

```javascript
const userNames = {first: "Jared", last: "Nelsen"};
const age = 28;
const user = {...userNames, age};
```

The spread operator can be used to replace Objects.assign();

```javascript
this.setState({result: { ...this.state.result, hits: updatedHits}});
```

### Client Side Cache

You can cache search results in memory by storing them in the state of a component and doing a list diff when it is retrieved.

## Advanced React Components

### Ref a DOM Element

The **ref attribute** gives you access to a node in your elements. Warning! This is generally an antipattern in React but there are special cases when you might need to do this:

1. To use the DOM API
2. To Invoke imperative DOM node animations
3. To integrate with a third party library that needs the DOM node

This is how to focus on an input field:

```javascript
//This is the input component we are referencing

<input
type="text"
value={value}
onChange={onChange}
ref={el => this.input = el} //This is using the ref of the DOM node
/>

//This is how to focus it
   	class Search extends Component {
        //Use component did mount to call focus after component mounts
        componentDidMount(){
            if(this.input){
                this.input.focus();
            }
        }
    }
```

### Loading...

Let's build a loading component:

```javascript
const Loading = () => <div>Loading...</div>
```

Then we can store a property:

```javascript
//In the App component
constructor(props){
    super(props);
    this.state = {
        ...
        isLoading: false, // Add the property with a default value
    }
}
```

Then you can set the loading variable in calls to setState().

```javascript
this.setState({isLoading: true});
```

Then you can show and hide the Loading component with ternary statements:

```javascript
{isLoading ? <Loading />} : <Button>...This is a button that is being ommitted</button>
```

### Higher Order Components

Higher order components, HOCs, are equivalent to higher order functions. They take any input, usually a component, but also optional arguments and return a component as an output. Generally, they are used to enhance existing components.

Here is an example of enhancing a component on the fly: (generally prefixed name with 'with'):

```javascript
const withLoading = (Component) => (props) =>
	props.isLoading ? <Loading /> : <Component {...props}>
```

Notice we spread the props forward with '...'.

## State Management in React

### Lifting State

So far only the App component of the app is stateful. We pass properties to subcomponents to manage state there.

Moving substate from one component to another is known as **lifting state**. 

To add state to a stateless object you must:

1. Add a constructor
2. Add initial state

### setState() revisited

We have already seen how to set a state by calling:

```javascript
this.setState({value: 'hello'})
```

However, we can also pass a function to it to set the state:

```javascript
this.setState(() => {
    ...
});
```

There is a crucial case where it makes sense to use a function over an object: when you update the state based on the previous state or props. If you don't use a function, the local state management can cause bigs. 

### Taming the State

As applications get bigger the challenge is to control and tame the state. React achieves this with unidirectional data flow and a simple API to manage state in components. **It is possible to introduce bugs when an object is passed over a function in setState().** Redux is an example of a state management solution.

***Growing applications is equivalent to scaling state!***

# FIN

