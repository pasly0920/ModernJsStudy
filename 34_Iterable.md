# 34장 이터러블

## 34.1 이터레이션 프로토콜 Iteration Protocol

ES6에서 도입된 **이터레이션 프로토콜**은 순회 가능한, 즉 이터러블한 데이터 컬렉션을 만들기 위해 미리 약속한 규칙이다. (ECMA Script 사양에서)

ES6 전 : 순회 가능한 데이터 컬렉션, 예를 들면 배열, 문자열, 유사 배열 객체, DOM 객체 등은 통일된 규약 없이 각자의 구조를 가지고 순회할 수 있었다.

ES6 이후 : 순회 가능한 데이터 컬렉션을 이터레이션 프로토콜을 준수하는 이터러블로 통일하였다.

------

### 34.1.1 이터러블

#### 이터러블 프로토콜 Iteration Protocol

Well-known Symbol인 Symbol.iterator을 프로퍼티 키로 사용한 메서드를 직접 구현하거나,프로토타입 체인을 통해 상속 받은 Symbol.iterator 메서드를 호출하면 이터러블 프로토콜을 준수한 이터레이터를 반환한다.

**이러한 규약을 이터러블 프로토콜이라 하며, 이터러블 프로토콜을 준수한 객체를 이터러블**이라고 한다.

이터러블인지는 다음과 같이 확인할 수 있다.

```javascript
const isIterable = v => v == null && typeof v[Symbol.iterator] ==='fuction';

// false를 고르시오. (1개)
// 배열, 문자열, Map, Set은 이터러블이다.
isIterable([]);
isIterable('');
isIterable(new Map());
isIterable(new Set());
isIterable({});
```

**이터러블은 for...of 문으로 순회할 수 있으며 스프레드 문법과 배열 디스터럭처링 할당의 대상으로 사용할 수 있다.**

```javascript
const array = [1, 2, 3];

// 배열은 Array.prototype의 Symbol.iterator 메서드를 상속받는 이터러블이다.
console.log(Symbol.iterator in array); // true

// 이터러블인 배열은 for...of 문으로 순회 가능하다.
for (const item of array) {
    console.log(item);
}

// 이터러블인 배열은 스프레드 문법의 대상으로 사용할 수 있다.
console.log([...array]); // [1, 2, 3]

// 이터러블인 배열은 배열 디스트럭처링 할당의 대상으로 사용할 수 있다.
const [a, ...rest] = array;
console.log(a, rest); // 1, [2, 3]
```

이터러블 프로토콜을 준수하지 않은 일반 객체는 위의 네 가지 경우에 모두 사용되지 않는다.

```javascript
const obj = [a :1, b: 2];

console.log(Symbol.iterator in obj); // false

for (const item of obj) { // TypeError
    console.log(item);
}

const [a, b] = obj; // TypeError

// 단, 스프레트 프로퍼티 제안(stage 4)에서는 객체 리터럴 내부에서 스프레드 문법의 사용을 허용한다.
const.log({...obj}); // {a :1, b: 2}
```

하지만 일반 객체도 이터러블 프로토콜을 준수하도록 구현할 수 있다. 이에 대해서는 34.6절에서 알아보자.

### 34.1.2 이터레이터

#### 이터러이터 프로토콜 Iteration Protocol

이터러블의 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환한다.

이터레이터는 next 메서드를 소유하며 next 메서드를 호출하면 이터러블을 순회하며 value와 done 프로퍼티를 갖는 **이터레이터 리절트 객체**를 반환한다. value 프로퍼티는 현재 순회 중인 이터러블의 값을 나타내며, done 프로퍼티는 이터러블의 순회 완료 여부를 나타낸다.

**이러한 규약을 이터레이터 프로토콜이라 하며, 이터레이터 프로토콜을 준수한 객체를 이터레이터**라 한다. 이터레이터는 이터러블의 요소를 탐색하기 위한 포인터 역할을 한다.

다음의 코드로 확인해보자.

```javascript
// 배열은 이터러블 프로토콜을 준수한 이터러블이다.
const array = [1, 2, 3];

// Symbol.iterator 메서드는 이터레이터를 반환한다.
const itertator = array[Symbol.iterator]();

// Symbol.iterator 메서드가 반환한 이터레이터는 next 메서드를 갖는다.
console.log('next' in iterator); // true

// next 메서드를 호출하면 이터러블을 순회하여 이터레이터 리절트 객체를 반환한다.
// 이터레이터 리절트 객체는 value와 done 프로퍼티를 갖는 객체다.
console.log(iterator.next()); // { value: 1, done: false}
console.log(iterator.next()); // { value: 2, done: false}
console.log(iterator.next()); // { value: 3, done: false}
console.log(iterator.next()); // { value: 4, done: true}
```

## 34.2 빌트인 이터러블

자바스크립트는 이터레이션 프로토콜을 준수한 객체인 빌트인 이터러블을 제공한다. 다음의 표준 빌트인 객체들은 빌트인 이터러블이다.

|빌트인 이터러블|Symbol.iterator 메서드|
|------|------|
|Array|Array.prototype[Symbol.iterator]|
|String|String.prototype[Symbol.iterator]|
|Map|Map.prototype[Symbol.iterator]|
|Set|Set.prototype[Symbol.iterator]|
|TypedArray|TypedArray.prototype[Symbol.iterator]|
|arguments|arguments[Symbol.iterator]|
|DOM 컬렉션|NodeList.prototype[Symbol.iterator]|
||HTMLCollection.prototype[Symbol.iterator]|

------

## 34.3 for...of 문

for...of 문은 이터러블을 순회하면서 이터러블의 요소를 변수에 할당한다. for...of 문의 문법은 다음과 같다.

```javascript
for (변수선언문 of 이터러블) { ... }
```

for...of 문은 내부적으로 이터레이터의 next 메서드를 호출하여 이터러블을 순회하며 next 메서드가 반환한 이터레이터 리절트 객체의 프로퍼티 값을 for...of 문의 변수에 할당한다.

그리고 이터레이터 리절트 객체의 done 프로퍼티 값이 false이면 이터러블의 순회를 계속하고 true 이면 순회를 중단한다.

```javascript
for (const item of [1, 2, 3]) {
    // item 변수에 순차적으로 1, 2, 3 할당
    console.log(item); // 1 2 3
}
```

위의 내부동작을 for 문으로 표현하면 다음과 같다.

```javascript
const iterable = [1, 2, 3]; // 이터러블

// 이터러블의 Symbol.iterator 메서드 호출, 이터레이터 생성
const iterator = iterable[Symbol.iterator]();

for (;;) {
    // 이터레이터의 next 메서드를 호출하여 이터러블 순회
    // 이때 next 메섯드는 이터레이터 리절트 객체 반환
    const res = iterator.next();

    // next 메서드가 반환한 이터레이터 리절트 객체의 done 프로퍼티 값이 true면 순회 중단
    if (res.done) break;

    // 이터레이터 리털트 객체의 value 프로퍼티 값을 item 변수에 할당
    const item = res.value;
    console.log(item); // 1 2 3
}
```

for...of 문은 for...in 문의 형식과 매우 유사하다.

```javascript
for (변수선언문 in 객체) { ... }
```

for...in 문은 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 true인 프로퍼티를 순회하며 열거(enumeration)한다. 이떄 프로퍼티 키가 심벌인 프로퍼티는 열거하지 않는다.

------

## 34.4 이터러블과 유사 배열 객체

유사 배열 객체는 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있고, length 프로퍼티를 갖기 때문에 for 문으로 순회할 수 있는 객체를 말한다.

```javascript
// 유사 배열 객체
const arrayLike = {
    0: 1,
    1: 2,
    2: 3,
    length: 3
};

// length 프로퍼티 활용해서 for 문으로 순회가능
for (let i = 0; i < arrayLike.length; i++) {
    // 유사 배열 객체는 배열 처럼 인덱스로 프로퍼티 값 접근 가능
    console.log(arratLike[i]); // 1 2 3
}
```

유사 배열 객체는 이터러블이 아닌 일반 객체이다. 즉 유사 배열 객체에는 Symbol.iterator 메서드가 없고, for...of 문으로 순회할 수 없다.

```javascript
for (const item of arrayLike) {
    console.log(item); // 1 2 3
} // TypeError: arrayLike is not iterable
```

단, arguments, NodeList, HTMLCollection은 유사 배열 객체면서 동시에 이터러블 하다.

ES6에서 이터러블이 도입되면서 이 유사 배열 객체들에 Symbol.iterator 메서드를 구현하였다.

하지만 이 객체들은 이터러블이 된 후에도 length 프로퍼티를 가지며, 인덱스로 접근할 수 있으므로 유사 배열 객체이기도 한 것이다. 배열도 같은 과정을 거쳐 이터러블이 되었다.

단, 모든 유사 배열 객체가 이터러블인 것은 아니다. 위 예제의 arrayLike의 경우 이터러블이 아니다. 이러한 객체를 이터러블로 변환하려면 Array.from 메서드를 활용하면 된다.

```javascript
// Array.from은 유사 배열 객체 또는 이터러블을 배열로 변환한다.
const arr = Array.from(arrayLike);
console.log(arr); // [1, 2, 3]
```

------

## 34.5 이터레이션 프로토콜의 필요성

앞서 말했다싶이, ES6에서 순회 가능한 데이터 컬렉션들이 이터레이션 프로토콜을 준수하는 이터러블로 통일되었다.

for...of 문, 스프레드 문법, 배열 디스트럭처링 할당 등은 다양한 이터러블 객체를 사용할 수 있으며, 데이터 소비자(data consumer)라고 할 수 있다. 반대로 이터러블은 데이터 공급자(data producter)의 역할을 한다고 볼 수 있는데, 그 이유에 대해 알아보자.

만약 데이터 공급자가 통일되지 않은 순회 방식을 갖는다면, 데이터 소비자는 다양한 순회 방식을 모두 지원해야 하며, 이는 효율적이지 않다.

하지만 다양한 데이터 공급자가 이터레이션 프로토콜을 준수하도록 규정하면, 데이터 소비자는 이터레이션 프로토콜만 지원하도록 구현하면 된다.

1. 이터러블을 지원하는 데이터 소비자는 내부에서 Symbol.iterator 메서드를 호출해 이터레이터를 생성
2. 이터레이터의 next 메서드를 호출하여 이터러블을 순회하며 이터레이터 리절트 객체 반환
3. 이터레이터 리절트 객체의 value/done 프로퍼티 값 취득

따라서, 이터레이션 프로토콜은 데이터 소비자와 데이터 공급자를 연결하는 인터페이스 역할을 하게 된다.

------

## 34.6 사용자 정의 이터러블

### 34.6.1 사용자 정의 이터러블 구현

이터레이션 프로토콜을 준수하지 않는 일반 객체도 이터레이션 프로토콜을 준수하도록 구현하면 사용자 정의 이터러블이 된다. 간단한 사용자 정의 이터러블을 구현해보자.

```javascript
/// 피보나치 수열을 구현한 사용자 정의 이터러블
const fibonacci = {
    // Symbol.iterator 메서드를 구현하여 이터러블 프로토콜을 준수한다.
    [Symbol.iterator]() {
        let [pre, cur] = [0, 1];
        const max = 10; // 수열 최대값 지정

        // Symbol.iterator 메서드는 next 메서드를 소유한 이터레이터 반환
        // next 메서드는 done과 value 프로퍼티 갖는 이터레이터 리절트 객체 반환
        return {
            next () {
                [pre, cur] = [cur, pre + cur];
                return { value: cur, done: cur >= max };
            }
        };
    }
};

// 이터러블인 fibonacci 객체를 순회할 때마다 next 메서드 호출
for (const num of fibonacci) {
    console.log(num); // 1 2 3 5 8
}
```

이터러블은 for...of 문 외에도 스프레드 문법, 배열 디스트럭처링 할당에도 사용될 수 있다.

```javascript
const arr = [...fibonacci];
console.log(arr); // [1, 2, 3, 5, 8 ]

const [first, second, ...rest] = fibonacci;
console.log(first, second, rest); // 1 2 [3, 5, 8 ]
```

------

### 34.6.2 이터러블을 생성하는 함수

앞에서 살펴본 fibonacci 이터러블은 내부에 수열의 최대값을 가지고 있다. 이 수열의 최대값은 고정된 값이기 때문에, 외부에서 전달할 수 있도록 수정해보자. 수열의 최대값을 인수로 전달받아 이터러블을 반환하는 함수를 만들면 된다.

```javascript
// 수열의 최대값 인수로 전달
const fibonacciFunc = function (max) {
    let [pre, cur] = [0, 1];

    return {
        [Sumbol.iterator]() {
            return {
                next() {
                    [pre, cur] = [cur, pre + cur];
                    return { value: cur, done: cur >= max };
                }
            };
        }
    };
};

// 수열의 최대값을 인수로 전달하며 함수 호출
for (const num of fibonacciFunc(10)) {
    console.log(num); // 1 2 3 5 8
}
```

------

### 34.6.3 이터러블이면서 이터레이터인 객체를 생성하는 함수

위의 fibonacciFunc 함수는 이터러블을 반환한다.

만약 이터레이터를 생성하려면 이터러블의 Symbol.iterator 메서드를 호출해야 한다

```javascript
// fibonacciFunc 는 이터러블을 반환한다.
const iterable = fibbonacciFunc(5);
// 이터러블의 Symbol.iterator 메서드는 이터레이터를 반환한다.
const iterator = iterable[Symbol.iterator]();

console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: 3, done: false }
console.log(iterator.next()); // { value: 5, done: true }
```

------

이터러블이면서 이터레이터인 객체를 생성하면 Symbol.iterator 메서드와 next 메서드를 소유하므로 따로 호출하지 않아도 된다.

```javascript
{
    [Symbol.iterator]() { return this; },
    next() {
        return { value: any, done: boolean};
    }
}
```

앞에서 살펴본 fibonacciFunc 함수를 이터러블이면서 이터레이터인 객체를 생성하여 반환하는 함수로 변경해보자.

```javascript
// 이터러블이면서 이터레이터인 객체를 반환하는 함수
const fibonacciFUnc = function (max) {
    let [pre, cur] = [0, 1];

    return {
        [Symbol.iterator]() { return this; },
        next() {
            [pre, cur] = [cur, pre + cur];
            return { value: cur, done: cur >= max };
        }
    };
};

// iter는 이터러블이면서 이터레이터
let iter = fibonacciFunc(10);

// iter는 이터러블이므로 for...of 문으로 순회 가능
for (const num of iter) {
    console.log(num); // 1 2 3 5 8
}

iter = fibonacciFunc(10);

// 이터레이터이므로 이터레이션 리절트 객체 반환하는 next 메서드 소유
console.log(iter.next()); // { value: 1, done: false }
console.log(iter.next()); // { value: 2, done: false }
console.log(iter.next()); // { value: 3, done: false }
console.log(iter.next()); // { value: 5, done: false }
console.log(iter.next()); // { value: 8, done: false }
console.log(iter.next()); // { value: 13, done: true }
```

------

### 34.6.4 무한 이터러블과 지연 평가

무한 이터러블을 생성하는 함수를 통해 무한 수열을 구현할 수 있다.

```javascript
// 무한 이터러블 생성하는 함수
const fibonacciFunc = function () {
    let [pre, cur] = [0, 1];

    return {
        [Symbol.iterator]() { return this; },
        next() {
            [pre, cur] = [cur, pre + cur];
            // 무한 구현해야 하므로 done 프로퍼티 생략
            return { value: cur };
        }
    };
};

// fibonacciFunc 함수는 무한 이터러블을 생성한다.
for (const num of fibonacciFunc()) {
    if (num > 10000) break;
    console.log(num); // 1 2 3 5 8 ...
}

// 배열 디스트럭처러링 할당을 통해 무한 이터러블에서 3개의 요소만 취득
const [f1, f2, f3] = fibonacciFunc();
console.log(f1, f2, f3); // 1 2 3
```

위의 34.5 절에서 언급했다 싶이, 이터러블은 데이터 공급자 역할을 한다. 따라서 배열이나 문자열 등은 모든 데이터를 메모리에 미리 확보한 뒤 다음 데이터를 공급한다.

하지만, 무한 이터러블은 **지연 평가(lazy evaluation)**를 통해 데이터를 생성한다. 지연 평가란 데이터가 필요한 시점이 되면 그제서야 데이터를 생성하는 기법이다. 즉, **평가가 필요할 때까지 평가를 늦추는 기법이 지연평가다.**

위의 경우 이터러블을 순회할 때 내부에서 이터레이터의 next를 메서드를 호출할 때 데이터가 생성된다.

지연 평가를 사용하면 불필요한 데이터를 미리 생성하지 않아 빠른 실행 속도를 기대할 수 있고, 불필요한 메모리를 소비하지 않으며, 무한도 표현할 수 있다는 장점이 있다.
