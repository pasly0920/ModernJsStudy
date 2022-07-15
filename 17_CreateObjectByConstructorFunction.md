# 17장 생성자 함수에 의한 객체 생성

---

객체는 10장의 객체 리터럴 이외의 다양한 방법으로 생성할 수 있다. 다양한 객체 생성 방식 중에서 생성자 함수를 사용하여 객체를 생성하는 방식을 살펴보자.

## 17.1 Object 생성자 함수

---

new 연산자와 함께 Object 생성자 함수를 호출하면 빈 객체를 생성하여 반환한다. 빈 객체를 생성한 이후 프로퍼티 또는 메서드를 추가하여 객체를 완성할 수 있다.

생성자 함수란 new 연산자와 함께 호출하여 객체(인스턴스)를 생성하는 함수이다. 생성자 함수에 의해 생성된 객체를 인스턴스라 한다.

```javascript
const person = new Object();  //빈 객체의 생성

person.name = "Lee";  //프로퍼티 추가
person.sayHello = function(){
    consloe.log('Hi! My name is' + this.name);
}  // this는 메소드를 호출한 객체가 저장되어 있는 속성

console.log(person);  // {name; "Lee", sayHello: f}
person.sayHello();  // Hi! My name is Lee
```

Object 생성자 함수 이외에도 String, Number, Boolean, Function, Array, Date, RegExp, Promise 등의 빌트인 생성자 함수가 있다.

```javascript
const strObj = new String('Lee');
console.log(typeof strObj);  //object
console.log(strObj);  //String {"Lee"}

const numObj = new Number(123);
console.log(typeof numObj);  //object
conselo.log(numObj);  //Number {123}

const arr = new Array(1,2,3);  //Array 생성자 함수에 의한 Array 객체(배열) 생성
console.log(typeof arr);  //object
conselo.log(arr);  //[1,2,3]
```

## 17.2 생성자 함수

---

### 17.2.1 객체 리터럴에 의한 객체 생성 방식의 문제점

객체 리터럴에 의한 객체 생성 방식은 단 하나의 객체만 생성한다. 따라서 동일한 프로퍼티를 갖는 객체를 여러 개 생성해야 하는 경우 매번 같은 프로퍼티를 기술해야 하기 때문에 비효율적이다.

객체는 프로퍼티를 통해 객체 고유의 상태를 표현한다. 그리고 메서드를 통해 상태 데이터인 프로퍼티를 참조하고 조작하는 동작을 표현한다. 따라서 프로퍼티는 객체마다 프로퍼티 값이 다를 수 있지만 메서드는 내용이 동일한 경우가 일반적이다. 따라서 객체 리터럴의 경우 프로퍼티 구조가 동일함에도 불구하고 매번 같은 프로퍼티와 메서드를 기술해야 한다.

### 17.2.2 생성자 함수에 의한 객체 생성 방식의 장점

생성자 함수에 의한 객체 생성 방식은 마치 객체(인스턴스)를 생성하기 위한 템플릿(클래스)처럼 생성자 함수를 사용하여 프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성할 수 있다.

```javascript
function Circle(radius) {  //생성자 함수
    this.radius = radius;  //생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
    this.getDiameter = function(){
        return 2* this.radius;
    };
}

const circle1 = new Circle(5)  //반지름이 5인 Circle 객체를 생성
const circle2 = new Circle(10)  //반지름이 10인 Circle 객체를 생성

console.log(circle1.getDiameter());  //10
console.log(circle2.getDiameter());  //20
```

*this

this는 객체 자신의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수다. this가 가리키는 값, 즉 this바인딩은 함수 호츌 방식에 따라 동적으로 결정된다.

|함수 호출 방식|this가 가리키는 값(this바인딩)|
|---|---|
|일반 함수로서 호출|전역 객체|
|메서드로서 호출|메서드를 호출한 객체(마침표 앞의 객체)|
|생성자 함수로서 호츨|생성자 함수가 (미래에) 생성할 인스턴스|

```javascript
function foo() {
    console.log(this);
}  // 함수는 다양한 방식으로 호출될 수 있다.

foo();  // 전역 객체는 브라우저 환경에서는 window, Noje.js 환경에서는 global을 가리킨다.

const obj = { foo };  // ES6 프로퍼티 축약 표현

obj.foo();  // obj, 메서드로서 호출

const inst = new foo();  // inst, 생성자 함수로서 호출
```

생성자 함수는 이름 그대로 객체를 생성하는 함수다. 자바스크립트의 생성자 함수는 자바와 같은 클래스 기반 객체지향 언어의 생성자와는 다르게 그 형식이 정해져 있는 것이 아니라 일반 함수와 동일한 방법으로 생성자 함수를 정의하고 new 연산자와 함께 호출하면 해당함수는 생성자 함수로 동작한다. 만약 new 연산자와 함께 생성자 함수를 호출하지 않으면 생성자 함수가 아니라 일반 함수로 동작한다.

```javascript
const circle3 = Circle(15);  // new 연사자와 함께 호출하지 않아 일반 함수로서 호출된다.

conselo.log(circle3); // undefined, 일반 함수로서 호출된 Circle은 반환문이 없으므로 암묵적으로 undefined를 반환한다.

console.log(radius);  // 15, 일반 함수로서 호출된 Circle 내의 this는 전역 객체를 가리킨다.
```

### 17.2.3 생성자 함수의 인스턴스 생성 과정

생성자 함수의 역할은 프로퍼티 구조가 동일한 인스턴스를 생성하기 위한 템플릿(클래스)으로서 동작하여 인스턴스를 생성하는 것과 생성된 인스턴스를 초기화(인스턴스 프로퍼티 추가 및 초기값 할당)하는 것이다. 생성자 함수가 인스턴스를 생성하는 것은 필수이고, 생성된 인스턴스를 초기화하는 것은 옵션이다.

```javascript
function Circle(radius) {  // 생성자 함수
    // 인스턴스 초기화
    this.radius = radius;
    this.getDiameter = function() {
        return 2 * this.radius;
    };
}

const circle1 = new Circle(5); // 반지름이 5인 Circle 객체를 생성
```

this에 프로퍼티를 추가하고 필요에 따라 전달된 인수를 프로퍼티의 초기값으로서 할당하여 인스턴스를 초기화한다. JS는 new 연산자와 함께 생성자 함수를 호출하면 암묵적인 처리를 통해 인스턴스를 생성하고 반환한다.

1. 인스턴스 생성과 this 바인딩 : 암묵적으로 빈 객체가 생성되고 이것이 바로 인스턴스고 this에 바인딩된다. 이 과정은 런타임 이전에 실행된다.  
*바인딩 : 식별자와 값을 연결하는 과정을 의미한다. this 바인딩은 this와 this를 가리킬 객체를 바인딩하는 것이다.

2. 인스턴스 초기화 : 런타임 과정에서 this에 바인딩되어 있는 인스턴스를 초기화한다. this에 바인딩되어 있는 인스턴스에 프로퍼티나 메서드를 추가하고 생성자 함수가 인수로 전달받은 초기값을 인스턴스 프로퍼티에 할당하여 초기화하거나 고정값을 할당한다.

3. 인스턴스 반환 : 생성자 함수 내부의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.

```javascript
function Circle(radius) {
    // 1. 암묵적으로 빈 객체가 생성되고 this에 바인딩된다.

    // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
    this.radius = radius;
    this.getDiameter = function() {
        return 2 * this.radius;
    };

    // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
}

// 인스턴스 생성. Circle 생성자 함수는 암묵적으로 this를 반환한다.
const circle1 = new Circle(1); // 반지름이 1인 Circle 객체를 생성
conselo.log(circle);  // Circle {radius: 1, getDiameter: f}
```

만약 this가 아닌 다른 객체를 명시적으로 반환하면 this가 반환되지 못하고 return 문에 명시한 객체가 반환된다.

명시적으로 원시 값을 반환하면 원시 값 반환은 무시되고 암묵적으로 this가 반환된다.

명시적으로 this가 아닌 다른 값을 반환하는 것은 생성자 함수의 기본 동작을 훼손한다. 따라서 생성자 함수 내부에서 return 문을 반드시 생략해야 한다.

### 17.2.4 내부 메서드 [[Call]]과 [[Construct]]

함수 선언문 또는 함수 표현식으로 정의한 함수는 일반적인 함수를 호출할 수도 있지만 생성자 함수로서 호출할 수도 있다. 생성자 함수로서 호출한다는 것은 new 연산자와 함께 호출하여 객체를 생성하는 것을 의미한다.

함수는 객체이고 일반 객체가 가지고 있는 내부 슬롯과 내부 메서드를 모두 가지고 있기 때문에 일반 객체와 동일하게 동작한다.

하지만 일반 객체와 다른점은 함수는 호출할 수 있다는 것이다. 따라서 함수 객체는 함수로서 동작하기 위해 함수 객체만을 위한 [[Environment]], [[FormalParameters]] 등의 내부 슬롯과 [[Call]],[[Construct]] 같은 내부 메서드를 추가로 가지고 있다.

함수가 일반 함수로서 호출되면 함수 객체의 내부 메서드 [[Call]]이 호출되고 new 연산자와 함께 생성자 함수로서 호출되면 내부 메서드 [[Construct]]가 호출된다.

내부 메서드 [[Call]]을 갖는 함수 객체를 callable이라 하고 callable은 호출할 수 잇는 객체, 즉 함수를 말한다. 내부 메서드 [[Construct]]를 갖는 함수 객체를 constructor이라 하고, constructor은 생성자 함수로서 호출할 수 있는 함수이다.

모든 함수 객체는 호출할 수 있으므로 내부 메서드 [[Call]]을 갖고 있지만 [[Construct]]은 가질 수도 있고 아닐 수도 있다.

### 17.2.5 constructor와 non-constructor의 구분

자바스크립트 엔진은 함수 정의를 평가하여 함수 객체를 생성할 때 함수 정의 방식에 따라 함수를 constructor와 non-consturctor로 구분한다.

- constructor : 함수 선언문, 함수 표현식, 클래스(클래스도 함수)
- non-constructor : 메서드, 화살표 함수

함수를 프로퍼티 값으로 사용하면 일반적으로 메서드로 통칭한다. 하지만 ES6의 메서드 축약 표현만 메서드로 인정한다. 함수가 어디에 할당되어 있는지에 따라 메서드인지를 판단하는 것이 아니라 함수 정의 방식에 따라 constructor와 non-constructor를 구분한다.

함수를 생성자 함수로서 호출하면 내부 메서드 [[Construct]]가 호출된다. non-constructor인 함수 객체는 내부 메서드 [[Construct]]를 갖지 않는다. 따라서 non-constructor인 함수 객체를 생성자 함수로서 호출하면 에러가 발생한다.

```javascript
function foo() {}  // 일반 함수 정의 : 함수 선언문, 함수 표현식 (constructor)
    const bar = function () {};
    const baz = {
        x: function () {}
    };  // 프로퍼티 x의 값으로 할당된 것은 일반 함수로 정의된 함수다. 이는 메서드로 인정하지 않는다.

new foo();  // -> foo {}
new bar();  // -> bar {}
new baz.x();  // -> x {}

const arrow = () => {} ;  // 화살표 함수 (non-constructor)

new arrow();  // TypeError: arrow is not a consturctor

const obj = {
    x() {}
};  // 메서드 축약 표현만 메서드로 인정

new obj.x();  // TypeError: obj.x is not a constructor
```

### 17.2.6 new 연산자

new 연산자와 함께 함수를 호출하면 해당 함수는 생성자 함수로 동작한다. 즉 함수 객체의 내부 메서드 [[Call]]이 호출되는 것이 아니라 [[Construct]]가 호출된다. 이때 new 연산자와 함께 호출하는 함수는 non-constructor가 아닌 constructordldjdi gksek.

```javascript
function add(x, y) {
    return x+y;
}

let inst = new add();  // 생성자 함수로서 정의하지 않은 일반 함수를 new 연산자와 함께 호출

console.log(inst);  //{}, 함수가 객체를 반환하지 않았으므로 반환문이 무시된다. 따라서 빈 객체가 생성되어 반환한다.

function createUser(name, role) {
    return { name, role };
}  // 객체를 반환하는 일반 함수

inst = new createUser('Lee', 'sehee');  // 일반 함수를 new 연산자와 함께 호출
console.log(inst);  // { name: "Lee", role: "admin"}  // 함수가 생성한 객체를 반환한다.
```

```javascript
function Circle(radius) {
    this.radius = radius;
    this.getDiameter = function() {
        return 2 * this.radius;
    };
}

const circle = Circle(5); // 일반 함수로서 호출
console.log(circle);  // undefined

console.log(radius);  // 5, 일반 함수 내부의 this는 전역 객체 window를 가리킨다.
console.log(getDiameter());  // 10

circle.getDiameter();  // TypeError: Cannot read property 'getDiameter' of undefined
```

일반 함수로 호출되어 radius 프로퍼티와 getDiameter 메서드는 전역 객체의 프로퍼티와 메서드가 된다.

일반 함수와 생성자 함수에 특별한 형식적 차이가 없으므로 생성자 함수는 첫 문자를 대문자로 기술하는 파스칼 케이스로 명명하여 일반 함수와 구별할 수 있도록 노력한다.

### 17.2.7 new.target

생성자 함수가 new 연산자 없이 호출되는 것의 위험성을 회피하기 위해서 ES6에서는 new.target을 지원한다.

new.target은 this와 유사하게 constructor인 모든 함수 내부에서 암묵적인 지역 변수와 같이 사용되며 메타 프로퍼티라고 부른다.

new 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 new.target은 함수 자신을 가리킨다. new 연산자 없이 일반 함수로서 호출된 함수 내부의 new.target은 undefined다.

```javascript
function Circle(radius) {
    //이 함수가 new 연산자와 함께 호출되지 않는다면 new.target은 undefined이다.
    if (!new.target){
        // new 연산자와 함께 생tjd자 함수를 재귀 호출하여 생성된 인스턴스를 반환하다.
        return new Circle(radius);
    }

    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    }
}

const circle = Circle(5);  //new 연산자 없이 생성자 함수를 호출하여도 new.target을 통해 생성자 함수로서 호출된다.
console.log(circle.getDiameter());
```

대부분의 빌트인 생성자 함수 (Object, String, Number, Boolean, Function, Array, Date, RegExp,
Promise 등)는 new 연산자와 함께 호출되었는지를 확인한 후 적절한 값을 반환한다.

Object와 Fuction 생성자 함수는 new 연산자 없이 호출해도 new 연산자와 함께 호출했을 때와 동일하게 동작한다.

하지만 String, Number, Boolean 생성자 함수는 new 연산자와 함께 호출했을 때만 객체를 생성하고 반환하지만 new 연산자 없이 호출하면 문자열, 숫자, 불리언 값을 반환하다. 이를 통해 데이터 타입을 변환하기도 한다.

```javascript
let obj = new Object();
console.log(obj);  // {}

obj = object();
console.log(obj);  // {}


const str = String(123);
console.log(str, typeof str); // 123 string
```
