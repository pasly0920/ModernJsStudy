#10장. 객체 리터럴
프로퍼티 -> 객체(프로퍼티의 집합) -> 객체 리터럴


##10.3 프로퍼티
프로퍼티는 키와 값으로 구성된다.
프로퍼티 키: 빈 문자열을 포함하는 모든 문자열 또는 심벌 값(일반적으로 문자열을 사용한다.)
프로퍼티 값: 자바스크립트에서 사용할 수 있는 모든 값

```js
var person = {
  name: 'Lee', //프로퍼티 키는 name, 프로퍼티 값은 'one'
  age: 22 // 프로퍼티 키는 age, 프로퍼티 값은 22
};
```

프로퍼티 키를 설정할 때 고려해야 할 사항
1. 프로퍼티 키는 프로퍼티 값에 접근할 수 있는 이름으로서 식별자의 역할을 한다.
2. 프르퍼티 키 네이밍
식별자 네이밍 규칙을 따르지 않는 프로퍼티 키: 따옴표로 감싸야 한다.
식별자 네이밍 규칙을 따르는 프로퍼티 키: 따옴표를 생략할 수 있다.

```javascript
var person = {
  firstName: 'Ung-mo',
  'last-name': 'Lee'
};
console.log(person); // {firstName: "Ung-me", last-name: "Lee"}
```


여기서 식별자 네이밍 규칙은 다음의 세 가지를 말한다.
(1) 식별자는 특수문자를 제외한 문자, 숫자, 언더스코어(_), 달러 기호를 포함할 수 있다.
(2) 식별자는 숫자로 시작할 수 없다.
(3) 예약어는 식별자로 사용될 수 없다. 
3. 프로퍼티 키 생성
프로퍼티 키로 사용할 표현식을 대괄호로 묶어 프로퍼티 키를 동적으로 생성할 수 있다.

```js
var obj = {};
var key = 'Hello';
obj[key] = 'world';
console.log(obj); // {hello: "world"}
```


##10.4 메서드
메서드: 프로퍼티 값으로 사용된 함수(일반 함수와 구분하기 위해)

```js
var circle = {
  radius: 5,

  getDiameter: function() {
    return 2 * this.radius; //this 키워드는 객체 자신을 가리키는 참조변수이다.(circle)
  }
};
console.log(circle.getDiameter()); //10
```


##10.5 프로퍼티 접근
프로퍼티에 접근하는 2가지 방법
마침표 표기법: 마침표 프로퍼티 접근 연산자(.)를 사용
대괄호 표기법: 대괄호 프로퍼티 접근 연산자([ … ])를 사용

식별자 네이밍 규칙을 따르는 프로퍼티 키는 마침표 표기법과 대괄호 표기법을 모두 사용할 수 있다.
식별자 네이밍 규칙을 따르지 않는 프로퍼티 키는 대괄호 표기법만 사용할 수 있다.
마침표 프로퍼티 접근 연산자 또는 대괄호 프로퍼티 접근 연산자의 좌측: 객체로 평가되는 표현식
마침표 프로퍼티 접근 연산자의 우측 또는 대괄호 프로퍼티 접근 연산자의 내부: 피로퍼티 키

```js
var person = {
  name: 'Lee'
};
// 마침표 표기법에 의한 프로퍼티 접근
console.log(person.name); // Lee 
// 대괄호 표기법에 의한 프로퍼티 접근
console.log(person['name']); // Lee
```


##10.6 프로퍼티 값 갱신
이미 존재하는 프로퍼티에 새로운 값을 할당하면 프로퍼티 값은 갱신된다.

```js
var person = {
  name: 'Lee'
};
// person 객체에 name 프로퍼티가 존재하므로 name 프로퍼티의 값이 갱신된다.
person.name ="Kim";
console.log(person); //{name: "Kim"}
```

##10.7 프로퍼티 동적 생성
존재하지 않는 프로퍼티에 값을 할당하면 프로퍼티가 동적으로 생성되어 추가되고 프로퍼티 값이 할당된다.

```js
var person = {
  name: 'Lee'
};
// person 객체에는 age 프로퍼티가 존재하지 않는다. 따라서 person 객체에 age 프로퍼티가 동적으로 생성되고 값이 할당된다.
person.age = 20;
console.log(person); // {name: "Lee", age: 20}
```


##10.8 프로퍼티 삭제
delete 연산자는 객체의 프로퍼티를 삭제한다.

```js
var person = {
  naem: 'Lee'
};
person.age = 20; // 프로퍼티 동적 생성
delete person.age;
delete person.address;
console.log(person); // {name: "Lee"}
```

---
##10.1 객체란?
객체: 6가지 원시값(숫자, 문자열, 불리언, undefined, null, 심벌)을 제외한 모든 값(함수, 배열, 정규 표현식)
원시 타입: 단 하나의 값, 변경 불가능한 값
객체 타입: 다양한 타입의 값(원시 값 도는 다른 객체)을 하나의 단위로 구성한 복합적인 자료구조, 변경 가능한 값

객체는 프로퍼티와 메서드로 구성된 집합체로 상태와 동작을 하나의 단위로 구조화할 수 있다.
프로퍼티: 객체의 상태를 나타내는 값
메서드: 프로퍼티(상태 데이터)를 참조하고 조작할 수 있는 동작


##10.2 객체 리터럴에 의한 객체 생성
인스턴스: 클래스에 의해 생성되어 메모리에 저장된 실체

프로토타입 기반 객체 지향 언어인 자바스크립트의 객체 생성 방법 5가지
객체 리터럴
Object 생성자 함수
생성자 함수
Object.create 메서드
클래스(ES6)
리터럴: 사람이 이해할 수 있는 문자 또는 약속된 기호를 사용하여 값을 생성하는 표기법
객체 리터럴: 객체를 생성하기 위한 표기법

객체 리터럴은 중괄호 내에 0개 이상의 프로퍼티를 정의한다.

```js
var person = {
  name: 'Lee',
  sayHello: function() {
    console.log(`Hello! My name is ${this.name}.`);
  }
};
console.log(typeof person);
console.log(person); // {name: "Lee", sayHello: f}
```

--
##10.9 ES6에서 추가된 객체 리터럴의 확장 기능
###10.9.1 프로퍼티 축약 표현
ES6에서는 프로퍼티 값으로 변수를 사용하는 경우 변수 이름과 프로퍼티 키가 동일한 이름일 때 프로퍼티 키를 생략할 수 있다. 이때 프로퍼티 키는 변수 이름으로 자동 생성된다.

```js
let x = 1, y = 2;

const obj = {x, y}; //프로퍼티 축약 표현

console.log(obj); // {x:1, y: 2}
```

###10.9.2 계산된 프로퍼티 이름
ES5에서 계산된 프로퍼티 이름으로 프로퍼티 키를 동적 생성하려면 객체 리터럴 외부에서 대괄호 표기법을 사용해야 했지만, ES6에서는 문자열 또는 문자열로 타입 변환할 수 있는 값으로 평가되는 표현식을 사용해 프로퍼티 키를 동적으로 생성할 수 있다. 단, 프로퍼티 키로 사용할 표현식을 대괄호로 묶어야 한다. 이를 계산된 프로퍼티 이름이라 한다.

```js
// ES5
var perfix = 'prop';
var i = 0;
var obj = {};

// 계산된 프로퍼티 이름으로 프로퍼티 키 동적 생성
obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;

console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}

###10.9.3 메서드 축약 표현
ES5에서 메서드를 정의하려면 프로퍼티 값으로 함수를 할당해야 했지만, ES6에서는 메서드를 정의할 때 function 키워드를 생략한 축약 표현을 사용할 수 있다.

//ES5
var obj = {
  name: 'Lee',
  sayHi: function() {
    console.log('Hi! ' + this.name);
  }
};

obj.sayHi(): // Hi! Lee

//ES6
const obj = {
  name: 'Lee',
  sayHi() {
    console.log('Hi! ' + this.name);
  }
};

obj.sayHi(); // Hi! Lee
```
