## React hooks

 - Hooks are used to get class components like features in functional components.
 - Hooks don't work inside class component.

**Rules of using hooks**

 - Only call hooks at top level
 - Don't call hooks at conditionals, loops and nested components.
 - Only call hooks from react functional component. 
## useState hook
***Simple useState***

    import  React, { useState } from  'react'
    function  App() {
	    const [count, updateCount] = useState(0)
	    return (
		    <>
			    <button  onClick={()=>{updateCount((prev)=>{return  prev + 1})}}>{count}</button>
		    </>
	    )
    }
    export  default  App
##  useEffect hook

 - the effect hook lets you to perform side effects in functional components.
 - It is close replacement of class component life cycle methods **componentDidMount()**, **componentDidUpdate()**, **componentWillUnmount()**

### useEffect as replacememt of componentDidMount and componentDidUpdate

title of the webpage updates every time the component is updates.

> using class component

    import  React, { Component } from  'react'
    class  App  extends  Component {
    constructor(){
    super()
    this.state = {
    count : 0
    }
    }
    componentDidMount(){
    document.title = this.state.count
    }
    componentDidUpdate(){
    document.title = this.state.count
    }
    updateCount = () => {
    this.setState({
    count : this.state.count + 1
    })
    }
    render() {
    return (
    <button  onClick={this.updateCount}>{this.state.count}</button>
    )
    }
    }
    export  default  App

> Using functional component    

    import  React, { useEffect, useState } from  'react'
       function  App() {
   	    const [count, updateCount] = useState(0)
   	    // this useEffect will run everytime this component is mounted and coponent is updated
   	    useEffect(()=>{
   		    document.title = 'title is :  ' + count
   	    })
   	    return (
   		    <>
   			    <button  onClick={()=>{updateCount((prev)=>{return  prev + 1})}}>{count}</button>
   		    </>
   	    )
       }
    export  default  App
### conditionally running useEffect
the title will update only if the count 2 is changing.

> With class component

    import  React, { Component } from  'react'
    class  App  extends  Component {
	    constructor() {
		    super()
		    this.state = {
			    count1: 0,
			    count2: 0
		    }
	    }
	    componentDidMount() {
		    document.title = this.state.count2
	    }
	    componentDidUpdate(prevProp, prevState) {
		    if(prevState.count2 !== this.state.count2){
			    document.title = this.state.count2
		    }
	    }
	    updateCount1 = () => {
		    this.setState({
			    count1: this.state.count1 + 1
		    })
	    }
	    updateCount2 = () => {
		    this.setState({
			    count2: this.state.count2 + 1
		    })
	    }
	    render() {
		    return (
			    <>
				    <button  onClick={this.updateCount1}>{this.state.count1}</button>
				    <button  onClick={this.updateCount2}>{this.state.count2}</button>
			    </>
		    )
	    }
    }
    export  default  App

> With functional component

    import  React, { useEffect, useState } from  'react'
    function  App() {
	    const [count1, updateCount1] = useState(0)
	    const [count2, updateCount2] = useState(0)
	    useEffect(()=>{
		    document.title = count2
	    }, [count2])
	    return (
		    <>
			    <button  onClick={()=>{updateCount1((prev)=>{return  prev + 1})}}>{count1}</button>
			    <button  onClick={()=>{updateCount2((prev)=>{return  prev + 1})}}>{count2}</button>
		    </>
	    )
    }
    export  default  App
### running useEffect only once (mimic of componentDidMount())

> with class component

    import  React, { Component } from  'react'
    class  App  extends  Component {
    componentDidMount(){
    console.log('Only once')
    }
    render() {
    return (
    <div>Hello</div>
    )
    }
    }
    export  default  App

> Using functional components

    import  React, { useEffect } from  'react'
    function  App() {
	    useEffect(()=>{
		    console.log('only once')
	    }, [])
	    return (
		    <div>Hello</div>
	    )
    }
    export  default  App
### useEffect with cleanup (mimic of componentWillunmount())
logging a message just before unmounting the child component

> Using class component

    import  React, { Component } from  'react'
    class  Comp  extends  Component {
	    // this will run just before this component is unmounted
	    componentWillUnmount(){
		    console.log('I am going to unmount')
	    }
	    render(){
		    return(
			    <h1>Child component</h1>
		    )
	    }
    }
    class  App  extends  Component {
	    constructor(){
		    super()
		    this.state = {
			    bool : true
		    }
	    }
	    toggle = () => {
		    this.setState({
			    bool : !this.state.bool
		    })
	    }
	    render() {
		    return (
			    <>
				    <button  onClick={this.toggle}>toggle</button>
				    {this.state.bool ? <Comp/> : null}
			    </>
		    )
	    }
    }
    export  default  App

> Using functional component

If there is any return statement in useEffect function , then that return statement executes just before the component is um-mounted. So if we put any line of code in that return statement then that line of code will run just before the component is unmounted.

    import  React, { useEffect, useState } from  'react'
    function  Comp () {
	    useEffect(()=>{
		    return (()=>{
			    console.log('I am going to unmount')
		    })
	    })
	    return (
		    <h1>Child component</h1>
	    )
    }
    function  App() {
	    const [bool, setBool] = useState(true)
	    return (
			    <>
				    <button  onClick={()=>{setBool(!bool)}}>toggle</button>
				    {bool ? <Comp/> : null}
			    </>
	    )
    }
    export  default  App
### combined mounting and unmounting 

in this code we are trying to start counting on a component when it mounts and reset the counting when it un-mounts

> Using class component

    import  React, { Component } from  'react'
    class  Comp  extends  Component {
	    constructor() {
		    super()
		    this.state = {
			    count: 0
		    }
	    }
	    increaseCount = () => {
		    this.setState({
			    count: this.state.count + 1
		    })
		    console.log('incrememt running')
	    }
	    componentDidMount() {
		    this.interval = setInterval(this.increaseCount, 1000)
	    }
	    componentWillUnmount() {
		    clearInterval(this.interval)
	    }
	    render() {
		    return (
			    <h1>{this.state.count}</h1>
		    )
	    }
    }  
    
    class  App  extends  Component {
	    constructor() {
		    super()
		    this.state = {
			    bool: false
		    }
	    }
	    toggle = () => {
		    this.setState({
			    bool: !this.state.bool
		    })
	    }
	    render() {
		    return (
			    <>
				    <button  onClick={this.toggle}>toggle</button>
				    {this.state.bool ? <Comp  /> : null}
			    </>
		    )
	    }
    }
    export  default  App

> Using functional component

    import  React, { useEffect, useState } from  'react'
    function  Comp () {
	    const [count, updateCount] = useState(0)
	    let  interval;
	    function  increase () {
		    updateCount((prev)=>{return  prev + 1})
	    }
	    useEffect(()=>{
		    // on mount
		    interval = setInterval(increase, 1000)
		    // on un-mount
		    return (()=>{
			    clearInterval(interval)
		    })
	    },[])
	    return (
		    <h1>{count}</h1>
	    )
    }
    function  App() {
	    const [bool, updateBool] = useState(false)
	    return (
		    <>
			    <button  onClick={()=>{updateBool(!bool)}}>toggle</button>
			    {bool ? <Comp/> : null}
		    </>
	    )
    }
    export  default  App
### fetching api with useEffect

> using class component

    import  React, { Component } from  'react'
    class  App  extends  Component {
	    componentDidMount(){
	    fetch('https://api.covid19api.com/summary')
	    .then((res)=>{return  res.json()})
	    .then((res)=>{console.log(res)})
	    }
	    render() {
		    return (
			    <div>see in console</div>
		    )
	    }
    }
    export  default  App

> Using functional component

    import  React, { useEffect } from  'react'
    function  App() {
	    useEffect(()=>{
	    fetch('https://api.covid19api.com/summary')
	    .then((res)=>{return  res.json()})
	    .then((res)=>{console.log(res)})
	    }, [])
	    return (
		    <div>See in console</div>
	    )
    }
    export  default  App


	## useContext hook
context hook is the replacement of of context api . But, it does the task in more simplified way.

> Creating and consuming value with help of simple context

App.js

    import  React  from  'react'
    import  Comp  from  './comp'
    const  context1 = React.createContext()
    const  context2 = React.createContext()  
    
    function  App() {
	    return (
		    <context1.Provider  value='vikas'>
			    <context2.Provider  value='Anand'>
				    <Comp/>
			    </context2.Provider>
		    </context1.Provider>
	    )
    }
    export  default  App
    export {context1, context2}

Comp.js

    import  React  from  'react'
    import { context1, context2 } from  './App'
    function  Comp() {
	    return (
		    <context1.Consumer>
			    {(fname)=>{
				    return (
					    <context2.Consumer>
						    {(lname)=>{
							    return (
								    <h1>{fname}  {lname}</h1>
							    )
						    }}
					    </context2.Consumer>
				    )
			    }}
		    </context1.Consumer>
	    )
    }
    export  default  Comp;

> Using useContext hook  
> (useContext hook only simplifies the consumer part of the context)

App.js

    import  React  from  'react'
    import  Comp  from  './comp'
    const  context1 = React.createContext()
    const  context2 = React.createContext()  
    
    function  App() {
	    return (
		    <context1.Provider  value='vikas'>
			    <context2.Provider  value='Anand'>
				    <Comp/>
			    </context2.Provider>
		    </context1.Provider>
	    )
    }
    export  default  App
    export {context1, context2}
Comp.js

    import  React, {useContext} from  'react'
    import { context1, context2 } from  './App'
    function  Comp() {
	    const  fname = useContext(context1)
	    const  lname = useContext(context2)
	    return (
		    <h1>{fname}  {lname}</h1>
	    )
    }
    export  default  Comp;
## useReducer hook

 - useReducer is ahook that is used for state menagement
 - It is an alternative of useState hook
 - useState is build using useReducer
 
***useReducer hook try to mimic the reduce() method in JavaScript***
|reducer|  useReducer|
|--|--|
| array.reduce(reducer, initialValue) |  useReducer(reducer, initialState)|
|singleValue = reducer(accumulator, itemValue)| newState = reducer(currentState, action)|
|reduce method returns a single value| useReduce returns a pair of values [newState, dispatch]|

> useReducer example : simple variable
> 
this code demonstrate a counter app

    import  React, { useReducer } from  'react'
    let  initialState = 0;
    const  reducer = (state, action) => {
	    switch(action) {
		    case  'increase': return  state + 1
		    case  'decrease' : return  state - 1
		    case  'reset' : return  initialState
		    default : return  state
	    }
    }
    function  App() {
	    const [count, updateCount] = useReducer(reducer, initialState)
	    return (
		    <>
			    <h1>{count}</h1>
			    <button  onClick={()=>{updateCount('decrease')}}>-</button>
			    <button  onClick={()=>{updateCount('reset')}}>0</button>
			    <button  onClick={()=>{updateCount('increase')}}>+</button>
		    </>
	    )
    }
    export  default  App

> useReducer example : managing two states

    import  React, { useReducer } from  'react'
    let  initialState = {
	    counter1: 0,
	    counter2: 0
    };
    const  reducer = (state, action) => {
    switch (action) {
	    case  'increase1': return {...state, counter1 :  state.counter1 + 1}
	    case  'increase2': return {...state, counter2 :  state.counter2 + 1}
	    case  'reset': return  initialState
	    default: return  state
    }
    }
    function  App() {
	    const [count, updateCount] = useReducer(reducer, initialState)
	    return (
		    <>
			    <div>
				    <span>{count.counter1}</span>
				    <button  onClick={() => { updateCount('increase1') }}>+1</button>
			    </div>
			    <div>
				    <span>{count.counter2}</span>
				    <button  onClick={() => { updateCount('increase2') }}>+1</button>
			    </div>
			    <button  onClick={() => { updateCount('reset') }}>0</button>
		    </>
	    )
    }
    export  default  App
***Managing global state with help of useReducer and useContext***
The state is defined is Parent component. But it can used and updated in multiple child components.

> App.js

    import  React, { useReducer } from  'react'
    import  Comp1  from  './comp1'
    import  Comp2  from  './comp2'
    import  Comp3  from  './comp3'
    const  IncrementCtx = React.createContext()
    let  initialState = {
	    counter1: 0,
	    counter2: 0,
	    counter3: 0
    };
    const  reducer = (state, action) => {
	    switch (action) {
		    case  'increase1': return { ...state, counter1:  state.counter1 + 1 }
		    case  'increase2': return { ...state, counter2:  state.counter2 + 1 }
		    case  'increase3': return { ...state, counter3:  state.counter3 + 1 }
		    case  'reset': return  initialState
		    default: return  state
	    }
    }
    function  App() {
	    const [count, updateCount] = useReducer(reducer, initialState)
	    return (
		    <>
			    <IncrementCtx.Provider  value={{countVal : count, updateCountVal : updateCount}}>
				    <Comp1  />
				    <Comp2  />
				    <Comp3  />
			    </IncrementCtx.Provider>
			    <button  onClick={()=>{updateCount('reset')}}>reset</button>
		    </>
	    )
    }
    export  default  App
    export { IncrementCtx }

> comp1.js

    import  React, { useContext } from  'react'
    import { IncrementCtx } from  './App'
    function  Comp1() {
    const  value = useContext(IncrementCtx)
    // console.log(value)
    return (
    <>
    <div>
    <span>{value.countVal.counter1}</span>
    <button  onClick={()=>{value.updateCountVal('increase1')}}>+1</button>
    </div>
    </>
    )
    }
    export  default  Comp1

> comp2.js

    import  React, { useContext } from  'react'
    import { IncrementCtx } from  './App'
    function  Comp2() {
    const  value = useContext(IncrementCtx)
    // console.log(value)
    return (
    <>
    <div>
    <span>{value.countVal.counter2}</span>
    <button  onClick={()=>{value.updateCountVal('increase2')}}>+1</button>
    </div>
    </>
    )
    }
    export  default  Comp2

> comp3.js

    import  React, { useContext } from  'react'
    import { IncrementCtx } from  './App'
    function  Comp3() {
    const  value = useContext(IncrementCtx)
    // console.log(value)
    return (
    <>
    <div>
    <span>{value.countVal.counter3}</span>
    <button  onClick={()=>{value.updateCountVal('increase3')}}>+1</button>
    </div>
    </>
    )
    }
    export  default  Comp3
***Difference in useState and useReducer and when to use them***
| Scenario | useState | useReducer|
|--|--|--|
| type of state | number, string, boolean | object, array|
|number of state transition| one or two| too many|
|related state transition| no| yes|
|business logic| no business logic| complex business logic|
|global vs local| local| global|

## useCallback hook
study again
## useMemo hook
study again

## useRef hook
**it is used to store the reference of a component of element.**

***reference of an element***
the button will automatically get clicked on mount of component

    import  React, { useEffect, useRef } from  'react'
    function  App() {
	    const  ref1 = useRef(null)
	    useEffect(()=>{
		    ref1.current.click()
	    }, [])
	    return (
		    <>
			    <button  onClick={()=>{console.log('hello')}}  ref={ref1}>click</button>
		    </>
	    )
    }
    export  default  App
    
***reference of a component***
did not find the answer *********

## React custom hooks
***custom hook to update the title of webpage***

> App.js

    import  React, { useState } from  'react'
    import  useUpdateTitle  from  './useUpdateTitle'
    function  App() {
    const [count, updateCount] = useState(0)
    useUpdateTitle(count)
    return (
    <button  onClick={()=>{updateCount(count+1)}}>count {count}</button>
    )
    }
    export  default  App

> useUpdateTitle.js : ***Hook***

    import  React, { useEffect } from  'react'
    function  useUpdateTitle(count) {
    useEffect(()=>{
    document.title = `count ${count}`
    }, [count])
    }
    export  default  useUpdateTitle
***custom hook to for counter app***

> App.js

    import  React  from  'react'
    import  useCounter  from  './useCounter'
    const  Counter = (props) => {
    const [count, increase, decrease, reset] = useCounter()
    return (
    <div>
    <span>{props.name}  {count}</span>
    <button  onClick={decrease}>-1</button>
    <button  onClick={increase}>+1</button>
    <button  onClick={reset}>0</button>
    </div>
    )
    }
    function  App() {
    return (
    <>
    <Counter  name={'counter 1'}/>
    <Counter  name={'counter 2'}/>
    </>
    )
    }
    export  default  App

> useCounter.js : ***Hook***

    import  React, { useState } from  'react'
    function  useCounter() {
    const [count, updateCount] = useState(0)
    const  increase = () => {
    updateCount(prev  =>  prev + 1)
    }
    const  decrease = () => {
    updateCount(prev  =>  prev - 1)
    }
    const  reset = () => {
    updateCount(0)
    }
    return [count, increase, decrease, reset]
    }
    export  default  useCounter
***form handling using custom hook***

> App.js

    import  React  from  'react'
    import  useInput  from  './useInput'
    function  App() {
    const [val1, update1, reset1] = useInput('')
    const [val2, update2, reset2] = useInput('')
    const  submitHandler = (e) => {
    e.preventDefault()
    alert(`Hello ${val1} ${val2}`)
    reset1()
    reset2()
    }
    return (
    <>
    <form  onSubmit={submitHandler}>
    <div>First name : <input  type='text'  value={val1}  onChange={update1}/></div>
    <div>Last name : <input  type='text'  value={val2}  onChange={update2}/></div>
    <button  type='submit'>Submit</button>
    </form>
    </>
    )
    }
    export  default  App

> useInput.js ***Hook***

    import  React, { useState } from  'react'
    function  useInput(initial) {
    const [val, updateVal] = useState(initial)
    const  update = (e) => {
    updateVal(e.target.value)
    }
    const  reset = () => {
    updateVal(initial)
    }
    return [val, update, reset]
    }
    export  default  useInput


