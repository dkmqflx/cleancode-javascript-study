## 66. HTML Semantic Element

- 웹 표준은 중요하다

- 채용공고만 봐도 확인할 수 있다

```html
<body>
  <header></header>

  <main>
    <article>
      <section></section>
    </article>

    <!-- 개발자 도구 보면 자동으로 tbody가 삽입된 것을 확인할 수 있다 -->
    <table>
      <tr>
        <td></td>
      </tr>
    </table>
  </main>

  <footer></footer>
</body>
```

## 67, NodeList

- Node : 문서내에 모든 객체

  - DOM이라고 생각해도 된다

- Element: Tag로 둘러싸인 요소

```js
const arr = document.querySelectorAll('a');
// arr 은 NodeList
// 공식문서 보면 NodeList가 제공하는 메서드를 확인할 수 있다
// https://developer.mozilla.org/ko/docs/Web/API/NodeList
// 인스턴스 메서드 항목

// NodeList 사요할 때는 아래처럼 형변환을 거쳐야지 일반적인 배열 처럼 사용할 수 있다
const arrFromNode = [...arr];
```

## 68. innerHTML

- innerHTML은 굉장히 오래되었고 좋지 않은 API

- XSS에 취약하다

  - 구글 로그인 창에서 개발자 도구를 열어서 Console 탭을 확인해본다

```html
<html>
  <body>
    <main></main>

    <script>
      document.querySelector('main').innerHTML = '<h1?>Hello World</h1>';
    </script>
  </body>
</html>
```

- [공식문서](https://developer.mozilla.org/en-US/docs/Web/API/Element/innerHTML)를 보면 아래처럼 setHTML을 사용하라고 되어있다

  - 아직은 실험적인 API

  - 프로덕션에 사용하기에는 아직

  - can i use에서도 검색해보면 지원하지 않는 브라우저가 많은 것을 확인할 수 있다

- Replacing the contents of an element

  - Setting the value of innerHTML lets you easily replace the existing contents of an element with new content.

> Note: This is a security risk if the string to be inserted might contain potentially malicious content. When inserting user-supplied data you should always consider using Element.SetHTML() instead, in order to sanitize the content before it is inserted.

- 그렇기 때문에 대신 사용할 수 있는 것이 insertAdjacentHTML

- 공식문서를 보면 다음과 같이 나와있다

- 요소(element)의 내용을 변경하는 대신 HTML을 문서(document)에 삽입하려면, insertAdjacentHTML() 메서드를 사용하십시오.

- Syntax

  ```js
  const content = element.innerHTML;

  element.innerHTML = htmlString;
  ```

- Value

  - 요소(element)의 자손의 HTML 직렬화를 포함하는 DOMString 입니다. Setting the value of innerHTML 의 값을 설정(대입)하면 요소의 모든 자손이 제거되고, 문자열 htmlString에 지정된 HTML을 파싱하고, 생성된 노드로 대체합니다.

- insertAdjacentHTML이 50% 더 좋은 성능을 가진다

- 직렬화 하고 파싱하고 이런 작업을 하기 때문에 성능도 좋지 않고 보안적으로도 좋지 않다

- insertAdjacentHTML 공식문서 보면 파싱에 대한 이야기 없는 것을 확인할 수 있다

- insertAdjacentText는 렌더링 문자뎔만 할 때 사용한다

- innerText도 사용할 수 있다

  - 하지만 공식문서 보면 textContent와 사용의 차이점이 나와있다고 나와있다

- innerText는 화면에 있는 텍스트를 그대로 읽어온다

  - 그리고 리플로우가 발생한다

- textContent는 화면에 있는 그대로가 아니고 XSS 공격에서 안전하다

- innerHTML => insertAdjancentHTML

- 문자열만 렌더링 : innerHTML = innerTexst => textContent

  - insertAdjacentText도 있다.

- 아래 3개만 사용해도 충분하다
- insertAdjacentHTML
- insertAdjacentElement
- insertAdjacentText

## 69. Data Attributes

```html
<html>
  <body>
    <h1>Hello World</h1>

    <main></main>

    <!-- 아래처럼 존재하지 않는 비표준 속성을 사용할 수 도 있다 -->
    <!-- 보안적으로도 좋지 않고, 문법적으로도 좋지 않은 방법 -->
    <h1 안녕="하세요"></h1>

    <!-- 이러한 문제점을 해결하기 위해 등장한 것이  데이터 속성 -->

    <main id='electriccars' data-id="1" data-index-number='1234' ></h1>

    <script>
      const main = document.querySelector('electriccars');
      main.dataset.columns // 3
      main.dataset.indexNumber // 1234, kebab case로 선언되어 있더라도  camelCase로 사용한다


    </script>
  </body>
</html>
```

## 70. Black Box Event Listener

- 개인적인 사례, 주관이 들어간 챕터

  - 팀에 따라서 의도적으로 이렇게 작성할 수 도 있다

- 내부가 구현이 어떻게 동작될지 예측할 수 없는 경우

- 추상화가 잘못된 사례로 너무 과하게 추상화가 되거나

- 명시적인 코드가 아닌 경우

```js
const button = document.querySelector('button');

// 버튼.이벤트_등록('이벤트_타입', 리스터_함수_실행) => 반응형으로 실행된다
button.addEventListener('click', onClick);
button.addEventListener('click', handleClick);

// 어떤 이벤트가 일어날 지 알 수 없다
// 즉, 무슨일이 일어날 지 알 수 없다

// 아래처럼 작성하면 무슨일이 일어날 지 예측할 수 있다
function getLog(e) {
  console.log(e);
}
button.addEventListener('click', getLog);
```

- search bar를 만들 때에도 엔터 버튼에서도 동작해야 한다

- 아래처럼 함수이름을 작성하면 무엇을 제출하는지 알 수 없다

```js

// 검색
const handleClick = (e) => {
  1. input을 받는 코드
  2. 유효성 검사를 하는 코드
  3. form을 전송하는 코드
}

button.addEventListener('click', handleClick);

// 엔터에 대응하기 위해 아래처럼 등록해주어야 하는데 keyup에 handleClick은 어울리지 않는다
button.addEventListener('keyup', handleClick);

// 그렇기 때문에 아래처럼 함수이름을 변경해준다
const handleSearch = (e) => {
  1. input을 받는 코드
  2. 유효성 검사를 하는 코드
  3. form을 전송하는 코드
}

// 그리고 form에 submit 이벤트를 등록해준다
// 함수명이 이전의 handleClick과 달리 handleSearch 이므로 무슨 일이 일어날지 예측할 수 있다
form.addEventListener('onsubmit', handleSearch)


```
