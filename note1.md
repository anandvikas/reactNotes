# React Tutorials
## Functional components
Properties (props) => Functional Components => HTML (jsx)

> index.js

    import  reactDom  from  'react-dom'
    import  App  from  "./App.js";
    reactDom.render(<App/>, document.getElementById('root'))

> App.js

    const  App = () => {
	    return (
		    <h1>Hello world !</h1>
	    )
    }
    export  default  App;

## Class component

> App.js 

     import {Component} from  'react'
        class  App  extends  Component {
    	    render(){
    		    return (
    			    <h1>Hello world!</h1>
    		    )
    	    }
        }
    export  default  App;
## JSX
 1. JavaScript XML (JSX) -is an extension to the JavaScript syntax
 2. JSX is not neccessary to write React code. But it s used to make it more readable and easy to write.
 3. JSX is ultimately transpiles to pure javascript language with help of '**babel**'

> With using JSX

    const  App =() => {
	    return (
		    <div  className='class1'>
			    <h1>Hello world!</h1>
		    </div>
	    )
    }
    export  default  App;
*in JavaScript 'class' is a reserved keyword hence, we use 'className' to give class in HTML*

> Without using JSX

    import {createElement} from  'react'
    const  App =() => {
	    return (createElement('div', {className:'class1'}, createElement('h1', null, 'Hello world!')))
    }
    export  default  App;

## Props
### Props in functional components --
Props are properties that are used to send data from parent component to child component. In parent component It is passed like an attribute. And in child component it is received line a parameter. Props are Immutable.

> App.js

    import  Comp1  from  './comp1.js'
    const  App = () => {
	    return (
	    <div>
		    <Comp1  name='Peter'  />
		    <Comp1  name='Tony'  />
		    <Comp1  name='Steve'  />
	    </div>
	    )
    }
    export  default  App;

> comp1.js

    const  Comp1 = (props) => {
	    return (
		    <div>
			    <h1>{props.name}</h1>
		    </div>
	    )
    }
    export  default  Comp1;
***to access child elements in props***

> App.js

    import  Comp1  from  './comp1.js'
    const  App = () => {
	    return (
		    <div>
			    <Comp1  name='Peter'>
				    <p>aka Spiderman</p>
			    </Comp1>
			    <Comp1  name='Tony'>
				    <button>aka Ironman</button>
			    </Comp1>
			    <Comp1  name='Steve'>
				    <h3>aka Captain</h3>
			    </Comp1>
		    </div>
	    )
    }
    export  default  App;

> comp1.js

    const  Comp1 = (props) => {
	    return (
		    <div>
			    <h1>{props.name}</h1>
			    {props.children}
		    </div>
	    )
    }
    export  default  Comp1;
    
### Props in functional components --
*in class component properties are available through this keyword*

> App.js

    import  Comp2  from  './comp2.js'
    const  App = () => {
	    return (
		    <div>
			    <Comp2  name='Peter'>
				    <p>aka Spiderman</p>
			    </Comp2>
			    <Comp2  name='Tony'>
				    <button>aka Ironman</button>
			    </Comp2>
			    <Comp2  name='Steve'>
				    <h3>aka Captain</h3>
			    </Comp2>
		    </div>
	    )
    }
    export  default  App;

> comp2.js

    import { Component } from  'react'
    class  Comp2  extends  Component {
	    render() {
		    return (
			    <div>
				    <h1>{this.props.name}</h1>
				    {this.props.children}
			    </div>
		    )
	    }
    }
    export  default  Comp2;
## State
|Props| State |
|--|--|
|Props are passed to the component |State is managed within the component.  |
|Received from parent component. |Variable declared within the body.|
|Props are immutable. |State can be changed.|
|props - functional component |useState hook - functional component|
|this.props - class component |this.state - class component|

***this set of code increase the number by 1 on every click of button.***
> App.js

    import  Comp2  from  './comp2.js'
    const  App = () => {
	    return (
		    <div>
			    <Comp2  />
		    </div>
	    )
    }
    export  default  App;

> comp2.js

    import { Component } from  'react'
    class  Comp2  extends  Component {
	    constructor() {
		    super()
		    this.state = {
			    number : 1
		    }
	    }
	    changeNum(){
		    this.setState({
			    number : this.state.number + 1
		    })
		    console.log(this.state.number)
	    }
	    render() {
		    return (
			    <div>
				    <h1>{this.state.number}</h1>
				    <button  onClick={()=>  this.changeNum()}>Increase</button>
			    </div>
		    )
	    }
    }
    export  default  Comp2;
however in above code the value in console log is generally one number less then the value in DOM. This is because the **setState** method works asynchronously. To solve this problem we can give a callback function in setState as a second parameter.  Which will run only after the task is completed.

    ...
    changeNum(){
	    this.setState({
		    number : this.state.number + 1
	    }, ()=>{console.log(this.state.number)})
    }
    ...

*IF we modify the state without using setState method, then the state would be be modified but the component will not re-rendered.*

for exmple the following code will only increase number in console but not in DOM.

    ...
    changeNum(){
	    this.state.number = this.state.number + 1
	    console.log(this.state.number)
    }
    ...
**Previous state in setState method**
the following code will not increase the number by 5 in one click.

    import { Component } from  'react'
    class  Comp2  extends  Component {
	    constructor() {
		    super()
		    this.state = {
			    number : 1
		    }
	    }
	    changeNum(){
		    this.setState({
		    number : this.state.number + 1
		    }, ()=>{console.log(this.state.number)})
	    }
	    increaseFive(){
		    this.changeNum()
		    this.changeNum()
		    this.changeNum()
		    this.changeNum()
		    this.changeNum()
	    }
	    render() {
		    return (
			    <div>
				    <h1>{this.state.number}</h1>
				    <button  onClick={()=>  this.increaseFive()}>Increase</button>
			    </div>
		    )
	    }
    }
    export  default  Comp2;
this is because React tries to handle multiple setState in a single go to optimise the performance. To solve this problem we can use an inbuilt argument which contain the previous value of the state.

    ...
    changeNum(){
	    this.setState((prev)=>{
		    return {number :  prev.number + 1}
	    }, ()=>{console.log(this.state.number)})
    }
    increaseFive(){
	    this.changeNum()
	    this.changeNum()
	    this.changeNum()
	    this.changeNum()
	    this.changeNum()
    }
    ...

**Do's and Dont's of state**

 - Never modify the state directly. Always use 'setState' method.
 - setState is asynchronous. So use callback function as second parameter (if required)
## Destructuring in Props and States
### destructuring - funcional components

> App.js

    import  Comp1  from  './comp1.js'
    const  App = () => {
    return (
    <div>
    <Comp1  fname='Vikas'  lname='Anand'  />
    <Comp1  fname='Vishal'  lname='Anand'  />
    <Comp1  fname='Vivek'  lname='Anand'  />
    </div>
    )
    }
    export  default  App;

> comp1.js

    const  Comp1 = ({fname, lname}) => {
	    return (
		    <div>
			    <h1>Name : {fname} | last name : {lname}</h1>
		    </div>
	    )
    }
    export  default  Comp1;
OR

    const  Comp1 = (props) => {
	    let {fname, lname} = props
	    return (
		    <div>
			    <h1>Name : {fname} | last name : {lname}</h1>
		    </div>
	    )
    }
    export  default  Comp1;
### destructuring - class components

> App.js

    import  Comp2  from  './comp2.js'
    const  App = () => {
	    return (
		    <div>
			    <Comp2  fname='Vikas'  lname='Anand'  />
			    <Comp2  fname='Vishal'  lname='Anand'  />
			    <Comp2  fname='Vivek'  lname='Anand'  />
		    </div>
	    )
    }
    export  default  App;

> comp2.js

    import { Component } from  'react'
    class  Comp2  extends  Component {
	    render() {
		    let {fname, lname} = this.props
		    return (
			    <div>
				    <h1>Name : {fname} | last name : {lname}</h1>
			    </div>
		    )
	    }
    }
    export  default  Comp2;
	## Event binding
event binding is done to counter the problems related to this keyword in class component.
**problem**

    class  Comp1  extends  Component {
    constructor(){
    super()
    this.state = {
    message :  'Good Morning'
    }
    }
    changed(){
    this.setState({
    message :  'Good Afternoon'
    })
    }
    render() {
    return (
    <>
    <h1>{this.state.message}</h1>
    <button  onClick={this.changed}>click</button>
    </>
    )
    }
    }
    export  default  Comp1;
*error comes out*

> Uncaught TypeError: Cannot read properties of undefined (reading
> 'setState')

**solution**
Sol1 : convert the regular function into arrow function.

    class  Comp1  extends  Component {
    ...
    changed = () => {
    this.setState({
    message :  'Good Afternoon'
    })
    }
    render() {
    return (
    <>
    <h1>{this.state.message}</h1>
    <button  onClick={this.changed}>click</button>
    </>
    )
    }
    }
    export  default  Comp1;

Sol2 : use of bind method

    class  Comp1  extends  Component {
    ...
    changed(){
    this.setState({
    message :  'Good Afternoon'
    })
    }
    render() {
    return (
    <>
    <h1>{this.state.message}</h1>
    <button  onClick={this.changed.bind(this)}>click</button>
    </>
    )
    }
    }
    export  default  Comp1;
Sol3 : use arrow function in calling

    class  Comp1  extends  Component {
    ...
    changed(){
    this.setState({
    message :  'Good Afternoon'
    })
    }
    render() {
    return (
    <>
    <h1>{this.state.message}</h1>
    <button  onClick={()=>this.changed()}>click</button>
    </>
    )
    }
    }
    export  default  Comp1;
Sol4 : Bind in constructor

    class  Comp1  extends  Component {
    constructor(){
    super()
    this.state = {
    message :  'Good Morning'
    }
    this.changed = this.changed.bind(this)
    }
    changed(){
    this.setState({
    message :  'Good Afternoon'
    })
    }
    render() {
    return (
    <>
    <h1>{this.state.message}</h1>
    <button  onClick={this.changed}>click</button>
    </>
    )
    }
    }
    export  default  Comp1;
***sol : 1 ; sol : 4 is mostly prefered for optimised performance.***

## Passing method as a prop

> App.js

    import { Component } from  "react";
    import  Comp1  from  "./components/comp1";
    class  App  extends  Component {
    constructor(){
    super()
    this.parentMethod = this.parentMethod.bind(this)
    }
    parentMethod(name){
    console.log(name)
    }
    render () {
    return (
    <Comp1  parentMethod={this.parentMethod}/>
    )
    }
    }
    export  default  App;

> Comp1.js

    import { Component } from  "react";
    class  Comp1  extends  Component {
    render() {
    return (
    <>
    <button  onClick={()=>{this.props.parentMethod('child')}}>click</button>
    </>
    )
    }
    }
    export  default  Comp1;
    
## conditional rendering
**using if-else**

    class  Comp1  extends  Component {
    constructor(){
    super()
    this.state = {
    isShow :  true
    }
    }
    render() {
    if(this.state.isShow){
    return (
    <h1>Show</h1>
    )
    } else {
    return (
    <h1>hide</h1>
    )
    }
    }
    }
    export  default  Comp1;
***Using ternary operator*** 

    class  Comp1  extends  Component {
    constructor(){
    super()
    this.state = {
    isShow :  false
    }
    }
    render() {
    return(
    this.state.isShow ? <h1>Show</h1> : <h1>Hide</h1>
    )
    }
    }
    export  default  Comp1;
***Using logical operator*** *: but it will return nothing if first condition is false.*

    class  Comp1  extends  Component {
    constructor(){
    super()
    this.state = {
    isShow :  true
    }
    }
    render() {
    return(
    this.state.isShow && <h1>Show</h1>
    )
    }
    }
    export  default  Comp1;
## list rendering

    import { Component } from  "react";
    class  Comp1  extends  Component {
    arr = ['vikas', 'vishal', 'vivek', 'vipul']
    render() {
    return(
    this.arr.map(val=><h1>{val}</h1>)
    )
    }
    }
    export  default  Comp1;
## key prop in list rendering

 - it is unique string attribute for each list item
 - value of key prop cannot be accessed in receiving child like other prop values.
 - It gives the items a stable identity.
 - It helps react to identify which item have changed, updated or removed. And helps in efficient update of user interface.

**When to use index as key**

 - when items of key are static (not changing)

## Basics of form handling

> App.js

    import { Component } from  'react'
    class  App  extends  Component {
	    constructor() {
		    super()
		    this.state = {
			    formData: {
				    input1: '',
				    input2: ''
			    }
		    }
	    }
	    updateFormData = (e) => {
		    this.setState({
			    formData: { ...this.state.formData, [e.target.name]:  e.target.value }
		    })
	    }
	    submitForm = (e) => {
		    e.preventDefault()
		    alert(`${this.state.formData.input1}\n${this.state.formData.input2}`)
	    }
	    render() {
		    return (
			    <>
				    <form  onSubmit={this.submitForm}>
					    <input  type='text'  name='input1'  value={this.state.formData.input1}  placeholder='input 1'  onChange={this.updateFormData}  /><br  />
					    <input  type='text'  name='input2'  value={this.state.formData.input2}  placeholder='input 2'  onChange={this.updateFormData}  />
					    <input  type='submit'  value='Submit'  />
				    </form>
			    </>
		    )
	    }
    }
    export  default  App;
## component lifecycle method
| Lifecycle | Description | Methods|
|--|--|--|
| Mounting | When instance of component is created and inserted into DOM | constructor, getDerivedStateFromProps, render, componentDidMount|
| Updating| When component is re-rendered (change in prop or state etc. ) | getDerivedStateFromProps, shouldComponentUpdate, render, getSnapShotBeforeUpdate, componenDidUpdate|
| Unmounting| When a component is removed from DOM | componentWillMount|
| Error Handling| when there is any error in any lifecycle method or in constructor of any child component  | getDerivedStateFromError, componentDidCatch|

### 1. Mounting Lifecycle Methods

> sequence

| sequence | method | purpose|
|--|--|--|
| 1 | constructor() | called whenever a component is created.Initialise states and bind events.|
|2| getDerivedStatesFromProps(props, state)| this is called when the state of the component depends on changes in props over time.|
|3| render()| read props and state and return JSX, Lifecycle methods of child component is also called in this method.|
|4| componentDidMount()| invoked immediatly after the component and all its child component are loaded.|

> App.js

    import { Component } from  'react'      
    
    import  LifeCycle1  from  './lifecycle1';
    class  App  extends  Component {
	    render () {
		    return (
			    <LifeCycle1/>
		    )
	    }
    }
    export  default  App;

> lifecycle1.js

    import  React, { Component } from  'react'
    import  LifeCycle2  from  './lifecycle2'  
    
    export  class  LifeCycle1  extends  Component {
    // constructor METHOD
    constructor(props) {
	    super(props)
	    this.state = {
		    name: 'vikas'
	    }
	    console.log('Lifecycle1 : constructor')
    }
    // getDerivedStateFromProps METHOD
    static  getDerivedStateFromProps(props, state) {
	    console.log('Lifecycle1 : getDerivedStateFromProps')
	    return  null;
    }
    // componentDidMount METHOD
    componentDidMount() {
	    console.log('Lifecycle1 : componentDidMount')
    }
    // render METHOD
    render() {
	    console.log('Lifecycle1 : render')
	    return (
		    <LifeCycle2  />
	    )
    }
    }
    export  default  LifeCycle1

> lifecycle2.js

    import  React, { Component } from  'react'  
    
    export  class  LifeCycle2  extends  Component {
    // constructor METHOD
    constructor(props) {
    super(props)
    this.state = {
    name: 'vikas'
    }
    console.log('Lifecycle2 : constructor')
    }
    // getDerivedStateFromProps METHOD
    static  getDerivedStateFromProps(props, state) {
    console.log('Lifecycle2 : getDerivedStateFromProps')
    return  null;
    }
    // componentDidMount METHOD
    componentDidMount() {
    console.log('Lifecycle2 : componentDidMount')
    }
    // render METHOD
    render() {
    console.log('Lifecycle2 : render')
    return (
    <div></div>
    )
    }
    }
    export  default  LifeCycle2
***Output***

    Lifecycle1 : constructor
    Lifecycle1 : getDerivedStateFromProps
    Lifecycle1 : render
    Lifecycle2 : constructor
    Lifecycle2 : getDerivedStateFromProps
    Lifecycle2 : render
    Lifecycle2 : componentDidMount
    Lifecycle1 : componentDidMount
### Updating Lifecycle Methods


| sequence | method name | purpose|
|--|--|--|
|1  | getDerivedStateFromProps(props, state)| called everytime a component is re-rendered|
|2  | shouldComponentUpdate(nextProp, nextState) | dictates if component should re-render or not|
|3  | render | do renders, return JSX|
|4  | getSnapShotBeforeUpdate | called right before the changes from vertual DOM are to be reflected in the actual DOM, (just before the updation hapen)|
|5  | componenDidUpdate | called after the render is finished in re-render cycle|


