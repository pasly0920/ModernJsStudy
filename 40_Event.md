# Ch.40 이벤트

## 40.1 Event Driven Programming

브라우저는 특정 행동을 감지하여 Event를 trigger한다. 이 때, 특정 함수를 호출할 수 있고 이를 **Event Handler** 라고 한다. Handler 함수를 적절하게 호출하기 위해서 브라우저의 특정 이벤트에 함수를 **등록**할 수 있다.

이벤트와 Event Handler를 통해 사용자와 app이 상호 작용하는 것, 프로그램의 흐름을 이벤트 중심으로 제어하는 것을 **Event Driven Programming**이라고 한다.

## 40.2 Event Type

이벤트의 종류를 알아보자. ↓↓링크참고

[MDN Event Document](https://developer.mozilla.org/ko/docs/Web/Events)

### 40.2.1 Mouse Event

| 이벤트 타입 | 발생 시점                                                                                  |
| :---------: | ------------------------------------------------------------------------------------------ |
|    Click    | 마우스 클릭                                                                                |
|  DblClick   | 마우스 더블 클릭                                                                           |
|  MouseDown  | 마우스 누름                                                                                |
|   MouseUp   | 마우스 땜                                                                                  |
|  MouseMove  | 커서 움직임                                                                                |
| MouseEnter  | 커서 요소 안으로 이동 ([Bubbling](https://ko.javascript.info/bubbling-and-capturing) 안함) |
| MouseLeave  | 커서 요소 밖으로 이동 ([Bubbling](https://ko.javascript.info/bubbling-and-capturing) 안함) |
|  MouseOver  | 커서 요소 안으로 이동 ([Bubbling](https://ko.javascript.info/bubbling-and-capturing) 함)   |
|  MouseOut   | 커서 요소 밖으로 이동 ([Bubbling](https://ko.javascript.info/bubbling-and-capturing) 함)   |

- Bubbling

  이벤트 전파랑 관련이 있는데, 한 요소에 이벤트가 발생하면 요소의 부모로 올라가면서 할당된 Event Handler동작

### 40.2.2 KeyBoard Event

| 이벤트 타입 | 발생 시점                               |
| :---------: | --------------------------------------- |
|   KeyDown   | 키를 누름. 단, 문자키는 연속적으로 발생 |
|    KeyUp    | 카를 놓음                               |
|  KeyPress   | 폐지됨                                  |

### 40.2.3 Focus Event

| 이벤트 타입 | 발생 시점                          |
| :---------: | ---------------------------------- |
|    Focus    | 요소가 포커스 받음 (Bubbling 안함) |
|    Blur     | 요소가 포커스 잃음 (Bubbling 안함) |
|   FocusIn   | 요소가 포커스 받음 (Bubbling 함)   |
|  FocusOut   | 요소가 포커스 잃음 (Bubbling 함)   |

- 주의

  FocusIn 과 FocusOut은 프로퍼티로 등록하면 크롬, 사파리에서 동작 안한다. AddEventListener 사용해야 한다.

### 40.2.4 Form Event

| 이벤트 타입 | 발생 시점                                                                                                                        |
| :---------: | -------------------------------------------------------------------------------------------------------------------------------- |
|   Submit    | Form 요소의 자식 요소중 input(type = text, checkbox, radio) 과 select에서 Enter 키 누름 / button 과 input(type = submit) 을 클릭 |
|    Reset    | Form 요소의 자식 요소중 input(type = reset) 을 클릭                                                                              |

### 40.2.5 Change Event

|   이벤트 타입    | 발생 시점                                                                                                                           |
| :--------------: | ----------------------------------------------------------------------------------------------------------------------------------- |
|      Input       | input(type = text, checkBox, radio), select, textarea 요소의 값이 변경                                                              |
|      Change      | input과 동일하나 요소가 focus를 잃었을 때 값이 변경돼있으면 발생 (React는 input과 동일)                                             |
| ReadyStateChange | [document.readyState](https://developer.mozilla.org/ko/docs/Web/API/Document/readyState) 값(loading, interactive, complete) 이 변경 |

- document.readyState

  - loading : document 로딩 중
  - interactive : loading 후 리소스 로딩 중 - 이때 DomContentLoaded 이벤트 발생
  - complete : DOM 생성 완료 - 이때 onLoad 이벤트 발생

```javascript
<script>
  window.addEventListener('DOMContentLoaded', e =>
    console.log('DOMContentLoaded', document.readyState),
  );
  window.addEventListener('load', e =>
    console.log('load', document.readyState),
  );
  document.onreadystatechange = function () {
    console.log(document.readyState);
  };
</script>

// 결과

// interactive
// DOMContentLoaded interactive
// complete
// load complete
```

### 40.2.6 DOM Mutation Event

|   이벤트 타입    | 발생 시점                   |
| :--------------: | --------------------------- |
| DOMContentLoaded | HTML 문서의 DOM 생성이 완료 |

### 40.2.7 View Event

| 이벤트 타입 | 발생 시점                                                  |
| :---------: | ---------------------------------------------------------- |
|   Resize    | 브라우저 윈도우의 크기를 리사이즈시 (Window 객체에서 발생) |
|   Scroll    | 브라우저나 요소를 스크롤시                                 |

### 40.2.8 Resource Event

| 이벤트 타입 | 발생 시점                                                  |
| :---------: | ---------------------------------------------------------- |
|    Load     | DOMContentLoaded 이벤트 발생 후, 모든 리소스의 로딩이 완료 |
|   UnLoad    | 리소스가 언로드시 (새 페이지 요청)                         |
|    Abort    | 리소스 로딩 중단시                                         |
|    Error    | 리소스 로딩 에러시                                         |

## 40.3 Event Handler Register

Event Handler 를 등록하는 방법은 3가지이다.

### 40.3.1 Attribute 이용

```javascript
<!DOCTYPE html>
<html>
<body>
  <button onclick="onClickHandler()">Click Me</button>
  <script>
    function onClickHandler() {
      alert("Button Clicked!");
    }
  </script>
</body>
</html>
```

이때, Attribute는 암묵적으로 생성되는 DOM 프로퍼티의 onclick Event Handler에 할당 된다. 즉, 함수 호출문을 넣어야 하고, 인수도 넣을 수 있다. 여러 줄을 세미콜론으로 넣을 수도 있다.

```javascript
function onclick(e) {
  onClickHandler();
}
```

### 40.3.2 프로퍼티 이용

### 40.3.3 addEventListener 메서드 이용

```javascript
<!DOCTYPE html>
<html>
<body>
  <button>Click Me</button>
  <script>
    const $button = document.querySelector('button');
    // 직접 바인딩
    $button.onclick = function() {
      console.log('Button Clicked!');
    };
    // 메소드 이용
    $button.addEventListener("click", function () {
      console.log("Button Clicked!");
    }, true);
  </script>
</body>
</html>
```

- Event Target을 가져와서 직접 DOM 프로퍼티의 onclick Event Handler를 등록 할 수 있다.
- 2개의 Event Handler를 바인딩하면 두번째만 실행된다. But, addEventLister는 중복되지 않는 여러 개를 등록 할 수 있다.
- addEventlistener 사용법
  - [Target].addEventListener('[type]', [Handler], [capture사용? - 기본값 false])

## 40.4 Event Handler 제거

```javascript
<!DOCTYPE html>
<html>
<body>
    <button>Click Me</button>
    <script>
      const $button = document.querySelector("button");
      function handler() {
        console.log("Button Clicked!");
      }

      $button.addEventListener("click", handler, true);
      // 이벤트 제거시 모든 인수가 같아야한다.
      $button.removeEventListener("click", handler, true);
      // 가명함수 내에서 자신의 이벤트를 삭제할 수 있다.
      $button.addEventListener("click", function handler() {
        $button.removeEventListener("click", handler);
      });
      // 무명함수 내에서 자신의 이벤트를 삭제할 수 있다. -> strict mode 에서는 금지
      $button.addEventListener("click", function () {
        $button.removeEventListener("click", arguments.callee);
      });

      $button.onclick = handler;
      // 프로퍼티에 직접 할당시, null을 할당해야 이벤트 핸들러가 제거된다. (remove메서드 안됨)
      $button.onclick = null;
    </script>
  </body>
</html>
```

- 요약
  - 모든 인수가 같아야 이벤트가 삭제된다.
  - 무명함수가 인수로 전달되면 이벤트를 삭제할 수 없다.
  - 가명함수와 무명함수 내에서 이벤트를 삭제할 수 있다.
  - 프로퍼티에 직접 할당시, null을 할당해야 이벤트 핸들러가 제거된다.

## 40.5 Event Object

이벤트가 발생하면 Event Handler의 첫번째 인수로 전달되는 객체

```javascript
<!DOCTYPE html>
<html>
<body>
    // 이 둘은 같다.
    <button onclick="handler(e)">Click Me</button>
    <script>
      function onclick(e) {
        handler(e);
      }
    </script>
  </body>
</html>
```

### 40.5.1 Event Object 상속 구조

MouseEvent의 상속구조 : Object -> Event -> UIEvent -> MouseEvent  
→ 모두 생성자 함수이다.

```javascript
<!DOCTYPE html>
<html>
<body>
    <script>
      let e = new Event('foo'); // e.type = 'foo'
      e = new MouseEvent('click');
    </script>
  </body>
</html>
```

- 프로토타입 체인
  - Event <-[[prototype]]- UIEvent <-[[prototype]]- MouseEvent <-[[prototype]]- click
  - 모든 이벤트 객체의 공통 프로퍼티는 Event에 정의, 하위 이벤트 객체에는 타입에 따른 고유한 프로퍼티 정의
  - 예를 들어 onload 이벤트는 Event 객체의 인스턴스, oninput 이벤트는 InputEvent 객체의 인스턴스이다.

### 40.5.2 Event Object 공통 프로퍼티

Event.prototype 의 프로퍼티이다. 모든 Event Object가 상속받는다.

|  공통 프로퍼티   | 설명                                                                                                                      | 타입     |
| :--------------: | ------------------------------------------------------------------------------------------------------------------------- | -------- |
|       type       | 이벤트 타입                                                                                                               | string   |
|      target      | 이벤트 발생한 DOM 요소                                                                                                    | DOM 요소 |
|  currentTarget   | 이벤트 핸들러가 바인딩된 DOM 요소                                                                                         | DOM 요소 |
|    eventPhase    | 이벤트 전파 단계 0: 이벤트없음, 1: 캡처링단계 2: 타깃 단계, 3: 버블링 단계                                                | number   |
|     bubbles      | 이벤트를 버블링으로 전파하는지                                                                                            | boolean  |
|    cancelable    | preventDefault 매서드롤 호출하여 이벤트의 기본 동작을 취소할 수 있는지 - 버블링하지 않는 이벤트는 cancelable: false 이다. | boolean  |
| defaultPrevented | preventDefault 메서드롤 호출했는지                                                                                        | boolean  |
|    isTrusted     | 사용자 행위에 의해 발생한 이벤트인지 - 인위적으로 발생한 이벤트는 isTrusted: false 이다.                                  | boolean  |
|    timeStamp     | 이벤트가 발생한 시각(1970년 부터 경과한 밀리초)                                                                           | boolean  |

- e.target vs e.currentTarget
  - 일반적으로 같은 DOM 요소를 가르키지만, 이벤트 위임에 따라 다를 수도 있다.

### 40.5.3 MouseEvent 객체

1. 마우스 포인터의 좌표 정보를 나타내는 프로퍼티

   | 프로퍼티  | 설명                                              |
   | :-------: | ------------------------------------------------- |
   | screenX/Y | 마우스 포인터의 모니터 화면 영역에서의 위치       |
   | clientX/Y | 마우스 포인터의 ViewPort에서의 위치               |
   |  pageX/Y  | 마우스 포인터의 문서 영역에서의 위치              |
   | offsetX/Y | 마우스 포인터의 이벤트 발생한 DOM 요소에서의 위치 |

2. 누른 버튼 정보를 나타내는 프로퍼티

   | 프로퍼티 | 설명                                                     |
   | :------: | -------------------------------------------------------- |
   |  altKey  | alt 키가 눌려있는가                                      |
   | ctrlKey  | ctrl 키가 눌려있는가                                     |
   | shiftKey | shift 키가 눌려있는가                                    |
   |  button  | 마우스 어떤버튼을 눌렀는가 - 0: 좌클릭, 1: 휠, 2: 우클릭 |

### 40.5.4 KeyboardEvent 객체

1. 누른 키 정보를 나타내는 프로퍼티

   |     프로퍼티      | 설명                                                                        |
   | :---------------: | --------------------------------------------------------------------------- |
   | alt,ctrl,shiftKey | 마우스 이벤트와 동일                                                        |
   |      metaKey      | command 나 window 키가 눌려있는가                                           |
   |        key        | 엔터키의 경우 'Enter' 반환                                                  |
   |      keyCode      | 엔터키의 경우 '13' 반환 [(참고)](https://www.toptal.com/developers/keycode) |

## 40.6 Event 전파

```javascript
<!DOCTYPE html>
<html>
<body>
  <ul id="ul">
    <li id="li1">one</li>
    <li id="li2">two</li>
    <li id="li3">three</li>
  </ul>
</body>
</html>
```

- li요소를 클릭하면 클릭 이벤트가 발생한다. 이때, 생성된 이벤트 객체는 Event Target인 li를 중심으로 DOM 트리를 통해 전파된다. 이벤트 객체가 전파되는 방향에 따라 3단계로 구분된다.
  - capturing phase : 상위 -> 하위 요소로 이벤트 전파
  - target phase : Event Target에 이벤트가 도달
  - bubbling phase : 하위 -> 상위 요소로 이벤트 전파

```javascript
<!DOCTYPE html>
<html>
<body>
    <ul id="ul">
      <li id="li1">one</li>
      <li id="li2">two</li>
      <li id="li3">three</li>
    </ul>
    <script>
      const $ul = document.getElementById("ul");
      const $li2 = document.getElementById("li2");
      $ul.addEventListener(
        "click",
        (e) => {
          console.log(`이벤트 단계: ${e.eventPhase}`); // 1 : capturing phase
          console.log(`이벤트 타깃: ${e.target}`); // 항상 li2로 동일
          console.log(`커렌트 타깃: ${e.currentTarget}`); // ul
        },
        true // {capture: true}
      );
      $li2.addEventListener("click", (e) => {
        console.log(`이벤트 단계: ${e.eventPhase}`); // 2 : target phase
        console.log(`이벤트 타깃: ${e.target}`); // 항상 li2로 동일
        console.log(`커렌트 타깃: ${e.currentTarget}`); // li2
      });
      $ul.addEventListener("click", (e) => {
        console.log(`이벤트 단계: ${e.eventPhase}`); // 3 : bubbling phase
        console.log(`이벤트 타깃: ${e.target}`); // 항상 li2로 동일
        console.log(`커렌트 타깃: ${e.currentTarget}`); // ul
      });
    </script>
  </body>
</html>
```

- li 요소 two 를 선택했을 때 일어나는일

  - 상위 -> 하위
    - window -> document -> html -> body -> ul -> li2
  - click Event 객체 생성, 이때 { target: li2 }
  - click Event 객체가 window에서 시작하여 target DOM 까지 전파 (capturing phase)
  - target DOM에 도달 (target phase)
  - 다시 click Event 객체가 target DOM에서 window로 재전파 (bubbling phase)

- Bubbling phase를 하지 않는 이벤트
  - Focus Event : focus/blur
  - Resource Event : load/unload/abor/error
  - Mouse Event : mouseenter/mouseleave
  - 이 이벤트를 상위 요소에서 캐치하려면 반드시 capturing phase 에서 해야한다. (대체 이벤트가 있으므로 추천 x)

### 40.7 Event Delegation (이벤트 위임)

여러 요소에 동일한 Event Handler를 적용할 때, 상위 요소에 Event Handler를 적용하여 성능을 향상시키는 방법이다.

```javascript
<!DOCTYPE html>
<html>
<body>
    <ul id="ul">
      <li id="li1">one</li>
      <li id="li2">two</li>
      <li id="li3">three</li>
    </ul>
    <script>
      const handler1 = (e) => {
        console.log(e.target.id);
      };
      document.getElementById("li1").addEventListener("click", handler1);
      document.getElementById("li2").addEventListener("click", handler1);
      document.getElementById("li3").addEventListener("click", handler1);

      // 위 아래 코드는 결과가 같다
      const handler2 = (e) => {
        if (!e.target.matches("#ul > li")) return; // 모든 하위 DOM 요소에 반응하므로 필요한 요소만 필터링한다.
        console.log(e.target.id);
      };
      document.getElementById("ul").addEventListener("click", handler2);
    </script>
  </body>
</html>
```

### 40.8 DOM 요소의 기본 동작 중단

## 40.8.1 DOM 요소의 기본 동작 중단

DOM 요소는 기본 동작이 있는데, 예를 들면 a tag는 href attribute 에 지정된 링크로 이동하는 것이다.  
이벤트 객체의 preventDefault 메서드는 기본 동작을 중단한다.

```javascript
<!DOCTYPE html>
<html>
<body>
    <a href="https://www.naver.com">go Naver</a>
    <script>
      document.querySelector('a').onclick = e => {
        e.preventDefault(); // 링크를 클릭해도 이동하지 않음
      }
    </script>
  </body>
</html>
```

### 40.8.2 이벤트 정파 방지

stopPropagation 메서드는 이벤트 전파를 중단한다.

```javascript
<!DOCTYPE html>
<html>
<body>
  <ul id="ul">
    <li id="li1">one</li>
    <li id="li2">two</li>
    <li id="li3">three</li>
  </ul>
  <script>
    const handler2 = (e) => {
      if (!e.target.matches("#ul > li")) return;
      console.log(e.target.id);
    };
    document.getElementById("ul").addEventListener("click", handler2);
    document.getElementById("li2").onclick = (e) => {
      e.stopPropagation(); // li2 요소는 상위 요소로 이벤트를 전파하지 않는다. 즉, handler2 호출안됨.
    };
  </script>
</body>
</html>
```

## 40.9 Event Handler This

### 40.9.1 Event Handler Attribute 방식

```javascript
<!DOCTYPE html>
<html>
<body>
  <button onclick="handler1()">Click Me</button>
  <button onclick="handler2(this)">Click Me</button>
  <script>
    function handler1() {
      console.log(this); // Window
    }
    function handler2(arg) {
      console.log(arg); // <button onclick="handler2(this)">
    }
  </script>
</body>
</html>
```

- 암묵적으로 일반 함수내에 handler가 호출되므로 this는 전역 객체를 가리킨다.
- 인수로 this를 전달하면 이벤트가 바인딩된 DOM 요소를 가리킨다.

### 40.9.2 Event Handler 프로퍼티 방식 + addEventListener 메서드 방식

```javascript
<!DOCTYPE html>
<html>
<body>
  <h1>Click Title</h1>
  <button>Click Button</button>
  <script>
    document.querySelector("h1").onclick = function (e) {
      console.log(this); // 둘다 이벤트 바인딩한 DOM 요소를 가리킨다.
    };
    document.querySelector("button").addEventListener("click", function (e) {
      console.log(this); // 둘다 e.currentTarget을 가리킨다. 위랑 같은말.
    });
  </script>
  </body>
</html>
```

- 일반 함수를 바인딩하면, 프로퍼티에 직접 바인딩하던 addEventListener 메소드를 사용하던 this는 바인딩한 DOM 요소(e.currentTarget)을 가리킨다.
- 화살표 함수를 바인딩하면, 함수 자체의 this 바인딩을 갖지 않으므로 상위 스코프의 this를 가리킨다.

```javascript
<!DOCTYPE html>
<html>
<body>
  <button>Click Button</button>
    <script>
      class cls {
        constructor() {
          this.$button = document.querySelector("button");

          this.$button.onclick = this.handler; // this 는 DOM
          this.$button.onclick = this.handler.bind(this) // 직접 this를 바인딩 하면 this 는 instance
        }

        handler() {
          console.log(this);
        }
        handler = () => {
          console.log(this); // 화살표 함수의 this 는 instance
        }
      }
      new cls();
    </script>
  </body>
</html>
```

- 클래스 내부 메소드를 이벤트 핸들러로 바인딩하면, 클래스 메소드의 this는 클래스의 인스턴스가 아니라 바인딩한 DOM을 가리킨다.

## 40.10 이벤트 핸들러에 인수 전달

## 40.11 커스텀 이벤트

### 40.11.1 커스텀 이벤트 생성

커스텀 이벤트는 이벤트 생성자 함수를 호출하여 명시적으로 생성한 이벤트 객체에 임의의 이벤트 타입을 지정하는 것이다. 새로운 이벤트 타입을 지정할때는 일반적으로 CustomEvent 생성자 함수를 사용한다.

```javascript
const customEvent1 = new CustomEvent("foo");
console.log(customEvent1.type); // foo
console.log(customEvent1.bubbles); // false - 기본 커스텀 이벤트 객체는 버블링 안됨
console.log(customEvent1.cancelable); // false - 기본 커스텀 이벤트 객체는 preventDefault 메서드 안됨

const customEvent2 = new CustomEvent("foo", {
  bubbles: true,
  cancelable: true,
});
console.log(customEvent2.bubbles); // true
console.log(customEvent2.cancelable); // true
```

- CustomEvent 생성자 함수의 두번째 인자로 이벤트의 프로퍼티를 임의로 수정할 수 있다.
- 그러나, isTrusted 프로퍼티는 항상 false로 수정할 수 없다.

### 40.11.2 커스텀 이벤트 dispatch(이벤트 발생)

커스텀 이벤트를 발생시키려면 이벤트 핸들러를 등록하고, dispatchEvent()를 호출하면 된다.

```javascript
// 등록
const $button = document.querySelector("button");
$button.addEventListener("click", (e) => console.log(e));
// dispatchEvent() 호출
const customEvent = new MouseEvent("click");
$button.dispatchEvent(customEvent); // click 이벤트 발생
```

- 일반적인 이벤트 핸들러는 비동기 처리하지만, dispatchEvent 메서드는 동기처리한다. 즉, 커스텀 이벤트에 바인딩된 이벤트 핸들러를 직접 호출하는 것과 같다.

```javascript
// 등록
const $button = document.querySelector('button');
$button.addEventListener('click', e => console.log(e.detail.message)); // hello uoslife
// dispatchEvent() 호출
const customEvent = new CustomEvent('foo'. {
  detail: { message: 'hello uoslife' } // 전달하고 싶은 정보
});
$button.dispatchEvent(customEvent); // click 이벤트 발생

```

- CustomEvent 생성자 함수는 두번째 인수로 이벤트와 함께 전달하고 싶은 정보를 담은 detail 프로퍼티를 포함하는 객체를 전달 할 수 있다.
- 반드시 addEventListener 메서드 방식으로 이벤트 핸들러를 등록해야 커스텀 이벤트 객체를 사용할 수 있다. onfoo 라는 attribute가 없기 때문이다.
