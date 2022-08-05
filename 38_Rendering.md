# 38장. 브라우저의 렌더링 과정

웹 애플리케이션의 클라이언트 사이드 자바스크립트는 브라우저에서 HTML, CSS와 함께 실행된다. 따라서 브라우저 환경을 고려할 때 더 효율적인 클라이언트 사이드 자바스크립트 프로그래밍이 가능하다.

이를 위해 브라우저가 HTML, CSS, 자바스크립트로 작성된 텍스트 문서를 어떻게 파싱(해석)하여 브라우저에 렌더링하는지 살펴보자.

* 렌더링(Rendering) : HTML, CSS, 자바스크립트로 작성된 문서를 파싱하여 브라우저에 시각적으로 출력하는 것

브라우저는 다음과 같은 과정을 거쳐 렌더링을 수행한다.

1. 브라우저는 HTML, CSS, 자바스크립트, 이미지, 폰트 파일 등 렌더링에 필요한 리소스를 요청하고 서버로부터 응답을 받는다.

2. 브라우저의 렌더링 엔진은 서버로부터 응답된 HTML과 CSS를 파싱하여 DOM과 CSSOM을 생성하고 이들을 결합하여 렌더 트리를 생성한다.

3. 브라우저의 자바스크립트 엔진은 자바스크립트를 파싱하여 AST를 생성하고 바이트코드로 변환하여 실행한다. 이때 자바스크립트는 DOM API를 통해 DOM이나 CSSOM을 변경할 수 있고 변경된 후 렌더 트리로 결합된다.

4. 렌더 트리를 기반으로 HTML 요소의 레이아웃을 계산하고 브라우저 화면에 HTML 요소를 페인팅한다.

## 38.1 요청과 응답

브라우저의 핵심 기능은 필요한 리소스(HTML, CSS, 자바스크립트, 이미지, 폰트 등의 정적 파일 또는 서버가 동적으로 생성한 데이터)를 서버에 요청하고 서버로부터 응답 받아 브라우저에 시각적으로 렌더링하는 것이다.

서버에 요청을 전송하기 위해 브라우저는 주소창을 제공한다. 브라우저의 주소창에 URL을 입력하고 엔터 키를 누르면 URL의 호스트 이름이 DNS를 통해 IP 주소로 변환되고 이 IP 주소를 갖는 서버에게 요청을 전송한다.

서버는 루트 요청에 대해 서버의 루트 폴더에 존재하는 정적 파일 index.html을 클라이언트로 응답한다. 만약 index.html이 아닌 다른 정적 파일을 서버에 요청하려면 브라우저의 주소창에 요청할 정적 파일의 경로와 파일 이름을 URI의 호스트 뒤의 패스에 기술하여 서버에 요청한다.

브라우저의 렌더링 엔진이 HTML을 파싱하는 도중에 외부 리소스를 로드하는 태그, 즉 CSS 파일을 로드하는 link 태그, 이미지 파일을 로드하는 img 태그, 자바스크립트를 로드하는 script 태그 등을 만나면 HTML의 파싱을 일시 중단하고 해당 리소스 파일을 서버로 요청한다.

## 38.2 HTTP 1.1 과 HTTP 2.0

* HTTP (HyperText Transfer Protocol) : 웹에서 브라우저와 서버가 통신하기 위한 프로토콜이다.

HTTP/1.1 은 기본적으로 커넥션(connection)당 하나의 요청과 응답만 처리한다. 즉 여러 개의 요청을 한 번에 전송할 수 없고 응답 또한 마찬가지다. 따라서 HTML 문서 내에 포함된 여러 개의 리소스 요청이 개별적으로 전송되고 응답 또한 개별적으로 전송된다. 따라서 요청할 리소스의 개수에 비례하여 응답 시간도 증가하는 단점이 있다.

HTTP/2는 커넥션당 여러 개의 용청과 응답이 동시에 가능하다.

## 38.3 HTML 파싱과 DOM 생성

브라우저의 요청에 의해 서버가 응답한 HTML 문서는 문자열로 이루어진 순수한 텍스트이다. 이 HTML 문서를 브라우저에 시각적인 픽셀로 렌더링하려면 HTML 문서를 브라우저가 이해할 수 있는 자료구조(객체)로 변환하여 메모리에 저장해야 한다.

따라서 브라우저의 렌더링 엔진은 HTML 문서를 파싱하여 브라우저가 이해할 수 있는 자료구조인 DOM(Document Object Model)을 생성한다.

즉, DOM은 HTML 문서를 파싱한 결과물이다. 바이트, 문자, 토큰, 노드, DOM의 과정을 통해 생성된다. (39장 "DOM"에서 자세히 살펴보자)

## 38.4 CSS 파싱과 CSSOM 생성

CSS를 로드하는 link 태그나 style 태그를 만나면 DOM 생성을 일시 중단하고 CSS도 파싱 과정을 거치며 CSSOM을 생성한다.

CSS 파싱을 완료하면 HTML 파싱이 중단된 지점부터 다시 HTML을 파싱하기 시작하여 DOM 생성을 재개한다.

## 38.5 렌더 트리 생성

생성된 DOM과 CSSOM은 렌더링을 위해 렌터 트리로 결합된다.

렌더 트리는 렌더링을 위한 트리 구조의 자료구조다. 따라서 브라우저 화면에 렌더링되지 않는 노드 (meta태그, script태그 등)와 CSS에 의해 비표시(display: none 등)되는 노드들은 포함하지 않는다.

완성된 렌더 트리는 각 HTML 요소의 레이아웃을 계산하는 데 사용되며 브라우저 화면에 픽셀을 렌더링하는 페인팅 처리에 입력된다.

다음과 같은 경우 반복해서 레이아웃 계산과 페인팅이 재차 실행된다.

* 자바스크립트에 의한 노드 추가 또는 삭제

* 브라우저 창의 리사이징에 의한 뷰포트 크기 변경

* HTML 요소의 레이아웃에 변경을 발생시키는 스타일 변경 (width/height, margin, padding, border, display, position, top/right/bottom/left 등)

리렌더링은 비용이 많이 들어 성능에 악영향을 주는 작업이므로 빈번하게 발생하지 않도록 주의할 필요가 있다.

## 38.6 자바스크립트 파싱과 실행

DOM은 HTML 문서의 구조와 정보뿐만아니라 HTML 요소와 스타일 등을 변경할 수 있는 프로그래밍 인터페이스로서 DOM API를 제공한다. 이를 이용해 이미 생성된 DOM을 동적으로 조작할 수 있다.

DOM을 생성하다가 자바스크립트 파일을 로드하는 script 태그나 자바스크립트 코드를 콘텐츠로 담은 script 태그를 만나면 DOM 생성을 일시 중단한다.

script 태그의 src 어트리뷰트에 정의된 자바스크립트 파일을 서버에 요청하여 로드한 자바스크립트 파일이나 script 태그 내의 자바스크립트 코드를 파싱하기 위해 자바스크립트 엔진에 제어권을 넘긴다.

자바스크립트 엔진은 자바스크립트 코드를 파싱하여 CPU가 이해할 수 있는 저수준 언어로 변환하고 실행한다. 자바스크립트 엔진은 AST(Abstract Syntax Tree 추상적 구문 트리)를 생성한다. 그리고 AST를 기반으로 인터프리터가 실행할 수 있는 중간 코드인 바이트코드를 생성하여 실행한다.

* 토크나이징(tokenizing) : 자바스크립트 소스코드를 어휘 분석하여 문법적 의미를 갖는 코드의 최소 단위인 토큰들로 분해한다.

* 파싱(parsing) : 토큰들의 집합을 구문 분석하여 AST를 생성한다.

## 38.7 리플로우와 리페인트

자바스크립트 코드에 DOM이나 CSSOM을 변경하는 DOM API가 사용된 경우 DOM이나 CSSOM이 변경된 후 다시 렌더 트리로 결합되고 이를 기반으로 레이아웃과 페인트 과정을 거쳐 화면에 다시 렌더링한다. 이를 리플로우, 리페인트라 한다.

리플로우는 레이아웃 계산을 다시 하는 것을 말하며, 노드 추가/삭제, 요소의 크기/위치 변경, 윈도우 리사이징 등 레이아웃에 영향을 주는 변경이 발생한 경우에 한하여 실행된다.

리페인트는 재결합된 렌더 트리를 기반으로 다시 페인트를 하는 것을 말한다.

## 38.8 자바스크립트 파싱에 의한 HTML 파싱 중단

파싱은 직렬적으로 수행된다. 브라우저는 동기적으로 위에서 아래 방향으로 파싱하고 실행한다. 이것이 script 태그의 위치에 따라 HTML 파싱이 블로킹되어 DOM 생성이 지연될 수 있다는 것을 의미한다.

```javascript
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <link rel="stylesheet" href="style.CSS">
        <script src="app.js"></script>
    </head>
    <body>
        <ul>
            <li id="apple">Apple</li>
            <li id="banana">Banana</li>
            <li id="orange">Orange</li>
        </ul>
    </body>
</html>
```

자바스크립트 코드에서 DOM 이나 CSSOM을 변경하는 DOM API를 사용할 경우 DOM이나 CSSOM이 이미 생성되어 있어야 한다. 아직 생성이 완료되지 않은 상태라면 문제가 발생할 수 있다. 즉 script 태그의 위치에 의해 블로킹이 발생한다는 것이다.

body 요소 가장 아래에 자바스크립트를 위치시키는 것은 좋은 아이디어 이다.

* DOM이 완성되지 않은 상태에서 자바스크립트가 DOM을 조작하면 에러가 발생할 수 있다.

* 자바스크립트 로딩/파싱/실행으로 인해 HTML 요소들의 렌더링에 지장받는 일이 발생하지 않아 페이지 로딩 시간이 단축된다.

## 38.9 script 태그의 async/defer 어트리뷰트

자바스크립트 파싱에 의한 DOM 생성이 중단되는 문제를 근본적으로 해결하기 위해 HTML5부터 script 태그에 async와 defer 어트리뷰트가 추가되었다.

scr 어트리뷰트를 통해 외부 자바스크립트 파일을 로드하는 경우에만 async와 defer 어트리뷰트를 사용할 수 있다.

두 개의 어트리뷰트 모두  HTML 파싱과 외부 자바스크립트 파일의 로드가 비동기적으로 동시에 진행된다.

* async 어트리뷰트 : 자바스크립트의 파싱과 실행은 자바스크립트 파일의 로드가 완료된 직후 진행되며, 이때 HTML 파싱이 중단된다. script 순서와는 상관없이 로드가 완료된 자바스크립트 먼저 실행되므로 순서가 보장되지 않는다.

* defer 어트리뷰트 : 자바스크립트 파싱과 실행은 HTML 파싱이 완료된 직후, 즉 DOM 생성이 완려된 직후(이때 DOMContentLoaded 이벤트가 발생한다) 진행된다.