## 32. JavaScript 배열은 객체다

```js
/**
 * JavaScript의 배열은 객체다
 */
const arr = [1, 2, 3];

// 배열에 새로운 값들을 넣고 있다.
arr[3] = "test";
arr["property"] = "string value";
arr["obj"] = {};

arr[{}] = [1, 2, 3];

arr["func"] = function () {
  return "hello";
};

// 1, 2, 3, 'test' 까지만 출력된다
// 이외의 나머지 key에 할당한 값들을 출력되지 않는다
// 이는 단순히 i가 숫자 값을 가지기 때문
for (let i = 0; i < arr.length; i++) {
  console.log(arr[i]); // 1, 2, 3, 'test'
}
```

- 하지만 아래처럼 배열을 출력하면 위에서 할당한 값들이 정상적으로 배열의 값으로 있는 것을 확인할 수 있다

```js
console.log(arr);

/*

0: 1
1: 2
2: 3
3: "test"
[object Object]: (3) [1, 2, 3]
obj: {}
property: "string value"
length: 4

*/
```

- 즉, 배열에 마치 객체에서 key와 value를 설정하듯이 값을 입력하면, 객체처럼 값이 입력된 것을 확인할 수 있다.

- 심지어 배열 내의 함수도 객체의 메서드처럼 실행할 수 있다.

```js
console.log(arr.func()); // 'hello'
```

- 이런 특징을 잘 이해하고 있어야 한다.

- 배열 여부를 확인하기 위해서 아래 두 방법은 사용하지 않는 것이 좋다

```js
/**
 * JavaScript의 배열은 객체다
 */
const arr = [1, 2, 3];

// 문자열 property에도 length가 있기 때문에 '[1, 2, 3]'도 true가 된다
if (arr.length) {
  console.log("배열 확인");
}

if (typeof arr === "object") {
  console.log("배열 확인");
}
```

- 배열 여부를 확인하려면 Array.isArray() 메서드를 사용하는 것이 좋다.

```js
if (Array.isArray(arr)) {
  console.log("배열 확인");
}
```

## 33. Array.length

- Array.length는 배열의 길이보다는 배열의 마지막 요소의 인덱스를 의미하는 것에 가깝다.

- 아래처럼 배열의 길이를 0으로 설정하면 배열이 초기화된다.

```js
const arr = [1, 2, 3];
arr.length = 0;
console.log(arr); // []
```

- Array.length의 이러한 특성을 염두하고 주의해서 사용해야 한다.

```js
/**
 * Array.length
 */

const arr = [1, 2, 3];

console.log(arr.length); // 3

arr.length = 10;

console.log(arr.length); // 10

console.log(arr) //  [1, 2, 3, , , , , , , , 10]
```

```js
/**
 * Array.length
 */
const arr = [1, 2, 3];

arr[3] = 4;

console.log(arr.length); // 4

arr[9] = 10;

console.log(arr.length); // 10,  [1, 2, 3, 4 , , , , , , , 10]
```

- 그렇기 때문에 이러한 배열의 속성을 이용해서 아래처럼 사용해서 배열을 초기화할 수 있다

```js
/**
 * Array.length
 */

// prototype의 속성을 사용
Array.prototype.clear = function () {
  this.length = 0;
};

const arr = [1, 2, 3];
arr.clear(); // arr -> []

// 함수를 정의
function clearArray(array) {
  array.length = 0;

  return array;
}
```

## 34. 배열 요소에 접근하기

```js
/**
 * 배열 요소에 접근하기
 */


// 아래처럼 사용할 때, inputs의 인덱스인 0과 1이 무엇인지 알 수 없다.
function operateTime(input, operators, is) {
  inputs[0].split("").forEach((num) => {
    cy.get(".digit").contains(num).click();
  });

  inputs[1].split("").forEach((num) => {
    cy.get(".digit").contains(num).click();
  });
}

// 위 코드를 아래처럼 개선할 수 있는데
// destructuring을 사용하면 조금 더 명시적으로 확인할 수 있다
function operateTime(input, operators, is) {
  const [firstInput, secondInput] = inputs;

  firstInput.split("").forEach((num) => {
    cy.get(".digit").contains(num).click();
  });

  secondInput.split("").forEach((num) => {
    cy.get(".digit").contains(num).click();
  });
}


// 또 다른 방법으로는 함수에서 전달받을 때 부터  destructuring을 해주는 것이다 
function operateTime([firstInput, secondInput], operators, is) {
  firstInput.split("").forEach((num) => {
    cy.get(".digit").contains(num).click();
  });

  secondInput.split("").forEach((num) => {
    cy.get(".digit").contains(num).click();
  });
}
```
- 아래 코드도 이러한 방식으로 개선할 수 있다.

```js
/**
 * 배열 요소에 접근하기
 */
function clickGroupButton() {
  const confirmButton = document.getElementsByTagName("button")[0];
  const cancelButton = document.getElementsByTagName("button")[1];
  const resetButton = document.getElementsByTagName("button")[2];

  // ...some code

  // 위 코드도 아래처럼 수정해줄 수 있다

  const [confirmButton, cancelButton, resetButton] = document.getElementsByTagName("button");
}
```

- 하나의 요소에 대해서도 destructering을 할 수 있다

```js
/**
 * 배열 요소에 접근하기
 */
function formatDate(targetDate) {

  // as-is
  const date = targetDate.toISOString().split("T")[0];

  // to-be
  const [date] = targetDate.toISOString().split("T");

  const [year, month, day] = date.split("-");

  return `${year}년 ${month}월 ${day}일`;
}
```

## 35. 유사 배열 객체

- 유사배열객체는 말 그대로 '배열'이 아닌 '객체'이다. 

- 그런데 아래 코드처럼, length 속성과 인덱싱된 요소를 가진 유사배열객체를 Array.from() 메서드를 사용하여 신기하게도 새로운 배열을 만드는 것을 볼 수 있다.

- 유사 배열 객체 (length 속성과 인덱싱 된 요소를 가진 객체)

```js
/**
 * 유사 배열 객체
 */
const arrayLikeObject = {
  0: "HELLO",
  1: "WORLD",
  length: 2,
};

console.log(Array.isArray(arrayLikeObject)); // false

const arr = Array.from(arrayLikeObject); // arr은 배열이 된다

console.log(Array.isArray(arr)); // true
console.log(arr); // [ 'HELLO', 'WORLD' ]
```

- 자바스크립트 함수 내부에 있는 arguments나 webAPI의 node list 도 유사배열객체다.

- 유사배열객체는 배열의 고차함수 메서드를 사용할 수 없다.

- Array.from() 메서드를 통해 배열로 변환해야 배열의 고차함수 메서드를 사용할 수 있다.

  - map, forEach, reduce ...

```js
/**
 * 유사 배열 객체
 */

// 매개변수 미리 선언해두지 않아도, 전달된 매개변수를 arguments라는 값으로 다룰 수 있다
function generatePriceList() {
  return arguments.map((arg) => arg + "원"); // 동작하지 않는다, arguments는 유사배열이기 때문
  // return Array.from(arguments).map((arg) => arg + "원"); // 아래처럼 배열로 변환해 주어야지 map 함수를 사용할 수 있다.
  // arguments의 __protp__를 확인해보면 map, filter, reduce 등등이 없기 때문이다
}

generatePriceList(100, 200, 300, 400, 500, 600);
```

## 36. 불변성

- 원래 배열을 새로운 배열에 할당하는 경우, 원본 배열 조작해도 newArray도 영향을 받는다

```js
/**
 * 불변성 (immutable)
 *
 * 불변성을 지키기 위해
 *
 * 1. 배열을 복사한다
 * 2. 새로운 배열을 반환하는 메서드들을 활용한다
 */

const originArray = ["123", "456", "789"];
const newArray = originArray;

originArray.push(10);
originArray.push(11);
originArray.push(12);
originArray.unshift(0);
// newArray도 변경된다
```

- 배열을 복사해서 사용하거나 새로운 배열을 반환하는 메서드를 사용해야한다

  - map, filter, slice ...

```js
const numbers = [1, 2, 3];

const numbersCopy = [...numbers];
```

## 37. for 문 배열 고차 함수로 리팩터링

```js
/**
 * 배열 고차 함수
 *
 * 1. 원화 표기
 */

const price = ["2000", "1000", "3000", "5000", "4000"];

function getWonPrice(priceList) {
  // let temp = [];

  // for (let i = 0; i < priceList.length; i++) {
  //   temp.push(priceList[i] + "원");
  // }

  // return temp;

  // 위 로직을 아래처럼 대신할 수 있다
  return priceList.map((price) => price + "원");
}
```

- 또는 아래처럼 별도의 함수로 빼줄 수 있다

```js
const price = ["2000", "1000", "3000", "5000", "4000"];

// 별도의 함수로 빼준다
const suffix = (price) => price + "원";

function getWonPrice(priceList) {
  return priceList.map(suffix);
}
```

- 아래처럼 1000원 초과 리스트만 출력 추가 요구사항이 있는 경우에는 다음과 같이 수정해줄 수 있다.

```js
/**
 * 배열 고차 함수
 *
 * 1. 원화 표기
 * 2. 1000원 초과 리스트만 출력
 */

const price = ["2000", "1000", "3000", "5000", "4000"];

const suffix = (price) => price + "원";
const isOverOneThousand => (price) => Number(price) > 1000

function getWonPrice(priceList) {
  const isOverList = priceList.filter(isOverOneThousand)
  return priceList.map(suffix);
}
```

## 38. 배열 메서드 체이닝 활용하기

- 추가적으로 가격 순 정렬 요구사항이 있을 수 있다고 하자

```js
/**
 * 배열 고차 함수 => 체이닝
 *
 * 1. 원화 표기
 * 2. 1000원 초과 리스트만 출력
 * 3. 가격 순 정렬
 */
const price = ["2000", "1000", "3000", "5000", "4000"];

function getWonPrice(priceList, orderType) {
  let temp = [];

  for (let i = 0; i < priceList.length; i++) {
    if (priceList[i] > 1000) {
      temp.push(priceList[i] + "원");
    }
  }

  if (orderType === "ASCENDING") {
    someAscendingSortFunc(temp);
  }

  if (orderType === "DESCENDING") {
    someDescendingSortFunc(temp);
  }

  return temp;
}
```

- for문을 사용하는 위 코드를 다음과 같이 수정할 수 있다

```js
/**
 * 배열 고차 함수 => 체이닝
 *
 * 1. 원화 표기
 * 2. 1000원 초과 리스트만 출력
 * 3. 가격 순 정렬
 */

const price = ["2000", "1000", "3000", "5000", "4000"];

const suffixWon = (price) => price + "원";
const isOverOneThousand = (price) => Number(price) > 1000;
const ascendingList = (a, b) => a - b;

function getWonPrice(priceList) {
  const isOverList = priceList.filter(isOverOneThousand);
  const sortList = isOverList.sort(ascendingList);

  return sortList.map(suffixWon);
}
```

- 이렇게 코드를 개선하더라도 해당 로직들이 늘어날 수 있는데 그 때마다 `--List`로 처리를 해주는 함수도 늘어나게 된다

- 이 때 사용할 수 있는 것이 메서드 체이닝이다

- 메서드 체이닝을 사용해서 아래와 같이 조금 더 간결하게 선언적으로 코드를 작정할 수 있다

```js
function getWonPrice(priceList) {
  return priceList
    .filter(isOverOneThousand) // filter 원하는 조건에 맞는 배열 리스트 만들기
    .sort(ascendingList) // sort 정렬
    .map(suffixWon); // map을 통해서 배열 요소들을 다시 정리
}
```

## 39. map vs forEach

- forEach

  - 요소마다 함수만 실행, 반환값은 undefined

  - 리턴이 필요 없을 때 사용

- map

  - 요소마다 함수를 실행해서 새로운 배열 반환

  - 리턴이 필요할 때 사용

```js
/**
 * map vs forEach
 */
const prices = ["1000", "2000", "3000"];

prices.forEach((price) => console.log(price + "원"));
prices.map((price) => console.log(price + "원"));

const newPricesForEach = prices.forEach((price) => price + "원"); // undefined

const newPricesMap = prices.map((price) => price + "원"); // ['1000원', '2000원', '3000원']
```

## 40. Continue & Break

- 고차함수에서는 continue, break 등 사용이 불가능하다

- 아래처럼 forEach와 같은 고차함수를 내에서 break가 있으면 문법 에러가 발생한다

```js

/**
 * Continue & Break
 */

const orders = ['first', 'second', 'third'];

orders.forEach(function(order) {
  if(order === 'second') {
    break;
  }

  console.log(order);
});
```

- 따라서 아래처럼 try-catch를 사용한다

```js
const orders = ["first", "second", "third"];

try {
  orders.forEach(function (order) {
    if (order === "second") {
      throw new Error("error");
    }

    console.log(order);
  });
} catch (e) {}
```

- 아니면 for of 또는 for in을 사용해서 break, continue를 사용한다

```js
for (const variable in object) {
  statement;
}

for (const variable of object) {
  statement;
}

/*
variable
매번 반복마다 다른 속성이름(Value name)이 변수(variable)로 지정된다.

object
반복작업을 수행할 객체로 열거형 속성을 가지고 있는 객체
*/
```

- 또는, every, some, find,, findeIndex 와 같은 함수를 사용해서 특정 조건이 만족되면 배열이 종료되도록 한다
