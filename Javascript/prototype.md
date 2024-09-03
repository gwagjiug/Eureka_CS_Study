### 프로토타입 객체

Java C++ 과 같은 클래스 기반 객체지향 프로그래밍 언어와 달리 자바스크립트는 프로토타입 기반 객체지향 프로그래밍 언어이다. 따라서 자바스크립트의 동작 원리를 이해하기 위해서는 프로토타입의 개념을 잘 이해하고 있어야 한다.

클래스 기반 객체지향 프로그래밍 언어는 객체 생성 이전에 클래스를 정의하고 이를 통해 객체를 생성한다.

하지만 프로토타입 기반 객체지향 프로그래밍 언어는 클래스 없이(Class-less)도 객체를 생성할 수 있다.

**그래서 프로토타입이란?** 자바스크립트에서 프로토타입이란 객체 지향 프로그래밍에서 상속 기능을 구현하는 방식 중 하나이다. 쉽게 말해, 객체의 부모 역할을 하는 다른 객체를 가리키는 링크라고 생각할 수 있다.

이 프로토타입을 통해 객체들은 다른 객체로부터 속성이나 메서드를 상속받아 사용할 수 있다.

객체를 생성할 때 자바스크립트는 해당 객체의 프로토타입을 자동으로 설정한다.

```javascript
function parents() {
  this.a = 'A';
  this.b = 'B';
}

console.log(parents.prototype);
console.log(parents);
```
![](https://velog.velcdn.com/images/gawgjiug/post/8f7c6812-67c9-4212-9a8d-03db260e703e/image.png)



위 사진을 보면 코드상에서 `parents` 객체에 prototype 이라는 속성을 선언한 적도 없는데 자동으로 prototype 이라는 객체를 가지고 있는 것을 볼 수 있다.

이를 통해 자바스크립트의 모든 객체는 자신의 부모 역할을 하는 담당 객체인 `Prototype` 객체와 연결되어있고, 

객체를 선언함과 동시에 `Parents.prototype` 이라는 빈 Object 가 어딘가에 존재하고, Parents 함수로부터 생성 된 모든 자식 객체들은 언제든지 어딘가에 존재하는 `Parents.prototype` 에 들어있는 값들 모두 가져다 사용할 수 있다.

예를 들어, 자바스크립트의 모든 배열은 Array.prototype 이라는 기본 프로토타입을 가지며, 이 프로토타입에는 배열 객체가 사용할 수 있는 다양한 메서드 `sort, push, pop` 가 정의되어 있다.

```javascript
const arr = new Array(4, 2, 1, 10);
//Array 객체의 자식인 arr 생성한 것과 마찬가지

arr.sort(); 
//arr.sort 메서드를 정의한적도 없는데 사용할 수 있는 이유?
//부모 객체인 Array의 유전자인 prototype 객체에 정의되어 있기 때문에 자식인 arr도 사용할 수 있는 것.

```

![](https://velog.velcdn.com/images/gawgjiug/post/d64ff1cc-bb5b-4b41-b832-ad21d71888e0/image.png)



### 프로토타입 체인

프로토타입 체인이란 자바스크립트에서 객체가 속성이나 메서드를 찾을 때 사용하는 검색 메커니즘이다. 객체에 해당 속성이나 메서드가 없으면 자바스크립트는 자동으로 프로토타입 체인을 따라가며, 부모 객체의 프로토타입을 검색한다. 이 체인을 따라 올라가면서 속성이나 메서드를 찾게되면 그 값을 반환하고, 최종적으로 찾지 못하면 `undefined` 를 반환한다.

```javascript
function parents() {
  this.a = 'A';
  this.b = 'B';
}

parents.prototype.name = 'gwag';

const child = new parents();

console.log(child.name);
```
![](https://velog.velcdn.com/images/gawgjiug/post/915a456b-cc27-43d1-a9ee-0209362dd980/image.png)


위 사진을 보면 코드상으로 child 객체에 사용자는 name 이라는 속성을 정의한 적이 없는데, name을 사용할 수 있는 것을 볼 수 있다.

이 경우가 프로토타입 체인과 연관되어있는 것인데. JS는 내부적으로 해당 코드를 우선적으로 child 객체가 name이라는 데이터를 가지고 있는지 확인하고, 없다면 childe의 부모의 `유전자(prototype)` 을 검색하고 없다면 계속해서 부모의 부모를 찾는 방식으로 데이터를 검색하게 된다.

이런식으로 Object의 속성이나 메서드를 찾을 때 사용하는 `메커니즘` 자체를 우리는 `Prototype Chain` 이라고 한다.

이러한 개념을 이해하면 이어질 `프로토타입을 이용한 상속` 에 관련 된 개념도 쉽게 이해할 수 있다.

### 그렇다면 이러한 메커니즘을 가능하게 해주는 것은 무엇일까?


![](https://velog.velcdn.com/images/gawgjiug/post/11d6266f-e793-4375-bbd9-bc6de5051668/image.png)

위 사진을 보게되면 우리가 함수를 정의하면, 함수만 생성되는 것이 아니라 자동으로 Prototype Object도 같이 생성이 된다고 앞서서 얘기했었는데, 

이렇게 생성 된 함수는 prototype 이라는 속성을 통해 Prototype Object 에 접근할 수 있게 된다. 

그리고 이러한 Prototype Object 는 앞서 말한 `프로토타입 체인` 메커니즘을 가능하게 만드는 `__proto__` 라는 속성을 기본으로 가지게 된다.

여기서 `__proto__`  은 Prototype Link 라고도 불리는데, `__proto__`  속성은 모든 객체가 빠짐없이 가지고 있는 속성을 얘기하고, `__proto__`  는 객체가 생성될 때 조상이었던 함수의 Prototype Object를 가리키게 된다.

그래서 우리가 위의 예제들 처럼 자식 객체들이 부모객체가 가지고 있는 유전자 즉, Prototype Object 안에 정의되어있는 모든 속성과 함수들을 사용할 수 있는 것이다.


![](https://velog.velcdn.com/images/gawgjiug/post/e0fdef04-bccd-4814-903d-b031f30a05dd/image.png)


```javascript
function parents() {
  this.a = 'A';
  this.b = 'B';
}

const child = new parents();

console.log(child.__proto__);
```
![](https://velog.velcdn.com/images/gawgjiug/post/8ef8044a-2157-4c1d-b473-dfa42b1bdc29/image.png)


그리고 추가적으로 `__proto__` 와 `prototype` 의 차이점에 대해 헷갈려하는 분들이 많은데, `prototype`은 생성자 함수의 속성이고 `__proto__` 는 객체 인스턴스의 속성이라는 점을 알고 있으면 프로토타입을 이해하는 데에 있어서 혼동을 겪지 않을 것이다.


```javascript

function Person() {
  this.name = 'John';
}

Person.prototype.greet = function() {
  console.log('Hello, ' + this.name);
};

const person1 = new Person();

console.log(person1.__proto__ === Person.prototype); // true
```

위 예제에서 person1 객체는 Person 함수로 생성된 것이며, person1.__proto__는 Person.prototype을 가리킵니다. 따라서 person1은 Person.prototype에 정의된 greet 메서드를 사용할 수 있습니다


### 요약

* 함수는 Prototype Object를 자동으로 생성합니다.
* 이 Prototype Object는 **prototype**이라는 속성으로 함수와 연결되어 있습니다.
* 객체 인스턴스는 생성자 함수의 **prototype**을 __proto__ 속성을 통해 참조합니다.


이 메커니즘을 통해 자바스크립트의 객체는 상속과 메서드 공유를 가능하게 합니다. prototype은 생성자 함수와 연결된 속성이고, __proto__는 생성된 객체가 부모 객체를 참조하는 링크 역할을 한다
