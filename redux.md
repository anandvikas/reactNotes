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

   
