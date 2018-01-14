---
layout: post
title:      "Combine Reduce Function w/ an example"
date:       2018-01-10 22:21:58 -0500
permalink:  combine_reduce_function_w_an_example
---


I wanted to write out how the combine reduce function works for the sole purpose of understanding it better myself. Rubber ducking post. 

Okay, so Redux offers a Combine Reduce function which combines all of your reducers. 

Reducers generally look something like this (please excuse the verbose names):
```
export function booksReducerFunction(state = { books: [] }, action){
  switch (action.type) {
  case "ADD_BOOK":
    return [].concat(state, action.payload)
  case "REMOVE_BOOK":
    let idx = state.indexOf(action.payload)
    return [].concat(state.slice(0, idx), state.slice(idx + 1, state.length))
  default:
    return state
  }
}

export function recommendedBooksReducerFunction(state = { recommendedBooks: [] }, action){
  switch (action.type) {
  case "ADD_RECOMMENDED_BOOK":
    return [].concat(state, action.payload)
  case "REMOVE_RECOMMENDED_BOOK":
    let idx = state.indexOf(action.payload)
    return [].concat(state.slice(0, idx), state.slice(idx + 1, state.length))
  default:
    return state
  }
}
```

As you can see, they take in state and action objects as parameters and based on the action type, they make changes to the state and return the modified state. Notice that the initial default value for the state is an object with an empty array. The final combined state will be a single object with one or more arrays that represent the state for each reducer: 
```
state = { books: [], recommendedBooks: [] }
```

Okay, so Redux allows you pass reducers into the combineReducers function to receive a single reducer that does the job of all the reducers you passed in. Let's define a single reducer object with all of our reducers:

```
singleReducerObject = { books: booksReducerFunction, recommendedBooks: recommendedBooksReducerFunction }
```

Okay, now we can receive our single reducer function by simply typing:
`singlerReducerFunction = combineReducers(singleReducerObject)` 

Note: you may need to import this by including `import { combineReducers } from 'redux' `at the top of your file.

The combineReducers function looks like this under the hood:
```
export function combineReducers(reducers){
  return (state = {}, action) => {
    return Object.keys(reducers).reduce(
      (nextState, key)=>{
        nextState[key] = reducers[key](state[key], action);
        return nextState
      }, {}
    )
  }
}
```

So let's break this down: what's happening here?

Well, you can notice that we're returning a function that takes in two arguments, `state` (defaulted to an empty object, {})  and `action`, and returns a single final `nextState` object. The function returns a state object by calling a reduce function on the array of the individual reducers:
```
Object.keys(reducers).reduce(
  (nextState, key)=>{
    nextState[key] = reducers[key](state[key], action);
    return nextState
  }, {})
```

In other words, here's the array we're performing the reduce method on:
```
[books, recommendedBooks] 
```

Remember, a reduce method simply takes an array and returns a single value by combining them in some way. The arguments to a reduce function are 1) the function that combines the values of the array and 2) a starting value (in this case, an empty array (`{}`) (since we don't want to mutate anything in functional programming). 

On the first iteration of the reduce method, we're updating the recommended books portion of the state and adding that to an empty object: When we're done, we'll return a state object as follows, and use that as a starting point for our next iteration:
```
state = { recommendedBooks: recommendedBooks(state.recommendedBooks, action) }
```

For example, if the action type was to add a recommended book, this might evaluvate to something like:
```
state = { recommendedBooks: [ { name: "Gödel, Escher, Bach: an Eternal Golden Braid", datePrinted: 1979 }  ]  }
```

Moving on, for the second iteration of reduce, we're still updating the state object but now we're doing the books portion of it:
```
state.books =  books(state.books, action)
```

Finally we return the state object w/ the updated settings
```
state = { recommendedBooks: recommendedBooks(state.recommendedBooks, action), 
books: books(state.books, action) }
```

This also explains why if an action type exists in multiple reducers, it will run through all of those changes (e.g. for example, let's say both the *books* and *recommendedBooks* reducers have an 'ADD_BOOK' action type defined in their case statements: then we would add books to both arrays in the state object. The end!
