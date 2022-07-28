# 30. Date

**Date** : 날짜 & 시간(연, 월, 일, 시, 분, 초, 밀리초)를 위한 메서드를 제공하는 표준 빌트인 객체 & 생성자 함수
UTC(= GMT) : 국제 표준시
KST(한국 표준시) = UTC + 9h(9h 빠름)
현재 날짜 & 시간 - JS 코드가 실행된 시스템의 시계에 의해 결정됨
***

## 30.1 Date 생성자 함수

Date 생성자 함수로 생성한 Date 객체 - 내부적으로 현재 날짜 & 시간을 나타내는 정수값 가짐
> **Date 생성자 함수로 객체를 생성하는 방법 4가지**
>
> 1. new Date()
> 2. new Date(milliseconds)
> 3. new Date(dateString)
> 4. new Date(year, month[, day, hour, minute, second, millisecond])

### 30.1.1 new Date()

1)인수 없이 new 연산자와 함께 호출하는 경우
: 현재 날짜 & 시간을 가지는 Date 객체를 반환함

```javascript
new Date(); // -> Thu Jul 28 2022 01:03:18 GMT+0900 (대한민국 표준시)
```

2)인수 없이 new 연산자 없이 호출하는 경우
: Date 객체 반환 X, 날짜 & 시간 정보를 나타내는 문자열을 반환

```javascript
Date(); // -> "Thu Jul 28 2022 01:03:18 GMT+0900 (대한민국 표준시)"
```

### 30.1.2 new Date(milliseconds)

Date 생성자 함수에 숫자 타입의 밀리초를 인수로 전달
=> 1970년 1월 1일 00:00:00(UTC)을 기점으로 인수로 전달된 밀리초만큼 경과한 날짜 & 시간을 나타내는 Date 객체 반환

```javascript
new Date(0); // -> Thu Jul 01 1970 09:00:00 GMT+0900 (대한민국 표준시)

/*
1d = 24h * 60 * 60 * 1000ms = 86400000ms
*/
new Date(86400000); // -> Fri Jan 02 970 09:00:00 GMT+0900 (대한민국 표준시)
```

### 30.1.3 new Date(dateString)

Date 생성자 함수에 날짜 & 시간을 나태내는 문자열을 인수로 전달
=> 지정된 날짜 & 시간을 나타내는 Date 객체 반환

- 인수로 전달한 문자열 - Date.parse 메서드에 의해 해석 가능한 형식이어야 함

```javascript
new Date('May 26, 2020 10:00:00');
// -> Tue May 26, 2020 10:00:00 GMT+0900 (대한민국 표준시)

new Date('2020/03/26/ 10:00:00');
// -> Tue May 26, 2020 10:00:00 GMT+0900 (대한민국 표준시)
```

### 30.1.4 newDate(year, month[, day, hour, minute, second, millisecond])

Date 생성자 함수에 연, 월, 일, 시, 분, 초, 밀리초를 의미하는 숫자를 인수로 전달
=> 지정된 날짜 & 시간을 나타내는 Date 객체 반환

- 이때 연,월은 반드시 지정해야 함!
- 지정하지 X 옵션 정보 -> 0 or 1로 초기화됨
| 인수 | 내용 |
|-----|-----|
| year | 연을 나타내는 1900년 이후의 정수. 0~99 -> 1900~1999로 처리됨 |
| month | 월을 나타내는 0~11까지의 정수(주의: 0부터 시작, 0=1월) |
| day | 일을 나타내는 1~31까지의 정수 |
| hour | 시를 나타내는 0~23까지의 정수 |
| minute | 분을 나타내는 0~59까지의 정수 |
| second | 초를 나타내는 0~59까지의 정수 |
| millisecond | 밀리초를 나타내는 0~999까지의 정수 |
- 연, 월 지정 X -> 1970년 1월 1일 00:00:00(UTC)을 나타내는 Date 객체 반환

```javascript
// 월을 나타내는 2 => 3월 의미함 2020/3/1 00:00:00:00
new Date(2020, 2);
// -> Sun Mar 01 2020 00:00:00 GMT+0900 (대한민국 표준시)

// 월을 나타내는 2 => 3월 의미함 2020/3/26 10:00:00:00
new Date(2020, 2, 26, 10, 00, 00, 0);
// -> Thu Mar 26 2020 00:00:00 GMT+0900 (대한민국 표준시)

// 가독성 좋은 표현 방식
new Date('2020/3/26/10:00:00');
// -> Thu Mar 26 2020 10:00:00 GMT+0900 (대한민국 표준시)
```

***

## 30.2 Date 메서드

### 30.2.1 Date.now

1970년 1월 1일 00:00:00(UTC)을 기점으로 현재 시간까지 경과한 밀리초 -> 숫자로 변환

### 30.2.2 Date.parse

1970년 1월 1일 00:00:00(UTC)을 기점으로 인수로 전달된 지정시간까지의 밀리초 -> 숫자로 변환

- new Date(dateString)와 같은 형식의 인수 사용

### 30.2.3 Date.UTC

1970년 1월 1일 00:00:00(UTC)을 기점으로 인수로 전달된 지정시간까지의 밀리초 -> 숫자로 변환

- new Date(year, month[, day, hour, minute, second, millisecond])와 같은 형식의 인수 사용
- 인수 - 로컬 타입(KST)가 아닌 UTC로 인식됨
- month : 0~11까지의 정수, 0부터 시작하므로 주의!

***

### 30.2.4 Date.prototype.getFullYear

Date 객체의 연도를 나타내는 정수 변환

```javascript
new Date('2020/07/24').getFullYear(); // ->2020
```

### 30.2.5 Date.prototype.setFullYear

- Date 객체에 연도를 나타내는 정수를 설정
- 옵션 : 월, 일 설정 가능 (인수에 같이 순서대로 넣어주면 됨)

```javascript
const today = new Date(); // -> 현재 날짜 & 시간을 가지는 Date 객체 생성

// 년도/월/일 지정
today.setFullYear(1900,0,1);
today.getFulYear(); // -> 1900
```

***

### 30.2.6 Date.prototype.getMonth

Date 객체의 월을 나타내는 정수 변환 (0~11)

```javascript
new Date('2020/07/24').getMonth(); // -> 6
```

### 30.2.7 Date.prototype.setMonth

- Date 객체에 월을 나타내는 정수 설정 (0~11)
- 옵션 : 일 설정 가능

```javascript
const today = new Date();

// 년도/월/일 지정
today.setFullYear(1900,0,1);
today.getFulYear(); // -> 1900
```

***

### 30.2.8 Date.prototype.getDate

Date 객체의 날짜를 나타내는 정수 반환 (1~31)

```javascript
new Date('2020/07/24').getDate(); // -> 24
```

### 30.2.9 Date.prototype.setDate

Date 객체에 날짜를 나타내는 정수 설정 (1~31)

```javascript
const today = new Date();

today.setDate(1);
today.getDate(); // -> 1
```

***

### 30.2.10 Date.prototype.getDay

Date 객체의 요일을 나타내는 정수 반환 (0~6)
| 요일 | 반환값 |
|-----|-----|
| 일요일  | 0 |
| 월요일 | 1 |
| 화요일 | 2 |
| 수요일 | 3 |
| 목요일 | 4 |
| 금요일 | 5 |
| 토요일 | 6 |

```javascript
new Date('2020/07/24').getDay(); // -> 5 = 금요일
```

***

### 30.2.11 Date.prototype.getHours

Date 객체의 시간을 나타내는 정수 반환 (0~23)

```javascript
new Date('2020/07/24/12:00').getHours(); // -> 12
```

### 30.2.12 Date.prototype.setHours

- Date 객체에 시간을 나타내는 정수 설정 (0~23)
- 옵션 : 분, 초, 밀리초 설정 가능

```javascript
const today = new Date();

// 시간/분/초/밀리초 지정
today.setHours(0, 0, 0, 0); // 00:00:00:00
today.getHours(); // -> 0
```

***

### 30.2.13 Date.prototype.getMinutes

Date 객체의 시간을 나타내는 정수 반환 (0~59)

```javascript
new Date('2020/07/24/12:30').getMinutes(); // -> 30
```

### 30.2.14 Date.prototype.setMinutes

- Date 객체의 시간을 나타내는 정수 설정 (0~59)
- 옵션 : 분, 초, 밀리초 설정 가능

```javascript
const today = new Date();

// 분/초/밀리초 지정
today.setMinutes(5, 10, 999); // HH:05:10:999
today.getMinutes(); // -> 5
```

***

### 30.2.15 Date.prototype.getSeconds

Date 객체의 초를 나타내는 정수 반환 (0~59)

```javascript
new Date('2020/07/24/12:30:10').getSeconds(); // -> 10
```

### 30.2.16 Date.prototype.setSeconds

- Date 객체에 초를 나타내는 정수 설정 (0~59)
- 옵션 : 밀리초 설정 가능

```javascript
const today = new Date();

// 초/밀리초 지정
today.setSeconds(10, 0); // HH:MM:10:000
today.getSeconds(); // -> 10
```

***

### 30.2.17 Date.prototype.getMilliseconds

Date 객체의 밀리초를 나타내는 정수 반환 (0~999)

```javascript
new Date('2020/07/24/12:30:10:150').getMilliseconds(); // -> 150
```

### 30.2.18 Date.prototype.setMilliseconds

- Date 객체에 밀리초를 나타내는 정수 설정 (0~999)

```javascript
const today = new Date();

// 밀리초 지정
today.setMilliseconds(123);
today.getMilliseconds(); // -> 123
```

***

### 30.2.19 Date.prototype.getTime

1970년 1월 1일 00:00:00(UTC)을 기점으로 Date 객체의 시간까지 경과된 밀리초 반환

```javascript
new Date('2020/07/24/12:30').getTime(); // -> 1595561400000
```

### 30.2.20 Date.prototype.setTime

- Date 객체에 1970년 1월 1일 00:00:00(UTC)을 기점으로 경과된 밀리초를 설정
- 옵션 : 밀리초 설정 가능

```javascript
const today = new Date();

today.setTime(86400000); // 86400000ms = 1day임
console.log(today); // -> Fri Jan 02 1970 09:00:00 GMT+0900 (대한민국 표준시)
```

***

### 30.2.21 Date.prototype.getTimezoneOffset

- UTC & Date 객체에 지정된 로캘(locale) 시간과의 차이를 분 단위로 반환
- KST = UTC에 9시간 더한 시간
=> UTC = KST - 9h

```javascript
const today = new Date(); // today의 지정 로캘 = KST

//UTC & today의 지정 로캘 KST와의 차이 = -9시간임
today.getTimezoneOffset() / 60; // 분 단위니까 /60 => -9시간
```

***

### 30.2.22 Date.prototype.toDateString

사람이 읽을 수 있는 형식의 문자열로 Date 객체의 날짜를 반환

```javascript
const today = new Date('2020/7/24/12:30');

today.toString(); // -> Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
today.toDateString(); // -> Fri Jul 24 2020
```

### 30.2.23 Date.prototype.toTimeString

사람이 읽을 수 있는 형식으로 Date 객체의 시간을 표현한 문자열을 반환

```javascript
const today = new Date('2020/7/24/12:30');

today.toString(); // -> Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
today.toTimeString(); // -> 12:30:00 GMT+0900 (대한민국 표준시)
```

### 30.2.24 Date.prototype.toISOString

ISO 8601 형식으로 Date 객체의 날짜 & 시간을 표현한 문자열을 반환

```javascript
const today = new Date('2020/7/24/12:30');

today.toString(); // -> Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
today.toISOString(); // -> 2020-7-24T03:30:00.000Z

today.toISOString().slice(0,10); // -> 2020-7-24
today.toISOString().slice(0,10).replace(/-/g, ''); // -> 20200724
```

### 30.2.25 Date.prototype.toLocaleString

- 인수로 전달한 로캘을 기준으로 Date 객체의 날짜 & 시간을 표현한 문자열을 반환
- 인수 생략한 경우 : 브라우저가 동작 중인 시스템의 로캘을 적용

```javascript
const today = new Date('2020/7/24/12:30');

today.toString(); // -> Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
today.toLocaleString(); // 인수 생략 -> 2020. 7. 24. 오후 12:30:00
today.toLocaleString('ko-KR'); // -> 2020. 7. 24. 오후 12:30:00
today.toLocaleString('en-US'); // -> 7/24/2020, 12:30:00 PM
```

### 30.2.26 Date.prototype.toLocaleTimeString

- 인수로 전달한 로캘을 기준으로 Date 객체의 시간을 표현한 문자열을 반환
- 인수 생략한 경우 : 브라우저가 동작 중인 시스템의 로캘을 적용

```javascript
const today = new Date('2020/7/24/12:30');

today.toString(); // -> Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
today.toLocaleString(); // 인수 생략 -> 오후 12:30:00
today.toLocaleString('ko-KR'); // -> 오후 12:30:00
today.toLocaleString('en-US'); // -> 12:30:00 PM
```

***

## 30.3 Date를 활용한 시계 예제

다음 예제는 현재 날짜 & 시간을 초 단위로 반복 출력함

```javascript
(function printNow() {
  const today = new Date();
 
  const dayNames = [
    '(일요일)',
    '(월요일)',
    '(화요일)',
    '(수요일)',
    '(목요일)',
    '(금요일)',
    '(토요일)'
  ];
  
  // getDay 메서드 : 해당 요일을 나타내는 정수 반환 (0~6)
  const day = dayNames[today.getDay()];
  
  const year = today.getFullYear();
  const month = today.getMonth() + 1;
  const date = today.getDate();
  let hour = today.getHours();
  let minute = today.getMinutes();
  let second = today.getSeconds();
  const ampm = hour >= 12 ? 'PM' : 'AM';
  
  // 12시간제로 변경
  hour %= 12;
  hour = hour || 12; // hour가 0이면 12를 재할당
  
  // 10 미만인 분 & 초를 2자리로 변경
  minute = minute < 10 ? '0' + minute : minute;
  second = second < 10 ? '0' + second : second;
  
  const now = `${year}년 ${month}월 ${date}일 ${day} ${hour}:${minute}:${second} ${ampm}`;
  
  console.log(now);
  
  // 1초마다 printNow 함수를 재귀 호출함
  // 41.2.1절 "setTimeout / clearTimeout" 참고
  setTimeout(printNow, 1000);
}());
```
