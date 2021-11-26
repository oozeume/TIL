react에서 네트워크 통신을 도와주는 api인 axios와 fetch를 비교하고 장단점을 알아본다.

# fetch

### 특징

- 라이브러리 업데이트에 따른 에러 방지가 가능하다.
- JSON으로 변환해주는 과정 필요하다.

```javascript
return fetch('주소')
  .then((response) => response.json())
  .then((data) => console.log(data));
```

# axios

브라우저, Node.js를 위한 Promise API를 활용하는 HTTP 비동기 통신 라이브러리

### 특징

- 자동 JSON 데이터 변환 지원
- response timeout 처리 방법이 있다.
- 크로스 브라우징 최적화로 브라우저 호환성 좋다.
- 구형 브라우저 지원 가능
- XSRF(사이트 간 요청 위조) 보안 기능 지원
  유지보수 및 가독성을 위해 모듈화해서 주로 사용한다.

```javascript
// 글로벌 axios 기본(defaults) 설정
axios.defaults.baseURL = 'https://api.example.com';
axios.defaults.headers.common['Authorization'] = AUTH_TOKEN;
axios.defaults.headers.post['Content-Type'] =
  'application/x-www-form-urlencoded';

// 사용자 정의 인스턴스 기본 설정
import axios from 'axios';

const api = axios.create({
  baseURL: `주소`,
});

export default api;
```

```javascript
api
  .get(url, { option })
  .then((response) => {})
  .catch();
```

| fetch                                                        | axios                                     |
| ------------------------------------------------------------ | ----------------------------------------- |
| 요청 객체에 url이 있다                                       | 요청 객체에 url이 없다                    |
| 브라우저 내장 API로서 별도의 설치없이 브라우저에서 사용 가능 | 써드파티 라이브러리로 설치 및 import 필요 |
| 지원하지 않는 브라우저가 있음                                | 브라우저 호환성 좋다(크로스 브라우징)     |
| .json() 메소드를 사용해야함                                  | JSON데이터 자동변환                       |
| 네트워크 에러 발생시 기다려야함                              | catch로 에러로그를 보여준다               |
| HTTP 요청을 가로챌 수 있음                                   | 해당 기능 존재하지않음                    |

# axios interceptors로 API 관리

- axios의 interceptors를 이용하면 then이나 catch로 처리되기 전에 요청이나 응답을 가로챌 수 있다.
- 모든 axios API 요청에 baseURL과 headers를 넣어줄 수 있다.

```javascript
api.defaults.baseURL = '주소';
api.interceptors.request.use((config) => {
  if (!config.headers['Authorization']) {
    config.headers['Authorization'] = `Bearer ${process.env.REACT_APP_KEY}`;
  }
  config.headers['Content-Type'] = 'application/json';
  return config;
});
```
