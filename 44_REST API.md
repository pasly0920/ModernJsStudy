# REST API

+ **REST**(REpresentational State Transfer) : HTTP를 기반으로 클라이언트가 서버의 리소스에 접근하는 방식을 규정한 아키텍처
+ **REST API** : REST를 기반으로 서비스 API를 구현한 것
  + REST의 기본 원칙을 성실히 지킨 서비스 디자인 => "RESTful"이라고 표현
  + 웹이 HTTP를 제대로 못 사용하고 있는 걸 보고 HTTP의 장점을 최대한 활용할 수 있는 아키텍처로 소개됨
  + HTTP 프로토콜을 의도에 맞게 디자인하도록 유도함

***

## 44.1 REST API의 구성

+ REST API : 자원 (resource), 행위 (verb), 표현 (representations)의 3가지 요소로 구성됨

+ REST는 자체 표현 구조(self-descriptiveness)로 구성됨 -> REST API만으로 HTTP 요청의 내용을 이해할 수 있음

구성 요소| 내용 | 표현 방법
|:----------:|:----------:|:----------:|
자원 (resource)| 자원| URI(엔드포인트)
행위 (verb)| 자원에 대한 행위| HTTP 요청 메서드
표현 (representations)| 자원에 대한 행위의 구체적 내용| 페이로드

> 🔍 [페이로드란?](https://ko.wikipedia.org/wiki/%ED%8E%98%EC%9D%B4%EB%A1%9C%EB%93%9C_(%EC%BB%B4%ED%93%A8%ED%8C%85)) (payload)
: 전송의 근본적인 목적이 되는 데이터의 일부분으로 그 데이터와 함께 전송되는 헤더와 메타데이터와 같은 데이터는 제외함

***

## 44.2 REST API 설계 원칙

+ REST의 가장 중요한 기본적인 원칙 2가지를 살펴보자!

**1. URI는 리소스를 표현해야 한다.**

+ 리소스를 식별할 수 있는 이름은 동사보다는 명사를 사용함
-- ex) 이름에 get 같이 리소스에 대한 행위의 표현 들어가면 안 됨

> // bad  
GET /getTodos/1  
GET /todos/show/1  
> // good  
GET /todos/1

**2. 리소스에 대한 행위는 HTTP 요청 메서드로 표현한다.**

+ HTTP 요청 메서드 : 클라이언트가 서버에게 요청의 종류 & 목적(리소스에 대한 행위)을 알리는 방법
+ 주로 5가지 요청 메서드(GET, POST, PUT, PATCH, DELETE 등)를 사용하여 CRUD를 구현함

< HTTP 요청 메서드 5가지 >
HTTP 요청 메서드| 종류 | 목적 | 페이로드
|:----------:|:----------:|:----------:|:----------:|
GET| index/retrieve| 모든/특정 리소스 취득| X
POST| create| 리소스 생성| O
PUT| replace| 리소스의 전체 교체| O
PATCH| modify| 리소스의 일부 수정| O
DELETE| delete| 모든/특정 리소스 삭제| X

+ 리소스에 대한 행위 - HTTP 요청 메서드로 표현, URI에 표현 X

> // bad : URI에 리소스 삭제 행위인 delete를 표현함  
GET /todos/delete/1  
// good : 리소스 삭제 행위를 HTTP 요청 메서드인 DELETE로 표현함  
DELETE /todos/1

***

## 44.3 JSON Server를 이용한 REST API 실습

HTTP 요청을 전송 & 응답 받으려면 서버가 필요함

-> JSON Server를 사용하여 가상 REST API 서버 구축해서 실습해보자!

### 44.3.1 JSON Server 설치

+ JSON Server : json 파일을 사용하여 가상 REST API 서버를 구축할 수 있는 툴

+ npm을 사용하여 JSON Server를 설치

>
mkdir json-server-exam && cd json-server-exam  
npm init -y  
npm install json-server --save-dev

### 44.3.2 db.json 파일 생성

+ 프로젝트 루트 폴더(/json-server-exam)에 다음과 같이 db.json 파일 생성

+ db.json 파일 - 리소스를 제공하는 데이터베이스 역할을 함

```json
{
  "todos": [
    {
      "id": 1,
      "content": "HTML",
      "completed": true
    },
    {
      "id": 2,
      "content": "CSS",
      "completed": false
    },
    {
      "id": 3,
      "content": "Javascript",
      "completed": true
    }
  ]
}
```

### 44.3.3 JSON Server 실행

#### case 1. 명령어로 JSON Server 설정 변경하기

+ JSON Server가 db.json 파일의 변경 감지하게 하려면 watch 옵션 추가

>// 기본 포트(3000) 사용 / watch 옵션 적용  
>json-server--watch db.json

+ 기본 포트 : 3000, 변경 시 port 옵션 추가

>// 포트 5000으로 변경 / watch 옵션 적용  
json-server--watch db.json --port 5000

#### case 2. package.json 파일의 scripts 수정

+ 매번 명령어 입력하기 번거로움

+ package.json 파일의 scripts 다음과 같이 수정하자

```json
{
  "name": "json-server-exam",
  "version": "1.0.0",
  "scripts": {
    "start": "json-server --watch db.json"
  },
  "devDependencies": {
    "json-server": "^0.16.1"
  }
}
```

+ JSON Server 실행 시엔 터미널에 `npm start` 명령어 입력

***

### 44.3.4 GET 요청

#### 목적 1. todos 리소스에서 모든 todo를 취득(index)
>
> 1) json-server-exam/public 생성, JSON Server 중단 후 재실행
> 2) 다음과 같이 get_index.html 추가
> 3) <http://localhost:3000/get_index.html>로 접속

```HTML
<!DOCTYPE html>
<html>
<body>
  <pre></pre>
  <script>
    // XMLHttpRequest 객체 생성
    const xhr = new XMLHttpRequest();

    // HTTP 요청 초기화
    // todos 리소스에서 모든 todo를 취득(index)
    xhr.open('GET', '/todos');

    // HTTP 요청 전송
    xhr.send();

    // load 이벤트는 요청이 성공적으로 완료된 경우 발생한다.
    xhr.onload = () => {
      // status 프로퍼티 값이 200이면 정상적으로 응답된 상태다.
      if (xhr.status === 200) {
        document.querySelector('pre').textContent = xhr.response;
      } else {
        console.error('Error', xhr.status, xhr.statusText);
      }
    };
  </script>
</body>
</html>
```

#### 목적 2. todos 리소스에서 id를 사용해 특정 todo 취득(retrieve)
>
>1) public 폴더에 다음과 같이 get_retrieve.html 추가
>2) <http://localhost:3000/get_retrieve.html>로 접속

```HTML
<!DOCTYPE html>
<html>
<body>
  <pre></pre>
  <script>
    // XMLHttpRequest 객체 생성
    const xhr = new XMLHttpRequest();

    // HTTP 요청 초기화
    // todos 리소스에서 id를 사용하여 특정 todo를 취득(retrieve)
    xhr.open('GET', '/todos/1');

    // HTTP 요청 전송
    xhr.send();

    // load 이벤트는 요청이 성공적으로 완료된 경우 발생한다.
    xhr.onload = () => {
      // status 프로퍼티 값이 200이면 정상적으로 응답된 상태다.
      if (xhr.status === 200) {
        document.querySelector('pre').textContent = xhr.response;
      } else {
        console.error('Error', xhr.status, xhr.statusText);
      }
    };
  </script>
</body>
</html>
```

### 44.3.5 POST 요청

+ 목적 : todos 리소스에 새로운 todo 생성

+ POST 요청 시엔 setRequestHeader 메서드 사용 -> 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입 지정해야 함

>1) public 폴더에 다음과 같이 post.html 추가
>2) <http://localhost:3000/post.html>로 접속

```HTML
<!DOCTYPE html>
<html>
<body>
  <pre></pre>
  <script>
    // XMLHttpRequest 객체 생성
    const xhr = new XMLHttpRequest();

    // HTTP 요청 초기화
    // todos 리소스에 새로운 todo를 생성
    xhr.open('POST', '/todos');

    // 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정
    xhr.setRequestHeader('content-type', 'application/json');

    // HTTP 요청 전송
    // 새로운 todo를 생성하기 위해 페이로드를 서버에 전송해야 한다.
    xhr.send(JSON.stringify({ id: 4, content: 'Angular', completed: false }));

    // load 이벤트는 요청이 성공적으로 완료된 경우 발생한다.
    xhr.onload = () => {
      // status 프로퍼티 값이 200(OK) 또는 201(Created)이면 정상적으로 응답된 상태다.
      if (xhr.status === 200 || xhr.status === 201) {
        document.querySelector('pre').textContent = xhr.response;
      } else {
        console.error('Error', xhr.status, xhr.statusText);
      }
    };
  </script>
</body>
</html>
```

### 44.3.6 PUT 요청

+ **PUT : 특정 리소스 전체를 교체할 때 사용**

+ 목적 : todos 리소스에서 id로 todo를 특정하여 id를 제외한 리소스 전체를 교체
+ PUT 요청 시엔 setRequestHeader 메서드 사용 -> 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입 지정해야 함

>1) public 폴더에 다음과 같이 put.html 추가
>2) <http://localhost:3000/put.html>로 접속

```html
<!DOCTYPE html>
<html>
<body>
  <pre></pre>
  <script>
    // XMLHttpRequest 객체 생성
    const xhr = new XMLHttpRequest();

    // HTTP 요청 초기화
    // todos 리소스에서 id로 todo를 특정하여 id를 제외한 리소스 전체를 교체
    xhr.open('PUT', '/todos/4');

    // 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정
    xhr.setRequestHeader('content-type', 'application/json');

    // HTTP 요청 전송
    // 리소스 전체를 교체하기 위해 페이로드를 서버에 전송해야 한다.
    xhr.send(JSON.stringify({ id: 4, content: 'React', completed: true }));

    // load 이벤트는 요청이 성공적으로 완료된 경우 발생한다.
    xhr.onload = () => {
      // status 프로퍼티 값이 200이면 정상적으로 응답된 상태다.
      if (xhr.status === 200) {
        document.querySelector('pre').textContent = xhr.response;
      } else {
        console.error('Error', xhr.status, xhr.statusText);
      }
    };
  </script>
</body>
</html>
```

### 44.3.7 PATCH 요청

+ **PATCH : 특정 리소스의 일부를 수정할 때 사용**

+ 목적 : todos 리소스의 id로 todo를 특정하여 completed만 수정
+ PATCH 요청 시엔 setRequestHeader 메서드 사용 -> 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입 지정해야 함

>1) public 폴더에 다음과 같이 patch.html 추가
>2) <http://localhost:3000/patch.html>로 접속

```html
<!DOCTYPE html>
<html>
<body>
  <pre></pre>
  <script>
    // XMLHttpRequest 객체 생성
    const xhr = new XMLHttpRequest();

    // HTTP 요청 초기화
    // todos 리소스의 id로 todo를 특정하여 completed만 수정
    xhr.open('PATCH', '/todos/4');

    // 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정
    xhr.setRequestHeader('content-type', 'application/json');

    // HTTP 요청 전송
    // 리소스를 수정하기 위해 페이로드를 서버에 전송해야 한다.
    xhr.send(JSON.stringify({ completed: false }));

    // load 이벤트는 요청이 성공적으로 완료된 경우 발생한다.
    xhr.onload = () => {
      // status 프로퍼티 값이 200이면 정상적으로 응답된 상태다.
      if (xhr.status === 200) {
        document.querySelector('pre').textContent = xhr.response;
      } else {
        console.error('Error', xhr.status, xhr.statusText);
      }
    };
  </script>
</body>
</html>
```

### 44.3.8 DELETE 요청

+ 목적 : todos 리소스의 id를 사용하여 todo를 삭제

>1) public 폴더에 다음과 같이 delete.html 추가
>2) <http://localhost:3000/delete.html>로 접속

```html
<!DOCTYPE html>
<html>
<body>
  <pre></pre>
  <script>
    // XMLHttpRequest 객체 생성
    const xhr = new XMLHttpRequest();

    // HTTP 요청 초기화
    // todos 리소스에서 id를 사용하여 todo를 삭제한다.
    xhr.open('DELETE', '/todos/4');

    // HTTP 요청 전송
    xhr.send();

    // load 이벤트는 요청이 성공적으로 완료된 경우 발생한다.
    xhr.onload = () => {
      // status 프로퍼티 값이 200이면 정상적으로 응답된 상태다.
      if (xhr.status === 200) {
        document.querySelector('pre').textContent = xhr.response;
      } else {
        console.error('Error', xhr.status, xhr.statusText);
      }
    };
  </script>
</body>
</html>
```
