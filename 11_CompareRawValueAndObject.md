# 11. 원시 값과 객체 비교

6장에서 설명했듯이 데이터 타입은 원시 값과 객체로 나뉘어진다. 그렇다면 원시 타입과 객체 타입의 차이는 무엇일까?

- 원시 값은 변경 불가능한 값이다. 이에 비해, 객체는 변경 가능한 값이다.

- 원시 값을 변수에 할당하면 변수에는 실제 값이 저장된다. 이에 비해, 객체를 변수에 할당하면 변수에는 참조값이 저장된다.

## 11.1 원시 값

---

### 11.1.1 변경 불가능 값

값을 변경할 수 없다는 것은 무엇을 일컫는 것 일까? 우선적으로 변수와 값의 구분을 확실히 알아야 한다. 변수는 하나의 값을 저장하기 위한 저장소(메모리)이며 식별자는 변수를 구분짓기 위한 이름이다. 값은 변수에 저장된 데이터로서 표현식이 평가되어 생성된 결과이다.

그렇기에 변경 불가능하다는 것은 원시 값 자체를 변경할 수 없다는 것이지 변수 값을 변경할 수 없다는 것이 아니다. 즉, 변수에는 얼마든지 재할당을 통해 값을 변경할 수 있다.

```javascript
var score;

score = 80;

score = 90;
```

원시 값을 할당한 변수에 사로운 값을 재할당하면(score = 90) 기존 메모리에 할당되어 있는 변수의 값(score = 80)이 바뀌는 것이 아니라 재할당된 변수 값을 위한 메모리 공간이 새롭게 할당되어진다.

변수가 참조하던 메모리 공간의 주소가 새롭게 바뀐 이유는 원시 값이 변경 불가능한 성질을 갖고 있기 때문이다. 만약 변경 가능했다면, 변수 값을 재할당했을 때 메모리 공간이 새로 주어지는 것이 아니라 기존 메모리 주소의 값이 변경되었을 것이다.

따라서 이미 할당되어 있는 변수의 값을 변경하려면 새로운 메모리 공간을 확보한 후 이곳에 재할당한 값을 저장해야 한다. 이러한 값의 특성을 **불변성**이라고 한다.

### 11.1.2 값에 의한 전달

그렇다면 변수에 변수를 할당한다면 원시 값의 불변성을 제대로 지켜질까?

```javascript
var score = 80;
var copy = score;

console.log(score);
// 80
console.log(copy;
// 80

socre = 100;

console.log(score);
// 100
console.log(copy);
// ???
)
```

## 11.2 객체

---

객체는 프로퍼티 수가 정해져 있지 않으며, 동적으로 추가되고 삭제할 수 있다. 또한 프로퍼티 값에도 제한이 없다. 그렇기에, 객체는 원시 값처럼 사전에 메모리를 할당하고 저장할 수 없다.
그렇기에 객체는 원시 값과 다른 방식으로 작동한다.

### 11.2.1 변경 가능한 값

객체는 변경 가능한 값이다.

```javascript
var person = {
    name : 'Lee'
};

person.name = 'Kim';

person.address = 'seoul';

console.log(person);
// {name: 'Kim', address: 'seoul'}
```

앞서 말했듯이 객체는 프로퍼티 수가 정해져있지 않기에, 값을 복사해서 생성하는 일은 매우 비용이 많이 든다. 그렇기에 메모리의 효율적 사용을 위해, 객체는 변경 가능한 값으로 설정되어 있다.

### 11.2.2 깊은 복사 & 얕은 복사

값을 복사하는 방법에는 깊은 복사(deep copy)와 얕은 복사(shallow copy)가 있다.

```javascript
let num1 = 1;
let num2 = num1;

num2 = 2;

console.log(num1, num2);
// 1, 2 <= 깊은 복사(depp copy)
// 2, 2 <= 얕은 복사(shallow copy)
```

어떤 방식의 복사가 되냐에 따라 깊은 복사가 될 수 있고 얕은 복사가 될 수 있다.

원시값은 깊은 복사를 따른다.

```javascript
const person1 = {
    name = 'kim',
    age = 23
}

let person2 = person1;

person2.name = 'park';
person2.age = '18';

console.log(person1, person2);
// {name : "park", age : 18}, {name : "park", age : 18}
```

객체의 경우, 원시값과 다르게 얕은 복사(shallw copy)를 따른다. 이는 객체의 프로퍼티를 무수히 확장할 수 있는 객체의 특성때문에 얕은 복사를 하게 된 것이다.

```javascript
const person1 = {
    name = 'kim',
    age = 23,
    hobby1 = '게임',
    hobby2 = '테니스',
    hobby3 = '축구',
    hobby4 = '야구',
    hobby5 = '영화감상',
    hobby6 = '코딩',
}

let person2 = person1; 
    // 이처럼 프로퍼티의 개수가 무수히 많을 때 만약 깊은 복사로 하나의 프로퍼티를 복사하는데 1초가 걸린다면 굉장히 비효율적이며 메모리로 엄청나게 많이 소비될 것이다.
```

하지만 얕은 복사를 통해, 모든 프로퍼티를 다 복사하지말고 복사할 객체의 메모리 주소를 참조할 수 있도록 person1 변수만 복사한다면 person1의 프로퍼티 값이 필요할 때만 참조값을 참조하면 되는 것이다.

### 11.2.3 참조에 의한 전달

하지만 객체는 변경 가능한 값이기에, 그에 따른 단점도 존재한다. 바로 여려 식별자가 하나의 객체를 공유하여 부작용을 발생시킨다는 것이다.

```javascript
var person = {
    name : 'Lee'
};

var copy = person; // 참조 값을 복사(얕은 복사)

console.log(person === copy) // true

person.name = 'Kim';

copy.address = 'seoul';

console.log(person); // {name : "Kim", address : "seoul"}
console.log(copy); // {name : "Kim", address : "seoul"}
```

객체를 가리키는 변수(person)을 다른 변수(copy)에 할당하면, 원본의 참조 값이 복사되어 전달되는데, 이를 참조에 의한 전달이라고 한다.

원본 person과 사본 copy는 서로 다른 메모리 주소를 갖고 있지만, 동일한 참조 값을 공유한다. 즉, 두 개의 식별자가 하나의 객체를 공유한다. 그렇기에 person이나 copy 둘 중 하나가 값을 변경한다면 다른 하나의 변수도 영향을 받는다.

```javascript
var person1 = {
    name : 'Kim'
}

var person2 = {
    name : 'Kim'
}

console.log(person1 === person2) // ?
console.log(person1.name === person2.name) // ?
```
