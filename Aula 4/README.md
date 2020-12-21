# Aula 4 - Treinamento - JavaScript e ReactJS

O foco desta aula é entendermos como os componentes da aplicação necessitam, muitas vezes, compartilhar informações entre si.

Esses componentes podem estar "próximos" na árvore do Virtual DOM, o que nos pode levar a tentar realizar esse compartilhamento por meios da propriedades (`props`). No entanto, quando esses componentes estão em contextos e locais diferentes no app, devemos fazer com que ambos se comuniquem com uma mesma entidade, responsável por gerenciar os dados (ou estado) naquele contexto.

## Referências
 - [ReduxJs](https://redux.js.org/)
 - [React Redux](https://react-redux.js.org/)
 - [ReduxJs - Async Logic](https://redux.js.org/tutorials/essentials/part-5-async-logic#thunks-and-async-logic)

## Assuntos Abordados

* Gerenciamento do Estado
  - [Context API](#context-api)
  - Redux Pattern

* Implementação no React
  - React Redux

* Handson

## Context API

**Criando um contexto**

```jsx
import { createContext } from 'react'

const AppearanceContext = createContext({
  primaryColor: 'cornflowerblue',
  setPrimaryColor: () => {}
})

export default AppearanceContext
```

**Provendo um contexto à uma subárvoce do Virtual DOM**

```jsx
const MyComponent = () => {
  ...

  return (
    <AppearanceContext.Provider
      value={{
        primaryColor: ???,
        setPrimaryColor: ???
      }}
    >
      ...
    </AppearanceContext.Provider>
  );
};
```

**Acessando os dados do contexto**
```jsx

const PageContent = () => {
  const { primaryColor } = useContext(AppearanceContext);
  ...
}
```

**Atualizando o Contexto**

```jsx
const AppSection = () => {
  const [primaryColor, setPrimaryColor] = useState("#ff00ff");

  return (
    <AppearanceContext.Provider
      value={{
        primaryColor,
        setPrimaryColor
      }}
    >
    ...
```
```jsx
const ColorPicker = () => {
  const { primaryColor, setPrimaryColor } = useContext(AppearanceContext);

  return (
    <input
      type="color"
      value={primaryColor}
      onChange={(event) => setPrimaryColor(event.target.value)}
    />
  );

};
```

**Reagindo a Alterações no Contexto**

```jsx
const ColorPicker = () => {
  const { primaryColor } = useContext(AppearanceContext);

  useEffect(() => {
    console.log(primaryColor)
  }, [primaryColor])

  ...
```

[**EXEMPLO**](https://codesandbox.io/s/context-api-example-xe055?file=/src/App.js)

## Redux Pattern

### *Store* Global

![Redux Pattern](https://blog.codecentric.de/files/2017/12/Bildschirmfoto-2017-12-01-um-08.53.32.png)

### Visão Geral

![Redux Pattern Components](https://www.dotnetcurry.com/images/reactjs/redux/redux.png)

### Actions

```js
// actionsTypes.js

export const INCREMENT = "INCREMENT";
export const DECREMENT = "DECREMENT";
export const RESET = "RESET";
export const ADD = "ADD";
```

```js
// actions.js

import { ADD, DECREMENT, INCREMENT, RESET } from "./actionTypes";

export function increment() {
  return {
    type: INCREMENT
  };
}

export function decrement() {
  return {
    type: DECREMENT
  };
}

export function reset() {
  return {
    type: RESET
  };
}

export function add(amount) {
  return {
    type: ADD,
    payload: amount
  };
}
```

### Reducer

```js
import { ADD, DECREMENT, INCREMENT, RESET } from "./actionTypes";

const initialState = 0;

export default function (state = initialState, action) {
  switch (action.type) {
    case INCREMENT:
      return state + 1;
    case DECREMENT:
      return state - 1;
    case RESET:
      return 0;
    case ADD:
      return state + action.payload;
    default:
      return state;
  }
}
```

### Store

```js
import { createStore } from "redux";
import reducer from "./reducer";

export default createStore(reducer);
```

### Acessando o Estado
```js
import store from "../redux/store";

store.getState()
```

### Alterando o Estado por meio de Ações
```js
import { decrement } from "../redux/actions";
import store from "../redux/store";

...

store.dispatch(decrement())

...
```

### Escutando por Alteração no Estado

```js
import React, { useEffect, useState } from "react";
import store from "../redux/store";

export default function CounterApp() {
  const [counter, setCounter] = useState(store.getState());

  useEffect(() => {
    store.subscribe(() => {
      setCounter(store.getState());
    });
  }, []);

  return <h1>Counter: {counter}</h1>;
}
```

## React Redux

Populando o `store` aos nossos componentes.
```js
const store = createStore(rootReducer)

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```

Mapeando cada `dispatch` e estado em propriedades do componente.

```js
const TodoList = () => {
  ...
}

const mapStateToProps = (state) => {
  return {
    todos: state
  };
};

const mapDispatchToProps = {
  addTodo
};

export default connect(mapStateToProps, mapDispatchToProps)(TodoList);
```

### [Hooks](https://react-redux.js.org/api/hooks)

```js
const counter = useSelector(state => state.counter)
```

```js
const dispatch = useDispatch()
```

```js
const store = useStore()
```

### Alterando Estado sem `connect`

```js
const dispatch = useDispatch()
const incrementCounter = useCallback(
  () => dispatch({ type: 'increment-counter' }),
  [dispatch]
)

incrementCounter()
```

## [**HANDSON**](https://codesandbox.io/s/black-worker-ljv8u)

## Extensões

- [Redux-Saga](https://redux-saga.js.org/)
- [Redux-Thunk](https://www.npmjs.com/package/redux-thunk)
- [DVA](https://dvajs.com/api/#%E8%BE%93%E5%87%BA%E6%96%87%E4%BB%B6)
