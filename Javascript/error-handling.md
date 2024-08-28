## 에러 처리의 필요성

- 발생한 에러에 대해 대처하지 않으면 프로그램은 강제 종료된다
- `try ... catch` 문을 사용하면 강제 종료되지 않고 계속해서 코드를 실행시킬 수 있다

## 2. try ... catch ... finally

- 에러 처리를 구현하는 기본적인 방법 2가지
  - 예외적인 상황이 발생하면 반환하는 값(`null` , -1 등)을 if문이나 옵셔널 체이닝 연산자 등을 이용
  - 에러 처리 코드를 미리 등록해 두고 에러가 발생하면 에러 처리 코드로 점프하도록 구현
    - try catch finally 문도 여기에 해당
      ```jsx
      try {
        // 실행할 코드 (에러 발생 가능성이 있는 코드)
      } catch (err) {
        // try 코드 블록에서 에러가 발생하면 이 코드 블록의 코드가 실행된다
        // err에는 try 코드 블록에서 발생한 Error 객체가 전달된다
      } finally {
        // 에러 발생과 상관없이 반드시 한 번 실행된다
      }
      ```

## 3. Error 객체

- `Error` 생성자 함수에는 에러를 상세히 설명하는 에러 메시지를 인수로 전달할 수 있다
  ```jsx
  const error = new Error('invalid');
  ```
- `Error` 생성자 함수가 생성한 에러 객체는 `message` 프로퍼티와 `stack` 프로퍼티를 갖는다
  - `message` Error 생성자 함수에 인수로 전달한 에러 메시지
  - `stack` 에러를 발생시킨 콜스택의 호출 정보를 나타내는 문자열 (디버깅 목적으로 사용)
- `Error` 생성자 함수의 종류
  | 생성자 함수 | 인스턴스 |
  | -------------- | ------------------------------------------------------------------------------ |
  | Error | 일반적인 에러 객체 |
  | SyntaxError | 자바스크립트 문법에 맞지 않는 문을 해석할 때 발생하는 에러 객체 |
  | ReferenceError | 참조할 수 없는 식별자를 참조했을 때 발생하는 에러 객체 |
  | TypeError | 피연산자 또는 인수의 데이터 타입이 유효하지 않을 때 발생하는 에러 객체 |
  | RangeError | 숫자값의 허용 범위를 벗어났을 때 발생하는 에러 객체 |
  | URIError | encodeURI 또는 decodeURI 함수에 부적절한 인수를 전달했을 때 발생하는 에러 객체 |
  | EvalError | eval 함수에서 발생하는 에러 객체 |

## 4. throw 문

- 에러 객체를 생성한다고 해서 에러가 발생하는 것은 아니다
- 에러를 발생시키려면 `try` 코드 블록에서 `throw` 문으로 에러 객체를 던져야 한다
- 에러를 던지면 `catch` 문의 에러 변수가 생성되고, 던져진 에러 객체가 할당된다
- 그리고 `catch` 코드 블록이 실행되기 시작한다

```jsx
try {
  // 에러 객체를 던지면 catch 코드 블록이 실행되기 시작한다
  throw new Error('something wrong');
} catch (error) {
  console.log(error);
}
```

## 5. 에러의 전파

- 에러는 호출자(caller) 방향으로 전파된다
  - 즉, 콜 스택의 아래 방향(실행 중인 실행 컨텍스트가 푸시되기 직전에 푸시된 실행 컨텍스트 방향)으로 전파된다

```jsx
const foo = () => {
  throw Error('foo에서 발생한 에러');
};

const bar = () => {
  foo();
};

const baz = () => {
  bar();
};

try {
  baz();
} catch (err) {
  console.error(err);
}
```

- 위의 코드에서 `throw` 된 에러를 캐치하지 않으면 호출자 방향으로 전파된다
- `throw` 된 에러를 어디에서도 캐치하지 않으면 프로그램은 강제 종료된다
- 비동기 함수인 `setTimeout` 이나 프로미스 후속 처리 메서드의 콜백 함수는 호출자가 없다
  - 이들은 태스크 큐나 마이크로태스크 큐에 일시 저장되었다가 콜 스택이 비면 이벤트 루프에 의해 콜 스택으로 푸시되어 실행된다
  - 이때 콜 스택에 푸시된 콜백 함수의 실행 컨텍스트는 콜 스택의 가장 하부에 존재하게 된다 → 따라서 에러를 전파할 호출자가 존재하지 않는다

---

# axios에서 커스텀 에러 정의하기

보통 비동기 통신 과정에서 발생하는 에러는 상태 코드로 우선 처리하는 경우가 많습니다.

- `10x` 정보 확인
- `20x` 통신 성공
- `30x` 리다이렉트
- `40x` 클라이언트 오류
- `50x` 서버 오류

또 각각이 세부적으로 나누어져 있어 상태 코드만 보고도 어떠한 에러인지 파악할 수 있습니다.
하지만 프로젝트를 진행하다 보면, 팀 고유의 에러 코드와 에러 메시지, 상세 설명 등을 추가하여 에러에 대한 구체적인 설명을 부여하기도 합니다.

```json
{
  "status": 404,
  "error_code": -4024,
  "error_message": "INVALID_ROOM_CODE",
  "description": "유효하지 않은 모임 코드입니다."
}
```

이와 같은 경우 기본적으로 제공하는 `Error` 객체와 `AxiosError` 로는 처리가 부족할 수 있기 때문에 axios에서는 에러를 커스텀 할 수 있는 기능을 제공합니다.

## 커스템 에러 생성하기

먼저 TypeScript를 사용한다 가정하고 커스텀 에러 객체에 대한 타입을 정의합니다.

```javascript
import { AxiosError, AxiosResponse } from 'axios';

// CustomErrorResponse 인터페이스 정의
export interface CustomErrorResponse {
  status: number;
  errorCode: number;
  errorMessage: string;
  description: string;
}

// CustomError 인터페이스 정의
export interface CustomError extends AxiosError<CustomErrorResponse> {
  response: AxiosResponse<CustomErrorResponse>;
  status: number;
  errorCode: number;
  errorMessage: string;
  description: string;
}
```

그 다음 axios를 이용하여 받은 response를 바탕으로 에러 객체를 생성합니다. 아래 `createCustomError` 함수의 리턴값이 `CustomErrorResponse`가 되어 에러가 발생한 상황에 대해 구체적인 상태값을 확인할 수 있습니다.

```javascript
function createCustomError(error: AxiosError): AxiosError | CustomErrorResponse {
  const customError = error as CustomError;

  if (customError.response) {
    customError.status = customError.response.status;
    customError.errorCode = customError.response.data.errorCode;
    customError.errorMessage = customError.response.data.errorMessage ?? '';
    customError.description = customError.response.data.description ?? '';
  }

  return customError;
}
```

이제 axios에서 직접 사용해 봅시다. axios를 사용하여 response로 받은 응답값을 `createCustomError`에 넘겨 `CustomErrorResponse` 객체를 받아야 하기 때문에 axios에서 제공하는 response interceptor를 사용해야 합니다.

```javascript
import axios, { AxiosResponse } from 'axios';

// Axios 인스턴스 생성
export const axiosInstance = axios.create({
  baseURL: 'https://api.example.com',
});

// Response Interceptor 설정
axiosInstance.interceptors.response.use(
  (response: AxiosResponse) => response,
  (error: AxiosError) => Promise.reject(createCustomError(error));
);
```

이제 api를 호출할 때 만들어 둔 axios instance를 이용하여 아래와 같이 사용할 수 있습니다.

```javascript
async function fetchData() {
  try {
    const response = await axiosInstance.get('/some-endpoint');
    return response.data;
  } catch (error: unknown) {
    // CustomError라면
    if ('status' in error && 'errorCode' in error) {
      console.error('Error Message:', error.errorMessage);
      console.error('Error Code:', error.errorCode);
      console.error('Description:', error.description);
    } else {
      console.error('An unexpected error occurred:', (error as AxiosError).message);
    }
  }
}

const data = await fetchData();
```

---

## Reference

- 모던 자바스크립트 딥다이브
- [MDN Web Docs - Error](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Error)
