
## 1. 모듈의 일반적 의미

- 모듈이란 애플리케이션을 구성하는 개별적 요소로서 재사용 가능한 코드 조각을 말한다
- 일반적으로 모듈은 기능별로 분리되어 개별 파일로 작성한다
  - 이때 모듈이 성립하려면 모듈은 자신만의 <b>파일 스코프(모듈 스코프)</b>를 가질 수 있어야 한다
  - 파일 스코프를 갖는 모듈의 자산(변수, 함수, 객체 등)은 기본적으로 캡슐화되어 다른 모듈에서 접근할 수 없다

---

## 2. 자바스크립트와 모듈

- 자바스크립트가 만들어진 지 얼마 안 되었을 때에는 JS로 만든 스크립트의 크기도 작고 기능도 단순했기 때문에 오랫동안 **모듈 관련 표준 문법이 존재하지 않았다**
  - 자바스크립트는 `script` 태그를 사용하여 외부의 파일을 로드할 수는 있지만, 파일마다 독립적인 파일 스코프를 갖지 않는다. 다시 말해, 자바스크립트 파일을 여러 개의 파일로 분리하여 `script` 태그로 로드해도 분리된 자바스크립트 파일들은 **결국 하나의 자바스크립트 파일 내에 있는 것처럼 동작**한다
  - 즉, 모든 자바스크립트 파일은 하나의 전역을 공유한다
- 자바스크립트를 브라우저 환경에 국한하지 않고 범용적으로 사용하려는 움직임이 생기면서 모듈 시스템이 등장했다
  - `AMD` 가장 오래된 모듈 시스템 중 하나로 `require.js`라는 라이브러리를 통해 처음 개발되었다. 비동기 로딩을 지원하는 모듈 시스템으로, 주로 브라우저 환경에서 사용되며 `define()`과 `require()` 함수를 사용한다
  - `CommonJS` 주로 Node.js 환경에서 사용되며, `require()`와 `module.exports`를 사용해 모듈을 가져오고 내보낸다
  - `UMD` CommonJS와 AMD를 모두 지원하는 모듈 시스템으로, 코드가 다양한 환경에서 동작할 수 있도록 설계되었다
- JavaScript의 모듈 시스템이 표준화되지 않은 채로 여러 구현이 등장하면서, 이를 통합하고 표준화할 필요성이 커졌다. 이에 따라 ES6(ECMAScript 2015)에서 ES 모듈(ESM) 시스템이 도입되었다.

> **[참고]** CommonJS와 ESM은 JavaScript에서 모듈을 정의하고 가져오는 방식에 있어 서로 다른 접근 방식을 제공한다
>
> - `CommonJS` 주로 서버 측 환경에서 사용된다. Node.js는 CommonJS 모듈 시스템을 기본으로 사용한다.
> - `ESM` 브라우저와 서버 측 모두에서 사용할 수 있다. ESM은 ECMAScript 2015(ES6)에서 표준으로 도입되었으며, 현재는 Node.js에서도 지원된다.

---

## 3. 모듈의 핵심 기능

모든 호스트 환경에 공통적으로 적용되는 모듈의 핵심 기능에 대해 알아보자.

### 1) 엄격 모드

- 모듈은 항상 엄격모드(use strict)로 실행된다. 선언되지 않은 변수에 값을 할당하는 등의 코드는 에러를 발생시킨다.

### 2) 모듈 레벨 스코프

- 모듈은 자신만의 스코프가 있기 때문에 모듈 내부에서 정의한 변수나 함수는 다른 스크립트에서 접근할 수 없다
- 따라서 외부에 공개하려는 모듈은 export 해야 하고, 내보내진 모듈을 가져와 사용하려면 import 해야 한다
  - `export` 지시자를 변수나 함수 앞에 붙이면 외부 모듈에서 해당 변수나 함수에 접근할 수 있다 (모듈 내보내기)
  - `import` 지시자를 사용하면 외부 모듈의 기능을 가져올 수 있다 (모듈 가져오기)
- 브라우저 환경에서도 `<script type="module">`을 사용해 모듈을 생성하면 **독립적인 스코프**가 만들어진다

  ```html
  <script type="module">
    // user는 해당 모듈 안에서만 접근 가능하다
    let user = 'John';
  </script>

  <script type="module">
    alert(user); // Error: user is not defined
  </script>
  ```

### 3) 단 한 번만 평가됨

- <em>동일한 모듈이 여러 곳에서 사용되더라도 모듈은 최초 호출 시 단 한 번만 실행된다</em>
- 실행 후 결과는 해당 모듈을 가져가려는 모든 모듈에 내보내진다

```javascript
// a.js
import { admin } from './admin.js';
admin.name = 'Pete';
```

```javascript
// b.js
import { admin } from './admin.js';
alert(admin.name); // Pete
```

- a.js와 b.js 모두 같은 객체를 가져오므로 a.js에서 객체에 가한 조작을 b.js에서도 확인할 수 있다
- 즉, 위의 예시처럼 모듈은 단 한 번만 실행되고 실행된 모듈은 필요한 곳에서 공유되므로 어느 한 모듈에서 `admin` 객체를 수정한다면 다른 모듈에서도 변경사항을 확인할 수 있다

---

## 4. 브라우저 특정 기능

브라우저 환경에서 `type="module"`이 붙은 스크립트가 일반 스크립트와 어떤 점이 다른지 알아보자

### 1) 지연 실행

- 모듈 스크립트는 <em>항상</em> 지연 실행된다. 즉, 외부 스크립트, 인라인 스트립트와 관계없이 `defer` 속성을 붙인 것처럼 실행된다
  > 브라우저는 <b>defer</b> 속성이 있는 스크립트(이하 defer 스크립트 또는 지연 스크립트)를 '백그라운드’에서 다운로드 합니다. 따라서 지연 스크립트를 다운로드 하는 도중에도 HTML 파싱이 멈추지 않습니다. 그리고 defer 스크립트 실행은 페이지 구성이 끝날 때까지 지연 됩니다.
- 외부 모듈 스크립트 `<script type="module" src="...">`를 다운로드할 때 브라우저의 HTML 처리가 멈추지 않는다. 브라우저는 외부 모듈 스크립트와 기타 리소스를 병렬적으로 불러온다.
- 모듈 스크립트는 DOM이 완전히 준비될 때까지 대기 상태에 있다가 HTML 문서가 완전히 만들어진 이후에 실행된다. (모듈의 크기가 아주 작아서 HTML보다 빨리 불러온 경우도 포함)
- 스크립트의 상대적 순서가 유지된다. 문서상 위쪽의 스크립트부터 차례로 실행된다

```javascript
<script type="module">
  // 모듈 스크립트는 지연 실행되기 때문에 페이지가 모두 로드되고 난 다음에 alert 함수가 실행되므로
  // alert 창에 object가 정상적으로 출력된다. 모듈 스크립트는 아래쪽의 button 요소를 '볼 수' 있다.
  alert(typeof button);
</script>


<script>
  // 일반 스크립트는 페이지가 완전히 구성되기 전이라도 바로 실행된다.
  // 버튼 요소가 페이지에 만들어지기 전에 접근하였기 때문에 undefined가 출력된다.
  alert(typeof button);
</script>

<button id="button">Button</button>
```

> 모듈을 사용할 땐 HTML 페이지가 완전히 나타난 이후에 모듈이 실행된다는 점에 유의해야 한다. 페이지 내 특정 기능이 모듈 스크립트에 의존적인 경우, 모듈이 완전히 로딩되기 전에 페이지가 먼저 사용자에게 노출되면 사용자가 혼란을 느낄 수 있기 때문이다.

### 2) 인라인 스크립트의 비동기 처리

- **일반 스크립트** 기본적으로 동기적으로 실행되지만, `async`나 `defer` 속성을 통해 비동기 실행이 가능하다
- **모듈 스크립트** 기본적으로 비동기적으로 실행된다

  - 아래 예시에서 가져오기(`./analytics.js`) 작업이 끝나면 HTML 파싱이 끝나지 않았거나 다른 스크립트가 대기 상태에 있더라도 모듈이 바로 실행된다

  ```html
  <!-- 필요한 모듈(analytics.js)의 로드가 끝나면 문서나 다른 <script>가 로드되길 기다리지 않고 바로 실행된다 -->
  <script async type="module">
    import { counter } from './analytics.js';

    counter.count();
  </script>
  ```

### 3) 외부 스크립트

`type="module"`이 붙은 외부 모듈 스크립트에는 두 가지 큰 특징이 있다

- `src` 속성값이 동일한 외부 스크립트는 한 번만 실행된다
- 외부 사이트 같이 다른 오리진에서 모듈 스크립트를 불러오려면 CORS 헤더가 필요하다

  - 모듈이 저장되어 있는 원격 서버가 `Access-Control-Allow-Origin: *` 헤더를 제공해야만 외부 모듈을 불러올 수 있다

    ```html
    <!-- another-site.com이 Access-Control-Allow-Origin을 지원해야만 외부 모듈을 불러올 수 있다 -->
    <!-- 그렇지 않으면 스크립트는 실행되지 않는다 -->
    <script type="module" src="http://another-site.com/their.js"></script>
    ```

---

## 5. CommonJS

- 다른 모듈을 사용할 때에는 `require`을 사용하고, 모듈을 내보낼 때에는 `module.exports`를 사용한다
- `module`은 현재 모듈에 대한 정보를 담고 있는 객체이다

  ```javascript
  // a.js

  const func = () => {
    console.log('Hello World!');
  };

  module.exports = { func };
  ```

  ```javascript
  // b.js

  const func = require('./a.js');
  ```

### 1) module.exports

- 기본적으로 `module.exports`는 모듈의 단일 객체를 내보낸다
  - 모듈에서 내보낼 수 있는 것은 <em>단 하나의 객체</em>이므로, 만약 여러 개의 값을 내보내고 싶다면 객체 형태로 내보내는 방식이 일반적이다
- `module.exports`는 기본적으로 빈 객체로 설정되어 있지만, 원하는 값으로 변경할 수 있다

---

## 6. ES6 모듈 (ESM)

- ES6에서는 클라이언트 사이드 자바스크립트에서도 동작하는 모듈 기능을 추가했다
  - IE를 제외한 대부분의 브라우저에서 ES6 모듈을 사용할 수 있다
- `script` 태그에 `type="module"` 어트리뷰트를 추가하면 로드된 자바스크립트 파일은 모듈로서 동작한다

  ```jsx
  <script type="module" src="app.mjs"></script>
  ```

- 일반적인 자바스크립트 파일이 아닌 ESM임을 명확히 하기 위해 ESM의 파일 확장자는 `mjs` 를 사용할 것을 권장한다

### 1) 모듈 스코프

일반적인 자바스크립트 파일은 `script` 태그로 분리해서 로드해도 독자적인 모듈 스코프를 갖지 않는다

```jsx
// foo.js

var x = 'foo'; // 전역변수
console.log(window.x); // foo
```

```jsx
// bar.js

var x = 'bar'; // 전역변수 - foo.js의 x와 중복된 선언
consoel.log(window.x); // bar - 전역변수 x의 값이 재할당 됨
```

```html
<!doctype html>
<html>
  <body>
    <script src="foo.js"></script>
    <script src="bar.js"></script>
  </body>
</html>
```

위의 HTML 파일에서 `script` 태그로 분리해서 로드된 2개의 자바스크립트 파일은 하나의 자바스크립트 파일 내에 있는 것처럼 동작한다. 즉, 하나의 전역을 공유한다.

<em>**그러나 ESM은 파일 자체의 독자적인 모듈 스코프를 제공한다.**</em> 모듈 내에 선언한 식별자는 모듈 스코프가 다르기 때문에 모듈 외부에서 참조할 수 없다.

```jsx
// foo.mjs

var x = 'foo'; // 전역변수 아님
console.log(x); // foo
console.log(window.x); // undefined
```

```jsx
// bar.mjs

var x = 'bar'; // 전역변수 아님
console.log(x); // bar
consoel.log(window.x); // undefined
```

```jsx
<!DOCTYPE html>
<html>
<body>
	<script src="foo.mjs"></script>
	<script src="bar.mjs"></script>
</body>
</html>
```

### 2) `export` 키워드

모듈 내부에서 선언한 식별자를 외부에 공개하여 다른 모듈들이 재사용할 수 있게 하려면 `export` 키워드를 사용한다

```jsx
// lib.mjs
export const pi = Math.PI;

// 함수의 공개
export function square(x) {
  return x * x;
}
```

`export` 할 대상을 하나의 객체로 구성하여 한 번에 export 할 수도 있다

```jsx
export { pi, square };
```

### 3) `import` 키워드

다른 모듈에서 공개한 식별자를 자신의 모듈 스코프 내부로 로드할 때 사용한다

```jsx
// app.mjs
import { pi, square } from './lib.mjs';

console.log(pi); // 3.1415926535
console.log(square(100)); // 100
```

모듈이 export한 식별자 이름을 일일이 지정하지 않고 하나의 이름으로 한번에 import할 수도 있다. 이때 import되는 식별자는 `as` 뒤에 지정한 이름의 객체에 프로퍼티로 할당된다

```jsx
import * as lib from './lib.mjs';
console.log(lib.pi);
```

모듈이 export한 식별자 이름을 변경하여 import할 수도 있다

```jsx
import { pi as PI } from './lib.mjs';
```

모듈에서 하나의 값만 export한다면 `default` 키워드를 사용할 수 있다. `default` 키워드를 사용하는 경우 기본적으로 이름 없이 하나의 값을 export한다

```jsx
// lib.mjs
export default x => x + x;
```

- `default` 키워드를 사용하는 경우 `var` `let` `const` 키워드는 사용할 수 없다
- `default` 키워드와 함께 export한 모듈은 `{}` 없이 임의의 이름으로 import한다

  ```jsx
  // app.mjs

  import square from './lib.mjs';
  console.log(square(3)); // 9
  ```

---

## 7. export vs. module.exports

### 1) 모듈 시스템

- `module.exports`는 CommonJS 모듈 시스템의 일부이며, 주로 Node.js 환경에서 사용된다
- `export`는 ES 모듈 시스템의 일부로, 브라우저와 Node.js를 포함한 다양한 환경에서 사용된다

### 2) 내보내기 방식

- `module.exports`는 <b>한 번에 하나의 객체(또는 함수, 변수 등)</b>를 내보낼 수 있다. 즉, 모듈의 기본 객체로 무엇을 내보낼지를 지정한다
- `export`는 여러 개의 값을 개별적으로 내보낼 수 있으며, export default를 사용해 단일 값을 기본으로 내보낼 수도 있다

### 3) 호환성

- `module.exports`는 CommonJS 기반의 Node.js 환경에서 널리 사용되며, ESM 기반 코드와의 호환성 문제로 인해 특정 상황에서 변환이 필요할 수 있다
- `export`는 최신 JavaScript 표준에 포함된 기능으로, 브라우저와 Node.js 등 다양한 환경에서 사용 가능하다

---

## 8. 빌드 툴

- 브라우저 환경에서 모듈을 단독으로 사용하는 경우는 흔치 않다. 대부분 Webpack과 같은 도구를 이용하여 모듈을 번들링하여 프로덕션 서버에 올리는 방식을 사용한다
- 빌드 툴의 역할
  - HTML의 `<script type="module">`에 넣을 주요(main) 모듈(진입점 역할을 하는 모듈)을 선택한다
  - 주요 모듈에 의존하고 있는 모듈 분석을 시작으로 모듈 간의 의존 관계를 파악한다
  - 모듈 전체를 한데 모아 하나의 큰 파일을 만든다 - 이 과정에서 `import` 문이 번들러 내 함수로 대체되므로 기존 기능은 그대로 유지된다
  - 이런 과정 중에 변형이나 최적화도 함께 수행된다
- 번들링 툴을 사용하면 스크립트들은 하나 혹은 여러 개의 파일로 번들링된다
  - 이때 번들링 전 스크립트에 있던 `import` `export` 문은 특별한 번들러 함수로 대체된다
  - 번들링 과정이 끝나면 기존 스크립트에서 `import` `export` 가 사라지므로 `type="module"`도 필요없게 되어 번들링 과정을 거친 스크립트는 일반 스크립트처럼 취급할 수 있다
