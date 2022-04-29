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
in cakeContainer.js create 'mapStateToProps' and 'mapDispatchToProps' functions.

> components/cakeContainer.js

    ...
    import { buyCake } from  './redux'
    ...
    const  CakeContainer = () => {
	    return (
	    ...
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
    ...
