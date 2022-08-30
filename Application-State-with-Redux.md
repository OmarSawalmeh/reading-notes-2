# **Application State with Redux**
In this comprehensive tutorial, Dan Abramov - the creator of Redux - will teach you how to manage state in your React application with Redux.

State management is absolutely critical in providing users with a well-crafted experience with minimal bugs.

It's also one of the hardest aspects of a modern front-end application to get right.

Redux provides a solid, stable, and mature solution to managing state in your React application. Through a handful of small, useful patterns, Redux can transform your application from a total mess of confusing and scattered state, into a delightfully organized, easy to understand modern JavaScript powerhouse.

The principles of Redux aren't new, but they are packaged and presented for you in an easy to use a library that not only elevates your applications but also improves your general understanding of building JavaScript UIs.

In this course, Dan Abramov will show you the fundamentals of Redux, so that you can start using it to simplify your applications.

Redux is a predictable state container for JavaScript apps.

It helps you write applications that behave consistently, run in different environments (client, server, and native), and are easy to test. On top of that, it provides a great developer experience, such as live code editing combined with a time traveling debugger.

You can use Redux together with React, or with any other view library. It is tiny (2kB, including dependencies), but has a large ecosystem of addons available.

## **Installation**

**Redux Toolkit**

Redux Toolkit is our official recommended approach for writing Redux logic. It wraps around the Redux core, and contains packages and functions that we think are essential for building a Redux app. Redux Toolkit builds in our suggested best practices, simplifies most Redux tasks, prevents common mistakes, and makes it easier to write Redux applications.

RTK includes utilities that help simplify many common use cases, including store setup, creating reducers and writing immutable update logic, and even creating entire "slices" of state at once.

Whether you're a brand new Redux user setting up your first project, or an experienced user who wants to simplify an existing application, Redux Toolkit can help you make your Redux code better.

Redux Toolkit is available as a package on NPM for use with a module bundler or in a Node application:

```
# NPM
npm install @reduxjs/toolkit

# Yarn
yarn add @reduxjs/toolkit
```

## **Basic Example**
The whole global state of your app is stored in an object tree inside a single store. The only way to change the state tree is to create an action, an object describing what happened, and dispatch it to the store. To specify how state gets updated in response to an action, you write pure reducer functions that calculate a new state based on the old state and the action.

```js
import { createStore } from 'redux'

/**
 * This is a reducer - a function that takes a current state value and an
 * action object describing "what happened", and returns a new state value.
 * A reducer's function signature is: (state, action) => newState
 *
 * The Redux state should contain only plain JS objects, arrays, and primitives.
 * The root state value is usually an object. It's important that you should
 * not mutate the state object, but return a new object if the state changes.
 *
 * You can use any conditional logic you want in a reducer. In this example,
 * we use a switch statement, but it's not required.
 */
function counterReducer(state = { value: 0 }, action) {
  switch (action.type) {
    case 'counter/incremented':
      return { value: state.value + 1 }
    case 'counter/decremented':
      return { value: state.value - 1 }
    default:
      return state
  }
}

// Create a Redux store holding the state of your app.
// Its API is { subscribe, dispatch, getState }.
let store = createStore(counterReducer)

// You can use subscribe() to update the UI in response to state changes.
// Normally you'd use a view binding library (e.g. React Redux) rather than subscribe() directly.
// There may be additional use cases where it's helpful to subscribe as well.

store.subscribe(() => console.log(store.getState()))

// The only way to mutate the internal state is to dispatch an action.
// The actions can be serialized, logged or stored and later replayed.
store.dispatch({ type: 'counter/incremented' })
// {value: 1}
store.dispatch({ type: 'counter/incremented' })
// {value: 2}
store.dispatch({ type: 'counter/decremented' })
// {value: 1}
```

## **Testing redux reducers with Jest**
**Reducers**

Just a quick refresher on what reducer is before we go into testing and code. Redux documentation is still great, in fact it covers unit tests really well you don’t even have to read this post. To summarize it, the reducer is a pure function that takes the previous state and an action, and returns the next state.

**Pure functions**

Because it’s a pure function, it’s easy to test. It’s a function that produces no side effects; given the same input, will always return the same output; doesn’t rely on external state.

**What to test**

Usually reducer consists of initial state and switch statement to handle every action. I like to break down my store into different sub-stores and have separate reducers for each sub-store. Sometimes one switch/case may handle few actions, because the business logic is the same and outcome should be the same. Some example GET_POST and UPDATE_POST should update the same store and produce same outcome.

It’s important to test reducers. That’s where business logic should happen and where new application state is formed based on external (API) or internal response.

**Boilerplate**

As any unit test, it starts with boilerplate setup and writing empty tests just to outline what needs to be tested. I like to to test the initial state and then every switch/case in the reducer to see if action.payload will produce expected store. If you stop and think about it — it’s simple.

```js
import reducer from './getPostsReducer';
import * as types from '../actions/posts/getPostsReduxAction';
import expect from 'expect';

describe('team reducer', () => {
  it('should return the initial state');
  it('should handle GET_POST_SUCCESS');
  it('should handle UPDATE_POST_SUCCESS');
  it('should handle GET_POST_FAIL');
  it('should handle GET_POST_START');
```

Add tests and mock data
From here, we can just start filling in the tests one by one. I also like to create mock files and store the API response at the time of testing. So I can test agains “real” data. This strategy uncovered few accidental API changes where the response was changed without any warning. Instead of blaming anyone, we can just look at the mock data and see what’s expected on the front end.

```js
import reducer from './getPostReducer';
import * as actions from '../actions/posts/getPost';
import { UPDATE_POST_SUCCESS } from '../actions/posts/updatePost';
import expect from 'expect';
import getPostMock from '../mocks/getPostMock';

describe('post reducer', () => {
  it('should return the initial state', () => {
    expect(reducer(undefined, {})).toEqual({});
  });

  it('should handle GET_POST_START', () => {
    const startAction = {
      type: actions.GET_POST_START
    };
    // it's empty on purpose because it's just starting to fetch posts
    expect(reducer({}, startAction)).toEqual({});
  });

  it('should handle GET_POST_SUCCESS', () => {
    const successAction = {
      type: actions.GET_POST_SUCCESS,
      post: getPostMock.data, // important to pass correct payload, that's what the tests are for ;)
    };
    expect(reducer({}, successAction)).toEqual(getPostMock.data);
  });

  it('should handle UPDATE_POST_SUCCESS', () => {
    const updateAction = {
      type: UPDATE_POST_SUCCESS,
      post: getPostMock.data,
    };
    expect(reducer({}, updateAction)).toEqual(getPostMock.data);
  });

  it('should handle GET_POST_FAIL', () => {
    const failAction = {
      type: actions.GET_POST_FAIL,
      error: { success: false },
    };
    expect(reducer({}, failAction)).toEqual({ error: { success: false } });
  });
});
```

---

[Reading Page](./README.md)

---