# 18장 함수와 일급 객체

## 18.1 일급 객체

- 다음 조건을 만족하는 객체는 일급 객체라 한다.

1. 무명의 리터럴로 생설할 수 있다. 즉, 런타임에 생성 가능하다.
2. 변수나 자료구조(객체, 배열 등)에 저장할 수 있다.
3. 함수의 매개변수에 전달할 수 있다.
4. 함수의 반환값으로 사용할 수 있다.

자바스크립트의 함수는 위의 조건을 모두 만족하므로 일급 객체이다.

```javascript
const increase = function (num) { // 무명의 리터럴 함수를 변수에 저장
    return ++num;
};

const decrease = function (num {
    return --num;
});

const auxs = { increase, decrease }; // increase, decrease 함수를 객체에 저장

function makeCounter(aux) { // 함수를 매개변수에 전달
    let num = 0;

    retrun function () { // 함수를 반환값으로 사용
        num = aux(num);
        return = num;
    };
}
```

함수는 객체와 동일하게 취급할 수 있으므로, 값을 사용할 수 있는 곳이라면 어디서든지 리터럴로 정의할 수 있으며, 이것은 런타임에 객체로 평가된다는 것을 의미한다.

위의 네 가지 조건 중 일급 객체로서 함수가 가지는 가장 큰 특징은 3, 4에 해당한다. 이는 함수형 프로그래밍을 가능하게 한다.

- 함수와 일반 객체의 차이점

1. **함수**는 객체이지만, 일반 객체와는 다르게 **호출할 수 있다.**
2. 함수 객체는 자신만의 고유한 프로퍼티를 소유한다.

------

## 18.2 함수 객체의 프로퍼티

함수는 객체이므로 프로퍼티를 가질 수 있다. 브라우저 콘솔에서 console.dir을 통해 객체 내부를 확인해볼 수 있다.

square 함수의 모든 프로퍼티의 프로퍼티 어트리뷰트는 Object.getOwnPropertyDescriptors 메서드로 확인해볼 수 있다.

- 다음 코드를 직접 치고 실행해보자.

```javascript
function square(number) {
    return number * number;
}

console.dir(square);

console.log(Object.getOwnPropertyDescriptors(square));

console.log(Object.getOwnPropertyDescriptors(square, '__proto__'));

console.log(Object.getOwnPropertyDescriptors(Object.prototype, '__proto__'));
```

arguments, caller, length, name, prototype은 함수 객체의 고유한 프로퍼티이다.

하지만 __proto__는 접근자 프로퍼티이며, Object.prototype 객체의 프로퍼티를 상속받은 것을 알 수 있다. Object.prototype 객체의 프로퍼티는 모든 객체가 상속받아 사용할 수 있다. (상속 -> 19장 프로토타입)

------

## 18.2.1 arguments 프로퍼티

함수 객체의 arguments 프로퍼티 값은 arguments 객체로, 함수 호출 시 전달된 인수(argument)의 정보를 담고 있는 순회 가능한(iterable) 유사 배열 객체이다. (이터러블 -> 34장)

arguments는 함수 내에서 지역 변수처럼 사용되기 때문에 외부에서는 참조할 수 없다.

함수 객체의 arguments 프로퍼티는 ES3부터 표준에서 폐지되었다. 따라서 Function.arguments와 같은 사용법은 권장되지 않으며, 함수 내의 지역변수처럼 사용할 수 있는 arguments 객체를 참조하도록 한다.

```javascript
function multyply(x, y) {
    console.log(arguments) ; // 지역변수처럼 사용
    return x * y ;
}
```

12장에서 함수에 대해 공부했던 것을 기억해보자. 함수를 정의할 때 선언한 매개변수는 함수 몸체 내부에서 변수와 동일하게 취급되었다.

즉, 함수가 호출되면 함수 몸체 내에서 암묵적으로 매개변수가 **선언**되고, **undefined로 초기화** 된 이후 인수가 할당된다.

그리고 자바스크립트 엔진이 매개변수와 인수의 개수가 일치하는지 확인하지 않으므로, 인수가 부족할 경우 undefined로 초기화된 상태를 유지하는 것, 인수가 초과된 경우 초과한 만큼 무시되는 것을 확인한 바 있다.

무시된 인수가 그냥 버려지는 것은 아니다. 암묵적으로 arguments 객체의 프로퍼티로 보관된다.

```javascript
function multiply(x, y) {
    console.log(arguments) ;
    return x * y ;
}

console.log(multiply(1, 2, 3));
```

코드를 실행해보고, 확인해보자.

이렇게 매개변수와 인수의 개수가 일치하는 지 확인하지 않기 때문에, 함수가 호출되면 인수 개수를 확인하고, 그에 따라 동작을 달리 정의할 필요가 있을 수 있다.

이때 arguments 객체가 유용히 사용된다. arguments 객체는 개수를 확정할 수 없는 **가변 인자 함수**를 구현할 때 유용하다.

```javascript
function sum() {
    let res = 0;

    for (let i = 0; i < arguments.length ; i++ ) {
        res += arguments[i];
    }

    return res;
}
```

arguments 객체는 배열 형태를 갖추고 있긴 하지만, 실제 배열이 아니라 유사 배열 객체(array-like object)이다. **유사 배열 객체란 length 프로퍼티를 가진 객체로, for 문으로 순회할 수 있는 객체를 말한다.** (ES6)

유사 배열 객체는 배열 메서드를 사용할 경우 에러가 발생하기 때문에, 배열 메서드를 사용하려면 간접 호출 해야한다. (배열 메서드 -> 27장, 간접호출 -> 22장)

간접 호출을 하려는 번거로움을 해결하기 위해 ES6부터는 Rest 파라미터가 도입되었다. (Rest 파라미터 -> 26장)

------

## 18.2.2 caller 프로퍼티

caller 프로퍼티는 ESMAScript 사양에 포함되지 않은 비표준 프로퍼티이다. 이후 표준화될 예정도 없는 프로퍼티이므로 사용하지 말고, 관심 있으면 책 256 페이지를 살펴보아라.

------

## 18.2.3 length 프로퍼티

함수 객체의 length 프로퍼티는 함수를 정의할 때 선언한 매개변수의 개수를 가리킨다.

반면 arguments 객체의 length 프로퍼티는 인자의 개수를 가리키기 때문에, 두 객체의 length 프로퍼티 값은 다를 수 있다.

------

## 18.2.4 name 프로퍼티

name 프로퍼티는 함수 이름을 나타내며, ES6부터 정식 표준이 되었다. 익명 함수 표현식의 경우 name 프로퍼티가 ES5에서는 빈 문자열을 값으로, ES6에서는 식별자를 값으로 갖기 때문에 주의해야 한다.

또한 12장에서 살펴본 것 처럼 함수 이름과 함수 객체의 식별자는 다르다는 것을 잊지 말도록 하자.

------

## 18.2.5 proto 접근자 프로퍼티

모든 객체는 [[Prototype]]이라는 내부 슬롯을 갖는다. (프로토타입 -> 19장)

__proto__프로퍼티는 [[Prototype]] 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티이다. 내부 슬롯에는 직접 접근할 수 없고, 간접적으로 접근자 프로퍼티를 통해 접근해야한다.

```javascript
const obh = { a: 1 };

// 객체 리터럴 방식으로 생성한 Object.prototype
console.log(obj.__proto === Object.prototype); // ture

// 객체 리터럴 방식으로 생성한 객체는 프로토타입 객체인 Object.prototype를 상송 받는다.
// hasOwnProperty 메서드는 Object.prototype의 메서드이다. 전달 받은 프로퍼티 키가 객체 고유의 것인지 확인한다.
console.log(obj.hasOwnProperty('a')); // true
console.log(obj.hasOwnProperty('__proto__')); // false
```

------

## 18.2.6 prototype 프로퍼티

생성자 함수로 호출할 수 있는 함수 객체, 즉 constructor가 소유하는 프로퍼티를 prototype 프로퍼티라고 한다. 일반 객체와 non-constructor에는 prototype 프로퍼티가 없다.

즉, **함수 객체는 prototype 프로퍼티를 소유한다.**

prototype 프로퍼티는 함수가 객체를 생성하는 생성자 함수로 호출될 때, 생성자 함수가 생성할 인스턴스의 프로토타입 객체를 가리킨다.
