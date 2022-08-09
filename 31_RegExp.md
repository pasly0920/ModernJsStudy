# 31 정규표현식

## 31.1 정규표현식이란?

---

정규표현식은 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어다. 정규표현식은 문자열을 대상으로 패턴 매칭 기능을 제공한다. 패턴 매칭 기능이란 특정 패턴과 일치하는 문자열을 검색하거나 추출 또는 치환할 수 있는 기능을 말한다.

```javascript
const telNum = '010-2064-6347';
// 사용자로부터 입력받은 전화번호

const regExp = /^\d{3}=\d{4}-\d{4}$/;
// 정규 표현식 리터럴로 휴대폰 번호의 패턴을 정의한다.

regExp.test(tel); // false
// tel이 휴대폰 번호 패턴에 매칭하는지 테스트 해본다.
```

예를 들어, 회원가입 화면에서 사용자로부터 입력받은 휴대폰 번호가 유효한지 체크하는 경우, 휴대폰 번호는 "숫자 3개" + "-" + "숫자 4개" + "-" + "숫자 4개"라는 일정한 패턴이 있다. 이를 위해 앞에 보이는 코드대로 정규 표현식을 작성할 수 있다.

이렇듯 정규표현식을 사용하면 반복문과 조건문 없이 패턴을 정의하고 간단히 입력값(value)를 체크할 수 있다.

## 31.2 정규표현식의 생성

---

정규표현식을 작성할 때는 일정한 패턴이 존재한다.

```javascript
const target = 'is this all there is'; // 패턴 : is, 플래그 : i

const regExp = /is/i; // 대소문자 구별 없이(i의 역할) is를 검색한다.

regExp.test(test); // true
```

```javascript
const target = 'is this all there is'; // 패턴 : is, 플래그 : i

const regExp = new RegExp(/is/i); // 생성자 함수로도 사용 가능

regExp.test(test); // true
```

## 31.3 RegExp 메서드

---

정규표현식을 사용하는 메서드에는 exec, test, match, replac 등 다양한 메서드들이 있다.

### 31.3.1 RegExp.prototype.exec

exec 메서드는 인수로 전달받은 문자열에 대해 정규표현식의 패턴을 검색하여 매칭 결과를 배열로 반환합니다.

만약 매칭 결과가 없는 경우, null로 해석합니다.

```javascript
const target = 'is this all there is';
const regExp = /is/;

regExp.exec(target);
// ["is", index: 5, input: 'is this all there is', groups: undefined]
```

### 31.3.1 RegExp.prototype.test

test 메서드는 인수로 전달받은 문자열에 대해 정규표현식의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환한다.

### 31.3.1 RegExp.prototype.match

match 메서드는 대상 문자열과 인수로 전달받은 정규표현식과의 매칭 결과를 배열로 반환한다.

```javascript
const target = 'is this all there is';
const regExp = /is/;

regExp.match(target);
// ["is", index: 5, input: 'is this all there is', groups: undefined]
```

```javascript
const target = 'is this all there is';
const regExp = /is/g;

regExp.match(target); // ["is", "is"]
```

exec 메서드와 match 메서드의 차어짐어느 모든 패턴을 검색하는 g 플래그를 지정해도 exec 메서드는 첫 번째 매칭 결과만을 반환하지만, match 메서드는 g 플래그로 지정되는 모든 매칭 결과를 배열로 반환한다.

## 31.4 플래그

---

패턴과 함께 정규표현식을 구성하는 플래그는 여러 종류가 있다. 그 중에서도 중요한 플래그 3개를 알아보자.

- i -> 대소문자 구분없이 모든 패턴을 검색한다.

- g -> 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색한다.

- m -> 문자열의 행이 바뀌더라도 패턴 검색을 계속한다.

```javascript
const target = 'Is this all there is';

target.match(/is/); // 대소문자를 구별하여 한 번만 검색한다.
// ["is", index: 5, input: 'is this all there is', groups: undefined]

target.match(/is/i); // 대소문자를 구별하지 않고 한 번만 검색한다.
// ["Is", index: 5, input: 'is this all there is', groups: undefined]

target.match(/is/g); // 대소문자를 구별하여 전역 검색한다.
// ["is", "is"]

target.match(/is/ig); // 대소문자를 구별하지 않고 전역 검색한다.
// ["is", "is"]
```

## 31.5 패턴

---

### 31.5.1 임의의 문자열 검색

.은 임의의 문자 한 개를 의미한다. 문자 내용은 무엇이든 상관없다. 예시를 통해 알아보자.

```javascript
const target = 'Is this all there is';

const regExp = /.../g;

target.match(regExp); // ["Is ", "thi", "s a", "ll ", "the", "re ", "is?"]
```

### 31.5.2 반복 검색

{m,n}은 최소 m번, 최대 n번 반복되는 문자열을 의미한다.

```javascript
const target = "A AA B BB Aa Bb AAA";

const regExp = /A{1,2}/g; // 'A'가 최소 1번, 최대 2번 반복되는 문자열을 전역 검색한다.

target.match(regExp); // ["A", "AA", "AA", "A"]
```

```javascript
const target = "A AA B BB Aa Bb AAA";

const regExp = /A{2}/g; // 'A'가 2번 반복되는 문자열을 전역 검색한다.

target.match(regExp); // ["AA", "AA"]
```

```javascript
const target = "A AA B BB Aa Bb AAA";

const regExp = /A{2,}/g; // 'A'가 최소 2번 이상 반복되는 문자열을 전역 검색한다.

target.match(regExp); // ["AA", "AAA"]
```

```javascript
const target = "A AA B BB Aa Bb AAA";

const regExp = /A+/g; // 'A'가 최소 1번 이상 반복되는 문자열을 전역 검색한다.

target.match(regExp); // ["A", "AA", "A", "AAA"]
```

### 31.5.3 OR 검색

|은 or의 의미를 갖는다.

```javascript
const target = "A AA B BB Aa Bb";

const regExp = /A|B/g; //A 또는 B를 전역 검색한다.

target.match(regExp); // ["A", "A", "A", "B", "B", "B", "A", "B"]
```

```javascript
const target = "A AA B BB Aa Bb";

let regExp = /A+|B+/g; //A 또는 B가 한 번 이상 반복되는 문자열을 전역 검색한다.
regExp = /[AB]+/g; //,, (위의 코드와 동일하다)

target.match(regExp); // ["A", "AA", "B", "BB", "A", "B"]
```

```javascript
const target = "A AA ZZ BB Aa Bb";

const regExp = /[A-Z]/g; //A부터 Z의 범위에서 한 번 이상 반복되는 문자열을 전역 검색한다.

target.match(regExp); // ["A", "AA", "BB", "ZZ", "A", "B"]
```

```javascript
const target = "A BB 1234";

let regExp = /[0-9]/g; //숫자 중에서 한 번 이상 반복되는 문자열을 전역 검색한다.
regExp = /[\d]/g; // ,, (위의 코드와 동일하다)

target.match(regExp); // ["1234"]
```

```javascript
const target = "A BB 1234";

const regExp = /[\D]/g; // 숫자가 아닌 문자가 한 번 이상 반복되는 문자열을 검색한다.

target.match(regExp); // ["A", "BB"]
```

### 31.5.4

[] 내부에서의 ^은 not의 의미를 갖는다. 예를 들어, [^0-9]는 숫자를 제외한 문자를 의미한다.

```javascript
const target = "AA BB 12 Aa Bb";

const regExp = /[^0-9]+/g; //숫자를 제외한 문자열을 전역 검색한다.

target.match(regExp); // ["AA", "BB", "Aa", "Bb"]
```

### 31.5.5 시작 위치로 검색

[] 외부에서의 ^은 문자열의 시작을 의미한다.

```javascript
const target = "https://naver.com";

const regExp = /^https/; //https로 시작하는지 검색한다.

target.test(regExp); // true
```

### 31.5.6 마지막 위치로 검색

$는 문자열의 마지막을 의미한다.

```javascript
const target = "https://naver.com";

const regExp = /$com/; //https로 시작하는지 검색한다.

target.test(regExp); // true
```

## 31.6 자주 사용하는 정규표현식

---

### 31.6.1 아이디로 사용 가능한지 검사

```javscript
const id = 'abc123';

/^[A-Za-z0-9]{4,10}$/.test(id); // ture
// 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4~10자리인지 검사한다.
```

### 31.6.2 핸드폰 번호 형식에 맞는지 검사

```javscript
const cellPhoneNum = '010-2064-6347';

/^\d{3}-\d{3,4}-\d{4}$/.test(cellPhoneNum); // true
```

**대부분의 예시들은 구글링을 통해 쉽게 접할 수 있으니, 참고하거나 궁금하면 구글링을 하도록 하자. 갓글링...**
