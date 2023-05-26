## 5. var를 지양하자

- let, const는 ES2015 부터 생긴 것

- ES2015 이전에는 어쩔 수 없이 var 예약어만 사용해서 변수를 선언해야만 했다

### var

- var는 함수 스코프를 가진다

```js
var name = "이름";
var name = "이름2"; // 아무런 에러 없이 동일한 이름의 변수 선언 가능하다
var name = "이름3";
var name = "이름3"; // 값 똑같아도 아무러 에러 없다

console.log(name); // '이름3'
```

```js
console.log(name); // undefined

var name = "이름";
```

### let & const

- 블록 단위의 스코프를 가진다

  - 이것이 가장 중요한 핵심

- Temporal Dead Zone이라는 속성을 가질 수 있다

- 이 두 속성을 통해서 안전하게 코드를 작성할 수 있다

```js
let name = "이름3";
let name = "이름3"; // 동일한 이름의 변수 선언할 수 없다고 에러 발생
let name = "이름3";
```

```js
const name = "이름3";
const name = "이름3"; // 동일한 이름의 변수 선언할 수 없다고 에러 발생
const name = "이름3";
```

```js
// let은 재할당 가능
let name;

name = '이름';
name = '이름'1;
name = '이름2';


// const는 재할당 불가능

const name;

name = '이름' // 에러 발생
```

## 6. function scop & block scope

- var는 함수 단위 스코프를 가진다

```js
var global = "전역";

if (global === "전역") {
  // 무조건 실행된다

  var global = "지역 변수"; // 한번 더 변수를 선언하고 할당해준 것이다

  console.log(global); // 지역 변수
}

console.log(global); // 지역 변수
```

- let은 블록 레벨 스코프를 가진다

```js
let global = "전역";

if (global === "전역") {
  // 무조건 실행된다

  let global = "지역 변수";

  console.log(global); // 지역 변수
}

console.log(global); // 전역 변수
```

- 블록만 선언해도 동일한 결과 나온다

```js
let global = "전역";

{
  // 무조건 실행된다

  let global = "지역 변수";

  console.log(global); // 지역 변수
}

console.log(global); // 전역 변수
```

- 이렇게 안전하게 코딩하기 위해 let, const 사용한다

- let 보다 const를 사용하는 것이 좋다

```js

// 선언과 동시에 할당이 이뤄지고 있다
const person = {
  name:'kim',
  age:'32';
}

person = {name:'park', age:'29'} // 재할당 불가능하기 때문에 에러 발생


// 값을 바꿀 수 있는 방법은 다음과 같다

person.name = 'park'
person.age = '28'
```

- 배열의 경우 아래와 같이 처리할 수 있다

```js

// 선언과 동시에 할당이 일워지고 있다
const person =[ {
  name:'kim',
  age:'32';
}]

person.push({name:'park', age:'29'})


consolo.log(person) // 두 객체 확인할 수 있다

```

- 예시를 통해 const는 재할당만 불가능하고 객체, 배열 같은 레퍼런스 객체를 조작할 때는 문제가 없다는 것을 알 수 있다

## 7. 전역 공간 사용 최소화

- 전역공간을 사용하지 말아야할 이유

  1. 경험을 통해

  2. 누군가 혹은 자바스크립트 생태계에서 사용하지 말라고 하기에

  3. 강의 또는 책을 통해서 사용하지 말라기에

  4. 회사 혹은 멘토가 권하기에

  5. Lint에서 알려주는 경우

- 전역 공간이란

  - Global 또는 Window로 나뉜다

    - 브라우저 환경 : Window가 최상위

    - Node : Global이 최상위

  - 크게 다른 것은 없다

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <script src="./iindex1.js"></script>
    <script src="./iindex2.js"></script>
  </head>
  <body></body>
</html>
```

```js
// index1.js

var globalVar = "global";

console.log(globalVar);
console.log(window.globalVar);
```

- `window.` 해보면 globalVar가 있는 것을 확인할 수 있다

- 그리고 index2에서도 index1에 있는 변수에 접근할 수 있다

```js
// index1.js

console.log(globalVar); // 'global'
```

- 파일을 나누면 스코프(코드 구역)도 나뉘는게 아니다.

- 더 안좋은 사례는 아래와 같다

```js
// index1.js

var globalVar = "global";

console.log(globalVar);

// 1초후에 콘솔 출력된다
window.setTimeout(() => console.log("1초"), 1000);

var setTimeout = "setTimeout";
```

- 이 코드를 아래와 같이 수정해준다

```js
// index1.js

var globalVar = "global";

console.log(globalVar);

var setTimeout = "setTimeout";

// 이런식으로 변수도 선언하고 함수도 선언할 수 있다.
function setTimeout() {
  console.log("function");
}
```

- index2도 아래처럼 수정해준다

```js
// index2.js

window.setTimeout(() => console.log("1초"), 1000); // 실행 안된다, 위애 setTimeout 변수가 string으로 되었기 때문
```

- 이렇게 코드 작성해도 콘솔에는 에러가 발생하지 않는다. (실행하면 에러 발생)

- 그 이유는 브라우저 web api이기 때문에 자바스크립트 코드를 작성하는 이 단계에서는 에러가 발생하지 않는 것

<br/>

- 또 다른 예시는 아래코드이다.

```js
// index1.js

const arr = [10, 20, 30];

for (var index = 0; index < array.length; index++) {
  const element = arr[index];
}

// window. 해보면 index가 3인 것을 확인할 수 있다
```

- 즉, var인 index가 전역 스코프에서 확인될 수 있다

- 아래 개념에 대해 학습하면 왜 전역 공간을 더럽히지 말아야 하는 이유를 알 수 있다

  - IIFC(즉시 실행 함수)
  - 클로저
  - const & let

- 전역 공간 더럽히면 안되는 이유

  - 어디에서나 접근이 가능 하다

  - 스코프 분리가 위험하다

- 따라서,

  1. 전역변수를 애초애 만들지 않는다

  2. 지역 변수만 만든다

  3. window, global을 조작하지 않는다

  4. const, let 만 사용해도 많은 부분이 해결 될 수 있다.

  5. 즉시실행함수, 모듈, 클로저 등이 있는데, 이렇게 스코프를 나누는 방법에 대해서 고민해볼 필요가 있다

## 8. 임시변수 제거하기

- 임시변수란

  - 어느 특정 공간 스코프 안에서 전역변수처럼 사용되는 것

- 아래 코드의 result도 함수가 커지면 전역 공간이나 다름 없는 상황이 생길 수도 있다

- 임시객체들이 매우 위험할 수 있다

- 잘게 함수를 쪼갠나면 문제가 없겠지만, 이렇게 임시변수를 만들게 되면 문제가 생길 수 있다

```js
function getElements() {
  const result = {}; // 임시 객체, 임시 객체가 생기는 순간 접근해서 조작하고 싶은 유혹이 생길 수 있다

  result.title = document.querySelector(".title");
  result.text = document.querySelector(".text");
  result.value = document.querySelector(".value");

  return result;
}
```

- 위 함수를 아래처럼 수정해줄 수 있다

- 누가봐도 사이드 이펙트가 많지 않은 함수가 될 수 있다

```js
function getElements() {
  return   {
      title: document.querySelector('.title');
      text: document.querySelector('.text');
      value: document.querySelector('.value');
  };

}
```

- 아래는 date 객체를 사용하는 함수

- 임시 변수가 있는 이런 함수도 문제 생길 수 있다

```js
function getDateTime(targetDate) {
  let month = targetDate.getMonth();
  let day = targetDate.getDate();
  let hour = targetDate.Hours();

  month = month > 10 ? month : "0" + month;
  day = day > 10 ? day : "0" + day;
  hour = hour > 10 ? hour : "0" + hour;

  return { month, day, hour };
}
```

- 이후에 이 함수가 할 수 없는 추가적인 스펙이 생길 때 문제가 생길 수 있다

- 무엇인가 날짜에 대한 요구사항이 생길 때, 함수를 추가로 만드느냐 혹은 함수를 유지보수 하며 수정할 것이냐에 따른 고민을 할 수 있다

- 함수를 100군데 이상 사용되는 것처럼 여러군데에서 사용될 수 있기 때문에, 항상 바로 반환할 수 있는 형태로 함수를 바꿔주는 것이 좋다

- 즉, 하나의 역할만 하는 함수로 바꿔준다

```js
function getDateTime(targetDate) {
  const month = targetDate.getMonth();
  const day = targetDate.getDate();
  const hour = targetDate.Hours();

  return {
    month: (month = month > 10 ? month : "0" + month),
    day: (day = day > 10 ? day : "0" + day),
    hour: (hour = hour > 10 ? hour : "0" + hour),
  };
}
```

- 추가 수정사항이 생길 때, 아래와 같은 함수를 추가해서, 기존 함수를 재사용할 수 있다

```js
function getCurrentDateTime() {
  const currentDateTime = getDateTime(new Date());

  return {
    month: currentDateTime.month + "분 전",
    day: currentDateTime.day + "분 전",
    hour: currentDateTime.hour + "분 전",
  };
}
```

- 함수를 한번 더 추상화 했기 때문에 재사용할 수 있는 것

- 그리고 함수 내부에 만약 `computedKrDate()` 라는 함수가 사용되어야 한다면 아래처럼도 사용할 수 있다

```js
function getCurrentDateTime() {
  const currentDateTime = getDateTime(new Date());

  return {
    month: computedKrDate(currentDateTime.month) + "분 전",
    day: computedKrDate(currentDateTime.day) + "분 전",
    hour: computedKrDate(currentDateTime.hour) + "분 전",
  };
}
```

- 아래 함수와 같이 하나의 역할만 함수를 만들어주어서 임시변수의 유혹을 받지 않도록 한다

```js
function getRandomNumber(min, max) {
  const randomNumber = Math.floor(Math.random() * (max + 1) + min);

  return randomNumber;
}
```

- 즉, 임시변수를 사용하는 유혹을 받지 않도록 함수가 하나의 역할만 하도록 추상화해주면, 재사용성을 높일 수 있다

- 아래와 같은 완전한 명령헝 코드에서 temp값은 계속해서 반복문, 조건문에서 바뀌게 되고 최종적으로 이 값이 예측하기 어렵다

- 따라서 임시 변수를 사용하지 않는 방법을 고민한다

```js
// 아래 코드는 예시
function getSomeValue(params) {
  let temp = '';
  for (let i = 0; i < array.length; i++) {
    temp = array[index];
    temp = array[index];
    temp = array[index];
    temp = array[index];
  }

  if(temp ??){

  }else if (temp ??){

  }
}
```

- 임시변수를 제거한다, 그 이유는

  - 명령형으로 가득한 로직이 나온다

  - 어디서 어떻게 잘못되었는지 디버깅이 어렵다

  - 타인이 추가적인 코드를 작성하고 싶은 유혹에 빠지기 쉽다

  - 때문에 함수는 하나의 역할만 해야하는데 임시 변수가 추가됨에 따라서 코드 유지보수가 어려워 진다

- 해결책은

  - 함수를 나누는 것

  - 그리고 바로 반환하는 것

  - 고차 함수르 사용하는 방법도 있다

    - 고차함수는 map, filter, reduce

  - 선언형 코드로 바꿔보는 연습을 한다

## 9. 호이스팅 주의하기

- 호이스팅은 선언과 할당이 분리된 것

- 언제? -> 바로 런타임시

- 런타임시는 동작할 때를 말한다

- 코드를 작성할 때는 이 스코프는 이렇게 동작할 것이라고 하는데

- 런타임때는 예상한 스코프 대로 동작하지 않을 수 있고, 그 예시가 바로 호이스팅이다.

- 호이스팅은 var로 선언한 변수가 초기화가 제대로 되지 않았을 때 undefined로 최상단에 끌어올려줄 수 있는 것

- let, const 쓰면 해결된다

- var는 함수 스코프를 가지는 예제

```js
var global = 0;

function outer() {
  console.log(global); // undefined, 선언과 할당이 분리된 상황, var은 함수 레벨 스코프 가지기 때문에 전역 변수의 0 적용 안됨
  var global = 5;

  function inner() {
    var global = 10;

    console.log(global); // 10
  }

  inner();

  global = 1; // 재할당해주었다

  console.log(global); // 1
}

outer();
```

- 아래와 같은 상황으로, 선언과 할당 부분이 메모리 공간을 선언하기 전에 미리 할당하기 때문에 이러한 일이 발생하는 것

```js
var global;
console.log(global); // undefined, 선언과 할당이 분리된 상황
global = 5;
```

- 아래는 또 다른 예시

```js
function duplicatedVar() {
  var a;

  console.log(a); // undefined

  var a = 100;

  console.log(a); // 100
}
```

- 아래는 변수를 선언하고 함수를 할당한 예시로 함수가 정상적으로 동작한다

```js
var sum;

sum = function () {
  return 1 + 2;
};

console.log(sum()); //3
```

- 아래처럼 코드를 작성하도 동작하는데 함수도 호이스팅 되기 때문이다

```js
var sum;

console.log(sum()); // 3

function sum() {
  return 1 + 2;
}
```

- 그리고 아래처럼 중복 선언을 해도 동작한다

```js
var sum;

console.log(sum()); // 10

function sum() {
  return 1 + 2;
}

function sum() {
  return 1 + 2 + 3;
}

function sum() {
  return 1 + 2 + 3 + 4;
}
```

- 이 문제를 해결할 수 있는 방법은, 변수 선언 -> 할당 -> 초기화 완료 -> 정확한 분리를 하는 것이다

```js
var sum = 11;

console.log(sum); // 11

function sum() {
  return 1 + 2;
}

function sum() {
  return 1 + 2 + 3;
}

function sum() {
  return 1 + 2 + 3 + 4;
}
```

- 따라서 함수를 만들 때 const를 사용해서 함수를 선언하는 것이 좋다

- 아래처럼 함수 표현식을 사용해서 선언한다

```js
const sum = function () {
  return 1 + 2;
};

console.log(sum());
```

- 호이스팅이란 런타임시에 바로 선언을 최상단으로 끌어올려지는 것

- 문제는 코드를 작성할 때 예측하지 못한 실행 결과가 노출되는 것

- 이런 예측하지 못하는 상황을 해결하기 위해

1. var 사용을 지양한다 -> let, const

2. 함수 표현식을 사용한다
