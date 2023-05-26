## 10. 타입 검사

- typeof는 문자열로 반환하는 것이 특징

  - typeof true : 'boolean'

- 아래처럼 함수처럼도 사용가능하다

  - typeof (true)

- PRIMITIVE vs REFERENCE

  - 원시타입은 불변하지만,

  - 객체 값(Array, Function, Date)들은 typeof로 감별하기가 어렵다

  - 아래는 함수를 사용할 때의 예시로 클래스의 타입은 함수로 나오게 된다

    ```js
    function A(){}

    typeof A // 'function'

    class C

    typeof C // 'function', 생성자 함수의 suger syntac 때문


    ```

  - 래퍼 객체의 예시도 있다.

    ```js
    const str = new String("문자열");

    typeof str; // 'object'
    ```

  - 가장 치명적인 예시는 아래와 같이 null

    - 이것은 자바스크립트에서도 인정한 언어적 오류, 수정을 할 수 없기에 수정하지 않는 것

    ```js
    typeof null; // 'object'
    ```

- 레퍼런스 검사의 또 다른 어려움은 자바스크립트가 동적으로 변하는 언어이기 때문에 타입도 동적으로 변한다는 것이다

- 그래서 타임 검사할 때 조심해야 한다

```js
// instanceof 연산자 사용하면 객체의 프로토타입 체인을 검사할 수 있다

function Person(name, age) {
  this.name = name;
  this.age = age;
}

const p = new Person("kim", 32);

const x = {
  name: "test",
  age: 99,
};

p instanceof Person; // true
x instanceof Person; // false
```

- 여기의 크나큰 함정이 있는데 다음과 같다

- 레퍼런스이기 때문에 최상위는 Object 객체 이기 때문에 타입 검사가 어려워 진다

```js
const arr = [];

const func = function () {};

const date = new Date();

arr instanceof Array; // true

func instanceof Function; // true

date instanceof Date; // true

arr instanceof Object; // true

func instanceof Object; // true

date instanceof Object; // true
```

- 아래처럼 해결할 수 있다

  - 래퍼객체 까지 감별할 수 있다

```js
Object.prototype.toString.call(arr);
Object.prototype.toString.call(func);
Object.prototype.toString.call(date);
```

- 구글에 검사할 때,아래와 같이 검색해서 확인해본다

  - javascript is array

  - javascript is function

- 자바스크립트 언어는 동적인 타입을 가지는 언어이기 때문에 타입 검사도 어렵다

- 그래서 검색을 해서 상황에 맞는 타입 검사 방법을 검색해서 적용해준다

- 일일이 외울 필요는 없다

- typeof는 무적이 아니다

- instance가 있다는 것

---

## 11. undefined & null

```js
!null; // true

!!null; // false,  !! 있으면 형변환시켜준다

typeof null; // object 타입

null === false; // false

!null === true; // true

// null => 수학적으로는 0
null + 123; // 123

// 선언했지만 값은 정의되지 않고 할당 X
let varb;

typeof varb; // 'undefined', undefined 타입

// null과 달리 NaN 출력된다
undefined + 10; // NaN

!undefined; // true

undefined == null; // true;
undefined === null; // false
!undefined === !null; // true
```

- 이렇게 혼동되는 부분이 많기 때문에 null 또는 undefined을 어떻게 사용할지 컨벤션을 정해주는 것이 좋다

- undefined, null은 값이 없거나 정의되지 않은 것

- null 같은 경우 명시적으로 값이 없다는 것을 표현한 것

## 12. eqeq(==) 줄이기

- eqeq는 자바스크립트의 동등연산자를 의미한다

- `==`동등연산자 사용하면 암묵적인 형변환이 일어난다. 따라서 엄격한 동등 연산자 `===`를 사용한다

```js
"1" == 1; // true

1 == true; // true
```

## 13. 형변환 주의하기

- 명시적 형변환을 하도록 한다

```js
11 + "ㄹㄹㄹ"; // '11ㄹㄹㄹ'

!!"ㄴ"; // true

!!""; //false

// 암묵적이 아닌, 명시적 변환
parsInt("9.999", 10); // 9,
// 두번째 인자는  몇진수로 할지
// 뒤에 넣는 값을 지정하지 않으면 10진수는 기본 값이 아니다
// 따라서 10진수라는 것을 지정해준다
```

## 14. isNaN

- 숫자는 10진수이더라도 컴퓨터는 2진수

- 이러한 간극에서 생기는 소수점을 다루기 위해, 자바스크립트에서는 IEEE 754을 기반으로 부동소수점 방식을 사용해서 해결하려고 한다

```js
Number.MAX_SAFE_INTEGER; // 가장 큰 수

Number.isInteger; // 검사

isNaN; // is Not A Number, 숫자가 아니다

typeof isNaN; // object

isNaN(123); // false -> 숫자가 숫자가 아니다 -> 숫자가 맞다,  그래서 false

isNaN(123 + "테스트"); // true, 느슨한 검사
```

- js에서도 이렇게 문제가 많은 것을 확인하고 ES2015+부터 폴리필이라는 것을 만들었다

- 아래처럼 Number를 한번만 붙여주면 엄격한 검사를 한다

```js
Number.isNaN(123 + "테스트"); // false, 엄격한 검사
// 이렇게 Number를 붙여 준다
```
