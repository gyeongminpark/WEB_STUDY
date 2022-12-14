## πRedux κ°λμ λ¦¬

> <https://velog.io/@sanbondeveloper/React-Redux-%EA%B0%9C%EB%85%90-%EC%A0%95%EB%A6%AC>

<br />

## πReactμμ Redux μ¬μ©νκΈ°

> λ¦¬μ‘νΈ νλ‘μ νΈμμ λ¦¬λμ€λ₯Ό μ¬μ©ν  λ κ°μ₯ λ§μ΄ μ¬μ©νλ ν¨ν΄μ **νλ μ  νμ΄μλ μ»΄ν¬λνΈ**μ **μ»¨νμ΄λ μ»΄ν¬λνΈ**λ₯Ό λΆλ¦¬νλ κ²μ΄λ€.  
> **νλ μ  νμ΄μλ μ»΄ν¬λνΈ**λ `props`λ₯Ό λ°μ μμ νλ©΄μ UIλ₯Ό λ³΄μ¬μ£Όλ μ­ν μ νλ μ»΄ν¬λνΈλ₯Ό λ§νκ³ , **μ»¨νμ΄λ μ»΄ν¬λνΈ**λ λ¦¬λμ€μ μ°λλμ΄ μνλ₯Ό λ°μμ€κ³  μ€ν μ΄μ μ‘μμ λμ€ν¨μΉνλ μ»΄ν¬λνΈλ₯Ό λ§νλ€.  
> μ΄λ¬ν ν¨ν΄μ νμ μ¬ν­μ μλμ§λ§, μ½λ μ¬μ¬μ©μ±κ³Ό κ΄μ¬μ¬ λΆλ¦¬μ λμμ μ€ μ μλ€.

### β¨μ€μΉ

```bash
npm i redux react-redux
```

### β¨νλ μ  νμ΄μλ μ»΄ν¬λνΈ μμ±

- UI μ­ν μ νλ€.

```jsx
import React from 'react';

function Counter({ number, onIncrease, onDecrease }) {
  return (
    <div>
      <h1>{number}</h1>
      <div>
        <button onClick={onIncrease}>+1</button>
        <button onClick={onDecrease}>-1</button>
      </div>
    </div>
  );
}

export default Counter;
```

### β¨μ»¨νμ΄λ μ»΄ν¬λνΈ μμ±

- `connect`

- λ¦¬λμ€μ μ°λλ μ»΄ν¬λνΈ

```jsx
import React from 'react';
import Counter from '../components/Counter';
import { connect } from 'react-redux';
// μ‘μ μμ± ν¨μ
import { increase, decrease } from '../modules/counter';

function CounterContainer({ number, increase, decrease }) {
  return (
    // νλ μ  νμ΄μλ μ»΄ν¬λνΈ λ λλ§
    <Counter number={number} onIncrease={increase} onDecrease={decrease} />
  );
}

const mapStateToProps = (state) => ({
  number: state.counter.number,
});

const mapDispatchToProps = (dispatch) => ({
  increase: () => {
    dispatch(increase());
  },
  decrease: () => {
    dispatch(decrease());
  },
});

// λ¦¬λμ€μ μ»΄ν¬λνΈ μ°λ
// connect ν¨μλ ν¨μλ₯Ό λ°ν
export default connect(mapStateToProps, mapDispatchToProps)(CounterContainer);
```

### β¨Redux λͺ¨λ μμ±

- μ‘μ νμ, μ‘μ μμ± ν¨μ, λ¦¬λμ ν¨μλ₯Ό κΈ°λ₯λ³λ‘ νλμ νμΌμ μμ±νλ λ°©μ μ¬μ© (**Ducks ν¨ν΄**)

```jsx
// μ‘μ μ μ - 'λͺ¨λ μ΄λ¦/μ‘μλͺ' νμμ κΆμ₯
const INCREASE = 'counter/INCREASE';
const DECREASE = 'counter/DECREASE';

// μ‘μ μμ± ν¨μ
// dispatch νΈμΆμ μ¬μ©
export const increase = () => ({ type: INCREASE });
export const decrease = () => ({ type: DECREASE });

// μ΄κΈ° μν κ°
const initialState = {
  number: 0,
};

// λ¦¬λμ ν¨μ
export default function counter(state = initialState, action) {
  switch (action.type) {
    case INCREASE:
      return {
        number: state.number + 1,
      };
    case DECREASE:
      return {
        number: state.number - 1,
      };
    default:
      return state;
  }
}
```

### β¨λ£¨νΈ λ¦¬λμ μμ±

- `combineReducers` ν¨μ μ¬μ©

- `createStore` ν¨μλ₯Ό ν΅ν΄ μ€ν μ΄λ₯Ό μμ±ν  λ νλμ λ¦¬λμ ν¨μλ§ μ¬μ©ν  μ μλ€.

- λ°λΌμ λ¦¬λμ ν¨μκ° μ¬λ¬κ°μΌ κ²½μ° μ΄λ€μ νλμ λ¦¬λμλ‘ ν©μ³ μ£Όμ΄μΌ νλ€.

```jsx
import { combineReducers } from 'redux';
import counter from './counter';
import todos from './todos';

// νλμ λ¦¬λμ ν¨μλ‘ ν©μ³μ£ΌκΈ°
const rootReducer = combineReducers({
  counter,
  todos,
});

export default rootReducer;
```

### β¨μ€ν μ΄ μμ±

- λ¦¬μ‘νΈ μ νλ¦¬μΌμ΄μμ λ¦¬λμ€λ₯Ό μ€μ§μ μΌλ‘ μ μ©νλ κ³Όμ 

- μ£Όλ‘ `src/index.js`μμ κ΅¬ν

```jsx
const store = createStore(rootReducer);
```

#### Redux DevTools μ μ©νκΈ°

```jsx
// μΌλ°
const store = createStore(
  rootReducer,
  // μ‘°κ±΄λΆ
  window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__(),
);

// λΌμ΄λΈλ¬λ¦¬ μ¬μ©
//  npm i redux-devtools-extension
import { composeWithDevTools } from 'redux-devtools-extension';

const store = createStore(rootReducer, composeWithDevTools());
```

### β¨Provider μ»΄ν¬λνΈ

- `react-redux`μμ μ κ³΅

- Context APIμ λμΌνκ² `Provider`λ‘ νμ μ»΄ν¬λνΈλ₯Ό κ°μΈμ `store`μ μνκ°μ μ¬μ©ν  μ μλλ‘ νλ€.

```jsx
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  // Provider μ»΄ν¬λνΈλ₯Ό ν΅ν΄ νμ μ»΄ν¬λνΈλ‘ store μ λ¬
  <Provider store={store}>
    <App />
  </Provider>,
);
```

### β¨useSelectorμ useDispatch

- Hookμ μ΄μ©ν λ°©λ²

- `connect` λ°©μκ³Ό λ€λ₯΄κ² μ΄ λ°©λ²μ κ²½μ° μ±λ₯ μ΅μ ν(λΆλͺ¨ μ»΄ν¬λνΈ λ¦¬λ λλ§μ λ°λ₯Έ) μ΄λ£¨μ΄μ§μ§ μλλ€.

- λ°λΌμ μ§μ  `React.memo`λ₯Ό μ¬μ©ν΄μΌ νλ€.

```js
import React, { useCallback } from 'react';
import { useSelector, useDispatch } from 'react-redux';
import Counter from '../components/Counter';
// μ‘μ μμ± ν¨μ
import { increase, decrease } from '../modules/counter';

function CounterContainer() {
  const number = useSelector((state) => state.counter.number);
  const dispatch = useDispatch();

  // λ¦¬λ λλ§ λ  λλ§λ€ μ½λ°± ν¨μκ° μ¬μμ±λλ κ²μΌ λ°©μ§
  const onIncrease = useCallback(() => dispatch(increase()), [dispatch]);
  const onDecrease = useCallback(() => dispatch(decrease()), [dispatch]);
  return (
    // νλ μ  νμ΄μλ μ»΄ν¬λνΈ λ λλ§
    <Counter number={number} onIncrease={onIncrease} onDecrease={onDecrease} />
  );
}

export default React.memo(CounterContainer);
```

### β¨useStore

- μ€ν μ΄μ μ§μ  μ κ·Όνλ λ°©λ²

```jsx
import { useStore } from 'react-redux';

const store = useStore();
console.log(store.getState());
```
