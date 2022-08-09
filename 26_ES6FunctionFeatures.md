# 26장 ES6 함수의 추가 기능

## 26.1 함수의 구분

---

ES6 이전까지 JS의 함수는 다양한 목적으로 사용되었고 이는 편리하지만 여러 문제를 유발할 수 있으며, 성능 면에서도 손해를 유발한다. 다음 예제를 살펴보자.

```javascript
var foo = function () {
  return 1;
};

//일반적인 함수로 호출
foo(); // -> 1

//생성자 함수로서 호출
new foo(); // -> foo {}

//메서드로서 호출
var obj = { foo: foo };
obj.foo(); // -> 1
```

이처럼 ES6 이전의 함수는 사용 목적에 따라 명확히 구분되지 않는다. 즉 ES6 이전의 모든 함수는 일반 함수로서 호출할 수 있는 것은 물론 생성자 함수로서 호출할 수 있다. 다시 말해 ES6 이전의 모든 함수는 callable이면서 constructor이다. 여기서 callable은 호출할 수 있는 함수 객체를 의미하며 인스턴스를 생성할 수 있는 함수 객체를 constructor이라고 부른다.

주의할 것은 상기했던 내용에 따라, 객체에 바인딩된 함수, 메서드 역시 일반함수는 물론 생성자 함수로 호출 가능하다는 점이다.
이러한 일은 자주 일어나지는 않지만 일어날 가능성이 존재한다. 이에 대한 예시를 살펴보겠다.

```javascript
//프로퍼티 f에 바인딩된 함수는 callable이며 constructor이다.
var obj = {
  x: 10,
  f: function () {
    return this.x;
  },
};

// 프로퍼티 f에 바인딩된 함수를 메서드로서 호출
console.log(obj.f());

// 프로퍼티 f에 바인딩된 함수를 일반 함수로써 호출
var bar = obj.f;
console.log(bar()); // undefined

console.log(new obj.f()); //f {}
```

위와 같은 일이 일어날 수 있다는 것은 성능면에서도 문제가 있다. 객체에 바인딩된 함수가 constructor라는 것은 객체에 바인딩된 함수가 prototype 프로퍼티를 가지며, 프로토타입 객체도 생성한다는 것을 의미하기 때문이다. 함수에 전달되어 보조 함수의 역할을 하는 콜백 함수도 마찬가지이다. 콜백함수도 constructor이기에 불필요한 프로토타입 객체를 생성한다.

이처럼 ES6 이전의 모든 함수는 사용 목적에 따라 명확한 구분이 없으므로 호출 방식에 특별한 제약이 없고 생성자 함수로 호출되지 않아도 프로토타입 객체를 생성한다. 이는 혼란스러우며 실수를 유발할 가능성이 있고 성능에도 좋지 않기에 ES6에서는 함수를 목적에 따라 세 가지 종류로 명확히 구분했다.

| ES6 함수의 구분    | constructor | prototype | super | arguments |
| ------------------ | ----------- | --------- | ----- | --------- |
| 일반 함수(Normal)  | O           | O         | X     | O         |
| 메서드(Method)     | X           | X         | O     | O         |
| 화살표 함수(Arrow) | X           | X         | X     | X         |

여기서 일반 함수란 함수 선언문이나 함수 선언식으로 정의한 함수이머 ES6 이전의 함수와 차이가 없다. 하지만 ES6의 메서드와 화살표 함수는 이전의 함수와 명확한 차이가 있다. 일반 함수는 constructor이지만 메서드 함수와 화살표 함수는 non-constructor이다. 이에 대해 조금 더 자세히 알아보자.

## 26.2 메서드

---

ES6 이후 제대로 정의된 메서드는 메서드 축약 표현으로 정의된 함수만을 의미한다.

```javascript
const obj = {
    x: 1,
    foo() {return this.x;}
    //foo는 메서드이다.
    bar : function() { return this.x };
    //bar에 바인딩된 함수는 메서드가 아닌 일반 함수이다.
}


console.log(obj.foo()); // 1
console.log(obj.bar()); // 1

new obj.foo(); // -> TypeError: obj.foo is not a constructor
new obj.bar(); // -> bar{}

obj.foo.hasOwnProperty('prototype');// false -> obj.foo는 constructor가 아닌 ES6 메서드이므로 prototype 프로퍼티가 없음
obj.bar.hasOwnProperty('prototype');// true -> obj.bar는 constructor인 일반 함수이므로 prototype 프로퍼티가 있다.

String.prototype.toUpperCase.prototype; // undefined
Number.isFinite.prototype; //undefined
```

ES6 사양에서 정의한 메서드(이하 ES6 메서드)는 인스턴스를 생성할 수 없는 non-constructor이다. 따라서 ES6 메서드는 생성자 함수로서 호출할 수 없다. 참고로 표준 빌트인 객체가 제공하는 프로토타입 메서드와 정적 메서드는 모두 non-constructor이다.

**ES6 메서드는 자신을 바인딩한 객체를 가리키는 내부 슬롯 [[HomeObject]]를 갖는다.** super 참조는 내부 슬롯 [[HomeObject]]를 사용하여 수퍼클래스의 메서드를 참조하므로 내부 슬롯 [[Homeobject]]를 가지는 ES6 메서드는 super키워드를 사용할 수 있다.

아래는 그 예제이다.

```javascript
const base = {
  name: "Lee",
  sayHi() {
    return `Hi! ${this.name}`;
  },
};

const derived = {
  __proto__: base,
  //sayHi는 ES6 메서드로 [[HomeObject]]를 갖는다.
  //[[HomeObject]]는 sayHi가 바인딩된 객체인 derived를 가리키고 super는 sayHi의 [[Homeobject]]의 프로토타입인 base를 가리킨다.
  sayHi() {
    return `${super.sayHi()}. how are you doing`;
  },
};

console.log(derived.sayHi()); // Hi! Lee. how are you doing
```

ES6메서드가 아닌 함수는 super 키워드를 사용할 수 없다. ES6 메서드가 아닌 함수는 내부 슬롯 [[HomeObject]]를 가지지 않기 때문이다. 이처럼 ES6 메서드는 본연의 기능 (super)를 추가하고 의미적으로 맞지 않는 기능(constructor)은 제거했다. 따라서 메서드를 정의할 때, 프로퍼티 값으로 익명 함수 표현식을 할당하는 ES6 이전의 방식은 사용하지 않는 것이 좋다.

## 26.3 화살표 함수

---

화살표 함수는 function 키워드 대신 화살표(=>)를 사용하여 기존의 함수 정의 방식보다 간략하게 함수를 정의할 수 있다. 화살표 함수는 표현 뿐만 아니라 내부 동작 역시 간략하다. 특히 화살표 함수는 콜백 함수 내부에서 this 가 전역 객체를 가리키는 문제를 해결하기 위한 대안으로 유용하다.

### 26.3.1 화살표 함수의 정의

- 화살표 함수 정의는 함수 표현식으로 정의되며 호출 방식은 기존과 동일하다.
- 매개변수가 여러 개인 경우 소괄호 () 안에 매개변수를 선언한다.
- 매개 변수가 한 개인 경우 소괄호 ()를 생략할 수 있다.
- 매개 변수가 없다고 하더라도 소괄호 ()를 생략할 수 없다.
- 함수 몸체가 하나의 statement로 구성된다면 {}를 생략할 수 있다.
- 함수 몸체를 감싸는 중괄호를 생략할 경우 함수 몸체 내부의 statement는 expression이어야 한다. (expression이 아닌 statement는 return 할 수 없기 때문)
- 객체 리터럴을 반환하는 경우 객체 리터럴을 소괄호로 감싸 주어야 한다.
- 함수 몸체 가 여러 개의 statement로 구성된다면 함수 몸체를 감싸는 중괄호 {}를 생략할 수 없다. 이 떄 반환값이 있다면 명시적으로 return 을 사용해 반환해야 한다.
- 화살표 함수 역시 즉시 실행 함수로 사용할 수 있다.
- 화살표 함수도 일급 객체이므로 map, filter, reduce와 같은 고차 함수에 인수로 전달할 수 있다.

```javascript
const mulitply = (x, y) => x * y;
multiply(2, 3); // -> 6

const double = (x) => x * 2;
const double = (x) => {
  return x * 2;
};
// 위 둘은 동일함

const MainPage = () => {
  return <div>//...</div>;
};

const create = (id, content) => ({ id, content });
const create = (id, content) => {
  return { id, content };
};
// 위 두 식은 동일

[1, 2, 3].map((v) => v * 2); // -> [2, 4, 6];
```

이처럼 화살표 함수는 콜백 함수로 정의할 때 유용하다. 화살표 함수는 표현만 간략한 것이 아니다. 화살표 함수는 일반 함수의 기능을 간략화했으며 this도 편리하게 설계되었다. 일반 함수와 화살표 함수의 차이에 대해 살펴보자.

### 26.3.2 화살표 함수와 일반 함수의 차이

- 화살표 함수는 인스턴스를 생성할 수 없는 non-constructor이다. 화살표 함수는 인스턴스를 생성할 수 없으므로 prototype 프로퍼티가 없고 프로토타입도 생성하지 않는다.
- 중복된 매개변수 이름을 선언할 수 없다. 일반함수는 중복된 매개변수 이름을 선언해도 에러가 발생하지 않는다.(단 strict mode에서는 에러 발생)
- 화살표 함수는 함수 자체의 this, arguments, super, new, target 바인딩을 갖지 않는다. 따라서 화살표 함수 내부에서 this, arguments, super, new.target 등을 참조 시에 스코프 체인을 통해 상위의 스코프체인의 것을 참조하며 화살표 함수가 여러 번 중첩된 경우 더 상위의 스코프를 참조할 수도 있다.

```javascript
const Foo = () => {};
new Foo(); // TypeError : Foo is not a constructor

const arrow = (a, a) => a + a; // SyntaxError: Duplicate parameter name not allowed in this context
```

### 26.3.3 this

화살표 함수가 일반 함수와 구별되는 가장 큰 특징은 this이다. 그러나 화살표 함수는 다른 함수의 인수로 전달되어 콜백 함수로 사용되는 경우가 많다. 화살표 함수의 this는 일반 함수의 this와 다르게 작동한다. 이는 콜백함수의 this 문제를 해결하기 위해 의도적으로 설계된 것으로 콜백 함수 내부의 this 문제를 다시 한 번 살펴보자.

22장에서 살펴보았듯이 this는 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다. 다시말해 함수를 정의할때 this가 정해지는 것이 아니라 함수가 어디서 호출되었는지에 따라서 this에 바인딩할 객체가 동적으로 결정된다. 이때 주의할 것은 일반 함수로서 호출되는 콜백함수의 경우이다. 고차 함수의 인수로 전달되어 고차 함수 내부에서 호출되는 콜백함수도 중첩함수라고 할 수 있다. 다음 예제를 살펴보자.

```javascript
class Perfixer {
  constructor(prefix) {
    this.prefix = prefix;
  }

  //add 메서드는 인수로 전달받은 배열 arr을 순회하며 배열의 모든 요소에 prefix를 추가한다.
  add(arr) {
    return arr.map(function (item) {
      return this.prefix + item;
    }); // TypeError : Cannot read property 'prefix' of undefined;
  }
}

const perfixer = new Prefix("-webkit-");
console.log(prefixer.add(["transition", "user-select"]));
```

위 예제를 실행시 기대값은 ['webkit-transition', '-webkit-user-select']다. 하지만 TypeError가 발생한다. 이유는 this가 undefined를 가리키기 때문이다. 메서드의 형태인 add 내에서 this는 prefixer객체를 제대로 가리키고 있지만 그 안의 map으로 전달된 함수는 일반함수로 this가 undefined를 가리키기에 문제가 발생했다. 이전에 살펴보았듯이 일반함수로 호출되는 모든 함수의 내부의 this는 전역 객체를 가리키고 이 안에서 strict mode는 암묵적으로 적용된다. 그런데 일전에 이야기했듯이 클래스 내부의 모든 코드는 strict mode가 자동 적용되기에 strict mode에서 일반함수로서 호출된 모든 함수는 전역 객체가 아니라 undefined가 바인딩되므로 콜백 함수 내부의 this에서 undefined가 바인딩된다.

이떄 발생하는 문제가 콜백함수 내부의 this 바인딩 문제로 콜백함수 내부의 this가 외부 함수의 this가 다른 값을 가리키고 있기에 문제가 발생한 것이고 ES6 이전에는 아래와 같은 방식들로 문제를 회피했다.

- add 메서드를 호출한 prefixer 객체를 가라키는 this를 일단 회피시킨 후에 콜백 함수 내부에서 사용한다.

```javascript
...
add(arr) {
    const that = this;
    return arr.map(function(item) {
        return that.prefix + ' ' + item;
    });
}
```

- Array.prototype.map의 두 번째 인수로 add 메서드로 호출한 prefixer 객체를 가리키는 this를 전달한다.

```javascript
...
add(arr){
    return arr.map(function(item) {
        return this.prefix + ' ' + item;
    }, this); // this에 바인딩 된 값이 콜백 함수 내부의 this에 바인딩된다.
}
```

- Function.prototype.bind 메서드를 사용하여 add 메서드를 호출한 prefixer 객체를 가리키는 this를 바인딩한다.

```javascript
... add(arr) {
    return arr.map(function(item) {
        return this.prefix + ' ' + item;
    }.bind(this)); // this에 바인딩된 값이 콜백함수 내부의 this에 바인딩된다.
}
```

**화살표 함수는 함수 자체의 this를 갖지 않는다.** 따라서 화살표 함수 내부의 this를 참조하면 상위 스코프의 this를 그대로 참조한다. 이를 lexical this라고 한다. 이는 마치 lexical scope와 같이 화살표 함수의 this가 함수가 정의된 위치에 의해 결정된 다는 것을 의미한다.

화살표 함수를 제외한 모든 함수에는 this 바인딩이 반드시 존재했고, 따라서 ES6에 화살표 함수가 도입되기 전까지 스코프 체인을 통해 this를 탐색할 필요가 없었다. 하지만 화살표 함수에는 자체 this 바인딩이 존재하지 않고 따라서 화살표 함수 내에서 this를 참조 스코프 체인을 통해서 상위 스코프에서 this를 탐색한다.

화살표 함수가 중첩되어 있을 경우 상위 스코프에도 this 바인딩이 없기에 스코프 체인 상에서 가장 가까운 상위 함수 중에서 화살표 함수가 아닌 함수의 this를 참조한다. 만약 화살표 함수가 전역 함수라면 화살표 함수의 this는 전역 객체를 가리킨다. 전역 함수의 상위 스코프는 전역이고 this는 전역 객체를 가리키기 때문이다.

메서드를 화살표 함수로 정의하는 것은 피해야 한다. 이 경우 this가 메서드를 가진 객체가 아닌 상위 스코프를 의미할 수 있기 때문이다. 메서드를 정의할 때는 ES6 메서드 축약표현으로 정의한 ES6 메서드를 사용하는 것이 좋다.

```javascript
//Good
const person = {
    name: 'Lee',
    sayHi() {
        console.log(`Hi ${this.name}`);
    }
}
person.sayHi(); // Hi lee

//Bad
const person = {
    name: 'Lee',
    sayHi: () => console.log(`Hi ${this.name}`);
}
person.sayHi(); //Hi
//sayHi에 할당된 화살표 함수 내부의 this는 전역 객체를 가리키므로 이 예제를 브라우저에서 실행 시에 window를 가리키게 되고 window.name(빌트인 프로퍼티)은 빈 문자열로 빈 값이 나옴.
```

프로퍼티를 동적으로 추가할 때는 ES6 메서드 정의를 사용할 수 없으므로 일반 함수를 할당한다.

```javascript
function person(name) {
  this.name = name;
}

person.prototype.sayHi = function () {
  console.log(`Hi ${this.name}`);
};
```

일반 함수가 아닌 ES6 메서드를 동적 추가하고 싶다면 다음과 같이 객체 리터럴을 바인딩한 후에 프로토타입의 constructor 프로퍼티와 생성자 함수 간의 연결을 재설정한다.

```javascript
function person(name) {
  this.name = name;
}

person.prototype = {
  constructor: person,
  sayHi() {
    console.log(`Hi ${this.name}`);
  },
};
```

다음과 같은 예를 살펴보자.

```javascript
//Bad
class person {
  constructor() {
    this.name = "Lee";
    this.sayHi = () => console.log(`Hi ${this.name}`);
  }
}

//Good
class person {
  name = "Lee";
  sayHi() {
    console.log(`Hi ${this.name}`);
  }
}
```

sayHi 클래스 필드에 할당된 화살표 함수의 상위 스코프는 사실 클래스 외부다. 하지만 this는 클래스 외부의 this를 참조하지 않고 클래스가 생성할 인스턴스를 참조한다. 따라서 sayHi클래스 필드에 할당한 화살표 함수 내부에서 참조한 this는 constructor 내부의 this 바인딩과 같다. constuctor 내부의 this 바인딩은 클래스가 생성할 인스턴스를 가리키므로 sayHi 클래스 필드에 할당한 화살표 내부의 this 함수 또한 클래스가 생성한 인스턴스를 가리킨다.

하지만 이는 프로토타입 메서드가 아닌 인스턴스 메소드가 되므로 즉 인스턴스별로 가지는 인스턴스 메소드가 되어 인스턴스가 각각 개별로 메소드를 가지게 되므로 성능상이나 메모리 상으로 손해이므로 피하는 것이 좋다. 즉 메서드를 정의할 때는 ES6 메서드 축약 표현으로 정의한 ES6 메서드를 사용하는 것이 좋다.

### 26.3.4 super

화살표 함수는 함수 자체의 super 바인딩을 갖지 않는다. 따라서 화살표 함수 내부에서 super을 참조시 this와 마찬가지로 상위 스코프의 super을 참조한다.

```javascript
class Base {
  constructor(name) {
    this.name = name;
  }

  sayHi() {
    return `Hi ${this.name}`;
  }
}

class Derived extends Base {
  //화살표 함수의 super는 상위 스코프인 constructor의 super를 가리킨다.
  sayHi = () => `${super.sayHi()} how are you doing`;
}

const derived = new Derived("Lee");
console.log(derived.sayHi()); // Hi ! Lee how are you doing
```

super는 내부 슬롯 [[HomeObject]]를 갖는 ES6 메서드 내에서만 사용할 수 있는 키워드다. sayHi 클래스 필드에 할당한 화살표 함수는 ES6 메서드는 아니지만 함수 자체의 super 바인딩을 갖지 않으므로 super을 참조해도 에러가 발생하지 않고 constructor의 super 바인딩을 참조한다.this와 마찬가지로 클래스 필드에 할당한 화살표 함수 내부에서 super를 참조하면 constructor 내부의 super 바인딩을 참조한다. Derived 클래스의 경우에 constructor는 생략되었지만 암묵적으로 constructor가 생성된다.

### 26.3.5 arguments

화살표 함수는 함수 자체의 arguments 바인딩을 갖지 않으며 나머지와 마찬가지로 상위 스코프의 arguments를 참조한다. arguments 객체는 함수를 정의할 때, 매개변수의 개수를 확정할 수 없는 가변 인자 함수를 구현할 때 유용하다. 하지만 화살표 함수에는 arguments 객체를 사용할 수 없고 상위 스코프의 arguments 객체를 참조할 수는 있지만 상위 함수에 전달된 인수 목록을 참조하므로 그다지 도움이 되지 않는다.

따라서 화살표 함수로 가변 인자 함수를 구현해야 할 떄는 반드시 Rest 파라미터를 사용해야 한다.

## 26.4 Rest 파라미터

---

### 26.4.1 기본 문법

Rest 파라미터(나머지 매개변수)는 매개변수 이름 앞에 세 개의 점 ...을 붙여서 정의한 매개변수를 의미한다. Rest파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받는다. 일반 매개 변수와 Rest 파라미터는 동시에 사용이 가능하다. 이 때 함수에 전달된 인수들은 순차적으로 할당된다.

```javascript
function foo(param, ...rest) {
  console.log(param); //1
  console.log(rest); //[2, 3, 4, 5]
}

foo(1, 2, 3, 4, 5);
```

Rest 파리미터는 말 그대로 먼저 선언된 매개변수에 할당된 인수를 제외한 나머지 인수들로 구성된 배열이 할당되므로 Rest 파라미터는 반드시 마지막 파라미터여야 하며 단 하나만 선언할 수 있다. 동시에 Rest 파라미터는 함수 정의시 선언한 매개변수의 개수를 나타내는 함수 객체의 length 프로퍼티에 영향을 끼치지 않는다. Rest 파라미터는 length 계산시 없는 것으로 처리 0으로 계산.

### 26.4.2 Rest 파라미터와 arguments 객체

ES5에서 함수를 정의할 때 가변 인자 함수의 경우 매개변수를 통해서 인자를 전달받는 것이 불가능하므로 arguments 객체를 활용하여 인수를 전달받았다. arguments 객체는 함수 호출 시 전달된 인수들의 정보를 담고 있는 순회 가능한 유사 배열 객체이며, 함수 내부에서 지역 변수처럼 사용할 수 있다.

하지만 arguments 객체는 배열이 아닌 유사 배열 객체이므로 배열 메서드를 사용하려면 Function.prototype.call이나 Function.prototype.apply 메서드를 활용하여 arguments 객체를 배열로 변환해야하는 번거로움이 있었다.

```javascript
function sum() {
  console.log(agruments);
}

sum(1, 2); // {length: 2, '0' : 1, '1': 2}

function sum() {
  //유사 배열 객체인 arguments 객체를 배열로 변환한다.
  const array = Array.prototype.slice.call(arguments);

  return array.reduce(function (pre, cur) {
    return pre + cur;
  }, 0);
}

function sum(...args) {
  //Rest 파라미터 args에는 배열 [1, 2, 3, 4, 5] 가 할당된다.
  return args.reduce((pre, cur) => pre + cur, 0);
}

console.log(sum(1, 2, 3, 4, 5)); //15
```

ES6에서는 rest 파라미터를 사용하여 가변 인자 함수의 인수 목록을 배열로 직접 전달받을 수 있다. 이를 통해 유사 배열 객체인 arguments 객체를 배열로 변환하는 번거로움을 피할 수 있다.

함수와 ES6 메서드는 Rest 파라미터와 arguments 객체를 동시에 사용할 수 있다. 하지만 화살표 함수는 함수 자체의 arguments 객체를 갖지 않으므로 화살표 함수로 가변 인자 함수를 구현해야 할 때는 반드시 Rest 파라미터를 사용해야 한다.

## 26.5 매개변수 기본값

---

함수를 호출할 때 매개변수의 개수만큼 인수를 전달하는 것이 바람직하지만 그렇지 못하더라도 에러가 발생하지 않는다. JS 엔진이 매개변수의 개수와 인수의 개수를 체크하지 않기 때문이다. 인수를 전달받지 못한 매개변수의 값은 undefined이다. 이를 방치하면 의도치 못한 에러가 발생할 수 있다. 따라서 매개변수에 인수가 잘 전달되었는지 확인하여 인수가 전달되지 않은 경우 매개변수에 기본값을 할당할 필요가 있다. 즉 방어 코드가 필요하다.

```javascript
function sum(x, y) {
  //인수가 전달되지 않아 매개변수의 값이 undefined인 경우 기본값을 할당
  x = x || 0;
  y = y || 0;

  return x + y;
}

function sum(x = 0, y = 0) {
  return x + y;
}
// ES6에서 도입된 매개변수 기본값을 통해 위에서 수행한 인수 체크 및 초기화를 간소화
// 매개 변수 기본값은 매개변수에 인수를 전달하지 않은 경우와 undefined를 전달한 경우에만 유효하다.
```

앞서 살펴본 Rest 파라미터에는 기본값을 지정할 수 없다.
매개변수 기본값은 함수 정의 시 선언한 매개변수 개수를 나타내는 함수 객체의 length 프로퍼티와 arguments 객체에 아무런 영향을 주지 않는다.

```javascript
function foo(...rest = []){
    //...
} // SyntaxError Rest parameter may not have a default initializer

function sum(x, y = 0){
    console.log(arguments);
}
console.log(sum.length); //1 기본값이 있는 parameter가 무시됨

sum(1); //Arguments {'0' : 1}
sum(1, 2); // Arguments {'0' : 1, '1' : 2}
```
