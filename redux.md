# React redux tutorials

 - **Redux is a predictable state container for JavaScript apps.**
 - It is a library of JavaScript.
 - Redux is not tied to react. It can be use with React, Angular, Vue etc. and even with vanilla JavaScript.
 - Redux stores the state (data) of an application
# React Redux
React redux is the official Redux UI binding library for React. 

### basic Setup

    npm init --yes
    npm install redux
now create a file named index.js and write the code in that
## three core concepts of redux
| concept |  purpose|
|--|--|
| Store | It holds the state(data) of the application |
|action| It describes the changes which needed to be done in the state(updating of state)|
|reducer| It actually perform the updating task depending on the action and the current state. |
## three principals of using redux

 1. store all the states the the application in a single object, which would be maintained by the redux.
 2. To update the state object of the application we must let redux know about that with an **action**. Not allowed to directly update the state object.
 3. We must use pure reducers to perform the action.
 pure reducers : 
pure reducers are functions which take 'previous state' and 'action' as arguments and returns the new state.

## Actions

 - They are the plain JavaScript objects. with a type property.
 - It is the only way the Application can interact with the store.
 - It have a type property that indicates the type of action to be performed.
 - Action creator  is a function which returns an action.
  
Action

    {
    type : ACTION_1,
    info : 'first redux action'
    } 
Action Creator

    function  actionCreator () {    
	    return {    
		    type : ACTION_1,    
		    info : 'first redux action'    
	    }    
    }
## Reducer

 - A reducer is a function that accepts previous state and action to be performed as argument and returns the updated state.

reducer

    const  initState = {
    cakeCount : 10
    }
    const  reducer = (state = initState, action) => {
    switch(action.type){
    case  ACTION_1: return {...state, cakeCount :  state.cakeCount - 1}
    default: return  state
    }
    }
## Store

 - There is only one store for the entire application.
 - It holds the state of the application.
 - allow access to state via **getState()**
 - allow state to be updated via **dispatch(action)**
 - registers listeners via **subscribe(listener)**
 - un-registers listeners via function **returned** by **subscribe(listener)**

to use store we need to import redux in index.js

    const  redux = require('redux');
    const  createStore = redux.createStore;

### creating the store
the createStore method takes reducer as argument. 

    const  store = createStore(reducer)
    
with help of getstate method we get the current state of store.

    console.log('initial state',  store.getState())
    const  unsubscribe = store.subscribe(()=>{console.log('Update state  ',  store.getState())})
dispatching actions.

    store.dispatch(buyCake())
    store.dispatch(buyCake())
    store.dispatch(buyCake())
    
    unsubscribe()

## Complete cake shop app

    const  redux = require('redux');
    const  createStore = redux.createStore;  
    
    // action type
    const  BUY_CAKE = 'BUY_CAKE'  
    
    // action creator
    const  buyCake = () => {
	    return {
		    type : BUY_CAKE,
		    quantity : 1
	    }
    }  
    
    // initial State
    const  initialState = {
	    numOfCakes : 10
    }  
    
    // reducer
    const  reducer = (state = initialState, action) => {
	    switch(action.type){
		    case  BUY_CAKE : return {...state, numOfCakes :  state.numOfCakes - 1}
		    default : return  state
	    }
    }  
    
    // store
    const  store = createStore(reducer)  
    
    // other methods
    console.log('initial state  ',  store.getState())
    const  unsubscribe =  store.subscribe(()=>{console.log('Update state  ',  store.getState())})  
    
    // dispatching the action
    store.dispatch(buyCake())
    store.dispatch(buyCake())
    store.dispatch(buyCake())  
    
    unsubscribe()
### restocking the cake

    ...
    // action type
    ...
    const  CAKE_RESTOCKED = 'CAKE_RESTOCKED'
    
      
    
    // action creator
    ...
    const  restockCake = (qty = 1) => {
	    return {
		    type : CAKE_RESTOCKED,
		    quantity : qty
	    }
    }  
    ...
    // reducer
    const  reducer = (state = initialState, action) => {
	    switch(action.type){
		    case  BUY_CAKE : return {...state, numOfCakes :  state.numOfCakes - 1}
		    case  CAKE_RESTOCKED : return {...state, numOfCakes :  state.numOfCakes +  action.quantity}
		    default : return  state
	    }
    }  
    ...
    // dispatching the action
    ...
    store.dispatch(restockCake(3))  
    
    unsubscribe()

## Binding the action creators
bindActionCreators can be used to bind actions to a variable.

    ...
    // importing
    const  bindActionCreators = redux.bindActionCreators
    ...
    // dispatching actions using bindActionCreators
    const  actions = bindActionCreators({buyCake, restockCake},  store.dispatch)
    actions.buyCake()
    actions.buyCake()
    actions.buyCake()
    actions.restockCake(3)  
    
    unsubscribe()
## Adding one more product

> with help of only one reducer

    // action type
    ...
    const  BUY_ICECREAM = 'BUY_ICECREAM'
    const  ICECREAM_RESTOCKED = 'ICECREAM_RESTOCKED'  
    
    // action creator
    ...
    const  buyIceCream = () => {
	    return {
		    type : BUY_ICECREAM,
		    quantity : 1
	    }
    }
    const  restockIceCream = (qty = 1) => {
	    return {
		    type : ICECREAM_RESTOCKED,
		    quantity : qty
	    }
    }  
    
    // initial State
    const  initialState = {
	    numOfCakes : 10,
	    numOfIceCreams : 20
    }  
    
    // reducer
    const  reducer = (state = initialState, action) => {
	    switch(action.type){
		    case  BUY_CAKE : return {...state, numOfCakes :  state.numOfCakes - 1}
		    case  CAKE_RESTOCKED : return {...state, numOfCakes :  state.numOfCakes +  action.quantity}
		    case  BUY_ICECREAM : return {...state, numOfIceCreams :  state.numOfIceCreams - 1}
		    case  ICECREAM_RESTOCKED : return {...state, numOfIceCreams :  state.numOfIceCreams +  action.quantity}
		    default : return  state
	    }
    }      
    ...    
    // dispatching actions using bindActionCreators
    const  actions = bindActionCreators({buyCake, restockCake, buyIceCream, restockIceCream},  store.dispatch)
    ...
    actions.buyIceCream()
    actions.buyIceCream()
    actions.restockIceCream(2)  
    
    unsubscribe()
with help of seperate reducers.

> NOTE : since createStore method can accept only one argument. So, we
> need to combine multiple reducers in one.

    const  redux = require('redux');
    const  createStore = redux.createStore;
    const  bindActionCreators = redux.bindActionCreators;
    const  combineReducers = redux.combineReducers;  
    
    // action type
    const  BUY_CAKE = 'BUY_CAKE'
    const  CAKE_RESTOCKED = 'CAKE_RESTOCKED'
    const  BUY_ICECREAM = 'BUY_ICECREAM'
    const  ICECREAM_RESTOCKED = 'ICECREAM_RESTOCKED'  
    
    // action creator
    const  buyCake = () => {
	    return {
		    type : BUY_CAKE,
		    quantity : 1
	    }
    }
    const  restockCake = (qty = 1) => {
	    return {
		    type : CAKE_RESTOCKED,
		    quantity : qty
	    }
    }
    const  buyIceCream = () => {
	    return {
		    type : BUY_ICECREAM,
		    quantity : 1
	    }
    }
    const  restockIceCream = (qty = 1) => {
	    return {
		    type : ICECREAM_RESTOCKED,
		    quantity : qty
	    }
    }  
    
    // initial State
    const  cakeInitialState = {
	    numOfCakes : 10,
    }
    const  iceCreamInitialState = {
	    numOfIceCreams : 20
    }  
    
    // reducer
    const  cakeReducer = (state = cakeInitialState, action) => {
	    switch(action.type){
		    case  BUY_CAKE : return {...state, numOfCakes :  state.numOfCakes - 1}
		    case  CAKE_RESTOCKED : return {...state, numOfCakes :  state.numOfCakes +  action.quantity}
		    default : return  state
	    }
    }
    const  iceCreamReducer = (state = iceCreamInitialState, action) => {
	    switch(action.type){
		    case  BUY_ICECREAM : return {...state, numOfIceCreams :  state.numOfIceCreams - 1}
		    case  ICECREAM_RESTOCKED : return {...state, numOfIceCreams :  state.numOfIceCreams +  action.quantity}
		    default : return  state
	    }
    }  
    
    // combining the reducers
    const  rootReducer = combineReducers({
	    cake : cakeReducer,
	    iceCream : iceCreamReducer
    })  
    
    // store
    const  store = createStore(rootReducer)  
    
    // other methods
    console.log('initial state  ',  store.getState())
    const  unsubscribe =  store.subscribe(()=>{console.log('Update state  ',  store.getState())})    
    
    // dispatching actions using bindActionCreators
    const  actions = bindActionCreators({buyCake, restockCake, buyIceCream, restockIceCream},  store.dispatch)
    actions.buyCake()
    actions.buyCake()
    actions.buyCake()
    actions.restockCake(3)
    actions.buyIceCream()
    actions.buyIceCream()
    actions.restockIceCream(2)  
    
    unsubscribe()
## Adding logger middleware
install through npm

    npm install redux-logger
usage.

    ...
    const  applyMiddleware = redux.applyMiddleware
    const  reduxLogger = require('redux-logger')
    const  logger = reduxLogger.createLogger()
    ...
    ...
    const  store = createStore(rootReducer, applyMiddleware(logger))
    ...
    ...



   
