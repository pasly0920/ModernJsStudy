
# 35 스프레드 문법

- 스프레드 문법(전개 문법) ... : 하나로 뭉쳐 있는 여러 값들의 집합을 펼쳐서 개별적인 값들의 목록을 만드는 것
- 스프레드 문법을 사용할 수 있는 대상: 이터러블(Array, String, Map, Set, arguments, DOM 컬렉션 등)

```js
// ...[1, 2, 3]은 [1, 2, 3]을 개별 요소로 분리한다.
console.log(...[1, 2, 3]); // 1 2 3

// Array는 이터러블이므로 스프레드 문법(...)의 대상이 될 수 있다.
a = [1, 2, 3, 4,];
console.log(...a) // 1 2 3 4

// 문자열은 이터러블이므로 스프레드 문법의 대상이 될 수 있다.
console.log(...'Hello'); // H e l l o

// Map과 Set은 이터러블이므로 스프레드 문법의 대상이 될 수 있다.
console.log(...new Map([['a', '1'], ['b', '2']])); // [ 'a', '1' ] [ 'b', '2' ]
console.log(...new Set([1, 2, 3])); // 1 2 3

// 유사 배열 객체와 같이 이터러블이 아닌 일반 객체는 스프레드 문법의 대상이 될 수 없다.(P)
const arrayLike = {
    0: 1,
    1: 2,
    2: 3,
    length: 3
};

console.log(...arrayLike) // TypeError: Found non-callable @@iterator

// 유사 배열 객체를 이터러블인 배열로 변환하기(S)
const arr = Array.from(arrayLike);
console.log(arr); // [ 1, 2, 3 ]
console.log(...arr) // 1 2 3
```

- 스프레드 문법의 결과는 '값'이 아니라 '개별적인 값들의 목록'이다.
- 따라서 스프레드 문법은 연산자(피연산자를 연산하여 값을 생성하는 것)가 아니다.
- 스프레드 문법은 쉼표로 구분한 값의 목록을 사용하는 문맥에서 사용한다.

1. 함수 호출문의 인수 목록
2. 배열 리터럴의 요소 목록
3. 객체 리터럴의 프로퍼티 목록

```js
// 스프레드 문법의 결과는 값이 아니므로 변수에 할당할 수 없다.
const list = ... [1, 2, 3]; // SyntaxError: Unexpected token '...'
```

# 35.1 함수 호출문의 인수 목록에서 사용하는 경우(스프레드 문법의 활용1)

- 배열을 펼쳐서 개별적인 값들의 목록으로 만든 후 이를 함수의 목록으로 전달해야 하는 경우에, 스프레드 문법을 사용하여 간결한 코드를 작성할 수 있다.
- 예를 들어 Math.max 메서드를 이용해 배열의 요소 중 최대값을 구하는 상황을 생각해보자.

```js
// Math.max 메서드는 여러 개의 숫자를 인수로 전달받아 최대값을 반환한다.(가변 인자 함수)
console.log(Math.max(1)); // 1
console.log(Math.max(1, 2)); // 2
console.log(Math.max(1, 2, 3)); // 3
console.log(Math.max()); // -Infinity

// Math.max 메서드는 인수로 숫자만을 받으며 배열을 받지 않는다.(P)
console.log(Math.max([1, 2, 3])); // NaN


// 이를 해결하려면 배열을 펼쳐서 요소들을 개별적인 값들의 목록으로 만든 후, Math.max 메서드의 인수로 전달해야 한다.


// apply 함수는 2번째 인수로 배열을 전달받아 배열의 펼쳐진 요소들을 반환한다.(S1)
console.log(Math.max.apply(null, [1, 2, 3])); // 3

// 스프레드 문법을 사용하면 더 간결하고 가독성이 좋다.(S2)
console.log(Math.max(...[1, 2, 3])); // 3
```

- 스프레드 문법과 rest 파라미터는 형태가 동일하지만 서로 반대되는 개념이다.
- rest 파라미터: 함수에 전달된 인수들의 목록을 배열로 전달받기 위해 매개변수 이름 앞에 ...을 붙인다.

```js
// rest 파라미터는 인수들의 목록을 배열로 전달받는다.
function foo(...rest) {
    console.log(rest);
}

// 스프레드 문법은 이터러블을 펼쳐서 개별적인 값들의 목록을 만든다.
foo(...[1, 2, 3]); // [ 1, 2, 3 ]
```

# 35.2 배열 리터럴 내부에서 사용하는 경우(스프레드 문법의 활용2)

- 스프레드 문법을 배열 리터럴에서 사용하면 ES5에서 사용하던 기존의 방식보다 더욱 간결한 코드를 작성할 수 있다.
- ES5에서 사용하던 방식과 비교하여 살펴보자.

## 35.2.1 concat

- 2개의 배열을 1개의 배열로 결합하는 경우, 스프레드 문법을 사용하여 간결한 코드를 작성할 수 있다.
- concat 메서드: 합치려고 하는 배열을 인수로 전달받아, 앞에 있는 배열과 인수로 전달받은 배열을 합친 배열을 반환한다.

```js
// 두 배열 [1, 2]과 [3, 4]를 이용하여 배열 [1, 2, 3, 4]를 만들자.


// ES5
// concat 메서드를 사용하여 배열 [1, 2]과 배열 [3, 4]를 합칠 수 있다.
var arr = [1, 2].concat([3, 4]);

console.log(arr); // [ 1, 2, 3, 4 ]


// ES6
// 스프레드 문법을 이용하면 별도의 메서드를 사용하지 않고 배열 리터럴만으로 두 배열을 합칠 수 있다.
const arr = [...[1, 2], ...[3, 4]];

console.log(arr); // [ 1, 2, 3, 4 ]
```

## 35.2.2 splice

- 어떤 배열의 중간에 다른 배열의 요소들을 추가하는 경우, 스프레드 문법을 사용하여 간결한 코드를 작성할 수 있다.
- splice 메서드: 첫 번째 인수로 시작 인덱스, 두 번째 인수로 제거할 요소의 개수, 세 번째 인수로 제거한 위치에 삽입할 요소들을 받으며 원본 배열의 중간에 있는 요소를 제거하고 배열의 중간에 요소를 추가한 배열을 반환한다.

```js
// 두 배열 [1, 4]과 [2, 3]을 이용하여 배열 [1, 2, 3, 4]를 만들자.


// ES5
var arr1 = [1, 4];
var arr2 = [2, 3];

Array.prototype.splice.apply(arr1, [1,0].concat(arr2));
console.log(arr1) // [ 1, 2, 3, 4 ]


// ES6
arr1.splice(1, 0, ...arr2);
console.log(arr1) // [ 1, 2, 3, 4 ]
```

## 35.2.3 배열 복사

- 배열을 복사하는 경우, 스프레드 문법을 사용하여 간결한 코드를 작성할 수 있다.
- slice 메서드: 앞 배열을 복사한 배열을 반환한다.

```js
// 배열 [1, 2]를 복사하자.


// ES5
var origin = [1, 2];
var copy = origin.slice();

console.log(copy); // [ 1, 2 ]
console.log(origin === copy) // false


// ES6
const copy2 = [...origin];

console.log(copy2) // [ 1, 2 ]
console.log(origin === copy2) // false
```

## 35.2.4 이터러블을 배열로 변환

- 이터러블을 배열로 변환하는 경우, 스프레드 문법을 사용하면 간결한 코드를 작성할 수 있다.

```js
// ES5
// Function.prototype.apply 또는 Function.prototype.call 메서드를 사용하여 slice 메서드를 호출하면 이터러블을 배열로 반환할 수 있다.
function sum() {
    var args = Array.prototype.slice.call(arguments);

    return args.reduce(function (pre, cur) {
        return pre + cur;
    }, 0);
}

console.log(sum(1, 2, 3)); // 6


// 이터러블이 아닌 유사 배열 객체도 배열로 마찬가지의 방법으로 배열로 변환할 수 있다.
const arrayLike = {
    0: 1,
    1: 2,
    2: 3,
    length: 3
};

const arr = Array.prototype.slice.call(arrayLike); 
console.log(arr) // [ 1, 2, 3 ]
console.log(Array.isArray(arr)); // true


// ES6 
// 스프레드 문법을 사용하면 더 간결하게 이터러블을 배열로 변환할 수 있다.
function sum() {
    return [ ...arguments].reduce((pre, cur) => pre + cur, 0);
}

console.log(sum(1, 2, 3)); // 6

// rest 파라미터를 이용한 조금 더 간결한 코드
// rest 파라미터 args는 함수에 전달된 인수들의 목록을 배열로 전달받는다.
const sum = (...args) => args.reduce((pre, cur) => pre + cur, 0);

console.log(sum(1, 2, 3)); // 6
```

- 이터러블이 아닌 유사 배열 객체는 스프레드 문법의 대상이 될 수 없다.

```js
const arrayLike = {
    0: 1,
    1: 2,
    2: 3,
    length: 3
};

const arr = [...arrayLike]; // TypeError: arrayLike is not iterable
```

# 35.3 객체 리터럴 내부에서 사용하는 경우(스프레드 문법의 활용3)

```js
const obj = { x: 1, y: 2};
const copy = { ...obj};

console.log(copy); // { x: 1, y: 2 }
console.log(obj === copy); // false

// 객체 병합
const merged = { x: 1, y: 2, ...{ a: 3, b: 4} };
console.log(merged) // { x: 1, y: 2, a: 3, b: 4 }
```

- Object.assign 메서드를 사용하여 여러 개의 객체를 변합하거나 특정 프로퍼티를 변경 또는 추가할 수 있다.

```js
// 객체 병합. 프로퍼티가 중복되는 경우 뒤에 위치한 프로퍼티가 우선권을 갖는다.
const merged = Object.assign({}, { x: 1, y: 2}, { y: 10, z: 3});
console.log(merged); // { x: 1, y: 10, z: 3 }

// 특정 프로퍼티 변경
const changed = Object.assign( {}, { x: 1, y: 2}, { y: 100});
console.log(changed); // { x: 1, y: 100 }

// 프로퍼티 추가
const added = Object.assign( {}, { x: 1, y: 2}, { z: 0});
console.log(added); // { x: 1, y: 2, z: 0 }
```

- 스프레드 프로퍼티는 더 간결하게 여러 객체를 병합하거나 특정 프로퍼티를 변경 또는 추가할 수 있다.

```js
// 객체 병합. 프로퍼티가 중복되는 경우 뒤에 위치한 프로퍼티가 우선권을 갖는다.
const merged = { ... { x: 1, y: 2}, ... { y: 10, z: 3} };
console.log(merged); { x: 1, y: 10, z: 3 };

// 특정 프로퍼티 변경
const changed = { ... { x: 1, y: 2}, y: 100};
console.log(changed); // { x: 1, y: 100 }

// 프로퍼티 추가
const added = { ...{x: 1, y: 2}, z: 0};
console.log(added) // { x: 1, y: 2, z: 0 }
```
