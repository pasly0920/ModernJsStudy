# Ch.45 프로미스

## 45.1 비동기처리 - 콜백함수

비동기 처리는 코드가 완료되지 않아도 기다리지 않고 즉시 종료하는 것이다. 비동기 함수의 내부 비동기 코드는 함수가 종료된 이후에 완료된다.

대표적인 비동기 함수 setTimeout

```javascript
let x = 1;
setTimeout(() => {
  x = 2;
}, 0); // 함수가 종료된 이후 완료
console.log(x); // 1
```

- 이유(실행순서)
  1. setTimeout 실행 컨택스트가 생성되고 콜 스택에 push 후 pop 된다.
  2. 이후 바로 console.log() 가 콜 스텍에 push 된다.
  3. 내부 비동기 함수 x=2는 콜 스텍이 비어있지 않으므로 Task Queue 에서 대기한다.
  4. 이후 console.log()가 pop 되면 x=2 함수가 콜 스텍에 push 후 pop 된다.

http Get 요청은 서버의 응답을 받는 비동기 함수이다. 따라서 비동기 처리를 해야한다.

### 45.1.1 콜백함수로 비동기처리

비동기처리를 하는 첫번째 방법

- 콜백함수를 인자로 넘겨주는 것이다. 그럼 비동기 처리가 끝난 후 handler 함수 실행할 수 있다.

```javascript
let x = 1;
function plus(cb) {
  setTimeout(() => {
    x++;
    cb(x);
  }, 100);
}
plus(console.log);
console.log(x); // 결과는?
```

- 하지만 콜백함수는 심각한 문제가 2개 있는데...

### 45.1.2 콜백함수의 문제

첫번째 문제는, 콜백함수는 여러번 비동기 처리를 할 때 가독성이 매우 떨어지며 이를 [콜백지옥(callback hell)](https://i0.wp.com/hanamon.kr/wp-content/uploads/2021/08/%E1%84%8F%E1%85%A9%E1%86%AF%E1%84%87%E1%85%A2%E1%86%A8%E1%84%8C%E1%85%B5%E1%84%8B%E1%85%A9%E1%86%A8.png?w=640&ssl=1)이라고 한다.

```javascript
let x = 1;
function plus(cb) {
  setTimeout(() => {
    x++;
    cb(x);
  }, 100);
}

plus((x) => {
  console.log(x);
  plus((x) => {
    console.log(x);
    plus((x) => {
      console.log(x);
      plus(console.log); // 1 2 3 4
    });
  });
});
```

두번째 문제는, 콜백함수는 에러처리가 힘들다는 것이다.

```javascript
try {
  setTimeout(() => {
    throw new Error("Error!!");
  }, 100);
} catch (err) {
  console.log("err : ", e); // 발생 안함
}
```

- 이유(실행순서)
  1. setTimeout는 비동기함수이므로 바로 콜스텍에 push 되고 pop된다.
  2. 이후 Error가 상위 호출자 (caller) 방향으로 throw 된다.
  3. setTimeout은 콜스텍에 없으므로 Error의 caller가 아니다. 따라서 setTimeout함수의 에러는 catch되지 않는다.

## 45.2 비동기처리 - Promise

### 45.2.1 Promise의 생성

Promise 생성자 함수를 통해서 만들 수 있다. 안에 콜백함수가 인수로 들어가는데, 첫 arg는 resolve함수, 둘째 arg는 reject함수이다. resolve는 성공했을 때 반환값을 인자로 받고, reject는 에러가 생겼을 때 반환값을 인자로 받는다. 나중에 then을 이용해서 반환값을 handle할 수 있다.

```javascript
let isError = false;
const prom = new Promise((resolve, reject) =>
  isError ? reject(new Error("Error!!")) : resolve("good!!")
);
// [[PromiseStatus]] : 'fullfilled'
// [[PromiseValue]] : 'good!!'
```

Promise 생성자 함수는 [[PromiseStatus]]내부슬롯으로 3가지 상태를 가진다.
|[[PromiseStatus]]|의미|상태변경조건|
|:---:|---|---|
|pending|비동기 처리 수행 전|프로미스 생성 직후|
|fulfilled|비동기 처리 수행 성공|resolve 함수 호출|
|rejected|비동기 처리 수행 실패|reject 함수 호출|

- fulfilled 와 rejected 는 합쳐서 settled 상태라고도 한다.

[[PromiseValue]]내부슬롯도 갖는데, 비동기 처리 결과 리턴값이 들어간다.

## 45.3 Promise 메소드

### 45.3.1 Promise.prototype.then

두 개의 콜백함수를 받아서 첫번째 인자 함수에는 fulfill PromiseValue 가, 두번째 임자 함수에는 rejected PromiseValue가 인자로 들어간다.

```javascript
let isError = false;
const prom = new Promise((resolve, reject) =>
  isError ? reject(new Error("Error!!")) : resolve("good!!")
);

prom.then(
  (val) => console.log(val),
  (err) => console.log(err)
);
prom.then(console.log, console.log); // 위와 같은 코드 - 인자를 생략할 수 있다.
```

### 45.3.2 Promise.prototype.catch

함수 1개를 인자로 받고, 함수에는 rejected PromiseValue가 인자로 들어간다.

```javascript
let isError = true;
const prom = new Promise((resolve, reject) =>
  isError ? reject(new Error("Error!!")) : resolve("good!!")
);

prom.catch((err) => console.log(err));
prom.catch(console.log); // 위와 같은 코드 - 인자를 생략할 수 있다.
prom.then(undefined, (err) => console.log(err)); // 위와 같은 코드 - catch를 then으로 치환할 수 있다.
```

- then보다는 catch가 가독성이 좋고, then의 fulfilled 처리 함수에서 에러가 발생하면 rejected 처리 함수로는 에러를 catch할 수 없으므로 에러는 catch로 처리를 추천

```javascript
let isError = false;
const prom = new Promise((resolve, reject) =>
  isError ? reject(new Error("Error!!")) : resolve("good!!")
);

prom.then(
  (val) => console.xxx(val),
  (err) => console.log(err) // err 발생 안함.
);
prom.then((val) => console.xxx(val)).catch((err) => console.log(err)); // then에서 에러가 발생하면 catch에서 잡힌다.
```

### 45.3.2 Promise.prototype.finally

status에 상관없이 1번 실행한다.

```javascript
let isError = true;
const prom = new Promise((resolve, reject) =>
  isError ? reject(new Error("Error!!")) : resolve("good!!")
);

prom.catch(() => console.log("finally"));
```

### 45.5 Promise Chaining

지금까지 살펴본 then, catch, finally 함수는 모드 Promise 객체를 리턴한다. 따라서 연속적으로 호출할 수 있는데, 이를 Promise Chaining 이라고 한다.

```javascript
let x = 1;
const prom = (val) => new Promise((resolve, reject) => resolve(++val));

prom(x)
  .then((val) => {
    console.log(val);
    return prom(val);
  })
  .then((val) => {
    console.log(val);
    return prom(val);
  })
  .then((val) => {
    console.log(val);
    return prom(val);
  }); // 2 3 4
```

## 45.6 Promise 정적 메서드

### 45.6.1 Promise.resolve / Promise.reject

Promise.resolve와 Promise.reject 메서드는 인자를 래핑하여 프로미스로 반환한다.

```javascript
// 위 아래 코드는 같다.
const resolvedPromise = Promise.resolve([1, 2, 3]);
const resolvedPromise = new Promise((resolve) => resolve([1, 2, 3]));

// 위 아래 코드는 같다.
const rejectedPromise = Promise.reject(new Error("Error!!"));
const rejectedPromise = new Promise((_, reject) =>
  reject(new Error("Error!!"))
);
```

### 45.6.2 Promise.all

Promise.all 메서드는 여러 개의 비동기 처리를 동시에 병럴로 처리할 때 사용한다.

```javascript
const prom1 = new Promise((resolve) => setTimeout(() => resolve(1), 3000));
const prom2 = new Promise((resolve) => setTimeout(() => resolve(2), 2000));
const prom3 = new Promise((resolve) => setTimeout(() => resolve(3), 1000));

Promise.all([prom1, prom2, prom3]).then(console.log); // [1, 2, 3] - 3초 이상 걸림

const prom1 = new Promise((_, reject) =>
  setTimeout(() => reject(new Error("Error 1")), 3000)
);
const prom2 = new Promise((_, reject) =>
  setTimeout(() => reject(new Error("Error 2")), 2000)
);
const prom3 = new Promise((_, reject) =>
  setTimeout(() => reject(new Error("Error 3")), 1000)
);

Promise.all([prom1, prom2, prom3]).catch(console.log); // ERROR 3 - 1초정도 걸림
```

- 배열 속 비동기처리가 동시에 시작해서 전부 끝날때까지 기다렸다가 결과를 각각 배열로 묶은 것을 resolve하는 Promise 객체로 리턴한다.
- 이후 각 객체를 순회하면서 then으로 처리할 수 있다.
- 에러가 발생하면 그대로 에러 처리 후 나머지 fulfilled 안해도 바로 종료한다.

### 45.6.3 Promise.race

Promise.race 메서드는 여러 개의 비동기 처리를 동시에 병럴로 처리할 때 사용한다.

```javascript
const prom1 = new Promise((resolve) => setTimeout(() => resolve(1), 3000));
const prom2 = new Promise((resolve) => setTimeout(() => resolve(2), 2000));
const prom3 = new Promise((resolve) => setTimeout(() => resolve(3), 1000));

Promise.all([prom1, prom2, prom3]).then(console.log); // 3 - 1초정도 걸림

const prom1 = new Promise((_, reject) =>
  setTimeout(() => reject(new Error("Error 1")), 3000)
);
const prom2 = new Promise((_, reject) =>
  setTimeout(() => reject(new Error("Error 2")), 2000)
);
const prom3 = new Promise((_, reject) =>
  setTimeout(() => reject(new Error("Error 3")), 1000)
);

Promise.all([prom1, prom2, prom3]).catch(console.log); // ERROR 3 - 1초정도 걸림
```

- Promise.all 이랑 비슷하지만, 병렬로 동시에 Promise들을 시작한 후 가장 먼저 끝나는 것을 resolve하는 Promise 객체로 리턴
- 에러가 발생할 때는 Promise.all 처럼 바로 에러 Promise를 리턴한다.

### 45.6.4 Promise.allSettled

배열로 전달받는 Promise가 전부 settled 되면 status와 value를 객체로 만든 배열을 resolve하는 Promise를 리턴한다.

```javascript
const prom1 = new Promise((resolve) => setTimeout(() => resolve(1), 3000));
const prom2 = new Promise((resolve) => setTimeout(() => resolve(2), 2000));

Promise.allSettled([prom1, prom2]).then(console.log);
// [
//   { status: 'fulfilled', value: 1 },
//   { status: 'rejected', reason: 2 }
// ] - .catch로는 가져올 수 없다.
```

## 45.7 마이크로테스크 큐

Promise 객체는 다른 비동기 처리 함수보다 우선순위가 높다. 비동기 handler 함수도 task queue에 들어가서 실행될 차례를 기다리는데, Promise의 handler 함수는 특별히 task queue 보다 우선순위가 높은 microtask queue 에 들어간다.

```javascript
setTimeout(() => console.log(1), 0); // task queue

Promise.resolve() // microtask queue
  .then(() => console.log(2))
  .then(() => console.log(3));
// 2 3 1
```

## 45.8 fetch

fetch 함수는 XMLhttpRequest 객체와 마찬가지로 HTTP 요청 전송 기능을 제공하는 클라이언트 Web API다.

```javascript
fetch(url).then(console.log); // HTTP Get - { type, url, status, body ... }
fetch(url)
  .then((res) => res.json()) // res 중 body만 뽑아냄
  .then(console.log); // { id, name ... }
```

- HTTP 에러가 발생하면 catch() 로는 잡히지 않는다. CORS 에러 등으로 요청자체가 안될 때만 reject한다. HTTP 에러는 response 중 ok 프로퍼티가 false인지 확인해서 처리한다.

```javascript
fetch(WrongUrl)
  .then((res) => (res.ok ? res.json() : new Error(res.statusText)))
  .then(console.log)
  .catch(console.log);
```

Get이 아닌 다른 메서드로 HTTP요청하려면 fetch 두번째 인자에 payload를 넣어주면 된다.

```javascript
fetch(url); // GET
fetch(
  url,
  (payload = {
    method: "POST",
    headers: { "content-Type": "application/json" },
    body: JSON.stringify(payload),
  })
); // POST
```
