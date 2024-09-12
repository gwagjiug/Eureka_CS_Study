# 가비지 컬렉션

자바스크립트에서 가비지 컬렉션은 사용하지 않는 메모리를 자동으로 회수하여 메모리 누수를 방지하는 기능을 말한다.

자바스크립트는 개발자가 명시적으로 메모리를 할당하거나 해제하지 않는다. 필요하지 않게 된 객체를 자동으로 정리하는 방식으로 동작한다.

이 과정을 가비지 컬렉터(Garbage Collector)가 수행한다.

즉, 우리가 생성한 원시값, 객체, 함수 등 메모리를 차지 하는 모든 것들을 `쓸모가 없어지면` 찾아내 삭제하는 과정을 말한다.

그러면 왜?? 굳이 삭제할까??

> 가비지 컬렉터는 한정된 메모리 자원을 효율적으로 관리하기 위해 설계된 장치이다. 컴퓨터의 메모리는 유한하기 때문에 프로그램이 실행되면서 더 이상 필요하지 않은 데이터를 자동으로 정리하여 새로운 데이터의 공간을 확보하기 위함임



그렇다면 그 `쓸모가 없어짐` 이라는 기준은 어떻게 생길까??

# 가비지 컬렉션 기준

자바스크립트는 도달 가능성(reachability) 이라는 개념을 사용해 메모리 관리를 수행한다. 

여기서 `도달 가능한` 값은 쉽게 말해 어떻게든 접근하거나 사용할 수 있는 값을 의미한다.
그렇게 `도달 가능하다` 라고 판단 된 값은 메모리에서 삭제되지 않는다.

1. 아래의 소개되는 값들은 그 태생부터 `도달 가능` 하기 때문에 명백한 이유 없이는 삭제 되지 않는다.

* 현재 함수의 지역 변수와 매개변수

```javascript
function exampleFunction(param) {
    // param은 매개변수
    let localVar = "지역 변수입니다."; // localVar는 지역 변수
    
    console.log(param);  // 매개변수 참조 가능
    console.log(localVar);  // 지역 변수 참조 가능
}

exampleFunction("매개변수 값");
```
> 즉 **현재 실행 중인 함수의 지역 변수와 매개변수** 는 함수가 실행되는 동안에는
메모리에 남아 있고, 함수가 종료되면 더 이상 사용되지 않는 것으로 간주되어 가비지 컬렉터가 이를 처리할 수 있다. 그러나 함수가 실행 중인 동안에는 이 값들은 메모리에서 "도달 가능" 한 상태로 유지되어 삭제되지 않는다.

---

* 중첩 함수의 체인에 있는 함수에서 사용되는 변수와 매개변수

```javascript
function outerFunction(outerParam) {
    let outerVar = "외부 함수 변수";

    function innerFunction(innerParam) {
        let innerVar = "내부 함수 변수";
        
        // 외부 함수의 변수와 매개변수에 접근 가능
        console.log(outerVar);  // "외부 함수 변수"
        console.log(outerParam);  // 외부 함수의 매개변수 값 출력
        console.log(innerVar);  // "내부 함수 변수"
        console.log(innerParam);  // 내부 함수의 매개변수 값 출력
    }

    return innerFunction;
}

const myFunction = outerFunction("외부 매개변수 값");
myFunction("내부 매개변수 값");
```

> 중첩 함수의 체인에 있는 함수에서 사용되는 변수와 매개변수 라는 문징은 **중첩 함수** 구조에서 내부 함수가 외부 함수의 변수와 매개변수에 접근 가능하다는 것을 의미한다.

> 자바스크립트에서 함수는 렉시컬 스코프 규칙을 따르기 때문에, 내부 함수는 자신을 포함하는 외부 함수의 변수나 매개변수를 기억하고 사용할 수 있다.

* 전역 변수

* 기타 루트(root) 값

2. 루트가 참조하는 값이나 체이닝으로 루트에서 참조할 수 있는 값은 도달 가능한 값이 된다.

전역 변수에 객체가 저장되어 있다고 가정해보자. 이 객체의 프로퍼티가 또 다른 객체를 참조하고 있다면, 프로퍼티가 참조하는 객체는 `도달 가능한 값` 이 된다. 

### 예제

```javascript
let user ={
	name : 'John"
}
```


이 코드에서 전역 변수 "user"는 `{name : John}` 이라는 객체를 참조한다. 이러한 상태를 `도달 가능한 상태` 라고한다.

```javascript
user = null;
```

이제 `user` 더 이상 객체를 참조하지 않게 되고 결과적으로는 `{name : John}` 객체는 도달할 수 없는 상태가 되면서 가비지 컬렉터가 이를 인식하고 메모리에서 삭제 시킨다.

---

### 참조 두개 예제

그렇다면 한 객체를 여러 변수에서 참조하는 경우를 보자

```javascript
let user = {
    name: "John"
};

let admin = user;  // user를 admin으로 복사
```

이제 user와 admin 두 변수가 같은 객체를 참조하고 있다. `user` 를 `null`로 설정해도, `admin` 이 여전히 `{name : John}` 객체를 참조하므로, 객체는 `도달 가능한 상태` 로 인식된다.


이런 경우에는 두 전역변수의 참조를 모두 삭제해야 `{name : John}` 객체를 `도달 불가능` 상태로 만들 수 있다.

```javascript
user = null;  // user의 참조를 삭제
admin = null; // admin의 참조를 삭제
```

### 연결된 객체

더 복잡한 구조에서 객체들이 서로를 참조하는 경우를 보자 예를 들어 두 객체가 서로를 참조하는 관계를 생각해보자.

```javascript
function marry(man, woman) {
    woman.husband = man;
    man.wife = woman;
    return {
        father: man,
        mother: woman
    };
}

let family = marry({
    name: "John"
}, {
    name: "Ann"
});

```

이 예제 코드에서 `family` 객체는 `man` 과 `woman` 객체를 참조하고 있다. 그리고 또 `man`은 `woman` 을 `woman` 은 `man` 을 참조한다. 

즉 현재 모든 객체는 `도달 가능한` 상태이다.

![](https://velog.velcdn.com/images/gawgjiug/post/38060711-c411-4aa2-8443-21ba81def1ee/image.png)




```javascript
delete family.father;
delete family.mother.husband;
```
![](https://velog.velcdn.com/images/gawgjiug/post/e0dd5fe1-ed23-4ae4-b0bd-d5e99eb21998/image.png)


삭제한 두 개의 참조 중 하나만 지웠다면, 모든 객체는 여전히 `도달 가능한` 상태였겠지만, 참조 두개를 지우게 되면 John 으로 들어갈 수 있는 참조는 모두 사라져 John은 `도달 가능한` 상태에서 벗어나게 된다.

왜냐하면 외부로 나가는 참조 `wife`는 `도달 가능성` 에 영향을 주지 못하기 때문이다. 모든 객체는 외부에서 자신에게 들어오는 참조만이 `도달 가능성` 에 영향을 준다.

위 사진의 결과로 John은 이제 도달 가능한 상태가 아니기 때문에 메모리에서 제거된다.

**가비지 컬렉션 후 메모리 구조**

![](https://velog.velcdn.com/images/gawgjiug/post/9f84a5c1-2b35-4b34-b6e7-399aee4691a0/image.png)

> 루트 객체는 자바스크립트 엔진에서 기본적으로 접근할 수 있는 최상위 참조 가능한 객체를 말한다 브라우저 환경에서는 `window` Node.js 환경에서는 `global` 객체를 말한다.

> 전역 변수는 **스크립트 전체에서 접근할 수 있는 변수** 를 말한다. 즉, 전역 스코프에서 선언된 변수로 어디서든지 접근할 수 있는 것을 말한다.


### 도달할 수 없는 섬

위 사진들 처럼 객체들은 연결되어 마치 하나의 독립 된 섬 같은 구조를 만든다. 그리고 이 섬에 도달할 방법이 없는 경우, `들어오는 참조가 없는 경우` 섬을 구성하는 객체 전부가 메모리에서 삭제되는 것이다.

위 객체에서 `루트 객체` `family` 객체이다 정확하게 얘기하면 **루트 객체의 속성에 해당하는 전역 변수로 선언된 것** 즉, `family`는 **전역 변수**로 선언되어 있기 때문에 `window` 객체의 속성으로 간주된다.

```javascript
family = null;
```

이렇게 루트 객체인 `family` 가 아무것도 참조하지 않도록 하였다. 

![](https://velog.velcdn.com/images/gawgjiug/post/3ba6e34a-769e-4085-b07c-d7051a99f52f/image.png)

그렇다면 메모리 구조는 다음과 같을 것이다.

이 사진을 보면 가비지 컬렉션에서 `도달할 수 없는 섬` 예제는 `도달 가능성` 이라는 개념이 얼마나 중요한지 보여준다.

John 과 Ann은 여전히 서로를 참조하고 있고, 두 객체 모두 외부에서 들어오는 참조를 갖고 있지만.

앞서 말했 던 가비지 컬렉션의 기준인 `기타 루트(root) 값` 과 `루트가 참조하는 값이나 체이닝으로 루트에서 참조할 수 있는 값` 이라는 기준에 미치지 못하기 때문에 섬(객체) 전체가 도달할 수 없는 사태가 되고, 섬을 구성하는 객체 전부가 메모리에서 제거 되는 것이다.


---

## 요약

* `도달 가능한 객체` 는 메모리에서 삭제되지 않으며, 프로그램이 실행되는 동안 참조가 유지된다.

* `참조 삭제` 는 객체가 더 이상 도달할 수 없는 상태가 되도록 만든다.

* `객체 간의 참조` 는 복잡한 구조를 만들 수 있고, 이들 객체가 외부에서 참조되지 않으면 메모리에서 제거된다.


[출처: 모던 javascript 튜토리얼](https://ko.javascript.info/garbage-collection#ref-712)
