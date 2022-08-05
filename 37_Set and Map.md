# Set과 Map

## 37.1 Set

- **Set 객체** : 중복되지 않는 유일한 값들의 집합
- 배열과 유사 but 차이가 있음

### 배열 vs. Set 객체

구분| 배열| Set 객체
|:----------:|:----------:|:----------:|
동일한 값을 중복하여 포함 가능| O| X
요소 순서에 의미 O| O| X
인덱스로 요소에 접근 가능| O| X

- 수학적 집합을 구현하기 위한 자료구조임 (= 수학적 집합의 특성을 가짐)
-> 교집합, 합집합, 차집합, 여집합 등 구현 가능

***

### 37.1.1 Set 객체의 생성

- **Set 생성자 함수**로 생성함

#### 1) 인수를 전달 X : 빈 Set 객체가 생성됨

```javascript
const set = new Set();
console.log(set); // Set(0) {} -> 여기서  Set(size) {요소값}
```

#### 2) 이터러블을 인수로 전달받아 Set 객체 생성

- 이터러블의 중복된 값은 Set 객체에 요소로 저장 X
+) 반복 가능한(iterable, 이터러블) 객체 : 배열을 일반화한 객체 ex) 배열, 문자열...

```javascript
// 중복된 값 저장 X
const set1 = new Set([1, 2, 3, 3]);
console.log(set1); // Set(3) {1, 2, 3}

const set2 = new Set('hello');
console.log(set2); // Set(4) {"h", "e", "l", "o"}
```

- 이 특성으로 배열에서 중복된 요소 제거 가능 !

```javascript
/* 1) filter 메소드를 사용한 배열의 중복 요소 제거
 : 인수로 전달받은 배열에서 해당 index의 요소값이 첫번째로 나타나는 위치의 index === 현재 index
      이 조건에 대해 true가 리턴되는 요소만 배열로 리턴함
*/ 
const uniq = array => array.filter((v, i, self) => self.indexOf(v) === i);
console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [2, 1, 3, 4]

/* 2) Set을 사용한 배열의 중복 요소 제거
 : 중복 요소를 제거하고 싶은 배열을 인수로 전달
*/
const uniq = array => [...new Set(array)];
console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [2, 1, 3, 4]
```

> ✏️ **Array.prototype.filter(cb(ele, idx, arr))** : 배열에 접근하는 프로토타입 메소드

```javascript
// 배열 각각의 요소에 대해서 콜백함수 실행. 콜백함수에는 배열 각각의 요소, index, 배열자체(thisValue)가 인수로 들어가고 true가 리턴되는 요소만 배열로 리턴.
let arr = [1, 2, 3];
arr.filter((ele) => ele < 3); // [1, 2]
```

> ✏️  **indexOf() 함수**
>
문자열(string)에서 특정 문자열(searchvalue)을 찾고, 검색된 문자열이 '첫번째'로 나타나는 위치 index를 리턴함

```javascript
string.indexOf(searchvalue, position)
```

- 파라미터
-- `searchvalue` : 필수 입력값, 찾을 문자열
-- `position` : optional, 기본값은 0, string에서 searchvalue를 찾기 시작할 위치
- 찾는 문자열이 없으면 -1을 리턴합니다.
- 문자열을 찾을 때 대소문자를 구분합니다.

***

### 37.1.2 요소 개수 확인

- **Set.prototype.size 프로퍼티** 사용

```javascript
const { size } = new Set([1, 2, 3, 3]);
console.log(size); // 3
```

- size 프로퍼티 - setter 함수 없이 getter 함수만 존재하는 **접근자 프로퍼티**임
=> size 프로퍼티에 숫자 할당해서 Set 객체의 요소 개수 변경 불가능

> ✏️ 접근자 프로퍼티 : 자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수로 구성된 프로퍼티 (16.3.2절 참고...)

```javascript
const set = new Set([1, 2, 3]);

console.log(Object.getOwnPropertyDescriptor(Set.prototype, 'size'));
// {set: undefined, enumerable: false, configurable: true, get: ƒ}
// 프로퍼티 어트리뷰트인 [[Set]], [[Enumerable]]이 생략됨, [[Configurable]]값이 true.

set.size = 10; // size 프로퍼티에 숫자 할당 불가능하므로 무시된다.
console.log(set.size); // size값은 그대로 3임
```

***

### 37.1.3 요소 추가

- Set 객체에 요소 추가할 땐 **Set.prototype.add 메서드** 사용

```javascript
const set = new Set(); // 빈 Set 객체 생성
console.log(set); // Set(0) {}

set.add(1); // 요소로 1 추가
console.log(set); // Set(1) {1}
```

- add 메서드 : 새로운 요소가 추가된 Set 객체 반환함
-> add 메서드를 연속적으로 호출 가능

```javascript
const set = new Set();

set.add(1).add(2); // set.add(1) = 1이 추가된 Set 객체임 -> 거기에 .add(2)한 것
console.log(set); // Set(2) {1, 2}
```

- Set 객체에 중복된 요소의 추가는 허용 X
-> 에러 없이 무시됨

```javascript
const set = new Set();

set.add(1).add(2).add(2); // 2를 중복 추가함 -> 무시
console.log(set); // Set(2) {1, 2}
```

- Set 객체는 NaN과 NaN, +0과 -0은 같다고 평가 -> 중복 추가 허용 X
*) 일치 비교 연산자 (===)와의 차이 : (===)는 NaN과 NaN은 다름, +0과 -0은 같다고 평가

```javascript
const set = new Set();

console.log(NaN === NaN); // false
console.log(0 === -0); // true

// NaN과 NaN을 같다고 평가하여 중복 추가를 허용하지 않는다.
set.add(NaN).add(NaN);
console.log(set); // Set(1) {NaN}

// +0과 -0을 같다고 평가하여 중복 추가를 허용하지 않는다.
set.add(0).add(-0);
console.log(set); // Set(2) {NaN, 0}
```

- Set 객체 : 객체 & 배열과 같이 js의 모든 값을 요소로 저장 가능

```javascript
const set = new Set();

set
  .add(1)
  .add('a')
  .add(true)
  .add(undefined)
  .add(null)
  .add({})
  .add([]);

console.log(set); // Set(7) {1, "a", true, undefined, null, {}, []}
```

***

### 37.1.4 요소 존재 여부 확인

- Set 객체에 특정 요소가 존재하는지 확인할 땐 **Set.prototype.has 메서드** 사용
- has 메서드 : 특정 요소의 존재 여부 나타내는 불리언 값 반환

```javascript
const set = new Set([1, 2, 3]);

console.log(set.has(2)); // 2가 있으므로 true 출력
console.log(set.has(4)); // 4가 없으므로 false 출력
```

***

### 37.1.5 요소 삭제

- Set 객체에 특정 요소를 삭제할 땐 **Set.prototype.delete 메서드** 사용
- delete 메서드 : 삭제 성공 여부 나타내는 불리언 값 반환
- delete 메서드엔 삭제하려는 요소값을 인수로 전달해야 함
-- Set 객체는 순서에 의미가 없으므로 인덱스가 없음 -> 인덱스를 인수로 전달하면 안 됨

```javascript
const set = new Set([1, 2, 3]);

// 요소 2를 삭제한다.
set.delete(2);
console.log(set); // Set(2) {1, 3}

// 요소 1을 삭제한다.
set.delete(1);
console.log(set); // Set(1) {3}
```

- 존재하지 않는 요소를 삭제하려 하면 에러 없이 무시됨

```javascript
const set = new Set([1, 2, 3]);

// 존재하지 않는 요소 0을 삭제하면 에러없이 무시된다.
set.delete(0);
console.log(set); // Set(3) {1, 2, 3}
```

- delete 메서드는 삭제 성공 여부 나타내는 불리언 값 반환하므로(add 메서드처럼 Set 객체를 반환하는게 아니라서) 연속적으로 호출 불가능

```javascript
const set = new Set([1, 2, 3]);

// delete는 불리언 값을 반환한다.
set.delete(1).delete(2); // TypeError: set.delete(...).delete is not a function
```

***

### 37.1.6 요소 일괄 삭제

- **Set.prototype.clear 메서드** 사용
- clear 메서드 : 항상 `undefined` 반환함

```javascript
const set = new Set([1, 2, 3]);

set.clear();
console.log(set); // Set(0) {}
```

***

### 37.1.7 요소 순회

- **Set.prototype.forEach 메서드** 사용
: Array.prototype.forEach 메서드와 유사하게 콜백 함수 & forEach 메서드의 콜백 함수 내부에서 `this`로 사용될 객체(옵션)을 인수로 전달함

> 콜백 함수는 3개의 인수 전달받음

- 1번째 인수 : 현재 순회 중인 요소값
- 2번째 인수 : 현재 순회 중인 요소값
- 3번째 인수 : 현재 순회 중인 Set 객체 자체

*) 1번째 = 2번째 인수 : 그냥 Array.prototype.forEach 메서드와 인터페이스 통일하기 위한 거임
(Array.prototype.forEach 메서드의 콜백 함수 - 2번째 인수로 현재 순회 중인 요소의 인덱스 전달 받음
but, Set 객체는 인덱스를 안 가지므로 Set.prototype.forEach 메서드 - 2번째 인수로 그냥 요소값 전달 받음)

```javascript
const set = new Set([1, 2, 3]);

set.forEach((v, v2, set) => console.log(v, v2, set));
/* 요소값, 요소값, Set 객체 출력됨
1 1 Set(3) {1, 2, 3}
2 2 Set(3) {1, 2, 3}
3 3 Set(3) {1, 2, 3}
*/
```

- Set 객체 - 이터러블임
-> `for...of`문으로 순회 가능
-> 스프레드 문법 & 배열 디스트럭처링의 대상이 될 수 있음

```javascript
const set = new Set([1, 2, 3]);

// Set 객체는 Set.prototype의 Symbol.iterator 메서드를 상속받는 이터러블이다.
console.log(Symbol.iterator in set); // true

// 이터러블인 Set 객체는 for...of 문으로 순회할 수 있다.
for (const value of set) {
  console.log(value); // 1 2 3
}

// 이터러블인 Set 객체는 스프레드 문법의 대상이 될 수 있다.
console.log([...set]); // [1, 2, 3]

// 이터러블인 Set 객체는 배열 디스트럭처링 할당의 대상이 될 수 있다.
const [a, ...rest] = [...set];
console.log(a, rest); // 1, [2, 3]
```

- Set 객체는 요소의 순서에 의미 X
- but, Set 객체를 순회하는 순서 = 요소가 추가된 순서

***

### 37.1.8 집합 연산

집합 연산을 수행하는 프로토타입 메서드를 구현해보자 !

### 교집합

: 집합 A & 집합 B의 공통 요소로 구성됨

#### < 방법 1 >

```javascript
Set.prototype.intersection = function (set) {// 함수의 인수로 set 변수를 전달받음
  const result = new Set(); // 빈 Set 객체 생성해서 result라는 상수가 가리키게 함

  for (const value of set) { // set의 value에 대해 아래의 if문을 반복
    /* 2개의 set의 요소가 공통되는 요소이면 교집합의 대상이다.
     여기서 this = 인수로 전달 받은 set 변수가 가리키는 Set 객체
     has 메서드로 해당 value값이 Set 객체에 있는지 확인
     -> true를 반환하면 result가 가리키는 Set 객체의 요소로 해당 value 값을 추가
    */
    if (this.has(value)) result.add(value);
  }

  return result; // result가 가리키는 Set 객체 리턴
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 교집합
console.log(setA.intersection(setB)); // Set(2) {2, 4}
// setB와 setA의 교집합
console.log(setB.intersection(setA)); // Set(2) {2, 4}
```

#### < 방법 2 >

```javascript
Set.prototype.intersection = function (set) {
  return new Set([...this].filter(v => set.has(v)));
  // 여기서 this = 함수를 호출한 Set 객체(=.intersection() 앞에 있는 객체)
  // set 변수가 가리키는 Set 객체에 this가 가리키는 Set 객체의 v(value)가 존재해서 true가 리턴되는 요소 v만 배열로 리턴
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 교집합
console.log(setA.intersection(setB)); // Set(2) {2, 4}
// setB와 setA의 교집합
console.log(setB.intersection(setA)); // Set(2) {2, 4}
```

***

### 합집합

- 집합 A & 집합 B의 중복 없는 모든 요소로 구성됨

#### <방법 1 >

```javascript
Set.prototype.union = function (set) {
  // result에 this(union 메서드를 호출한 Set 객체)를 복사
  const result = new Set(this);

  for (const value of set) {
    // 합집합은 2개의 Set 객체(union 메서드를 호출한 Set 객체 & set변수가 가리키는 Set 객체)의 모든 요소로 구성된 집합이다. 중복된 요소는 포함되지 않는다.
    // add 메서드는 중복된 요소를 추가해도 에러 없이 무시돼서 괜찮음
    result.add(value); // result가 가리키는 Set 객체에 value를 요소로 추가함
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 합집합
console.log(setA.union(setB)); // Set(4) {1, 2, 3, 4}
// setB와 setA의 합집합
console.log(setB.union(setA)); // Set(4) {2, 4, 1, 3} 객체에 숫자가 추가된 순서만 다름
```

#### <방법 2 >

```javascript
Set.prototype.union = function (set) {
  return new Set([...this, ...set]);
  // 새로운 Set 객체에 2개의 Set 객체(union 메서드를 호출한 Set 객체 & set변수가 가리키는 Set 객체)의 요소를 모두 집어넣음
  //-> Set 생성자 함수에 인수로 전달받은 이터러블에서 중복된 값은 Set 객체에 요소로 저장 안 됨
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 합집합
console.log(setA.union(setB)); // Set(4) {1, 2, 3, 4}
// setB와 setA의 합집합
console.log(setB.union(setA)); // Set(4) {2, 4, 1, 3}
```

***

### 차집합

- 차집합 A - B : 집합 A엔 존재하지만 집합 B에는 존재하지 않는 요소로 구성됨

#### < 방법1 >

```javascript
Set.prototype.difference = function (set) {
  // this(Set 객체)를 복사
  const result = new Set(this);

  for (const value of set) {
    // 차집합은 어느 한쪽 집합에는 존재하지만 다른 한쪽 집합에는 존재하지 않는 요소로 구성된 집합이다.
    result.delete(value);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA에 대한 setB의 차집합
console.log(setA.difference(setB)); // Set(2) {1, 3}
// setB에 대한 setA의 차집합
console.log(setB.difference(setA)); // Set(0) {}
```

#### < 방법2 >

```javascript
Set.prototype.difference = function (set) {
  return new Set([...this].filter(v => !set.has(v)));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA에 대한 setB의 차집합
console.log(setA.difference(setB)); // Set(2) {1, 3}
// setB에 대한 setA의 차집합
console.log(setB.difference(setA)); // Set(0) {}
```

***

### 부분 집합 & 상위 집합

- 집합 A가 집합 B에 포함되는 경우
-> 집합 A : 부분 집합 (subset)
-> 집합 B : 상위 집합 (superset)

#### < 방법 1>

```javascript
// this(= isSuperset 메서드를 호출한 Set 객체)가 subset(이 가리키는 Set 객체)의 상위 집합인지 확인한다.
Set.prototype.isSuperset = function (subset) {
  for (const value of subset) {
    // superset의 모든 요소가 subset의 모든 요소를 포함하는지 확인
    if (!this.has(value)) return false;
  }

  return true; // 모두 포함할 경우에만 true를 리턴
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA가 setB의 상위 집합인지 확인한다.
console.log(setA.isSuperset(setB)); // true -> setA가 setB의 상위 집합임
// setB가 setA의 상위 집합인지 확인한다.
console.log(setB.isSuperset(setA)); // false
```

#### < 방법 2>

```javascript
// this가 subset의 상위 집합인지 확인한다.
Set.prototype.isSuperset = function (subset) {
  const supersetArr = [...this];
  return [...subset].every(v => supersetArr.includes(v));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA가 setB의 상위 집합인지 확인한다.
console.log(setA.isSuperset(setB)); // true
// setB가 setA의 상위 집합인지 확인한다.
console.log(setB.isSuperset(setA)); // false
```

## 37.2 Map

- Map 객체 : 키(key)와 값(value)의 쌍으로 이루어진 컬렉션
- 객체와 유사 but 차이가 있음

### 객체 vs. Map 객체

구분| 객체| Map 객체
|:----------:|:----------:|:----------:|
key로 사용할 수 있는 값| 문자열 or 심벌 값 | 객체를 포함한 모든 값
이터러블| X| O
요소 개수 확인| Object.keys(obj).length| map.size

***

### 37.2.1 Map 객체의 생성

- **Map 생성자 함수**로 생성함

#### 1. 인수를 전달 X : 빈 Map 객체가 생성됨

```javascript
const map = new Map();
console.log(map); // Map(0) {}
```

#### 2. 이터러블을 인수로 전달받아 Set 객체 생성

- 이때 이터러블은 key & value가 쌍으로 이루어진 요소로 구성돼야 함

```javascript
const map1 = new Map([['key1', 'value1'], ['key2', 'value2']]);
console.log(map1); // Map(2) {"key1" => "value1", "key2" => "value2"}

// 키 & 값 쌍으로 안 넣은 경우
const map2 = new Map([1, 2]); // TypeError: Iterator value 1 is not an entry object
```

- 중복된 key를 갖는 요소 존재 -> value가 덮어써짐
=> Map 객체엔 중복된 키를 갖는 요소 존재할 수 X

```javascript
const map = new Map([['key1', 'value1'], ['key1', 'value2']]);
console.log(map); // Map(1) {"key1" => "value2"}
// 값이 'value2'로 덮어써짐
```

***

### 37.2.2 요소 개수 확인

- **Map.prototype.size 프로퍼티** 사용

```javascript
const { size } = new Map([['key1', 'value1'], ['key2', 'value2']]);
console.log(size); // 2
```

- size 프로퍼티 - setter 함수 없이 getter 함수만 존재하는 **접근자 프로퍼티**임
=> size 프로퍼티에 숫자 할당해서 Set 객체의 요소 개수 변경 불가능

```javascript
const map = new Map([['key1', 'value1'], ['key2', 'value2']]);

console.log(Object.getOwnPropertyDescriptor(Map.prototype, 'size'));
// {set: undefined, enumerable: false, configurable: true, get: ƒ}

map.size = 10; // 무시된다.
console.log(map.size); // 2
```

***

### 37.2.3 요소 추가

- Map 객체에 요소 추가할 땐 **Map.prototype.set 메서드** 사용

```javascript
const map = new Map();
console.log(map); // Map(0) {}

map.set('key1', 'value1');
console.log(map); // Map(1) {"key1" => "value1"}
```

- set 메서드 : 새로운 요소가 추가된 Map 객체 반환함
-> set 메서드를 연속적으로 호출 가능

```javascript
const map = new Map();

map
  .set('key1', 'value1')
  .set('key2', 'value2');

console.log(map); // Map(2) {"key1" => "value1", "key2" => "value2"}
```

- Map 객체에 중복된 key를 갖는 요소 존재할 수 X
-> 중복된 key를 갖는 요소 추가하면 에러 없이 value가 덮어써짐

```javascript
const map = new Map();

map
  .set('key1', 'value1')
  .set('key1', 'value2');

console.log(map); // Map(1) {"key1" => "value2"}
```

- Map 객체는 NaN과 NaN, +0과 -0은 같다고 평가 -> 중복 추가 허용 X
*) 일치 비교 연산자 (===)와의 차이 : (===)는 NaN과 NaN은 다름, +0과 -0은 같다고 평가

```javascript
const map = new Map();

console.log(NaN === NaN); // false
console.log(0 === -0); // true

// NaN과 NaN을 같다고 평가하여 중복 추가를 허용하지 않는다.
map.set(NaN, 'value1').set(NaN, 'value2');
console.log(map); // Map(1) { NaN => 'value2' }

// +0과 -0을 같다고 평가하여 중복 추가를 허용하지 않는다.
map.set(0, 'value1').set(-0, 'value2');
console.log(map); // Map(2) { NaN => 'value2', 0 => 'value2' }
```

- Map 객체는 key 타입에 제한 없음 -> 객체를 포함한 모든 값을 key로 사용 가능
(일반 객체와의 차이점임)

```javascript
const map = new Map();

const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

// 객체도 키로 사용할 수 있다.
map
  .set(lee, 'developer')
  .set(kim, 'designer');

console.log(map);
// Map(2) { {name: "Lee"} => "developer", {name: "Kim"} => "designer" }
```

***

### 37.2.4 요소 취득 - Set 객체와 다른 점

- Map 객체에서 특정 요소 취득할 땐 **Map.prototype.get 메서드** 사용

```javascript
const map = new Map();

const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

map
  .set(lee, 'developer')
  .set(kim, 'designer');

console.log(map.get(lee)); // developer
console.log(map.get('key')); // undefined
```

***

### 37.2.5 요소 존재 여부 확인

- Map 객체에 특정 요소가 존재하는지 확인할 땐 **Map.prototype.has 메서드** 사용
- has 메서드 : 특정 요소의 존재 여부 나타내는 불리언 값 반환

```javascript
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

console.log(map.has(lee)); // lee가 있으므로 true
console.log(map.has('key')); // 'key'가 없으므로 false
```

***

### 37.2.6 요소 삭제

- Map 객체에 특정 요소를 삭제할 땐 **Map.prototype.delete 메서드** 사용
- delete 메서드 : 삭제 성공 여부 나타내는 불리언 값 반환

```javascript
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

// kim이라는 key 값을 가진 key-value 쌍을 삭제
map.delete(kim);
console.log(map); // Map(1) { {name: "Lee"} => "developer" }
```

- 존재하지 않는 key로 Map 객체의 요소를 삭제하려 하면 에러 없이 무시됨

```javascript
const map = new Map([['key1', 'value1']]);

// 존재하지 않는 키 'key2'로 요소를 삭제하려 하면 에러없이 무시된다.
map.delete('key2');
console.log(map); // Map(1) {"key1" => "value1"}
```

- delete 메서드는 삭제 성공 여부 나타내는 불리언 값 반환하므로(set 메서드처럼 Map 객체를 반환하는게 아니라서) 연속적으로 호출 불가능

```javascript
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

map.delete(lee).delete(kim); // TypeError: map.delete(...).delete is not a function
```

***

### 37.2.7 요소 일괄 삭제

- **Map.prototype.clear 메서드** 사용
- clear 메서드 : 항상 `undefined` 반환함

```javascript
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

map.clear();
console.log(map); // Map(0) {}
```

***

### 37.2.8 요소 순회

- Map.prototype.forEach 메서드 사용
: Array.prototype.forEach 메서드와 유사하게 콜백 함수 & forEach 메서드의 콜백 함수 내부에서 this로 사용될 객체(옵션)을 인수로 전달함

> 콜백 함수는 3개의 인수 전달받음
>
>- 1번째 인수 : 현재 순회 중인 요소값(value)
>- 2번째 인수 : 현재 순회 중인 요소키(key)
>- 3번째 인수 : 현재 순회 중인 Map 객체 자체

```javascript
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

map.forEach((v, k, map) => console.log(v, k, map));
/*
developer {name: "Lee"} Map(2) {
  {name: "Lee"} => "developer",
  {name: "Kim"} => "designer"
}
designer {name: "Kim"} Map(2) {
  {name: "Lee"} => "developer",
  {name: "Kim"} => "designer"
}
*/
```

- Map 객체 - 이터러블임
-> `for...of`문으로 순회 가능
-> 스프레드 문법 & 배열 디스트럭처링 할당의 대상이 될 수 있음

```javascript
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

// Map 객체는 Map.prototype의 Symbol.iterator 메서드를 상속받는 이터러블이다.
console.log(Symbol.iterator in map); // true

// 이터러블인 Map 객체는 for...of 문으로 순회할 수 있다.
for (const entry of map) {
  console.log(entry); // [{name: "Lee"}, "developer"]  [{name: "Kim"}, "designer"]
}

// 이터러블인 Map 객체는 스프레드 문법의 대상이 될 수 있다.
console.log([...map]);
// [[{name: "Lee"}, "developer"], [{name: "Kim"}, "designer"]]

// 이터러블인 Map 객체는 배열 디스트럭처링 할당의 대상이 될 수 있다.
const [a, b] = map;
console.log(a, b); // [{name: "Lee"}, "developer"]  [{name: "Kim"}, "designer"]
```

- Map 객체 : 이터러블이면서 이터레이터인 객체를 반환하는 메서드를 제공
*) 34.6.3절 참고...

Map 메서드 | 설명
|:----------:|:----------:|
Map.prototype.keys| Map 객체에서 요소키를 값으로 갖는 이터러블이면서 이터레이터인 객체를 반환
Map.prototype.values| Map 객체에서 요소값을 값으로 갖는 이터러블이면서 이터레이터인 객체를 반환
Map.prototype.entries| Map 객체에서 요소키와 요소값을 값으로 갖는 이터러블이면서 이터레이터인 객체를 반환

```javascript
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

// Map.prototype.keys는 Map 객체에서 요소키를 값으로 갖는 이터레이터를 반환한다.
for (const key of map.keys()) {
  console.log(key); // {name: "Lee"} {name: "Kim"}
}

// Map.prototype.values는 Map 객체에서 요소값을 값으로 갖는 이터레이터를 반환한다.
for (const value of map.values()) {
  console.log(value); // developer designer
}

// Map.prototype.entries는 Map 객체에서 요소키와 요소값을 값으로 갖는 이터레이터를 반환한다.
for (const entry of map.entries()) {
  console.log(entry); // [{name: "Lee"}, "developer"]  [{name: "Kim"}, "designer"]
}
```

- Map 객체는 요소의 순서에 의미 X
- but, Map 객체를 순회하는 순서 = 요소가 추가된 순서
