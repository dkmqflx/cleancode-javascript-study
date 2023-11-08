## 15. min - max

1.  최소값와 최대값을 다룬다

2.  최소값와 최대값 포함 여부를 결정해야한다 (이상-초과 / 이하-미만)

3.  혹은 네이밍에 최소값과 최대값 포함 여부를 표현한다.

```js
// 난수를 생성하는 함수
function genRandomNumber(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}

// 최소 값과 최대 값에 대한 상수를 미리 만들어주는 것이 좋다
const MIN_NUMBER = 1;
const MAX_NUMBER = 45;

// 코드만 보고도 랜덤 값의 최소, 최대 범위를 알 수 있다.
genRandomNumber(MIN_NUMBER, MAX_NUMBER);
```

- 아래처럼 최소값과 최대값의 포함 여부를 정하는 것이 중요하다

- 모호하지 않도록 컨벤션을 정해주는 것이 좋다.

- 즉, MIN, MAX 다룰 때 항상 이상, 이하 인지 또는 초과, 미만 인지에 대한 컨벤션을 정해준다.

```js
const MAX_AGE = 20;

function isAdult(age) {
  // 최소값, 최대값 (포함되는지 vs 안되는지)
  // 이상, 초과 vs 이하, 미만
  if (age > MAX_AGE) {
  }
}

isAdult(30);
```

- 아래처럼 변수명을 정해주면 미만, 초과를 변수명만 보고도 알 수 있다.

```js
const MIN_IN_NUMBER = 1; // 1이 포함이 된다
const MAX_IN_NUMBER = 45; // 45가 포함이 된다

const MAX_LIMIT_NUMBER = 45; // 45가 포함이 되지 않는다
```

## 16. begin - end

```js
/**
 * begin - end
 *
 * 경계를 포함하지만 제외하는 경우
 * begin : 동일한 값, 고정됨
 * end: : 다른 값, 포함되거나 포함되지 않음
 *
 * 즉, 시작은 포함되지만 끝은 포함되지 않을 수도 있는 경우 begin, end로 변수명을 짓는다
 * 많은 date picker 라이브러리들도 이런식으로 이름을 짓는다
 * 아래 함수를 사용해서 달력에서 숙박 예약을 하는 경우, 시작 날짜는 고정되지만 끝나는 날짜는 고정되지 않는다.
 */

function reservationDate(beginDate, endDate) {
  // ...some code
}

reservationDate("YYYY-MM-DD", "YYYY-MM-DD");
```

## 17. first - last

```js
/**
 * first - last
 *
 * 규칙성이 없는 경우 사용할 수 있다. (ex. 1, 2, 3, 4, 5 - 이런 데이터는 규칙성이 있는 데이터)
 * 포함된 양 끝을 의미한다.
 * 부터 ~~~ 까지
 * DOM에서도 firstChild, lastChild 이런식으로 element의 자식 요소에 접근할 수 있다.
 */

const students = ["포코", "존", "현석"];

function getStudents(first, last) {
  // ...some code
}

getStudents("포코", "현석"); // 포코와 현석은 양 끝으로 포함된다.
```

## 18. prefix - suffix

- js의 `get`, `set`

- 리액트 hook은 `use`로 시작하는 것도 prefix

- 단일 파일만 있는 경우, `폴더이름`, 복수 파일이 있는 경우 `폴더이름s` 도 suffix의 예시

## 19. 매개변수의 순서가 경계다

- 매개변수의 순서만 잘 지켜도 경계가 정해진다

```js
/**
 * 매개변수의 순서가 경계다
 *
 * 호출하는 함수의 네이밍과 인자의 순서의 연관성을 고려한다.
 *
 * 1. 매개변수를 2개가 넘지 않도록 만든다. -> (1, 50), ('2020-10-01', '2020-10-02') 등 매개변수의 순서를 통해서 경계를 알 수 있다
 *
 * 2개 이상의 인자를 갖는 경우 아래와 같은 방법으로 처리할 수 있다
 *
 * 2. arguments, rest parameter
 * 3. 매개변수를 객체에 담아서 넘긴다.
 * 4. 랩핑하는 함수 <- 이미 만든 함수가 있는 경우, 아래 코드가 예시
 */

// ES2015+
function someFunc(someArg1, someArg2, someArg3, someArg4) {}

function getFunc(someArg1, someArg3) {
  someFunc(someArg1, undefined, someArg3);
}
```
