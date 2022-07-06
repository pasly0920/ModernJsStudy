# 데이터 타입

데이터 타입은 값의 한 종류를 말합니다. 자바스크립트에는 7개의 데이터 타입이 있으며, 모든 값은 데이터 타입을 갖습니다. 또한 7개의 데이터 타입은 원시 타입과 객체 타입으로 분류할 수 있습니다.

원시 타입

- 숫자
- 문자열
- 불리언
- undefined
- null
- 심벌(symbol)

객체 타입(객체, 함수, 배열 등)

## 6.1 숫자 타입

---

숫자 타입은 모든 수를 '실수'로 처리합니다.

```javascript
var integer = 10; 
// 정수로 표현
var double = 10.12;
// 소수점까지 표현
var nergtive = -20;
// 음의 정수 표현
console.log(1 === 1.0);
// 숫자 타입은 모두 실수로 처리되기에 같다.
```

또한 숫자 타입에는 3가지의 특별한 값이 존재합니다.

```javascript
console.log(10/0);
// 양의 무한대, 즉 Infinity가 나타남
console.log(10/-0);
음의 무한대, 즉 -Infinity가 나타남
console.log(1 * 'String');
// NaN 즉, 숫자가 아님을 나타냄
```

cf) NaN을 표시할 때, nan, Nan 등 정확한 표기법을 지키지 않으면 오류가 발생하며, NaN을 제외한 표기법으로 작성하면 이들을 값이 아닌 식별자로 자바스크립트 엔진이 인식하게 된다.

```javascript
var x = nan;
// nan is not undefined라는 문구와 함께 값이 아닌 식별자로 인식
```

## 6.2 문자열 타입

---

문자열은 텍스트 데이터를 나타낼 때 사용되며, 문자열은 나타내기 위해선 '', "", `` 이 3가지로 문자열을 감싸게 됩니다.

```javascript
var string;
string = '문자열';
// 작은따음표
string = "문자열";
// 큰따음표
string = `문자열`;
// 백틱(ES6 문법)
string = '작은따음표로 감싼 문자열 내의 "큰따음표"는 문자열로 인식한다.';
string = "작은따음표로 감싼 문자열 내의 '작은따음표'는 문자열로 인식한다.";
```

또한 따음표로 감싸지 않은 문자열은 자바스크립트 엔진이 이를 식별자로 인식합니다.

```javascript
var string = hello;
// hello is not defined라는 문구와 함께 hello를 식별자로 인식하게 됩니다.
```

## 6.3 템플릿 리터럴

---

ES6부터 템플릿 리터럴을 이용한 문자 표기법이 도입되었습니다. 템플릿 리터럴에는 멀티라인 문자열, 표현식 삽입, 태그드 템플릿 등 다양한 문자열 처리 기능을 제공합니다. 템플릿 리터럴을 사용하기 위해선, 백틱(``)을 사용합니다.

```javascript
var template = `template literal`;
console.log(template)
// template literal이 콘솔에 찍히게 됩니다.
```

### 6.3.1 멀티라인 문자열

일반 문자열에서는 줄바꿈(개행)이 허락되지 않기에, 일반 문자열 내에서 줄바꿈 등의 공백을 표현하려면 백슬래시(\)로 시작하는 이스케이프 시퀸스를 사용해야만 합니다.

이스케이프 시퀸스 참조 : <https://heeestorys.tistory.com/697>

하지만 템플릿 리터럴을 이용한다면 이스케이프 시퀸스 없이 문자열의 줄바꿈 등을 표현할 수 있습니다.

```javascript
var template = "<ul>\n\t<li>안녕하세요</li>\n</ul>";
console.log(template);

<ul>
    <li>안녕하세요</li>
</ul>
// 템플릿 리터럴을 사용하지 않을 때

var template = `<ul>
    <li>안녕하세요</li>
</ul>`;
console.log(template);

<ul>
    <li>안녕하세요</li>
</ul>
// 템플릿 리터럴을 사용할 때
```

### 6.3.2 표현식 삽입

템플릿 리터럴 내에서는 표현식 삽입을 통해 간단히 문자열을 삽입할 수 있습니다. 이를 통해, 문자열 연산자보다 가독성 좋고 간편하게 문자열을 조합할 수 있습니다.

```javascript
var first = '김';
var second = '영찬';

console.log("제 이름은 " + first + second " 입니다.")
// 연산자를 이용한 문자열 조합

var first = '김';
var second = '영찬';

console.log(` 제 이름은 ${first}${second} 입니다.`);
// 템플릿 리터럴을 이용한 문자열 조합
```

## 6.4 불리언 타입

---

불리언 타입의 값은 참, 거짓을 나타내는 true와 false가 있습니다.

```javascript
var a = true;
console.log(a);
// true

var b = false;
console.log(b);
// false
```

## 6.5 undefinde 타입

---

초기에 var로 선언한 변수는 암묵적으로 undefined로 초기화되어 메모리에 저장됩니다. 즉 선언만 했을 뿐, 변수에 특정 값을 할당하지 않았기에 undefined(정의되지 않음)이 나타나게 됩니다.

```javascript
var c;
console.log(c);
// undefined
```

## 6.6 null 타입

---

null은 변수에 값이 없다는 것을 의도적으로 명시하기 위해 사용합니다. null 역시 NaN과 동일하게 대소문자를 확실하게 구분지어서 사용해야만 합니다.

```javascript
var d = "kim";
d = null;
// 변수 d의 값이 더 이상 필요 없어졌다면, 변수 d가 "kim"을 참조하지 않기 위해 null값을 재할당해줍니다.
// 하지만 전역 변수 d를 설정해서 null로 참조값을 없애는 것보단 지역 변수로 스코프를 줄여쓰는게 보다 효율적이라 생각됨.
```

## 6.7 심벌(symbol) 타입

---

ES6에서 새롭게 추가된 타입으로, 변경 불가능한 원시 타입의 값입니다. 원시 타입은 객체를 제외한 값의 유형으로 숫자, 불리언, 문자, 등 앞서 설명한 내용들입니다. 심벌은 다른 값과 중복되지 않은 유일무이한 값이기에, 주로 이름의 충동 위험이 없는 객체의 유일한 프로퍼티 키를 만들기 위해 사용되어집니다.

프로퍼티는 키와 값으로 이루어져 있으며, 함수 역시 값으로 취급되어 프로퍼티로 사용 가능합니다.

```javascript
var person = {
    name : 'lee',
    age : 24,
    sayHello : funtion() {
        console.log(`반가워요, ${this.name}`)
    }
}
```

```javascript
var a = symbol('key');
var b = symbol('key');

console.log(a === b);
// false
console.log(a == b);
// false
```

이처럼 symbol은 각 변수마다 오직 하나만을 갖고 있기 때문에 변수들끼리의 구분이 가능합니다.

## 6.9 데이터 타입의 필요성

---

값은 메모리에 저장하고 참조할 수 있어야 합니다. 메모리에 값을 저장하려면 먼저 확보해야 할 메모리의 공간의 크기를 결정해야 합니다. 

자바스크립트 엔진은 데이터 타입, 즉 값의 종류에 따라 정해진 크기의 메모리 공간을 확보합니다.  즉, 변수에 할당되는 값의 데이터 타입에 따라 확보해야할 메모리 공간의 크기가 결정됩니다.

## 6.10 동적 타이핑

---

### 6.10.1 동적 타입 언어와 정적 타입 언어

정적 타입 언어는 변수의 타입을 변경할 수 없으며, 변수에 선언한 타입에 맞는 값만 할당할 수 있습니다. 대표적인 언어로는 C,C++,자바, 코틀린 등이 있습니다.

하지만 자바스크립트는 정적 타입 언어와 다르게 변수를 선언할 때 타입을 미리 선언하지 않습니다. 다만, let, const, var을 이용해 변수를 선언할 뿐입니다. 그렇기에 어떠한 데이터 타입의 값이라도 자유롭게 할당할 수 있습니다.

```javascript
var foo;
console.log(typeof foo);
// undefined

foo = 3;
console.log(typeof foo);
// number

foo = 'hello';
console.log(typeof foo);
// string

foo = true;
console.log(typeof foo);
// boolean

foo = null;
console.log(typeof foo);
// object

foo = symbol();
console.log(typeof foo);
// symbol

foo = {};
console.log(typeof foo);
// object

foo = [];
console.log(typeof foo);
// object

foo = funtion() {};
console.log(typeof foo);
// funtion
```

이렇듯 자바스크립트의 타입 결정은 선언이 아닌 값의 할당에서 결정됩니다. 그렇기에, 값을 다른 타입으로 할당할 수 있으며 이를 동적 타이핑이라고 합니다.

### 6.10.2 동적 타입 언어오 변수

동적 타입 언어는 변수에 어떤 데이터 타입의 값이라도 할당할 수 있다는 편리함이 있지만 그만큼의 단점도 존재합니다. 

변수의 값이 언제든 변경할 수 있기에, 코드의 양이 많아지나면 이에 대한 혼란도 증가하며 변수 값을 추적하기 어려워질 수도 있습니다. 이렇기에 책에서는 5가지의 주의사항을 제시합니다.

- 변수는 꼭 필요한 경우에 한해 제한적으로 사용합니다

- 변수의 유효 범위(스코프)는 최대한 좁게 만들어 변수끼리의 충돌로 인한 부작용을 방지해야 합니다.

- 전역 변수는 최대한 사용하지 않습니다.

- 변수보다는 상수(const)를 사용해 값의 변경을 최대한 억제합니다.

- 변수의 이름은 변수의 목적이나 용도를 파악할 수 있도록 네이밍합니다.

</br>
</br>

## _**가독성 좋은 코드가 제일 좋은 코드입니다.**_
