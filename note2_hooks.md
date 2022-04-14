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
