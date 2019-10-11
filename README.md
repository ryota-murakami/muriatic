# React AppState [![CircleCI](https://circleci.com/gh/ryota-murakami/react-appstate.svg?style=svg)](https://circleci.com/gh/ryota-murakami/react-appstate) [![tested with jest](https://img.shields.io/badge/tested_with-jest-99424f.svg)](https://github.com/facebook/jest) [![jest](https://jestjs.io/img/jest-badge.svg)](https://github.com/facebook/jest) [![code style: prettier](https://img.shields.io/badge/code_style-prettier-ff69b4.svg?style=flat-square)](https://github.com/prettier/prettier) [![All Contributors](https://img.shields.io/badge/all_contributors-2-orange.svg?style=flat-square)](#contributors) [![Netlify Status](https://api.netlify.com/api/v1/badges/f98aafdd-e136-41f0-b626-23471689ff2f/deploy-status)](https://app.netlify.com/sites/react-appstate/deploys)

> 🌏 React Global Store for Sharing Data between each components Built on Context.

![movie](./movie.gif)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

  - [Usage](#usage)
  - [Why](#why)
  - [Resource](#resource)
  - [Installation](#installation)
  - [API](#api)
    - [`<Provider appState={Store} />`](#provider-appStateappState-)
    - [`const [appState, setStore] = useStore()`](#const-appState-setappState--useappState)
  - [Get value from `appState`](#get-value-from-appState)
    - [update appState with `setStore(appState: Object)`](#update-appState-with-setappStateappState-object)
  - [Advanced](#advanced)
  - [TypeScript](#typescript)
    - [Example](#example-1)
  - [LICENSE](#license)
  - [Contributors](#contributors)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Usage

```js
// index.js
import React, { Fragment } from 'react'
import ReactDOM from 'react-dom'
import Provider, { useStore } from 'react-appstate'
 
// initialStore must be Plain Object
const initialStore = { count: 0 }
 
ReactDOM.render(
  <Provider appState={initialStore}>
    <App />
  </Provider>,
  document.getElementById('root')
)
 
function App() {
  const [appState, setStore] = useStore()

  return (
    <Fragment>
      <div>
        <button onClick={() => setStore({ count: appState.count + 1 })}>increment</button>
        <button onClick={() => setStore({ count: appState.count - 1 })}>decrement</button>
      </div>
      <p>I have {appState.apple.count} apples </p>
    </Fragment>
  )
}
```

## Play 👇

[![Edit react-appstate-exampe](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/react-appstate-exampe-oreg7?fontsize=14)

[https://react-appstate-demo.netlify.com/](https://react-appstate.netlify.com/) Same code as above usage's one.

## Why

I wanted a sharable state over the component hierarchy that could be setup immediately (in one shot). The goal was to have something similar to a **global version of `setState()`** with a simple interface.

Although there are many similar libraries and blog posts with code examples, they tended to be unnecessarily complicated / difficult to reuse. Muriatic is awesome for prototyping, experimenting, and developing small apps.

Now, the`setStore()` custom hook is packed it as an npm package to make setup one shot anywhere! 🍸

## Resources

- [React + TypeScript Cheatsheets](https://github.com/typescript-cheatsheets/react-typescript-cheatsheet#example-app): Example App [React TypeScript Todo Example 2019 Mid](https://github.com/ryota-murakami/react-typescript-todo-example-2019) is created with react-appstate.

## Installation

```
npm install react-appstate
yarn add react-appstate
```

## API

### `<Provider appState={Store} />`

+ Make your GlobalStore as a plain Javascript Object.(eg: `const GlobalStaate = {foo: "bar"}`)  
+ Wrap Provider in your root app component.
```js
import Provider from 'react-appstate'

// initialStore must be Plain Object
const initialStore = { count: 0 }

ReactDOM.render(
  <Provider appState={initialStore}>
    <App />
  </Provider>,
  document.getElementById('root')
```

### `const [appState, setStore] = useStore()`

+ Gives interface to access and set the global appState.

## Get value from `appState`

```js
// example
import { useStore } from 'react-appstate'

const AppleComponent = () => {
  const [appState, setStore] = useStore()
  
  return (<div><{appState.thisIsMyValue}/div>)
}
```

### update appState with `setStore(appState: Object)`

```js
// example
import { useStore } from 'react-appstate'

const NintendoComponent = () => {
  const [appState, setStore] = useStore()
  const orderSmashBros = () => setStore({sales: appState.sales + 1 })
  
  return (<button onClick={orderSmashBros}>You can not wait!!</button>)
}
```

## Advanced

This is an abstract example utilizing [custom Hooks](https://reactjs.org/docs/hooks-custom.html).

- **src/index.js**
```js
import React from 'react'
import ReactDOM from 'react-dom'
import Provider, { useStore } from 'react-appstate'
import { Layout } from './style'
import useAction from './actions'

const initialStore = { count: 0 }
ReactDOM.render(
  <Provider appState={initialStore}>
    <App />
  </Provider>,
  document.getElementById('root')
)

function App() {
  const action = useAction()
  return (
    <Layout>
      <div>
        <button onClick={action.increment}>increment</button>
        <button onClick={action.decrement}>decrement</button>
      </div>
      <p>{useStore().appState.count}</p>
    </Layout>
  )
}
```

- **src/actions.js**
```js
import { useStore } from 'react-appstate'

function useAction() {
  const [appState, setStore] = useStore()

  const Action = {}
  Action.increment = () => setStore({ count: appState.count + 1 })
  Action.decrement = () => setStore({ count: appState.count - 1 })

  return Action
}

export default useAction
```

### Multiple Stores

**・Play 👇**

[![Edit react-appstate-multiple-appState-example](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/react-appstate-multiple-appState-example-zwqxd?fontsize=14)

## TypeScript

This package contains an `index.d.ts` type definition file, so you can use it in TypeScript without extra configuration.

### Example

```typescript
import React, { ReactElement } from 'react'
import ReactDOM from 'react-dom'
import Provider, { useStore } from 'react-appstate'

interface Food {
  id: string
  name: string
}

type TodoList = Todo[]

interface Store {
  FoodList: FoodList
}

let initialStore: Store = {
  foodList: []
}

const App = () => {
const [appState, setStore] = useStore<Store>() // pass appState object type as generic
const item1: Food = {id: 'j4i3t280u', name: 'Hamburger'}
const item2: Food = {id: 'f83ja0j2t', name: 'Fried chicken'}
setStore({foodList: [item1, item2]})

const foodListView: ReactElement[] = appState.foodList.map((f: Food) => <p key={f.id}>{f}</p>)

return (<div>{foodListView}</div>)
}

ReactDOM.render(
    <Provider appState={initialStore}>
      <App>
    </Provider>,
  document.getElementById('root')
)
```

## LICENSE

MIT

## Contributors

Thank you to all these wonderful people ([emoji key](https://github.com/kentcdodds/all-contributors#emoji-key)):
I want to improve this library (especially stability) and your contribution is so helpful!

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore -->
<table>
  <tr>
    <td align="center"><a href="http://ryota-murakami.github.io/"><img src="https://avatars1.githubusercontent.com/u/5501268?s=400&u=7bf6b1580b95930980af2588ef0057f3e9ec1ff8&v=4" width="100px;" alt="ryota-murakami"/><br /><sub><b>ryota-murakami</b></sub></a><br /><a href="https://github.com/ryota-murakami/react-appstate/commits?author=ryota-murakami" title="Code">💻</a> <a href="https://github.com/ryota-murakami/react-appstate/commits?author=ryota-murakami" title="Documentation">📖</a> <a href="https://github.com/ryota-murakami/react-appstate/commits?author=ryota-murakami" title="Tests">⚠️</a></td>
    <td align="center"><a href="https://github.com/jackHedaya"><img src="https://avatars0.githubusercontent.com/u/20172754?v=4" width="100px;" alt="Jack Hedaya"/><br /><sub><b>Jack Hedaya</b></sub></a><br /><a href="https://github.com/ryota-murakami/react-appstate/commits?author=jackHedaya" title="Documentation">📖</a></td>
  </tr>
</table>

<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the [all-contributors](https://github.com/kentcdodds/all-contributors) specification. Contributions of any kind are welcome!
