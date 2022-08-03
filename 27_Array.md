# Ch.27 배열

## JavaScript 배열의 특징

1. JavaScript의 배열은 객체

    배열은 프로퍼티로 Index, length 가지는 객체. 각 값을 Element 라고 함.

      ```javascript
      console.dir(Object.getOwnPropertyDescriptors(['a','b','c']))
      //결과 - Object
      {
        0: {value: 'a', writable: true, enumerable: true, configurable: true}
        1: {value: 'b', writable: true, enumerable: true, configurable: true}
        2: {value: 'c', writable: true, enumerable: true, configurable: true}
        length: {value: 3, writable: true, enumerable: false, configurable: false}
        [[Prototype]]: Object
      }
      ```

    다른 언어의 배열은 메모리 주소에 순서대로 값을 저장한다.
    But) JavaScript는 해시 테이블로 구형된 객체에 값을 저장한다.
    So ) JavaScript는 임의요소에 접근이 느리고, 삽입 삭제가 빠르다.
    모던 자바스크립트 엔진은 임의요소 접근도 2배정도 빠르게 최적화를 한다.

2. Dense Array & Sparse Array

    Dense Array 란 배열의 length 만큼 Element를 가지는 배열
    Sparse Array 란 배열의 length 보다 작은 Element를 가지는 배열

      ```javascript
      const sparse = [, 2, , 4];
      console.log(sparse.length); // 4
      console.log(sparse); // [empty, 2, empty, 4]
      Object.getOwnPropertyDescriptors(sparse); // { '1': {}, '3': {}, lenth: {} }
      //주의* Empty 접근 시 null이 아닌 undefined
      ```

    배열의 요소가 2개인데 배열의 length 를 4로 바꾸면, 빈 요소만큼 Empty가 됨
    => [1, 2] => [1, 2, empty x 2]

3. 유사배열객체

    배열처럼 index로 임의 접근이 되고, length 프로퍼티를 갖는 객체
    instanceof Array 가 false 이므로 **배열의 매소드를 사용할 수 없다**

## 배열의 생성

```javascript
// 1. Array.of(ele, ...)
// arguments를 요소로 가지는 배열 리턴
Array.of(1, "hello"); // [1, 'hello']

// 2. Array.from(obj, cb(ele, idx))
// arg1(객체)를 arg2(콜백함수) 인자로 넣고 나온 리턴값의 배열 리턴, 리턴 배열의 크기는 arg1의 length 프로퍼티
// 사용법: 유사배열객체를 배열로 만들 수 있음
Array.from({ length: 3 }, (ele, idx) => idx); // [0, 1, 2]
Array.from({ length: 2, 0: 1, 1: 2 }, (ele, idx) => ele * ele); // [1, 4] - 유사배열객체
Array.from({ 0: 1, 1: 2 }, (ele, idx) => ele); // []

// 3. Array 생성자 함수
const arr = new Array(3); // [empty x 3]
```

## 배열의 접근

```javascript
// 1. 대괄호 []
// index로 임의 접근이 가능하다. 없는 index는 undefined를 리턴(객체와 동일)
const arr = [, 1];
arr[1]; // 1
arr[0]; // undefined
arr[2]; // undefined

// 2. Array.prototype.filter(cb(ele, idx, arr))
// 배열 각각의 요소에 대해서 콜백함수실행. 콜백함수에는 배열 각각의 요소, index, 배열자체(thisValue)가 인수로 들어가고 true가 리턴되는 요소만 배열로 리턴.
let arr = [1, 2, 3];
arr.filter((ele) => ele < 3); // [1, 2]

// 3. Array.prototype.slice(idx, idx)
// 배열의 arg1 index 부터 arg2 index - 1 까지의 요소만 배열로 리턴
// arg2 가 생략되면 arg1 index 부터 배열의 끝까지 전부 배열로 리턴
// arg1, arg2 모두 생략되면 얕은 복사된 배열 리턴
let arr = [1, 2, 3];
arr.slice(1, 2); // [2]
arr.slice(1); // [2, 3]
Array.prototype.slice.call({ length: 1, 0: 1, 1: 2 }); // [1, 2] - 유사배열객체를 얕은 복사햐여 배열로 바꿀 수 있음
```

## 배열의 추가 및 수정

```javascript
let arr = [1, 2, 3];

// 1. 대괄호 []
// index로 접근한 element를 수정할 수 있음. 없는 index에 element 추가하면 length가 따라서 변함.
arr[1] = 22; // [1, 22, 3]
arr[4] = 5; // [1, 2, 3, empty, 5];
arr.foo = "bar"; // [1, 2, 3, foo: 'bar'] - arr.length: 3

// 2. Array.prototype.unshift(ele) & Array.prototype.push(ele)
// unshift() 는 맨 앞에 element 추가, push() 는 맨 뒤에 element 추가
// 제거된 요소 리턴
arr.unshift(0); // [0, 1, 2, 3]
arr.push(4); // [1, 2, 3, 4]

// 3. Array.prototype.concat(ele, ...)
// arguments 를 배열의 요소로 추가 후 리턴, 배열 인자는 내부 요소가 추가됨
arr.concat(4); // [ 1, 2, 3, 4]
arr.concat([4], 5); // [1, 2, 3, 4, 5]

// 4. Array.prototype.splice(idx, num, ele ...)
// arg1은 index, arg2는 교체할 element 수 - 생략하면 index 부터 전부
// arg3은 교체할 element 들 - 생략하면 요소가 교체가 아닌 삭제
// 교체.제거된 요소 리턴
arr.splice(1); // [1]
arr.splice(1, 1); // [1, 3]
arr.splice(1, 1, 4, 5); // [1, 4, 5, 3]

// 5. Array.prototype.reverse()
// 원본 배열의 순서를 뒤집는다
arr.reverse(); // [3, 2, 1]

// 6. Array.prototype.fill(ele, idx, idx)
// arg2 idx 부터 arg3 idx -1 까지 모든 요소를 ele로 교체
// arg2 idx, arg3 idx는 생략가능
arr.fill(0); // [0, 0, 0]
arr.fill(0, 1); // [1, 0, 0]
arr.fill(0, 1, 2); // [1, 0, 3]

// 7. Array.prototype.flat(num)
// num 만큼 중첩 배열을 평탄화, 생략하면 1
[1, [2, [3]]].flat() // [1, 2, [3]]
[1, [2, [3]]].flat(2) // [1, 2, 3]

// 8. Array.prototype.sort(cb(a, b))
// Array를 정렬하여 리턴 - 인수 생략하면 유니코드 오름차순
[1, 2, 11].sort() // [1, 11, 2] - 11의 앞자리 1이 2보타 유니코드상 낮음
[(1, 2, 11)].sort((a, b) => a - b) // [1, 2, 11] 아래에 자세한 설명
[(1, 2, 11)].sort((a, b) => b - a); // [11, 2, 1]

// 9. Array.prototype.map(cb(ele, idx, arr), this)
// 배열 각각의 요소에 대해서 콜백함수실행. 콜백함수에는 배열 각각의 요소, index, 배열자체(thisValue)가 인수로 들어감. 콜백함수의 리턴값이 배열의 요소가 되므로 length 가 같다.
// 희소배열의 empty는 콜백함수 순회 안함
// arg2 this 는 arg1 콜백함수 내부에서 this로 사용될 객체를 지정할 수 있다.
class Prefixer {
  constructor(prefix) {
    this.prefix = prefix;
  }
  add(arr) {
    return arr.map(function (item) {
      return this.prefix + item;
    }, this);
  }
}
const prefixer = new Prefixer("hi-");
console.log(prefixer.add(["uoslife"])); // ['hi-uoslife']
```

7번 Array.prototype.sort(cb(a,b))

  결과적으로 반환된 배열은 진접한 요소를 cb의 인수 a, b에 넣었을때 양수, 0, 음수가 리턴될때 각각 만족하는 조건이 다름. 리턴값은 인접한 두 요소의 자리를 바꾸는가? 라고 생각하면 편하다. 양수가 리턴되면 자리를 바꾸고(b-> a 순서), 0은 순서를 유지하고, -1은 자리를 안바꾼다(a-> b 순서). 에를 들면 [1, 3, 2].sort((a,b) => a-b) 에서 [1, 2, 3]이 리턴되는데, 1 - 3 < 0 이므로 1 -> 3 순서가 되고, 3 - 2 > 0 이므로 2 -> 3 순서가 되어서, [1, 2, 3] 배열이 리턴된다.

## 배열의 삭제

```javascript
let arr = [1, 2, 3];

// 1. delete
// index로 임의 접근한 element 제거. length 에는 영향 없음
delete arr[1]; // [1, empty, 3] - arr.length: 3

// 2. Array.prototype.shift(ele) & Array.prototype.pop(ele)
// shift() 는 맨 앞의 element 제거, pop() 은 맨 뒤의 element 제거
arr.shift(); // [2, 3]
arr.pop(); // [1, 2]
```

## 기타 메소드

```javascript
let arr = [1, 2, 3];
// 1. Array.isArray()
// 배열이면 true, 아니면 false 리턴
Array.isArray({ 0: 1, length: 1 }); // 유사배열도 false

// 2. Array.prototype.indexOf(ele, idx)
// 배열의 요소를 찾아서 index를 리턴 - 중복되면 첫번째 요소의 index, 없으면 -1 리턴
// arg2는 검색을 시작할 index
arr.indexOf(1); // 0
arr.indexOf(1, 1); // -1

// 3. Array.prototype.join(str)
// 배열의 요소 사이에 str을 넣은 문자열 리턴
arr.join("+"); // '1+2+3'

// 4. Array.prototype.includes(ele, idx, idx)
// arg2 idx 부터 arg2 idx - 1 까지 요소에 ele가 포함되어있는지 확인
// arg2 idx 와 arg3 idx 는 생략 가능
arr.includes(3); // true
arr.includes(3, -1); // true  - 마지막 요소부터 확인

// 5. Array.prototype.forEach(cb(ele, idx, arr))
// 배열 각각의 요소에 대해서 콜백함수실행. 콜백함수에는 배열 각각의 요소, index, 배열자체(thisValue)가 인수로 들어감.
// 희소배열의 empty는 콜백함수 순회 안함
let rst = [];
arr.forEach((ele, idx, arr) => rst.push("" + ele + idx + arr[idx])); // rst = ['101', '212', '323']

// 6. Array.prototype.reduce(cb(pre, cur, idx, arr), iv)
// 콜백함수의 인자는 preValue, currentValue, index, thisArray. 배열의 요소를 순환하면서 cb함수를 실행하고, 리턴된 값이 반복적으로 preValue(Accumulator)에 인수로 들어감. 마지막 cb함수를 실행하고 누적된 결과 리턴
arr.reduce((acc, cur, idx, arr) => acc + cur, 0); // 0+1+2+3 = 6
const err = [].reduce((acc, cur)=> acc + cur) // error : 초가깂을 설정하는게 좋음

// 7. Array.prototype.some(cb(ele, idx, arr))
// 배열의 요소를 순환하면서 한번이라도 콜백함수가 true이면 true
arr.some((ele) => ele !== 2); // true

// 8. Array.prototype.every(cb(ele, idx, arr))
// 배열의 요소를 순환하면서 모든 콜백함수가 true이면 true
arr.some((ele) => ele !== 2); // false

// 9. Array.prototype.find(cb(ele, idx, arr)) & findIndex(cb(ele, idx, arr))
// 배열의 요소를 순환하면서 콜백함수가 true인 첫 번째 요소 리턴 & 요소의 index 리턴
arr.find((ele) => ele > 1); // 2
arr.findIndex((ele) => ele > 1); // 1
```

## 배열 메소드 사용 꿀팁

```javascript
// 평균구하기
  const arr = [1, 2, 3];
  const average = arr.reduce((acc, cur, i, { length })=> {
    return i === length - 1 ? acc + cur / length : acc + cur;
  }, 0)

// 요소 중복 횟수 구하기
  const arr = [ 'a', 'b', 'c', 'a' ];

  const count = arr.reduce((acc, cur) => {
    acc[cur] = (acc[cur] || 0) + 1;
    return acc;
  }, {});

// 중첩 배열 평탄화
  const arr = [1, [2, [3]]];

  const flatten = arr.reduce((acc, cur) => {
    acc.concat(cur), []});

// 중복 요소 제거
  const arr = [ 1, 1, 2, 3, 1, 4, 3, 5 ];

  const result = arr.reduce((acc, cur, idx, arr) => 
    arr.indexOf(cur) === i ? [...acc, cur] : acc
    , []
  );

  const result = arr.filter((ele, idx, arr)=> arr.indexOf(ele) === idx);
```
