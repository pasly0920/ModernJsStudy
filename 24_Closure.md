# 24 클로저

- 클로저: 함수와 그 함수가 선언된 렉시컬 환경(상위 스코프)과의 조합

## 24.1 렉시컬 스코프

- 외부 렉시컬 환경에 대한 참조 -> 렉시컬 환경 -> 실행 컨텍스트
- 렉시컬 스코프(정적 스코프): 함수가 정의된 위치에 의해 함수의 상위 스코프가 결정되는 방식
- 상위 스코프에 대한 참조는 함수 정의가 평가되는 시점에 함수가 정의된 환경(위치)에 의해 렉시컬 환경의 '외부 렉시컬 환경에 대한 참조'에 저장할 참조값이 결정된다.

## 24.2 함수 객체의 내부 슬롯 [[Environment]]

- 상위 스코프: 함수 정의가 위치하는 스코프
- 렉시컬 스코프가 가능하려면 함수는 상위 스포크를 기억해야 하는데, 이를 위해 함수는 자신의 내부 슬롯 [[Environment]]에 상위 스코프의 참조를 저장한다.
- 이때 이 상위 스코프의 참조는 현재 실행 중인 실행 컨텍스트의 렉시컬 환경을 가리킨다.
- 또한 이 상위 스코프의 참조는 자신이 호출되었을 때 생성될 함수 렉시컬 환경의 '외부 렉시컬 환경에 대한 참조'에 저장될 참조값과 같다.

```js
const x = 1;

function foo () {
    const x = 10;

    // 상위 스코프는 호출된 위치가 아닌 정의된 위치애 의해 결정된다.(렉시컬 스코프)
    bar();
}

function bar () {
    console.log(x);
}

foo(); // Q1. 1번: 1, 2번: 10
bar(); // Q2. 1번: 1, 2번: 10

// Q3. 함수 bar와 foo의 내부 슬롯 [[Environment]]에는 무엇이 저장되어 있을까?
// Q4. 함수 bar와 foo의 렉시컬 환경의 OuterLexicalEnvironmentReference 컴포넌트에는 무엇이 저장되어 있을까?
// Q5. 만약 자바스크립트가 동적 스코프를 따른다면 위 문제의 정답은 달라지는가?
```

## 24.3 클로저와 렉시컬 환경

- 중첩 함수의 클로저: 외부 함수보다 중첩 함수가 더 오래 유지되는 경우, 중첩 함수가 이미 생명 주기가 종료한 외부 함수의 변수를 참조하는 것
- 클로저: 상위 스코프의 식별자를 참조하고 있고 중첩 함수가 외부 함수보다 더 오래 유지되는 함수
- 자유 변수: 클로저에 의해 참조되는 상위 스코프의 변수 -> 클로저: '자유 변수에 닫혀있다', 즉 '자유 변수에 묶여 있는 함수'

클로저인 함수

```js
const x = 1;

// 과정1
function outer () {
    const x = 10;
    const inner = function () { console.log(x); }; // 과정2
    return inner;
}

// 외부 함수 outer를 호출하면 중첩 함수 inner를 반환한다.
// outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 팝되어 제거된다.
const innerFunc = outer(); // 과정3
innerFunc(); // 과정4
```

클로저가 아닌 함수 1

```js
// 상위 스코프의 어떤 식별자도 참조하지 않는 함수는 클로저가 아니다.

function foo() {
    const x = 1;
    const y = 2;

    function bar() {
        const z = 3;

        console.log(z);
    }

    return bar;
}

const bar = foo();
bar(); // 3
```

클로저가 아닌 함수 2

```js
function foo() {
    const x = 1;

    // 함수 bar는 상위 스코프의 식별자를 참조하고 있지만 곧바로 소멸하므로 클로저가 아니다.
    function bar() {
        console.log(x);
    }
    bar();
}

foo() // 1
```

## 24.4 클로저의 활용

- 클로저를 사용하는 목적: 상태가 의도치 않게 변경되지 않도록 은닉하고, 특정 함수에게만 상태 변경을 허용히여 상태를 안전하게 변경하고 유지하기 위함

오류 가능성을 가진 안 좋은 코드

```js
let = num = 0;

const increase = function () {
    return ++num;
};

console.log(increase());
console.log(increase());
console.log(increase());
```

클로저를 사용한 안정적인 코드

```js
const increase = (function () {
    let num = 0;

    return function () {
        return ++num;
    };
}());

console.log(increase());
console.log(increase());
console.log(increase());
```

카운트 상태를 감소시키는 안 좋은 코드

```js
const counter = (function () {
    let num = 0;

    return {
        increase() {
            return ++num;
        },
        decrease() {
            return num > 0 ? --num : 0;
        }
    };
}());

console.log(counter.increase()); // 1
console.log(counter.increase()); // 2

console.log(counter.decrease()); // 1
console.log(counter.decrease()); // 0
```

위 예제를 생성자 함수로 표현한 안 좋은 코드

```js
const Counter = (function() {
    let num = 0;

    function Counter() {

    }

    Counter.prototype.increase = function () {
        return ++num;
    };

    Counter.prototype.decrease = function () {
        return num > 0 ? --num : 0;
    };

    return Counter;
}());

const counter = new Counter();

console.log(counter.increase()); // 1
console.log(counter.increase()); // 2

console.log(counter.decrease()); // 1
console.log(counter.decrease()); // 0
```

클로저를 사용한 좋은 코드

```js
function makeCounter(aux) {
    let counter = 0;

    return function () {
        counter = aux(counter);
        return counter;
    };
}

function increase(n) {
    return ++n;
}

function decrease(n) {
    return --n;
}

const increaser = makeCounter(increase);
console.log(increaser()); // 1
console.log(increaser()); // 2

const decreaser = makeCounter(decrease);
console.log(decreaser()); // -1
console.log(decreaser()); // -2
```

클로저로 증감이 연동된 카운터를 구현한 코드

```js
const counter = (function () {
    let counter = 0;

    return function (aux) {
        counter = aux(counter);

        return counter;
    };
}());

function increase(n) {
    return ++n;
}

function decrease(n) {
    return --n;
}

console.log(counter(increase)); // 1
console.log(counter(increase)); // 2

console.log(counter(decrease)); // 1
console.log(counter(decrease)); // 0
```

## 24.5 캡슐화와 정보 은닉

- 캡슐화: 프로퍼티(객체의 상태)와 메서드(프로퍼티를 참조하고 조작할 수 있는 동작)를 하나로 묶는 것
- 정보 은닉: 객체의 특정 프로퍼티나 메서드를 감출 목적으로 캡슐화를 사용하는 것
- 정보 은닉은 적절치 못한 접근으로부터 상태가 변경되는 것을 방지해 정보를 보호하고, 객체 간의 상호 의존성을 낮추는 효과가 있다.
- 대부분의 객체지향 프로그래밍 언어는 클래스를 정의하고 그 클래스를 구성하는 맴버(프로퍼티와 메서드)에 대하여 public, private, protected 같은 접근 제한자를 선언하여 공개 범위를 한정할 수 있다. public으로 선언된 프로퍼티와 메서드는 클래스 외부에서 참조할 수 있지만 private으로 선언된 경우에는 클래스 외부에서 참조할 수 없다.
- 하지만 자바스크립트는 이러한 접근 제한자를 제공하지 않는다. 따라서 자바스크립트 객체의 모든 프로퍼티와 메서드는 기본적으로 public하다.

```js
function Person(name, age) {
    this.name = name; // 변수 name 프로퍼티는 현재 외부로 공개되어 있어서 외부에서 자유롭게 참조하거나 변경할 수 있다. public
    let _age = age; // 변수 _age는 생성자 함수 Person의 지역 변수이므로 생성자 함수 Person 외부에서 참조할 수 없다. private

    // 인스턴스 메서드
    this.sayHi = function () {
        console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
    };
}

const me = new Person('Lee', 20);
me.sayHi() // Hi! My name is Lee. I am 20
console.log(me.name); // Lee
console.log(me._age); // undefined

const you = new Person('Kim', 30);
you.sayHi() // Hi! My name is Kim. I am 30
console.log(you.name); // Kim
console.log(you._age); // undefined
```

인스턴스 메서드 sayHi를 프로포타입 메서드로 변경하여 sayHi의 중복 생성을 방지한 코드

```js
const Person = (function () {
    let _age = 0; // private

    // 생성자 함수
    function Person(name, age) {
    this.name = name; // public
    _age = age; 
    }

    // 프로포타입 메서드
    Person.prototype.sayHi = function () {
        console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
    }

    // 생성자 함수를 반환
    return Person;
}());

const me = new Person('Lee', 20);
me.sayHi(); // Hi! My name is Lee. I am 20.
console.log(me.name); // Lee
console.log(me._age); // undefined

const you = new Person('Kim', 30);
you.sayHi(); // Hi! My name is Kim. I am 30.
console.log(you.name); // Kim
console.log(you._age); // undefined

// 클로저의 문제: 변수 _age의 값이 변경된다.
me.sayHi(); // Hi! My name is Lee. I am 30.
```

## 24.6 자주 발생하는 실수

잘못된 코드

```js
var funcs = [];

for (var i = 0; i < 3; i++) {
    funcs[i] = function () { return i; };
}

for ( var j = 0; j < funcs.length; j++) {
    console.log(funcs[j]());
}
// 3
// 3
// 3
```

올바른 코드

```js
const funcs = [];

for (let i = 0; i < 3; i++) {
    funcs[i] = function () { return i; };
}

for (let j = 0; j < funcs.length; j++) {
    console.log(funcs[j]());
}
// 0
// 1
// 2
```

클로저 이용

```js
var funcs = [];

for(var i = 0; i < 3; i++){
    funcs[i] = (function (id) {
        return function () {
            return id;
        };
    }(i));
}

for(var j = 0; j < funcs.length; j++) {
    console.log(funcs[j]());
}
// 0
// 1
// 2
````
