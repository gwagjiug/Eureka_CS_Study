### this는 무엇인가?
---

* 대부분의 경우 `tihs`의 값은 함수를 호출한 방법에 의해 결정된다.

* `this` 는 호출한 놈을 말한다.

* 기본적으로는 `this` 는 window 객체를 말한다.

* 예외의 경우 

1. 화살표 함수에서 `this` 가 조금 달라진다.

2. Strict Mode에서는 `this` 가 조금 달라진다.

#### this 

---
* 대부분의 경우 `this` 의 값은 함수를 호출한 방법 즉,  호출한 놈에 의해 결정된다.

```js
function printThis() {
  console.log(this);
}
printThis(); // window 객체

let person1 = {
  name: '김길동',
  whoIsThis: printThis,
};

let person2 = {
  name: '홍길동',
  whoIsThis: printThis,
};

let person3 = {
  name: '짐길동',
  whoIsThis: printThis,
};

person1.whoIsThis(); // person1 객체
person2.whoIsThis(); // person2 객체
person3.whoIsThis(); // person3 객체
```

#### bind 함수

* `bind` 함수로 `this`를 설정할 수 있다.

```js
function print() {
  console.log('this: ', this);
  console.log('this.fullname: ', this.fullname);
}

let person1 = {
  fullname: '홍길동',
};
let person2 = {
  fullname: '김길동',
};
let bindPrint = print.bind(person1); // person1 객체로 바인딩
bindPrint(); // person1
let bindPrint2 = bindPrint.bind(person2);
bindPrint2(); // person1! bind는 단 한번만 할 수 있다.
```


#### Arrow Function this

---

* 화살표 함수가 나오기 전 까지는 함수는 어떻게 호출되는지에 따라 자신의 `this` 값을 정의했다.

* 하지만 화살표 함수는 자신을 포함하고 있는 외부 Scope에서 this를 계승받는다.

```js
let person = {
  name: '곽지욱',
  printThis: function () {
    console.log(this) //person 객체
    setTimeout(() => {
      console.log(this); // person 객체
    });
  },
};
```
* 화살표 함수를 사용할 경우 나를 감싸고 있는 상위 스코프의 this를 계승받기 때문에 person 객체를 받는 것

```js
let person = {
	name: '곽지욱',
	printThis: () => {
		console.log(this); // window 객체 출력
	}
};
```

* 그렇기에 위 경우에서는 상위 스코프가 전역 스코프이기 때문에 window 객체를 출력하는 것

#### Strict Mode 

---

* 엄격 모드에서는 호출한 놈이 없을 경우 기본값을 `window`로 하지 않고 `undefined`로 한다.

```js
'use strict';
function printThis() {
	console.log(this);
}
printThis(); // undefined
```


#### 예상 질문

---
* this가 뭔가요? 🔥
* this 바인딩이란? 🔥
* this는 동적으로 바인딩이 된다고 하는데 바인딩되는 객체가 어떻게 다르나요?
