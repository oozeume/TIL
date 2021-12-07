# TypeScript의 클래스

## 클래스의 타입을 지정하기

클래스를 생성할 때, 인스턴스의 속성을 정의하기 위해서 constructor를 사용해야한다.

```
class Person {
  name: string;
  constructor(a: string){
    this.name = a;
  }
}

let person1 = new Person('kim')
let person2 = new Person('park')
```

constructor 안에서 값을 생성하면, 이 Person이라는 클래스로부터 생성된 모든 인스턴스들은 this.name의 값을 사용할 수 있게된다.
타입스크립트에서 constructor값을 사용하기 위해서는 필드에 값을 미리 생성해주어야한다.

## 속성 - 접근 제어 속성 설정

- 접근제어자를 통해 접근 가능한 범위를 설정
- 각 속성에 데이터 타입 지정 가능

```tsx
class Book {
  // public : 클래스 외부에서 접근 가능(기본값으로 생략가능)
  public title: string;

  // private : Book 클래스 내부에서만 접근 가능
  private author: string;

  // protected : Book 클래스를 포함한 서브 클래스에서만 접근 가능
  protected paper_type: string;

  // constructor () 매개변수 앞에 public, private, protectedfmf 사용하면 Book 클래스의 타입을 별도 선언하지 않아도 된다.
  constructor(title: string, public pages: number) {
    this.title = title;
    this.pages = pages;
  }
}

// 인스턴스 생성
let indRevo = new Book('책이름', 250);
console.log(indRevo); // Book {}
```

## 메서드 - 접근 제어 메서드 설정

- 접근 제어자를 사용해 외부에서의 접근을 제어할 수 있다.

```tsx
class Book {
  public title: string;
  private author: string;
  public pages: number = 150;
  protected paper_type: string = '밍크지';

  constructor(title: string, pages: number) {
    this.title = title;
    this.pages = pages;
  }

  // 메서드
  //public 메서드 - 클래스 외부에서 접근 가능
  public printPages(): string {
    return `${this.page}페이지`;
  }

  //private 메서드 - Book 클래스 내부에서만 접근 가능
  private printAuthor(authorName: string): void {
    this.author = authorName;
  }
}

// 인스턴스 생성
let indRevo = new Book('책이름', 250);
console.log(indRevo.printPages()); // '250페이지'
```

## 상속

TypeScript는 ES6와 마찬가지로 클래스 상속에 extends 키워드를 사용한다.
constructor()를 사용해 상속 받은 수퍼 클래스의 생성자를 서브 클래스의 생성자로 덮어쓸 수 있다. 이 때 super()를 사용해 수퍼 클래스의 생성자에 요구되는 인자를 전달해야한다.

```tsx
class E_Book extends Book {
  paper_type = '스크린';
  constructor(title: string, pages: number, public is_downloadable: boolean) {
    super(title, pages);
    this.is_downloadable = is_downloadable;
  }
}

// 수퍼 클래스의 protected 속성은 접근 가능
console.log(this.paper_type);
```

수퍼 클래스의 protected 속성 - 서브 클래스에서 접근 가능
private 속성 - 서브 클래스에서 접근 불가능(여기서는 부모인 Book 안에서만 접근가능)
