## Making cake shop app with react redux
### Step 1 :
make a component named 'CakeContainer'

> components/cakeContainer.js

    import  React  from  'react'
    const  CakeContainer = () => {
	    return (
		    <div>
			    <h2>Number of cakes</h2>
			    <button>Buy cake</button>
		    </div>
	    )
    }
    export  default  CakeContainer
### Step 2 :
use **CakeContainer** in App.js

> App.js

    import  React  from  'react'
    import  CakeContainer  from  './components/cakeContainer'
    const  App = () => {
	    return (
		    <>
			    <CakeContainer  />
		    </>
	    )
    }
    export  default  App
### Step 3 :
make action types of cake

> components/redux/cake/actionTypes.js

    export  const  BUY_CAKE = 'BUY_CAKE'
### Step 4 :
import action types and make action creators.
> components/redux/cake/actionCreators.js

    import { BUY_CAKE } from  "./actionTypes"
    export  const  buyCake = () => {
	    return {
		    type : BUY_CAKE
	    }
    }
### Step 5 :
make reducer
> components/redux/cake/reducers.js

    import { BUY_CAKE } from  "./actionTypes"
    const  initialState = {
	    numOfCakes : 10
    }
    const  cakeReducer = (state = initialState, action) => {
	    switch (action.type) {
		    case  BUY_CAKE : return {...state, numOfCakes :  state.numOfCakes - 1};
		    default : return  state
	    }
    }
    export  default  cakeReducer;
### Step 6 :
import createStore fro redux and create a store with help of it.
> components/redux/store.js

    import {createStore} from  'redux';
    import  cakeReducer  from  './cake/reducers';
    const  store = createStore(cakeReducer);
    export  default  store;
### Step 7 :

 - import Provider from react-redux and wrap the JSX of App component in it.
 - import the store and pass it as a prop in Provider

> App.js

    ...
    import { Provider } from  'react-redux'
    import  store  from  './components/redux/store'
    ...
    const  App = () => {
	    return (
		    <>
			    <Provider  store={store}>
				    <CakeContainer  />
			    </Provider>
		    </>
	    )
    }
    ...

### Step 8 :
create a index.js file in redux folder which will export all the action creators.
> components/redux/index.js

    export {buyCake} from  './cake/actionCreators'
### Step 9 :
in cakeContainer.js create 'mapStateToProps' and 'mapDispatchToProps' functions. and connect them with the component. 

*'mapStateToProp' and 'mapDispatchToProps' functions are take are wraped around the main component (CakeContainer) with help of connet. And whenever the 'CakeContainer' component is called 'mapStateToProp' and 'mapDispatchToProps' functions are called first and 'CakeComponent' function is called after them. 'mapStateToProp' and 'mapDispatchToProps' functions return some data which is recieved in 'CakeComponent' as a prop. This data a generally state value and action.*

> components/cakeContainer.js

    import  React  from  'react'
    import {connect} from  'react-redux'
    import { buyCake } from  './redux'  
    
    const  CakeContainer = (props) => {
	    return (
		    <div>
			    <h2>Number of cakes : {props.numOfCakes}</h2>
			    <button  onClick={props.buyCake}>Buy cake</button>
		    </div>
	    )
    }  
    
    const  mapStateToProps = (state) => {
	    return {
		    numOfCakes :  state.numOfCakes
	    }
    }  
    
    const  mapDispatchToProps = (dispatch) => {
	    return {
		    buyCake : () =>  dispatch(buyCake())
	    }
    }  
    
    export  default  connect(mapStateToProps, mapDispatchToProps)(CakeContainer)
***at this stage the app must be working***  

### Step 10 :
with help of hooks we can mimic the functionality of 'mapStateToProps' and 'mapDispatchToProps' in more easy way.

 - useSelector : it recieves state as argument and returns it's values
 - useDispatch : it is used to mimic the 'mapDispatchToProps' function

> components/hooksCakeContainer.js

    import  React  from  'react'
    import { useDispatch, useSelector } from  'react-redux'
    import { buyCake } from  './redux'  
    
    const  HooksCakeContainer = () => {
	    const  numOfCakes = useSelector((state) => {return  state.numOfCakes})
	    const  dispatch = useDispatch()
	    return (
		    <div>
			    <h2>Number of cakes : {numOfCakes}</h2>
			    <button  onClick={()=>{dispatch(buyCake())}}>Buy cake</button>
		    </div>
	    )
    }  
    
    export  default  HooksCakeContainer
### Step 11 :
import the 'CakeContainer' component with 'HooksCakeContainer' component in App.js. And the app should work fine.

> App.js

    ...
    const  App = () => {
	    return (
		    <>
			    <Provider  store={store}>
				    <HooksCakeContainer/>
			    </Provider>
		    </>
	    )
    }
    ...
### Step 12 : introducing a new product (ice cream)
> components/redux/icecream/actionTypes.js

    export  const  BUY_ICECREAM = 'BUY_ICECREAM'
    
> components/redux/icecream/actionCreator.js

    import { BUY_ICECREAM } from  "./actionTypes";
    export  const  buyIceCream = () => {
	    return {
		    type : BUY_ICECREAM
	    }
    }
> components/redux/icecream/reducers.js

    import { BUY_ICECREAM } from  "./actionTypes"
    const  initialState = {
	    numOfIceCreams : 20
    }
    const  iceCreamReducer = (state = initialState, action) => {
	    switch (action.type) {
		    case  BUY_ICECREAM : return {...state, numOfIceCreams :  state.numOfIceCreams - 1};
		    default : return  state
	    }
    }
    export  default  iceCreamReducer;
    
### Step 13 : adding in store
> components/redux/store.js

Since createStore function can accept only one reducer. So we must combine (wrap cakeReducer and iceCreamReducer) multiple reducers in a single reducer.

> components/redux/rootReducer.js

    import { combineReducers } from  "redux";
    import  cakeReducer  from  "./cake/reducers";
    import  iceCreamReducer  from  "./icecream/reducers";  
    
    const  rootReducer = combineReducers({
	    cake : cakeReducer,
	    iceCream : iceCreamReducer
    })  
    
    export  default  rootReducer;
now in store.js instead of cakeReducer pass rootReducer in createStore function.
> components/redux/store.js

    import {createStore} from  'redux';
    import  rootReducer  from  './rootReducer';  
    
    const  store = createStore(rootReducer);  
    
    export  default  store;
export the action creator from index.js
> components/redux/index.js

    export {buyCake} from  './cake/actionCreators'
    export {buyIceCream} from  './icecream/actionCreator'
### Step 14 : making component for ice cream
> components/hooksIceCreamContainer.js

    import  React  from  'react'
    import { useDispatch, useSelector } from  'react-redux'
    import { buyIceCream } from  './redux'  
    
    const  HooksIceCreamContainer = () => {
    const  numOfIceCreams = useSelector((state) => {return  state.iceCream.numOfIceCreams})
    const  dispatch = useDispatch()
	    return (
		    <div>
			    <h2>Number of ice creams : {numOfIceCreams}</h2>
			    <button  onClick={()=>{dispatch(buyIceCream())}}>Buy ice cream</button>
		    </div>
	    )
    }  
    
    export  default  HooksIceCreamContainer
### Step 15 : import in App.js

> App.js

    ...
    const  App = () => {
	    return (
		    <>
			    <Provider  store={store}>
				    <HooksCakeContainer/>
				    <HooksIceCreamContainer/>
			    </Provider>
		    </>
	    )
    }
    ...

### Step 16 : Logger middleware
***logger is use to log information related to store in the console , every time any update is done. To use logger we need to add it in the store.***
install logger middleware in npm

    npm install redux-logger
add logger in store
> components/redux/store.js

    import {createStore, applyMiddleware} from  'redux';
    import  rootReducer  from  './rootReducer';
    import  logger  from  'redux-logger'  
    
    const  store = createStore(rootReducer, applyMiddleware(logger));  
    
    export  default  store;
### Step 17 : redux dev tool
redux dev tool is also used to track the store but it is a much better option compared to logger. 

 
 - Install the redux dev tools extension.
 - install redux dev tools extension in npm.
 ```
npm install --save redux-devtools-extension
```
 - Add in store
```
import {createStore} from  'redux';
import  rootReducer  from  './rootReducer';
import {composeWithDevTools} from  'redux-devtools-extension'

const  store = createStore(rootReducer, composeWithDevTools());  

export  default  store;
```

> **NOTE : TO USE BOTH LOGGER AND REDUX DEV TOOLS.**

    import {createStore, applyMiddleware} from  'redux';
    import  rootReducer  from  './rootReducer';
    import  logger  from  'redux-logger'
    import {composeWithDevTools} from  'redux-devtools-extension'
    
    const  store = createStore(rootReducer, composeWithDevTools(applyMiddleware(logger)));  
    
    export  default  store;
### Step 18 : action payload
payload will specify the amount of cakes to be reduced.
> components/newCakeContainer.js

    import  React, { useState } from  'react'
    import {connect} from  'react-redux'
    import { buyCake } from  './redux'  
    
    const  CakeContainer = (props) => {
	    const [number, setNumber] = useState(1)
	    return (
		    <div>
			    <h2>Number of cakes : {props.numOfCakes}</h2>
			    <input  type='number'  value={number}  onChange={(e)=>{setNumber(e.target.value)}}/>
			    <button  onClick={()=>{props.buyCake(number)}}>Buy cake</button>
		    </div>
	    )
    }  
    
    const  mapStateToProps = (state) => {
	    return {
		    numOfCakes :  state.cake.numOfCakes
	    }
    }  
    
    const  mapDispatchToProps = (dispatch) => {
	    return {
		    buyCake : (number) =>  dispatch(buyCake(number))
	    }
    }  
    
    export  default  connect(mapStateToProps, mapDispatchToProps)(CakeContainer)

Update the action creator
> components/redux/cake/actionCreatosr.js

    import { BUY_CAKE } from  "./actionTypes"
    export  const  buyCake = (number = 1) => {
    	return {
    		type : BUY_CAKE,
    		payload : number
    	}
    }
Updeate thre reducer
> components/redux/cake/reducers.js

    ...
    const  cakeReducer = (state = initialState, action) => {
	    switch (action.type) {
		    case  BUY_CAKE : return {...state, numOfCakes :  state.numOfCakes -  action.payload};
		    default : return  state
	    }
    }
    ...
Import NewCakeContainer in App.js
### Step 19 : second parameter in 'mapstateToProps' and 'mapDispatchToProp'
the second parameter to 'mapstateToProps' and 'mapDispatchToProp' functions contains the props thar are send to that component. we can use it if we want.

> components/itemsContainer.js

    import  React  from  'react'
    import {connect} from  'react-redux'
    import { buyCake, buyIceCream } from  './redux'  
    
    const  ItemsContainer = (props) => {
    console.log(props)
    return (
    <div>
    <h2>Number : {props.item}</h2>
    <button  onClick={props.buyItem}>Buy</button>
    </div>
    )
    }  
    
    const  mapStateToProps = (state, ownProps) => {
    const  itemState =  ownProps.item === 'cake' ?  state.cake.numOfCakes :  state.iceCream.numOfIceCreams
    return {
    item : itemState
    }
    }  
    
    const  mapDispatchToProps = (dispatch, ownProps) => {
    const  dispatchFunction =  ownProps.item === 'cake' ? ()=>dispatch(buyCake()) : ()=>dispatch(buyIceCream())
    return {
    buyItem : dispatchFunction
    }
    }  
    
    export  default  connect(mapStateToProps, mapDispatchToProps)(ItemsContainer)
Add component in App.js

    ...
    const  App = () => {
	    return (
		    <>    
			    <ItemsContainer  item='cake'/>
			    <ItemsContainer  item='iceCream'/>
			    </Provider>
		    </>
	    )
    }
    ...

### Step 20 : Asynchronous actions with react and redux.
|Synchronous Actions| Asynchronous actions |
|--|--|
|As soon as the action is dispatched the state updates immediately  | The state updates after completing some task. Just like callback Actions. |

***create a new state 'user'***

> conponents/redux/user/actionTypes.js

    export  const  FETCH_USERS_REQUEST = 'FETCH_USERS_REQUEST'
    export  const  FETCH_USERS_SUCCESS = 'FETCH_USERS_SUCCESS'
    export  const  FETCH_USERS_FAILURE = 'FETCH_USERS_FAILURE'
> conponents/redux/user/actionCreator.js

    import { FETCH_USERS_REQUEST, FETCH_USERS_SUCCESS, FETCH_USERS_FAILURE } from  "./actionTypes";  
    
    export  const  fetchUsersRequest = () => {
	    return {
		    type : FETCH_USERS_REQUEST
	    }
    }  
    
    export  const  fetchUsersSuccess = (users) => {
	    return {
		    type : FETCH_USERS_SUCCESS,
		    payload : users
	    }
    }  
    
    export  const  fetchUsersFaliure = (error) => {
	    return {
		    type : FETCH_USERS_FAILURE,
		    payload : error
	    }
    }
> conponents/redux/user/reducers.js

    import { FETCH_USERS_FAILURE, FETCH_USERS_REQUEST, FETCH_USERS_SUCCESS } from  "./actionTypes"  
    
    const  initialState = {
	    loading : false,
	    users : [],
	    error : ''
    }  
    
    const  usersReducer = (state = initialState, action) => {
	    switch(action.type){
		    case  FETCH_USERS_REQUEST: return {...state, loading : true};
		    case  FETCH_USERS_SUCCESS: return {loading : false, users :  action.payload, error : ''};
		    case  FETCH_USERS_FAILURE: return {loading : false, users : [], error :  action.payload}
		    default : return  state
	    }
    }  
    
    export  default  usersReducer;
> conponents/redux/index.js

    ...
    export * from  './user/actionCreator'
> conponents/redux/rootReducer.js

    ...
    import  usersReducer  from  "./user/reducers";  
    
    const  rootReducer = combineReducers({
    cake : cakeReducer,
    iceCream : iceCreamReducer,
    user : usersReducer
    })
### Step 21 : using redux thunk
install redux-thunk from npm

    npm install redux-thunk
import use middleware in store.js

> conponents/redux/store.js

    ...
    import  thunk  from  'redux-thunk'
    const  store = createStore(rootReducer, composeWithDevTools(applyMiddleware(logger, thunk)));
    ...

