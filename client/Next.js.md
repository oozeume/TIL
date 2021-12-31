# Next.js

React 기반 Framework
폴더 및 파일 기반 Routing 지원, Server Side Rendering 을 지원(SEO적용)
서버사이드렌더링은 결국 서버의 역할이 필요하다. 즉 Next는 자체적으로 서버를 가지고있다.

Next에서 pre-rendering(사전 렌더링)을 위해 두 가지 형식 제안

1. Static-Generation
   HTML을 빌드 타임에 각 페이지별로 생성, 해당 페이지로 요청이 올 경우 이미 생성된 HTML 반환
2. Server-Side-Rendering
   요청이 올 때마다 해당하는 HTML 문서를 그때 그때 생성하여 반환

정적 문서로 충분한 화면이면서 빠른 HTML 문서 반환이 필요하다면 SSG 방식, 매 요청마다 달라지는 화면이면서 서버 사이드로 이를 렌더링 하고자하면 SSR 방식을 사용한다.

## Data-Fetching

pageProps Data-Fetching 메소드를 통해 미리 가져온 초기 객체 데이터

## Pre-Rendering (사전 렌더링)

미리 HTML을 만들어 두는 것 -> 더 좋은 퍼포먼스, 검색엔진최적화
Pre-Rendering의 두가지 종류 정적 생성(SSG)과 서버사이드렌더링(SSR)
둘의 차이점은 어제 HTML파일을 생성하는가에 있다.

### 1) SSG (Static-Site-Generation)

Static Generation 방식의 경우 데이터 없이 실행하는 방법과 데이터와 함께 실행하는 방법으로 세분화 된다.

- 프로젝트가 빌드하는 시점에 HTML파일 생성하고 모든 요청에 재사용
- 정적 생성된 페이지들은 CDN에 캐시
- getStaticProps / getStaticPaths

유저가 페이지를 요청하는 시점이 미리 만들어놓아도 상관없으면 정적 생성 사용

### 2) SSR (Server-Side-Rendering)

- 매 요청마다 HTML파일 생성
- 항상 최신 상태 유지
- getServerSideProps

### 1-1. getStaticProps

특정 데이터와 함께 렌더링을 해야할 경우
pre-rendering으로 데이터를 받아서 정적 HTML를 생성해두고 다음 요청 떄 생성된 HTML을 반환

### 1-2. getStaticPaths

Dynamic Route에 Static Generation을 적용하여 각 제품군의 상세 페이지를 미리 정적 생성하고 싶은 경우 사용한다.
먼저 getStaticProps로 데이터를 가져온다. Static이기 때문에 매 요청마다 렌더링되지않고 가져온 데이터로 서버에 정적 HTML을 생성할 것이다.

### 1-3. getServerSideProps

getServerSideProps 내부는 브라우저 환경이 아니다. 서버에서 동작.

```jsx
// 예시 코드 추가필요
export async function getServerSideProps(context) {
  const id = context.params.id;
  const apiURL = `apiurl주소`;
  const res = await Axios.get(apiUrl);
  const data = res.data;

  return {
    props: {
      item: data,
    },
    // 응답값인 data(item)를 props로 사용할 수 있다.
  };
}
```

인자값인 context는 params나 요청, 응답, 쿼리등이 담겨서 온다.

<br>

## Dynamic Routing

create next-app 으로 생성하게되면, pages 폴더가 함께 들어있는데 라우팅 처리가 된다.
pages 폴더 -> view 폴더 -> [id].js 라고 파일을 작성하게되면
localhost:3000/view/[id] -> [id]에 어떤 값을 입력해도 같은 화면이 보이게된다.

상품 id가 달라도 하나의 페이지로 관리하고 next/link를 이용해 새로고침 없이 페이지간 이동 가능하다.

### \_app.js (Next에서 제공)

- 페이지 전환 시 레이아웃 유지
- 페이지 전환 시 상태값 유지
- componentDidCatch를 이용해서 커스텀 에러 핸들링
- 추가적인 데이터를 페이지로 주입 가능
- Global CSS 선언 가능

### \_document.js (Next에서 제공)

<Html>, <Head>, <body> 태그를 수정하여 사용해야할 때는 _document.js 파일을 필수적으로 사용해야한다. 
서버에서만 렌더링 되는 파일, onClick같은 이벤트 핸들러는 작동하지 않는다.

## create next-app 으로 설치하면

1. 컴파일과 번들링이 자동으로 된다(webpack, babel)
2. 자동 리프레시 기능으로 수정하면 화면에 바로 반영
3. SSR(Server-Side-Rendering) 지원
4. Static 파일 지원

## 서버사이드렌더링
