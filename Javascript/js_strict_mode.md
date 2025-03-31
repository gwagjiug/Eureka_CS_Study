# strict mode란?

<aside>
💡 유연한 JS 검증체계를 딱딱하게

암묵적인 “느슨한 모드(sloppy mode)”를 해제하고, 명시적인 “엄격 모드(strict mode)”를 사용하는 방법이다. - MDN

</aside>

- 자바스크립트 언어의 문법을 보다 엄격히 적용하여 기존에는 무시되던 오류를 발생시킬 가능성이 높거나 자바스크립트 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시킨다.

### 사용법

- 해당 스코프의 시작에  `"use strict"` 지시어를 사용
- **전체 스크립트에 적용**하거나, **특정 함수 내에서만 활성화**할 수 있다.
- 모듈은 기본적으로 strict mode가 적용
    
    ```jsx
    function myStrictFunction() {
        // 모듈이기 때문에 기본적으로 strict mode가 적용
    }
    
    export default myStrictFunction;
    ```
    
    package.json에 `"type": "module"`을 추가하면 strict mode로 적용된다. → ESM(type: module)로 전환
    
    ```json
    {
      "name": "js",
      "version": "1.0.0",
      "description": "",
      "main": "index.js",
      "type": "module", 
      "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1",
      },
      "author": "",
      "license": "ISC",
    }
    ```
    
- 외부 라이브러리가 non-strict mode일 경우도 있기 때문에 전역에 strict mode를 적용하는 것은 바람직하지 않다. 이런 경우, 즉시 실행 함수로 스크립트 전체를 감싸서 스코프를 구분하고 즉시 실행 함수의 선두에 strict node를 적용한다.

```jsx
// strict mode는 즉시실행함수로 감싼 스크립트 단위로 적용하는 것이 바람직하다.
(function () {
  'use strict';

  // Do something...
}());
```

## **strict mode가 발생시키는 에러**

### **암묵적 전역 변수**

```jsx
function foo() {
  x = 10;
}

foo();
console.log(x); // 전역 변수가 된 변수를 암묵적 전역 변수(implicit global)
```

```jsx
(function () {
  'use strict';

  x = 1;
  console.log(x); // ReferenceError: x is not defined
}());
```

### **매개변수 이름의 중복**

- 중복(duplicate) 매개 변수 값을 허용하지 않는다.

```jsx
function foo(val1, val2, val1) {}; // undefined -> 중복되는 parameter에서 뒤에 것이 우선시 됨

'use strict' 
function foo(val1, val2, val1) {}; // Uncaught SyntaxError: Duplicate parameter
```

### 함수의 블록스코프

- Block 내에서, var는 느슨한 모드와 동일한 스코프이지만, 함수는 Block-Scope이다.
    - 즉 block 내 함수는 block의 Declarative Environment Record에 존재

```jsx
느슨한 모드에서는 var 및 함수 선언 모두 Block-Scope를 갖는다. 
즉, 블록 내에 정의되어 있더라도 함수 전체에서 액세스할 수 있다.

if (true) {
  var x = 10;
  function bar() {
    console.log('I am inside the block');
  }
}

console.log(x);  // 10
bar();           // I am inside the block
```

```jsx
'var'은 엄격 모드에서도 블록 범위가 되지 않으므로 'var y'는 여전히 블록 외부에서 액세스할 수 있다.
그러나 bar() 함수는 이제 블록 범위다. if 블록 내부에 선언되었기 때문에 블록 외부에서 호출할 수 없다.

'use strict';

if (true) {
  var y = 20;
  function bar() {
    console.log('I am inside the block in strict mode');
  }
}

console.log(y);   // 20 (var still works)
bar();            // Uncaught ReferenceError: bar is not defined

```

### strict mode를 사용하면 좋은점

strict mode에서는 일반적인 Javascript의 의미에서 몇 가지를 변경한다.

1. JS 오류는 아니지만, 함정이 될 일을 오류가 발생하도록 변경하여 제거한다. 
⇒ 코드의 문제점을 알려 결과적으로 디버깅이 쉬워진다.
2. JS 엔진의 최적화 처리를 어렵게 만드는 오류를 수정한다.
⇒ non-strict mode의 동일한 코드보다 빠르게 수행할 수 있다.
3. 향후 ECMAScript 버전과 충돌할 수 있는 구문의 사용을 금지한다.
⇒ 발생가능한 에러를 예방한다.
- `"use strict"` 을 통한 strict mode 사용은 대부분의 경우 바람직하므로 권장되는 습관
- Lint를 사용한다면 strict mode의 역할을 대부분 대체하며, 둘 다 이중으로 검사해도 좋다.
