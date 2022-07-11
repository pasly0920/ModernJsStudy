# 15장 let, const와 블록 레벨 스코프

이번 장에서는 let,const 키워드를 var와 대조하여 알아보고, 블록 레벨 스코프에 대해 집중적으로 탐구한다.

## var 키워드 변수 선언의 문제점
1. 중복 선언이 가능하다.
2. 함수의 코드 블록만을 **지역 스코프**로 인정한ㅏ.<br>
따라서, 함수가 아닌 if문이나 for문 안에서 var 키워드로 선언한 변수가 외부에서 접근이 가능하다. 그렇기 때문에 의도치 않게 값이 변경될 수 있다.
```javascript
var x = 1;
var i = 1;

if (true) {
    var x = 10; // x의 값이 바뀐다.
}

for(var i = 0; i < 5; i++){
    console.log(i);
}
console.log(x); // 10
console.log(i); // 5
//의도와 다르게 변수의 값이 출력된다.
```
3. 변수 호이스팅을 일으키기 때문에 의도치 않은 오류를 발생시킬 여지를 남긴다.
```javascript
console.log(foo); // 변수 호이스팅에 의해 undefinded

foo = 123;

console.log(foo); // 할당문에 의해 123 

var foo;
```

## let 키워드
1. 변수 중복 선언 금지 (SyntaxError)
2. 블록 레벨 스코프
3. 변수 호이스팅이 발생하지 않는 것처럼 동작한다.
> ### var 키워드로 선언한 경우
> 런타임 이전에 **선언 단계**와 **초기화 단계**가 한번에 진행되어, 어디서 호출하든 undefinded 반환
> 
> ### let 키워드로 선언한 경우
> **선언 단계**와 **초기화 단계**가 분리되어 진행됩니다. 따라서 선언되기 이전에는 참조할 수 없습니다(ReferenceError)
> 이렇게 스코프의 시작 지점부터 초기화 지점까지 변수를 참조할 수 없는 구간을 **일시적 사각지대(TDZ)** 라고 부른다.
> 하지만, let 키워드에 호이스팅이 없는 것은 아니다.

```javascript
let foo = 1; // 전역 변수
{
    console.log(foo); // ReferenceError: Cannot access 'foo' before initialization
    let foo = 2; // 지역 변수
}
```
변수 호이스팅이 발생하지 않는다면 위 예제는 전역 변수 foo의 값을 출력해야 한다. 하지만 let 키워드로 선언한 변수도 여전히 호이스팅이 발생하기 때문에 **참조 에러**가 발생한다.<br>
자바스크립트는 ES6에서 도입된 let, const를 포함해서 모든 선언(var, let, const, function, function*,class 등)을 호이스팅한다.<br>
단, ES6에서 도입된 let,const,class를 사용한 선언문은 호이스팅이 발생하지 않는 것처럼 동작한다.

## 전역 객체와 let
var 키워드로 선언한 전역 변수,전역 함수 그리고 선언하지 않은 변수에 값을 할당한 암묵적 전역은 전역 객체 window의 프로퍼티가 된다. 전역 객체의 프로퍼티를 참조할 때, window를 생략할 수 있다.
```javascript
// 이 코드는 브라우저 환경에서 작동한다.

var x = 1;
y = 2;
function foo() {}

// var 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티다.
console.log(window.x); // 1
console.log(x); // 1

// 암묵적 전역은 전역 객체 window의 프로퍼티다.
console.log(window.y); // 2
console.log(y); // 2

// 함수 선언문으로 정의한 전역 변수는 전역 객체 window의 프로퍼티다.
console.log(window.foo); // f foo() {}
console.log(foo); // f foo() {}
```

let 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다.

## const 키워드
const 키워드는 상수를 선언하기 위해 사용한다. 특징은 let과 대부분 동일하므로 다른 점에 대해서 다룬다.
1. const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야한다 (SyntaxError)
2. let 키워드와 마찬가지로 블록 레벨 스코프, 변수 호이스팅이 발생하지 않는 것처럼 동작한다.
3. var, let과 다르게 재할당이 금지된다. (TypeError)


## 상수를 사용하는 이유
1. 코드 내에서 상수의 의미를 문자로 파악할 수 있음
2. 유지보수성이 대폭 향상
```javascript
//세전 가격
let preTaxPrice = 100;

//세후 가격
// 0.1의 의미를 명확히 알기 어렵기 때문에 가독성이 좋지 않다.
let afterTaxPrice = preTaxPrice + (preTaxPrice * 0.1);

console.log(afterTaxPrice); // 110
```

상수의 이름은 **대문자**로 선언해 상수임을 명확히 나타내고, 여러 단어로 이루어진 경우에는 **스네이크 케이스**로 표현하는 것이 일반적이다.

## const 키워드와 객체
const 키워드로 선언된 변수에 객체를 할당한 경우 값을 변경할 수 있다.
```javascript
const person ={
    name: 'Lee'
};

// 객체는 변경 가능한 값이다. 따라서 재할당 없이 변경이 가능하다.
person.name = 'Kim';

console.log(person); // {name: "kim"}
```
const 키워드는 재할당을 금지할 뿐 "불변"을 의미하지는 않는다. 즉 새로운 값을 재할당하는 것은 불가능하지만 프로퍼티 동적 생성, 삭제, 프로퍼티 값의 변경을 통해 **객체**를 변경하는 것은 가능하다.

## var vs let vs const

변수 선언에는 기본적으로 const를 사용하고 let은 재할당이 필요한 경우에 한정해 사용하는 것이 좋다. 다음 convention을 권장한다.
- ES6을 사용한다면 var 키워드는 사용하지 않는다.
- 재할당이 필요한 경우에 한정해 let 키워드를 사용한다. 이때 변수의 스코프는 최대한 좁게 만든다.
- 변경이 발생하지 않고 읽기 전용으로 사용하는 (재할당이 필요 없는 상수) 원시 값과 객체에는 const 키워드를 사용한다. const 키웓는 재할당을 금지하므로 var, let 키워드보다 안전하다.
