# styled-components typescript로 세팅하기

```
yarn add styled-components @types/styled-components styled-normalize
```

## global style type 작성

global에 원하는 css를 작성하기 전에, type을 선언합니다.

```javascript
import 'styled-components';

declare module 'styled-components' {
  export interface DefaultTheme {
    textColor: string;
    cardbgColor: string;
    bgColor: string;
    accentColor: string;
  }
}
```

## global style 작성

global에 원하는 css를 작성합니다.

```javascript
import { createGlobalStyle } from 'styled-components';

const GlobalStyle = createGlobalStyle`
  html, body, div, span, applet, object, iframe,
  h1, h2, h3, h4, h5, h6, p, blockquote, pre,
  a, abbr, acronym, address, big, cite, code,
  del, dfn, em, img, ins, kbd, q, s, samp,
  small, strike, strong, sub, sup, tt, var,
  b, u, i, center,
  dl, dt, dd, ol, ul, li,
  fieldset, form, label, legend,
  table, caption, tbody, tfoot, thead, tr, th, td,
  article, aside, canvas, details, embed, 
  figure, figcaption, footer, header, hgroup, 
  menu, nav, output, ruby, section, summary,
  time, mark, audio, video {
    margin: 0;
    padding: 0;
    border: 0;
    font-size: 100%;
    font: inherit;
    vertical-align: baseline;
  }
  /* HTML5 display-role reset for older browsers */
  article, aside, details, figcaption, figure, 
  footer, header, hgroup, menu, nav, section {
    display: block;
  }
  body {
    line-height: 1;
  }
  ol, ul {
    list-style: none;
  }
  blockquote, q {
    quotes: none;
  }
  blockquote:before, blockquote:after,
  q:before, q:after {
    content: '';
    content: none;
  }
  table {
    border-collapse: collapse;
    border-spacing: 0;
  }
  * {
    box-sizing: border-box;
  }
  body {
    font-family: 'Source Sans Pro', sans-serif;
    background-color: ${(props) => props.theme.bgColor};
    color: ${(props) => props.theme.textColor};
  }
  a {
    text-decoration: none;
    color: inherit;
  }

  function App() {
    return (
      <GlobalStyle />
    );
  }
`;

export default GlobalStyle;
```

## 많이 사용하는 css 변수로 등록하는 theme 작성

```javascript
import { DefaultTheme } from 'styled-components';

export const darkTheme: DefaultTheme = {
  bgColor: '#2d3436',
  cardbgColor: 'whitesmoke',
  textColor: 'black',
  accentColor: '#ff7675',
};

export const lightTheme: DefaultTheme = {
  bgColor: 'whitesmoke',
  cardbgColor: 'white',
  textColor: 'black',
  accentColor: '#ff7675',
};
```

## 만든 theme 적용

```javascript
import { ThemeProvider } from 'styled-components';
import { theme } from './theme.ts';

ReactDOM.render(
  <ThemeProvider theme={theme}>
    <App />
  </ThemeProvider>,
  document.getElementById('root')
);
```
