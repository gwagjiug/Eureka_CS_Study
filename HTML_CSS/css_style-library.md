
## CSS (Cascading Style Sheets)

HTML 문서에 있는 요소들에 선택적으로 스타일을 적용할 수 있다

```CSS
p {
  color: red;
  width: 500px;
  border: 1px solid black;
}
```

<img src="https://velog.velcdn.com/images/corinthionia/post/85cb4f5f-9c8f-48f6-9b86-99130f432e8f/image.png" />
- `Rule set` 전체 구조
- `Selector (선택자)` Rule set의 맨 앞에 있는 HTML 요소 이름 - 꾸밀 요소를 선택
- `Declaration (선언)` 꾸밀 요소의 속성을 명시
- `Property (속성)` 주어진 HTML 요소를 꾸밀 수 있는 방법
- `Property value (속성 값)`

### 장점

- **표준화된 방식** - 웹의 스타일링을 위한 기본 표준으로, 모든 웹 브라우저에서 지원한다
- **전역 스타일링** - 스타일을 한 번 정의하면 모든 페이지에 쉽게 적용할 수 있다
- **분리된 스타일시트** - HTML과 CSS가 분리되어 있어 유지보수가 용이하다

### 단점

- **스코프 문제**: 전역 스타일링이기 때문에 스타일 충돌이 발생할 수 있다
- **정적 스타일링**: 동적 스타일링 / 조건부 스타일링이 어렵다

## CSS Preprocessor

- CSS 전처리기는 <mark>기본 CSS에 추가적인 기능을 제공</mark>하여 코드 작성과 유지보수를 더 쉽게 하고, 스타일시트의 재사용성을 높이는 도구이다
- 전처리기는 CSS 문법과 굉장히 유사하지만, 변수, 네스팅(중첩), 믹스인(mixin)과 같은 기능을 활용하여 CSS보다 확장된 기능을 제공한다
  - 대신 웹에서는 표준 CSS만 동작하기 때문에 전처리기를 표준 CSS로 컴파일 하는 과정이 필요하다
- 가장 많이 사용되는 CSS 전처리기에는 Sass(SCSS), LESS, 그리고 Stylus가 있다

### Sass & SCSS

- CSS 구문과 완전히 호환되도록 새로운 구문을 도입한 CSS의 Superset이다
- SCSS는 CSS와 유사한 구문을 사용하고, Sass는 더 간결한 구문을 사용한다

```Sass
/* Sass */
body
  font: 100% $font-stack
  color: $primary-color
```

```SCSS
/* 변수 사용 */
$primary-color: #333;

body {
  color: $primary-color;
}
```

```SCSS
/* 선택자 중첩 (Nesting) */
nav {
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }
}
```

```SCSS
/* 믹스인 (Mixin) */
@mixin border-radius($radius) {
  -webkit-border-radius: $radius;
  -moz-border-radius: $radius;
  border-radius: $radius;
}

.box {
  @include border-radius(10px);
}

```

```SCSS
/* 연산 */
.container {
  width: 100% / 3;
}
```

### 장점

- **코드 재사용** - 변수, 믹스인, 함수 등을 활용하여 코드 중복을 줄이고, 재사용성을 높인다
- **네스팅** - CSS 선택자를 중첩하여 가독성을 높인다
- **유지보수 용이성** - 변수와 믹스인을 사용하여 한 곳에서 스타일을 변경할 수 있어 유지보수가 용이하다
- **확장된 기능** - 연산, 조건문, 반복문 등 다양한 프로그래밍 기능을 제공한다

### 단점

- **컴파일 필요**: 브라우저가 직접 해석할 수 없어 CSS로 컴파일해야 한다
- **복잡성**: 기능이 많아 복잡해질 수 있으며, 러닝 커브가 있을 수 있다

## CSS-in-JS

- JavaScript 코드와 스타일을 같은 파일에서 관리할 수 있게 한다
- 대표적인 라이브러리 `styled-components` `emotion`
  - `styled-components`는 스타일을 컴포넌트 수준에서 관리할 수 있게 해주며, 스타일이 적용된 React 컴포넌트를 생성한다
  - `emotion`은 `styled` API와 `css` 함수 두 가지 주요 방식으로 스타일을 정의할 수 있다

```javascript
// App.js

import styled from 'styled-components';

const Button = styled.button`
  border: none;
  border-radius: 4px;
  cursor: pointer;
`;

function App() {
  return (
    <div>
      <Button>버튼 클릭</Button>
    </div>
  );
}

export default App;
```

JavaScript 변수를 사용하여 컴포넌트에 동적으로 스타일을 적용할 수 있다

```javascript
const Home = () => {
  const [isClicked, setIsClicked] = useState(false);
  return <Wrapper isClicked={isClicked}>Hello World!</Wrapper>;
};

const Wrapper = styled.div`
  width: 100%;
  height: 100%;
  background: ${props => (props.isClicked ? 'pink' : 'powderblue')};
`;
```

### 장점

- **컴포넌트 스코핑** - 스타일이 특정 컴포넌트에 스코핑되어 스타일 충돌이 없다
- **동적 스타일링** - 자바스크립트를 사용하여 조건부 스타일링이 가능하다

### 단점

- **런타임 오버헤드** - 스타일이 런타임에 적용되므로 성능에 영향을 줄 수 있다
- **빌드 복잡성** - 추가적인 빌드 설정이 필요할 수 있다
- [SSR, SSG, 서버 컴포넌트에서 사용이 어렵다](https://nextjs.org/docs/app/building-your-application/styling/css-in-js)

  > Warning: CSS-in-JS libraries which require runtime JavaScript are not currently supported in Server Components. Using CSS-in-JS with newer React features like Server Components and Streaming requires library authors to support the latest version of React, including concurrent rendering.

  > 많은 CSS-in-JS 라이브러리가 런타임에 스타일을 생성합니다. 이는 클라이언트에서 스타일을 동적으로 생성하고 적용하기 때문에 서버 컴포넌트에서는 성능에 부정적인 영향을 줄 수 있습니다. 서버 측에서는 미리 스타일을 생성하고 이를 HTML에 포함시켜 빠르게 렌더링하는 것이 이상적입니다.

## CSS Modules

- CSS Modules는 스타일 시트를 모듈화하여 각 클래스와 스타일을 로컬 범위로 스코핑한다
- 위에서 봤던 CSS의 전역 네임스페이스 문제를 해결하여 스타일이 다른 컴포넌트와 충돌하지 않도록 도와준다

아래와 같이 `Button.module.css`의 `.button` 클래스는 해싱된 고유 이름으로 컴파일된다.
예를 들어, `.button__hash123`와 같은 형태로 변환되어 전역 클래스 이름 충돌을 방지한다!

```css
/* Button.module.css */
.button {
  background-color: #3498db;
  color: white;
}

.button:hover {
  background-color: #2980b9;
}
```

### 장점

- **로컬 스코프** - 클래스 네임이 자동으로 고유화되어 스타일 충돌이 없다
- **단순함** - 기존 CSS 문법을 그대로 사용할 수 있다
- **효율성** - 런타임이 아닌 컴파일 타임에 처리되어 성능에 유리하다

### 단점

- **제한된 동적 스타일링** - 기본적으로 정적 스타일링을 제공하며, 동적 스타일링에는 추가적인 설정이 필요하다

### CSS-in-JS vs CSS Modules 비교

- `CSS-in-JS` 런타임에 스타일을 동적으로 생성하는 경우, JavaScript 코드가 실행되면서 스타일이 생성되고, DOM에 삽입되기 때문에 페이지 로드가 지연될 수 있다
- `CSS Modules` 컴파일 타임에 CSS 클래스 이름이 해싱되어 스타일이 최적화된 형태로 포함된다. 페이지 로드 시 브라우저는 최적화된 CSS 파일을 즉시 사용하여 스타일을 적용할 수 있다

## Tailwind CSS

Tailwind CSS는 유틸리티-퍼스트 CSS 프레임워크로, 웹 개발자들이 빠르게 사용자 인터페이스를 만들 수 있도록 돕는다

```javascript
<blockquote class="text-base md:text-md 3xl:text-lg">
  Oh I gotta get on that internet, I'm late on everything!
</blockquote>
```

### 장점

- **유틸리티 퍼스트** - 미리 정의된 유틸리티 클래스를 사용하여 빠르게 스타일을 적용할 수 있다
- **디자인 일관성** - 일관된 디자인 시스템을 쉽게 유지할 수 있다

### 단점

- **초기 설정 복잡성 & 러닝 커브** - 프로젝트에 Tailwind를 설정하는 과정이 복잡할 수 있다
- **클래스 네이밍** - HTML에 많은 클래스를 추가해야 하므로 가독성이 떨어질 수 있다

---

## Reference

- [MDN Web Docs - CSS 기초](https://developer.mozilla.org/ko/docs/Learn/Getting_started_with_the_web/CSS_basics)
