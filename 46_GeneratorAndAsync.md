# 46장. 제너레이터와 async/await

## 46.1 제너레이터란?

---

ES6에 도입된 제네레이터는 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 독특한 함수입니다.

제너레이터와 일반 함수의 차이는 다음과 같다.

- 제너레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도할 수 있다. 이는 함수의 제어권을 함수가 독점하는 것이 아니라 함수 호출자에게 양도(yeild)할 수 있다는 것을 의미한다.

- 제너레이터 함수는 함수 호출자와 함수의 상태를 주고받을 수 있다.

- 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다. 제너레이터 함수를 호출하면 함수 코드를 실행하는 것이 아니라 이터러블이면서 동시에 이터레이터인 제너레이터 객체를 반환한다.

## 46.2 제너레이터 함수의 정의

---

제너레이터 함수는 function* 키워드로 선언한다. 그리고 하나 이상의 yield 표현식을 포함한다.

제너레이터 함수 생성 방식은 다음과 같다.

```javascript
function* genDeFunc() {
    yield 1;
} // 제너레이터 함수 선언문

const genExpFunc = function () {
    yield 1;
} // 제너레이터 함수 표현식

const obj = {
    * genObjMethod() {
        yield 1;
    }
} // 제너레이터 메서드

class MyClass {
    * genClsMethod() {
        yield 1;
    }
} // 제너레이터 클래스 메서드
```

제너레이터 함수를 생성할 수 없는 방식은 다음과 같다.

```javascript
const genArrowFunc = * () => {
    yield 1;
} // 화살표 함수로 정의 불가능

function* genFunc() {
    yield 1;
}

new genFunc(); // 생성자 함수로 호출 불가능
```

## 46.3 제너레이터 객체

---

제너레이터 함수를 호출하면 일반 함수처럼 함수 코드 블록을 실행하는 것이 아니라 제너레이터 객체를 생성해 반환한다. 제너레이터 함수가 반환한 제너레이터 객체는 이터러블이면서도 동시에 이터레이터이다.

다시 말해, 제너레이터 객체는 symbol.iterator 메서드를 상속받는 이터러블이면서 value,done 프로퍼티를 갖는 이터레이터 결과값 객체를 반환하는 next 메서드를 소유한다는 것이다.

```javascript
function* genFunc() {
    yield 1;
    yield 2;
    yield 3;
}

const generator = genFunc();
// 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.

console.log(Symbol.iterator in generator) // true
// 이터러블은 symbol.iterator 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체다.
console.log('next' in generator) // true
// 이터레이터는 next 메서드를 갖는다.
```

제너레이터 객체는 next 메서드를 갖는 이터레이터이지만 이터레이터에는 없는 return, throw 메서드를 갖는다. 제너레이터 객체의 세 개의 메서드를 호출하면 다음과 같이 동작한다.

- next 메서드를 호출하면 제너레이터 함수의 yield 표현식까지 코드 블록을 실행하고 yield된 값을 value 프로퍼티 값으로, false를 done 프로퍼티 값으로 갖는 이터레이터 결과값 객체를 반환한다.

- return 메서드를 호출하면 인수로 전달받은 값을 value 프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 결과값 객체를 반환한다.

- throw 메서드를 호출하면 인수로 전달받은 에러를 발생시키고 undefined를 value 프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 결과값 객체를 반환한다.

```javascript
function* genFunc() {
    try {
        yield 1;
        yield 2;
        yield 3;
    } catch (e) {
        console.error(e);
    }
}

const generator = genFunc();

console.log(generator.next());
// {value: 1, done: false}

console.log(generator.return('End!'));
// {value: "End!", done" true}

console.log(generator.throw('Error!'));
// {value: undefined, done" true}
```

## 46.4 제너레이터의 일시 중지와 재개

---

제너레이터는 yield 키워드와 next 메서드를 통해 실행을 일시 중지했다가 필요한 시점에 다시 재개할 수 있다.

제너레이터 함수를 호출하면 제너레이터 함수의 코드 블록이 실행되는 것이 아니라 제너레이터 객체를 반환한다고 했다.

yield 키워드는 제너레이터 함수의 실행을 일시 중지시키거나 yield 키워드 뒤에 오는 표현식의 평가 결과를 제너레이터 함수 호출자에게 반환한다.

```javascript
function* genFunc() {
    yield 1;
    yield 2;
    yield 3;
}

const generator = genFunc();
// 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.
// 이터러블이면서 동시에 이터레이터인 제너레이터 객체는 next 메서드를 갖게 된다.

console.log(generator.next());// {value: 1, done: false}
// 처음 next 메서드를 호출하면 첫 번째 yield 표현식까지 실행되고 일시 중지된다.
// next 메서드는 이터레이터 결과값 객체({value, done})을 반환한다.
// value 프로퍼티에서는 첫 번째 yield 표현식에서 yield된 값 1이 할당된다.
// done 프로퍼티에서는 제너레이터 함수가 끝까지 실행되었는지를 나타내는 false 값이 할당된다.

console.log(generator.next());// {value: 2, done: false}

console.log(generator.next());// {value: 3, done: false}

console.log(generator.next());// {value: undefined, done: true}
// next 메서드를 호출하면 남은 yield 값이 없으므로 제너레이터 함수의 마지막까지 실행되고 value 값으로 undefined가 할당된다.
// done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었음을 나타내는 true 값이 할당된다.
```

```javascript
function* genFunc() {
    const x = yield 1;
    const y = yield (x + 10);
    return x + y;
}

const generator = genFunc(0);
// 처음 호출하는 next 메서드에는 인수를 전달하지 못하며, 만약 처음 호출하는 next 메서드에 인수를 전달하면 무시된다.

let res = generator.next();
console.log(res); // {value: 1, done: false}

let res = generator.next(10);
console.log(res); // {value: 20, done: false}

let res = generator.next();
console.log(res); // {value: 30, done: true}
```

## 46.5 제너레이터의 활동

---

### 46.5.1 이터러블의 구현

제너레이터 함수를 사용하면 이터레이션 프로토콜을 준수해 이터러블을 생성하는 방식보다 간단히 이터러블을 구현할 수 있다.

```javascript
const infiniterFibonacci = (function () {
    let [pre, cur] = [0, 1];

    retrun {
        [Symbol.iterator]() {return this;},
        next() {
            [pre, cur] = [cur, pre + cur];
            // 무한 이터러블이므로 done 프로퍼티를 생략한다.
            return {value:cur};
        }
    }
})();
// 무한 이터러블을 생성하는 함수

// infiniterFibonacci는 무한 이터러블이다.
for (const num of infiniterFibonacci) {
    if (num > 10000) break;
    console.log(num); // 1 2 3 5 8 ... 2584 4181 6765
}
```

```javascript
const infiniterFibonacci = (function* () {
    let [pre, cur] = [0, 1];

    while (true) {
        [pre, cur] = [cur, pre + cur];
        yield cur;
    }
})();
// 무한 이터러블을 생성하는 제너레이터 함수

// infiniterFibonacci는 무한 이터러블이다.
for (const num of infiniterFibonacci) {
    if (num > 10000) break;
    console.log(num); // 1 2 3 5 8 ... 2584 4181 6765
}
```

### 46.5.2 비동기 처리

제너레이터 함수는 next 메서드와 yield 표현식을 통해 함수 호출자와 함수의 상태를 주고받을 수 있다. 이러한 특성을 활용하면 프로미스를 사용한 비동기 처리를 동기 처리처럼 구현할 수 있다. 다시 말해, 프로미스의 후속 처리 메서드 then/catch/finally 없이 비동기 처리 결과를 반환하도록 구현할 수 있다.

```javascript
const fetch = require('node-fectch');

const async = generator => {
    const generator = genFunc();

    const onResulved = arg => {
        const result = generator.next(arg);

        return result.done
        ? result.value
        : result.value.then(res => onResulved(res))
    }
    return onResulved;
}
 (async(function* fetchTodo() {
    const url = 'http://jsonplaceholder.typecode.com/todos/1';

    const response = yield fetch(url);
    const todo = yield response.json();
    console.log(todo);
 })())
```

## 46.6 async/await

---

제너레이터를 통해 비동기적 처리가 가능하지만, 코드가 너무 장황하고 가독성이 좋지 않다. 그렇기에 ES8에서는 제너레이터보다 쉽고 가동성 좋게 비동기 처리를 동기 처리처럼 할 수 있는 코드인 async/await이 나타났다.

async/await은 promise 기반으로 동작한다. 하지만 promise의 후속 처리 메서드 없이 마치 동기 처리처럼 promise가 처리 결과를 반환하도록 구현할 수 있다.

```javascript
const fetch = require('node-fectch');

async function fetchTodo() {
    const url = 'http://jsonplaceholder.typecode.com/todos/1';

    const response = await fetch(url);
    const todo = await response.json();
    console.log(todo);
}

fetchTodo();
```

### 46.6.1 async 함수

await 키워드는 반드시 async가 존재하는 함수 내에서 사용해야만 한다. async 함수는 async 키워드를 사용해 정의하며 언제나 promise를 반환한다.

async 함수는 다음과 같이 생성할 수 있다.

```javascript
async function foo(n) {return n;}
foo(1).then(v => console.log(v)); // 1

const bar = async function(n) {return n;}
bar(2).then(v => console.log(v)); // 2

const baz = async n => n;
baz(3).then(v => console.log(v)); // 3

const obj = {
    async foo(n) { return n }
}
obj.foo(4).then(v => console.log(v)); // 4

class MyClass = {
    async bar(n) { return n }
}
const myClass = new MyClass();
myClass.bar(5).then(v => console.log(v)); // 5
```

여기서 주의해야할 점은 class 함수의 constructor 메서드는 인스턴스를 반환해야 하기 때문에, promise를 반환해야하는 async 함수를 만들 수 없다.

### 46.6.2 await 키워드

await 키워드는 promise가 settled 상태(비동기 처리가 수행된 상태)가 될 때까지 대기하다가 settled 상태가 되면 promise가 resolve한 처리 결과를 반환한다.

또한, await 키워드는 반드시 promise 앞에서 사용해아 한다.

```javascript
const fetch = require('node-fectch');

const getGithubUserName =  async id => {
    const url = await fetch('http://api.github.com/${id}'); // 1번
    const { name } = await res.json();
    console.log(name); // ungmo Lee
}

getGithubUserName('ungmo2')
```

await 키워드는 promise가 settled 될 때까지 대기한다 했다. 따라서 1번의 fetch 함수가 수행한 HTTP 요청에 대한 서버의 응답이 도착해서 fetch 함수가 반환한 promise가 settled 상태가 될 때까지 1번은 대기하게 된다. 이후 promise가 settled 상태가 되면 promise가 resolve한 처리 결과가 res 변수에 할당된다.

이처럼 await 키워드는 다음 실행을 일시 중지시켰다가 promise가 settled 상태가 되면 다시 재개한다. 다음 예제를 살펴보자.

```javascript
async function foo() {
    const a = await new Promise (resolve => setTimeout(() => resolve(1), 3000))
    const b = await new Promise (resolve => setTimeout(() => resolve(2), 2000))
    const c = await new Promise (resolve => setTimeout(() => resolve(3), 1000))

    console.log([a, b, c]) // [1, 2, 3]
}

foo(); // 6초 소요
```

모든 promise에 await 키워드를 사용하는 것을 주의해야 한다. 위 예제의 foo 함수는 종료될 때까지 6초가 소요된다. 첫 번째 promise가 settled 상태가 되기까지 3초, 두 번째 promise가 settled 상태가 되기까지 2초, 세 번째 promise가 settled 상태가 되기까지 1초가 걸린다.

그런데 foo 함수가 수행하는 3개의 비동기 처리는 서로 연관이 없다. 그러기에 앞의 비동기 처리가 수행될 때까지 기다릴 필요 없이 개별적으로 비동기 처리를 진행해도 문제가 없다.
그러므로 다음과 같은 코드로 개별적인 비동기 처리를 진행하는 것이 좋다.

```javascript
async function foo() {
    const res = await Promise.all([
    new promise (resolve => setTimeout(() => resolve(1), 3000))
    new promise (resolve => setTimeout(() => resolve(2), 2000))
    new promise (resolve => setTimeout(() => resolve(3), 1000))
  ])
    console.log(res) // [1, 2, 3]
}

foo(); // 3초 소요
```

다음과 같은 연쇄적 비동기 처리로 순서가 보장되어야 할 때에만 모든 promise에 await 키워드를 붙여야 한다.

```javascript
async function foo() {
    const a = await new Promise (resolve => setTimeout(() => resolve(n), 3000))
    const b = await new Promise (resolve => setTimeout(() => resolve(a + 1), 2000))
    const c = await new Promise (resolve => setTimeout(() => resolve(b +2), 1000))

    console.log([a, b, c]) // [1, 2, 3]
}

foo(); // 6초 소요
```

### 46.6.3 에러 처리

async/await의 에러 처리는 try/catch문을 사용할 수 있다. 콜백 함수를 인수로 전달받는 비동기 함수와는 달리 promise를 반환하는 비동기 함수는 명시적으로 호출 할 수 있기 떄문에 호출자가 명확한다.

```javascript
const fetch = require('node-fetch');

const foo = async () => {
    try {
        const wrongUrl = 'htpps://wrong.url';

        const response = await fetch(wrongUrl);
        const data = await response.json();
        console.log(data);
    } catch (err) {
        console.error(err);
    }
}

foo();
```

위 예제의 foo 함수의 catch 문은 HTTP 통신에서 발생한 네트워크 에러뿐 아니라 try 코드 블록 내의 모든 문에서 발생한 일반적인 에러까지 모두 캐치할 수 있다.

```javascript
const fetch = require('node-fetch');

const foo = async () => {
        const wrongUrl = 'htpps://wrong.url';

        const response = await fetch(wrongUrl);
        const data = await response.json();
        return datal
}

foo()
    .then(console.log)
    .catch(console.error);
```

async 함수 내에서 catch 문을 사용해서 에러 처리를 하지 않으면 async 함수는 발생한 에러를 reject하는 promise를 반환한다. 따라서 async 함수를 호출하고 Promise.prototype.catch 후속 처리 메서드를 사용해 에러를 캐치할 수도 있다.
