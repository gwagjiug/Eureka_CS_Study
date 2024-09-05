## JavaScript === dynamically typed language

## 자바스크립트 배열 vs 자료구조 배열

|  | 자료구조 배열 | 자바스크립트 배열 |
| --- | --- | --- |
| 개념 | **밀집 배열 (dense array)** | **희소 배열 (sparse array)** |
| 동적 크기 | 대부분 고정 크기 | 동적으로 크기가 조절 |
| 요소 타입 | 보통 동일한 타입의 요소만 저장 | 다양한 타입의 요소를 함께 저장 가능 |
| 메모리 할당 | 연속된 메모리 공간에 요소를 저장 | 실제로는 객체로 구현되어 있어, 메모리 사용 방식의 차이가 있을 수도 있음 (동적 타이핑 특성 때문) |
| 인덱싱 | 0부터 시작하는 정수 인덱스만 사용 | 정수 인덱스 외에도 문자열 키를 사용 가능 (권장되진 않음) |
| 성능 | 일반적으로 요소 접근이 매우 빠름 | 객체 기반이므로 자료구조 배열보다 접근 속도가 느릴 수 있음. |
| 메서드 |  프로그래밍 언어와 구현 방식에 따라 다름 | 자바스크립트 배열의 객체 특성 때문에 push(), pop(), map(), filter() 등 다양한 내장 메서드를 제공 |
- 배열의 요소를 위한 각각의 메모리 공간은 동일한 크기를 갖지 않아도 되며 연속적으로 이어져 있지 않을 수도 있다. 배열의 요소가 연속적으로 이어져 있지 않는 배열을 **희소 배열(sparse array)**이라 한다.
- 자바스크립트의 배열은 자료구조의 배열보다 더 유연하고 다양한 기능을 제공하지만, 일부 성능 측면에서는 차이가 있을 수 있고, 엄밀히 말해 일반적 의미의 배열이 아니다. 자바스크립트 배열은 실제로는 '배열과 유사한 객체'에 가깝다고 볼 수 있다.
- 이처럼 인덱스로 배열 요소에 접근할 때 일반적인 배열보다 느릴 수 밖에 없는 구조적인 단점을 보완하기 위해 대부분의 모던 자바스크립트 엔진은 배열을 일반 객체와 구별하여 보다 배열처럼 동작하도록 최적화하여 구현하였다.
    - 아래와 같이 배열과 일반 객체의 성능을 테스트 해보면 배열이 일반 객체보다 약 2배 정도 빠른 것을 알 수 있다.
    
    ```jsx
    const arr = [];
    
    console.time('Array Performance Test');
    
    for (let i = 0; i < 10000000; i++) {
      arr[i] = i;
    }
    console.timeEnd('Array Performance Test');
    // 약 340ms
    
    const obj = {};
    
    console.time('Object Performance Test');
    
    for (let i = 0; i < 10000000; i++) {
      obj[i] = i;
    }
    
    console.timeEnd('Object Performance Test');
    // 약 600ms
    
    ```
    

## **배열 리터럴**

배열은 Array 생성자로 생성된 Array 타입의 객체이며 프로토타입 객체는 Array.prototype이다.

`cosnt arr = [];` 배열 리터럴 `arr`의 프로토타입 객체는 `Array.prototype`

`const obj = {};` 객체 리터럴 `obj`의 프로토타입 객체는 `Object.prototype`

![image](https://github.com/user-attachments/assets/0c065ebc-3e01-495f-a15f-ad47e830a0e8)


# **Array Method**

*lib.es5.d.ts - TypeScript의 기본 라이브러리 정의파일*

- ✏️ 메소드는 `this`(원본 배열)를 변경한다.
- 🔒 메소드는 `this`(원본 배열)를 변경하지 않는다.

### Array → String

**join 🔒**

> join(separator?: string): string;
> 
- 배열을 문자열로 변환
- 구분자가 들어가도 되고, 안들어가도 된다.

```jsx
const s = ['a', 'b']
console.log(s.join()) // a,b
console.log(s.join(’, ’)) // a, b
console.log(s.join(’ | and ‘)) // a | and b
```

### String → Array

**split**

> split(separator: string | RegExp, limit?: number): string[];
> 
- 문자열을 배열로 변환
- 구분자를 설정하지 않게 되면, 하나의 배열로 들어가게 된다.
- 최대 몇개까지 반환할지 정하는 limit은 옵션

```jsx
const s = 'a, b'
console.log(s.split()) // [’a, b’] -> 구분자를 설정하지 않게 되면, 하나의 배열로 들어가게 된다.
console.log(s.split(',')) // [’a’, ‘b’]
console.log(s.split(',', 1)) // [’a’] -> 최대 몇개까지 반환할지 정하는 limit은 옵션
```

### remove API

**splice() ✏️**

> splice(start: number, deleteCount?: number): T[];
> 

```jsx
const arr = [1,2,3,4,5]
const result = arr.splice(0, 2)
console.log(result) // [3,4,5]
console.log(arr) // [3,4,5]
```

- 배열의  기존 요소를 삭제 또는 교체하거나 새 요소를 추가하여 배열의 내용을 변경한다.
- 즉, 원본 배열 자체를 수정한다.

### Copy API

**slice() 🔒**

> slice(start?: number, end?: number): T[];
> 

```jsx
const arr = [1,2,3,4,5]
const result = arr.slice(2, 5)
console.log(result) // [3,4,5]
console.log(arr) // [1,2,3,4,5]
```

```jsx
// 실제 코드에 적용 예
setCasts(response.cast.slice(0, 3)); // 영화 등장인물의 3명까지만 배열로 반환한다
```

- 배열에서 원하는 부분만 반환
- start부터 end전까지의 일부분을 그대로 복사한다.
- 즉, 원본 배열을 수정되지 않는다.

### combine two arrays API

**concat() 🔒**

> concat(...items: (T | ConcatArray<T>)[]): T[];
> 

두 개의 배열을 합쳐서 새로운 배열을 만든다.

```tsx
const fruits = ['사과', '딸기']
const fruits2 = ['수박', '참외']
const newFruits = fruits.concat(fruits2)
console.log(newFruits) // ['사과', '딸기','수박', '참외']
```

```
// 자식 컴포넌트로부터 추가된 댓글 업데이트 받기 
const refreshFunction = (newComment) => {
  setCommentLists(commentLists.concat(newComment))
}
```

### Searching API

```tsx
const fruits = ['사과', '딸기','수박', '참외', '사과']

console.log(fruits.indexOf('사과')) // 0
console.log(fruits.lastIndexOf('사과')) // 4
console.log(fruits.includes('사과')) // true
```

1. **indexOf() 🔒**: 인덱스 위치를 반환, 데이터가 없으면 -1 반환
2. **lastIndexOf() 🔒**: 똑같은 요소가 중복될 때, 마지막에 들어있는 데이터 인덱스 위치를 반환
3. **includes() [ES7] 🔒**: 배열에 존재하는 데이터인지 boolean값으로 반환

---

```jsx
const students = [
  new Student('A', 29, true, 45),
  new Student('B', 28, false, 80),
  new Student('C', 30, true, 90),
  new Student('D', 40, false, 66),
  new Student('E', 18, true, 88),
];
```

**find() 🔒**

- 배열 요소마다 하나씩 콜백함수가 호출되면서 해당하는 배열을 리턴

```jsx
console.log(students.find((student) => student.score===90))
// Student {name: "C", age: 30, enrolled: true, score: 90 }
```

**some() 🔒**

> some(predicate: (value: T, index: number, array: T[]) => unknown, thisArg?: any): boolean;
> 
- 배열에서 하나라도 콜백함수의 조건에 맞는 값이 있으면 true
- 하나의 조건만이라도 만족해도 될 때 사용

```jsx
// 50점 이하의 학생이 한명이라도 있다면 true
students.some((student) => student.score < 50)
```

**every() 🔒**

> every(predicate: (value: T, index: number, array: T[]) => unknown, thisArg?: any): boolean;
> 
- 모든 요소들이 콜백함수의 조건에 성립해야지만 true
- 모든 조건이 만족해야될 때 사용

```jsx
// 모든 학생이 50점 이상이라면 true
!students.every((student) => student.score >= 50)
```

**reduce() 🔒**

> reduce(callbackfn: (previousValue: T, currentValue: T, currentIndex: number, array: T[]) => T, initialValue: T): T;
> 
- 콜백함수를 전달하고, initialValue를 전달할 수도 있다.
- 콜백함수에서 리턴되는 값은 누적된 결과 값을 반환한다.
- 원하는 시작점부터 배열에 있는 모든 요소들의 값을 돌면서 누적한다.
- prev → 이전에 콜백함수에서 리턴된 값이 전달되어 온다.
- curr → 배열의 아이템을 순차적으로 받아온다.

```jsx
// 전체 score 합산
const result = scores.reduce((prev, curr) => prev + curr.score, 0)

// score의 평균
console.log(result / scores.length)
```

**filter() 🔒**

> filter(predicate: (value: T, index: number, array: T[]) => unknown, thisArg?: any): T[];
> 
- 원하는 값만 받아올 수 있다.

```jsx
// 등록된 학생들만 출력
console.log(students.filter((student) => student.enrolled))
// Student {name: "A", age: 29, enrolled: true, score: 45 }
// Student {name: "C", age: 30, enrolled: true, score: 90 }
// Student {name: "E", age: 18, enrolled: true, score: 88 }
```

- Structure
    1. 매개변수
    - predicate → 첫 번째 매개변수로, 콜백함수이다
    - thisArg? → 두 번째 매개변수로, 선택적이다
    
    1. predicate 함수의 구조
    - (value: T, index: number, array: T[]) ⇒ unknown
        - value: T → 현재 처리중인 배열 요소
        - index: number → 현재 요소의 인덱스
        - array: T[] → 필터링 중인 원본 배열
        - ⇒ unknown → 반환 타입이 unknown임을 나타냄
    
    1.  thisArg?: any
    - predicate 함수 내에서 this로 사용될 객체이다.
    - 선택적 매개변수이며, 타입은 any이다.
    
    1. : T[] 
    - 메소드의 반환타입으로, 원본 배열과 같은 타입의 요소를 가진 새 배열을 반환한다.
    - predicate 함수가 unknown을 반환하므로, 어떤 조건이든 사용할 수 있지만, 결과 배열의 타입은 원래 배열과 동일
    
    ```jsx
    // 실제 적용 코드 (댓글 삭제)
    const updatedComments = commentLists.filter(
      (comment) => comment._id !== props.comment._id
    );
    ```
    
    1. 매개변수
    - commentLists: 필터링할 원본 배열이다. 이 배열에 filter 메서드가 적용된다.
    1. predicate 함수의 구조
    - (comment) => comment._id !== props.comment._id
        - 매개변수: comment (배열의 각 요소를 나타냄)
        - 화살표 함수 구문 사용
        - 반환값: comment._id !== props.comment._id의 boolean 결과
    
    3. predicate 함수의 동작
    
    - 각 comment 객체의 _id를 props.comment._id와 비교한다.
    - 두 값이 다를 경우 true를 반환하고, 같을 경우 false를 반환한다.
    
    ⇒ 이 filter 연산은 props.comment._id와 일치하지 않는 모든 댓글을 새 배열에 포함시킨다. 
    
    즉, 특정 댓글을 제외한 나머지 모든 댓글을 선택하는 역할을 한다. 
    
    결과적으로 updatedComments는 원본 props.commentLists에서 props.comment._id와 일치하는 댓글을 제외한 새로운 배열이 된다.
    

**map() 🔒**

> map<U>(callbackfn: (value: T, index: number, array: T[]) => U, thisArg?: any): U[];
> 
- 배열 안에 들어있는 각 요소들을 변환하여 새로운 배열을 만든다.
- 지정된 콜백함수를 거쳐서 다시 새로운 값으로 반환된다.
    - U[] ⇒ 각 요소의 타입을 변경할 수 있다.
    - T[] ⇒ 요소의 타입을 변경하지 않는다.
- 따라서 U[]를 반환하여 새로운 타입의 배열을 생성한다.

```jsx
// 학생들 점수 출력
console.log(students.map((student) => student.score))
// [45, 80, 90, 66, 88]
```

- Structure
    1. map<U> → 메소드 이름과 제네릭 타입 매개변수 U를 선언
    
    1. 매개변수
    - callbackfn → 첫 번째 매개변수로, 콜백 함수이다.
    - thisArg? → 두 번째 매개변수로, 선택적이다.
    
    1. callbackfn 함수의 구조
    - (value: T, index: number, array: T[]) => U
        - value: T → 현재 처리중인 배열 요소  (T는 배열의 요소 타입)
        - index: number → 현재 요소의 인덱스
        - array: T[] → 매핑 중인 원본 배열
        - ⇒ U → 반환 타입이 U임을 나타냄 (U는 새로운 배열의 요소 타입)
    
    1. thisArg?: any:
    - callbackfn 함수 내에서 this로 사용될 객체
    - 선택적 매개변수이며, 타입은 any
    
    1. : U[] → 메소드의 반환 타입으로, U 타입의 요소를 가진 새 배열을 반환한다.

**forEach() ✏️?**

> forEach(callbackfn: (value: T, index: number, array: T[]) => void, thisArg?: any): void;
> 
- 단순 반복문 (반환값 없음)
- 배열의 요소마다 콜백함수를 수행한다.

```jsx
// 학생들 점수 출력
students.forEach((student) => console.log(student.score));

// 45
// 80
// 90
// 66
// 88
```

**forEach() vs map()**

| forEach() | map() |
| --- | --- |
| 항상 undefined 반환 | 새로운 배열 반환 |
| 메서드 체이닝에 적합 x | 다른 메서드와 체이닝 적합 |
