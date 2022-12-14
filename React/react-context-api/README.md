## ๐Context API

- React ํ๋ก์ ํธ์์ ์ ์ญ์ ์ผ๋ก ์ฌ์ฉํ  ๋ฐ์ดํฐ๊ฐ ์์ ๋ ์ ์ฉํ ๊ธฐ๋ฅ

- ์ฌ์ฉ์ ๋ก๊ทธ์ธ ์ ๋ณด, ์ ํ๋ฆฌ์ผ์ด์ ํ๊ฒฝ ์ค์ , ํ๋ง ์คํ์ผ(๋คํฌ ๋ชจ๋ ๋ฑ)

- redux, react-router, styled-components ๋ฑ์ ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ **Context API**๋ฅผ ๊ธฐ๋ฐ์ผ๋ก ๊ตฌํ๋์ด ์๋ค.

- Context API๋ฅผ ์ฌ์ฉํ๋ฉด `props`๋ฅผ ์ ๋ฌํ  ํ์์์ด ์ปดํฌ๋ํธ์์ ํด๋น ๊ฐ์ ๋ฐ๋ก ๊ฐ์ ธ์ฌ ์ ์๋ค

- `Provider` ํ์์์ context๋ฅผ ๊ตฌ๋ํ๋ ๋ชจ๋  ์ปดํฌ๋ํธ๋ `value` prop์ด ๋ณ๊ฒฝ๋  ๋๋ง๋ค ๋ค์ ๋ ๋๋ง๋๋ค. (์ฑ๋ฅ ์ ํ)

### โจcreateContext

- **Context ์์ฑ**

- ํ๋ผ๋ฏธํฐ๋ ์์ ์๋ ์๊ณ  ์์ ์๋ ์๋ค. (์์ ๊ฒฝ์ฐ `Provider` ๋ฏธ์ฌ์ฉ์ Context์ ๊ธฐ๋ณธ ์ํ๋ฅผ ์ง์ )

```jsx
import { createContext } from 'react';

const ColorContext = createContext({ color: 'black' });

export default ColorContext;
```

### โจConsumer

- **์ ์ญ ๋ฐ์ดํฐ ๊ฐ์ ธ์ค๊ธฐ**

- `value`๊ฐ ์ ์ญ ๋ฐ์ดํฐ์ ํด๋น๋๋ค.

```jsx
import ColorContext from '../contexts/color';

<ColorContext.Consumer>
  {(value) => (
    <div
      style={{
        width: '64px',
        height: '64px',
        background: value.color,
      }}
    />
  )}
</ColorContext.Consumer>;
```

### โจProvider

- ์ ์ญ ๋ฐ์ดํฐ ๊ฐ์ ๋ณ๊ฒฝํ  ์ ์๋ค.

- `Provider` ์ฌ์ฉ์ `value`๋ฅผ ๋ช์ํด์ฃผ์ง ์์ผ๋ฉด ์ค๋ฅ ๋ฐ์

- `Provider` ๋ด๋ถ์ ์์ฑ๋ ์ปดํฌ๋ํธ์์๋ง ์ ์ญ ๋ฐ์ดํฐ์ ์ ๊ทผ ๊ฐ๋ฅ

```jsx
import ColorContext from './contexts/color';

<ColorContext.Provider value={{ color: 'red' }}>
  <div>
    <ColorBox />
  </div>
</ColorContext.Provider>;
```

### โจ๋์  Context

- **ํจ์**๋ฅผ ์ ์ญ ๋ฐ์ดํฐ๋ก ์ ๋ฌํ  ์๋ ์๋ค.

- Context๋ฅผ ๋์ ์ผ๋ก ๋ณ๊ฒฝํ  ์ ์๋ค.

```jsx
// color.js

import { createContext, useState } from 'react';

const ColorContext = createContext({
  state: { color: 'black', subcolor: 'red' },
  actions: {
    setColor: () => {},
    setSubColor: () => {},
  },
});

const ColorProvider = ({ children }) => {
  const [color, setColor] = useState('black');
  const [subColor, setSubColor] = useState('red');

  const value = {
    state: { color, subColor },
    actions: { setColor, setSubColor },
  };

  return (
    <ColorContext.Provider value={value}>{children}</ColorContext.Provider>
  );
};

const { Consumer: ColorConsumer } = ColorContext;

export { ColorProvider, ColorConsumer };

export default ColorContext;
```

### โจuseContext Hook ์ฌ์ฉ

- React์์ ๊ธฐ๋ณธ์ ์ผ๋ก ์ ๊ณตํด์ฃผ๋ Hook

- `Consumer` ๋์  ์ฌ์ฉํ  ์ ์๋ค.

```jsx
import React, { useContext } from 'react';
import ColorContext from '../contexts/color';

const colors = ['red', 'orange', 'yellow', 'green', 'blue', 'indigo', 'violet'];

export default function SelectColor() {
  const value = useContext(ColorContext);

  return (
    <div>
      <h2>์์ ์ ํ</h2>
      <div style={{ display: 'flex' }}>
        {colors.map((color) => (
          <div
            key={color}
            style={{
              background: color,
              width: '24px',
              height: '24px',
              cursor: 'pointer',
            }}
            onClick={() => value.actions.setColor(color)}
            onContextMenu={(e) => {
              e.preventDefault();
              value.actions.setSubColor(color);
            }}
          />
        ))}
      </div>
      <hr />
    </div>
  );
}
```

### โจuseReducer ์ฌ์ฉ

- `useState` ๋์  `useReducer` ์ฌ์ฉํด๋ณด๊ธฐ

```jsx
import { createContext, useReducer } from 'react';

const ColorContext = createContext();

const initialState = {
  color: 'black',
  subColor: 'red',
};

const colorReducer = (state, action) => {
  switch (action.type) {
    case 'changeColor':
      return { ...state, color: action.text };
    case 'changeSubColor':
      return { ...state, subColor: action.text };
    default:
      break;
  }
};

const ColorProvider = ({ children }) => {
  const [colorState, dispatch] = useReducer(colorReducer, initialState);

  const value = {
    colorState,
    dispatch,
  };

  return (
    <ColorContext.Provider value={value}>{children}</ColorContext.Provider>
  );
};

const { Consumer: ColorConsumer } = ColorContext;

export { ColorProvider, ColorConsumer };

export default ColorContext;
```
