# 09. 타입 변환과 단축 평가

## 9.1 타입 변환이란?

자바스크립트의 모든 값은 타입이 있다.  
개발자의 의도에 따라 값의 타입을 변환하는 것을 '명시적 타입 변환' 또는 '타입 캐스팅'이라고 한다. 

```javascript
var x = 10;

var str = x.toString(); // 숫자를 문자열로 타입 캐스팅
console.log(typeof str, str); // string 10
```
<!-- var 변수 = 숫자.toString(진법);    //숫자를 문자로 변환해줌 - 변환하면서 진법을 바꿀 수 있음 -->
개발자의 의도와 상관없이 암묵적으로 타입이 자동 변환되기도 한다. 이를 '안묵적 타입 변환' 또는 '타입 강제 변환'이라 한다.  
타입 변환이란 기존 원시 값을 사용해 다른 타입의 새로운 원시 값을 생성하는 것이다.
```javascript
var x = 10;
var str = x + ''; // 문자열 연결 연산자는 숫자 타입 x의 값을 바탕으로 새로운 문자열을 생성
console.log(typeof str, str); //string 10, x변수의 값이 변경된 것은 아니다
console.log(typeof x, x); //number 10
```

## 9.2 암묵적 타입 변환
코드의 문맥을 고려해서 자바스크립트가 암묵적으로 문자열, 숫자, 불리언과 같은 원시 타입 중 하나로 타입을 자동 변환한다.  

### 9.2.1 문자열 타입으로 변환
```javascript
1 + '2' // -> "12"
```
+연산자는 피연산자 중 하나 이상이 문자열이므로 문자열 연결 연산자로 동작한다.  

### 9.2.2 숫자열 타입으로 변환
```javascript
1-'1' //->0
1*'10' //->10
1/'one'//->NaN(숫자가 아님)
```
산술 연산자의 역할은 숫자 값을 만드는 것이기 때문에 모든 피연산자는 코드 문맥상 모두 숫자 타입이어야 한다.
```javascript
'1' > 0 //-> ture
```
비교 연산자는 불리언 값을 만드는 것이기 때문에 피연산자의 크기를 비교하기 위해서 피연산자가 모두 숫자 타입이어야 한다.

### 9.2.3 불리언 타입으로 변환
if, for문과 같은 제어문 또는 삼항 조건 연산자의 조건식은 불리언 값으로 평가되어야 하는 표현식이다. 자바스크립트는 불리언 타입이 아닌 값을 Truthy 값(참으로 평가되는 값) 또는 Falsy 값(거짓으로 평가되는 값)으로 구분한다. 즉 제어문의 조건식과 같이 불리언 값으로 평가되어야 할 문맥에서 Truthy값은 true로, Falsy 값은 false로 암묵적 타입 변환된다.  
flase로 평가되는 Falsy 값 : flase, undefined, null, 0, -0, NaN, ''(빈 문자열)
```javascript
if('') console.log('1');
if('0') console.log('2');
if('str') console.log('3');
if('null') console.log('4');
//3
```

## 9.3 명시적 타입 변환
표준 빌트인 생성자 함수(String, Number, Boolean)를 new연산자 없이 호출하는 방법과 빌트인 메서드를 사용하는 방법이 있다.
<!--21장에서-->

### 9.3.1 문자열 타입으로 변환
1. String 생성자 함수를 new 연산자 없이 호출하는 방법
2. Object.prototype.toSting 메서드를 사용하는 방법
3. 문자열 연결 연산자를 이용하는 방법
```javascript
String(1);  // -> "1"
String(NaN);  // -> "NaN"

(Infinity).toString();  // -> "Infinity"
(true).toString();  // -> "true"

1+ '' ;  // -> "1"
false + '' ; // -> "false"
```

### 9.3.2 숫자 타입으로 변환
1. Number 생성자 함수를 new 연산자 없이 호출하는 방법
2. parseInt, parseFloat 함수를 사용하는 방법(문자열만 숫자 타입으로 변환 가능)
3. +단항 산술 연산자를 이용하는 방법
4. *산술 연산자를 이용하는 방법
```javascript
Number('-1');  // -> -1

parseInt('0');  // -> 0
parseFloat('10.53');  // -> 10.53

+'-1';  // -> -1  //문자열 타입 -> 숫자 타입
+true;  // -> 1  //불리언 타입 -> 숫자 타입
+false;  // -> 0

'-1'*1;  // -> -1
true*1;  // -> 1
```

### 9.3.3 불리언 타입으로 변환
1. Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
2. !부정 논리 연산자를 두 번 사용하는 방법

```javascript
Boolean('x');  // -> true
Boolean('');  // -> flase
Boolean('false');  // -> true
Boolean(NaN);  // -> flase
Boolean(Infinity);  // -> true
Boolean(null);  // -> false
Boolean(undefined);  // -> false
Boolean({});  // ->true  // 객체타입 -> 불리언 타입

!!'x';  // -> true
!!'';  // -> false
!!0;  // -> false
!![];  // -> true
```

## 9.4 단축 평가
### 9.4.1 논리 연산자를 사용한 단축 평가
논리합(||) 또는 논리곱(&&) 연산자 표현식의 평가 결과는 불리언 값이 아닐 수도 있다. 논리합 또는 논리곱 연산자 표현식은 언제나 2개의 피연산자 중 어느 한쪽으로 평가된다. ||(논리합), &&(논리곱) 연산자는 왼쪽부터 오른쪽으로 평가를 진행하는데,
중간에 평가 결과가 나오면 오른쪽 끝까지 가지 않고 평가 결과를 반환해 버린다.
이를 '단축 평가(short circuit evaluation)'라고 한다. 이때 논리 연산의 결과를 최종 결정하는 피연산자를 타입 변환하지 않고 그대로 반환한다.
논리합(||) 연산자는 두 중 하나의 피연산자가 true로 평가되면 true를 반환한다.  
논리곱(&&) 연산자는 두 개의 피연산자가 모두 true로 평가될 때 true를 반환한다.  


```javascript
'Cat' || 'Dog'  // -> "Cat"
false || 'Dog'  // -> "Dog"
'Cat' || false  // -> "Cat"
'Cat' && 'Dog'  // -> "Dog"
false && 'Dog'  // -> false
'Cat' && false  // -> false
```

조건이 참으로 평가되는 값일 때 if문을 논리곱(&&)연산자로 대체할 수 있고, 조건이 거짓으로 평가되는 값일 때 if문을 논리합(||)연산자로 대체할 수 있다.  

+ 객체를 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 참조할 때

~


### 9.4.2 옵셔널 체이닝 연산자
옵셔널 체이닝(optional chaning) 연산자는 ?.는 좌항의 피연산자가 null 또는 undefined인 경우 undefined를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어간다. 따라서 옵셔널 체이닝 연산자는 개체를 가리키기를 기대하는 변수가 null또는 undefined가 아닌지 확인하고 프로퍼티를 참조할 때 유용하다.
```javascript
var elem = null;

var value = elem?.value;
console.log(value);  //undefined
```

### 9.4.3 null 병합 연산자
null 병합(nullish coalescing) 연산자 ??는 좌항의 피연산자가 null 또는 undefined인 경우 우항의 피연산자를 반환하고 그렇지 않으면 좌항의 피연산자를 반환한다. 이 연산자는 변수에 기본값을 설정할 때 유용하다.
```javascript
var foo = null ?? 'default string' ;
console.log(foo);  // "default string"
```