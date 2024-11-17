

### 낙관적 업데이트

낙관적 업데이트의 개념은 다음과 같다.

* 데이터가 `실제 서버` 에 반영되기 전에 미리 UI에 반영하여 사용자 경험을 향상시키는 기법

* 사용자가 서버 응답을 기다리지 않고도 즉각적인 피드백을 받을 수 있게 해준다.

낙관적 업데이트의 주요 단계는 이렇다.

우선 업데이트를 수행하기 전에 현재 상태를 저장해야한다. 서버 요청이 실패했을 경우 원래 상태로 롤백하기 위함이다.



#### 어떻게?

낙관적 상태 업데이트의 과정은 이렇다. 

`queryClient.setQueryData` 메서드를 사용해서 로컬상태를 즉시 업데이트 한다. 

* 이 작업 전에 `queryClient.cancleQueries` 메서드로 해당 쿼리에 대한 모든 활성 쿼리를 취소해줄 수 있다.

바로 이 작업이 사용자가 변경 사항을 즉시 확인할 수 있도록 하는 것

---

그 다음 실제 데이터를 전송한다. 이때 `mutate` 혹은 `mutateAsync` 메서드를 사용하는 것

서버 요청이 성공하면, 서버로부터 받은 최신 데이터를 다시 상태에 반영한다. 이 작업은 `onSettled` 콜백 함수에서 수행. 


---

### 예제 코드

코드로 낙관적 업데이트를 알아보면 이렇다

```javascript
import { useMutation, useQueryClient } from 'react-query';

function MyComponent() {
    const queryClient = useQueryClient();

    // Mutation을 설정합니다
    const { mutate } = useMutation(updateData, {
        // 1. 낙관적 업데이트: 기존 상태 백업, 즉시 UI에 반영
        onMutate: async (newData) => {
            await queryClient.cancelQueries('myData'); // 활성 쿼리 취소
            
            // (1) 기존 상태를 백업
            const previousData = queryClient.getQueryData('myData'); // 현재 상태를 백업
            
            // (2) UI에 즉시 반영
            queryClient.setQueryData('myData', (oldData) => {
                return { ...oldData, ...newData }; // 새 데이터를 UI에 반영
            });

            // (3) 이전 상태 반환 (onError에서 롤백 시 사용)
            return { previousData }; // 이후의 onError 등에서 복원할 수 있도록 반환
        },
        
        // 2. 서버 요청 실패 시 백업한 상태로 롤백
        onError: (error, newData, context) => {
            queryClient.setQueryData('myData', context.previousData); // 이전 상태로 복원
        },
        
        // 3. 서버 응답 시 최신 데이터로 상태 갱신
        onSettled: () => {
            queryClient.invalidateQueries('myData'); // 데이터를 다시 가져와 최신 상태로 유지
        },
    });

    return (
        <button onClick={() => mutate({ key: 'value' })}>
            Update Data
        </button>
    );
}
```

onMutate 함수는 서버에 요청을 보내기 직전에 실행되며, 서버 요청이 실패했을 때는 UI 상태를 복원하는 동작을 수행한다.

>            await queryClient.cancelQueries('myData'); // 활성 쿼리 취소

먼저 활성 쿼리를 취소해주는 작업은 `정확성` 을 보장하기 위해 존재한다.

예를 들어 백엔드에서 데이터를 다시 가져오는 쿼리가 활성화 되어있다면, 서버 응답 시에 이전 데이터로 덮어 쓰는 문제가 발생할 수 있다

더 자세하게는 사용자가 UI 에서 데이터를 업데이트하자마자 백엔드에서 이전 데이터를 불러오는 쿼리가 완료되면 업데이트된 UI가 갑자기 예전 상태로 돌아가 보일 수 있다.

---

### 활성 쿼리 취소

활성 쿼리 취소가 `왜?` 필요한지에 대해서 더 자세히 설명해보겠다. 

예를 들어 게시글 페이지에서 댓글 리스트가 표시되고, 각 댓글에는 좋아요 버튼이 존재한다고 가정해보자.

사용자가 댓글에 좋아요를 누르면 해당 댓글의 좋아요 수가 UI에 즉시 반영되도록 낙관적 업데이트를 적용했다

동시에 이 페이지에는 일정 주기로 백엔드에서 `최신 댓글 데이터를 가져오는 쿼리` `fetchComment` 가 활성화 되어있었는데,

사용자가 좋아요 버튼을 클릭했을 때 낙관적 업데이트로 즉시 반영된 상태에서, 동시에 `fetchComment` 쿼리가 백엔드에서 이전의 댓글 리스트 데이터를 받아와 UI를 덮어 써버린다. 

위와 같은 현상을 방지하기 위해서 업데이트를 적용하기 전에 `cancleQueries` 를 호출해서 `fetchComment` 쿼리를 일시적으로 취소하는 것.
