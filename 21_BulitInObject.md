# 21 빌트인 객체

## 21.1 자바스크립트 객체의 분류

---

자바스크립트의 객체는 크게 3가지로 분류할 수 있습니다.

- **표준 빌트인 객체**

    표준 빌트인 객체는 ECMAScript에 정의된 객체를 말하며, 표준 빌트인 객체는 전역 객체의 프로퍼티로서 제공된다.

- **호스트 객체**

    호스트 객체는 ECMAScript에 정의되어 있지 않지만 자바스크립트 환경에서 추가로 제공하는 객체를 말합니다. 브라우저 환경에서는 DOM, Canvas와 같은 클라이언트 사이드 웹 API를 호스트 객체로 제공한다.

- **사용자 정의 객체**

    사용자 정의 객체는 표준 빌트인 객체와 호스트 객체처럼 기본 제공되는 객체가 아닌 사용자가 직접 정의한 객체를 말한다.

## 21.2 표준 빌트인 객체

---

자바스크립트는 Object, String, Boolean, Symbol, Date, Array, Function, Promise, JSON 등 40여 개의 표준 빌트인 객체를 제공한다.

생성자 함수 객체인 표준 빌트인 객체는 프로토타입 메서드와 정적 메서드를 제공하고 생성자 함수 객체가 아닌 표준 빌트인 객체는 정적 메서드만 제공합니다.

예를 들어, 표준 빌트인 객체인 String, Number, Boolean 등은 생성자 함수로서 호출되어 인스턴스를 생성할 수 있습니다.

```javascript
// String 생성자 함수에 의한 String 객체 생성
const strObj = new String('lee'); // String {"lee"}
console.log(typeof strObj); // object

// Number 생성자 함수에 의한 Number 객체 생성
const numObj = new Number(123); // Number {123}
console.log(typeof numObj); // object

// String 생성자 함수에 의한 String 객체 생성
const booleanObj = new Boolean(true); // Boolean {true}
console.log(typeof booleanObj); // object
```

```javascript
const numObj = new Number(1.5); // Number (1.5)
// 생성자 함수에 의한 Number 객체 생성

console.log(numObj.toFixed()); // 2
// toFixed는 Number.prototype의 프로토타입 메서드이다.

console.log(Nnumber.isInteger(0.5)); // false
// isInteger은 Number의 정적 메서드이다.
```

이렇듯 인스턴스를 생성할 수 있는 생성자 함수 객체는 정적 메소드와 프로토타입 메소드 모두를 사용할 수 있습니다.

## 21.3 원시값과 래퍼 객체

---

원시값은 객체가 아니기에 프로퍼티나 메서드를 가질 수 없습니다. 하지만 그럼에도 문자열인 원시값은 객체처럼 동작하게 됩니다.

```javascript
const str = 'hi';

console.log(str.length); // 2
console.log(str.toUpperCase()); // HI
```

이는 원시값인 문자열, 숫자, 불리언 값의 경우 이들 원시값에 대해 마치 객체처럼 마침표 표기법으로 접근하면 자바스크립트 엔진이 일시적으로 원시값을 연관된 객체로 변환해 주기 때문입니다.

즉, 원시값을 객체처럼 사용하면 자바스크립트 엔진은 암묵적으로 연관된 객체를 생성하여 생성된 객체로 프로퍼티에 접근하거나 메서드를 호출하고 다시 원시값으로 되돌린다.

이처럼 문자열, 숫자, 불리언 값에 대해 객체처럼 접근하면 생성되는 임시 객체를 **래퍼 객체**라고 한다.

```javascript
const str1 = 'Hello';
str1.slice(2, 3); // 'l'

const str2 = new String(str1);
return str2.slice(2, 3);

// str1이 메소드를 사용하기 위해선, str2와 같은 래퍼 객체들이 잠깐 str1을 객체로 만들었다가 메서드를 사용한 결과값이 나오면 이를 반환하고 str2는 가비지 컬렉터로 들어가 사라진다.
```

## 21.4 전역 객체

---

전역 객체는 코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체이며, 어떤 객체에도 속하지 않은 최상위 객체다.

전역 객체는 자바스크립트 환경에 따라 지칭하는 이름이 제각각이다. 브라우저 환경에서는 window가 전역 객체를 가리키지만 Node.js 환경에서는 global이 전역 객체를 가리킨다.

전역 객치의 특징은 다음과 같다.

- 전역 객체는 개발자가 의도적으로 생성할 수 없다. 즉, 전역 객체를 생성할 수 있는 생성자 함수가 제공되지 않는다.

- 전역 객체의 프로퍼티를 참조할 때 window를 생략할 수 있다.

```javascript
window.parseInt('F', 16) === parseInt('F', 16)  // 15
```

- var 키워드로 선언한 전역 변수와 선언하지 않은 변수에 값을 할당한 암묵적 전역, 그리고 전역 함수는 전역 객체의 프로퍼티가 된다. 하지만 let이나 const로 선언한 전역 변수는 전역 객체의 프로퍼티가 되지 않습니다.

```javascript
var foo = 1;
console.log(window.foo) // 1

bar = 2; // window.bar = 2;

function baz() {
    return 3;
}
console.log(window.baz()) // 3

let a = 123;
console.log(window.a) // undefined
```

### 21.4.1 빌트인 전역 프로퍼티

빌트인 전역 프로퍼티는 전역 객체의 프로퍼티를 의미합니다. 주로 전역에서 사용하는 값으로 제공됩니다.

- Infinity

Infisity 프로퍼티는 무한대를 나타내는 숫자값을 갖습니다.

```javascript
console.log(window.Infinity === Infinity); // true

console.log(3/0); // Intfinity
```

- NaN

NaN 프로퍼티는 숫자가 아님을 나타내는 숫자값을 갖는다.

```javascript
console.log(window.NaN); // NaN
console.log(1 * 'string') // NaN
```

- undefined

undefined 프로퍼티는 원시 타입을 값으로 갖는다.

```javascript
console.log(window.undefined); // undefined

var foo;
console.log(foo); // undefined
```

### 21.4.2 빌트인 전역 함수

빌트인 전역 함수는 전역에서 호출할 수 있는 빌트인 함수로서 전역 객체의 메서드이다.

``` javascript
// isFinite 는 인수가 유한수면 true, 무한수면 false를 반환합니다.
isFinite(10); // true
isFinite(null); // true
isFinite('hi'); // false
isFinite(Infinity); // false

// isNaN 은 전달받은 인수가 숫자 타입이 아니라면 true를, 숫자 타입이라면 false를 반환합니다.
// 필자도 숫자만 인풋값을 받는 기능을 쓸 때, isNaN을 사용하여 숫자값만 받도록 했다.
isNaN(10); // false
isNaN('hi') // true 'hi' -> NaN
isNaN(true) // false true -> 1

// parseInt는 문자열을 정수로 해석하여 반환합니다.
// ex.프롬프트나 인풋으로 받아온 value값의 타입은 문자열이기에, 계산을 해주려면 parseInt를 통해 정수로 바꿔줘야만 함. 
parseInt('10'); // '10' -> 10
```

### 21.4.3 암묵적 전역

```javascript
console.log(x); // 변수 호이스팅이 실행된다. -> undefined
console.log(y); // 변수 호이스팅이 실행되지 않음 -> ReferenceError : y is not defined

var x = 10;

function foo(){
    y = 20; //  변수의 선언없이 값을 할당하면 전역 객체 window에 프로퍼티로 속하게 된다. -> window.y
}

foo();

console.log(x+y); // 선언하지 않은 식별자 y를 전역에서 참조 가능하다. -> 30
console.log(window.x); // 10
console.log(window.y); // 20

delete x; // 전역 변수는 삭제되지 않는다.
delete y; // 전역 객체에 할당된 프로퍼티는 삭제된다.

console.log(winodw.x); // 10
console.log(window.y); // undefined
```

결국 y는 전역 객체의 프로퍼티가 되어 전역 변수처럼 동작하는 것을 암묵적 전역이라고 한다. 하지만 y는 변수 선언 없이 단지 전역 객체의 프로퍼티로 추가되었을 뿐이기에 y는 변수가 아니다. 그러므로 호이스팅이 발생하지도 않으며, 삭제가 가능하다.
