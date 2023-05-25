# webpack

코드의 양이 많아지면 코드의 유지보수가 쉽도록 코드를 모듈로 나누어 관리하는 모듈 시스템이 필요해진다. 그러나 Javascript는 언어 자체가 지원하는 모듈 시스템이 없다. webpack은 이런 한계를 극복하려고 이용하는 도구 중 하나이다.

### webpack의 역할 (번들러)

모듈로 연결된 여러개의 자바스크립트 파일을 하나로 합쳐줌 (번들러)
webpack으로 컴파일해서 브라우저에서 실행할 수 있는 형태로 바꿔야한다.

## Installation

```bash
npm install -g webpack
npm install --save-dev webpack webpack-dev-server
#or
yarn global add webpack
yarn add -D webpack webpack-dev-server
```

webpack의 결과물을 확인하기 위해 서버를 띄워야하므로 webpack-dev-server도 함께 설치

### IIFE

즉시 실행 함수 표현

- 독립적인 스코프가 생성
- IIFE 안에서 실행되는 함수는 외부 접근 불가능

### 모듈

- CommonJS : export 키워드로 모듈로 만들고 require()함수로 불러 들이는 방식
- AMD : 브라우저 환경(비동기)
- UMD : AMD 기반으로 CommonJS 까지 지원
- ES2015 표준 모듈 시스템 : export 키워드로 모듈을 만들고 import 구문으로 불러 들이는 방식

### webpack 실행 픨수 옵션

- --mode: development, production, none
- --entry: 모듈의 시작점
- --output: 결과 저장 경로

## webpack 설정 파일의 기본 형태 - webpack.config.js

```jsx
module.exports = {
  context: __dirname + '/app', // 모듈 파일 폴더
  entry: {
    // 엔트리 파일 목록
    app: './app.js',
  },
  output: {
    // 번들 파일
    path: __dirname + '/dist', // 번들 파일 폴더
    filename: '[name].bundle.js', // 번들 파일 이름 규칙
  },
  module: {
    rules: [
      {
        // ...
      },
    ],
  },
};
```

### entry

webpack을 이용하여 bundle하고 build할 애플리케이션의 시작점을 설정하는 옵션

### output

entry로 지정된 파일로부터 bundling을 진행하고, 그 결과를 어떻게 할지를 설정

- path : 어디에 생성할지에 대한 정보. 절대 경로를 통해 설정해줘야함 (\_\_dirname 사용)
- filename : bundling된 결과 파일의 이름

모든 config 파일은 이런 구조로 entry, output, module, plugins 네 가지 설정을 기본적인 옵션을 제공

### module

rules에 각종 loader들을 등록할 수 있다. 배열 형태로 여러 loader 등록

### loader

webpack의 loader는 다양한 리소스를 javascript에서 바로 사용할 수 있는 형태로 로딩하는 기능이다. loader는 webpack의 특징적인 기능이면서, webpack을 강력한 도구로 만드는 기능이다.

```jsx
module.exports = {
    ...
    output : {
        ...
    },
    module : {
        loaders : [ // 적용할 파일의 패턴(정규표현식)과 적용할 로더 설정
        {
            test : /\-tpl.html$/,
            exclude: /node_modules/,
            include: path.join(__dirname, 'src'),
            loader : 'handlebars'
            use: [{
              loader: "",
              options: {
                presets: []
              }
            }]
        }]
    }
}
```

- test : load할 파일 지정
- exclude, include : path 지정
- use : 사용할 module 작성
  - use 안에 loader와 options를 명시하여 loader에 대한 명세
  - presets : bundling 결과로부터 사용되지 않은 코드를 삭제하어 파일 크기를 줄여준다.

## webpack-dev-server

웹팩을 이용한 애플리케이션을 개발하는 것을 도와주는 도구

### webpack-dev-server 기본 설정

```jsx
// webpack.config.js
module.exports = {
  devServer: {
    static: {
      directory: path.join(__dirname, "dist"), // 정적 파일을 제공할 경로
    }
    publicPath: "/", // 브라우저를 통해 접근하는 경로 (기본값은 /)
    host: "dev.domain.com", // 개발환경에서 도메인을 맞춰야하는 상황에서 사용
    client: {
      overlay: true // 빌드시 에러나 경고를 브라우저 화면에 표시
    },
    port: 8080, // 개발 서버 포트 번호 설정 (기본값은 8080)
    stats: "errors-only", // 메세지 수준 정할 수 있다 ("none", "errors-only", "minimal", "normal","verbose" )
    historyApiFallback: true // 히스토리 API를 사용하는 SPA 개발 시 설정, 404 발생시 index.html로 리다이렉트
  }
}
```
