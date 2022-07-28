# 25 클래스

## 25.1 클래는 프로토타입의 문법적 설탕인가?

- 프로그래밍: 절차지향 프로그래밍, 객체지향 프로그래밍
- 객체지향 프로그래밍: 클래스 기반 객체지향 프로그래밍, 프로토타입 기반 객체지향 프로그래밍(자바스크립트)
- "클래스와 프로토타입은 모두 새로운 객체를 생성하는 방식이다."
- 생성자 함수와 프로토타입을 통해 상속을 구현할 수 있으므로, 프로토타입 기반 객체지향 언어인 자바스크립트는 클래스가 필요 없다.

```js
// ES5 생성자 함수
var Person = (function () {
    // 생성자 함수
    function Person(name) {
        this.name = name;
    }

    // 프로토타입 메서드
    Person.prototype.sayHi = function () {
        console.log('Hi! My name is ' + this.name);
    };

    // 생성자 함수 반환
    return Person;
}());

// 인스턴스(객체) 생성
var me = new Person('Lee');
me.sayHi(); // Hi! My name is Lee
```

- 자바스크립트는 ES6부터 클래스 기반 객체지향 프로그래밍에 익숙한 프로그래머를 위해 클래스를 도입하였다.
- 생성자 함수와 클래스는 프로토타입 기반의 객체지향을 구현했다는 점에서 유사하지만 클래스 생성자 함수 기반의 객체 생성 방식보다 견고하고 명료하다.
- 클래스와 생성자 함수의 차이

|클래스|생성자 함수|
|:---:|:---:|
|클래스를 new 연산자 없이 호출하면 에러가 발생한다.|생성자 함수를 new 연산자 없이 호출하면 일반 함수로서 호출된다.|
|클래스는 상속을 지원하는 extends와 super 키워드를 제공한다.|생성자 함수는 extends와 super 키워드를 지원하지 않는다.|
|클래스는 호이스팅이 발생하지 않는 것처럼 동작한다.|함수 선언문으로 정의된 생성자 함수는 함수 호이스팅이, 함수 표현식으로 정의한 생성자 함수는 변수 호이스팅이 발생한다.|
|클래스 내의 모든 코드에는 암묵적으로 strict mode가 지정되어 실행되며 strict mode를 해제할 수 없다.|생성자 함수는 암묵적으로 strict mode가 지정되지 않는다.|
|클래스의 constrcutor.프로토타입 메서드, 정적 메서드는 모두 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 false다. 즉, 열거되지 않는다.||

## 25.2 클래스의 정의

- 클래스 정의: class 키워드를 사용한다.
- 클래스의 이름 설정: 생성자 함수와 마찬가지로 일반적오로 파스칼 케이스(```var PascalCase```)를 사용한다.

```js
// 클래스 선언문(일반적으로 클래스를 정의하는 방식)
class Person {}

// 익명 클래스 표현식(표현식으로 클래스를 정의하는 방식)
const Person = class {}

// 기명 클래스 표현식
const Person = class MyClass {}
```

- 일급 객체로서 클래스가 갖는 네 가지 특징(값처럼 사용될 수 있다.)

1. 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다.
2. 변수나 자료구조(객체, 배열 등)에 저장할 수 있다.
3. 함수의 매개변수에 전달할 수 있다.
4. 함수의 반환값으로 사용될 수 있다.

- 클래스 몸체에 정의될 수 있는 세 가지 메서드

1. constructor(생성자)
2. 프로포타입 메서드
3. 정적 메서드

```js
// 클래스 선언문
class Person {
    // constructor 생성자
    constructor(name) {
        // 인스턴스 생성 및 초기화
        this.name = name;
    }

    // 프로포타입 메서드
    sayHi() {
        console.log(`Hi! My name is ${this.name}`);
    }

    // 정적 메서드
    static sayHello() {
        console.log('Hello!');
    }
}

// 안스턴스 생성
const me = new Person('Lee');

// 인스턴스 프로퍼티 참조
console.log(me.name);

// 프로토타입 메서드 호출
me.sayHi();

// 정적 메서드 호출
Person.sayHello();
```

- 클래스와 생성자 함수의 정의 방식은 비슷하다.

## 25.3 클래스 호이스팅

- 클래스는 함수로 평가된다.
- 클래스 선언문으로 정의한 클래스는 런타임(소스코드 평가 과정) 이전에 먼저 평가되어 객체를 생성한다.
- 그 객체는 생성자 함수로서 호출할 수 있는 함수 constructor다.

```js
// 클래스 선언문
class Person {}

console.log(typeof Person); // function
```

- <주의>클래스는 클래스 정의 이전에 참조할 수 없다.

```js
console.log(Person) // ReferenceError: Cannot access 'Person' before initialization

// 클래스 선언문 
class Person {}
```

- 클래스는 let, const 키워드로 선언한 변수처럼 호이스팅되기 때문에 클래스 선언문 이전에 일시적 사각지대에 빠지게 된다.
- 클래스 선언문은 호이스팅(런타임 이전에 먼저 실행되는 것)이 발생하지 않는 것처럼 보이나 그렇지 않다.

```js
const Person = '';

{
    // 호이스팅이 발쟁하지 않는다면 ''이 출력되어야 한다.
    console.log(Person); // ReferenceError: Cannot access 'Person' before initialization
    
    // 클래스 선언문
    class Person {}
}
```

## 25.4 인스턴스 생성

- 일반 함수: new 연산자 없이 호출된다.
- 생성자 함수: new 연산자와 함께 호출되어 인스턴스를 생성한다.
- 클래스: 클래스는 생성자 함수이므로 반드시 new 연산자와 함께 호출되어 인스턴스를 생성한다.

```js
class Person {}

// 클래스의 인스턴스1: 일반적으로 정의된 클래스
const me = new Person();
console.log(me); // Person {}

// new 연산자 없이 클래스를 호출하면 타입 에러가 발생한다.
const me = Person(); // TypeError: Class constructor Person cannot be invoked without 'new'
```

- 클래스 표현식으로 정의된 기명 클래스의 경우, 클래스를 가리키는 식별자를 가지고 인스턴스를 생성해야 한다.
- 이 경우, 클래스 표현식에서 사용한 클래스 이름: 클래스 내부에서만 유효한 식별장다 -> 외부 코드에서 접근 불가능하다 -> 기명 클래스 푤현식의 클래스 이름을 사용해 인스턴스를 생성하려고 하면 안 안된다.
- 이러한 방식 기명 함수 표현식과 마찬가지다.

```js
const Person = class MyClass {};

// 클래스의 인스턴스2: 표현식으로 정의된 기명 클래스
const me = new Person();

console.log(MyClass); // ReferenceError: MyClass is not defined

const you = new MyClass(); // ReferenceError: MyClass is not defined
```

## 25.5 메서드

- 클래스 몸체에는 0개 이상의 메서드만 선언할 수 있다.
- 클래스의 몸체에서 정의할 수 있는 메서드: constructor(생성자), 프로토타입 메서드, 정적 메서드

### 25.5.1 constructor

- 메서드 constructor: 인스턴스를 생성하고 초기화
- constructor는 이름을 변경할 수 없다.

```js
class Person {
    // 생성자
    constrcutor(name) {
        // 인스턴스 생성 및 초기화
        this.mame = name;
    }
}

// 클래스는 함수다.
console.log(typeof Person); //fuction
console.dir(Person)
```

### 25.5.2 프로토타입 메서드

- 생성자 함수를 사용하여 인스턴스를 생성하는 경우, 프로토타입 메서드를 생성하기 위해서는 명시적으로 프로토타입에 매서드를 추가해야 한다.

```js
// 생성자 함수
function Person(name) {
    this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHi = function () {
    console.log(`Hi! My name is ${this.name}`);
};

const me = new Person('Lee');
me.sayHi(); // Hi! My name is Lee
```

- 클래스 몸체에서 정의한 메서드는 자동으로 프로토타입 메서드가 된다.

```js
class Person {
    // 생성자
    constructor(name) {
        // 인스턴스 생성 및 초기화
        this.name = name;
    }

    // 프로토타입 메서드
    sayHi() {
        console.log(`Hi! My name is ${this.name}`);
    }
}

const me = new Person('Lee');
me.sayHi() // Hi! My name is Lee
```

### 25.5.3 정적 메서드

- 정적 메서드: 인스턴스를 생성하지 않아도 호출할 수 있는 메서드
- 클래스에 정적 메서드(클래스 메서드)를 생성하는 방법: static 키워드

```js
class Person {
    // 생성자
    constructor(name) {
        // 인스턴스 생성 및 초기화
        this.name = name;
    }

    // 정적 메서드
    static sayHi() {
        console.log('Hi!');
    }
}

// <비교>생성자 함수에 정적 메서드를 생성하는 방법: 생성자 함수에 정적 메서드 추가
// 생성자 함수
function Person(name) {
    this.name = name;
}

// 정적 메서드 추가
Person.sayHi = function () {
    console.log('Hi!');
};

// 정적 메서드 호출
Person.sayHi();
```

### 25.5.4 정적 메서드와 프로토타입 메서드의 차이

1. 정적 메서드와 프로토타입 메서드는 자신이 속해 있는 프로토타입 체인이 다르다.
2. 정적 메서드는 클래스로 호출하지만, 프로토타입 메서드는 인스턴스로 호출한다.
3. 정적 메서드는 인스턴스 프로퍼티를 참조할 수 없지만, 프로토타입 메서드는 인스턴스 프로퍼티를 참조할 수 있다.

```js
class Square {
    // 정적 메서드
    static area(width, height) {
        return width * height;
    }
}

console.log(Square.area(10, 10)); // 100
```

```js
class Square {
    constructor(width, height) {
        this.width = width;
        this.height = height;
    }

    // 프로토타입 메서드
    area() {
        return this.width * this.height;
    }
}

const square = new Square(10, 10);
console.log(square.area()); // 100
```

### 25.5.5 클래스에서 정의한 메서드의 특징

1. function 키워드를 생략한 메서드 축약 표현을 사용한다.
2. 객체 리터럴과는 다르게 클래스에 메서드를 정의할 때는 콤마가 필요 없다.
3. 암묵적으로 strict mode로 실행된다.
4. for...in문이나 Object.keys 메서드 등으로 열거할 수 없다. 즉, 프로퍼티의 열거 가능 여부를 나타내며, 불리언 값을 갖는 프로퍼티 어트리뷰트 [[Environment]]를 갖지 않는 non-constructor다. 따라서 new 연산자와 함께 호출할 수 없다.

## 25.6 클래스의 인스턴스 생성 과정

- 클래스는 생성자 함수이므로 반드시 new 연산자와 함께 호출되어 인스턴스를 생성한다.
- 클래스가 new 연산자와 함께 호출되면 클래스의 내부 메서드 [[construct]]가 호출된다.

1. 인스턴스 생성과 this 바인딩
2. 인스턴스 초기화
3. 인스턴스 반환

```js
class Person {
    // 생성자
    constructor(name) {
        // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩 된다.
        console.log(this); // Person {}
        console.log(Object.getPrototype(this) === Person.prototype); // true

        // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
        this.name = name

        // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
    }
}
```

## 25.7 프로퍼티

### 25.7.1 인스턴스 프로퍼티

- 인스턴스 프로퍼티는 constructor 내부에서 정의해야 한다.

```js
class Person {
    constructor(name) {
        // 인스턴스 프로퍼티
        this.name = name;
    }
}

const me = new Person('Lee')
console.log(me); // Person { name: 'Lee' }
```

- constructor 내부 코드가 실행되기 이전에 constructor 내부의 this에는 이미 클래스가 암묵적으로 생성한 인스턴스인 빈 객체가 바인딩되어 있다.
- constructor 내부에서 this에 인스턴스 프로퍼티를 추가한다.
- 클래스가 암묵적으로 생성한 빈 객체, 즉 인스턴스에 프로퍼티가 추가되어 인스턴스가 초기화된다.

```js
class Person {
    constructor(name) {
        // 인스턴스 프로퍼티
        this.name = name;
    }
}

const me = new Person('Lee');

// name은 public하다.
console.log(me.name) // Lee
```

- constructor 내부에서 this에 추가한 프로퍼티는 언제나 클래스가 생성한 인스턴스의 프로퍼티가 된다.

### 25.7.2 접근자 프로퍼티

- 접근자 프로퍼티: 자체적으로 값([[Value]] 내부 슬롯)을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수로 구성된 프로퍼티

```js
const person = {
    // 데이터 프로퍼티
    firstName: 'Ungmo',
    lastName: 'Lee',

    // fullName은 접근자 함수로 구성된 접근자 프로퍼티다.

    // getter 함수 
    get fullName() {
        return `${this.firstName} ${this.lastName}`;
    },

    // setter 함수
    set fullName(name) {
        // 배열 디스트럭처링 할당
        [this.firstName, this.lastName] = name.split(' ');
    }
};

// 데이터 프로퍼티를 통한 프로퍼티 값의 참조
console.log(`${person.firstName} ${person.lastName}`); // Ungmo Lee

// 접근자 프로퍼티를 통한 프로퍼티 값의 저장
// 접근자 프로퍼티 fullName에 값을 저장하면 setter 람수가 호출된다.
person.fullName = 'Heegun Lee';
console.log(person); // { firstName: 'Heegun', lastName: 'Lee', fullName: [Getter/Setter] }

// 접근자 프로퍼티를 통한 프로퍼티 값의 저장
// 접근자 프로퍼티 fullName에 값을 저장하면 setter 함수가 호출된다.
console.log(person.fullName); // Heegun Lee

//fullName은 접근자 프로퍼티다.
// 접근자 프로퍼티는 get, set, enumerable, configurable 프로퍼티 어트리뷰트를 갖는다.
console.log(Object.getOwnPropertyDescriptor(person, 'fullName')); // { get: [Function: get fullName]set: [Function: set fullName], enumerable: true, configurable: true }
```

### 25.7.3 클래스 필드 정의 제안

- 직접 읽어보시기 바랍니다.

### 25.7.4 private 필드 정의 제안

- 직접 읽어보시기 바랍니다.

### 25.7.5 static 필드 정의 제안

- 직접 읽어보시기 바랍니다.

## 25.8 상속에 의한 클래스 확장

### 25.8.1 클래스 상속과 생성자 함수 상속

- 상속에 의한 클래스 확장: 기존 클래스를 상속받아 새로운 클래스를 확장하여 정의하는 것
- <비교>프로토타입 기반 상속: 프로토타입 체인을 통해 다른 객체의 자산을 상속받는 것
- 클래스와 생성자 함수는 모두 인스턴스를 생성하는 함수라는 점에서 공통적이지만 클래스는 상속을 통해 기존 클래스를 확장할 수 있는 문법이 기본적으로 제공되지만 생성자 함수는 그렇지 않다.

```js
class Animal {
    constructor(age, weight) {
        this.age = age;
        this.weight = weight;
    }

    eat() { return 'eat'; }

    move() { return 'move'; }
}

// 상속을 통해 Animal 클래스를 확장한 Bird 클래스
class Bird extends Animal {
    fly() { return 'fly'; }
}

const bird = new Bird(1, 5);

console.log(bird); // Bird { age: 1, weight: 5 }
console.log(bird instanceof Bird); // true
console.log(bird instanceof Animal); // true

console.log(bird.eat()); // eat
console.log(bird.move()); // move
console.log(bird.fly()); // fly
```

### 25.8.2 extends 키워드

- 상속을 통해 클래스를 확장하는 방법: extends 키워드를 사용하여 상속받을 클래스를 정의
- 서브클래스(또는 파생 클래스 또는 자식 클래스): 상속을 통해 확장된 클래스
- 수퍼클래스(또는 베이스 클래스 또는 부모 클래스): 상속된 클래스

```js
// 수퍼(베이스/부모)클래스
class Base {}

// 서브(파생/자식)클래스
class Derived extends Base {}
```

### 25.8.3 동적 상속

- extends 키워드 앞에 오는 것: 클래스
- extends 키워드 뒤에 오는 것 -> 동적으로 상속받을 대상

1. 클래스
2. 생성자 함수
3. [[construct]] 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식

```js
// 생성자 함수
function Base(a) {
    this.a = a;
}

// 생성자 함수를 상속받는 서브클래스
class Derived extends Base {}

const derived = new Derived(1);
console.log(derived) // Derived { a: 1 }
```

```js
function Base1() {}

class Base2 {}

let condition = true;

// 조건에 따라 동적으로 상속 대상을 결정하는 서브클래스
class Derived extends (condition ? Base1 : Base2) {}

const derived = new Derived();
console.log(derived); // Derived {}

console.log(derived instanceof Base1); // true
console.log(derived instanceof Base2); // false
```

### 25.8.4 서브클래스의 constructor

- 직접 읽어보시기 바랍니다.

### 25.8.5 super 키워드

- 직접 읽어보시기 바랍니다.

### 25.8.6 상속 클래스의 인스턴스 생성 과정

- 직접 읽어보시기 바랍니다.

### 25.8.7 표준 빌트인 생성자 함수 확장

- 직접 읽어보시기 바랍니다.
