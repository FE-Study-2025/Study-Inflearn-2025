p1 5-3
# 통합 테스트

- 단위테스트는 여러 모듈이 조합되었을 때의 동작은 검증할 수 없음

## 통합테스트 항목
- 특정 상태를 기준으로 동작하는 컴포넌트 조합
- API와 함께 상호작용 하는 컴포넌트 조합


## 통합 테스트 장점
- 여러 개의 모듈이 동시에 상호 작용하는 것을 테스트 하기 때문에 단위 테스트에 비해 모킹의 비중이 적으며, 모듈 간 발생하는 에러 검증 가능
- 실제 앱이 동작하는 비즈니스 로직에 가깝게 기능 검증 가능
- 하위 모듈의 단위 테스트에서 검증할 수 있는 부분까지 한 번에 효율적으로 검증 가능

-> 앱의 상태를 어디서 어떻게 관리하고 변경할지 구조적인 설계가 중요
-> 좋은 설계를 위한 매개체가 된다!

# 통합 테스트 대상 선정하기
- 구성된 비즈니스 로직을 적절한 단위로 나누어 컴포넌트 집합 검증해야..
- 비즈니스 로직을 기준으로 통합 테스트로 나눌 때 고려할 점
    - 가능한 모킹을 하지 않고 최대한 앱의 실제 기능과 유사하게 검증 하기
    - 비즈니스 로직을 처리하는 상태 관리나 API 로직은 상위 컴포넌트로 응집해 관리하기
    - 변경 가능성을 고려해 여러 도메인 기능이 조합된 비즈니스 로직은 나누어 통합 테스트 작성
- ex. 메인 페이지는 네이게이션 바, 상품 검색, 상품 리승트 영역으로 나누어 테스트 작성
- ex. 장바구니 페이지는 상품 리스트, 가격 계산 영역으로 나누어 테스트 작성
- 컴포넌트간 결합도를 낮추고 견고한 설계를 통해 유지 보수하기 좋은 코드 작성 가능

# 상태 관리 모킹하기

앱의 전역 상태를 모킹해 테스트 전 후에 값을 변경하고 초기화해야 한다

```js
// __mocks__/zustand.js 자동 모킹 적용, 스토어 초기화
// __mocks__ 하위에 위치한 파일 -> vitest나 jest에서 특정 모듈을 자동 모킹

const { create: actualCreate } = await vi.importActual('zustand');
import { act } from '@testing-library/react';

// 앱에 선언된 모든 스토어에 대해 재설정 함수를 저장
const storeResetFns = new Set();

// 스토어를 생성할 때 초기 상태를 가져와 리셋 함수를 생성하고 set에 추가합니다.
export const create = createState => {
  const store = actualCreate(createState);
  const initialState = store.getState();
  storeResetFns.add(() => store.setState(initialState, true));
  return store;
};

// 테스트가 구동되기 전 모든 스토어를 리셋합니다.
// 테스트의 독립성 유지
beforeEach(() => {
  act(() => storeResetFns.forEach(resetFn => resetFn()));
});

```

    