# Async-await 동작원리 (ft. Generator)

**Async-await은 ES8에서 도입 !**

그렇다면 구형 브라우저에서  동작하기 위해서 Babel을 통해 트랜스 파일링을 하게 된다. 

내부 동작 방식을 파악하기 위해서 [babel의 try it out](https://babeljs.io/repl#?browsers=&build=&builtIns=false&corejs=3.6&spec=false&loose=false&code_lz=GYVwdgxgLglg9mABAQwBQEpEG8IIM5wA2ApgHSFwDmqA5MjegL4BQzyeAnpIqJLAogBGGbM0TjEuMARLkqtQQEYGYicgDuyGFBQZV4qTLIVqNQQCYVLYemaGix-TQgrmQA&debug=false&forceAllTransforms=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&timeTravel=false&sourceType=module&lineWrap=true&presets=env&prettier=false&targets=&version=7.16.6&externalPlugins=&assumptions=%7B%7D)에서  트랜스파일링을 해보면 다음과 같은 결과를 얻을 수 있다.

<img width="983" alt="image" src="https://github.com/user-attachments/assets/4ba57e18-e253-4757-a431-55578c829d17">

async-await은 내부적으로 Generator와 Promise의 산물 !

***즉, async-await은 동작하기 위해 제너레이터 함수로 변경이 되어서 실행된다.***

### Generator란?

- 제너레이터는 코드 블록의 **실행을 일시 중지했**다가 **필요한 시점에 재개**할 수 있는 특수한 함수
- 일반 함수는 실행 결과로 반환값이 출력되지만, 제너레이터 함수는 실행 결과로  Generator 객체가 출력된다.

**Generator 객체란?**
<img width="936" alt="image" src="https://github.com/user-attachments/assets/bcc2426b-29a3-417a-9693-05784bcd4cc9">

- Generator 객체는 Iterator와 iterable 인터페이스를 준수한다.
    - Iterator란? next 메서드를 가지고, next 메서드는 iteratorResult 객체를 반환한다.
    - iteratorResult 객체란? **value(iterator가 반환하는 값)**와 **done(iterator의 끝남 여부를 알려주는 값)** 프로퍼티를 갖는 객체이다.

코드 관점

> next() → yield → next() → yield → … → next() → return
> 
- async-await 함수 ⇒ 제너레이터 함수로 변경된다.
- 제너레이터 함수가 실행될 때 yield를 기준으로 promise에서 then 메소드로 넘겨가면서 실행한다.
- 이때 then 메소드로 넘어간 뒷부분이 마이크로태스크큐로 들어가게 되면서 콜스택이 비워진 다음에 이어서 실행된다.

### Generator가 중단된 yield (중단점)를 어떻게 기억할까?
<img width="1266" alt="image" src="https://github.com/user-attachments/assets/6428d7aa-7eda-473b-97c8-263291d95d98">

- Generator 객체는 내부적으로 **GeneratorState**와 **GeneratorContext** slot을 가지고 있다.
- **GeneratorState** → Generator의 진행상태와 관련된 값이 저장된다.
- **GeneratorContext** → Generator 함수의 **실행 컨텍스트**가 저장된다.

**GeneratorState Type**

1. Suspended Start - Generator 함수를 실행했을 때의 상태
2. Executing - Generator 객체의 next 메서드를 실행한 시점의 상태
3. Suspended Yield - Generator 함수의 실행 도중 yield 키워드를 만난 시점의 상태
4. Completed - Generator 함수의 실행 도중 return 키워드를 만나거나 함수의 끝에 도달했을 때의 상태
<img width="1077" alt="image" src="https://github.com/user-attachments/assets/1934575b-868d-4e8a-9004-117ec7f4dd03">


**GeneratorContext 실행 컨텍스트**

실행 컨텍스트 스택에 해당 과정을 통해 Generator 함수의 실행 컨텍스트의 push와 pop이 일어난다.  Generator 함수 내부에 있는 모든 코드가 실행이 되면 Generator 객체의 상태는 Completed가 되고 실행 컨텍스트가 pop이 된다.

### Generator 비동기 처리 패턴

Generator + Promise → 비동기 처리 코드를 동기식 코드처럼 작성하는 패턴 

하지만 해당 패턴을 사용하기 위해서는 Generator를 연속적으로 실행하는 유틸 함수가 필요했다.

⇒ 더 편하게 동기식 코드처럼 작성하는 방법 없나?  ⇒ Async-await의 등장
<img width="1152" alt="image" src="https://github.com/user-attachments/assets/f84186b0-51ca-475e-9532-a8281b47666b">

- Async-await은 Generator를 사용한 비동기 처리 패턴보다 더 편리하게 비동기 처리를 하기 위해 등장했다.
