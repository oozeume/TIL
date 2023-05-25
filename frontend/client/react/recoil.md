- react 자체 라이브러리
- react를 지원하는 전용 상태관리이므로 react 내부에 접근이 가능하여 react의 동시성모드, suspense에 손쉽게 지원 가능

## recoil 사용을 위한 세팅

프로젝트 최상단 위치에 RecoilRoot 설정해주어야한다.
RecoilRoot로 감싸주면 자동으로 store 생성

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import { RecoilRoot } from 'recoil'; // recoil 추가
import './index.css';
import App from './App';

const app = document.getElementById('root');
ReactDOM.render(
  <React.StrictMode>
    <RecoilRoot>
      <App />
    </RecoilRoot>
  </React.StrictMode>,
  app
);
```

## Atoms

상태의 단위, 업데이트와 구독이 가능하다.
atom이 변경(업데이트)되면 컴포넌트도 변경된 값으로 리렌더링

- atom함수를 사용해 생성

```javascript
const fontSizeState = atom({
  key: 'fontSizeState', // 전역으로 고유한 값의 키 필요
  default: 14, // 기본값
});
```

### useRecoilValue()

- atom의 value를 읽기위해 사용하는 hook
- setter 없이 상태값만 반환

### useRecoilState()

- atom의 value를 수정하기(상태를 읽고 쓸 때) 위해 사용
- setting해주는 함수 필요 [상태, setter] 반환
- 첫 요소가 상태 값이며, 두 번째 요소가 호출되었을 때 주어진 값을 업데이트하는 setter함수인 tuple을 리턴한다.
- react의 useState()와 비슷하지만 상태가 컴포넌트 간에 공유될 수 있다는 차이가 있다. (useState()는 인수를 default값을 주고, recoil의 useRecoilState()는 상태(value)를 인수로 갖는다.)

```javascript
functio FontButton() {
  const [fontSize, setFontSize] = useRecoilState(fontSizeState); // fontSizeState는 위의 atom파일을 따로 설정해주었다.
  return (
    <button onClick={() => setFontSize((size) => size+1)} style={{fontSize}}>
      Click
    </button>
  )
}
```

버튼을 클릭하면 버튼 글꼴 크기가 1만큼 증가하며, fontSizeState atom을 사용하는 다른 컴포넌트도 같이 변한다.

```javascript
function Text() {
  const [fontSize, setFontSize] = useRecoilState(fontSizeState);
  return <p style={{ fontSize }}>This text will increase in size too.</p>;
}
```
