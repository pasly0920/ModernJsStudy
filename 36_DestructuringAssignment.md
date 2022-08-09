# 36. 디스트럭처링 할당

디스트럭처링 할당(구조 분해 할당)은 구조화된 배열과 같은 이터러블 또는 객체를 destructuring하여 1개 이상의 변수에 개별적으로 할당하는 것을 의미한다. 배열과 같은 이터러블 또는 객체 리터럴에서 필요한 값만 추출하여 변수에 할당할 때 유용하다.

## 36.1 배열 디스트럭처링 할당

---

ES5에서 구조화된 배열을 destructuring하여 1개 이상의 변수에 할당하는 방법은 다음과 같다. 하지만 ES6의 배열 destructuring에서는 배열의 각 요소를 배열로부터 추출하여 1개 이상의 변수에 할당한다.

- 배열 destructuring의 대상(할당문의 우변)은 이터러블이어야 하며, 할당 기준은 배열의 인덱스이다. 즉 순서대로 할당이 된다.

- 이 때 변수의 개수와 이터러블의 요소의 개수는 반드시 일치할 필요는 없다.

- 이 동작 간에 기본값을 할당하는 것도 가능하나 이는 우변의 배열을 통한 할당보다 낮은 우선순위를 가진다. 즉 기본값이 할당되어 있는 변수에 우변의 배열을 통해 값을 할당한다면 최종적으로 저장되는 값은 우변의 배열의 해당 인덱스 값이다.

```javascript
//ES5
var arr = [1, 2, 3];

var one = arr[0];
// ...

//ES6
const arr = [1, 2, 3];
const [one, two, three] = arr;

const [x, y] = {}; //Type Error: {} is not iterable

let [x, y];
[x, y] = [1, 2]; // possible but not good
//상기 방식으로는 const로 선언이 불가하기 때문

const [a, b] = [1];
console.log(a, b); //1 undefined

const [a, b, c = 3] = [1, 2];
consol.elog(a, b, c)// 1 2 3

const [a, b = 10, c = 3] = [1, 2, 3];
console.log(a, b, c); //1 2 3
// 기본값보다 우변을 통해 할당된 값이 우선시 됨.
```

배열 destructuring 할당은 배열과 같은 이터러블에서 필요한 요소만 추출하여 변수에 할당하고 싶을 때 유용하다. 다음 예제를 살펴보자.

```javascript
const parsedURL = [
    'https://developer.mozilla.org/ko/docs/Web/JavaScript',
    'https',
    'developer.mozilla.org',
    'ko/docs/Web/JavaScript',
    index: 0,
    input: 'https://developer.mozilla.org/ko/docs/Web/JavaScript',
    groups: undefined
];

const [, protocol, host, path] = parsedURL;
/*
{
  protocol: 'https',
  host: 'developer.mozilla.org',
  path: 'ko/docs/Web/JavaScript'
}
*/
// 좌측 destructuring에서 제일 처음은 비어있으므로 넘어간 후에 하나씩 할당이 되어 다음과 같은 결과를 나타냄.
```

배열 destructuring 할당을 위한 변수에 Rest 파라미터와 유사하게 Rest 요소 ... 를 사용할 수 있다. Rest 요소는 Rest 파라미터와 마찬가지로 parameter의 제일 마지막에 위치해야 한다.

```javascript
const [x, ...y] = [1, 2, 3];
console.log(x, y); // 1 [ 2, 3 ]
```

## 36.2 객체 디스트럭처링 할당

---

ES5에서 객체의 각 프로퍼티를 객체로부터 destructuring하여 변수에 할당하기 위해서는 프로퍼티 키를 사용해야 한다.

```javascript
var user = { firstName: "Ungmo", lastName: "Lee" };

var firstName = user.firstName;
var lastName = user.lastName;

console.log(firstName, lastName); // Ungmo Lee
```

ES6의 객체 destructuring 할당은 객체의 각 프로퍼티를 객체로부터 추출하여 1개 이상의 변수에 할당한다. 이 때 객체 destructuring 할당의 대상(할당문의 우변)은 객체이어야 하며, 할당 기준은 프로퍼티 키이다.

- 즉 순서는 의미가 없으며 선언된 변수 이름과 프로퍼티 키가 일치하면 할당된다.

- 우변에 객체 또는 객체로 평가될 수 있는 식을 할당하지 않으면 에러가 발생한다.

- 객체의 프로퍼티 키와 다른 변수 이름으로 프로퍼티 값을 할당받으려면 다음과 같이 변수를 선언한다.

- 객체 destructuring 할당에서 변수에 기본값을 설정할 수 있다.

- 객체 destructuring 할당은 객체에서 프로퍼티 키로 필요한 프로퍼티 값만 추출하여 변수에 할당하고 싶을 때 유용하다.

- 객체 destructuring 할당은 객체를 인수로 전달받는 함수의 매개변수에도 사용할 수 있다.

- 배열의 요소가 객체인 경우 배열 destructuring 할당과 객체 destructuring 할당을 혼용할 수 있다.

- 중첩 객체의 경우에도 destructuring 할당이 가능하다.

- 객체 destructuring 할당을 위해 변수에 Rest 파라미터와 Rest 요소에 유사하게 Rest 프로퍼티 ...을 사용할 수 있다. 이러한 경우에도 반드시 제일 마지막에 위치해야 한다.

```javascript
const user = { firstName: "Ungmo", lastName: "Lee" };

// ES6 객체 디스트럭처링 할당
// 변수 lastName, firstName을 선언하고 user 객체를 디스트럭처링하여 할당한다.
// 이때 프로퍼티 키를 기준으로 디스트럭처링 할당이 이루어진다. 순서는 의미가 없다.
const { lastName, firstName } = user;
//이는 프로퍼티 축약 표현, 위와 아래는 동치
const { lastName: lastName, firstName: firstName } = user;

console.log(firstName, lastName); // Ungmo Lee

const { lastName, firstName } = null;
// TypeError: Cannot destructure property 'lastName' of 'null' as it is null.

// 프로퍼티 키를 기준으로 디스트럭처링 할당이 이루어진다.
// 프로퍼티 키가 lastName인 프로퍼티 값을 ln에 할당하고,
// 프로퍼티 키가 firstName인 프로퍼티 값을 fn에 할당한다.
const { lastName: ln, firstName: fn } = user;

console.log(fn, ln); // Ungmo Lee

const { firstName = "Ungmo", lastName } = { lastName: "Lee" };
console.log(firstName, lastName); // Ungmo Lee

const { firstName: fn = "Ungmo", lastName: ln } = { lastName: "Lee" };
console.log(fn, ln); // Ungmo Lee

const todo = { id: 1, content: "HTML", completed: true };
// todo 객체로부터 id 프로퍼티만 추출한다.
const { id } = todo;
console.log(id); // 1

function printTodo(todo) {
  console.log(
    `할일 ${todo.content}은 ${todo.completed ? "완료" : "비완료"} 상태입니다.`
  );
}

printTodo({ id: 1, content: "HTML", completed: true });
// 할일 HTML은 완료 상태입니다.

function printTodo({ content, completed }) {
  console.log(`할일 ${content}은 ${completed ? "완료" : "비완료"} 상태입니다.`);
}

printTodo({ id: 1, content: "HTML", completed: true });
// 할일 HTML은 완료 상태입니다.

const todos = [
  { id: 1, content: "HTML", completed: true },
  { id: 2, content: "CSS", completed: false },
  { id: 3, content: "JS", completed: false },
];

// todos 배열의 두 번째 요소인 객체로부터 id 프로퍼티만 추출한다.
const [, { id }] = todos;
console.log(id); // 2

const user = {
  name: "Lee",
  address: {
    zipCode: "03068",
    city: "Seoul",
  },
};

// address 프로퍼티 키로 객체를 추출하고 이 객체의 city 프로퍼티 키로 값을 추출한다.
const {
  address: { city },
} = user;
console.log(city); // 'Seoul'

// Rest 프로퍼티
const { x, ...rest } = { x: 1, y: 2, z: 3 };
console.log(x, rest); // 1 { y: 2, z: 3 }
```
