# 22장 This

동작을 나타내는 메서드는 자신이 속한 객체의 상태, 즉 프로퍼티를 참조하고 변경할 수 있어야 한다.
메서드가 자기 자신이 속한 객체의 프로퍼티를 참조하려면 먼저 **자신이 속한 객체를 가르키는 식별자를 참조할 수 있어야 한다.**

**객체 리터럴 방식**으로 생성한 객체의 경우 메서드 내부에서 메서드 자신이 속한 객체를 가리키는 식별자를 재귀적으로 참조할 수 있다.

하지만, 이러한 방법은 일반적이지 않으며 바람직하지도 않다. 이를 위해 자바스크립트는 this라는 특수한 식별자를 제공한다.

## this

this는 **자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수 self-referencing variable**이다.

this는 자바스크립트 엔진에 의해 암묵적으로 생성되며, 코드 어디서든 참조할 수 있다. 함수를 호출하면 arguments 객체와 this가 암묵적으로 함수 내부에 전달된다. 함수 내부에서 arguments 객체를 지역 변수처럼 사용할 수 있는 것처럼 this도 지역 변수처럼 사용할 수 있다.

**단 this가 가리키는 값, 즉 this 바인딩은 함수 호출 방식에 의해 동적으로 결정된다.**

> 바인딩이란 식별자와 값을 연결하는 과정을 의미한다. this 바인딩은 this와 this가 가리킬 객체를 바인딩하는 것이다.

```javascript
// 객체 리터럴
const circle = {
    radius: 5,
    getDiameter() {
        return 2 * this.radius;
    }
};

console.log(circle.getDiameter()); // 10
```

**객체 리터럴**의 메서드 내부에서는 this는 호출한 객체, circle을 가리킨다.

```javascript
// 생성자 함수
function Circle(radius) {
    // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
    this.radius = radius;
}

Circle.prototype.getDiameter = function () {
    // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
    return 2 * this. radius;
}

// 인스턴스 생성
const circle = new Circle(5);
console.log(circle.getDiameter()); // 10
```

**생성자 함수** 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다. 이처럼 this는 상황에 따라 가리키는 대상이 다르다.

> 자바스크립트의 this는 함수가 호출되는 방식에 따라 this 바인딩이 동적으로 결정된다. 전역에서 this 를 참조할 경우 window 객체를 참조한다.

## 함수 호출 방식과 this 바인딩

this 바인딩은 함수 호출 방식에 따라 동적으로 결정되는데, 예제를 통해 살펴보자.

### 1. 일반 함수 호출

기본적으로 this에는 전역 객체가 바인딩된다. strict mode가 적용된 일반 함수의 내부에서 this는 undefined가 바인딩된다.

```javascript
function foo() {
    console.log("foo's this:", this); // window
    function bar() {
        console.log("bar's this: ", this); // window
    }
    bar();
}
foo();
```

일반 함수에서 this에 전역 객체가 바인딩되기 때문에 어떠한 함수라도 일반 함수로 호출되면 this에 전역 객체가 바인딩된다.

### 2. 메서드 호출

메서드 내부의 this에는 메서드를 호출한 객체, 즉 메서드를 호출할 때 메서드 이름 앞의 마침표(.) 연산자 앞에 기술한 객체가 바인딩된다. 주의할 것은 메서드 내부의 this는 메서드를 소유한 객체가 아닌 호출한 객체에 바인딩 된다는 것이다.

```javascript
const person = {
    name: 'Lee',
    getName() {
        // 메서드 내부의 this는 메서드를 호출한 객체에 바인딩된다.
        return this.name;
    }
};

// 메서드 getName을 호출한 객체는 person이다.
console.log(person.getName()); // Lee
```

getName 메서드는 person 객체의 메서드로 정의되었는데, **메서드는 프로퍼티에 바인딩된 함수**이다. 즉, person 객체의 getName 프로퍼티가 가리키는 함수 객체는 person 객체에 포함된 것이 아니라 독립적으로 존재하는 별도의 객체이다. getName 프로퍼티가 함수 객체를 가리키고 있을 뿐이다.

따라서 getName 프로퍼티가 가리키는 함수 객체, 즉 getName 메서드는 다른 객체의 프로퍼티에 할당하는 것으로 다른 객체의 메서드가 될 수도 있고 일반 변수에 할당하여 일반 함수로 호출될 수도 있다.

```javascript
const anotherPerson = {
    name: 'Kim'
};

// getName 메서드를 anotherPerson 객체의 메서드로 할당
anotherPerson.getName = person.getName;

// getName 메서드를 변수에 할당
const getName = person.getName;

// getName 메서드를 일반 함수로 호출
console.log(getName()); // ''
// 일반 함수로 호출된 getName 함수 내부의 this.name은 브라우저 환경에서 window.name과 같다.
// 브라우저 환경에서 window.name은 브라우저 창의 이름을 나타내는 빌트인 프로퍼티이며 기본값은 ''이다.
// Node.js 환경에서 this.name은 undefined이다.
```

**따라서 메서드 내부의 this는 프로퍼티로 메서드를 가리키고 있는 개체와는 관계가 없고 메서드를 호출하는 객체에 바인딩 된다.**

```javascript
function Person(name) {
    this.name = name;
}

Person.prototype.getName = function() {
    return this.name;
};

const me = new Person('Lee');

// getName 메서드를 호출한 객체는 me다.
console.log(me.getName()); // #1 Lee

Person.prototype.name = 'Kim';

// getName 메서드를 호출한 객체는 Person.prototype이다.
console.log(Person.prototype.getName()); // #2 Kim
```

 #1의 경우 getName 메서드를 호출한 객체는 me다. 따라서 getName 메서드 내부의 this는 me를 가리키며, this.name은 'Lee'다.

 #2의 경우 getName 메서드를 호출한 객체는 Person.prototype이다. Person.prototype도 객체이므로 직접 메서드를 호출할 수 있다. 따라서 getName 메서드 내부의 this는 Person.prototype을 가리키며 this.name은 'Kim'이다.

### 생성자 함수 호출

생성자 함수 내부의 this에는 생성자 함수가 (미래에) 생성할 인스턴스가 바인딩된다.

```javascript
// 생성자 함수
function Circle(radius) {
    // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
    this.radius = radius;
    this.getDiameter = function() {
        return 2 * this.radius;
    };
}

// 반지름이 5인 Circle 객체를 생성
const circle1 = new Circle(5);
// 반지름이 10인 Circle 객체를 생성
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

생성자 함수는 객체(인스턴스)를 생성하는 함수다. new 연산자와 함께 호출하면 생성자 함수, 그렇지 않으면 일반 함수로 동작한다.

### Function.prototype.apply/call/bind 메서드에 의한 간접 호출

위 apply,call,bind 메서드는 Function.prototype의 메서드다. 즉, 이들 메서드는 모든 함수가 상속받아 사용할 수 있다.

Function.prototype.apply, Function.prototype.call 메서드는 this로 사용할 객체와 인수 리스트를 인수로 전달받아 함수를 호출한다. apply와 call 메서드의 사용법은 다음과 같다.

```javascript
function getThisBinding() {
    return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

console.log(getThisBinding()); // window

// getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩한다.
console.log(getThisBinding.apply(thisArg)); // { a: 1 }
console.log(getThisBinding.call(thisArg)); // { a: 1 }
```

apply와 call 메서드의 본질적인 기능은 함수를 호출하는 것이다. apply와 call 메서드는 함수를 호출하면서 첫 번째 인수로 전달한 특정 객체를 호출한 함수의 this에 바인딩한다.

두 메서드는 호출할 함수에 인수를 전달하는 방식만 다를 뿐 동일하게 동작한다.

```javascript
function getThisBinding() {
    console.log(arguments);
    return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

// getThisBinding 함수를 호출하면서 인수로 전달한 객체를 this에 바인딩한다.
// apply 메서드는 호출할 함수의 인수를 배열로 묶어 전달한다.
console.log(getThisBinding.apply(thisArg, [1,2,3]));
// Argument(3) [1,2,3, callee: f, Symbol(Symbol.iterator): f]
// { a: 1 }

// call 메서드는 호출할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달한다.
console.log(getThisBinding.call(thisArg, 1,2,3));
// Argument(3) [1,2,3, callee: f, Symbol(Symbol.iterator): f]
// { a: 1 }
```

두 메서드의 대표적인 용도는 arguments 객체와 같은 유사 배열 객체에 배열 메서드를 사용하는 경우다.

Function.prototype.bind 메서드는 apply와 call 메서드와 달리 함수를 호출하지 않는다. 다만 첫 번째 인수로 전달한 값으로 this 바인딩이 교체된 함수를 새롭게 생성해 반환한다.

```javascript
function getThisBinding() {
    return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

// bind 메서드는 첫 번째 인수로 전달한 thisArgs로 this 바인딩이 교체된
// getThisBinding 함수를 새롭게 생성해 반환한다.
console.log(getThisBinding.bind(thisArg)); // getThisBinding
// bind 메서드는 함수를 호출하지는 않으므로 명시적으로 호출해야 한다.
console.log(getThisBinding.bind(thisArg)); // {a: 1}
```

bind 메서드는 메서드의 this와 메서드 내부의 중첩 함수 또는 콜백 함수의 this가 불일치하는 문제를 해결하기 위해 유용하게 사용된다. 다음 예제를 살펴보자.

```javascript
const person = {
    name: 'Lee',
    foo(callback) {
        // #1
        setTimeout(callback, 100);
    }
};

person.foo(function () {
    console.log(`Hi! my name is ${this.name}.`); // #2 Hi! my name is .
});
```

 #2의 시점에서 this는 전역 객체 window를 가리킨다. 따라서 person.foo의 콜백 함수 내부에서 this.name은 window.name과 같다. 따라서 콜백 함수 내부의 this를 외부 함수 내부의 this와 일치시켜야 한다. 이때 bind 메서드를 사용하면 this를 일치시킬 수 있다.

```javascript
const person = {
    name: 'Lee',
    foo(callback) {
        // bind 메서드로 callback 함수 내부의 this 바인딩은 전달
        setTimeout(callback.bind(this), 100);
    }
};

person.foo(function() {
    console.log(`Hi! my name is ${this.name}.`); // Hi! my name is Lee.
})
```

지금까지 함수 호출 방식에 따라 this 바인딩이 동적으로 결정되는 것에 대해 살펴보았다. 이를 정리해 보면 다음과 같다.

|함수 호출 방식|this 바인딩|
|---------|---------|
|일반 함수 호출|전역 객체|
|메서드 호출|메서드를 호출한 객체|
|생성자 함수 호출|생성자 함수가 (미래에) 생성할 인스턴스|
|Function.prototype.apply/call/bind 메서드에 의한 간접 호출|Function.prototype.apply/call/bind 메서드에 첫 번째 인수로 전달한 객체|
