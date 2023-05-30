## 71. 공백

- 공백도 코드 작성의 일부다

- 숏코딩이라고 해서 아래처럼 한줄한줄 공백을 최대한 줄이려고 하는 경우 있다

```js
export const loadingElements = () => {
  const el = document.createElement('div');
  el.setAttribute('class', 'loading d-flex justify-center mt-3');
  const el2 = document.createElement('div');
  el2.setAttribute('class', 'relative d-flex justify-center mt-3');
  const el3 = document.createElement('div');
  el3.setAttribute('class', 'loading d-flex justify-center mt-3');
  el.append(el2);
  el2.append(el3);
  return el;
};
```

- 하지만 이렇게 할 필요가 없는데, 빌드될 때 더 줄이고 못생기게 만들 수 있기 때문이다

- 위 코드를 아래처럼 단위를 나눠서 수정할 수 있다

```js
export const loadingElements = () => {
  // 1. 선언
  // 2. 로직(문)
  // 3. 반환

  // 1. 선언만 모아 노는다
  const el = document.createElement('div');
  const el2 = document.createElement('div');
  const el3 = document.createElement('div');

  // 2. 로직, 문
  el.setAttribute('class', 'loading d-flex justify-center mt-3');
  el2.setAttribute('class', 'relative d-flex justify-center mt-3');
  el3.setAttribute('class', 'loading d-flex justify-center mt-3');

  // 2. 로직, 문
  el.append(el2);
  el2.append(el3);

  // 3. 반환, 한칸만 띄워도 명시적으로 나타낼 수 있다
  return el;
};
```

- es lint의 padding-line-between statments 룰을 사용하는 것을 권장한다

## 72. indent depth

- 복잡해질 수록 깊어진다

  - 복잡해지지 않도록 ㄴ력한ㄷ ㅏ

- 들여쓰기를 어느 레벨로 가져가느냐

  - HTML 보통 2 depth

  - JS 보통 2 depth

- 팀마다 개인마다 다르다

- depth가 중요한 것은, 중첩 될 때 마다 가독성이 떨어진다

- 이런 문제를 해결하기 위한 방법들을 앞서 언급했다

- 아래는 의식적으로 코드를 작성하는 방법

  - 조기반환

  - callback => Promise => Async & Await

  - 고차 함수 (map, reduce, filter)

  - 함수를 나누고 추상화하기

  - 메서드 체이닝 (`.then().then.then()`)

    ```js
    function test() {
      somePromise() // depth가 더 깊어지지 않는다
        .then() //
        .then() //
        .then();
    }
    ```

- 의식적으로 depth를 줄이는 것이 가장 중요하다

  - 리액트의 경우 Context hell

- 다음은 도구를 사용해서 통일 하는 방법

  - 첫번째, 에디터의 indet size를 통일한다

  - 프리티어의 tab width를 조절한다

  - eslint를 사용해서 indent rule를 적용한다

## 73. 스타일 가이드

- 네이밍 컨벤션을 포함하는 규칙을 위한 가이드라인으로 팀 혹은 집단을 위해 존재

- 즉, 협업에 큰 도움을 주기 위함

- 서로르 이해하기 위한 시간 절약

- 코드 품질을 올릴 수 있다

- 일관성

- 가독성 향상

- 유지보수 용이성

- 대표적인 스타일 가이드

  - vue style guide

  - rddux style guide

  - google style guide - js

  - javascript standard style

  - airbnb javascript style guide

    - 오래 되었다

  - rush stack

    - ms에서 모노레포를 위해 만든 스타일 가이드

  - mdn style guide

  - 수 많은 스타일 가이드를 보고 필요한 부분을 선택한다
