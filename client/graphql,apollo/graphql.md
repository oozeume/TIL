# GraphQL

데이터 베이스 또는 데이터 관리 시스템에 접근하기 위한 Query Language
GraphQL은 보통 하나의 엔드포인트 사용하며, 요청시 사용하는 쿼리문에 따라서 응답의 구조가 달라진다.

## GraphQL을 만든 이유

ios, android 등 다양한 기기에서 필요한 정보의 형태가 조금씩 달랐고 기존의 REST API로는 이것들을 일일히 구현하기 힘들었기때문에 정보를 요청하는 쪽에서 원하는 형태로 정보를 가져오고 수정할 수 있는 쿼리 언어를 만들게되었다.

## GraphQL 장점

- 필요한 정보만 선택적으로 받아올 수 있어서 over-fetching 문제 해결
- 여러 계층 정보 한 번에 받아올 수 있어서 under-fetching 문제 해결
- 요청 횟수 감소
- 하나의 endpoint에서 모든 요청을 처리
- 하나의 URI로 모든 요청 가능

## GraphQL로 정보 주고받기

## The Query and Mutation

### query

데이터를 받아올 때 사용
rest api로 치면 get 이랑 같음

### mutation

rest api로 치면 post,delete, put 이랑 같음

### subscription

websocket을 통해 실시간 양방향 통신 구현할 때 사용

## GraphQL 타입 시스템

### Scalar types 스칼라 타입

- ID : String, 고유 식별자 ID의 역할을 하는 것으로 이해
- String : UTF-8 문자열
- Boolean: true or false
- Int : 정수
- Float : 소수점 있는 값
- ! : 느낌표를 추가하면 null을 반환할 수 없다는 뜻

```jsx
type Character {
  name: String!
  appearsIn : [Episode]!
}
```

```jsx
type Character { // Character 타입 (객체 타입)
  name: String!
  // 필드 -> name
  // 느낌표를 붙이는 것 -> 이 필드를 쿼리할 때 항상 값을 반환한다는 뜻
  appearsIn : [Episode]!
  // 필드 -> appearsIn
  // Episode객체의 배열을 의미
}
```

### Enumeration types 열거형 타입

특정 값으로 제한되는 스칼라
타입의 인자가 허용된 값 중 하나임을 검증
필드가 항상 값의 열거형 집합 중 하나가 될 것

```jsx
const typedDefs = gql`
  enum Sex {
    male
    female
  }
  enum BloodType {
    A
    B
    AB
    O
  }
  enum Role {
    developer
    desinger
    planner
  }
  enum NewOrUsed {
    new
    used
  }
`;
```

### Lists 리스트 타입

특정한 타입의 배열을 반환

## Union types 유니온 타입

타입 여러개를 하나의 배열에 반환하고자 할 때

```jsx
const typeDefs = gql`
  union Given = Equipment | Supply
`;
// Given이라는 타입은 값으로 Equipment를 받을 수도 Supply를 받을 수도 있다.
```

## Input types 인풋 타입

인자들을 묶어서 실어보내는 것

## 예제

```jsx
// 페이지당 7개를 받아오는데 첫 페이지를 요청
query {
  peoplePaginated(page:1, per_page: 7){
    id
		first_name
		last_name
		sex
		blood_type
		role
  }
}
```
