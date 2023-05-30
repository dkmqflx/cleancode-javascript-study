## 63. 유효성 검사

- 사용자의 입력 값이 유효한지 검증하는 것

- 사용자와 상호작용 => 사용자의 입력을 받거나 그것을 통해서 **무언가** 하게 된다

- 이메일인 경우

  - 사용자의 입력이 이메일 포맷에 맞는지 검증한다

    - 이메일 포맷이 맞는 경우 그때서야 서버와 통신을 한다

    - 그렇기 때문에 유효성 검증을 통해서 서버와의 비용을 절감할 수 있다

- 어떻게 할까 ?

  - 정규식

  - 자바스크립트 문법 (문자열 검사를 ..)

  - 웹 표준 API

    - 예를들어 input 태그의 type 속성

      - text

      - password

- 그럼 유효성 검사는 어디서 할까 ?

  - 할 수 있는 모든 곳에서 다 처리하는 게 좋다

  - 사용자의 입력을 받으면

    - 클라이언트에서 처리하고

      - 이 때, HTML, JavScript 모두 처리하는 것이 좋다

    - 백엔드에서도 처리하는 것이 좋다

## 64. try ~ catch

- 예외를 처리한다 ?

- 프론트엔드에서는 사용자의 입력을 받는다

- 개발자가 모든 에러를 예측하여 처리하기가 어렵고 거의 불가능에 가깝다

- 가장 좋은 방법은 예외를 처리하는 방법을 사용하는 것

```js
function handleSubmit(input) {
  // 중요하지 않다고 생각하는 핸들링을 try에 포함시키지 않는 것
  // 하지만 사용자의 인풋을 받을 때는 모두 중요한 것이기 때문
  // 개발자가 임의로 판단해서 try catch에 빼면 안된다
  try {
    // 에외가 예상되는 코드 혹은 발생시킬 코드


  // try catch
    try{

    }catch(){

    }
  } catch (error) {
    // 예외를 처리하는 코드
  } finally {
  }
}
```

- 또 많이 실수하는 것은 중첩해서 try catch를 사용하는 것

```js


function handleSubmit(input) {

  try {
    try{

    }catch(){

    }
  } catch (error) {
    // 예외를 처리하는 코드
  } finally {
  }
}


```

- 그렇기 때문에 try catch는 사용할 때는 함수단위로 사용하도록 한다

```js
function util() {
  return; // some code
}

function handleSubmit(input) {
  try {
    // 에외가 예상되는 코드 혹은 발생시킬 코드
    util(); // util에는 try catch 없지만, 여기 try catch가 있기 때문에 여기서 잡아준다
  } catch (error) {
    // 예외를 처리하는 코드
  } finally {
  }
}
```

- 또한 이런 식으로 경우를 나누어서 에러 처리를 해준다

```js
function util() {
  return; // some code
}

function handleSubmit(input) {
  try {
  } catch (error) {
    // 1. 개발자를 위한 예외 처리 => 동료 개발자에게 제안을 하는 것, 혹은 나를 위해서 이러한 에러 처리를 해준다
    console.error(error);

    // 2. 사용자를 위한 예외 처리 => 사용자가 볼 수 있다고 생각하고
    alert(error.message); // 사용자를 위한 메세지를 보여주도록 한다

    // 3. 사용자에게 사용을 제안
    history.back();
    history.go("안전한 어딘가로 ... ");
    clear();
    element.focus(); // => 어딘가로 이동을 시켜서 다시 한번 사용자에 알려주기

    // 4. 에러 로그 수집
    sentry.전송(); // sentry : 에러가 발생 했을 때 수집하는 도구

    // 5. 비추천하지만 필요에 다라 사용되는 경우
    // 재귀 호출()

    // 내가 호출한 함수 handleSubmit 다시 호출하는 경우가 있다
    // 함수 사용하다가 예외가 발생하는 경우 호출
  } finally {
    // 데이터 분석을 위한 로그를 주로 많이 넣는 편
  }
}
```

## 65. 사용자에게 알려주기

- 사용자는

  - 동료 개발자

  - 내가 만든 앱을 이용하는 사용자

    - 내가 만든 앱은 라이브러리 => ex. react, axios, loadash 등
    - 내가 만든 실제 앱 등 => ex. 간단한 애플리케이션

```js
function React() {
  // 생성자로 사용하기 바랄 때

  if (!new.target) {
    throw new Error("생성자 입니다");
    // throw new ReferenceError("생성자 입니다");, 레퍼런스 에러를 던지고 싶은 경우
  }
}

React(); // 에러 발생, 생성자로 사용하지 않았기 때문

const react = new React(); // 에러 발생 안함
```
