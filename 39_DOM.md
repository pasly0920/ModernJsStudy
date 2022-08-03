# 39장. DOM

DOM이란 Document Object Model의 약자로 HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리구조이다.

쉽게 말해서 HTML의 각 요소들을 트리 형식으로 표현해주며. 개발자는 자바스크립트를 이용하여 이 트리에 접근할 수 있습니다. 그리고 트리 안에 있는 하나의 개체(body, a, h1 등)를 Node라고 부른다. 트리의 연결된 노드들 중 document와 html 노드를 제외하면 위쪽의 node는 부모 노드, 아래쪽의 node는 자식 노드이다.

[<https://spoon718.tistory.com/161> 링크의 그림 참조]

## 39.1 노드

---

### 39.1.1 노드 객체의 상속 구조

|input 요소 노드 객체의 특성|프로토타입을 제공하는 객체|
|-----|-----|
|객체|Object|
|이벤트를 발생시키는 객체|EventTarget|
|트리 자료구조의 노드 객체|Node|
|브라우저가 렌더링할 수 있는 웹 문서의 요소(HTMl, SVG)를 표현하는 객체|Element|
|웹 문서의 요소 중에서 HTML 요소를 표현하는 객체|HTMLElement|
|HTMl 요소 중에서 input 요소를 표현하는 객체|HTMLInputElement|

<https://stage-diary.tistory.com/144> 링크 속 게시물을 보면 알 수 있듯이, node 속 요소들을 각 자의 역할을 담당하며 이러한 기능들을 상속을 통해 서로 공유한다.

```javascript
<html>
    <body>
        <button>클릭</button>
    <script>
        const button = document.getElementByTagName("button");

        button.addEventListener("click", ()=> {
            window.alert("버튼 클릭 완료!");
        })
    </script>
    </body>
</html>
```

## 39.2 요소 노드 취득

---

### 39.2.1 접근자를 이용한 요소 노드 접근

```html
<html>
    <body>
    <ul id="fruits">
        <li id="apple">apple</li>
        <li class="banana">banana</li>
        <li id="orange">orange</li>
    </ul>
    <script>
        let apple = document.querySelector("#apple");
        apple = document.getElemnetById("apple);

        let banana = document.querySelector(".banana");
        banana = document.getElemnetsByClassName("banana");

        let orange = document.querySelector("ul > li")[2];

        console.log(apple.matches('#fruits > li.apple')) // true
    </script>
    </body>
</html>
```

<https://medium.com/@shlee1353/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-dom-%ED%81%B4%EB%9E%98%EC%8A%A4-%EC%84%A0%ED%83%9D%EC%9E%90-c13811ae53c9>
노드에 접근할 수 있는 방법은 다양하니 궁금하면 goekd 해당 링크를 참조해보는걸 권장한다.

## 39.3 노드 탐색

---

### 39.3.1 공백 텍스트 노드

HTML 요소 사이의 스페이스, 탭, 줄바꿈(개행) 등의 공백 문자는 텍스트 노드를 생성한다. 이는 자칫 오류나 버그로 불러일으킬 수 있기에, 의도적으로 공백 텍스트 노드를 사용하지 않는다면 조심하도록 하자.

```javascript
<ul><li>
apples</li><li>
banana</li></ul>
```

### 39.3.2 자식 노드 탐색

|프로퍼티|설명|
|-----|-----|
|Node.prototype.childNodes|자식 노드를 모두 탐색합니다.|
|Elemnet.prototype.children|자식 노드 중 요소 노드만 탐색합니다.|
|Node.prototpye.firstChild|첫 번째 자식 노드를 반환합니다. 반환한 노드는 텍스트 노드이거나 요소 노드입니다.|
|Node.prototpye.lastChild|마지막 자식 노드를 반환합니다.반환한 노드는 텍스트 노드이거나 요소 노드입니다.|
|Element.prototpye.fristElementChild|첫 번째 자식 요소를 반환합니다.|
|Element.prototpye.lastElementChild|마지막 자식 요소 노드를 반환합니다.|

```html
<html>
    <body>
    <ul id="fruits">
        <li id="apple">apple</li>
        <li class="banana">banana</li>
        <li id="orange">orange</li>
    </ul>
    <script>
        const fruits = document.querySelector("#fruits");
        
        console.log(fruits.childNodes);
        // fruits의 모든 자식 노드를 탐색한다 -> [text, li.apple, text, li.banana, text, li.orange, text]

        console.log(fruits.children);        
        // fruits의 요소 노드를 탐색한다 -> [ li.apple, li.banana, li.orange]

        console.log(fruits.firstChild);
        // fruits 요소의 첫 번째 자식을 탐색한다. -> #text
        
        console.log(fruits.lastChild);
        // fruits 요소의 마지막 자식을 탐색한다. -> #text

        console.log(fruits.firstElementChild);
        // fruits 요소의 첫 번째 자식을 탐색하고 요소 노드를 반환한다. -> li.apple

        console.log(fruits.lastElementChild);
        // fruits 요소의 마지막 자식을 탐색하고 요소 노드를 반환한다. -> li.orange

        console.log(fruits.hasChildNodes()); // true
        // fruits 요소에 자식 노드가 존재하는지 확인한다.
    </script>
    </body>
</html>
```

### 39.3.4 요소 노드의 텍스트 노드 검색

요소 노드의 텍스트 노드는 요소 노드의 자식 노드이다. 따라서 요소 노드의 텍스트 노드는 firstChild 프로퍼티로 접근 가능하다.

```html
<body>
    <div id="foo">hello</div>
<script>
    console.log(document.getElementById("foo").firstChild) // #text
</script>
</body>
```

### 39.3.4 부모 노드 탐색

```html
<body>
    <ul id="fruits">
        <li id="apple">apple</li>
        <li class="banana">banana</li>
        <li id="orange">orange</li>
    </ul>
<script>
    const appple = document.getElementById("apple");

    console.log(banana.parentNode); // true
</script>
</body>
```

### 39.3.4 형제 노드 탐색

|프로퍼티|설명|
|-----|-----|
|Node.prototype.previousSibling|자신의 이전 형제 노드를 탐색하여 반환합니다. 반환하는 형제 노드는 요소 노드뿐 아니라 텍스트 노드일 수 있습니다.|
|Elemnet.prototype.nextSibling|자신의 다음 형제 노드를 탐색하여 반환합니다. 반환하는 형제 노드는 요소 노드뿐 아니라 텍스트 노드일 수 있습니다.|
|Node.prototpye.previousElementSibling|자신의 이전 형제 노드를 탐색하여 반환합니다. 반환하는 형제 노드는 요소 노드만 입니다.|
|Node.prototpye.nextElementSibling|자신의 다음 형제 노드를 탐색하여 반환합니다. 반환하는 형제 노드는 요소 노드만 입니다.|

```html
<html>
    <body>
    <ul id="fruits">
        <li id="apple">apple</li>
        <li class="banana">banana</li>
        <li id="orange">orange</li>
    </ul>
    <script>
        const fruits = document.querySelector("#fruits");
        
        const { firstChild } = furits;
        console.log(firstChild); // #text
        // fruits의 첫 번째 자식 노드를 탐색한다. 

        const { nextSibling } = firstChild;
        console.log(nextSibling); // li.apple
        // fruits의 첫 번째 자식 노드(텍스트 노드)의 다음 형제 노드를 검색한다.

        const { previousSibling } = nextSibling;
        console.log(previousSibling); // #text
        // li.apple의 이전 형제 노드를 탐색한다.
        
        const { firstElemnetChild } = furits;
        console.log(firstElemnetChild); // li.apple
        // fruits의 첫 번째 자식 요소 노드를 탐색한다

        const { nextElemnetSibling } = firstElementChild;
        console.log(nextElemnetSibling); // li.banana
        // fruits의 첫 번째 자식 요소 노드(li.apple)의 다음 형제 노드를 탐색한다

        const { previousElementSibling } = nextElementSibling;
        console.log(previousElementSibling); // li.apple
        // li.banana의 이전 형제 요소 노드를 탐색한다
    </script>
    </body>
</html>
```

## 39.5 요소 노드의 텍스트 조작

---

### 39.5.1 nodeValue

지금까지 살펴본 노드 탐색, 노드 정보 프로퍼티는 모두 읽기 전용 접근자 프로퍼티이다. 지금부터 살펴볼 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티이다. 따라서 참조와 할당 모두 가능하다. 노드 객체의 nodeValue 프로퍼티를 참조하면서도 노드 객체의 값을 반환한다. 노드 객체의 값이란 텍스트 노드의 텍스트다. 따라서 텍스트 노드가 아닌 노드, 즉 문서 노드나 요소 노드의 nodeValue 프로퍼티를 참조하면 null를 반환한다. 텍스트 노드의 nodeValue 프로퍼티를 참조할 때만 텍스트 노드의 값, 즉 텍스트를 반환한다.

```html
<html>
    <body>
        <div id="foo">hello</div>
    <script>
        const foo = document.querySelector("#foo");

        console.log(foo.nodeValue); // null

        let { firstChild } = foo;
        console.log(firstChild.nodeValue); // hello

        firstChild.nodeValue = 'world';
        console.log(firstChild.nodeValue); // world
    </script>
    </body>
</html>
```

### 39.5.2 textContent

요소 노드의 textContent 프로퍼티를 참조하면 요소 노드의 콘텐츠 영역(시작 태그와 종료 태그 사이) 내의 텍스트를 모두 반환한다.

```html
<html>
    <body>
        <div id="foo">hello<span>world</span></div>
    <script>
        const foo = document.querySelector("#foo");

        console.log(foo.textContent); // hello world
    </script>
    </body>
</html>
```

## 39.6 DOM 조작

---

DOM 조작은 새로운 노드를 생성하여 DOM에 추가하거나 기존 노드를 삭제 또는 교체하는 것을 말한다.

### 39.6.1 innerHTML

innerHTML 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 HTML 마크업을 취득하거나 변경한다.

```html
<html>
    <body>
        <div id="foo">hello<span>world</span></div>
    <script>
        const foo = document.querySelector("#foo");

        console.log(foo.innerHTML); // hello <span>world</span>

        foo.innerHTML = "";
        console.log(foo.innerHTML); // '';
    </script>
    </body>
</html>
```

textContent 프로퍼티를 참조하면 HTML 마크업을 무시하고 텍스트만 반환하지만 innerHTML 프로퍼티는 HTML 마크업이 포함된 문자열을 그대로 반환한다.

### 39.6.2 노드 조작

```html
<html>
    <body>
        <ul id="fruits">
            <li>apple</li>
        </ul>
   <script>
        const fruits = document.querySelector("#fruits");

        const li = document.createElement('li'); // 요소 노드 생성
        const textNode = document.createTextNode('banana'); // 텍스트 노드 생성

        li.appendChild(textNode); // 텍스트 노드를 li 요소 노드의 자식 노드로 추가
        fruits.appenChild(li); // li 요소 노드를 fruits 요소 노드의 마지막 자식 노드로 추가
    </script>
    </body>
</html>
```

```html
<html>
    <body>
        <ul id="fruits">
            <li>apple</li>
            <li>orange</li>
        </ul>
   <script>
        const fruits = document.querySelector("#fruits");

        const li = document.createElement('li'); // 요소 노드 생성
        const textNode = document.createTextNode('banana'); // 텍스트 노드 생성

        li.appendChild(textNode); // 텍스트 노드를 li 요소 노드의 자식 노드로 추가
        fruits.insertBefore(li, fruits.lastElementChild);
        // apple - banana - orange

        const li2 = document.createElement('li'); // 교체할 요소 노드 생성
        li2.textNode = "grape";
        fruits.replace(li2, fruits.firstElementChild);
        // grape - banana - orange

        fruits.removeChild(fruits.lastElementChild); // 삭제할 노드 설정
        // grape - banana
    </script>
    </body>
</html>
```

## 39.7 어트리뷰트

HTML 문서의 구성 요소인 HTML 요소는 여러 개의 어트리뷰트(속성)을 가질 수 있다.

ex. input 엘리먼트에 value,id,class 등 여러 속성 부여 가능

### 39.7.1 HTML 어트리뷰트 조작

```html
<html>
    <body>
        <input id="user" tpye="text" value="hello">
   <script>
        const user = document.querySelector("#user");

        const inputValue = input.getAttribute('value');
        console.log(inputValue); // hello

        input.setAttribute('value', 'bye');
        console.log(input.getAttribute('value')); // bye
    </script>
    </body>
</html>
```

### DOM 프로퍼티와 어트리뷰트의 차이

얼핏 봐선, 어트리뷰트와 DOM 프로퍼티 간의 차이가 없이 속성 값에 접근하여 참조하는 것이 중복되어 보인다. 하지만 둘 간에는 명확한 차이점이 존재한다.

- 어트리뷰트 : HTML 속성의 값은 사용자 입력에 의해 변하지 않고 언제나 동일한 결과를 갖는다. 즉 초기 상태의 값은 변하지 않고 유지된다.

- DOM 프로퍼티 : 사용자 입력에 의한 상태 변화에 반응하며 언제나 최신 상태를 유지한다.

```html
<html>
    <body>
        <input id="user" tpye="text" value="hello">
   <script>
        const input = document.querySelector("#user");

        input.value = 'foo';
        console.log(input); // foo

        const inputValue = input.getAttribute('value');
        console.log(inputValue); // hello
    </script>
    </body>
</html>
```

## 39.8 스타일

---

### 39.8.1 인라인 스타일 조작

```html
<html>
    <body>
        <div style="color : red">hello</div>
   <script>
        const div = document.querySelector("div");

        div.style.color = 'blue' // 인라인 스타일 변경

        div.style.width = '100px' // 인라인 스타일 추가
        div.style.bakcgroundColor= 'yellow' // 인라인 스타일 추가
    </script>
    </body>
</html>
```

### 39.8.2 클래스 조작

클래스의 조작은 className와 classList로 조작 가능하다.

```html
<html>
    <style>
        .box {widt: 100px}
        .red { color : red}
        .blue { color : blue}
    </style>
    <body>
        <div class="box red">hello</div>
   <script>
        const div = document.querySelector("div");

        console.log(div.className) // 'box red'

        box.className = box.className.replace('red', 'blue')        console.log(div.className) // 'box blue'
    </script>
    </body>
</html>
```

className을 통해 원하는 HTML 요소의 할당된 class를 파악할 수 있다.

```html
<html>
    <style>
        .hidden {display : none}
    </style>
    <body>
        <div id="box">hello</div>
        <button id="button1"></button>
        <button id="button2"></button>
   <script>
        const box = document.querySelector("#box");
        const button1 = document.querySelector("#button1");
        const button2 = document.querySelector("#button2");

        button1.addEventListener("click", () => {
            box.classList.add('hidden');
        }) // button1을 클릭하면 box 요소 노드에 hidden이라는 class를 추가합니다.

        button2.addEventListener("click", () => {
        box.classList.remove('hidden');
        }) // button2을 클릭하면 box 요소 노드에 hidden이라는 class를 삭제합니다.
    </script>
    </body>
</html>
```

이렇듯 classList를 활용하여 class를 원하는 HTML 요소 노드에 추가하거나 삭제하고 수정할 수도 있다. 더 나아가 토글도 가능하다.

자세한 내용은 <https://hianna.tistory.com/469>을 참조해보자.
