## 변수의 타입 선언

```
let name:string = 'kim'
```

변수의 이름 옆에 콜론을 적고 타입을 정한다.

## 배열의 타입 선언

```
let name:string[] = ['a', 'b', 'c', 'd'] // string으로 구성된 배열
```

변수명 뒤에 타입과 빈배열을 적어준다.

## 함수의 parameter와 return값의 타입 정하기

```
function num(x: number):number {
  return x * x
}
```

파라미터의 타입 지정 - 파라미터 안에서 타입 선언
리턴 값의 타입 지정 - 파라미터 옆에 콜론을 붙여서 타입 선언

## 제네릭 Generic

함수에서 타입을 지정할 때 제네릭을 이용해서 다양한 타입을 함수마다 적용할 수 있다.

## Tuple

배열 안에서 타입을 다르게 사용하고 싶을 때

```tsx
let b: [string, number];
b = ['z', 1];
```

## void

함수에서 아무것도 반환하지 않을 때 사용

```tsx
function sayHello():void {
console.log('hello);
}
```

## never

에러를 반환하거나 영원히 끝나지 않는 함수의 타입으로 사용 가능

## enum

```tsx
enum Os {
  Window = 'win',
  Ios = 'ios',
  Android = 'and',
}

// myOs의 타입으로 enum인 Os를 지정
let myOs: Os;

// Os 안에있는 세가지만 사용할 수 있다.
myOs = Os.Window;
```

## null, undefined

```tsx
let a: null = null;
let b: undefined = undefined;
```

---

# interface

interface는 TypeScript 코드가 실행되기 전에 확인해준다.

```tsx
interface User {
  name: string;
  age: number;
}

let user: User = {
  name: 'kim',
  age: 30,
};
```

### optional

같은 속성을 가진 interface 안에서 어떤 요소에만 다른 값을 주고싶을 때 사용한다.

```tsx
interface User {
  name: string;
  age: number;
  gender?: string; // optional - ?를 붙인다
}

let user: User = {
  name: 'kim',
  age: 30,
};
```

### readonly

읽기 전용, 수정 불가능

```tsx
interface User {
  name: string;
  age: number;
  gender?: string;
  readonly birthYear: number;
}

let user: User = {
  name: 'kim',
  age: 30,
  birthYear: 2000, // 생성할 때만 할당 가능, 이후에 재할당 불가
};
```

### 문자열을 여러개 받을 때

```tsx
type Score = 'A' | 'B' | 'C' | 'D';

interface User {
  name: string;
  age: number;
  gender?: string;
  readonly birthYear: number;
  [grade: number]: Score;
}

let user: User = {
  name: 'kim',
  age: 30,
  birthYear: 2000,
  1: 'A',
  2: 'B',
};
```

## 함수에 대한 interface 정의

```tsx
interface Add {
  (num1: number, num2: number): number; // ( 인자 ): 함수의 return값
}

const add: Add = function (x, y) {
  return x + y;
};
```

```tsx
interface isAdult {
  (age: number): boolean;
}
const a: isAdult = (age) => {
  return age > 19;
};

a(33); // true
```

## Union Type 유니온 타입 (연산자를 이용한 타입 정의)

유니온 타입(Union Type)이란 자바스크립트의 OR 연산자(||)와 같이 A이거나 B이다 라는 의미의 타입이다.
(타입을 여러개 정하고 싶은 경우)

```
let name:(string | number) = 'kim'
```

OR의 의미로 사용한다. 둘 중 하나의 값으로 들어온다.

## interface로 클래스 정의 - implements
