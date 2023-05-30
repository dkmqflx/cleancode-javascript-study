## 60. Magig Number

- 마법같은 숫자

```js
/**
 * Magic Number
 *
 */

setTimeout(() => {
  scrollToTop(); // 어느 정도 지연된 후 호출된다
}, 3 * 60 * 1000);
// 3, 60, 1000을 읽기 위한 3번의 의식적인 흐름이 필요하다

// 아래처럼 상수로 Snake Case를 사용해서 작성할 수 있다
// 더 직관적으로 알 수 있다.
const COMMON_DELAY_MS = 3 * 60 * 1000;

setTimeout(() => {
  scrollToTop(); // 어느 정도 지연된 후 호출된다
}, DELAY_MS);
```

```js
// Numeric Operator

// 숫자 단위를 읽기 불편하다
const PRICE = {
  MIN: 1000000, // 1백만원
  MAX: 100000000, // 1억
};

// 이렇게 개선할 수 있다
const PRICE = {
  MIN: 1_000_000, // 1백만원
  MAX: 100_000_000, // 1억
};
console.log(PRICE); // 동일하게 동작한다
```

- 아래도 Magic Number를 사용해서 명시적으로 해주는 것이 좋다

```js
// before
getRandomPrice(0, 10);

// after
getRandomPrice(PRICE.MIN, PRICE.MAX);
```

- 아래는 하드코딩이 엄청나게 되어 있다

- 1과 5에 대한 정책이 바뀐다면 모두 코드를 수정해 주어야 한다.

- 사실 이런 값은 운영이나 백오피스를 통해서 서버에서 내려오는 값이어야 하지만

- 이런 값들이 하드코딩 되어 있을 수 있다

```js
// before
function isValidName(name) {
  return carName.length >= 1 && carName.length <= 5;
}

// after
// 일반적으로 Snake Case라면 개발자들이 건들지 않겠지만
// 혹시모를 상황 대비해서 아래처럼 freeze 해줄 수 도 있다.
// 이렇게 해주면, 위처럼 하드코딩 했을 때와는 달리 해당 상수만 변경해주면 된다
const CAR_NAME_LEN = Object.freeze({
  MIN: 1,
  MAX: 5,
});

function isValidName(name) {
  return (
    carName.length >= CAR_NAME_LEN.MIN && carName.length <= CAR_NAME_LEN.MAX
  );
}

function notivalidName = (value) => {
  if(!isArrayItemLengthRange(names, CAR_NAME_LEN.MIN, CAR_NAME_LEN.MAX)){
    alert(`자동차 이름은 ${CAR_NAME_LEN.MIN}자에서 ${CAR_NAME_LEN.MAX}자까지 입력할 수 있습니다.`)

  }
}
```

## 61. 네이밍 컨벤션

- 저장소, 폴더, 파일, 함수, 변수, 깃, 브랜치, 커밋 등 프로그래밍 전반적으로 이름을 네이밍을 위한 규칙이나 관습을 만드는 것

- 팀이나 개인의 차원에 따라 다를 수 있으며 특히 개인적인 견해와 해석에 따라 다를 수 있다.

- 하지만 기준을 설정할 때 기본적인 논리와 이유가 있어야 한다

- 가장 중요한 것은 네이밍을 정할 때 자바스크립트 예약어(키워드)와 겹치지 않도록 하는 것

- 대표적인 케이스

  - camelCase

  - PascalCase

  - kebab-case

  - SNAKE_CASE

- 접두사, 접미사

  - `prefix-*`, `*-suffix`

  - data-id
  - data-name
  - data-value

  - AppContainer
  - BoxContainer

  - ListComponent
  - ItemComponent

  - ICar
  - TCar

  - AType
  - BType

  - `동사-*` // 함수는 동사로 시작한다

  - `_`, `#` : private

- 연속적인 규칙

  ```js
  for (let i = 0; i < 10; i++) {
    for (let j = 0; j < 10; j++) {
      for (let h = 0; h < 10; h++) {
        // i, j 다음 h가 올 지 k가 올지 등
      }
    }
  }

  // 타입스크립트 제네릭
  function func(T, U)(name:T, value:U)
  ```

- 자료형 표현

  - 자료에 타입을 유추할 수 있도록 네이밍을 정해준다

  ```js
  const inputNumber = 10;
  const someArr = [];
  const strToNum = "some code";
  ```

- 이벤트 표현

  ```js
  function on-*
  function handle-*
  function *-Action
  function *-Event
  function take-*
  function *-Query
  function *-All

  ```

- CRUD

  ```js

  function generator-*
  function gen-*
  function make-*
  function get
  function set
  function remove
  function create
  function delete

  ```

- Flag

  ```js

  const isSubmit
  const isDisabled
  const isString
  const isNumber

  ```

- ETC

  ```js

  function selectBy-*()
  // id가 와야 한다면 function selectById(id) 이렇게 사용한다

  function selectAll

  ```

## 62. DOM API 접근 추상화

- 기존 코드는 다음과 같다

```js
/**
 *
 * HTML에 접근하는 JavaScript 코드 추상화
 */

export const loader = () => {
  const el = document.createElement("div");
  el.setAttribute("class", "loading d-flex justify-center mt-3");

  const el2 = document.createElement("div");
  el2.setAttribute("class", "relative d-flex justify-center mt-3");

  const el3 = document.createElement("div");
  el3.setAttribute("class", "loading d-flex justify-center mt-3");

  el.append(el2);
  el2.append(el3);
};
```

- 아래와 같이 위 코드를 추상화 할 수 있다

- JS 코드만 보고 CSS 코드를 안보이게 추상화 했다.

```js
/**
 *
 * HTML에 접근하는 JavaScript 코드 추상화
 */

const createLoader = () => {
  const el = document.createElement("div");
  const el2 = document.createElement("div");
  const el3 = document.createElement("div");

  return {
    el,
    el2,
    el3,
  };
};

const createLoaderStyle = ({ el, el2, el3 }) => {
  el.setAttribute("class", "loading d-flex justify-center mt-3");

  el2.setAttribute("class", "relative spinner-container");

  el3.setAttribute("class", "material spinner");

  return {
    newEl: el,
    newEl2: el2,
    newEl3: el3,
  };
};
const loader = () => {
  const { el, el2, el3 } = createLoader();
  const { newEl, newEl2, newEl3 } = createLoaderStyle({ el, el2, el3 });

  newEl.append(newEl2);
  newEl2.append(newEl3);
};
```

- 아래는 또 다른 예시인 모달 코드 관련 코드이다

```js
const element = document.querySelector("#modal");

element.style.backgroundColor = "black";

element.classList.add("--open");

element.classList.remove("--open");
```

- 위 코드도 아래처럼 추상화 해줄 수 있다

```js
// 모달을 만들어주는 함수 정의
const modal = () => {
  /**
   * 모달 생성 코드
   *
   */

  return document.querySelector("#modal");
};

const changeColor = (element) => {
  element.style.backgroundColor = "black";
};

const openModal = (element) => {
  element.classList.add("--open");
};

const closeModal = (element) => element.classList.remove("--open");
{
}

openModal(modal);
changeColor(modal);
closeModal(modal);
```
