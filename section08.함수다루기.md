## 50. 함수, 메서드, 생성자

- 함수 : 1급 객체로 동작한다
  
  - 변수나 데이터에 담길 수 있음.
  
  - 매개변수로 전달 가능 (콜백 함수)
  
  - 함수가 함수를 반환 (고차 함수)

```js
// 함수
function func() {
  return this; // 함수의 this는 전역 객체
}

func();
```

- 매서드 : 객체에 의존성이 있는 함수, OOP 행동을 의미함

```js
// 객체의 메서드

const obj = {
  // Conscise method 방식
  // key는 method 이고 value는 return 되는 this
  method() {
    return this;
  },
};

obj.method(); // 메서드의 this는 호출한 객체
```

- 생성자 : class를 만드는 함수 (현재는 사용 X)

```js
// 생성자 함수 => 인스턴스를 생성하는 역할 => Class
function Func() {
  return this;
}

const instance = new Func(); // 생성자 함수의 this는 생성될 인스턴스
```

## 51. argument & parameter

- parameter : 함수의 정의에서 사용되는 값

- argument : 함수를 호출 할 때 사용하는 값

- [mdn - parameter](https://developer.mozilla.org/ko/docs/Glossary/Parameter)

```js
/**
 * Argument & Parameter
 *
 * 매개변수, 인자, 인수....
 */

/**
 * Parameter (Formal Parameter)
 *
 * 형식을 갖춘, 매개변수
 */
function axios(url) {
  // some code
}

/**
 * Argument (Actual Parameter)
 *
 * 실제로 사용되는, 인자 or 인수
 */
axios("https://github.com");
```

## 52. 복잡한 인자 관리하기

- 파라미터가 여러개라도 파라미터의 순서가 있으면 파악하기가 쉬움.

- 순서가 상관없으면 객체 구조할당을 통해서 받기

- 통으로 객체를 받기보다는, 구조할당을 통해서 객체를 받을 것!

  - 섹션 6참고

```js
/**
 * 복잡한 인자 관리하기
 */
function toggleDisplay(isToggle) {
  // ...some code
}

function sum(sum1, sum2) {
  // ...some code
}

function genRandomNumber(min, max) {
  // ...some code
}

// 무조건 3개 이상은 나쁘다고 볼 수 없다

function timer(start, stop, end) {
  // ...some code
}

// 사각형 만들기 위해 4가지가 필요하다는 것 알 수 있다.
function genSquare(top, right, bottom, left) {
  // ...some code
}
```

- 반면 아래 함수는 전달해야 하는 인자들의 맥락을 파악하기 힘들다

```js
/**
 * 복잡한 인자 관리하기
 */
function createCar(name, brand, color, type) {
  return {
    name,
    brand,
    color,
    type,
  };
}
```

- destructuring 을 사용해서 인자를 관리할 수 있다

```js
/**
 * 복잡한 인자 관리하기
 */
function createCar({ name, brand, color, type }) {
  if (!name) {
    throw new Error("name is a required");
  }

  if (!brand) {
    throw new Error("brand is a required");
  }
}

// 아래처럼 정말 핵심적인 인자의 경우 아래처럼 빼줄 수 있다
function createCar(name, { brand, color, type }) {
  if (!name) {
    throw new Error("name is a required");
  }

  if (!brand) {
    throw new Error("brand is a required");
  }
}
```

## 53. Default Value

```js
/**
 * default value
 */

// 아래는 고전적인 코드의 예시
function createCarousel(options) {
  options = options || {}; // 방어코드가 필요하다

  // nullish 가 없는 경우 아래처럼 optional로 처리해주었다
  var margin = options.margin || 0;
  var center = options.center || false;
  var navElement = options.navElement || "div";

  // ..some code
  return {
    margin,
    center,
    navElement,
  };
}

createCarousel();
```

- 위 코드와 동일하게 동작하는 코드이다.

- 구조분해 할당을 통해 객체를 받을 경우 기본 값을 세팅해서 받는다

```js
/**
 * default value / default parameter
 */
function createCarousel({
  margin = 0,
  center = false,
  navElement = "div",
} = {}) {
  // ..some code
  return {
    margin,
    center,
    navElement,
  };
}

createCarousel(); // 에러 발생하지 않는다 '= {}' 때문
```

- 아래처럼 함수를 사용해서 필요한 값이 들어오지 않았을 때 에러를 던지도록 할 수 있다

```js
/**
 * default value
 */
const required = (argName) => {
  throw new Error("required is " + argName);
};

function createCarousel({
  items = required("items"), // 필수로 들어와야 한다
  margin = 0,
  center = false,
  navElement = "div",
} = {}) {
  // ... some code

  return {
    margin,
    center,
    navElement,
  };
}

// 에러 발생
console.log(
  createCarousel({
    margin: 10,
    center: true,
    navElement: "span",
  })
);
```

## 54. Rest Parameters

- 스프레이드 오퍼레이터와는 다르다.

- 배열로 받아서 사용 가능

- 파라미터 중 가장 나중에 받아야함.

```js
/**
 * Rest Parameters
 */
function sumTotal() {
  Array.isArray(arguments); // false

  // 배열로 변환해서 고차함수와 사용한다
  return Array.from(arguments).reduce((acc, curr) => acc + curr);
}

sumTotal(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
```

- 아래는 개선된 코드

```js
/**
 * Rest parameters
 */

function sumTotal(...args) {
  return args.reduce((acc, curr) => acc + curr, initValue);
}
sumTotal(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

// 추가적인 인자도 받을 수 있다.
function sumTotal(initValue, bonusValue, ...args) {
  return args.reduce((acc, curr) => acc + curr, initValue);
}

sumTotal(100, 99, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
```

## 55. void & return

- void 함수에는 함수의 반환이 존재하지 않기 때문에 return을 사용하지 말 것.

- 각 함수마다 반환값들이 있으니 공식문서를 참고해서 볼 것.

  - js에서 push 함수를 리턴하면 length가 나온다

  - void 함수를 리턴하면 undefind가 나온다

```js
/**
 * void & return
 */

// 반환 없기 때문에 return 사용하지 말 것
// 자바스크립트에서는 리턴 값이 없는 경우에는 undefined를 return 한다
function handleClick() {
  return setState(false);
}

function showAlert(message) {
  return alert(message);
}

function test(sum1, sum2) {
  const result = sum1 + sum2;
}

test(1, 2); // undefined

// return이 없는 함수를 return 하는 경우
function testVoidFunc() {
  return test(1, 2);
}

testVoidFunc(); // undefined
```

- 리턴이 필요할 경우 네이밍부터 잘해주자.

```js
/**
 * void & return
 */
function isAdult(age) {
  return age > 19;
}

function getUserName(name) {
  return "유저 " + name;
}
```

## 56. 화살표 함수

- 맹목적으로 화살표 사용하는 것이 아니라 화살표 함수 사용할 때 상황별로 조심해야할 때가 있다

- 메소드 안에서 화살표 함수를 사용할 경우 this 조작법 주의해야 한다

- call, apply, bind 모두 사용 불가

```js
/**
 * Arrow Function
 */
const user = {
  name: "Poco",
  getName: () => {
    return this.name;
  },
};

user.getName(); // undefined, 화살표 함수는 렉시컬 스코프를 가지기 때문에 선언될 시점에서의 상위 스코프가 this로 바인딩됩니다.

const user = {
  name: "Poco",
  getName() {
    return this.name;
  },
};

user.getName(); // "Poco"
```

- arguments 객체에서 this 사용 불가 하다 (rest parameter 사용 가능, ...args)

```js
/**
 * Arrow Function
 */
const user = {
  name: "Poco",
  getName: () => {
    return this.name;
  },
  newFriends: () => {
    const newFriendList = Array.from(arguments);

    return this.name + newFriendList;
  },
};

user.newFriends("Jang", "장"); // Uncaught ReferenceError: arguments is not defined
```

- 생성자 함수로도 사용할 수 없다

```js
const Person = (name, city) => {
  this.name = name;
  this.city = city;
};

const person = new Person("poco", "korea"); // 에러 발생
```

- 아래처럼 class를 사용할 때도 주의할 점들이 있다.

```js
class Parent {
  parentMethod() {
    console.log("parentMethod");
  }

  parentMethodArrow = () => {
    console.log("parentMethodArrow");
  };

  overrideMethod = () => {
    return "Parent";
  };
}

class Child extends Parent {
  childMethod() {
    super.parentMethod(); // 정상적으로 호출된다
    super.parentMethodArrow(); // 부모 클래스에서 화살표 함수로 선언된 함수를 호출하면 에러가 발생한다
  }

  overrideMethod() {
    return "Child";
  }
}

new Child().childMethod();
new Child().overrideMethod();
// 부모 - 화살표 함수, 자식 - 일반함수 인 경우, 오버라이딩 해도 부모 함수가 호출된다
// 부모 함수를 일반 함수로 변경하면 정상적으로 동작한다
```

## 57. Callback Function

- 공통되는 함수들을 받아서 새로운 함수를 만들 수 있다.

- 콜백함수는 함수를 다른 함수에 넘겨서 제어권을 위임할 수 있다

```js
function register() {
  const isConfirm = confirm("회원가입에 성공했습니다.");

  if (isConfirm) {
    redirectUserInfoPage();
  }
}

function login() {
  const isConfirm = conFirm("로그인에 성공했습니다.");

  if (isConfirm) {
    redirectIndexPage();
  }
}

//위와같이 두개로 나뉘어져있는 함수를
//콜백함수를 이용해서 아래와 같이 만들 수 있다.

function confirmModal(messaage, cbFunc) {
  const isConfirm = confirm(message);

  if (isConfirm && cbFunc) {
    cbFunc();
  }
}

function register() {
  confirmModal("회원가입에 성공했습니다.", redirectUserInfoPage);
}
function login() {
  confirmModal("로그인에 성공했습니다.", redirectIndexPage);
}
```

## 58. 순수 함수

- 사이드 이펙트가 없고, 외부에서 값을 조작할 수 없고 언제나 같은 값을 내뱉는 함수이다.

- 함수 내에서 사용하는 변수는 파라미터로 받거나, 함수 내에서 선언해서 사용한다.

- 객체를 다룰 때는 꼭 복사를 해서 사용하기! => 원본 객체가 손상됨.

- 아래는 순수함수가 아닌 예시

```js
/**
 * Pure Function
 */
let num1 = 10;
let num2 = 20;

function impureSum1() {
  return num1 + num2;
}

impureSum1(); // 30

num1 = 30;
impureSum1(); // 50
```

- 함수를 아래처럼 개선해 볼 수 있다

- 하지만 위 코드와 동일한 문제가 발생할 수 있다

- 즉, 예측이 불가능한 문제가 발생할 수 있다.

```js
let num1 = 10;

function impureSum2(newNum) {
  return num1 + newNum;
}

impureSum2(30); // 40

num1 = 100;
impureSum2(30); // 110
```

- 아래와 같은 순수함수로 코드를 개선한다

```js
function pureSum(num1, num2) {
  return num1 + num2;
}

pureSum(10, 20);
pureSum(10, 20);
pureSum(10, 20);
pureSum(30, 100);
pureSum(30, 100);
```

```js
/**
 * Pure Function
 */

function changeValue(num) {
  num++;

  return num;
}

changeValue(1); // 2
changeValue(2); // 3

////////////////////////////////

const obj = { one: 1 };

// 객체, 배열 => 새롭게 만들어서 반환
function changeObj(targetObj) {
  targetObj.one = 100;

  return targetObj;
}

changeObj(obj);

obj; // 원본 객체가 변경된 것을 확인할 수 있다

// 그렇기 때문에 객체의 경우에는 아래처럼 새롭게 만들어서 반환한다
function changeObj(targetObj) {
  return { ...targetObj, one: 1000 };
}
```

## 59. Closure

```js
function add(num1) {
  return function sum(num2) {
    return num1 + num2;
  };
}

const addOne = add(1)(3); // 항상 1을 더해준다
const addTwo = add(2)(4); // 항상 2를 더해준다
```

```js
// 전달된 두 인자를 기억하고 있다가 나중에 함수가 전달되면 두 인자를 계산하는 함수
function add(num1) {
  return function (num2) {
    return function (calculateFn) {
      return calculateFn(num1, num2);
    };
  };
}

function sum(num1, num2) {
  return num1 + num2;
}

function multiple(num1, num2) {
  return num1 * num2;
}

const addOne = add(1)(2);

// 함수를 전달해준다
const sumAdd = addOne(sum); // 3

const addOne = add(1)(2);
const sumMultiple = addOne(multiple); // 10
```

- 아래는 로그를 출력하는 예시

```js
/**
 * Closure
 */
function log(value) {
  return function (fn) {
    fn(value);
  };
}

const logFoo = log("foo"); // 전달된 함수의 인자로 'foo'가 전달된다

logFoo((v) => console.log(v));
logFoo((v) => console.info(v));
logFoo((v) => console.error(v));
logFoo((v) => console.warn(v));
```

```js
const arr = [1, 2, 3, "A", "B", "C"];

// before
const isNumber = (value) => typeof value === "number";
const isString = (value) => typeof value === "string";

// after - 1, type과 value를 인자돌 전달받는다
function isTypeOf(type, value) {
  return typeof value === type;
}

// 아래와 같이 사용할 수 밖에 없다
const isNumber = (value) => isTypeOf("number", value);
const isString = (value) => isTypeOf("string", value);

// after - 2, 클로저를 사용한다
function isTypeOf(type) {
  return function (value) {
    return typeof value === type;
  };
}

const isNumber = isTypeOf("number");
const isString = isTypeOf("string");

arr.filter(isNumber);
arr.filter(isString);
```

- endpoint별로 fetcher를 만들 때도 클로저를 활용할 수 있다

- 아래는 base url을 다루는 코드

```js
function fetcher(endpoint) {
  return function (url, options) {
    return fetch(endpoint + url, options)
      .then((res) => {
        if (res.ok) {
          return res.json();
        } else {
          throw new Error(res.error);
        }
      })
      .catch((err) => console.error(err));
  };
}

const naverApi = fetcher("http://naver.com");
const daumApi = fetcher("http://daum.net");

getDaumApi("/webtoon").then((res) => res);
getNaverApi("/webtoon").then((res) => res);
```

```js
/**
 * Closure
 */
someElement.addEventListener("click", debounce(handleClick, 500));

someElement.addEventListener("click", throttle(handleClick, 500));
```
