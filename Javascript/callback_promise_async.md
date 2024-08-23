# Callback / Promise / Async wait

자바스크립트는 동기 언어입니다. 따라서 호이스팅된 이후부터 코드가 나타나는 순서대로 자동적으로 나타나게 됩니다. 

따라서 비동기적으로 코드를 작성하기 위해 자바스크립트의 Promise, async wait와 브라우저 webAPI 등 이외의 도움들을 받습니다.

브라우저에서 지원하는 비동기 API는 DOM, Ajax, setTimeout이 있습니다. 

대표적으로 콜백함수를 이용한 setTimeout()에 대해 콜백이 뭔지 알아보겠습니다.

## Callback

여기서 콜백함수는 모두 비동기적으로 실행되는 것이 아닌 동기 또는 비동기적으로 실행될 수 있습니다. 

```jsx
console.log('1')
setTimeout(()=>console.log('2')
console.log('3')

1. Synchronous callback
function Synchronous(print) {
	print()
}
// 바로 실행되는 Synchronous 호출
Synchronous(()=>console.log('hello') // 1 3 hello 2

2. Asynchronous callback
function Asynchronous(print, timeout) {
	setTimeout(print, timeout)
}
// 언제 실행될지 모르는 Asynchronous 호출
Asynchronous(()=> console.log('async callback'), 2000) // 1 3 hello 2 async callback
```

예를 들어 비동기 처리를 하지 않고 콜백으로만 로그인을 수행할 때 클라이언트 측의 코드를 생각해봅시다. 

ex) 콜백 지옥… 😱

```jsx
const userStorage = new UserStorage();
const id = prompt('id: ')
const pw = prompt('pw: ')
userStorage.loginUser(
	id,
	pw,
	user => {
		userStorage.getRoles(
			user,
			userWithRole => {
				alert(
					`Hello ${userWithRole.name}, you have a ${userWithRole.role} role`
				);
			},
			error => {
				console.log(error);
			}
		};
	},
error => {
		console.log(error);
}
```

로그인을 하게 되면 로그인에 대한 정보와 유저에 대한 역할 정보를 확인하고 요청하는 과정에서 함수를 호출하고, 콜백 함수가 쓰입니다. 만약 그 안에 또 다른 콜백 함수가 쓰이는 과정이 반복적으로 일어나다보면 콜백 지옥의 문제점이 발생하겠죠?

가독성 뿐만 아니라 비지니스 로직을 한 눈에 파악하기도 어려울 겁니다. 또 에러가 발생하거나 디버깅, 유지보수를 하는 경우에도 어렵습니다.

병렬적으로, 또 효율적으로 네트워크 통신을 하기 위해서는 비동기를 사용하여 코드를 깔끔하게 짤 수 있습니다.

---

## Promise

Promise는 자바스크립트 ES6부터 제공하는 비동기를 간편하게 처리할 수 있는 오브젝트입니다. 비동기 처리 여부와 관련하여 `resolve`와 `reject`라는 `콜백함수`를 받는 실행함수를 서버에 전달합니다.

두 가지 포인트에 맞춰서 개념을 알고있으면 좋습니다.

### 1. state 성공/실패 상태

pending (promise 수행중인 상태) 

→ fulfilled (성공적으로 마친 상태) or rejected(파일이 없거나 네트워크 문제가 생긴 상태)

### 2. Producer 데이터 제공자 vs Consumer 데이터 소비자

```jsx
1. Producer
const promise = new Promise((resolve, reject) => {
	// heavy work(network, read files)
	console.log('doing somthing..')
	setTimeout(()=> {
			if (err) {
         reject(new Error('no network'))
      } else {
         resolve('success')
      }
		}, 2000);
});

2. Consumers: then, catch, finally
promise.then((value) => { // resolve('success')
	console.log(value) 
})
.catch(error=> { // reject(new Error())
	console.log(error) 
})
.finally(()=> {
	console.log('finally')
})	
```

Producer 부분에서 새로운 **promise를 만들면**, **자동적으로 함수가 바로 실행되는 특징**이 있습니다. 따라서 만약 네트워크 요청을 사용자가 요청했을 때만(예를 들어 버튼을 눌렀을 때만) 수행해야한다면, 불필요한 네트워크 처리가 이뤄질 수도 있다는 점을 유념해야합니다. 

위의 코드에서 2초동안 네트워크 통신이 이뤄진다고 가정하고,

2초 후에 Producer부분에서 promise가 성공적으로 처리되면 resolve를 반환하며 Consumer부분에서 promise.then 구문이 실행됩니다. 반대로 Producer 부분에서 성공적으로 처리되지 않으면 reject를 반환하고  Consumer에서 .catch 구문이 실행됩니다. 또한, 성공/실패 여부에 상관없이 finally구문은 실행됩니다.

## Promise chaning

promise를 사용할 때 .then에서 값 뿐만아니라 또 다른 promise를 전달해서 다른 비동기 방식을 묶어서 처리할 수도 있습니다. 이를 Promise chaning이라고 합니다.

```jsx
const fetchNumber = new Promise((resolve, reject)=> {
	setTimeout(()=> resolve(1), 1000))
})

fetchNumber
	.then(num=> num*2)
	.then(num=> num*3)
	.then(num=> {
		return new Promise((resolve, reject) => {
			setTimeout(()=>resolve(num-1), 1000)
			})
		})
		.then(num=> console.log(num))
```

### Error Handing

```jsx
const getChicken = () => {
	new Promise((resolve, reject)=>{
		setTimeout(()=>resolve('🐓'), 1000)
	});
const getEgg = (chicken) => {
	new Promise((resolve, reject)=>{
		setTimeout(()=>resolve(`${chicken}=> 🥚`), 1000)
	});
const cook = (egg) => {
	new Promise((resolve, reject)=>{
		setTimeout(()=>resolve(`${egg}=> 🍳`), 1000)
	});

getChicken()
	.then(getEgg)
	.catch(error=> {
		return '🥨' // 🥨 => 🍳
	})
	.then(cook)
	.then(console.log); // 🐓=> 🥚=> 🍳
	.catch(console.log)
```

3개의 promise를 반환하는 함수가 있고, 체이닝을 통해서 처리한다고 했을 때 에러처리를 잘해줘야 합니다. 만약 getEgg 함수 부분에서 어떠한 에러로 reject가 된다면, catch문을 통해 🥨 => 🍳가 출력될 것입니다. 이처럼 체이닝 중에서 어디에서 어떻게 에러처리를 해줘야 하는지가 중요합니다. 

이와 같은 체이닝 형태는 콜백지옥을 연상시키기도 합니다. 

---

## Async - awiat

앞서 Promise에서 체이닝을 할 수 있다고 했었는데, async-await은 체이닝된 Promise를 좀 더 간결하고 동기적으로 실행되는 것처럼 보이게 만들어줍니다. (동기식으로 코드를 작성하는 것처럼 보임)

Promise를 유지해서 사용해야 맞는 것이 있고, async-await으로 변환해야 좀 더 깔끔해지는 경우가 있습니다.

async 키워드를 함수 앞에 붙이면 Promise를 사용하지 않아도 자동적으로 promise로 변환이 되어집니다. (async는 promise를 좀 더 간편하게 사용할 수 있는 syntactic suger이당 🍬)

Promise와 Async는 같은 기능을 하지만, 코드에서는 어떤 차이점이 있는지 살펴봅시다.

```jsx
0. Promise
function fetchUser() {
	return new Promise((resolve, reject) => {
		// do network request in 10sec
		return 'success'
		})
};

1. **Async**
async function fetchUser() {
	// do network request in 10sec
	return 'success'
}

const user=fetchUser()
user.then(console.log)

// 정해진 ms가 지나면 resolve를 호출하는 promise를 리턴하는 function
function delay(ms) {
	return new Promise(resolve => setTimeout(resolve, ms))
}
0. promise 방식
// promise가 resolve되었을 때 'a'를 반환하는 promise 방식
function getA() {
	return delay(3000)
	.then(()=>'a')
}

1. async-await 방식 
// 동기적인 코드 형태처럼 실행할 수 있다.
async function getA) {
	await delay(3000)
	return 'a';
}
async function getB() {
	await delay(3000)
	return 'b';
}

// promise 방식으로 출력
function getAB() {
	return getA().then(a=>{
		return getB().then(b=>`${a}+${b}`)
	})
}

// async-await 방식으로 출력
async function getAB() {
	try {
		const a = await getA()
		const b = await getB()
	} catch() {
	}
	return `${a}+${b}`
}
getAB().then(console.log)
```

위 코드에서 볼 수 있듯이 promise 방식으로 처리할 때 체이닝이 이뤄지고, async-await 방식으로 처리할 때는 좀 더 깔끔한 코드로 비동기 처리를 해줄 수 있습니다. 

### async-await의 병렬처리 문제점

여기서 a,b을 받아오는데에는 각각 3초씩 걸려서 총 6초가 걸립니다. 하지만 a, b는 서로 연관이 되어있지 않기 때문에 await을 통해 순차적으로 진행하는 건 비효율적일 것입니다. 

이러한 문제점을 해결하기 위해서 1차적으로 promise로 함수로 바로 실행하여 병렬적인 처리가 이뤄지도록 하면 됩니다. 

따라서 **이런식으로 한꺼번에 요청을 받아오고 싶을 때(동시다발적으로 병렬적으로 처리하고 싶을 때)** **Promise API**를 유용하게 사용할 수 있습니다. 

## Promise API

**1. Promise.all API**

```jsx
async function getA) {
	await delay(3000)
	return 'a';
}
async function getB() {
	await delay(3000)
	return 'b';
}

function getAll() {
	return Promise.all([getA(), getB()]).then(ab=>ab.join(' + '))
}
getAll().then(console.log) // a + b
```

promise를 사용하여 바로 함수를 호출하고 병렬적으로 동시에 처리해주는 것입니다. 결과는 똑같이 6초가 걸리겠지만, 내부적으로 **병렬처리**가 된다는 점에서 차이가 있습니다. 

**2. Promise.race API**

```jsx
async function getA) {
	await delay(3000)
	return 'a';
}
async function getB() {
	await delay(1000)
	return 'b';
}

function getOnlyOne() {
	return Promise.race([getA(), getB()])
}
getOnlyOne().then(console.log) // b
```

Promise.race API를 사용하면 **가장 먼저 값을 수행**하는 인자를 반환합니다. a는 3초 b는 1초가 걸리기 때문에 b를 반환하게 되는 것입니다.

---

그럼 맨 앞부분에서 봤던 콜백 지옥 예시에서 다시 살펴봅시다.

promise를 사용하여 콜백해본다면?

```jsx
const userStorage = new UserStorage();
const id = prompt('id: ')
const pw = prompt('pw: ')
userStorage.loginUser(id,pw)
	.then(userStorage.getRoles)
	.then(user => alert(`Hello ${user.name}, you have a ${usser.role} role`)
	.catch(console.log(error);
```

아직까지도 뭔가 콜백 지옥처럼 보이는 것 같지요..??

async를 사용하여 콜백해본다면?

```jsx
const userStorage = new UserStorage();
const id = prompt('id: ')
const pw = prompt('pw: ')
async function checkUser() {
	try{
		const userId = userStorage.loginUser(id,pw)
		const user = userStorage.getRoles(userId)
		alert(`Hello ${user.name}, you have a ${usser.role} role`)
	} catch(error) {
		console.log(error)
	};
checkUser();
```

편-안

<aside>
💡 Promise나 async/await와 같은 문법을 사용하는 이유는 이런 비동기 처리의 흐름을 좀 더 명확하게 인지하고자 하는 노력인 것이다.

</aside>

<br />

하지만 앞서 말했듯이 무조건 async를 쓰는 것만이 정답이 아니라, 

어떻게 호출되는지에 따라서 더 적합한 비동기 처리 방식이 있는 것이고, 용도에 맞춰서 적절히 사용해야 합니다. 

콜백 방식은 별 다른 키워드 없이도 단순하게 구현할 수 있는 문법이기 때문에, 콜백 지옥을 맞이할 정도의 복잡한 상황이 아니라면 오히려 사용하기 가독성이 좋습니다. 

대표적인 예로, Node.js의 Express 프레임워크는 서버 라우팅을 콜백 함수로 처리하는 방식을 제공합니다.

```jsx
const express = require("express"); // Express 모듈 불러오기
const app = express(); // Express 앱 객체 생성

// /home url 경로에 GET 요청이 들어오면 이에 대한 라우팅 정의
app.get("/home", function (req, res) {
  // 응답 보내기
  res.send("Hello, Express!");
});
```

<br />

<aside>
💡 콜백 함수는 복잡하기 않고 비교적 심플한 비동기 작업을 처리해야 할 때 사용 good
  
Promise 객체는 비교적 복잡한 비동기 작업을 처리할 때 사용 good

</aside>
