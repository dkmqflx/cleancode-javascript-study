## 20. 값식문

- 개발자에게 문법이 중요한 이유는, 개발자는 프로그래밍 언어로 개발을 하기 때문이다

- 리액트의 JSX는 바벨을 만나면 두번째 코드처럼 트랜스파일링 된다.

- 아래 코드를 보면 id="msg" 부분이 {id: "msg"}라는 객체로 바뀐 것을 확인할 수 있다.

- 이렇게 객체로 바뀌기 때문에 객체의 값으로 식, 그리고 값을 넣을 수는 있지만 문은 넣을 수 없다.

```js
// This JSX:
ReactDOM.render(<div id="msg">Hello World!</div>, mountNode);

// Is transformed to this JS:
ReactDOM.render(
  React.createElement("div", { id: "msg" }, "Hello World!"),
  mountNode
);
```

- 코드를 아래처럼 수정해서 보면 이를 확인할 수 있다.
  
```js

// This JSX:
<div id={if (condition) { 'msg' }}>Hello World!</div>

// Is transformed to this JS:
// 아래 코드는 에러가 발생하는데 그 이유는 if 문이기 때문
// if 문을 객체의 값으로 넣으면 에러가 발생한다
React.createElement("div", {id: if (condition) { 'msg' }}, "Hello World!");

// 아래 코드처럼 객체에 if 문을 넣으면 에러가 발생하는 것과 같다
// 즉, : 오른쪽에 값이 들어가야 하는데 문이 들어가니까 에러가 발생하는 것
const object = {id: if (condition) { 'msg' }}

// 하지만 아래 코드는 동작하는데 그 이유는 삼항연사자 이기 때문
// 삼항연산으로 연산이 된 다음 값으로 귀결되는데, 표현식은 값으로 귀결되는 "식"이기 때문
ReactDOM.render(<div id={condition ? 'msg' : null}>Hello World!</div>, mountNode);

```

- 이렇게 문과 식을 잘 구별하고, 어떤 것이 값이 될 수 있는 지를 아는 것은 중요하다.

- 아래처럼은 switch '문'을 사용했음에도 가능한데 즉시실행함수로 바로 값을 리턴하기 때문에 문을 내부에서 사용할 수 있다.

```js
function ReactComponent() {
  return (
    <section>
      <h1>Color</h1>
      <h3>Name</h3>
      {/* 아래는 논리 연산자를 사용한 예시로 논리연산자를 사용한 것이어서 문이 아니다. */}
      <p>{this.state.color || "white"}</p>
      <h3>Hex</h3>
      <p>
        {(() => {
          switch (this.state.color) {
            case "red":
              return "#FF0000";
            case "green":
              return "#00FF00";
            case "blue":
              return "#0000FF";
            default:
              return "#FFFFFF";
          }
        })()}
      </p>
    </section>
  );
}
```

- 아래 로직은 좋지 않은데 임시변수를 사용하고 있기 때문이다.

- 즉시 실행함수로 감싸고 있고 for문을 사용해서 임시 변수 안에 값을 누적하고 있다.

- 그리고 해당 배열을 반환하는 방식

```js
function ReactComponent() {
  return (
    <tbody>
      {(() => {
        const rows = [];

        for (let i = 0; i < objectRows.length; i++) {
          rows.push(<ObjectRow key={i} data={objectRows[i]} />);
        }
        return rows;
      })()}
    </tbody>
  );
}
```

- 위 코드를 값과 식만 들어가도록 아래처럼 고차함수 map함수를 사용해서 수정할 수 있다.

```js
{
  objectRows.map((obj,i) => (
    <ObjectRow key={i} data={obj}>
  ))
}

```

- 아래 예시도 즉시실행함수를 사용하고 있는 예시

```js
function ReactComponent() {
  return (
    <div>
      {(() => {
        if (conditionOne) return <span>One</span>;
        if (conditionTwo) return <span>Two</span>;
        else conditionOne;
        return <span>Three</span>;
      })()}
    </div>
  );
}
```

- 위처럼 정신없는 if문도 논리연산자와 삼항연산자를 사용하면 개선시킬 수 있다.

```js
{
  conditionOne && <span>One</span>;
}
{
  conditionTwo && <span>Two</span>;
}
{
  !conditionTwo && <span>Three</span>;
}
```

- 이 개선이 우리의 목적은 아니다.

- 값을 넣어야 할 자리에 문을 넣고 있는지 구분을 잘하며 개발하자는 것이다.

## 21. 삼항 연산자 다루기

- 삼항 연산자도 일관성 있게 잘 세워야 한다.

- 조건 ? 참 : 거짓

  - 참, 거짓에서 식이 들어가면 안된다

- 편하다고, 혹은 숏코딩을 위해 삼항 연산자를 일관성 없게 나열하면 오히려 가독성을 해친다.

- 아래 코드는 첫번째 코드는 굉장히 가독성이 떨어지는 예시이다.

- 두번째 if 조건문을 사용한 example 함수와 똑같이 동작한다.

```js
function example() {
  return condition1
    ? value1
    : condition2
    ? value2
    : condition3
    ? value3
    : value4;
}

function example() {
  if (condition1) {
    return value1;
  } else if (condition2) {
    return value2;
  } else if (condition3) {
    return value3;
  } else {
    return value4;
  }
}
```

- 이렇게 하기 보다는 차라리 switch를 고려하는 것이 더 나은 방식이다.

```js

function example() {

  const temp = condition; // 여기 임시변수에 값을 담고 아래 switch를 통해서 처리한다

  switch(key){
    case value:
      break;
    default:
      break;
  }

}

```

- 아래처럼 사람을 위해서 `()`를 묶어서 더 알아보기 쉽게 코드를 작성할 수 도 있다.

```js

// as-is
const example = condition1 ? a === 0 ? "zero" : "positive" : "negative";

// to-be
const example = condition1 ? (a === 0 ? "zero" : "positive") : "negative";
```

- 아래는 삼항연산자를 유용하게 사용하는 예시

```js
const welcomeMessage = (isLogin) => {
  const name = isLogin ? getName() : "이름없음";

  return `안녕하세요 ${name}`;
};
```

```js
const isAdult = age > 19 ? "yes" : "no";
```

- 아래는 bad case

- 삼항연산자를 사용해서 값을 반환하지 않는 `() => void`인 함수를 실행하고 있다.

```js
function alertMessage(isAdult) {
  isAdult ? alert("입장이 가능합니다.") : alert("입장이 불가능합니다.");
  // alert 실행하는 두 함수 모두 undefined가 반환된다
}
```

- 따라서 이런 경우에는 `if else`를 사용하는 것이 더 적절하다고 생각

```js
function alertMessage(isAdult) {
  if(isAdult){
   alert("입장이 가능합니다.")
  }else{
   alert("입장이 불가능합니다.");
  } 
}
```

- 아니면 함수내에서 return으로 문자열을 반환하도록 코드를 수정하는 것도 하나의 방법

 ```js
function alertMessage(isAdult) {
  return isAdult ? "입장이 가능합니다." : "입장이 불가능합니다."
}
```

## 22. Truth & Falsy

```js
if ("string".length > 0) {
}

// 10이라는 숫자가 NaN이 아닐 때
// 그냥 if(10)으로 수정해도 된다
if (!isNaN(10)) {
}

if (boolean === true) {
}
```

- 위 코드들은 아래와 동일한데 그 이유는 조건 문 안의 값들이 truthy 이기 때문

- ```js
if ("string".length ) {
}

if (10) {
}

if (boolean) {
}
```

- 공식문서에서 [truthy](https://developer.mozilla.org/ko/docs/Glossary/Truthy), [falsy](https://developer.mozilla.org/ko/docs/Glossary/Falsy)에 대해서 자세히 볼 수 있다.

- null과 undefined로 조건을 확인하는 코드가 있을 때 다음과 같이 falsy를 사용해서 개선할 수 있다.

```js
function printName(name) {
  // if (name === undefined || name === null) {
  //   return "사람이 없네요";
  // }
  // 위처럼 처리하는 대신 아래처럼 코드를 수정할 수 있다
  if (!name) {
    return "사람이 없네요";
  }

  return "안녕하세요 " + name + "님";
}
```


## 23. 단축 평가 (short-circut evaluation)

- 아래 식의 AND와 OR 연산자 예시를 통해 어떻게 오른쪽 값으로 도달하는지 알 수 있다.

```js
// AND
true && true && "도달 O";

true && false && "도달 X";

// OR
false || false || "도달 O";

true || true || "도달 X";
```

- 아래와 같은 코드가 있을 때 || 연산자를 사용해서 코드를 더 깔끔하게 작성할 수 있다.

```js
function fetchData() {
  // if (state.data) {
  //   return state.data;
  // } else {
  //   return "Fetching...";
  // }

  // default value가 있는 경우 위 코드를 아래처럼 OR 연산자를 사용해서 개선할 수 있다
  return state.data || "Fetdching...";

  // 아래처럼 삼항 연산자를 사용할 필요가 없다
  return state.data ? state.data : "Fetdching...";

}
```

- 아래 코드도 || 연산자를 사용해서 개선할 수 있다.

```js
function favoriteDog(someDog) {
  // let favoriteDog;

  // if (someDog) {
  //   favoriteDog = somDog;
  // } else {
  //   favoriteDog = "냐옹";
  // }

  // return favoriteDog + "입니다";

  // 위 코드 또한 아래처럼 수정 가능하다
  return (someDog || "냐옹") + "입니다";
}
```

- 중첩되어 있는 경우에도 && 와 같은 연산자를 사용해서 개선할 수 있다.
  
```js
function getActiveUserName(user, isLogin) {
  // if (isLogin) {
  //   if (user) {
  //     if (user.name) {
  //       return user.name;
  //     } else {
  //       return "이름없음";
  //     }
  //   }
  // }
  // 아래처럼 && 연산자를 통해서 아래처럼 depth를 줄일 수 있다.

  // if (isLogin && user) {
  //   if (user.name) {
  //     return user.name;
  //   } else {
  //     return "이름없음";
  //   }
  // }
  // 여기서 아래처럼 한번 더 줄일 수 있다

  if (isLogin && user) {
    return user.name || "이름없음";
  }
}
```

## 24. else if 피하기

- else if 를 Promsie 의 then 처럼 생각하는 사람이 있다. `promise().then().then().then()...`

- 하지만 아래 코드처럼 첫번째 if 블록에 걸리기 때문에 `"x는 0과 같거나 크다"`가 출력된다

```js
const x = 1;

if (x >= 0) {
  console.log("x는 0과 같거나 크다");
} else if (x > 0) {
  console.log("x는 0보다 크다 ");
} else {
  console.log("Else");
}
```

- 위 코드는 아래 코드와 동일한다

- 아래 코드를 보면 else 안에서 if가 실행되는 것을 알 수 있다.

- 즉, 제일 처음 if를 실행하고 그 다음번의 if를 실행하고 그 다음 else를 실행하게 된다

- 그렇기 때문에 의식적으로 else if를 사용하지 않거나 switch로 대신하는 것이 좋다

```js
if (x >= 0) {
  console.log("x는 0과 같거나 크다");
} else {
  if (x > 0) {
    console.log("x는 0보다 크다 ");
  }
}
```

- 또는 아래처럼 더 명확하게 코드를 작성해준다

```js
if (x >= 0) {
  console.log("x는 0과 같거나 크다");
}

if (x > 0) {
  console.log("x는 0과 같거나 크다");
}
```

## 25. else 피하기

- 항상 else 문을 쓸 필요는 없다.

- 습관이 되면 안된다

```js
function getUserName(user) {
  if (user.name) {
    return user.name;
  } else {
    return "이름없음";
  }
}
```

- 위 코드는 다음과 같이 간단하게 처리할 수 있다 `return user.name || '이름없음'`

- 아래 코드처럼 else를 사용하지 않고 수정할 수 도 있다.

```js
function getActiveUserName(user) {
  if (user.name) {
    return user.name;
  }
  return "이름없음";
}
```

- else를 쓰지 않아야 하는 이유는 스타일상의 문제뿐만 아니라, 반전된 로직을 작성하게 되는 논리적 위험이 있기 때문이다.

- 하나의 함수가 두 가지 이상의 기능을 할 때 else를 무분별하게 사용하면, 다음과 같은 문제가 생길 수 있다.

```js
// age가 20 미만시 report라는 특수 함수를 실행하고, 손님에게 인사를 하는 로직을 작성하려고 한다.

function getHelloCustomer() {
  if (user.age < 20) {
    report(user);
  } else {
    return "안녕하세요";
  }
}
```

- 이 코드에서는 else 때문에, 20세 미만에게만 인사를 하지 않는 의도하지 않은 결과가 발생한다.

- 아래 코드처럼 else문을 없애면, 두 기능(미성년자 확인하여 신고, 손님에게 인사)이 분리되어 손님의 나이에 관계없이 인사하는 기능이 실행된다.

- 따라서 아래 코드처럼 리팩토링 하는 것이 적절하다.

```js
function getHelloCustomer() {
  if (user.age < 20) {
    report(user);
  }
  return "안녕하세요";
}
```

## 26. Early Return

- Early Return이란 조건문에서 먼저 Return할 수 있는 부분은 분리하여 우선 if문 내에서 Return하여 함수를 미리 종료하는 것이다.

- 이렇게 하면 뒤의 코드로 진입하지 않아 else문을 사용하지 않아도 된다.

- if-else문이 너무 많으면 읽기가 어렵고 조건에 대해 명시적이지 않을 수 있는데, 이럴 때 Early Return을 활용하여 리팩토링할 수 있다.

- Early Return으로 코드를 분리하면 로직은 변함이 없으면서 가독성이 좋고 더 명시적인 코드로 리팩토링 할 수 있다.

- 최상위에 Early Return을 통해 거르는 로직을 넣으면 조건문 확인 시 덜 헷갈린다.

```js
function loginService(isLogin, user) {
  // 1. 로그인 여부 확인
  if (!isLogin) {
    // 2. 토큰 존재 확인
    if (checkToken()) {
      // 3. 가입 여부 재확인
      if (!user.nickName) {
        return registerUser(user);
      } else {
        // 유저가 있는 경우 refresh token 보내고 로그인 성공 처리
        refreshToken();

        return "로그인 성공";
      }
    } else {
      throw new Error("No Token");
    }
  }
}
```

- 위와 같은 코드를 아래처럼 Early Return으로 리팩토링할 수 있다.

- 아래처럼 코드를 수정하면 사람이 읽기 편한 코드로 변환된 것을 확인할 수 있다.

```js
/*
 * Early Return
 *
 * 함수를 미리 종료
 * 사고하기 편하다.
 */

function loginService(isLogin, user) {
  // 1. 로그인 여부 확인
  if (isLogin) {
    return;
  }
  // 2. 토큰 존재 확인
  if (!checkToken()) {
    throw new Error("No Token");
  }
  // 3. 가입 여부 재확인
  if (!user.nickName) {
    return registerUser(user);
  }

  // 정상적인 로그인 로직
  refreshToken();

  return "로그인 성공";
}
```

- 다음은 두번째 예제 코드이다.

```js
function 오늘하루(condition, weather, isJob) {
  if (condition === "GOOD") {
    공부();
    게임();
    유튜브보기();

    if (weather === "GOOD") {
      운동();
      빨래();
    }

    if (isJob === "GOOD") {
      야간업무();
      조기취짐();
    }
  }
}
```

- 위와 같은 코드를 아래처럼 Early Return으로 리팩토링할 수 있다.

- 로직이 하나의 조건문에만 의존적이도록 만들어준다

```js
function 오늘하루(condition, weather, isJob) {
  // 컨디션이 안좋으면 바로 return 하도록
  if (condition !== "GOOD") {
    return;
  }

  공부();
  게임();
  유튜브보기();

  if (weather === "GOOD") {
    운동();
    빨래();
  }

  if (isJob === "GOOD") {
    야간업무();
    조기취짐();
  }
}
```

## 27. 부정 조건문 지양하기

- 부정조건문을 지양해야 하는 이유는 부정조건문을 사용하면 **생각을 여러 번** 해야 할 수 있다.

```js
if (!isNaN(3)) {
  console.log("숫자입니다");
}
// 숫자입니다.
```

- 위와 같은 부정조건문(isNaN)이 사용된 코드는 여러 번 생각해야 해서 실수할 수 있기 때문에

- 아래와 같이 명시적인 긍정조건문(isNumber - 커스텀함수) 코드를 사용하는 편이 좋다.

```js
function isNumber(num) {
  return !Number.isNaN(num) && typeof num === "number";
}

if (isNumber(3)) {
  console.log("숫자입니다");
}
// 숫자입니다.
```

- 프로그래밍 언어 자체가 if문이 처음에 오고 true부터 실행시키는데, 부정조건문을 사용하면 false 조건의 값을 반환하기 위해 불필요하게 else문까지 써야 할 수 있다.

```js
const isCondition = true;
const isNotCondition = false;

// isCondition가 참일 때 실행된다
if (isCondition) {
  console.log("참인 경우에만 실행");
}

// isNotCondition가 거짓일 때 실행된다
if (isNotCondition) {
  console.log("거짓인 경우에만 실행");
} else {
  console.log("참인 경우에만 실행");
}
```

- 이러한 이유들 때문에 부정조건문에 대한 사용은 지양하고 긍정조건문 사용을 지향하는 방향이 좋다.

- 부정조건문 사용하는 예외 경우

  - Early Return을 사용할 때

  - Form Validation할 때 - 유효성 검증

  - 보안 또는 검사하는 로직에서

## 28. Default Case 고려하기

- 사용자의 실수를 예방하기 위해 Default Case를 고려하는 의식적인 노력이 필요하다

- 함수에서 들어오야 할 인수가 전달되지 않을 경우 OR 연산자를 사용하여 안전하게 Default 값을 미리 설정해두는 방법이 권장된다.

```js
function sum(x, y) {
  return x + y;
}

sum(100, 200);
```

- 코드를 아래처럼 개선할 수 있다.

```js
function sum(x, y) {
  x = x || 0;
  y = y || 0;
  return x + y;
}

console.log(sum()); // 0
```

- 아래는 특정 element를 만들어주는 함수이다

```js
function createElement(type, height, width) {
  const element = document.createElement(type);

  element.style.height = height;
  element.style.width = width;

  return element;
}
```

- width나 height를 일일이 넣고 싶지 않은 경우도 있기 때문에 아래처럼 코드를 수정해 줄 수 있다

```js
function createElement(type, height, width) {
  const element = document.createElement(type);

  element.style.height = height || 100;
  element.style.width = width || 100;

  return element;
}
```

- 아래 예시는 switch 예시로, 월요일 부터 일요일까지 이외 케이스는 대응할 필요가 없다고 생각할 수 있다

```js
function registerDay(userInputDay) {
  switch (userInputDay) {
    case "월요일": // some code
    case "화요일": // some code
    case "수요일": // some code
    case "목요일": // some code
    case "금요일": // some code
    case "토요일": // some code
    case "일요일": // some code
  }
}
```

- 그럼에도 불구하고 유저가 오타를 입력할 수도 있기 때문에 default case를 통해서 처리해준다

```js
function registerDay(userInputDay) {
  switch (userInputDay) {
    case "월요일": // some code
    case "화요일": // some code
    case "수요일": // some code
    case "목요일": // some code
    case "금요일": // some code
    case "토요일": // some code
    case "일요일": // some code
    default:
      throw Error("입력값이 유효하지 않습니다");
  }
}

registerDay("월ㄹ요일"); // 사용자의 입력 실수 케이스
// Error: 입력값이 유효하지 않습니다
```

- 라우팅 처리를 할 때도 없는 경로에 접근시 NotFound 페이지를 설정해준다

```js
const Root = () => (
  <Router history={browserHistory}>
    <Switch>
      <Route exact path="/" component={App} />
      <Route path="/welcome" component={Welcome} />
      <Route component={NotFound} />
    </Switch>
  </Router>
);
```

- [Swiper React Component](https://swiperjs.com/react#swiper-props) 공식문서를 보아도 Default 처리가 되어 있는 것을 확인할 수 있는데

- 이처럼 라이브러리들도 default 처리를 해주는 것을 중요하게 생각한다. 

- parseInt() 함수에서도 두 번째 매개변수(radix)의 기본값은 10이 아니다.

<br/>

- [공식문서](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/parseInt) 에도 아래처럼 나와있다

> string의 진수를 나타내는 2부터 36까지의 정수입니다. 주의하세요. 기본 값이 10이 아닙니다! Number 자료형이 아닌 경우 Number로 변환합니다.

- 그럼에도 불구하고 10진수 정수를 반환하려는 의도로 해당 함수를 사용하면서 두 번째 매개변수에 10을 생략하는 잘못된 경우가 많다.

- 명시적으로 10을 넘겨주어야 한다

- 아니면 아래처럼 함수를 정의해서 사용할 수도 있다.

```js
function safeDecimalParseInt(number, radix) {
  return parseInt(number, radix || 10);
}
```

## 29. 명시적인 연산자 사용 지향하기

- 명시적으로 연산자를 사용하여 예측 가능하고 디버깅하기 쉬운 코드를 작성해야 한다.

- 우선순위가 먼저인 부분을 소괄호로 묶는 것이 바람직하다.

```js
(x + y) * z;
```

- 전위연산자나 후위연산자를 사용하는 것보다 되도록 연산자를 명시적으로 사용하는 것이 바람직하다.

```js
number--;

// 위와 같은 후위연산자를 사용하는 것보다 아래처럼 명시적인 코드를 사용하는 것이 좋다.
number = number - 1;
```

## 30. Nullish coalescing operator

- 널 병합 연산자 `(??)` 는 왼쪽 피연산자가 null 또는 undefined일 때 오른쪽 피연산자를 반환하고, 그렇지 않으면 왼쪽 피연산자를 반환하는 논리 연산자이다.

- 모든 falsy값이 아닌 undefined와 null일 때만 처리해주고 싶을 때 사용

- Null 병합 연산자 필요성

  - OR 연산자를 기본값으로 사용하거나 단축 평가하고 싶은 경우에 falsy 값 때문에 의도치 않은 결과가 생길 수 있다.

  - OR 연산자는 왼쪽 피연산자가 falsy 값일 때 오른쪽 피연산자를 반환하고, 그렇지 않으면 왼쪽 피연산자를 반환한다.

  - 그런데 숫자 0도 falsy값이기 때문에, 아래 코드처럼 0을 반환하고 싶은 의도와 달리 오른쪽 피연산자를 반환하는 문제가 발생할 수 있다.

```js
0 || 10; // 10
```

- 이런 경우에 대안으로 편리하게 사용할 수 있는 것이 null 병합 연산자(??)다.

- null 병합 연산자는 null과 undefined를 평가할 때만 사용하면 된다.

```js
0 ?? 10; // 0
null ?? 10; // 10
undefined ?? 10; // 10
```

```js
function helloWorld(message) {
  if (!message) {
    return "Hello! World";
  }

  return "Hello! " + (message || "World");
}

function helloWorld(message) {
  return "Hello! " + (message || "World");
}
```

- message가 undefined와 null일 때는 'Hello! World'를 출력하고, 이외에는 message를 출력해주고 싶다.

- 하지만 0을 넣었을 때 0은 falsy이므로 'Hello! World'가 출력된다.

- 아래처럼 코드를 수정해준다

```js
function helloWorld(message) {
  return "Hello! " + (message ?? "") + " World";
}

console.log(helloWorld(undefined)); // Hello! World
console.log(helloWorld(0)); // Hello! 0 World
```

- 아래와 같은 함수가 있다고 하자,

```js
function createElement(type, height, width) {
  const element = document.createElement(type || "div");

  element.style.height = String(height || 10) + "px";
  element.style.width = String(width || 10) + "px";

  return element;
}
```

- 이러한 경우 아래처럼 0을 입력해도 10이 출력이 되는데

- 그 이유는 0이 falsy한 값이기 때문이다

```js
const el = createElement("div", 0, 0);

el.style.height; // 10px
el.style.width; // 10px

// !!0; // false

// 0 || "10"; // "10";
```

- 그렇기 때문에 아래처럼 Null 병합 연산자를 사용해서 null 또는 undefined일 때만 기본 값이 설정되도록 한다

```js
function createElement(type, height, width) {
  const element = document.createElement(type || "div");

  element.style.height = String(height ?? 10) + "px";
  element.style.width = String(width ?? 10) + "px";

  return element;
}
```

- 다만 falsy한 값을 평가할 때는 OR 연산자를 사용해야 하는 것을 유념하면서 Null 병합 연산자를 사용하도록 한다

- 아래와 같은 함수에서 undefined를 넣으면 원하는 값인 `"Hello! World";`가 출력되는데

- 0인 경우에도 Early return이 되는 문제가 있다.

```js
function helloWorld(message) {
  if (!message) {
    return "Hello! World";
  }

  return "Hello! " + (message || "World");
}

helloWorld(undefined); //  "Hello! World";
helloWorld(0); //  "Hello! World";
```

- 그렇기 때문에 message가 0인 경우에도 출력되도록 처리하고 싶다면 아래처럼 OR 연산자를 사용해서 함수를 처리해준다

```js
function helloWorld(message) {
  return "Hello! " + (message || "World");
}
```

- 그리고 &&, || 와 같은 연산자와 ??는 함께 사용할 수 없다

- Null 병합 연산자는 옵셔널 체이닝 연산자와 함께 사용하면 궁합이 좋다.

```js

/**
 * @see - https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator#no_chaining_with_and_or_or_operators
 */
function getUserName(isLogin, user) {
  return isLogin && user ?? user.name; // Error
}
```

- 그렇기 때문에 아래처럼 괄호를 묶어서 우선순위를 명시해주는 방식으로 수정해주어야 한다

```js
function getUserName(isLogin, user) {
  return (isLogin && user) ?? user.name; // Error
}
```

```js
function onToggleModal(isShow) {
  return isShow ?? false;
}
```

## 31. 드모르간의 법칙

```js
/**
 * 드모르간의 법칙
 */

if (!(A && B)) {
}
// 아래처럼 변경해준다
if (!A || !B) {
}

/*
 *
 *
 */

if (!(A || B)) {
}
// 아래처럼 변경해준다
if (!A && !B) {
}
```

- 아래는 예시코드이다

```js
const isValidUser = true;
const isValidToken = true;

if (isValidToken && isValidUser) {
  console.log("로그인 성공");
}
// 예를 들어 위와 같이 로그인 성공 확인하는 조건문이 있는데, 로그인 실패 케이스를 추가로 만든다고 하면, 기존의 상수값을 활용하여 아래와 같이 코드를 작성할 수 있다.

if (!(isValidToken && isValidUser)) {
  console.log("로그인 실패");
}

/**
 * 하지만 !(isValidToken && isValidUser) 뒤에 추가 연산이 더 붙게 된다면 가독성이 떨어지고 유지보수가 어려울 수 있다.
 * 따라서 해당 연산의 소괄호를 한 꺼풀 벗기는 것이 좋을 수 있는데, 이를 위해 드모르간의 법칙을 활용하여
 * 아래처럼 !isValidToken || !isValidUser 으로 리팩토링할 수 있다.
  if (!isValidToken || !isValidUser) {
    console.log("로그인 실패");
  }
*/
```

- AND 부정

```js
if (A && B) {
  // 성공
}

// 위와 같은 코드의 조건을 부정하면 아래처럼 드모르간 법칙을 써서 코드를 작성할 수 있다.

if (!A || !B) {
  // 실패
}
```

- OR 부정

```js
if (A || B) {
  // 성공
}

// 위와 같은 코드의 조건을 부정하면 아래처럼 드모르간 법칙을 써서 코드를 작성할 수 있다.

if (!A && !B) {
  // 실패
}
```
