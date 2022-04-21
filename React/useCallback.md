# useCallback

메모이제이션된 콜백을 반환합니다. \*메모제이션: 메모이제이션은 컴퓨터 프로그램이 동일한 계산을 반복해야 할 때, 이전에 계산한 값을 메모리에 저장함으로써 동일한 계산의 반복 수행을 제거하여 프로그램 실행 속도를 빠르게 하는 기술이다. 동적 계획법의 핵심이 되는 기술

```javascript
const memoizedCallback = useCallback(() => {
  doSomething(a, b);
}, [a, b]);
```

## useCallback은 특정 함수를 새로 만들지 않고 재사용하고 싶을 때 사용하는 함수.

- useCallback은 첫 번째 인자로 넘어온 함수를, 두 번째 인자로 넘어온 배열 형태의 함수 실행 조건의 값이 변경될 때까지 저장해놓고 재사용

> 인라인 콜백과 그것의 의존성 값의 배열을 전달하세요. useCallback은 콜백의 메모이제이션된 버전을 반환할 것입니다. 그 메모이제이션된 버전은 콜백의 의존성이 변경되었을 때에만 변경됩니다. 이것은, 불필요한 렌더링을 방지하기 위해 (예로 shouldComponentUpdate를 사용하여) 참조의 동일성에 의존적인 최적화된 자식 컴포넌트에 콜백을 전달할 때 유용합니다. React Hooks useCallback 문서

- 함수를 재선언 하는것은 CPU, 큰 메모리도 차지하지 않지만, 한번 만든 함수를 재사용 필요할 때만 재생성 하는것은 중요.
- 따라서 단순히 컴포넌트 내에서 함수를 반복해서 생성하지 않기 위해서 useCallback()을 사용하는 것은 큰 의미가 없거나 오히려 <b>손해<b>인 경우도 있습니다.

# 그럼 언제 사용하나?

## 자바스크립트 함수 동등성

useCallback() hook 함수를 언제 사용해야하는지 제대로 이해하려면 자바스크립트 함수 간의 동등함이 어떻게 결정 되는지 알아야한다.

```javascript
const add1 = () => x + y;
//undefined
const add2 = () => x + y;
//undefined
add1 === add2;
//false
```

- 자바스크립트에선 함수도 객체 취급 메모리 주소에 의한 참조 비교가 일어나기 때문에.
- 이러한 자바스크립트 특징 때문에 리액트 컴포넌트 함수 내에서 어떤 다른 함수 인자로 넘기거나 자식 컴포넌트의 prop으로 넘길 때 에상치 못한 성능 이슈 발생한다.

## 의존 배열로 함수를 넘길 때

- 많은 React hook 함수들이 불필요한 작업을 줄이기 위해서 두 번째 인자로, 첫 번째 함수가 의존해야하는 배열을 받습니다.

예 useEffect
컴포넌트에서 API를 호출하는 코드는 fetchUser 함수가 변경될 때만 호출됩니다. 여기서 위에서 설명드린 자바스크립트가 함수의 동등성을 판단하는 방식 때문에 예상치 못한 무한 루프에 발생하게 됩니다. fetchUser는 함수이기 때문에, userId 값이 바뀌든 말든 컴포넌트가 랜더링될 때 마다 새로운 참조값으로 변경이 됩니다. 그러면 useEffect() 함수가 호출되어 user 상태값이 바뀌고 그러면 다시 랜더링이 되고 그럼 또 다시 useEffect() 함수가 호출되는 악순환이 반복

```javascript
import React, { useState, useEffect } from "react";

function Profile({ userId }) {
  const [user, setUser] = useState(null);

  const fetchUser = () =>
    fetch(`https://your-api.com/users/${userId}`)
      .then((response) => response.json())
      .then(({ user }) => user);

  useEffect(() => {
    fetchUser().then((user) => setUser(user));
  }, [fetchUser]);

  // ...
}
```

useCallback() hook 함수를 이용하면 컴포넌트가 다시 랜더링되더라도 fetchUser 함수의 참조값을 동일하게 유지시킬 수 있습니다. 따라서 의도했던 대로, useEffect()에 넘어온 함수는 userId 값이 변경되지 않는 한 재호출 되지 않게 됩니다.

```javascript
import React, { useState, useEffect } from "react";

function Profile({ userId }) {
  const [user, setUser] = useState(null);

  const fetchUser = useCallback(
    () =>
      fetch(`https://your-api.com/users/${userId}`)
        .then((response) => response.json())
        .then(({ user }) => user),
    [userId]
  );

  useEffect(() => {
    fetchUser().then((user) => setUser(user));
  }, [fetchUser]);

  // ...
}
```

### React가 리렌더링을 하는 조건

- props가 변경되었을 때
- state가 변경되었을 때
- 부모 컴포넌트가 렌더링되었을 때
- forceUpdate() 를 실행하였을 때

주로 React.memo와 사용한다.
