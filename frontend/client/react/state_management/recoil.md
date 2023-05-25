# Recoil

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

- 상태의 단위, 업데이트와 구독이 가능하다.
- atom이 변경(업데이트)되면 이를 구독하고있는 컴포넌트도 변경된 값으로 리렌더링된다.
- atom함수를 사용해 생성
  atom을 생성하기 위해 어플리케이션에서 고유한 키 값과 디폴트 값을 설정해야한다.

  ```javascript
  const fontSizeState = atom({
    key: 'fontSizeState', // 전역으로 고유한 값의 키 필요
    default: 14, // 기본값(초기값)
  });
  ```

- 모든 컴포넌트가 atom()에 연결되어서, 서로를 의존하지 않는다.

## useRecoilValue()

- atom의 value를 읽기위해 사용하는 hook
- setter 없이 상태값만 반환

## useRecoilState()

- atom의 value를 읽고, 수정하기(상태를 읽고 쓸 때, Recoil에서는 구독이라고 한다) 위해 사용
- react의 useState()와 비슷하지만 상태가 컴포넌트 간에 공유될 수 있다는 차이가 있다. (useState()는 인수를 default값을 주고, recoil의 useRecoilState()는 상태(value)를 인수로 갖는다.)
- 첫 요소가 상태 값이며, 두 번째 요소가 호출되었을 때 주어진 값을 업데이트하는 setter함수인 tuple을 리턴한다.
- atom의 state에 어떤 컴포넌트에서든 전역으로 받아올 수 있다.

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

## useSetRecoilState()

- atom의 value를 수정하기위해 사용하는 hook
- setter 함수만 반환한다.

## Selectors

- state를 입력 받아서 그걸 변형해 반환하는 순수함수를 거쳐 반환된 값
  selector를 이용해서 분류할 수 있다. (순수함수: 같은 입력이 들어오면, 해당 입력에 대한 출력을 항상 같은 함수 )
- atom의 output을 변형시킨다.
- atom에 의존하는 동적인 데이터를 만들 수 있게 해준다.

### get 프로퍼티

- "get"함수를 가지고 있는데, derived-state(파생된 데이터)를 return하는 곳이다. 원래의 state를 그냥 가져오는 것이 아닌, 'get' 프로퍼티를 통해 state를 가공하여 반환할 수 있다.
- get을 사용해 atom, 다른 selector에 접근 가능하다. 종속 관계가 생기므로 참조한 값이 업데이트 되면 해당 함수도 다시 실행된다.
