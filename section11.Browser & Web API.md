## 66. HTML Semantic Element

- �� ǥ���� �߿��ϴ�

- ä����� ���� Ȯ���� �� �ִ�

```html
<body>
  <header></header>

  <main>
    <article>
      <section></section>
    </article>

    <!-- ������ ���� ���� �ڵ����� tbody�� ���Ե� ���� Ȯ���� �� �ִ� -->
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

- Node : �������� ��� ��ü

  - DOM�̶�� �����ص� �ȴ�

- Element: Tag�� �ѷ����� ���

```js
const arr = document.querySelectorAll('a');
// arr �� NodeList
// ���Ĺ��� ���� NodeList�� �����ϴ� �޼��带 Ȯ���� �� �ִ�
// https://developer.mozilla.org/ko/docs/Web/API/NodeList
// �ν��Ͻ� �޼��� �׸�

// NodeList ����� ���� �Ʒ�ó�� ����ȯ�� ���ľ��� �Ϲ����� �迭 ó�� ����� �� �ִ�
const arrFromNode = [...arr];
```

## 68. innerHTML

- innerHTML�� ������ �����Ǿ��� ���� ���� API

- XSS�� ����ϴ�

  - ���� �α��� â���� ������ ������ ��� Console ���� Ȯ���غ���

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

- [���Ĺ���](https://developer.mozilla.org/en-US/docs/Web/API/Element/innerHTML)�� ���� �Ʒ�ó�� setHTML�� ����϶�� �Ǿ��ִ�

  - ������ �������� API

  - ���δ��ǿ� ����ϱ⿡�� ����

  - can i use������ �˻��غ��� �������� �ʴ� �������� ���� ���� Ȯ���� �� �ִ�

- Replacing the contents of an element

  - Setting the value of innerHTML lets you easily replace the existing contents of an element with new content.

> Note: This is a security risk if the string to be inserted might contain potentially malicious content. When inserting user-supplied data you should always consider using Element.SetHTML() instead, in order to sanitize the content before it is inserted.

- �׷��� ������ ��� ����� �� �ִ� ���� insertAdjacentHTML

- ���Ĺ����� ���� ������ ���� �����ִ�

- ���(element)�� ������ �����ϴ� ��� HTML�� ����(document)�� �����Ϸ���, insertAdjacentHTML() �޼��带 ����Ͻʽÿ�.

- Syntax

  ```js
  const content = element.innerHTML;

  element.innerHTML = htmlString;
  ```

- Value

  - ���(element)�� �ڼ��� HTML ����ȭ�� �����ϴ� DOMString �Դϴ�. Setting the value of innerHTML �� ���� ����(����)�ϸ� ����� ��� �ڼ��� ���ŵǰ�, ���ڿ� htmlString�� ������ HTML�� �Ľ��ϰ�, ������ ���� ��ü�մϴ�.

- insertAdjacentHTML�� 50% �� ���� ������ ������

- ����ȭ �ϰ� �Ľ��ϰ� �̷� �۾��� �ϱ� ������ ���ɵ� ���� �ʰ� ���������ε� ���� �ʴ�

- insertAdjacentHTML ���Ĺ��� ���� �Ľ̿� ���� �̾߱� ���� ���� Ȯ���� �� �ִ�

- insertAdjacentText�� ������ ���ڵ��� �� �� ����Ѵ�

- innerText�� ����� �� �ִ�

  - ������ ���Ĺ��� ���� textContent�� ����� �������� �����ִٰ� �����ִ�

- innerText�� ȭ�鿡 �ִ� �ؽ�Ʈ�� �״�� �о�´�

  - �׸��� ���÷ο찡 �߻��Ѵ�

- textContent�� ȭ�鿡 �ִ� �״�ΰ� �ƴϰ� XSS ���ݿ��� �����ϴ�

- innerHTML => insertAdjancentHTML

- ���ڿ��� ������ : innerHTML = innerTexst => textContent

  - insertAdjacentText�� �ִ�.

- �Ʒ� 3���� ����ص� ����ϴ�
- insertAdjacentHTML
- insertAdjacentElement
- insertAdjacentText

## 69. Data Attributes

```html
<html>
  <body>
    <h1>Hello World</h1>

    <main></main>

    <!-- �Ʒ�ó�� �������� �ʴ� ��ǥ�� �Ӽ��� ����� �� �� �ִ� -->
    <!-- ���������ε� ���� �ʰ�, ���������ε� ���� ���� ��� -->
    <h1 �ȳ�="�ϼ���"></h1>

    <!-- �̷��� �������� �ذ��ϱ� ���� ������ ����  ������ �Ӽ� -->

    <main id='electriccars' data-id="1" data-index-number='1234' ></h1>

    <script>
      const main = document.querySelector('electriccars');
      main.dataset.columns // 3
      main.dataset.indexNumber // 1234, kebab case�� ����Ǿ� �ִ���  camelCase�� ����Ѵ�


    </script>
  </body>
</html>
```

## 70. Black Box Event Listener

- �������� ���, �ְ��� �� é��

  - ���� ���� �ǵ������� �̷��� �ۼ��� �� �� �ִ�

- ���ΰ� ������ ��� ���۵��� ������ �� ���� ���

- �߻�ȭ�� �߸��� ��ʷ� �ʹ� ���ϰ� �߻�ȭ�� �ǰų�

- ������� �ڵ尡 �ƴ� ���

```js
const button = document.querySelector('button');

// ��ư.�̺�Ʈ_���('�̺�Ʈ_Ÿ��', ������_�Լ�_����) => ���������� ����ȴ�
button.addEventListener('click', onClick);
button.addEventListener('click', handleClick);

// � �̺�Ʈ�� �Ͼ �� �� �� ����
// ��, �������� �Ͼ �� �� �� ����

// �Ʒ�ó�� �ۼ��ϸ� �������� �Ͼ �� ������ �� �ִ�
function getLog(e) {
  console.log(e);
}
button.addEventListener('click', getLog);
```

- search bar�� ���� ������ ���� ��ư������ �����ؾ� �Ѵ�

- �Ʒ�ó�� �Լ��̸��� �ۼ��ϸ� ������ �����ϴ��� �� �� ����

```js

// �˻�
const handleClick = (e) => {
  1. input�� �޴� �ڵ�
  2. ��ȿ�� �˻縦 �ϴ� �ڵ�
  3. form�� �����ϴ� �ڵ�
}

button.addEventListener('click', handleClick);

// ���Ϳ� �����ϱ� ���� �Ʒ�ó�� ������־�� �ϴµ� keyup�� handleClick�� ��︮�� �ʴ´�
button.addEventListener('keyup', handleClick);

// �׷��� ������ �Ʒ�ó�� �Լ��̸��� �������ش�
const handleSearch = (e) => {
  1. input�� �޴� �ڵ�
  2. ��ȿ�� �˻縦 �ϴ� �ڵ�
  3. form�� �����ϴ� �ڵ�
}

// �׸��� form�� submit �̺�Ʈ�� ������ش�
// �Լ����� ������ handleClick�� �޸� handleSearch �̹Ƿ� ���� ���� �Ͼ�� ������ �� �ִ�
form.addEventListener('onsubmit', handleSearch)


```
