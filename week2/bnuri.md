p1 3-2

# 단위 테스트
단일 함수의 결괏값 또는 단일 컴포넌트(클래스)의 상태(UI)나 행위를 검증

### 공통 컴포넌트는 단위 테스트에 적합

### Arrange-Act-Assert(AAA) 패턴
- Arrange: 테스트를 위한 환경 만들기(컴포넌트 렌더링)
- Act: 테스트할 동작 시뮬레이션(클릭, 타이핑, 메서드 호출)
- Assert: 기대 결과대로 실행되었는지 검증

### 단위 테스트 코드 작성 시 주의사항
- 상세한 테스크 디스크립션 통해 가독성 향상
- 내부 DOM 구조나 로직에 영향을 받지 않게 테스팅 라이브러리 API를 통해 적절한 요소 검증
- 컴포넌트의 최종 렌더링 결과물인 DOM 구조가 올바르게 변경되었는지 검증

### it 함수
- 테스트의 실행 단위 - test description, 기대 결과에 대한 코드 작성
- it 함수는 test 함수의 alias로서 동일한 역할
- 기대 결과와 실제 결과가 같다면 테스트는 성공적으로 통과

### Assertion(단언)
- 테스트가 통과하기 위한 조건을 기술하여 검증 실행

### Matchers
- 기대 결과를 검증하기 위해 사용되는 일종의 API 집합
- vitest에서는 다양한 기본 매처를 제공. 확장하여 단언 실행 가능
- https://vitest.dev/api/expect.html
- https://github.com/testing-library/jest-dom#custom-matchers
