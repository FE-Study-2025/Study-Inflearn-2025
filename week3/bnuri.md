p1 3-4

# setup과 teardown

setup: 테스트를 실행하기 전 수행해야 하는 작업
teardown: 테스트를 실행한 뒤 수행해야 하는 작업

같은 스코프 내에서 beforeAll -> beforeEach 순으로 실행 됨

보통 afterEach에서 초기화 // 모킹한 모듈의 히스토리를 초기화 하여 

afterEach -> afterAll 순으로 실행 

- setup과 teardown을 통해 테스트 실행 전, 후 실행되어야 할 반복 작업을 깔끔하게 관리할 수 있다.
- 테스트 별로 별도의 스코프로 동작하기 때문에 독립적인 테스트를 구성하는데 도움을 받을 수 있다.
- setup, teardown에서 전역 변수를 사용한 조건 처리는 독립성을 보장하지 못하고, 신뢰성이 낮아지므로 지양하자!

### React Testing Library
- UI 컴포넌트를 사용자가 사용하는 방식으로 테스트
- DOM과 이벤트 인터페이스를 기반으로 요소를 조회하고, 다양한 동작을 시뮬레이션 가능
    - https://testing-library.com/docs/queries/about
- 인터페이스 기반의 직관적인 코드와 내부 구현에 종속되지 않는 견고한 테스트 코드

### Spy 함수
- 함수의 호출 여부, 인자, 반환 값 등 함수 호출에 관련된 다양한 값 저장
- 콜백 함수나 이벤트 핸들러가 올바르게 호출 되었는지 검증 가능
