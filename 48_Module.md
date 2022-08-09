
# 48 모듈

# 48.1 모듈의 일반적 의미

- 모듈: 애플리케이션을 구성하는 개별적 요소로서 재사용 가능한 코드 조각
- export: 공개가 필요한 모듈을 선택적으로 공개하는 것
- import: 모듈 사용자가 모듈이 공개한 자산을 자신의 스코프 내로 불려들여 재사용하는 것

# 48.2 자바스크립트와 모듈

- 자바스크립트는 웹페이지의 단순한 보조 기능을 처리하기 위한 제한적인 용도로 탄생했기 때문에 모듈이 성립하기 위해 필요한 파일 스코프와 import, export를 지원하지 않는다.
- CommonJS와 AMD(Asynchronous Module Definition)은 모듈 시스템을 도입하여 자바스크립트가 클라이언트 사이드에서도 사용될 수 있도록 한다.
- Node.js는 모듈 시스템을 지원하는 CommonJS를 따르므로 Node.js 환경에서 파일들은 독립적인 파일 스코프(모듈 스코프)를 갖는다.

# 48.3 ES6 모듈(ESM)

- ES6에서는 클라이언트 사이드 자바스크립트에서도 동작하는 모듈 기능을 추가했다.
- ES6 모듈의 사용법: script 태그에 type="module" 어트리뷰트를 추가한다. 이때 ESM의 파일 확장자는 mjs를 사용한다.

```html
<script type="module" src="app.mjs"></script>
```

## 48.3.1 모듈 스코프

- ESM은 독자적인 모듈 스코프를 갖는다.
- 일반적인 자바스크립트 파일은 script 태그로 분리해서 로드해도 독자적인 모듈 스코프를 갖지 않는다. 즉, 하나의 전역을 공유한다. 따라서 foo.js에서 선언한 변수 x와 bar.js에서 선언한 변수 x는 중복 선언되며 의도치 않게 x 변수의 값이 덮어써진다.(P)

```js
// foo.js
// 변수 x는 전역 변수다.
var x = 'foo';
console.log(window.x); // foo
```

```js
// boo.js
// 변수 x는 전역 변수다.
var x = 'boo';

// foo.js에서 선언한 전역 변수 x의 값이 재할당되었다.
console.log(window.x); // boo
```

```html
<!DOCTYPE html>
<html>
<body>
    <script src="foo.js"></script>
    <script src="boo.js"></script>
</body>
</html>
```

- ESM은 파일 자체의 독자적인 모듈 스코프를 제공하므로 모듈 내에서 var 키워드로 선언한 변수는 전역 변수가 아니며 window 객체의 프로퍼티도 아니다.(S)

```mjs
// foo.mjs
var x = 'foo';
console.log(x); // foo
console.log(window.x); // undefined
```

```mjs
// boo.mjs
var x = 'boo';
console.log(x); // boo
console.log(window.x); // undefined
```

```html
<!DOCTYPE html>
<html>
<body>
    <script type="module" src="foo.mjs"></script>
    <script type="module" src="boo.mjs"></script>
</body>
</html>
```

- 확장자 mjs로 선언하면 모듈 스코프가 다르기 때문에 모듈 내에서 선언한 식별자는 모듈 외부에서 참조할 수 없다.

```mjs
// foo.mjs
var x = 'foo';
console.log(x); // foo
```

```mjs
// boo.mjs
console.log(x); // ReferenceError: x is not defined
```

```html
<!DOCTYPE html>
<html>
<body>
    <script type="module" src="foo.mjs"></script>
    <script type="module" src="boo.mjs"></script>
</body>
</html>
```

## 48.3.2 export 키워드

- 모듈은 독자적인 모듈 스코프를 갖기 때문에 모듈 내부에서 선언한 모든 식별자는 기본적으로 해당 모듈 내부애서만 참조할 수 있다.
- export 키워드를 사용하면 모듈 내부에서 선언한 식별자를 외부에 공개하여 다른 모듈들이 재사용할 수 있도록 할 수 있다.

export 키워드를 사용하는 방법

1. export 키워드는 변수 함수, 클래스 등 모든 식별자의 선언문 앞에 사용한다.
2. export 할 대상을 하나의 객체로 구성하여 한 번에 export할 수 있다.

```mjs
// lib.mjs
// 1번 방식
// 변수의 공개
export const pi = Math.PI;

// 함수의 공개
exprot function square(x) = {
    return x*x;
}

// 클래스의 공개
export class Person {
    constructor(name) {
        this.name = name;
    }
}
```

```js
// lib.mjs
const pi = Math.PI;

function square(x) = {
    return x*x;
}

class Person {
    constructor(name) {
        this.name = name;
    }
}

// 변수, 함수, 클래스를 하나의 객체로 구성하여 공개
export { pi, square, Person};
```

## 48.3.3 import 키워드

- import 키워드를 사용하여 다른 모듈에서 공개한 식별자를 자신의 모듈 스코프 내부로 로드할 수 있다. 이때 다른 모듈이 export한 식별자 이름으로 import 해야 하머 ESM의 경우 파일 확장자를 생략할 수 있다.
- 아래 코드에서 app.mjs는 애플리케이션의 진입점이므로 반드시 script 태그로 로드해야 한다.
- lib.mjs는 app.mjs에 의해 로드되는 의존성이므로 script 태그로 로드하지 않아도 된다.

```js
// app.mjs
// 같은 폴더 내의 lib.js 모듈이 export한 식별자 이름으로 import 해야 한다.
// ESM의 경우 파일 확장자를 생략핡 수 있다.
import { pi, square, Person } from './lib.mjs';

console.log(pi); // 3.141592653589793
console.log(square(10)); // 100
console.log(new Person('Lee')); // { name: 'Lee' }
```

```html
<!DOCTYPE html>
<html>
<body>
    <script type="module" src="app.mjs"></script>
</body>
</html>
```

- as 키워드: 모듈이 export한 식별자 이름 지정하기 위한 키워드

```js
// app.mjs
// lib.mjs 모듈이 export한 모든 식별자를 lib 객체의 프로퍼티로 모아 import한다.
import * as lib from './lib.mjs';

console.log(lib.pi); // 3.141592653589793
console.log(lib.square(10)); // 100
console.log(new lib.Person('Lee')); // Person { name: 'Lee' }


// lib.mjs 모듈이 export한 식별자 이름을 일일이 변경하여 import한다.
import { pi as PI, square as sq, Person as P} from './lib.mjs';

console.log(PI);
console.log(sq(10));
console.log(new P('Kim')); // Person { name: 'Kim' }
```

- default 키워드: 모듈에서 이름 없이 하나의 값만 export하기 위한 키워드

```mjs
// lib.mjs
export default x => x * x;
```

```js
import square form './lib.mjs'
console.log(square(3)); // 9
```
