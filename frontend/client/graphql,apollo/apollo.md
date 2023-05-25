# Apollo

GraphQL의 클라이언트 라이브러리 중 하나이다.  
GraphQL을 사용한다면 거의 필수적으로 사용하는 상태 관리 플랫폼  
frontend(apollo-client), backend(apollo-server)모두 제공  
Apollo Client는 리액트, ios, android에서 모두 사용가능

## Apollo Client - react

- 메모리 캐싱 in-memory cache
- 로컬 상태관리 local state management
- 에러 핸들링
- 리액트 뷰 레이어

## 0.Setup

```bash
yarn add @apollo/client graphql
```

</br>

## 1. Apollo Client 생성 - ApolloClient 생성자

ApolloClient 생성자를 이용해서 ApolloClient 생성.  
ApolloClient 생성자는 두 개의 옵션(link, cache)을 가진 객체를 인자로 받는다.

```jsx
import { ApolloClient, InMemoryCache } from '@apollo/client';
import { createHttpLink } from 'apollo-link-http';

const client = new ApolloClient({
  link: createHttpLink({ uri: 'graphyQL API의 endpoint정보' }),
  cache: new InMemoryCache(),
});
```

createHttpLink는 HTTP를 통해 원격 GraphQL 서버와 연동할 수 있도록 HttpLink 객체를 생성해주는 팩토리 함수  
이 함수의 인자로 연동할 GraphQL 서버의 uri를 설정해줘야 한다.

cache는 InMemoryCache를 통해 생성된 인스턴스로, 쿼리 결과를 가져온 후 캐싱한다.  
client를 생성하고 나면 data를 fetching할 준비가 된다.

</br>

## 2. 리액트에 Apollo Client 연결 → ApolloProvider 컴포넌트

생성한 ApolloClient 객체를 React에 연결한다.  
앱 내의 특정 컴포넌트만 GraphQL API호출이 필요한 것이 아니라면 모든 리액트 앱 내 컴포넌트에서 ApolloClient를 사용할 수 있도록 App.js파일(혹은 최상위 컴포넌트)에 작성해준다.

ApolloProvider 컴포넌트로 최상위 컴포넌트를 감싸주고 client prop으로 ApolloClient 객체를 넘겨준다.  
이렇게 해주면 리액트 앱 내의 모든 컴포넌트에서 GraphQL API 연동이 가능하다.

@apollo/client package에 내장되어 있으므로 import 해서 사용

```jsx
import { ApolloClient, InMemoryCache, ApolloProvider } from '@apollo/client';
import { createHttpLink } from 'apollo-link-http';

const client = new ApolloClient({
  link: createHttpLink({ uri: 'graphyQL API의 endpoint URL' }),
  cache: new InMemoryCache(),
});

function App() {
  return (
    <ApolloProvider client={client}>
      <App />
    </ApolloProvider>
  );
}
```

</br>

## 3. GraphQL API 호출 → useQuery

데이터 요청을 위해 useQuery를 사용해 GraphQL쿼리를 인자로 넘겨서 호출한다.  
useQuery함수는 응답 데이터 뿐 아니라 로딩 여부, 오류 데이터까지 함께 객체로 리턴한다.

```jsx
const { loading, error, data } = useQuery(GET_NOTE);
```

응답을 기다리는 동안에는 loading 프로퍼티가 true이므로 사용자에게 로딩중이라는 메세지를 표시해줄 수 있다.  
또는 호출이 실패된 경우에는 error 프로퍼티 값을 읽어서 예외처리를 해줄 수 있다.

```jsx
// 편의상 하나의 쿼리, 타입선언을 하나의 파일 안에 작성하였습니다.
// App.tsx

import { useQuery, gql } from '@apollo/client';

interface NoteItem {
  id: number;
  content: string;
}

interface NoteList {
  note: NoteItem[];
}

const GET_NOTE = gql`
  query getQuery {
    getNote {
      id
      content
    }
  }
`;

function MainComponent() {
  const { loading, error, data } = useQuery < NoteList > GET_NOTE;

  return (
    <>
      {loading ? (
        <p> Loading... </p>
      ) : (
        <div>
          {data &&
            data.note.map((item) => <h1 key={item.id}>{data.content}</h1>)}
        </div>
      )}
    </>
  );
}
```
