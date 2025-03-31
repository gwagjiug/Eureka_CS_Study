## 1. 호이스팅 (Hoisting)

### 개요
* 호이스팅은 자바스크립트에서 변수와 함수 선언이 코드 실행 전에 메모리 상단으로 끌어올려지는 현상이다. 

* 이로 인해 변수와 함수는 실제 코드 실행 전에 선언될 수 있다.

### 변수 호이스팅

- **예시 코드**:
  ```javascript
  console.log(x); // undefined
  var x = 5;
  console.log(x); // 5
  ```

  * 위 코드는 호이스팅에 의해 다음과 같이 변환된다.
 
    
  ```javascript
  var x;
  console.log(x); // undefined
  x = 5;
  console.log(x); // 5
  ```
  
* 여기서 변수 'x'는 선언만 끌어올려지고, 초기화는 원래 위치에서 이루어진다.

* `var`는 선언과 동시에 초기화가 이루어진다. 즉, 선언과 동시에 undefiend가 할당되는 것.

* 그러나 `let` 과 `const`는 다르다. 선언만 될 뿐 초기화가 이루어지지 않는 `TDZ` 에 들어가게 된다.

* `TDZ`란? Temporal Dead Zone 으로 자바스크립의 변수 생성 단계 중 첫 번째 단계인 **선언** 단계에서 변수 객체가 생성되고 초기화 되지 않은 상태에 이른 것을 말한다.

* 모든 선언(function, var, let, const, 및 class)는 Javascript에서 호이스팅 되며, var 선언은 undefined로 초기화 되지만, let 및 const 선언은 초기화 되지 않은 상태로 유지된다.

```java script
console.log(hoist); // Output: undefined
var hoist = 'The variable has been hoisted.';
```
```javascript
console.log(hoist); // Output: ReferenceError: hoist is not defined ...
let hoist = 'The variable has been hoisted.';
```

### 함수 호이스팅

* 함수는 선언 자체가 호이스팅 되어 호출하기 전에 정의된 함수를 사용할 수 있다.

```javascript
foo(); // "Hello"

function foo() {
    console.log("Hello");
}
```

### 결론

* 호이스팅은 함수를 어디에 선언 하였든지, 신경 스지 않고 필요한 곳에서 자유롭게 사용하기 위해 만들어진 기능이다.

* 그러나 이 기능이 때로는 의도치 않은 버그를 생성할 여지가 있고, 따라서 호이스팅을 의도적으로 사용하는 경우가 아니라면 호이스팅이 되지 않거나,

* TDZ 으로 초기화 되는 `let`,`const` 함수 표현식을 사용하는 것을 권장
---

## 2. 'var' 키워드의 문제점

### 스코프 문제

* `var` 로 선언된 변수는 함수 전체에서 유효하다. 블록 내에서 선언을 해도 블록 외부에서 접근 가능한 것이 특징이고

* `let` 으로 선언된 변수는 블록 내에서만 유효하다. 블록 외부에서 접근하려고하면 `ReferenceError` 가 발생

```javascript
function example() {
    if (true) {
        var x = 10;
    }
    console.log(x); // 10
}

example();
```


### 재선언 문제

* 같은 스코프 내에서 같은 이름의 변수를 여러 번 선언할 수 있다. 이러한 점은 코드의 예측 가능성을 낮출 수 있음

```javascript
var z = 30;
var z = 40; // 재선언 가능
console.log(z); // 40
```

---

## 예상 질문

* 호이스팅이 뭔가요? 🔥🔥🔥🔥
* var 키워드의 문제점은 무엇이 있나요? 🔥🔥
* let 키워드는 var 키워드와 어떤 점이 다른가요? 🔥🔥🔥
* TDZ 🔥🔥🔥

