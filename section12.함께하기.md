## 71. ����

- ���鵵 �ڵ� �ۼ��� �Ϻδ�

- ���ڵ��̶�� �ؼ� �Ʒ�ó�� �������� ������ �ִ��� ���̷��� �ϴ� ��� �ִ�

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

- ������ �̷��� �� �ʿ䰡 ���µ�, ����� �� �� ���̰� ������� ���� �� �ֱ� �����̴�

- �� �ڵ带 �Ʒ�ó�� ������ ������ ������ �� �ִ�

```js
export const loadingElements = () => {
  // 1. ����
  // 2. ����(��)
  // 3. ��ȯ

  // 1. ���� ��� ��´�
  const el = document.createElement('div');
  const el2 = document.createElement('div');
  const el3 = document.createElement('div');

  // 2. ����, ��
  el.setAttribute('class', 'loading d-flex justify-center mt-3');
  el2.setAttribute('class', 'relative d-flex justify-center mt-3');
  el3.setAttribute('class', 'loading d-flex justify-center mt-3');

  // 2. ����, ��
  el.append(el2);
  el2.append(el3);

  // 3. ��ȯ, ��ĭ�� ����� ��������� ��Ÿ�� �� �ִ�
  return el;
};
```

- es lint�� padding-line-between statments ���� ����ϴ� ���� �����Ѵ�

## 72. indent depth

- �������� ���� �������

  - ���������� �ʵ��� �����Ѥ� ��

- �鿩���⸦ ��� ������ ����������

  - HTML ���� 2 depth

  - JS ���� 2 depth

- ������ ���θ��� �ٸ���

- depth�� �߿��� ����, ��ø �� �� ���� �������� ��������

- �̷� ������ �ذ��ϱ� ���� ������� �ռ� ����ߴ�

- �Ʒ��� �ǽ������� �ڵ带 �ۼ��ϴ� ���

  - �����ȯ

  - callback => Promise => Async & Await

  - ���� �Լ� (map, reduce, filter)

  - �Լ��� ������ �߻�ȭ�ϱ�

  - �޼��� ü�̴� (`.then().then.then()`)

    ```js
    function test() {
      somePromise() // depth�� �� ������� �ʴ´�
        .then() //
        .then() //
        .then();
    }
    ```

- �ǽ������� depth�� ���̴� ���� ���� �߿��ϴ�

  - ����Ʈ�� ��� Context hell

- ������ ������ ����ؼ� ���� �ϴ� ���

  - ù��°, �������� indet size�� �����Ѵ�

  - ����Ƽ���� tab width�� �����Ѵ�

  - eslint�� ����ؼ� indent rule�� �����Ѵ�

## 73. ��Ÿ�� ���̵�

- ���̹� �������� �����ϴ� ��Ģ�� ���� ���̵�������� �� Ȥ�� ������ ���� ����

- ��, ������ ū ������ �ֱ� ����

- ���θ� �����ϱ� ���� �ð� ����

- �ڵ� ǰ���� �ø� �� �ִ�

- �ϰ���

- ������ ���

- �������� ���̼�

- ��ǥ���� ��Ÿ�� ���̵�

  - vue style guide

  - rddux style guide

  - google style guide - js

  - javascript standard style

  - airbnb javascript style guide

    - ���� �Ǿ���

  - rush stack

    - ms���� ��뷹���� ���� ���� ��Ÿ�� ���̵�

  - mdn style guide

  - �� ���� ��Ÿ�� ���̵带 ���� �ʿ��� �κ��� �����Ѵ�
