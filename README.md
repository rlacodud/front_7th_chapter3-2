## 배포 링크
[basic 배포 경로](https://rlacodud.github.io/front_7th_chapter3-2/index.basic)
[advanced 배포 경로](https://rlacodud.github.io/front_7th_chapter3-2/index.advanced)

## 과제의 핵심취지

- React의 hook 이해하기
- 함수형 프로그래밍에 대한 이해
- 액션과 순수함수의 분리

## 과제에서 꼭 알아가길 바라는 점

- 엔티티를 다루는 상태와 그렇지 않은 상태 - cart, isCartFull vs isShowPopup
- 엔티티를 다루는 컴포넌트와 훅 - CartItemView, useCart(), useProduct()
- 엔티티를 다루지 않는 컴포넌트와 훅 - Button, useRoute, useEvent 등
- 엔티티를 다루는 함수와 그렇지 않은 함수 - calculateCartTotal(cart) vs capaitalize(str)

### 기본과제

- Component에서 비즈니스 로직을 분리하기
- 비즈니스 로직에서 특정 엔티티만 다루는 계산을 분리하기
- 뷰데이터와 엔티티데이터의 분리에 대한 이해
- entities -> features -> UI 계층에 대한 이해

- [x] Component에서 사용되는 Data가 아닌 로직들은 hook으로 옮겨졌나요?
- [x] 주어진 hook의 책임에 맞도록 코드가 분리가 되었나요?
- [x] 계산함수는 순수함수로 작성이 되었나요?
- [x] Component에서 사용되는 Data가 아닌 로직들은 hook으로 옮겨졌나요?
- [x] 주어진 hook의 책임에 맞도록 코드가 분리가 되었나요?
- [x] 계산함수는 순수함수로 작성이 되었나요?
- [x] 특정 Entitiy만 다루는 함수는 분리되어 있나요?
- [x] 특정 Entitiy만 다루는 Component와 UI를 다루는 Component는 분리되어 있나요?
- [x] 데이터 흐름에 맞는 계층구조를 이루고 의존성이 맞게 작성이 되었나요?

### 심화과제

- 이번 심화과제는 Context나 Jotai를 사용해서 Props drilling을 없애는 것입니다.
- 어떤 props는 남겨야 하는지, 어떤 props는 제거해야 하는지에 대한 기준을 세워보세요.
- Context나 Jotai를 사용하여 상태를 관리하는 방법을 익히고, 이를 통해 컴포넌트 간의 데이터 전달을 효율적으로 처리할 수 있습니다.

- [x] Context나 Jotai를 사용해서 전역상태관리를 구축했나요?
- [x] 전역상태관리를 통해 domain custom hook을 적절하게 리팩토링 했나요?
- [x] 도메인 컴포넌트에 도메인 props는 남기고 props drilling을 유발하는 불필요한 props는 잘 제거했나요?
- [x] 전체적으로 분리와 재조립이 더 수월해진 결합도가 낮아진 코드가 되었나요?


## 과제 셀프회고

<!-- 과제에 대한 회고를 작성해주세요 -->

### 과제를 하면서 내가 알게된 점, 좋았던 점은 무엇인가요?
#### 1. 함수형 프로그래밍(Functional Programming)
함수형 프로그래밍은 사실상 프로그래밍 철학에 가까운 개념으로, 한 문장으로 말하자면 아래와 같습니다.
> 데이터는 변하지 않으며, 프로그램은 순수 함수들의 조합으로 구성된다.
 
**(1) 데이터는 변하지 않는다(Immutability)**
객체나 배열의 값을 직접 바꾸는 대신, 기존 데이터를 기반으로 새로운 데이터를 만들어내야 합니다.
그렇게 강조해대는 **불변성**의 개념인데 결국 상태가 언제 바뀌었는지 명확해야 **렌더링 퇴적화**나 **변경 추적**이 가능하기 때문에 강조하는 것입니다.

**🧐 왜 그래야 할까요?**
- 값이 중간에 바뀌지 않으니 예측 가능성이 높아지고
- 디버깅이 쉬워지고
- 동시성 문제(멀티 스레드, 비동기)에서 충돌이 줄어들고
- 상태를 되돌리거나 시간 여행 디버깅(Time-travel debugging)이 가능해지기 때문입니다.
 
**(2) 순수 함수로 구성한다(Pure Function)**
순수 함수가 무엇일까요? 아래만 기억합시다!

- 같은 입력 → 항상 같은 출력
- 함수 외부 상태를 변경하지 않음
```javascript
function pureAdd(a, b) {
  return a + b;         // 같은 입력 → 같은 결과
}

function impureAdd(a, b) {
  console.log("실행됨"); // 외부 상태 사용 (콘솔)
  return a + b;
}
```

근데 저런 util 함수 단계정도면 충족하기 쉽지만 데이터를 다루는 등의 복잡한 로직에서는 그게 어떻게 가능할까요?

FP에서 말하고자 하는 바는 **모든 걸 순수함수로 만들라는 것이 아닙니다.**
**순수함수를 최대화**하고 **부수효과는 의도된 위치에 모아두어** 변경 및 관리가 용이하도록 하면 됩니다.

**(3) 함수는 조합 가능한 작은 블록이다(Composition)**
함수형 프로그래밍에서는 큰 문제를 해결하기 위해 함수를 계속 합성(조합)해 나가는 방식을 선호합니다.
```javascript
const double = x => x * 2;
const inc = x => x + 1;

const process = x => double(inc(x));

process(3); // 8
```
이걸 FP에서는 **함수 합성**이라고 하며, 레고 블럭 쌓듯이 작은 단위로 쪼개서 조합하는 걸 의미합니다.

이름에서부터 명령형 프로그래밍은 이렇게 해! 하고 **어떻게(how) 할지**를 설명한다면,
함수형 프로그래밍은 함수가 인자를 받고 결과를 돌려주듯이 어떤 결과를 주기 위해 **무엇(what)을 할지**를 설명합니다.
```javascript
// 명령형: 어떻게 반복할지 알려줌
let result = [];
for (let n of numbers) {
  result.push(n * 2);
}

// 선언형: 무엇을 할지 말함
numbers.map(n => n * 2);
```

#### 2. 엔티티(Entity)란?
폴더 구조나 파일명에서 자주 사용되는 엔티티(Entity)라는 용어가 있는데 정확히 무슨 뜻일까요?

**엔티티(Entity)**는 도메인(비즈니스 로직)의 핵심이 되는 **실제 데이터 객체**를 의미하며,
쉽게 말해 앱이 다루는 “진짜 대상”을 말합니다.

이 엔티티를 기준으로 Component, hook, function을 나눌 수 있습니다.
- 엔티티를 다루는 상태와 그렇지 않은 상태 - cart, isCartFull vs isShowPopup
- 엔티티를 다루는 컴포넌트와 훅 - CartItemView, useCart(), useProduct()
- 엔티티를 다루지 않는 컴포넌트와 훅 - Button, useRoute, useEvent 등
- 엔티티를 다루는 함수와 그렇지 않은 함수 - calculateCartTotal(cart) vs capaitalize(str)
 
왜 Entity를 기준으로 분리를 해야 하는걸까요?
지금 과제를 기준으로 생각해봅시다.

과제는 쇼핑몰 프로젝트이고 해당 프로젝트에서 다뤄지는 개체(data)에는 크게 상품, 쿠폰, 장바구니가 있습니다.
여기서 생각할 수 있는 구조로는 상품 카드, 쿠폰 카드, 장바구니 아이템 카드가 대표적으로 있겠죠.

만약 극단적으로 이 아이템 카드를 하나의 컴포넌트로 렌더하게끔 한다면
한 컴포넌트 내에서 분기를 추가하면서 렌더하게끔 할 것입니다.

그럼 문제 없는 거 아닌가요?🤔
몇개월 뒤 아이템 카드에 새로운 개체인 게시글이 추가되어야 한다고 할 때 컴포넌트 내에 분기를 추가해야 하고
이는 곧 다른 분기에도 영향을 줄 수 있는 **잠재적 위험**이 됩니다.

이를 방지하기 위해서도 우리는 각 개체별로 **다른 관심사를 격리**시키고 **독립적으로 확장**해나갈 수 있도록 보장해줘야 합니다.

#### 3. Jotai
상태 관리 라이브러리로는 Zustand를 사용해봤는데 Jotai는 처음 접해봤습니다. (Jotai 조타..)
Jotai는 React와 유사한 문법 체계를 가지고있어 React에 특화된 상태 관리 라이브러리입니다.

얼마나 비슷하길래..?하고 보면 진짜 거의 그대로 사용하는 급이네요
```javascript
// 1. React의 useState를 사용한 경우
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(c => c + 1)}>{count}</button>;
}
// 2. Jotai의 useAtom을 사용한 경우
import { atom, useAtom } from 'jotai';

// 컴포넌트 밖에 atom(상태의 단위)을 정의합니다
const countAtom = atom(0);

function Counter() {
  // useState와 사용법이 완전히 동일합니다!
  const [count, setCount] = useAtom(countAtom);
  return <button onClick={() => setCount(c => c + 1)}>{count}</button>;
}
```
이만큼 유사한 문법 체계를 가지고 있기 때문에 기존 React 프로젝트에서 전역 상태관리를 도입해야 한다면,
러닝커브도 적고 전환 시간을 최소화할 수 있을 것 같습니다.

---

### 이번 과제에서 내가 제일 신경 쓴 부분은 무엇인가요?
#### ✅ 과제 진행 과정
**(1) 개념 정립**
해당 챕터에서 다루는 디자인 패턴과 함수형 프로그래밍에 대해 이해하고 정리하는 시간을 가졌습니다.

**(2) 구조의 시각화**
그리고 프로젝트 구조를 시각화하기로 결심했습니다.

멘토링에서 구조를 시각화하는 연습을 해보라고 하셨던 조언과 더불어 이번 과제의 핵심은 **원래 구조의 문제점을 파악**하고 이를 개선하기 위해 **어떤 패턴**을 적용할지, **어떻게 리팩토링할지를 사고**하는 것이라고 생각했기 때문입니다.

**(2-1) 텍스트로 기능 정리**
처음부터 코드를 보면서 모든 기능을 정리하려 하면 복잡할 것 같아서 빌드한 페이지에 직접 액션을 해보며 기능설계서를 역으로 작성했고
혹시 누락하거나 잘못 작성한 부분이 있을까봐 AI에게 검증을 맡겼습니다.
```
# 쇼핑몰
- 쇼핑몰에 존재하는 상품 목록이 노출된다.
    - 재고가 5개 이하면 품절 임박 텍스트가 노출된다.
    - 할인이 있을 경우 해당 할인 정보를 노출한다.
    - 재고 소진 시 품절 버튼으로 변경 & 가격 영역이 SOLD OUT으로 변경된다.
- 검색 input 입력 → 해당 input값을 포함하는 상품들을 반환한다.
    - 검색어 입력 후 500ms 지연 후 검색이 실행된다. (debounce)
    - 상품명과 상품 설명 모두에서 검색한다.
    - 포함하는 상품이 없으면 empty UI를 반환한다.
    - 빈 값이면 전체 상품 리스트를 반환한다.
- 장바구니 담기 버튼 클릭 → 장바구니에 해당 상품이 담긴다.
    - 성공 토스트 팝업이 뜬다. (3초 후 자동으로 사라진다)
    - 우측에 해당 상품이 적용된다.
    - 헤더의 장바구니 아이콘 옆에 총 아이템 개수가 배지로 표시된다.
- 수량 input 클릭 → 장바구니에 담을 상품 수가 1씩 증감한다.
    - 지정 개수 이상이면 지정한 할인율이 적용된다.
    - 장바구니에 10개 이상인 상품이 있으면 추가 5% 할인이 적용된다. (최대 50%까지)
    - 재고 이상으로 증가할 경우, 수량 에러 토스트 팝업이 뜬다.
    - 1에서 0으로 감소할 경우, 장바구니에 해당 상품이 제거된다.
    - 재고 소진 시 품절 버튼으로 변경 & 가격 영역이 SOLD OUT으로 변경된다.
    - 각 장바구니 아이템에 적용된 할인율이 "-X%" 형태로 표시된다.
- 쿠폰 select 클릭 → 선택한 값으로 쿠폰 할인이 적용된다.
    - 성공 토스트 팝업이 뜬다. (3초 후 자동으로 사라진다)
    - 적용 불가한 쿠폰일 경우, 에러 토스트 팝업이 뜬다. (percentage 쿠폰은 10,000원 이상 구매 시 사용 가능)
- 결제 버튼 클릭 → 결제 성공 토스트 팝업이 뜬다.
    - 장바구니에 해당 상품이 제거된다.
    - 선택된 쿠폰이 해제된다.
- 장바구니 X 버튼 클릭 → 장바구니에 해당 상품이 제거된다.
- 관리자 페이지로 버튼 클릭 → 관리자 페이지로 이동한다.
- 데이터 저장/복원
    - 상품, 쿠폰, 장바구니 데이터가 localStorage에 자동으로 저장된다.
    - 페이지 새로고침 시 저장된 데이터가 자동으로 복원된다.

# 관리자
- 새 상품 추가 버튼 클릭 → 새 상품 추가 form이 노출된다.
    - 상품명을 입력하지 않고 추가 버튼 클릭 → 브라우저 기본 validation 에러가 노출된다.
    - 가격을 입력하지 않고 추가 버튼 클릭 → 브라우저 기본 validation 에러가 노출된다.
        - 문자열 입력 → 아무일도 일어나지 않는다. (숫자만 입력 가능)
    - 재고를 입력하지 않고 추가 버튼 클릭 → 브라우저 기본 validation 에러가 노출된다.
        - 문자열 입력 → 아무일도 일어나지 않는다. (숫자만 입력 가능)
    - 취소 버튼 클릭 → 새 상품 추가 form이 미노출된다.
    - 할인 추가 버튼 클릭 → 할인 추가 form이 노출된다. (기본: 10개 | 10%)
        - 할인 추가 버튼 클릭 → 할인 추가 form이 추가된다.
        - 할인 수량 input은 type="number"이며 min="1" 속성이 있어 0 이하 입력 시 브라우저 기본 validation이 작동한다.
        - 할인율 input은 type="number"이며 max="100" 속성이 있어 100 이상 입력 시 브라우저 기본 validation이 작동한다.
        - 각 할인 항목의 X 버튼 클릭 → 해당 할인 항목이 삭제된다.
    - 필수값(상품명, 가격, 재고)을 입력하고 추가 버튼 클릭 → 상품 목록에 추가된다.
        - 새 상품 추가 form이 미노출된다.
        - 상품 추가 성공 토스트 팝업이 뜬다. (3초 후 자동으로 사라진다)
- 삭제 버튼 클릭 → 해당 상품이 삭제된다.
    - 삭제 성공 토스트 팝업이 뜬다. (3초 후 자동으로 사라진다)
- 수정 버튼 클릭 → 상품 수정 form이 노출된다.
    - 수정 버튼 클릭 → 수정 성공 토스트 팝업이 뜬다. (3초 후 자동으로 사라진다)
    - 상품명을 입력하지 않고 수정 버튼 클릭 → 브라우저 기본 validation 에러가 노출된다.
    - 가격을 입력하지 않고 수정 버튼 클릭 → 브라우저 기본 validation 에러가 노출된다.
        - 문자열 입력 → 아무일도 일어나지 않는다. (숫자만 입력 가능)
    - 재고를 입력하지 않고 수정 버튼 클릭 → 브라우저 기본 validation 에러가 노출된다.
        - 문자열 입력 → 아무일도 일어나지 않는다. (숫자만 입력 가능)
    - 취소 버튼 클릭 → 상품 수정 form이 미노출된다.
    - 할인 추가 버튼 클릭 → 할인 추가 form이 노출된다. (기본: 10개 | 10%)
        - 할인 추가 버튼 클릭 → 할인 추가 form이 추가된다.
        - 할인 수량 input은 type="number"이며 min="1" 속성이 있어 0 이하 입력 시 브라우저 기본 validation이 작동한다.
        - 할인율 input은 type="number"이며 max="100" 속성이 있어 100 이상 입력 시 브라우저 기본 validation이 작동한다.
        - 각 할인 항목의 X 버튼 클릭 → 해당 할인 항목이 삭제된다.
    - 필수값(상품명, 가격, 재고)을 입력하고 수정 버튼 클릭 → 상품이 수정된다.
        - 상품 수정 form이 미노출된다.
        - 상품 수정 성공 토스트 팝업이 뜬다. (3초 후 자동으로 사라진다)
- 쿠폰 관리 탭 클릭 → 쿠폰 관리 콘텐츠가 노출된다.
    - 삭제 버튼 클릭 → 해당 쿠폰이 삭제된다.
        - 삭제 성공 토스트 팝업이 뜬다. (3초 후 자동으로 사라진다)
        - 삭제한 쿠폰이 현재 선택된 쿠폰이면 자동으로 해제된다.
    - 새 쿠폰 추가 버튼 클릭 → 새 쿠폰 생성 form이 노출된다.
        - 취소 버튼 클릭 → 새 쿠폰 생성 form이 미노출된다.
        - 쿠폰명을 입력하지 않고 쿠폰 생성 버튼 클릭 → 브라우저 기본 validation 에러가 노출된다.
        - 쿠폰코드를 입력하지 않고 쿠폰 생성 버튼 클릭 → 브라우저 기본 validation 에러가 노출된다.
            - 쿠폰 코드 입력 시 자동으로 대문자로 변환된다.
        - 할인금액을 입력하지 않고 쿠폰 생성 버튼 클릭 → 브라우저 기본 validation 에러가 노출된다.
        - 할인 타입 select 클릭 → 선택한 값으로 쿠폰 할인 타입이 변경된다.
        - 이미 존재하는 쿠폰 코드를 입력하고 쿠폰 생성 버튼 클릭 → 에러 토스트 팝업이 뜬다.
        - 필수값(쿠폰명, 쿠폰 코드, 할인 금액)을 입력하고 쿠폰 생성 버튼 클릭 → 쿠폰 목록에 추가된다.
            - 새 쿠폰 생성 form이 미노출된다.
            - 쿠폰 추가 성공 토스트 팝업이 뜬다. (3초 후 자동으로 사라진다)
- 쇼핑몰로 돌아가기 버튼 클릭 → 쇼핑몰 페이지로 돌아간다.
- 데이터 저장/복원
    - 상품, 쿠폰 데이터가 localStorage에 자동으로 저장된다.
    - 페이지 새로고침 시 저장된 데이터가 자동으로 복원된다.
```

**(2-2) Action / Calculation / Data로 재분류**
위에서 텍스트로 정리한 내용을 FP 관점에서 Action / Calculation / Data로 재분류했습니다.

**1️⃣ Action (부수효과)**

**[쇼핑몰]**
- 검색 input 입력
- 장바구니 담기 버튼 클릭
- 수량 input 클릭
- 쿠폰 select 클릭
- 결제 버튼 클릭
- 장바구니 X 버튼 클릭
- 관리자 페이지로 버튼 클릭
- 토스트 알림 표시/제거
- localStorage 자동 저장
- 검색 debounce 처리

**[관리자]**
- 새 상품 추가 버튼 클릭
- 취소 버튼 클릭
- 상품명 입력
- 가격 입력
- 재고 입력
- 할인 추가 버튼 클릭
- 할인 항목 X 버튼 클릭
- 추가 버튼 클릭
- 삭제 버튼 클릭
- 수정 버튼 클릭
- 쿠폰 관리 탭 클릭
- 삭제 버튼 클릭
- 새 쿠폰 추가 버튼 클릭
- 취소 버튼 클릭
- 쿠폰명 입력
- 쿠폰코드 입력
- 할인금액 입력
- 할인 타입 select 클릭
- 쿠폰 생성 버튼 클릭
- 쇼핑몰로 돌아가기 버튼 클릭
 
**2️⃣ Calculation (순수 로직)**
- 필터링
- 할인 계산
- 수량 계산
- validation 판단
- 쿠폰 코드 대문자 변환
- 재고 계산
- 가격 포맷팅
- 장바구니 총 개수 계산
- 쿠폰 적용 가능 여부 판단
- 중복 쿠폰 코드 검증
- 주문번호 생성
 
**3️⃣ Data (정적/상태 데이터)**
- 상품 리스트
- 장바구니 리스트
- 쿠폰 리스트
- 신규 쿠폰 form 데이터
- 할인 정책
- 신규 상품 form 데이터
- 상품 수정 form 데이터
- 검색어 (searchTerm, debouncedSearchTerm)
- 선택된 쿠폰 (selectedCoupon)
- 관리자 모드 상태 (isAdmin)
- 알림 리스트 (notifications)
- 활성 탭 (activeTab)
- 폼 표시 상태 (showProductForm, showCouponForm)
- 수정 중인 상품 ID (editingProduct)
- 장바구니 총 개수 (totalItemCount)

**(2-3) 피그잼으로 시각화하기**
이런 걸 처음 해보다보니 초반에는 가이드를 잡고 어떤 걸 어디로 분류하고 화살표는 어디까지 연결해야 할지에 대한 기준이 잡히지 않아 시간이 오래 걸렸습니다.

그러나 Q&A 때 테오가 화살표로 연결하는 건 함수를 호출하는 것을 의미하며, 이는 곧 a=>b라고 할 때 **a는 b를 알고 있지만 b는 a를 모른다**는 단방향 흐름을 의미한다고 하여 이해가 갔습니다.

그 이후로 개별 플로우는 시각화를 완성할 수 있었습니다.
[진행한 피그잼 링크](https://www.figma.com/board/DA6BbRA4PJ5ReQ1OI9Sk4D/7%EC%A3%BC%EC%B0%A8_%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8_%EB%8F%84%EC%8B%9D%ED%99%94?node-id=0-1&p=f&t=fMsjW2CT3zXIqSVI-0)
<img width="614" height="853" alt="image" src="https://github.com/user-attachments/assets/5a374a5f-b1c9-4131-9d97-b620382adbce" />

**(3) 본격적인 구조 개선**
**(3-1) 고민**
**1️⃣ 아이콘 정의**
**❗ 둘 다 삭제하는 아이콘인데, 역할을 어떻게 구분지어야 할까?**
<img width="708" height="192" alt="image" src="https://github.com/user-attachments/assets/314efe44-1d11-4f72-ba3b-79bdaa837b1c" />
<img width="762" height="296" alt="image" src="https://github.com/user-attachments/assets/0f157750-d668-44f3-864e-4bcea45b90a8" />

X 아이콘과 쓰레기통 모양 둘 다 무언가를 삭제할 때 사용하는 아이콘입니다.

네이밍을 할 때 시각적인 걸 기준으로 IconX, IconTrash라는 등의 이름으로 하게 되면 추후에 만약 아이콘이 변경되었을 때 이름 수정이나 아이콘 추가 작업이 필요하게 되고 이는 곧 불필요한 추가 작업을 초래하게 됩니다.

그래서 두개의 **역할을 명확하게** 정해두기로 했습니다.
- X 아이콘: 일반적으로 폼이나 모달 형태에서 닫기/지우기(on/off 가능)의 역할을 하는 아이콘 => IconClose
- 쓰레기통 아이콘:  영구적으로 데이터를 삭제하는 역할을 하는 아이콘 => IconDelete
그렇게 하니 네이밍을 쉽게 진행할 수 있었습니다!

**❗ 둘 다 장바구니 모양인데, 역할을 어떻게 구분지어야 할까?**
<img width="378" height="108" alt="image" src="https://github.com/user-attachments/assets/ea8a951d-21a2-4385-a9dd-ee3db5fd4ea8" />
<img width="220" height="90" alt="image" src="https://github.com/user-attachments/assets/4e76b081-fd14-48ec-aa31-78a52cd9e008" />

첫번째와 유사한 고민으로, 둘 다 장바구니 모양인데 어떻게 네이밍할지 고민됐습니다.
그래도 앞선 경험을 겪고나니 이번에도 역할 기반으로 구분해봤습니다.

- 카트 아이콘: 장바구니 개수를 보여주거나 클릭 시 액션이 존재할만한 영역(현재는 해당 기능까지는 미개발 상태) => IconCartButton
- 쇼핑백 아이콘: 단순한 장식 요소 => IconCartSymbol

**2️⃣ 동일한 내용의 토스트 팝업인데 중복 노출을 허용해도 되는가?**
<img width="558" height="1094" alt="image" src="https://github.com/user-attachments/assets/7fe3495f-ca62-481a-b369-d61bc8e797ec" />
동일한 내용의 토스트 팝업이 아직 화면에서 사라지지 않았는데 호출하는만큼 노출되는 게 에러처럼 보인다고 생각했습니다.
그래서 동일한 내용이라면 해당 토스트 팝업이 **사라지기 전까지는 중복 호출을 막는** 것으로 개선했습니다.

**3️⃣ 상품목록이 비어있으면 table 부분부터 안 나와야 하는 게 아닌가?**
<img width="1280" height="271" alt="image" src="https://github.com/user-attachments/assets/daac272e-d54b-4b78-8ac0-a08da0bc51f8" />
기존에는 상품 목록이 비어있어도 table header 부분까지는 노출되고 body 부분만 미노출되도록 작업되어있었지만,
개인적으로 이건 **적합하지 않은 UI**라고 판단되어 table 자체가 미노출되도록 수정했습니다.

**4️⃣ isRecommended는 어떻게 확인하나?**
<img width="1280" height="372" alt="image" src="https://github.com/user-attachments/assets/d2fa34fc-bdec-4a55-8b8e-0a017f719c85" />
상품 구조에서는 isRecommended를 기준으로 BEST 뱃지 노출 처리를 하고 있었는데
form에서는 도저히 isRecommended를 제어하는 영역을 찾을 수 없었고 이상한 부분이라고 생각했습니다.

일반적으로 BEST 뱃지는 **관리자 재량**으로 노출시키기 때문에 **form에서 isRecommended를 적용**할 수 있도록 개선했습니다.

**5️⃣ 의미없는 분기**
불필요한 분기는 삭제했습니다!
```
{(activeTab === "products" ? products : products).map((product) =>
 ```

**(3-2) 디자인 패턴 적용**
**1️⃣ 생성 패턴(Creational Patterns)**
**❗Factory Method(팩토리 메서드 패턴)**
쿠폰 생성 시 복잡한 검증 로직을 캡슐화하여 일관성을 보장합니다.
```
// models/coupon.ts
export interface AddCouponResult {
  coupons: Coupon[];
  success: boolean;
  errorMessage?: string;
}
export const addCoupon = (
  coupons: Coupon[],
  newCoupon: Coupon
): AddCouponResult => {
  // 1. 검증
  const validation = validateCouponForm(newCoupon);
  if (!validation.isValid) {
    return {
      coupons,
      success: false,
      errorMessage: validation.errorMessage,
    };
  }
  // 2. 중복 체크
  if (isCouponCodeDuplicate(coupons, newCoupon.code)) {
    return {
      coupons,
      success: false,
      errorMessage: "이미 존재하는 쿠폰 코드입니다.",
    };
  }
  // 3. 쿠폰 생성 및 반환
  return {
    coupons: [...coupons, newCoupon],
    success: true,
  };
};
// AdminCouponForm.tsx
const result = addCoupon(coupons, couponForm);
if (!result.success) {
  handleNotificationAdd(result.errorMessage || "", "error");
  return;
}
setCoupons(result.coupons);
 ```

**2️⃣ 구조 패턴(Structural Patterns)**
**❗Facade(파사드 패턴)**
```
// models/cart.ts
export const calculateCartTotal = (
  cart: CartItem[],
  selectedCoupon: Coupon | null
): {
  totalBeforeDiscount: number;
  totalAfterDiscount: number;
} => {
  // 내부적으로 여러 복잡한 계산을 수행
  const totalBeforeDiscount = calculateCartOriginalTotal(cart);
  const totalAfterItemDiscount = calculateTotalBeforeCoupon(cart);
  const totalAfterDiscount = applyCouponDiscount(
    totalAfterItemDiscount,
    selectedCoupon
  );
  return {
    totalBeforeDiscount,
    totalAfterDiscount,
  };
};
 ```
복잡한 할인 계산 로직을 분리함으로써 컴포넌트에서는 calculateCartTotal(cart, coupon)만 호출하면 됩니다.
```
// ShoppingCouponPayment.tsx
const totals = calculateCartTotal(cart, selectedCoupon);
 ```

**❗Composite (컴포지트 패턴)**
작은 순수 함수들을 조합하여 복잡한 로직을 구현함으로써
각 함수의 독립적인 테스트가 가능하고 함수 재사용성이 향상됩니다.
```
// calculateItemTotal은 여러 함수를 조합
export const calculateItemTotal = (
  cart: CartItem[],
  item: CartItem
): number => {
  // 할인율 계산
  const discountRate = getMaxApplicableDiscount(cart, item);  
  // 할인 가격 계산
  return calculateDiscountedPrice(                            
    item.product.price,
    item.quantity,
    discountRate
  );
};
// getMaxApplicableDiscount도 여러 함수 조합
export const getMaxApplicableDiscount = (
  cart: CartItem[],
  item: CartItem
): number => {
// 기본 할인
  const baseDiscount = getMaxDiscountRate(                    
    item.product.discounts,
    item.quantity
  );
  // 대량 구매 할인
  if (hasBulkPurchase(cart)) {                                
    return Math.min(baseDiscount + 0.05, 0.5);
  }
  return baseDiscount;
};
 ```

**3️⃣ 행동 패턴(Behavioral Patterns)**
**❗Strategy(전략 패턴)**
할인 타입(amount/percentage)에 따라 다른 계산 방식을 적용함으로써 새로운 할인 타입 추가 시 확장이 용이합니다.
```
// models/coupon.ts
export const applyCouponDiscount = (
  totalBeforeCoupon: number,
  coupon: Coupon | null
): number => {
  if (!coupon) {
    return totalBeforeCoupon;
  }
  // Strategy: 할인 타입에 따라 다른 알고리즘 적용
  if (coupon.discountType === "amount") {
    // 정액 할인 전략
    return Math.max(0, totalBeforeCoupon - coupon.discountValue);
  } else {
    // 정률 할인 전략
    return Math.round(
      totalBeforeCoupon * (1 - coupon.discountValue / 100)
    );
  }
};
 ```

**❗Template Method(템플릿 메서드 패턴) - 부분적 적용**
각 단계를 명확히 분리함으로써 검증 → 체크 → 생성의 순서가 항상 유지되도록 합니다.
```
// coupon.ts
export const addCoupon = (
  coupons: Coupon[],
  newCoupon: Coupon
): AddCouponResult => {
  // 템플릿: 1. 검증 → 2. 중복 체크 → 3. 생성
  const validation = validateCouponForm(newCoupon);  // 1단계
  if (!validation.isValid) {
    return { coupons, success: false, errorMessage: validation.errorMessage };
  }
  if (isCouponCodeDuplicate(coupons, newCoupon.code)) {  // 2단계
    return { coupons, success: false, errorMessage: "이미 존재하는 쿠폰 코드입니다." };
  }
  return { coupons: [...coupons, newCoupon], success: true };  // 3단계
};
 ```

**4️⃣ 추가 설계 원칙 및 패턴**
GoF 패턴 외에도 적용된 중요한 설계 원칙들을 적용했습니다.

**❗ Pure Function Pattern(순수 함수 패턴)**
동일한 입력에 대해 항상 동일한 출력을 보장하고 디버깅이 용이하다.
```
// discount.ts
export const getMaxDiscountRate = (
  discounts: Discount[],
  quantity: number
): number => {
  return discounts.reduce((maxDiscount, discount) => {
    return quantity >= discount.quantity && discount.rate > maxDiscount
      ? discount.rate
      : maxDiscount;
  }, 0);
};
```

**❗ Result Pattern(결과 패턴)**
예외 대신 Result 타입을 반환하여 에러를 명시적으로 처리함으로써 타입 안전성이 향상되고 에러 처리를 강제할 수 있습니다.
```
// cart.ts
export type AddToCartError = "OUT_OF_STOCK" | "EXCEEDS_STOCK" | null;
export interface AddToCartResult {
  cart: CartItem[];
  error: AddToCartError;
}
export const addProductToCart = (
  cart: CartItem[],
  product: ProductWithUI
): AddToCartResult => {
  const remainingStock = getRemainingStock(product, cart);
  if (remainingStock <= 0) {
    return { cart, error: "OUT_OF_STOCK" };
  }
  // ...
  return { cart: nextCart, error: null };
};
```

**❗ Immutability Pattern (불변성 패턴)**
기존 데이터를 변경하지 않고 새로운 데이터를 생성함으로써 React의 불변성 요구사항을 충족합니다.
```
// cart.ts
export const removeProductFromCart = (
  cart: CartItem[],
  productId: string
): CartItem[] => {
  return cart.filter((item) => item.product.id !== productId); // 새 배열 반환
};
export const updateCartItemQuantity = (
  cart: CartItem[],
  productId: string,
  newQuantity: number
): CartItem[] => {
  return cart.map((item) =>
    item.product.id === productId
      ? { ...item, quantity: newQuantity }  // 새 객체 생성
      : item
  );
};
```

**❗Single Responsibility Principle(단일 책임 원칙)**
엔티티별 모듈을 분리함으로써 모듈 간 결합도는 감소시키고 변경 영향 범위를 최소화합니다.
```
models/
  ├── cart.ts      → 장바구니 관련 로직만
  ├── coupon.ts    → 쿠폰 관련 로직만
  ├── discount.ts  → 할인 관련 로직만
  └── product.ts   → 상품 관련 로직만
```

**❗ Separation of Concerns (관심사의 분리)**
컴포넌트와 비즈니스 로직을 분리함으로써 재사용성을 향상시키고 유지보수가 용이하도록 합니다.
```
Before (컴포넌트에 비즈니스 로직 포함)
// ShoppingCartItem.tsx (이전)
const calculateItemTotal = (item: CartItem): number => {
  const { price } = item.product;
  const { quantity } = item;
  const discount = getMaxApplicableDiscount(item, cart);
  return Math.round(price * quantity * (1 - discount));
};

After (비즈니스 로직을 모델로 분리)
// models/cart.ts
export const calculateItemTotal = (cart, item) => { /* ... */ };
// ShoppingCartItem.tsx
const itemTotal = calculateItemTotal(cart, item);
 ```

**(4) Jotai 적용**
구조 개선을 진행하며 props drilling이 거듭될 때마다 컴포넌트 분리를 덜 해야 하나 싶어지기까지 했습니다.
```
src/basic/
├── App.tsx (L0) ⚠️ Props 생성
│   ├── cart: CartItem[]
│   ├── products: ProductWithUI[]
│   ├── setCart: Dispatch<...>
│   └── addNotification: (message, type) => void
│
└── pages/
    └── PageShopping.tsx (L1) ⚠️ Props 전달만 (사용 안 함)
        ├── cart: CartItem[] ⬇️
        ├── products: ProductWithUI[] ⬇️
        ├── setCart: Dispatch<...> ⬇️
        └── handleNotificationAdd: (message, type) => void ⬇️
        │
        └── shopping/
            └── ShoppingList.tsx (L2) ⚠️ Props 전달만 (사용 안 함)
                ├── cart: CartItem[] ⬇️
                ├── products: ProductWithUI[] ⬇️
                ├── setCart: Dispatch<...> ⬇️
                └── handleNotificationAdd: (message, type) => void ⬇️
                │
                └── product/
                    └── ShoppingProductItem.tsx (L3) ⚠️ Props 전달만 (사용 안 함)
                        ├── cart: CartItem[] ⬇️
                        ├── products: ProductWithUI[] ⬇️
                        ├── setCart: Dispatch<...> ⬇️
                        └── handleNotificationAdd: (message, type) => void ⬇️
                        │
                        └── components/entity/product/
                            └── ProductItem.tsx (L4) ✅ 최종 사용처
                                ├── cart: CartItem[] ✅ 실제 사용
                                ├── products: ProductWithUI[] ✅ 실제 사용
                                ├── setCart: Dispatch<...> ✅ 실제 사용
                                └── handleNotificationAdd: (message, type) => void ✅ 실제 사용
```
하지만 그게 과제의 목표이니 아랑곳하지 않고 props drilling을 진행했고 드디어 개선할 때입니다!!!!

적용하는 과정은 그리 어렵지 않았습니다.
useState로 정의했던 내용들은 다 Jotai를 활용하도록 변경하면 로컬 페이지에서는 문제없이 동작했습니다.

**(4-1) 돌아온 테스트와 로컬의 차이**
그러나 테스트에서는 "장바구니 담기"버튼을 찾지 못해서 테스트가 12개정도 실패했습니다..(돌아온 테스트와 로컬의 차이)
```
fireEvent.click(screen.getAllByText('장바구니 담기')[0]);
 ```

스냅샷을 확인해보니 테스트의 알림이 많이 생성되어 페이지에 남아있었고 그로 인해 버튼을 찾지 못하는 것처럼 보였습니다.

즉, 각 테스트의 상태가 격리되어야 하는 것이고 매 테스트마다 시작 부분에 App을 렌더하고 있으므로 각 App이 독립된 상태를 가지도록 하면 되지 않을까 싶었습니다.
```
test('상품을 검색하고 장바구니에 추가할 수 있다', async () => {
  render(<App />);
 ```

이를 위해 Provider로 App을 감싸서 App 렌더링 시 매번 새로운 Provider가 생성되도록 했고 이를 통해 이전 테스트의 상태가 영향을 주지 않게끔 할 수 있습니다!

---

### 이번 과제를 통해 앞으로 해보고 싶은게 있다면 알려주세요!
이번 과제를 진행하면서 느낀 점은 **레거시 코드를 분석하고 개선**하는 작업은 차라리 명확한 기준이 있어서 접근하기 쉬웠지만,
**처음부터 코드를 작성**할 때 함수형 프로그래밍이나 디자인 패턴을 **자연스럽게 고려하는 것은 훨씬 어렵다**는 것이었습니다.

앞으로는 기능 구현을 바로 시작하는 대신
- 기능 플로우를 설계하고
- Action / Calculation / Data 를 정리하고
- 엔티티 단위로 구조를 먼저 잡은 뒤
- 그 후에 실제 코드를 작성하는 방식
으로 **처음부터 구조의 흐름을 설계**하는 습관을 만들고 싶습니다.

이번 과제를 통해 그 중요성을 체감할 수 있었고, 다음 프로젝트나 기능 개발에 적용해보고 싶습니다!

### 리뷰 받고 싶은 내용이나 궁금한 것에 대한 질문 편하게 남겨주세요 :)
- 처음부터 기능과 구조를 설계할 때 어떤 기준이나 접근 방식으로 시작하면 좋을까요?
- 구조 설계 단계에서 피그잼이나 노션에 어떤 내용을 정리하면 실전에서 가장 효과적인지 노하우가 있을까요?
- 패턴과 함수형 사고를 자연스럽게 적용할 수 있도록 훈련하는 방법이 궁금합니다!

---

자세한 구현 과정과 회고는 아래 블로그에 정리했습니다! 😊
[7주차_디자인 패턴과 함수형 프로그래밍 그리고 상태 관리 설계](https://chaeng03.tistory.com/entry/%ED%95%AD%ED%95%B499-7%EC%A3%BC%EC%B0%A8%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4%EA%B3%BC-%ED%95%A8%EC%88%98%ED%98%95-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-%EA%B7%B8%EB%A6%AC%EA%B3%A0-%EC%83%81%ED%83%9C-%EA%B4%80%EB%A6%AC-%EC%84%A4%EA%B3%84)
[WIL 7주차_Chapter3-2. 디자인 패턴과 함수형 프로그래밍 그리고 상태 관리 설계](https://chaeng03.tistory.com/m/entry/%ED%95%AD%ED%95%B499-WIL-7%EC%A3%BC%EC%B0%A8Chapter3-2-%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4%EA%B3%BC-%ED%95%A8%EC%88%98%ED%98%95-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-%EA%B7%B8%EB%A6%AC%EA%B3%A0-%EC%83%81%ED%83%9C-%EA%B4%80%EB%A6%AC-%EC%84%A4%EA%B3%84)
