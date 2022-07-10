# 08 제어문

제어문은 조건에 따라 코드 블록을 실행할 때 사용된다. 일반적으로 코드는 위에서 아래 방향으로 순차적으로 실행되며, 제어문을 사용시 코드의 흐름을 인위적으로 제어할 수 있다.

하지만 코드의 실행순서가 변경되는 것은 순차적 실행이라는 직관을 무너뜨리므로 흐름을 혼란스럽게 하고 이런 복잡성이 부가되면 코드의 오류가 발생할 확률이 증가하기에 조심해야 한다.

## 8.1 블록문

---

블록문은 0개 이상의 statement를 중괄호로 묶은 것으로 JS는 블록문을 하나의 실행단위로 취급한다. 블록문은 단독 실행이 가능하긴 하나 일반적으로 제어문이나 함수를 정의할 때 사용하는 것이 일반적이다.

statement의 끝에는 세미콜론을 붙이는 것이 원칙이나 블록문은 언제나 문의 종료를 의미하는 자체 종결성을 갖기 때문에 블록문의 끝에는 세미 콜론을 붙이지 않는다.

```javascript
{
  var foo = 10;
} // 블록문
```

## 8.2 조건문

---

조건문은 주어진 조건식의 평가 결과에 따라서 코드 블럭의 실행 여부를 결정한다. 조건식은 불리언 값으로 평가될 수 있는 표현식이다. JS는 if ... else 문과 switch문으로 두 가지 조건문을 제공한다.

### 8.2.1 if ... else

```javascript
if(조건식) {
    //조건문이 참이면 이 코드 블록이 실행된다.
} else {
    //조건문이 거짓이면 이 코드 블록이 실행된다.
}

if(조건식1) {
    //조건식 1이 참이면 이 코드 블록이 실행된다.
} else if (조건식 2) {
    //조건식 2가 참이면 이 코드 블록이 실행된다.
} else {
    //조건식 1과 조건식 2가 모두 거짓이면 이 코드 블록이 실행된다.
}

if(조건문)
    statement1;
else
    statement2;

(조건문) ? statement1 : statement2; // 상기 if ... else문과 동일
```

if 문의 조건식은 Boolean 값으로 평가되어야 하며 Boolean 값이 아니라면 JS 엔진에 의해서 **암묵적으로 Boolean 값으로 강제변환되어 실행되니** 주의해야 한다. 추가적으로 if와 else 사이에 else if를 추가해서 분기를 추가적으로 설정할 수 있다.

코드 블록 내의 statement가 하나라면 중괄호를 생략 가능하다. 추가적으로 대부분의 if ... else문은 삼항 조건 연산자로 바꿔 쓸 수 있다. 이 때 삼항 조건 연산자는 값으로 평가되는 표현식을 만들기에 변수에 할당할 수 있으나 if ... else문은 표현식이 아닌 statement이기에 값이 반환되지 않고 이를 값처럼 사용할 수 없고 변수에 할당할 수 없다.

조건에 따라 단순히 값을 결정하는 경우 if ... else문보다는 삼항 조건 연사자를 사용하는 편이 좋고 조건이 복잡하여 실행할 내용이 복잡하다면 if ... else문을 사용하는 편이 좋다.

### 8.3.2 switch

switch 문은 주어진 표현식을 평가하여 그 값과 일치하는 표현식을 갖는 case 문으로 실행 흐름을 옮긴다. case문은 상황을 의미하는 표현식을 지정하고 콜론으로 마친다. 그리고 그 뒤에 실행할 statement들을 위치시킨다.

switch문의 표현식과 일치하는 case문이 없다면 실행 순서는 default문으로 이동한다. default문은 선택사항으로 사용할 수도 사용하지 않을 수도 있다.

```javascript
switch(표현식) {
    case(표현식 1) :
        // switch문의 표현식이 표현식1과 같으면 실행될 문
        break;
    case(표현식 2) :
        //switch문의 표현식이 표현식2와 같으면 실행될 문
        break;
    default :
        //switch문의 표현식과 일치하는 case문이 없을 때 실행될 문
}
```

if ... else문의 조건식은 Boolean 값으로 평가되어야 하지만 switch문의 표현식은 불리언 값보다는 문자열이나 숫자 값이 경우가 많으며 switch문은 단순 참 거짓보다는 다양한 상황에 따라서 실행할 코드 블록을 결정할 때 사용한다.

switch문 내의 각각의 case 문에서 break는 switch 코드 블록을 탈출하는 역할을 한다. break문이 없다면 다음 표현식이 일치하지 않더라도 다음 case문으로 연이어 이동한다. default문의 경우 switch문의 맨 마지막으로 break 여부에 상관없이 switch문이 종료되므로 별도로 break문을 필요로 하지 않는다.

break를 일부러 생략하여 상기와 같은 작동을 의도적으로 일으킬 수 있다. 다음 예제가 그러한 경우로 윤년인지 판별하여 각 달의 일수를 구하는 예제이다.

```javascript
const year = 2000; // 윤년
const month = 2;
let days = 0;

switch(month) {
    case 1: case 3: case 5: case 7: case 8: case 10: case 12:
        days = 31;
        break;
    case 4: case 6: case 9: case 11:
        days = 30;
        break;
    case 2:
        //윤년 계산
        break;
    case default:
        console.log("invalid");
}
```

만약 if ... else문으로 해결할 수 있다면 switch문보다 if ... else문으로 해결하는 것이 좋으나 조건문이 너무 많아 가독성이 떨어진다면 switch문을 사용하는 것이 낫다.

## 8.3 반복문

---

### 8.3.1 for 문

for문은 조건식이 거짓으로 평가될 때까지 코드 블록을 반복 실행한다. 가장 일반적으로 사용되는 for문의 형태는 다음과 같다.

```javascript
for(변수 선언문 or 할당문; 조건식; 증감식)
    조건식이 참인 경우 반복 실행될 statement;

for(let i = 0; i < 2; i++)
    console.log(i);

for(;;) { ... } //무한 반복문

for(let i = 1; i<=6; i++)
    for(let j=1; j<=6; j++)
        console.log(`{${i}, ${j}}`)
```

for문의 변수 선언문, 조건식, 증감식은 모두 옵션이므로 반드시 사용할 필요는 없다. 단 어떤 식도 선언하지 않으면 무한 루프가 된다. 무한 루프란 코드 블록을 무한히 반복 실행하는 문이다. 추가적으로 for 문 내에 for 문을 여러 번 중첩하는 일 또한 가능하다.

### 8.3.2 while 문

while문은 주어진 조건식의 평가 결과가 참이면 코드 블록을 계속해서 반복 실행한다. for 문은 반복 횟수가 명확할 때 주로 사용하고 while문은 반복횟수가 불명확할때 주로 사용한다.

```javascript
let count = 0;

while(count < 3){
    console.log(count);
    count ++;
}

while(true) { .... } //무한 루프

while(true){
    console.log(count);
    count++;

    if(count == 3)
        break;
}
```

while문은 조건문의 평가 결과가 거짓이 되면 코드 블록을 실행하지 않고 종료하며 조건식의 평가 결과가 Boolean 값이 아니라면 Boolean 값으로 강제 변환하여 논리적 참 거짓을 구별한다. 조건식의 결과가 언제나 참이면 무한 루프가 된다. 무한 루프를 탈출할 때는 if문으로 탈출 조건을 만들고 break를 설정하면 된다.

### 8.3.3 do ... while문

do ... while문은 코드 블록을 먼저 실행하고 조건식을 평가한다. 따라서 코드 블록은 무조건 한 번 이상 실행한다.

```javascript
do {
  console.log(count);
  count++;
} while (count < 3);
```

## 8.4 break 문

---

switch문과 while문에서 살펴보았듯이 break문은 코드 블록을 탈출한다. 좀더 정확히 하자면 코드 블록을 탈출하는 것이 아니라 레이블 문, 반복문(for, for ... in, for ... of, while, switch ....)문의 코드 블록을 탈출한다. 앞서 말한 예시 이외의 코드 블록을 탈출하려 시도시 SyntaxError가 발생한다.

참고로 label (레이블)문이랑 식별자가 붙은 statement를 의미한다.

```javascript
foo: console.log("foo");
//foo라는 레이블 식별자가 붙은 statement

foo: {
  console.log(1);
  break foo; // foo 레이블 블록문을 탈출
  console.log(2);
}
```

레이블 문은 프로그램의 실행 순서를 제어하는 데 사용한다. 사실 switch문의 case문과 default 문 역시 레이블 문으로 이러한 레이블 문을 탈출하려면 break문에 레이블 식별자를 지정한다.

실제로 break는 반복문 switch문을 탈출하는데 주로 사용하고 , 이 떄 break문에는 별도의 식별자를 지정하지 않는다.

## 8.5 continue 문

---

continue문은 반복문의 코드 블록 실행을 현 지점에서 중단하고 반복문의 증감식으로 실행 흐름을 이동시킨다. break문처럼 반복문을 탈출하지는 않는다.

```javascript
//특정 문자의 개수를 세는 코드
const string = "Hello world";
const search = "l";
let count = 0;

for (let i = 0; i < string.length; i++) {
  if (string[i] !== search) continue;
  count++; // continue문이 실행되면 이 문은 실행되지 않는다.
}
```
