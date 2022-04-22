# React Testing Library

React-testing-livrary란 React에서 제공하는 Testing 라이브러리입니다.

## Principal

> 우리는 웹 페이지가 사용되는 방식과 매우 유사한 테스트를 작성하도록 권장하는 방법과 유틸리티만 노출하려고 합니다.
> 유틸리티는 다음 지침 원칙에 따라 이 프로젝트에 포함됩니다.
> 렌더링 구성 요소와 관련된 경우 구성 요소 인스턴스가 아닌 DOM 노드를 처리해야 하며 구성 요소 인스턴스를 처리하도록 권장해서는 안 됩니다.
> 일반적으로 사용자가 사용하는 방식으로 응용 프로그램 구성 요소를 테스트하는 데 유용해야 합니다. 우리는 컴퓨터와 종종 시뮬레이션된 브라우저 환경을 사용하고 있기 때문에 여기에서 약간의 절충안을 만들고 있지만 일반적으로 유틸리티는 사용하도록 의도된 대로 구성 요소를 사용하는 테스트를 권장해야 합니다.
> 유틸리티 구현 및 API는 간단하고 유연해야 합니다.
> 결국 우리가 원하는 것은 이 라이브러리가 매우 가볍고 단순하며 이해하기 쉽게 만드는 것입니다.

React에서 가장 많이 쓰이는 테스팅 프레임워크인 Jest를 기반으로 React Testing Library 설치
CRA는 jest 내장되어져있다.

## React Testing Library 설정

- DOM에 렌더링해놓은 내용들을 테스트가 끝날 때 다음 테스트를 위해서 지워주는 것
- jest-dom가 제공하는 matcher를 Jest 테스트 러너에게 인식

## RTL 주요 API

- Dom 컴포넌트 렌더링 해주는 <b>render()</b>
- 함수와, 특정 이벤트를 발생시켜주는 <b>fireEvent</b> 객체
- DOM 특정 영역을 선택하기 위한 다양한 쿼리 함수

render() 함수는 @testing-library/react 모듈로 부터 바로 임포트가 가능, 인자로 랜더링할 React 컴포넌트를 넘김니다. 그리고 render() 함수는 React Testing Library 제공하는 모든 쿼리 함수와 기타 유틸리티 함수 담고 있는 객체를 리턴합니다. 따라서 다음과 같이 자바스크립트의 객체 Destructuring 문법으로 render() 함수가 리턴한 객체로 부터 원하는 쿼리 함수만 얻어올 수 있습니다.

```jsx
import { render, fireEvent } from "@testing-library/react";

const { getByText, getByLabelText, getByPlaceholderText } = render(
  <YourComponent />
);
```

## 이미지 src path 값을 확인하는 테스팅 예제

- Component.tsx

```jsx
import React from 'react'
import styled from 'styled-components'

export interface CompareProps {
  beforeImagePath: string
  afterImagePath: string
}
const CompareBox = () => {
  return (
    <CompareBoxWrapper>
      <BeforeImage>
        <img
          src="./before.png"
          alt="beforeImage"
          data-testid="beforeImagePath"
        />
      </BeforeImage>
      <AfterRefImage>
        <img src="./after.png" alt="afterImage" data-testid="afterImagePath" />
      </AfterRefImage>
    </CompareBoxWrapper>
  )
}
```

- test.tsx

```jsx
import { render } from "@testing-library/react";
import CompareBox, { CompareProps } from "./index";

test("renders CompareBox image", () => {
  const stubPath: CompareProps = {
    beforeImagePath: "./before.png",
    afterImagePath: "./after.png",
  };
  const { getByAltText } = render(<CompareBox />);

  const beforePath = getByAltText("beforeImage");
  const afterPath = getByAltText("afterImage");

  expect(beforePath).toHaveAttribute("src", stubPath.beforeImagePath);
  expect(afterPath).toHaveAttribute("src", stubPath.afterImagePath);
});
```

img src='string'형태 path가 정확히 들어오는지 테스트하는 코드이다

1. 테스트로 사용할 변수값을 stubPath 객체에 저장해준다.
2. 쿼리함수 getByAlt를 자바스크립트의 객체 Destructuring 문법으로 render() 불러온다.
3. 테스트코드를 진행한다.
