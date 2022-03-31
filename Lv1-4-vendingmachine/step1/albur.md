# Level-3 유튜브 Step2 - 앨버

- 분석 담당 코드

  - [#7](https://github.com/woowacourse/javascript-vendingmachine/pull/7)
  - [#32](https://github.com/woowacourse/javascript-vendingmachine/pull/32)

  - [#8](https://github.com/woowacourse/javascript-vendingmachine/pull/8)
  - [#11](https://github.com/woowacourse/javascript-vendingmachine/pull/11)

  - [#19](https://github.com/woowacourse/javascript-vendingmachine/pull/19)
  - [#27](https://github.com/woowacourse/javascript-vendingmachine/pull/27)

  - [#38](https://github.com/woowacourse/javascript-vendingmachine/pull/38)
  - [#40](https://github.com/woowacourse/javascript-vendingmachine/pull/40)

## 다른 크루들 코드 분석

#### UI를 어떻게 나누었는지 (컴포넌트 - O vs X)

- HTMLElment를 상속하는 CustomElement를 만들어주고 이를 상속하는 `상품관리탭`과 `잔돈충전탭`으로 UI를 나누어줌

- `상품관리탭`과 `잔돈충전탭`으로 나누고 각 탭 내부를 세부적인 컴포넌트로 나누어줌

- `메뉴탭`과 `상품관리탭`과 `잔돈충전탭`으로 나누어줌

<br>

#### UI와 도메인 분리를 어떻게 했는지?

- `상품CRUD`과 `잔돈CR`을 도메인으로 하고 나머지는 UI로 분리

- `UI`와 `도메인`을 따로 분리하지 않고 `탭별`로 관리

- `UI`와 `도메인`를 분리한 것처럼 보이지만 `addEventlistener` 같은 로직이 `도메인` 파일에서 진행됨
  `flux 패턴`을 사용하여 `store와 reducer`를 활용해 `도메인` 구현

<br>

#### 상품 각각 CRUD시 리렌더링 방식? (전체 or 부분)

- `상품 추가`와 `상품 삭제`시 해당되는 상품만 리렌더링해주는 방식

  ```javascript
  insertItem(product: Product) {
    $('tbody', this).insertAdjacentHTML(
      'beforeend',
      `<tr class="product-item" data-product-name="${product.name}" data-product-id="${product.id}">
          <td>${product.name}</td>
          <td>${markUnit(product.price)}</td>
          <td>${product.quantity}</td>
          <td class="product-item__button">
            <button type="button" class="product-item__edit-button button">수정</button>
            <button type="button" class="product-item__delete-button button">삭제</button>
          </td>
       </tr>
      `,
    );
  }

  deleteItem(product: Product) {
    $(`[data-product-id="${product.id}"]`).remove();
  }
  ```

- `상품 CRUD`가 일어날때 `상품관리 tab`이 리렌더링되는 방식

<br>

#### validation (UI에서? or Domain에서?)

- Domain에서만
- UI에서만

<br>

#### css

```css
#product-control-table td,
#product-control-table th,
#charge-control-table td,
#charge-control-table th {
  width: 118px;
  height: 40px;
  border-top: 1px solid #dcdcdc;
  border-bottom: 1px solid #dcdcdc;
}
```

- 다들 비슷하게 공통 부분을 추출해서 적용해주었다. 위처럼 특정 table의 th와 td를 지정해주는 것이 추후 유지보수할 때 좋을 것 같아서 가져왔다.

<br>

#### 자바스크립트 엔진 콜 스택, 메모리 힙의 데이터 저장 구조

https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FwyILC%2Fbtrdon3nQV9%2FyWgZ1qDmEZDwzINEm5dkf1%2Fimg.png

<br>

## 피드백 정리

#### 대분류(ex: 아키텍처, 함수/클래스, 컨벤션, DOM, 테스트 등)

<br>

- [[#8](https://github.com/woowacourse/javascript-vendingmachine/pull/8#discussion_r835781324) - 클래스내 멤버 변수 선언]

  ```javascript
   class View() {
    init() {
      this.props = {
        target: this.contentsContainer,
        coinVault: this.coinVault,
      };
      this.contentsContainer.textContent = ``;
      this.balanceChargeInput = new BalanceChargeInput(this.props);
      this.coinVaultTable = new CoinVaultTable(this.props);
    }
  ```

  리뷰어님의 `멤버변수로 만들 필요가 있을까요?`라는 피드백을 보고 클래스내의 멤버변수를 선언해주는 것이 성능에 영향을 줄 수 있겠다는 생각이 들었다.

  <br>

- [[#7](https://github.com/woowacourse/javascript-vendingmachine/pull/7/files#r835727878) - .eslintrc 와 .eslint.js]

  - eslintrc와 eslint.js의 차이?
  - 모르겠다..

<br>

- [[#7](https://github.com/woowacourse/javascript-vendingmachine/pull/7/files#r838078443) - 변수명을 숫자로 하면 안되는 이유]

  ```javascript
  interface ICoin {
    500: number;
    100: number;
    50: number;
    10: number;
  }
  ```

  ```
    변수명을 500으로 하면 문제인 이유는,

  1.  매직넘버와 실제 영어문장으로 된 코드가 섞여있을때 햇갈리기 때문입니다
  2.  의미론적으로 파악할 수가 없습니다. 의도가 무엇이고, Coin에 500 100 등이 있을때 생각하기가 어렵죠..
  3.  일반적으로 멤버변수를 숫자로 만들었다면, 그 객체는 잘못 설계했을 가능성이 높습니다.
  ```

<br>

- [[#19](https://github.com/woowacourse/javascript-vendingmachine/pull/19/files#r836525890) - 현재 구조에 의존한 element 접근]

  ```
  e.target.closest("tr")은 현재 구조에서는 동작하지만 구조가 조금만 변경되도 깨질 수 있는 구조입니다
  변경될 수 있는 dom의 구조에 의존해서 데이터를 가져오는 방식은 권장하기 어렵습니다
  다른 방식을 통해 입력값을 가져오는 방법을 고민해 보시면 좋을 것 같습니다
  ```

  `<tr>` 태그에 의존해서 다른 element에 접근해서 값을 찾아오고 있다. 현재 구조가 달라지면 해당 코드가 작동이 잘 안될 가능성이 크기 때문에, 이러한 코드들을 개선해봐도 좋겠다.

<br>

- [[#27](https://github.com/woowacourse/javascript-vendingmachine/pull/27/files#r835926174) - cypress alias]

  ```javascript
  cy.get('.charge-coin-count').eq(3).should('have.text', '4개');
  ```

  - 위 선택자는 alias를 통해서 10원이라는 정보에 별칭을 지어보면 좋을거같아요.
  - 변수를 따로 선언해서 명시적으로 값을 나타내는 것처럼 cypress에서도 `alias`를 활용해주면 좋겠다.

<br>

- [[#27](https://github.com/woowacourse/javascript-vendingmachine/pull/27/files#r835922642) - 경계값 테스트]

  ```javascript
  cy.get('.charge-control-input').type(MAX_PRICE + 1);
  ```

  - 위처럼 경계값을 테스트하면 직관적이고 편하다.

<br>

- [[#27](https://github.com/woowacourse/javascript-vendingmachine/pull/27/files#r835934011) - 생성자로 할당(타입스크립트)]

  ```javascript

  // 인자로 넘겨 받은 converTemplate을 할당해줄 수 있다.
  constructor(private readonly convertTemplate:
   (path: string) => void) {
    this.vendingmachineWrap = selectDom("#app");
    addEvent(this.vendingmachineWrap, "click", this.handleMenuTab);
  }
  ```

<br>

- [[#32](https://github.com/woowacourse/javascript-vendingmachine/pull/32/files#r836498728) - 태그 네임을 활용한 css 적용]

  ```css
  input::-webkit-outer-spin-button,
  input::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }
  ```

  - 현재 미션에서는 크게 문제가 없을거라 생각되지만 태그네임을 기준으로 css를 적용하는건 지양하시는게 좋아요 👀

<br>

- [[#32](https://github.com/woowacourse/javascript-vendingmachine/pull/32/files#r835947796) - dataset 적용을 위한 id 부여]

  ```javascript
  class Product implements IProduct {
  id: string;
  name: string;
  price: number;
  quantity: number;

  constructor(product: Product, id = Math.random().toString(36).substring(2, 9)) {
  ```

  - 상품 관리를 위해 상품을 생성할때 id를 부여!!
