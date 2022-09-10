# **Redux Toolkit**

## **Redux Fundamentals, Part 8: Modern Redux with Redux Toolkit**
As you've seen, many aspects of Redux involve writing some code that can be verbose, such as immutable updates, action types and action creators, and normalizing state. There's good reasons why these patterns exist, but writing that code "by hand" can be difficult. In addition, the process for setting up a Redux store takes several steps, and we've had to come up with our own logic for things like dispatching "loading" actions in thunks or processing normalized data. Finally, many times users aren't sure what "the right way" is to write Redux logic.

That's why the Redux team created Redux Toolkit: our official, opinionated, "batteries included" toolset for efficient Redux development.

Redux Toolkit contains packages and functions that we think are essential for building a Redux app. Redux Toolkit builds in our suggested best practices, simplifies most Redux tasks, prevents common mistakes, and makes it easier to write Redux applications.

Because of this, Redux Toolkit is the standard way to write Redux application logic. The "hand-written" Redux logic you've written so far in this tutorial is actual working code, but you shouldn't write Redux logic by hand - we've covered those approaches in this tutorial so that you understand how Redux works. However, for real applications, you should use Redux Toolkit to write your Redux logic.

When you use Redux Toolkit, all the concepts that we've covered so far (actions, reducers, store setup, action creators, thunks, etc) still exist, but Redux Toolkit provides easier ways to write that code.

## **What is Redux Toolkit?**
Redux Toolkit is the official Redux tool, opinionated, with batteries for efficient Redux development. It includes several utility functions that simplify the most common use cases of Redux, including:  

   -  Store configuration.
   -  Setting dimmers.
   -  Immutable update logic.
You can also use it to create entire "slices" of state at once without writing any action creators or type of action manually. 

Apart from the use cases, It also includes the most used Redux add-ons, such as Redux Thunk for asynchronous logic and Reselect for writing selector functions, such that you can use them right away. Essentially, the Redux Toolkit is intended to be the standard way of writing Redux logic, and it is highly recommended that you use it. 

‍## **Why Redux Toolkit?**
Firstly, the Redux Toolkit makes it easy to write good Redux applications. Additionally, it speeds up development by: 

   -  Emphasizing adherence to best practices.
   -  Providing good default behaviors.
   -  Detecting errors. 
   -  Making it easy to write simpler code. 
Overall, the Redux Toolkit is beneficial to all Redux users, regardless of skill level or experience. This goes for whether you are a new Redux user setting up your first project or an experienced one who wants to simplify an existing application. ‍

‍## **Getting Started with the Redux Toolkit**
In this section, we will look at two main scenarios:  

   -  Implementation within an existing project.
   -  Creating a new project.

**Getting Started with Redux Toolkit**
## **Purpose**

The Redux Toolkit package is intended to be the standard way to write Redux logic. It was originally created to help address three common concerns about Redux:

"Configuring a Redux store is too complicated"
"I have to add a lot of packages to get Redux to do anything useful"
"Redux requires too much boilerplate code"
We can't solve every use case, but in the spirit of create-react-app, we can try to provide some tools that abstract over the setup process and handle the most common use cases, as well as include some useful utilities that will let the user simplify their application code.

Redux Toolkit also includes a powerful data fetching and caching capability that we've dubbed "RTK Query". It's included in the package as a separate set of entry points. It's optional, but can eliminate the need to hand-write data fetching logic yourself.

These tools should be beneficial to all Redux users. Whether you're a brand new Redux user setting up your first project, or an experienced user who wants to simplify an existing application, Redux Toolkit can help you make your Redux code better.

## **Installation**

Using Create React App
The recommended way to start new apps with React and Redux is by using the official Redux+JS template or Redux+TS template for Create React App, which takes advantage of Redux Toolkit and React Redux's integration with React component

```js
# Redux + Plain JS template
npx create-react-app my-app --template redux

# Redux + TypeScript template
npx create-react-app my-app --template redux-typescript
```

## **An Existing App**
Redux Toolkit is available as a package on NPM for use with a module bundler or in a Node application:
```js
# NPM
npm install @reduxjs/toolkit
```

## **What's Included**
**Redux Toolkit includes these APIs:**

   -  configureStore(): wraps createStore to provide simplified configuration options and good defaults. It can automatically combine your slice reducers, adds whatever Redux middleware you supply, includes redux-thunk by default, and enables use of the Redux DevTools Extension.
   -  createReducer(): that lets you supply a lookup table of action types to case reducer functions, rather than writing switch statements. In addition, it automatically uses the immer library to let you write simpler immutable updates with normal mutative code, like state.todos[3].completed = true.
   -  createAction(): generates an action creator function for the given action type string. The function itself has toString() defined, so that it can be used in place of the type constant.
   -  createSlice(): accepts an object of reducer functions, a slice name, and an initial state value, and automatically generates a slice reducer with corresponding action creators and action types.
   -  createAsyncThunk: accepts an action type string and a function that returns a promise, and generates a thunk that dispatches pending/fulfilled/rejected action types based on that promise
   -  createEntityAdapter: generates a set of reusable reducers and selectors to manage normalized data in the store
   -  The createSelector utility from the Reselect library, re-exported for ease of use.

## **RTK Query**
RTK Query is provided as an optional addon within the @reduxjs/toolkit package. It is purpose-built to solve the use case of data fetching and caching, supplying a compact, but powerful toolset to define an API interface layer for your app. It is intended to simplify common cases for loading data in a web application, eliminating the need to hand-write data fetching & caching logic yourself.

RTK Query is built on top of the Redux Toolkit core for its implementation, using Redux internally for its architecture. Although knowledge of Redux and RTK are not required to use RTK Query, you should explore all of the additional global store management capabilities they provide, as well as installing the Redux DevTools browser extension, which works flawlessly with RTK Query to traverse and replay a timeline of your request & cache behavior.

RTK Query is included within the installation of the core Redux Toolkit package. It is available via either of the two entry points below:
```js
import { createApi } from '@reduxjs/toolkit/query'

/* React-specific entry point that automatically generates
   hooks corresponding to the defined endpoints */
import { createApi } from '@reduxjs/toolkit/query/react'
```

## **What's included**
**RTK Query includes these APIs:**

   -  createApi(): The core of RTK Query's functionality. It allows you to define a set of endpoints describe how to retrieve data from a series of endpoints, including configuration of how to fetch and transform that data. In most cases, you should use this once per app, with "one API slice per base URL" as a rule of thumb.
   -  fetchBaseQuery(): A small wrapper around fetch that aims to simplify requests. Intended as the recommended baseQuery to be used in createApi for the majority of users.
   -  <ApiProvider />: Can be used as a Provider if you do not already have a Redux store.
   -  setupListeners(): A utility used to enable refetchOnMount and refetchOnReconnect behaviors.
See the RTK Query Overview page for more details on what RTK Query is, what problems it solves, and how to use it.

---

[Reading Page](./README.md)

---

