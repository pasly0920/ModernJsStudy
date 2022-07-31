# 32장 String

표준 빌트인 객체인 String은 원시 타입인 문자열을 다룰 때 유용한 프로퍼티와 메서드를 제공한다.

## 32.1 String 생성자 함수

**21장을 다시 복습하고, 살펴보면 더 쉽습니다!**

표준 빌트인 객체인 String 객체는 생성자 함수 객체이므로 new 연산자와 함께 호출하여 String 인스턴스를 생성할 수 있다.

------

**String 생성자 함수에 인수를 전달하지 않고** new 연산자와 함께 호출하면 [[StringData]] 내부 슬롯에 빈 문자열을 할당한 String 래퍼 객체를 생성한다.

여기서 String 래퍼 객체란 21.3절에서 살펴본 것과 같이, 객체처럼 접근했을 때 생기는 임시 객체를 말한다.

```javascript
const strobj = new String();
console.log(strObj);
```

이 코드를 실행해보면 [[PrimitiveValue]]라는 접근할 수 없는 프로퍼티를 확인할 수 있으며, 이것은 [[StringData]] 내부 슬롯을 가리킨다.

ES5에서는 [[StringData]]를 [[PrimitiveValue]]라고 불렀다.

------

**String 생성자 함수의 인수로 문자열을 전달하면서** new 연산자와 함께 호출하면 [[StringData]] 내부 슬롯에 인수로 전달받은 문자열을 할당한 String 래퍼 객체를 생성한다.

```javascript
const strobj = new String('Lee');
console.log(strObj);
```

11.1.2절에서 살펴보았듯이, String 래퍼 객체는

1. 배열처럼 length 프로퍼티와 인덱스를 나타내는 숫자 형식의 문자열을 프로퍼티 키로 갖는다
2. 각 문자를 프로퍼티 값으로 갖는다.
3. 유사 배열 객체이면서 이터러블이다.

따라서 배열과 유사하게 **인덱스를 사용하여 각 문자에 접근할 수 있다.**

단, 문자열은 원시값이므로 변경할 수 없으며, 이 때 에러가 발생하지는 않는다.

```javascript
console.log(strObj[0]); //L

strObj[0] = 'S';
console.log(strObj); // 'Lee'
```

------

**String 생성자 함수의 인수로 문자열이 아닌 값을 전달**하면 인수를 문자열로 강제 변환한 후, [[StringData]] 내부 슬롯에 변환된 문자열을 할당한 String 래퍼 객체를 생성한다.

9.3절에서 살펴보았듯, new 연산자를 사용하지 않은 채 String 생성자 함수를 호출하면, String 인스턴스가 아닌 문자열을 반환한다. 이를 이용하여 명시적으로 타입을 변환하기도 한다.

```javascript
String(1); // "1"
String(NaN); // "NaN'
String(Infinity);

String(true);
String(false);
```

------

## 32.2 length 프로퍼티

문자열의 문자 개수를 반환하는 프로퍼티를 length 프로퍼티라고 한다. String 래퍼 객체 역시 length 프로퍼티를 갖는다.

위에서 언급했다싶이, 인덱스를 나타내는 숫자를 프로퍼티 키로, 각 문자를 프로퍼티 값으로 가지므로, **String 객체는 유사 배열 객체다.**

------

## 32.3 String 메서드

배열에는 원본 배열을 직접 변경하는 메서드(mutator method)와 새로운 배열을 생성하여 반환하는 메서드(accsessor method)가 있다.

하지만 String 객체에는 원본 String 래퍼를 직접 변경하는 메스드는 존재하지 않으며, 언제나 새로운 문자열을 반환한다. 문자열은 immutable한 원시값이기 때문에, **String 래퍼 객체도 읽기 전용 객체로 제공된다.**

```javascript
const strObj = new String('Lee');

console.log(Object.getOwnPropertyDesciptors(strObj)); // read-only이므로 writable 프로퍼티 어트리뷰트 값이 false
```

------

## 32.3.1 String.prototype.indexOf

indexOf 메서드는 메서드를 호출한 문자열, 즉 대상 문자열에서 인수로 전달받은 문자열을 검색하여, 첫 번째 인덱스를 반환하고, 검색에 실패하면 -1을 반환한다.

```javascript
const str = 'Hello World';

str.indexof('l');
str.indexof('or');
str.indexof('x');
```

그 외 indexOf 메서드와 관련된 내용은 밑의 코드를 참조하자.

```javascript
// 검색을 시작할 인덱스를 설정하려면, indexOf 두 번째 인수로 인덱스를 전달하면 된다.
str.indexof('l', 3);

// 특정 문자열이 존재하는지 확인할 때도 indexOf 활용할 수 있다.
if (str.indexOf('Hello') !== -1) {
    // 특정 문자열 존재할 때 처리할 내용
}

// ES6에서 도입된 String.prototype.includes 메서드 사용하면 가독성이 좋아진다.
if (str.includes('Hello')) {
    // 특정 문자열 존재할 때 처리할 내용
}

```

------

## 32.3.2 String.prototype.search

```javascript
const str = 'Hello World';

// 정규표현식과 매치하는 문자열을 대상 문자열에서 검색하여 일치하는 문자열의 인덱스를 반환한다.
str.search(/o/);
str.search(/f/);
```

------

## 32.3.3 String.prototype.includes

```javascript
// ES6에서 도입된 includes 메서드는 문자열 포함 여부를 true, false로 반환한다.

const str = 'Hello World';

str.includes('l', 3);
```

------

## 32.3.4 String.prototype.startsWith

```javascript
// ES6에서 도입된 startsWith 메서드는 인수로 전달받은 문자열로 대상 문자열이 시작하는지를 true, false로 반환한다.

const str = 'Hello World';

str.startswith('Wo');
str.startswith('Wo', 6);
```

------

## 32.3.5 String.prototype.endsWith

```javascript
// ES6에서 도입된 endsWith 메서드는 인수로 전달받은 문자열로 대상 문자열이 끝나는지를 true, false로 반환한다.

const str = 'Hello World';

str.endswith('Wo');
str.endswith('lo', 5); // 여기서 두 번째 인수는 검색할 문자열의 길이를 나타냄
```

------

## 32.2.6 String.prototype.charAt

```javascript
// charAt 메서드는 인수로 전달받은 인덱스가 대상 문자열에서 위치한 문자를 검색하여 반환한다.

const str = 'Hello';

str.charAt(5); // 인덱스가 문자열 범위 벗어난 경우 빈 문자열 '' 반환
```

charAt 메서드와 유사한 문자열 메서드로 charCodeAt과 codePointAt이 있다.

------

## 32.3.7 String.prototype.substring

```javascript
// substring 메서드는 첫 번째 인수로 전달 받은 인덱스부터 두 번째 인수로 전달 받은 인덱스 바로 이전 문자까지의 부분 문자열을 반환.

const str = 'Hello';

str.substring(1, 4); 
str.substring(1); // 두 번째 인덱스 생략 가능, 이 경우 첫 번째 인수의 인덱스부터 끝까지 반환 
```

substring 메서드의 첫 번째 인수는 두 번째 인수보다 작은 정수여야 정상이지만, 다음과 같이 인수를 전달해도 정상 동작한다.

- 첫 번째 인수 > 두 번째 인수 인 경우 두 인수 교환된다.
- 인수 < 0 또는 NaN 인 경우 0으로 취급된다.
- 인수 > 문자열의 길이 인 경우 인수는 문자열의 길이로 취급된다.

------

substring과 indexOf 메서드를 함께 사용하면 특정 문자열 기준으로 앞뒤에 위치한 부분 문자열도 취득할 수 있다.

```javascript
const str = 'Hello World';

str.substring(0, str.indexOf(' ')); 
str.substring(str.indexOf(' ') + 1, str.length); // 두 번째 인수 생략해도 같은 결과 나옴
```

------

## 32.3.8 String.prototype.slice

slice 메서드는 substring 메서드와 동일하게 동작하지만, slice 메서드에서는 음수인 인수를 전달할 수 있다. 음수인 인수는 대상 문자열의 뒤에서부터 시작하여 문자열을 잘라내여 반환한다.

```javascript
const str = 'Hello World';

str.substring(-5);
str.slice(-5);
```

------

## 32.3.9 String.prototype.toUpperCase

```javascript
const str = 'Hello World!';

str.toUpperCase(); // 모두 대문자로 변경
```

------

## 32.3.10 String.prototype.toLowerCase

```javascript
const str = 'Hello World!';

str.toLowerCase(); // 모두 소문자 변경
```

------

## 32.3.11 String.prototype.trim

```javascript
const str = '  foo  ';

// String.prototype.{trimStart, trimEnd} ; Proposal stage 4
str.trim(); // 앞뒤 공백 제거
str.trimStart(); // 앞 공백 제거
str.trimEnd(); // 뒤 공백 제거
```

32.3.13절에서 살펴볼 replace 메서드를 활용해서 정규 표현식을 인수로 전달하여 공백을 제거할 수도 있다.

------

## 32.3.12 String.prototype.repeat

```javascript
const str = 'abc';

str.repeat(); // ''
str.repeat(0); // ''
str.repeat(1); // 'abc'
str.repeat(2.5); // 'abcabc'
str.repeat(-1); // RangeError
```

------

## 32.3.13 String.prototype.replace

```javascript
const str = 'Hello world';

// replace 메서드는 첫 번째 인수로 전달받은 문자열 또는 정규표현식을 검색하여 두 번째 인수로 전달한 문자열로 치환한 문자열을 반환한다.
str.replace('world', 'Lee');
```

```javascript
const str = 'Hello world world';

// 검색된 문자열이 여러 번 존재하면 첫 문자열만 치환한다.
str.replace('world', 'Lee');
```

첫 번째 인수로 정규 표현식을 전달할 수 있다.

```javascript
const str = 'Hello Hello';

// 대소문자 구별하지 않고 전역 검색
str.replace(/hello/gi, 'Lee'); // 'Lee Lee'
/* 이와 같이 특수한 교체 패턴을 사용할 수 있다. 
자세한 내용은 MDN의 함수 설명을 찾아 보면 된다.
*/
```

두 번째 인수로 치환 함수를 전달할 수 있다. replace 메서드는

- 첫 번째 인수에 매치해본 결과를 두 번째 인수인 치환 함수의 인수로 전달하면서
- 동시에 호출하고,
- 치환 함수가 반환한 결과와 매치 결과를 치환한다.

카멜 케이스를 스네이크로 케이스로 변경하는 함수로 예시를 들어보자.

```javascript
function camelToSnake(camelCase) {
    return camelCase.replace(/.[A-Z]/g, match => {
        console.log(match); //'oW'
        return match[0] + '_' + match[1].toLowerCase();
    });
}

const camelCase = 'helloWorld';
camelToSnake(camelCase); // 'hello_world'
```

------

## 32.3.14 String.prototype.split

```javascript
const str = 'How are you doing?';

// 첫 번째 인수로 전달한 문자열 또는 정규 표현식을 검색하여 문자열을 구분, 분리된 각 문자열로 이루어진 배열 반환
str.split(' '); // 공백으로 구분
str.split(/\s/); // \s 는 스페이스, 탭 등의 공백을 의미
str.split(''); // 빈 문자열 전달하면 모든 문자 분리됨
str.split(); // 인수 생략하면 전체를 단일 요소로 하는 배열 반환
```

```javascript
const str = 'How are you doing?';

// 두 번째 인수로 배열의 길이 지정
str.split(' ', 3); // ["How", "are", "you"]
```

split 메서드는 배열을 반환하므로, Array.prototype.reverse, Array.prototype.join 메서드와 함께 사용하면 문자열 역순으로 뒤집을 수 있다.

```javascript
function reverseString(str) {
    return str.split('').reverse().join('');
}

reverseString('Uos Life');
```
