# 28장 number

표준 빌트인 함수 Number는 원시 타입인 숫자를 다룰 때 유용한 프로퍼티와 메서드를 제공한다.

## 28.1 Number 생성자 함수

표준 빌트인 객체인 Number는 **생성자 함수 객체**다. new 연산자와 함께 호출하여 number 인스턴스를 생성할 수 있다.

Number 생성자 함수에 인수를 전달하지 않고 new 연산자와 함께 호출하면 [[Numberdata]] 내부 슬롯에 0을 할당한 Number 래퍼 객체를 생성한다.

```javascript
const numObj = new Number();
console.log(numObj); // Number {[[PrimitiveValue]]: 0}
```

인수를 전달할 경우 내부 슬롯에 인수로 전달받은 숫자를 할당한다. 숫자가 아닌 값이면 강제로 타입을 변환한다.

```javascript
const numObj = new Number(10);
console.log(numObj); // Number {[[PrimitiveValue]]: 10}
```

```javascript
const numObj = new Number('10');
console.log(numObj); // Number {[[PrimitiveValue]]: 10}
```

```javascript
const numObj = new Number('Hello');
console.log(numObj); // Number {[[PrimitiveValue]]: NaN}
```

명시적 타입 변환에서 살펴보았듯이 new 연산자를 사용하지 않고 Number 생성자 함수를 호출하면 Number 인스턴스가 아닌 숫자를 반환한다. **이를 이용하여 명시적으로 타입을 변환하기도 한다**

```javascript
// 문자열 타입 => 숫자 타입
Number('0'); // 0
Number('-1'); // -1
Number('10.53'); // 10.53

// 불리언 타입 => 숫자 타입
Number(true); // 1
Number(false); // 0
```

## 28.2 Number 프로퍼티

### Number.EPSILON

1과 1보다 큰 숫자 중에서 가장 작은 숫자와의 차이와 같다. 프로그래밍에서 발생하는 **부동소수점 오차**를 바로잡기 위해 사용한다.

```javascript
0.1 + 0.2; // 0.3000000000004
0.1 + 0.2 === 0.3; // false

function isEqual(a, b) {
    return Math.abs(a-b) < Number.EPSILON;
}

isEqual(0.1 + 0.2, 0.3); // true
```

### Number.MAX_VALUE

자바스크립트에서 표현할 수 있는 가장 큰 양수 값이다. 이보다 큰 숫자는 Infinity다.

```javascript
Number.MAX_VALUE; // 1.7976931348523157e+308
Infinity > Number.MAX_VALUE; // true
```

### Number.MIN_VALUE

자바스크립트에서 표현할 수 있는 가장 작은 양수 값이다. 이보다 작은 숫자는 0이다.

```javascript
Number.MIN_VALUE; // 5e-324
Number.MIN_VALUE > 0; // true
```

### Number.MAX_SAFE_INTEGER

자바스크립트에서 안전하게 표현할 수 있는 가장 큰 정수값이다.

> 안전한 정수값은 -(2^53 - -1)과 2^53-1 사이의 정수값이다.

```javascript
Number.MAX_SAFE_INTEGER; // 9007199254740991
```

### Number.MIN_SAFE_INTEGER

자바스크립트에서 안전하게 표현할 수 있는 가장 작은 정수값이다.

```javascript
Number.MIN_SAFE_INTEGER; // -9007199254740991
```

### Number.POSITIVE_INFINITY

양의 무한대를 나타내는 숫자값 INFINITY와 같다.

```javascript
Number.POSITIVE_INFINITY; // Infinity
```

### Number.NEGATIVE_INFINITY

음의 무한대를 나타내는 숫자값 -Infinity와 같다.

```javascript
Number.NEGATIVE_INFINITY; // -Infinity
```

### Number.NaN

숫자가 아님을 나타내는 값이다. Number.NaN은 Window.NaN과 같다.

```javascript
Number.NaN; // NaN
```

## 28.3 Number 메서드

### Number.isFinite

인수로 전달된 값이 정상적인 유한수인지 검사하여 불리언으로 반환한다.

```javascript
Number.isFinite(0); // true
Number.isFinite(Number.MAX_VALUE); // true
Number.isFinite(Number.MIN_VALUE); // true

Number.isFinite(Infinity); // false
Number.isFinite(-Infinity); // false


// 인수가 NaN이면 언제나 false를 반환한다.
Number.isFinite(NaN); // false

// 빌트인 전역 함수 isFinite와 차이점이 있다.
// Number.isFinite는 전달받은 인수를 암시적 형 변환하지 않는다.
Number.isFinite(null); // false

isFinite(null); // true
```

### Number.isInteger

인수로 전달된 숫자값이 정수인지 검사하여 그 결과를 불리언 값으로 반환한다. 인수를 숫자로 암묵적 타입 변환하지 않는다.

```javascript
Number.isInteger(0); // true
Number.isInteger(123); // true
Number.isInteger(-123); // true

Number.isInteger(0.5); // false 
Number.isInteger('123'); // false
Number.isInteger(false); // false
Number.isInteger(Infinity); // false
Number.isInteger(-Infinity); // false
```

### Number.isNaN

인수로 전달된 값이 NaN이지 검사하여 불리언으로 반환한다. 암묵적 타입 변환을 허용하지 않는다.

```javascript
Number.isNaN(NaN); // true

Number.isNaN(undefined); // false

isNaN(undefinded); // true 
```

### Number.isSafeInteger

인수로 전달된 숫자값이 안전한 정수인지 검사하여 불리언 값으로 반환한다.

```javascript
Number.isSafeInteger(0); // true

Number.isSafeInteger(0.5); // false

Number.isSafeInteger('123'); // false
Number.isSafeInteger(false); // false

Number.isSafeInteger(Infinity); // false
Number.isSafeInteger(-Infinity); // false
```

### Number.prototype.toExponential

메서드는 숫자를 지수 표기법으로 변환하여 **문자열**로 반환한다. 인수로 소수점 이하로 표현할 자릿수를 전달할 수 있다.

```javascript
(77.1234).toExponential(); // "7.71234e+1"
(77.1234).toExponential(4); // "7.7123e+1"
(77.1234).toExponential(2); // "7.71e+1"
```

숫자 리터럴과 함께 메서드를 사용하면 에러가 발생한다. 숫자 뒤의 .은 의미가 모호하기 때문이다. 숫자 리터럴과 함께 메서드를 사용할 경우 그룹 연산자()를 사용하는 것을 권장한다.

```javascript
77.toExponential(); // SyntaxError: Invalid or unexpected token
```

### Number.prototype.toFixed

toFixed 메서드는 숫자로 반올림하여 **문자열**로 반환한다. 반올림하는 소수점 이하 자릿수를 나타내는 0 ~ 20 사이의 정수값을 인수로 전달할 수 있다. 인수를 생략하면 기본값 0이 지정된다.

```javascript
(12345.6789).toFixed(); // "12346"
(12345.6789).toFixed(1); // "12345.7"
(12345.6789).toFixed(2); // "12345.68"
(12345.6789).toFixed(3); // "12345.679"
```

### Number.prototype.toPrecision

인수로 전달받은 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림하여 **문자열**로 반환한다. 인수로 전달받은 전체 자릿수로 표현할 수 없는 경우 지수 표기법으로 결과를 반환한다.

전체 자릿수를 나타내는 0~21 사이의 정수값을 인수로 전달할 수 있다. 인수를 생략하면 기본값 0이 지정된다.

```javascript
(12345.6789).toPrecision(); // "12345.6789"
(12345.6789).toPrecision(1); // "1e+4"
(12345.6789).toPrecision(2); // "1.2e+4"
(12345.6789).toPrecision(6); // "12345.7"
```

### Number.prototype.toString

숫자를 **문자열**로 변환하여 반환한다. 진법을 나타내는 2~36 사이의 정수값을 인수로 전달할 수 있다. 인수를 생략하면 기본값 10진법이 지정된다.

```javascript
(10).toString(); // "10"
(16).toString(2); // "10000"
(16).toString(8); // "20"
(106).toString(16); // "10"
```
