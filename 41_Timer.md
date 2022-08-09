# 41장. 타이머

## 41.1 호출 스케줄링

함수를 명시적으로 호출하지 않고 일정 시간이 경과된 이후에 호출되도록 함수 호출을 예약할 때 타이머 함수를 사용한다. 이를 호출 스케줄링(scheduling a call)이라 한다.

타이머를 생성할 수 있는 타이머 함수 setTimeout과 setInterval, 타이머를 제거할 수 있는 타이머 함수 clearTimeout과 clearInterval을 제공한다.

## 41.2 타이머 함수

### 41.2.1 setTimeout / clearTimeout

setTimeout 함수는 두 번째 인수로 전달받은 시간(ms, 1/1000초)으로 단 한 번 동작하는 타이머를 생성한다. 이후 타이머가 만료되면 첫 번째 인수로 전달받은 콜백 함수가 호출된다. 즉, 콜백 함수는 두 번째 인수로 전달받은 시간 이후 단 한 번 실행되도록 호출 스케줄링된다.

```javascript
const timeoutId = setTimeout(func|code[, delay, param1, param2, ...]);
```

|매개변수|설명|
|---|---|
|func|타이머가 만료된 뒤 호출될 콜백 함수|
|delay|타이머 만료 시간(밀리초 단위), setTimeout 함수는 delay 시간을 단 한 번 동작하는 타이머를 생성한다. 인수 전달을 생략한 경우 기본값 0이 지정된다.|
|param1,|호출 스케줄링된 콜백 함수에 전달해야 할 인수가 존재하는 경우 세 번째 이후의 인수로 전달할 수 있다.|

```javascript
setTimeout(() => conssole.log('HI!'), 1000);

setTimeout(() => conssole.log(`HI! ${name}.`), 1000, 'Lee');
```

setTimeout 함수는 생성도니 타이머를 식별할 수 있는 고유한 타이머 id를 반환한다. 타이머 id는 브라우저 환경의 경우 숫자이며 Node.js 환경인 경우 객체다. setTimeout 함수가 반환한 타이머 id를 clearTimeout 함수의 인수로 전달하여 타이머를 취소할 수 있다.

```javascript
const timerID = setTimeout(() => conssole.log('HI!'), 1000);

clearTimeout(timerID);  //타이머가 취소되면 setTimeout 함수의 콜백 함수가 실행되지 않는다.
```

### 41.2.2 setInterval / clearInterval

setInterval 함수는 두 번째 인수로 전달받은 시간으로 반복 동작하는 타이머를 생성한다. 이후 타이머가 만료될 때마다 첫 번째 인수로 전달받은 콜백 함수가 반복 호출된다.

```javascript
const timerId = setInterval(func|code[, delay, param1, param2, ...]);
```

setInterval 함수가 반환한 타이머 id를 clearInterval 함수의 인수로 전달하여 타이머를 취소할 수 있다.

```javascript
let count = 1;

const timeroutId = setInterval(() => {
    conssole.log(count);
    if (count++ === 5) clearInterval(timeroutId);
}, 1000);
```

## 41.3 디바운스와 스로틀

scroll, resize, input, mousemove 같은 이벤트는 짧은 시간 간격으로 연속해서 발생한다. 이러한 이벤트에 바인딩한 이벤트 핸들러는 과도하게 호출되어 성능에 문제를 일으킬 수 있다.

디바운스와 스로틀은 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 과도한 이벤트 핸들러의 호출을 방지하는 프로그래밍 기법이다.

### 41.3.1 디바운스

디바운스는 짧은 시간 간격으로 에번트가 연속해서 발생하면 이벤트 핸들러를 호출하지 않다가 일정 시간이 경과한 이후에 이벤트 핸들러가 한 번만 호출되도록 한다. 즉, 디바운스는 짧은 시간 간격으로 발생하는 이벤트를 그룹화해서 마지막에 한 번만 이벤트 핸들러가 호출되로록 한다.

```javascript
<!DOCTYPE html>
<html>
<body>
    <input type="text">
    <div class="msg"></div>
    <script>
        const $input = document.querySelector('input');
        const $msg = document.querySelector('.msg');
        const debounce = (callback, delay) => {
            let timerId;  // debounce 함수는 timerId를 기억하는 클로저를 반환한다.
            return event => {
                if (timerId) clearTimeout(timerId);
                timerId = setTimeout(callback, delay, event);
            };
        };

        $input.oninput = debounce(e => {
            $msg.textContent = e.target.value;
        }, 300);
    </script>
</body>
</html>
```

input 이벤트는 사용자가 텍스트 입력 필드에 값을 입력할 때마다 연속해서 발생한다.

사용자가 입력을 완료했는지 여부는 정확히 알 수 없으므로 일정 시간 동안 텍스트 입력 필드에 값을 입력하지 않으면 입력이 완료된 것으로 간주한다. 이를 위해 debounce 함수가 반환한 함수는 debounce 함수에 두 번째 인수로 전달한 시간(delay)보다 짧은 간격으로 이벤트가 발생하면 이전 타이머를 취소하고 새로운 타이머를 재설정한다. 따라서 delay보다 짧은 간격으로 이벤트가 연속해서 발생하면 debounce 함수의 첫 번째 인수로 전달한 콜백 함수는 호출되지 않다가 delay 동안 input 이벤트가 더 이상 발생하지 않으면 한 번만 호출된다.

디바운스는 resize 이벤트 처리나 input 요소에 입력된 값으로 ajax 요청하는 입력 필드 자동완성 UI 구현, 버튼 중복 클릭 방지 처리 등에 유용하게 사용된다.

### 41.3.2 스로틀

스로틀은 짧은 시간 간격으로 이벤트가 연속해서 발생하더라도 일정 시간 간격으로 이벤트 핸들러가 최대 한 번만 호출되도록 한다. 즉, 스로틀은 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 일정 시간 단위로 이벤트 핸들러가 호출되로록 호출 주기를 만든다.

```javascript
<!DOCTYPE html>
<html>
<head>
    <style>
        .container {
            width: 300px;
            height: 300px;
        }

        .content {
            width: 300px;
            height; 1000vh;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="content"></div>
    </div>
    <div>
        // 일반 이벤트 핸들러가 scroll 이벤트를 처리한 횟수 :
        <span class="normal-count">0</span>
    </div>
    <div>
        // 스로틀 이벤트 핸들러가 scroll 이벤트를 처리한 횟수 :
        <span class="throttle-count">0</span>
    </div>
    <script>
        const $container = document.querySelector('.container');
        const $normalCount = document.querySelector('.normal-count');
        const $throttleCount = document.querySelector('.throttle-count');
        
        const throttle = (callback, delay) => {
            let timerId;  // throttle 함수는 timerId를 기억하는 클로저를 반환한다.
            return event => {
                if (timerId) return;
                timerId = setTimeout(() => {
                    callback(event);
                    timerId = null;
                }, delay, event);
            };
        };
        
        let normalCount = 0;
        $container.addEventListerner('scroll', () => {
            $normalCount.textContent = ++normalCount;
        });

        let throttleCount = 0;
        $container.addEventListerner('scroll', throttle(() => {
            $throttleCount.textContent = ++throttleCount;
        }, 100));  // throttle 함수가 반환하는 클로저가 이벤트 핸들러로 등록된다.
    </script>
</body>
</html>
```

짧은 시간 간격으로 연속해서 발생하는 이벤트의 과도한 이벤트 핸들러의 호출을 방지하기 위해 throttle 함수는 이벤트를 그룹화해서 일정 시간 단위로 이벤트 핸들러가 호출되도록 호출 주기를 만든다.

throttle 함수가 반환한 함수는 throttle 함수에 두 번째 인수로 전달한 시간(delay)이 경과하기 이전에 이벤트가 발생하면 아무것도 하지 않다가 dalay 시간 간격으로 콜백 함수가 호출된다.

스로틀은 scroll 이벤트 처리나 무한 스크롤 UI 구현 등에 유용하게 사용된다.
