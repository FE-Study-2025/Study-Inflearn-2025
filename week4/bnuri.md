p1 4-6
# 단위 테스트 작성하기

## 리액트 훅 테스트(feat. act 함수)

- 리액트 렌더링 매커니즘을 따른 단순 함수이기 때문에 독립적인 단위 테스트를 작성하기 적합
- React Testing Library의 renderHook API를 사용해 편하게 검증 가능

```js
const { result, rerender } = renderHook(useConfirmModal);

// result: 훅을 호출하여 얻은 결과 값을 반환 (result.current 값의 참조를 통해 최신 상태 추적 가능)

// rerender: 훅을 원하는 인자와 함께 새로 호출하여 상태 갱신
```

### act 함수
- 상호 작용(렌더링, 이펙트 등)을 함께 그룹화하고 실행해 렌더링과 업데이트가 실제 앱이 동작하는 것과 유사한 방식으로 동작
- 테스트 환경에서 act를 사용하면 가상의 돔에 제대로 반영되었다는 가정하에 테스트가 가능해짐
- 컴포넌트 렌더링 한 뒤 없데이트 하는 코드의 결과를 검증하고 싶을 때 사용
- react 테스팅 라이브러리의 render함수에는 내부적으로 act 함수를 호출함 (act도 추가로 구현되어 있음)
- 별도로 리액트 state를 업데이트하여 변경 사항을 검증해야 한다면 act 함수를 사용하여 state를 반영해야 함

# 타이머 테스트 
- 테스트 코드는 비동기 타이머와 무관하게 동기적으로 실행
- 타이머 모킹!

```js
describe('debounce', () => {
    beforeEach(() => {
        // teardown에서 모킹 초기화
        vi.userFakeTimers();
        // 테스트 당시의 시간에 의존하는 테스트의 경우 시간을 고정하지 않으면 테스트 깨질 수 있음 (일관된 환경에서 테스트 가능)
        vi.setSystemTime(new Date('2023-12-24));
    })

    afterEach(() => {
        // 타이머 모킹 해제 필수 (다른 테스트에 영향이 안가도록)
        // 타이머 복원
        vi.useRealTimers();
    })

    const spy = vi.fn();
    const debouncedFn = debounce(spy, 300);
    debouncedFn();
    vi.advanceTimersByTime(300); // 시간이 흐른 것 처럼 제어
    expect(spy).toHaveBeenCalled();

    // 한 번만 호출되었는지
    expect(spy).toHaveBeenCalledTimes(1);
})

```


# fireEvent 
- DOM 이벤트를 시뮬레이션 하기 위해 제공
- 내장되어 있기 때문에 별도의 설치 없이 사용 가능

```js

import { fireEvent } from '@testing-library/react';
fireEvent.click(getByText(container, 'Submit));

``` 

### fireEvent vs userEvent
- fireEvent는 DOM이벤트만 발생시키는 반면, userEvent는 다양한 상호 작용을 시뮬레이션 가능
    - 클릭 이벤트가 발생한다면,, pointerdown, mousedown, pointerup, mouseup, click, focus가 연쇄적으로 발생
    - 실제 상황처럼 disabled된 버튼이나 인풋 입력이 불가능함
- 테스트 코드 작성 시에는 userEvent를 활용해 실제 상황과 유사한 코드로 테스트의 신뢰성을 높이는게 좋다
- userEvent에서 지원하지 않는 부분이 있을 때, fireEvent 활용을 고민하자



