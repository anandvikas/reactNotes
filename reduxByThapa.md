## React Redux

> Setup
> 
create a react app

    npx create-react-app

install react-redux

    npm install redux react-redux
### creating a counter app using redux

> Making action creators (/actions/index.js)
/

    / actionCreator 1
    const  incNumber = () => {
	    return {
		    type : 'INCREMENT'
	    }
    }  
    
    // actionCreator 2
    const  decNumber = () => {
	    return {
		    type : 'DECREMENT'
	    }
    }  
    
    // exporting action creators
    export {incNumber, decNumber}


> making reducer (/reducers/upDown.js)

    // defining initial state
    const  initialState = 0; 
    
    // making reducer
    const  changeTheNumber = (state = initialState, action) => {
	    switch (action.type) {
		    case  'INCREMENT' : return  state + 1;
		    case  'DECREMENT' : return  state - 1;
		    default : return  state;
	    }
    }
    export {changeTheNumber}
**if we have multiple reducers then we can wrap them all in a single variable. 
*This is optional***

> wrapping the reducers (/reducers/index.js)

    // importing reducers
    import { changeTheNumber } from  "./upDown"; 
    
    // importing the wrapper
    import {combineReducers} from  'redux';  
    
    // wrapping
    const  rootReducer = combineReducers({
    changeTheNumber:changeTheNumber
    })  
    
    export  default  rootReducer;
    

> creating store (/store/store.js)

    // importing createStore method
    import { createStore } from  "redux"; 
    
    // importing reducers
    import  rootReducer  from  "../reducers/index";
    
    // creating store
    const  store = createStore(
	    rootReducer,
	    // this line will allow us to use redux dev tools for chrome extension
	    window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
    )
    
    // exporting store
    export  default  store;

> importing store in index.js

    import  reactDom  from  'react-dom'
    import  App  from  "./App.js";
    import  './index.css'
    
    // importing store
    import  store  from  './store/store'
    // importing provider
    import {Provider} from  'react-redux'
    store.subscribe(()=>console.log(store.getState())); 
    
    reactDom.render(<Provider  store={store}><App/></Provider>, document.getElementById('root'))

> Using store in App.js

    import  React  from  'react'
    import { useSelector, useDispatch } from  'react-redux'
    import { incNumber, decNumber } from  './actions/index'
    
    const  App = () => {
    const  myState = useSelector((state) =>  state.changeTheNumber)
    const  dispatch = useDispatch()
    return (
	    <div>
		    <button  onClick={()=>{dispatch(decNumber())}}>-</button>
		    <span>{myState}</span>
		    <button  onClick={()=>{dispatch(incNumber())}}>+</button>
	    </div>
    )
    }
    export  default  App


