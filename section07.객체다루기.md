## 41. Shorthand Properties

- 아래는 css의 Shorthand Properties의 예시

```css
/* as-is */
.before {
  background-color: #000;
  background-image: url(images/bg.gif);
  background-repeat: no-repeat;
  background-position: left top;

  margin-top: 10px;
  margin-right: 5px;
  margin-bottom: 10px;
  margin-left: 5px;
}

/* to-be */
.after {
  background: #000 url(images/bg.gif) no-repeat left top;

  margin: 10px 5px 10px 5px;
}
```

- 자바스크립트 객체의 프로퍼티와 메소드를 축약해서 쓰자.

```js
/**
 * Shorthand Properties
 */

// before
const counterApp = combineReducers({
  counter: counter,
  extra: extra,
});

// after
const counterApp = combineReducers({
  counter,
  extra,
});
```

```js
/**
 * Shorthand Properties
 * Concise Method -> 간결한 메서드
 * ES2015+
 */

// before
const firstName = "poco";
const lastName = "jang";

const person = {
  firstName: "poco",
  lastName: "jang",
  getFullName: function () {
    return this.firstName + " " + this.lastName;
  },
};

// after
const firstName = "poco";
const lastName = "jang";

const person = {
  firstName,
  lastNam,
  getFullName() {
    // Concise Method
    return this.firstName + " " + this.lastName;
  },
};
```

## 42. Computed Property Name

- Computed Property Name은 객체의 프로퍼티 key를 문자열로 변환할 수 있는 표현식을 사용해(변수, 함수 등) 동적으로 지정하는 문법이다.

- 이를 통해 객체 리터럴 내부에서 Computed Property Name으로 프로퍼티 키를 동적으로 생성할 수 있다.

- 프로퍼티 키로 사용할 표현식을 대괄호([])로 묶어야 한다.

- 아래 코드는 id, password 입력창 컴포넌트이다.

- Computed Property Name을 통해 onChange 이벤트가 걸린 입력창의 name을 동적으로 받아 value를 수정할 수 있다.

```js
import React, { useState } from "react";

function SomeComponent() {
  const [state, setState] = useState({
    id: "",
    password: "",
  });

  const handleChange = (e) => {
    setState({
      // 아래 [e.target.name] 가 Computed Property Name
      [e.target.name]: e.target.value,
    });
  };

  return (
    <React.Fragment>
      <input value={state.id} onChange={handleChange} name="name" />
      <input value={state.password} onChange={handleChange} name="password" />
    </React.Fragment>
  );
}

export default SomeComponent;
```

- 아래는 리덕스 예시

```js
const noop = createAction("INCREMENT");

const reducer = handleActions(
  {
    [noop]: (state, action) => ({
      counter: state.counter + action.payload,
    }),
  },
  { counter: 0 }
);
```

- 아래는 Vuex의 예시

```js
import Vuex from "vuex";
import { SOME_MUTATION } from "./mutation-types";

export const SOME_MUTATION = "SOME_MUTATION";

const store = new Vuex.Store({
  state: {
    // some code...
  },
  mutations: {
    [SOME_MUTATION](state) {},
    //[함수명](매개변수){함수몸체}
  },
});
```

- 아래는 함수 예시

```js
const funcName0 = "func0";
const funcName1 = "func1";
const funcName2 = "func2";

// 아래처럼 computed property names를 이용해서 함수를 선언할 수 있따.
const obj = {
  [funcName0]() {
    return "func0";
  },
  [funcName1]() {
    return "func1";
  },
  [funcName2]() {
    return "func2";
  },
};

// 아래처럼 동적으로 사용할 수 있다.
for (let i = 0; i < 3; i++) {
  console.log(obj[`func${i}`]());
}
```

## 43. Lookup Table

- Lookup Table이란 key - value 이루어진 테이블을 말한다.

- 어떤 데이터가 인자로 들어오느냐에 따라 다른 데이터를 넣어주어야 할 때는, 객체를 Lookup Table 형태로 저장해둔 후 인자에 따라 다른 데이터를 리턴하는 식으로 한다.

- 아래 코드는 인자로 들어온 User type에 따라 다른 데이터를 리턴한다

```js
/**
 * Object Lookup Table
 */
function getUserType(type) {
  if (type === "ADMIN") {
    return "관리자";
  } else if (type === "INSTRUCTOR") {
    return "강사";
  } else if (type === "STUDENT") {
    return "수강생";
  } else {
    return "해당 없음";
  }
}
```

- 아래 코드는 Switch문을 통해 인자에 맞는 데이터를 return 해주는데, 위 방식보다 더 권하는 방식이다

```js
/**
 * Object Lookup Table
 */
function getUserType(type) {
  switch (key) {
    case "ADMIN":
      return "관리자";
    case "INSTRUCTOR":
      return "강사";
    case "STUDENT":
      return "수강생";
    default:
      return "해당 없음";
  }
}
```

- 하지만 이러한 방식들은 한 눈에 보기 어려울 뿐더러, 다른 데이터를 추가적으로 넣어주어야 할 때 복잡해진다.

- 아래 코드처럼 Object Lookup Table을 사용해서 해결할 수 있다.

- 가장 바람직한 형태라고 생각

```js
/**
 * Object Lookup Table
 */
function getUserType(type) {
  // JS에서는 Snake Case로 상수를 관리한다
  const USER_TYPE = {
    ADMIN: "관리자",
    INSTRUCTOR: "강사",
    STUDENT: "수강생",
  };

  return USER_TYPE[type] || "해당 없음";
}

getUserType("ADMIN"); // 관리자
getUserType("NO"); // 해당 없음
```

- 또는 아래처럼 Nullish Operator를 사용할 수 도 있다.

```js
/**
 * Object Lookup Table
 */
function getUserType(type) {
  // JS에서는 Snake Case로 상수를 관리한다
  const USER_TYPE = {
    ADMIN: "관리자",
    INSTRUCTOR: "강사",
    STUDENT: "수강생",
    UNDEFINED: "해당없음",
  };

  return USER_TYPE[type] ?? USER_TYPE.UNDEFINED;
}

getUserType("ADMIN"); // 관리자
getUserType("NO"); // 해당 없음
```

- 이러한 상수를 constants 폴더 파일에 넣어준 후, 해당 파일을 임포트해 return 해주면 사이드 이펙트도 없고 유지보수하기 간편하다.

```js
import USER_TYPE from "./constants/..some.js";

function getUserType(type) {
  return USER_TYPE[type] || USER_TYPE.UNDEFINED;
}
```

- 또는 내부에 지역변수를 선언할 필요 없이 바로 return 하는 팩토리 함수같은 느낌으로도 처리할 수 도 있다.

```js
/**
 * Object Lookup Table
 */
function getUserType(type) {
  return (
    {
      ADMIN: "관리자",
      INSTRUCTOR: "강사",
      STUDENT: "수강생",
    }[type] ?? "해당 없음"
  );
}

getUserType('ADMIN')
```

## 44. Object Destructuring

- 아래의 코드에서는 인자의 순서(name, age, location)가 중요하다.

- 따라서 매개변수 순서를 잘못 전달할 시 문제가 발생한다.

```js
function Person(name, age, location) {
  this.name = name;
  this.age = age;
  this.location = location;
}

const poco = new Person("poco", 30, "korea");
```

- 아래 코드처럼 인스턴스를 생성할 때 객체로 매개변수를 전달한 후, 생성자 함수에서 이를 디스트럭쳐링하게끔 하면, 인자의 순서대로 전달하지 않아도 문제가 없다.

```js
function Person({ name, age, location }) {
  this.name = name;
  this.age = age;
  this.location = location;
}

const poco = new Person({
  name: "poco",
  age: 30,
  location: "korea",
});
```

- 옵션으로 전달할 인자가 있을 시 객체 디스트럭쳐링을 이용할 수 있다.

- 아래 코드는 생성자함수 안에서 각 프로퍼티에 Null 병합연산자를 통해 디폴트값을 설정했는데 가독성면에서 좋지 않다.

```js
function Person({ name, age, location }) {
  this.name = name;
  this.age = age ?? 30;
  this.location = location ?? "korea";
}

// 인자의 순서가 중요하지 않음
const poco = new Person({
  name: "fgStudy",
});

console.log(poco); // Person { name: 'fgStudy', age: 30, location: 'korea' }
```

- name 이라는 값이 필수적이고 나머지가 옵션이라면 아래 코드처럼 따로 옵션 객체를 만든 후 값을 미리 넣어준 다음에 인자로 전달한다

```js
// 명시적
function Person(name, { age, location }) {
  this.name = name;
  this.age = age;
  this.location = location;
}

// 옵션들을 만들어서 전달한다
const fgStudyOptions = {
  age: 30,
  location: "korea",
};

const fg = new Person("fgStudy", fgStudyOptions);
console.log(fg); // Person { name: 'fgStudy', age: 30, location: 'korea' }
```

- 배열이 객체임을 이용해, 인덱스를 통해 배열의 요소를 간편하게 디스트럭쳐링해서 사용할 수 있다.

- orders의 첫 번째, 세 번째 원소를 변수로 만들기 위해서는 원하지 않는 원소는 빈칸으로 놔둘 수 있다.

- 이는 배열 요소가 많아지게 될 시 불편하다.

```js
const orders = ["First", "Second", "Third"];

// before
const st = orders[0];
const rd = orders[2];

// after
const [first, , third] = orders;
```

- 이렇게 하는 것 보다 조금 더 명시적으로 할 수 있는 방법이 있는데 아래처럼 배열이 객체임을 이용해 인덱스로 디스트럭쳐링하자.

```js
const { 0: st2, 2: rd2 } = orders;
// 첫번째 속성을 st2
// 세번째 속성을 rd2

console.log(st2, rd2);
```

- 아래는 리액트 예시

```js
// before
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

// after
function Welcome({ name }) {
  return <h1>Hello, {name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(element, document.getElementById("root"));
```

## 45. Object.freeze

- 자바스크립트에서는 객체의 불변성을 위해 freeze 메소드를 사용한다.

- 하지만 freeze는 직속 프로퍼티만 동결되므로(shllow only) 중첩 객체는 동결할 수 없다.

- 아래 코드를 보면 PENDING 프로퍼티는 동결되었으나, 중첩 객체인 OPTIONS는 동결되지 않았음을 확인할 수 있다.

```js
/**
 * Object.freeze
 *
 * 중첩된 freeze를 해결하는 방법
 * 1. 대중적인 유틸 라이브러리 (lodash)
 * 2. 직접 유틸 함수 생성 - 팀내에서 함수를 만들어준다
 * 3. stackoverflow 찾아보기
 * 4. TypeScript => readonly
 */
const STATUS = Object.freeze({
  PENDING: "PENDING",
  SUCCESS: "SUCCESS",
  FAIL: "FAIL",
  OPTIONS: {
    GREEN: "GREEN",
    RED: "RED",
  },
});

console.log(Object.isFrozen(STATUS.PENDING)); // true
console.log(Object.isFrozen(STATUS.OPTIONS)); // false
```

- 실제로 아래처럼 OPTIONS에 접근해서 값을 변경하거나 추가하면 실제로 객체가 변경된 것을 확인할 수 있다

```js
const STATUS = Object.freeze({
  PENDING: "PENDING",
  SUCCESS: "SUCCESS",
  FAIL: "FAIL",
  OPTIONS: {
    GREEN: "GREEN",
    RED: "RED",
  },
});

console.log(Object.isFrozen(STATUS.PENDING)); // true
console.log(Object.isFrozen(STATUS.OPTIONS)); // false

STATUS.OPTIONS.GREEN = "G";
STATUS.OPTIONS.YELLOW = "Y";

console.log(STATUS.OPTIONS); // { GREEN: 'G', RED: 'RED', YELLOW: 'Y' }
```

- 따라서 OPTIONS에 프로퍼티를 추가하거나 수정, 삭제하는 등의 변경이 가능하다.

- 해결방법은 모든 프로퍼티에 대해 재귀적으로 freeze 메소드 호출해준다

```js
const STATUS = Object.freeze({
  PENDING: "PENDING",
  SUCCESS: "SUCCESS",
  FAIL: "FAIL",
  OPTIONS: {
    GREEN: "GREEN",
    RED: "RED",
  },
});

function deepFreeze(target) {
  // 1. 객체를 순회
  // 2. 값이 객체인지 확인
  // 3. 객체이면 재귀
  // 4. 그렇지 않으면 Object.freeze

  Object.keys(target).forEach((key) => {
    if (typeof target[key] === "object" && !Object.isFrozen(target[key]))
      deepFreeze(target[key]);
  });

  return Object.freeze(target);
}

deepFreeze(STATUS);

console.log(Object.isFrozen(STATUS)); // true
console.log(Object.isFrozen(STATUS.OPTIONS)); // true
```

## 46. Prototype 조작 지양하기

- 지양해야 하는 이유는

- 첫번째, JS는 이미 많이 발전했다.

  - Class를 사용할 수 있기 때문에 굳이 Prototype을 사용할 필요가 없다

- 두번째, JS 빌트인 객체를 건들지말자

  - JS는 이미 많이 발전했기 때문에 굳이 Prototype에 함수를 정의해서 사용할 필요가 없다

```js
/**
 * Prototype 조작 지양하기
 *
 * 1. 이미 JS는 많이 발전했다.
 *  프로토타입을 건들지 않기 위한 방법
 *   1-1. 직접 만들어서 모듈화 하기
 *   1-2. 직접 만들어서 모듈화한 것을 => 배포(NPM에 배포한다)
 * 2. JS 빌트인 객체를 건들지말자
 */

class Car {
  constructor(name, brand) {
    this.name = name;
    this.brand = brand;
  }

  sayName() {
    return this.brand + "-" + this.name;
  }
}

const casper = new Car("캐스퍼", "현대");
```

## 47. hasOwnProperty

- property를 가졌는지 확인

```js
/**
 * hasOwnProperty
 */

const person = {
  name: "hyeonseok",
};
person.hasOwnProperty("name"); // true

const foo = {
  hasOwnProperty: function () {
    return "hasOwnProperty";
  },
  bar: "string",
};

foo.hasOwnProperty("bar"); // hasOwnProperty
Object.prototype.hasOwnProperty.call(foo, bar); // true, 이렇게 Object의 hasOwnProperty와 call을 사용해야 한다

/**
 * call을 사용해야 하는 이유
 * https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty
 *
 * 자바스크립트는 프로퍼티 명칭으로서 hasOwnProperty를 보호하지 않습니다.
 * 그러므로, 이 명칭을 사용하는 프로퍼티를 가지는 객체가 존재하려면,
 * 올바른 결과들을 얻기 위해서는 외부 hasOwnProperty 를 사용해야합니다.
 * 즉, 다른 객체의 hasOwnProperty와 겹칠 수 있기 때문이다
 *
 * var foo = {
 *  hasOwnProperty: function() {
 *    return false;
 *  }
 *
 */

// 아래처럼 함수를 정의해서 사용할 수 있다
function hasOwnProp(targetObj, targetProp) {
  return Object.prototype.hasOwnProperty.call(targetObj, targetProp);
}

hasOwnProp(person, "name"); // true

hasOwnProp(foo, "hasOwnProperty"); // true
```

## 48. 직접 접근 지양하기

- model에 직접 접근하는 것은 좋지 않다

- 아래 코드는 model 프로퍼티에 너무 쉽게 접근할 수 있다는 문제가 있다.

- 이는 프로퍼티가 누구나 변경할 수 있다는 문제가 있다.

```js
/*
 * 직접 접근 지양하기
 */
const model = {
  isLogin: false,
  isValidToken: false,
};

// login, logout이 model에 직접 접근할 수 있음
function login() {
  model.isLogin = true;
  model.isValidToken = true;
}

function logout() {
  model.isLogin = false;
  model.isValidToken = false;
}

someElement.addEventListener("click", login);
```

- 따라서 model의 프로퍼티를 직접 접근하지 못하게끔 막아야 한다! → 캡슐화

- 캡슐화

  - 특정 프로퍼티에 대한 접근을 미리 정해진 메소드들을 통해서만 가능하게 만든다.

  - 객체 외부에서 함부로 접근하면 안되는 프로퍼티나 메소드에 직접 접근할 수 없도록 한다.

- model 프로퍼티에 접근할 수 있는 메소드를 만들고, 메소드로만 해당 프로퍼티를 접근하도록 해 안전성을 높이자.

- 캡슐화를 통한 간접 접근하도록 한다

- 아래처럼 객체에 접근하는 별도의 함수를 빼준다

```js
/**
 * 직접 접근 지양하기
 * flux 아키텍처
 * action -> reducer -> state 변화
 * 이렇게 상태 변화를 힘들게 하는 이유는 예측 가능하도록 하기 위해
 *
 * 예측 가능한 코드를 작성해서 동작이 예측 가능한 앱을 만들도록 한다
 * JS의 get, set을 사용하는 것도 하나의 예시
 */

// 직접 접근 지양
const model = {
  isLogin: false,
  isValidToken: false,
};

// model에 대신 접근
function setLogin(bool) {
  model.isLogin = bool;
  serverAPI.log(model.isLogin); // model이 변할 때 마다 로그를 보낸다
}

// model에 대신 접근
function setValidToken(bool) {
  model.isValidToken = bool;
  serverAPI.log(model.isValidToken); // model이 변할 때 마다 로그를 보낸다
}

// model에 직접 접근 X
function login() {
  setLogin(true);
  setValidToken(true);
}

// model에 직접 접근 X
function logout() {
  setLogin(false);
  setValidToken(false);
}

someElement.addEventListener("click", login);
```

## 49. Optional Chaining

```js
/**
 * Optional Chaining (?.)
 */

const js = {
  name: {
    pasts: ["Mocha", "LiveScript"],
    current: "JavaScript",
  },
  author: "Brendan Eich",
  birth: "1995-12-4",
  extension: ".js",
  paradigm: ["script", "object", "functional"],
};

// before
if (js) {
  if (js.name) {
    if (js.name.current) {
      return js.name.current;
    }
  }
}

// after - 1
if (js && js.name && js.name.current) {
  return js.name.current;
}

// after - 2
if (js?.name?.current) {
  return js.name.current;
}
```

```js
/**
 * Optional Chaining
 */

const js = {
  version: [
    {
      name: "1st & 2nd",
      birth: "1998-10",
    },
    {
      name: "3rd",
      birth: "2000-11",
    },
    {
      name: "5th",
      birth: "2010-07",
    },
  ],
  name: {
    pasts: ["Mocha", "LiveScript"],
    current: "JavaScript",
  },
  author: "Brendan Eich",
  birth: "1995-12-4",
  extension: ".js",
  paradigm: ["script", "object", "functional"],
};

// before
if (js.version && js.version.length > 0) {
  return js.version[0].name;
}

// after
if (js?.version?.length > 0) {
  return js.version[0].name;
}
```
