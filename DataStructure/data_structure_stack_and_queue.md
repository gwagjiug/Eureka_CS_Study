### Stack / Queue
---
### 📚 Stack
* 먼저 스택과 큐에 대해 말하기 전에 배열, 리스트 , 스택 , 큐 등은 대표적인 **선형 자료구조**(linear data structure) 라는 것을 짚고 넘어 가야한다.

* 선형 자료구조는 연속적으로 데이터가 나열되는 자료구조를 말한다. 추후에 다루겠지만 하나의 데이터 뒤에 N개의 데이터가 이어질 수 있는, 1:N 또는 N:N 구조로 데이터가 나열되는 자료구조를 비선형 자료구조라고 한다.

* 기술 면접에서는 **특정 자료구조의 개념을 직접 묻기 보다는 특수한 상황에서 해당 상황을 해결하는 데 적합한 자료구조가 무엇인지 묻는** 방식으로 자주 출제 된다는 것을 기억하자
---
![image](https://github.com/gawgjiug/Eureka_CS_Study/assets/99489686/4dd7bc31-ce62-4f78-a78c-c547955dbdbd)

* 접시 더미와 마찬가지로 추가 또는 제거는 상단에서만 실용적이다



### 특징
---

#### 스택은 후입선출(LIFO Last In First Out) 구조

* 스택에 데이터를 삽입하는 연산을 **Push**라 하고, 데이터를 삭제하는 연산을 **Pop**이라고 하며 마지막에 저장한 데이터를 삭제한다.

* 스택은 top이라는 변수를 이용해 데이터를 마지막으로 저장한 인덱스를 기억하는데, 따라서 push 나 pop 연산을 할 때 배열을 이용해도 O(1)의 시간복잡도로 마지막 데이터에 접근 할 수 있다.

* ## 주요 메서드

 **`1. isEmpty()`**, **`isFull()`**
 * 각각 스택이 비어있는지, 가득 찼는지를 boolean 형태로 리턴하는 메서드이다

 **`2.push()`**
 * 스택에 새로운 원소를 삽입한다.
 * 스택이 가득 차 있다면 에외를 던진다.

 **`3. peek()`**
 * 최상층에 위치한 데이터를 읽어온다.

 **`4. pop()`**
 * 최상층에 위치한 데이터를 읽어오고 해당 데이터를 스택에서 제거한다.

---

### 스택의 구현

 **1. 배열사용**

 * 배열로 후입선출을 구현하기 위해선 인덱스를 사용해야한다 먼저 top을 가리키는 인덱스를 t라는 변수에 저장한다. 아래 그림과 같이 push 할때는 인덱스에 1을 더한 값에 데이터를 넣어주면 되고, pop 할때는 인덱스에서 1을 빼주면 된다

![](https://velog.velcdn.com/images/gawgjiug/post/a984d066-2e3a-4d51-b5b1-fd9bd2371473/image.png)


* 여기서 데이터를 삭제한다는 것은 메모리에서 지워버리는 것이 아니라 (배열은 컴파일 타임에 크기가 정해지기 때문에 메모리 반납이 무의미하다.) 따라서 여기서 데이터 삭제는 '무시한다'에 조금 더 가깝다 값을 무시해버리면 이를 데이터 삭제로 간주하는 것

* 배열의 큰 장점은 '속도'이다 


```java
class Stack
{
    private int arr[];
    private int top;
    private int capacity;

    // Stack 초기화를 위한 생성자
    Stack(int size)
    {
        arr = new int[size];
        capacity = size;
        top = -1;
    }

    // Stack에 요소 `x`를 추가하는 유틸리티 함수
    public void push(int x)
    {
        if (isFull())
        {
            System.out.println("Overflow\nProgram Terminated\n");
            System.exit(-1);
        }

        System.out.println("Inserting " + x);
        arr[++top] = x;
    }

    // Stack에서 최상위 요소를 꺼내는 유틸리티 함수
    public int pop()
    {
        // Stack 언더플로 확인
        if (isEmpty())
        {
            System.out.println("Underflow\nProgram Terminated");
            System.exit(-1);
        }

        System.out.println("Removing " + peek());

        // Stack 크기를 1로 줄이고 (선택적으로) 팝된 요소를 반환합니다.
        return arr[top--];
    }

    // Stack의 최상위 요소를 반환하는 유틸리티 함수
    public int peek()
    {
        if (!isEmpty()) {
            return arr[top];
        }
        else {
            System.exit(-1);
        }

        return -1;
    }

    // Stack의 크기를 반환하는 유틸리티 함수
    public int size() {
        return top + 1;
    }

    // Stack이 비어 있는지 확인하는 유틸리티 함수
    public boolean isEmpty() {
        return top == -1;               // or return size() == 0;
    }

    // Stack이 가득 찼는지 확인하는 유틸리티 함수
    public boolean isFull() {
        return top == capacity - 1;     // or return size() == capacity;
    }
}

class Stack_java
{
    public static void main (String[] args)
    {
        Stack stack = new Stack(3);

        stack.push(1);      // Stack에 1 삽입
        stack.push(2);      // Stack에 2 삽입

        stack.pop();        // 상단 요소 제거(2)
        stack.pop();        // 최상위 요소 제거(1)

        stack.push(3);      // Stack에 3 삽입

        System.out.println("The top element is " + stack.peek());
        System.out.println("The stack size is " + stack.size());

        stack.pop();        // 최상위 요소 제거(3)

        // Stack이 비어 있는지 확인
        if (stack.isEmpty()) {
            System.out.println("The stack is empty");
        }
        else {
            System.out.println("The stack is not empty");
        }
    }
}
```



#### 실행결과 

> Inserting 1
>
> Inserting 2
>
> Removing 2
>
> Removing 1
>
> Inserting 3
>
> The top element is 3
>
> The stack size is 1
>
> Removing 3
>
> The stack is empty


* 고정 크기의 스택을 구현할 수 있지만 구현이 복잡하다.


### 자바 컬렉션으로 구현


```java

import java.util.Stack;

public class Stack_Collection {
    public static void main(String[] args) {
        Stack<String> stack = new Stack<String>();

        stack.push("A");
        stack.push("B");
        stack.push("C");
        stack.push("D");

        System.out.println("top element is : " + stack.peek());


        stack.pop();
        stack.pop();

        System.out.println("stack size : " + stack.size());

        if (stack.empty()){
            System.out.println("스택이 비어있습니다");
        }
        else {
            System.out.println("스택이 비어있지 않습니다.");
        }

    }
}
```


> top element is : D
>
> stack size : 2
>
> 스택이 비어있지 않습니다.

* 스택은 주로 어떤 작업의 실행을 취소할 때, 웹 브라우저에서 뒤로가기 할 떄 등 최근에 처리한 작업들을 하나씩 꺼낼 때 사용한다.

* 스택 컬렉션을 사용할 경우 구현이 간단하고 사용법이 쉽지만, 내부적으로 동적 배열로 구현되어 있어, 크기가 자동으로 조절되어 고정된 크기의 스택을 생성할 수 없고 일부 상황에서 성능이 떨어질 수 있음.


* ### 🍡 Queue

--- 

![image](https://github.com/gawgjiug/Eureka_CS_Study/assets/99489686/544f3eb3-500a-49a0-aaa8-260aff553c3b)

> 큐는 **데이터가 순차적으로 들어오는 형태**로, 먼저 들어온 데이터가 먼저 나간다.

### 특징
---

#### 선입선출(FIFO First In First Out)구조
`Queue`란 데이터를 순서대로 세운 형태의 자료구조를 뜻한다.
`front`로 정한 곳에서는 `조회/삭제` 연산이 일어나고, `Rear`로 정한 곳에서 `삽입` 연산이 발생한다.


### 큐의 연산 

 **`1. isEmpty()`**, **`isFull()`**
 * 앞서 설명한 스택과 동일하게 각각 큐가 비었는지, 가득 찼는지를 boolean 형태로 반환한다.

 **`2. enqueue()`**
 * 큐에 새로운 원소를 삽입한다.
 * 가득 차 있다면 예외를 던진다.
 * 큐에 rear에 새로운 데이터 삽입

 **`3. peek()`**
 * 큐의 front에 있는 데이터 확인

 **`4. dequeue`**
 * 큐의 frot에서 데이터 삭제

---
### 큐 사용법

* 자바에서 Queue는 Collection 프레임워크의 일부이고 java.util 패키지에 소속되어 있기 때문에 Queue를 사용하려면 먼저 다음과 같이 import를 해야한다

```java
import java.util.Queue;
import java.util.LinkedList;

```

* Queue는 연결 리스트로 보통 구현되기 때문에 LinkedList도 import 해야 함.

---

### Queue 선언

```java

Queue <String> str = new LinkedList<String>(); //String Type Queue 선언

```

---

### Queue 값 추가 삭제

```java

Queue <String> queue = new LinkedList<>();

queue.add("Hello") //Hello add
queue.offer("World") //World 추가함

queue.remove(); //"Hello" 반환
queue.poll(); // "World" 반환

queue.remove(); // NoSuchElementException 발생
queue.offer(); //null

```

---

### 시간 복잡도

* Insert O(1)

* Deletion O(1)

* Search O(n)



### LinkedList를 이용해서 Queue 구현

```java

import java.util.LinkedList;

public class LinkedListQueue<T> {
    private LinkedList<T> list;

    // Queue 생성자
    public LinkedListQueue() {
        list = new LinkedList<>();
    }

    // Queue가 비어있는지 확인
    public boolean isEmpty() {
        return list.isEmpty();
    }

    // Queue에 요소 추가
    public void enqueue(T item) {
        list.addLast(item);
    }

    // Queue에서 요소 제거 및 반환
    public T dequeue() {
        if (isEmpty()) {
            System.out.println("Queue is empty. Cannot dequeue.");
            return null;
        }
        return list.removeFirst();
    }

    // Queue의 첫 번째 요소 반환
    public T peek() {
        if (isEmpty()) {
            System.out.println("Queue is empty. Cannot peek.");
            return null;
        }
        return list.getFirst();
    }

    // Queue의 현재 크기 반환
    public int size() {
        return list.size();
    }

    // Queue 출력
    public void display() {
        if (isEmpty()) {
            System.out.println("Queue is empty.");
            return;
        }
        System.out.print("Queue: ");
        for (T item : list) {
            System.out.print(item + " ");
        }
        System.out.println();
    }

    public static void main(String[] args) {
        LinkedListQueue<Integer> queue = new LinkedListQueue<>();
        queue.enqueue(1);
        queue.enqueue(2);
        queue.enqueue(3);
        queue.enqueue(4);
        queue.enqueue(5);
        queue.display(); // Queue: 1 2 3 4 5
        System.out.println("Dequeued item: " + queue.dequeue()); // Dequeued item: 1
        queue.display(); // Queue: 2 3 4 5
        System.out.println("Front item: " + queue.peek()); // Front item: 2
        System.out.println("Queue size: " + queue.size()); // Queue size: 4
    }
}
```

* 당연히 큐도 배열로 구현할 수 있다. 큐를 배열로 구현하면 rear의 인덱스를 이용해서 큐가 가득 찼는지 쉽게 확인할 수 있고, 디큐를 수행할 때 frot나 rear만 수정하면 된다.
  이러한 방식은 시간 복잡도에서 이점이 있지만 한계도 있다. 데이터 삽입 연산으로 큐를 꽉 채운 뒤, 1개만 남기고 삭제 연산을 수행하면 rear의 인덱스를 보고 큐에 데이터가 가득 찼다고 생각할 수 있다는 점
  

* 큐를 사용하는 대표적인 예로 운영체제에서 프로세스가 CPU를 할당받기 전까지 대기하는 준비 큐가 있고, 그 외에도 어떠한 작업을 처리할 때 작업 요청이 들어온 순서대로 처리하기 위해 주로 큐를 사용한다.

### 예상 질문

#### Q1. 스택으로 큐를 구현할 수 있는가? 그 반대는?
> 가능하다. 이를 위해 두 개의 스택을 사용하면 된다. 하나는 입력스택 다른 하나는 출력스택

```java
import java.util.Stack;

public class QueueUsingStacks<T> {
    private Stack<T> inputStack = new Stack<>();
    private Stack<T> outputStack = new Stack<>();

    public void enqueue(T item) {
        inputStack.push(item);
    }

    public T dequeue() {
        if (outputStack.isEmpty()) {
            while (!inputStack.isEmpty()) {
                outputStack.push(inputStack.pop());
            }
        }
        if (outputStack.isEmpty()) {
            throw new RuntimeException("Queue is empty");
        }
        return outputStack.pop();
    }

    public boolean isEmpty() {
        return inputStack.isEmpty() && outputStack.isEmpty();
    }
}
```
* 두개의 스택을 사용해서 큐의 선입선출 특성을 유지할 수 있게끔 만든다, 입력 스택의 요소를 출력 스택으로 옮기는 과정에서 순서가 뒤집혀서
올바른(큐의 모습) 순서로 반환됨.

>input{3,5,2}
>
>dequeue
>
>output{2,5,3}
>
>pop(Stack) -> '3' **선입 선출**

---
### 큐로 스택을 구현할 수 있는가?

* 큐로 스택을 구현하는 것도 가능하다. 이를 위해 두 개의 큐를 사용하면 된다. 하나를 주 큐로 다른 하나를 보조 큐로 사용한다.

```java
import java.util.LinkedList;
import java.util.Queue;

public class StackUsingQueues<T> {
    private Queue<T> queue1 = new LinkedList<>();
    private Queue<T> queue2 = new LinkedList<>();

    public void push(T item) {
        queue1.offer(item);
    }

    public T pop() {
        if (queue1.isEmpty()) {
            throw new RuntimeException("Stack is empty");
        }

        while (queue1.size() > 1) {
            queue2.offer(queue1.poll());
        }

        T poppedItem = queue1.poll();

        // Swap the names of queue1 and queue2
        Queue<T> temp = queue1;
        queue1 = queue2;
        queue2 = temp;

        return poppedItem;
    }

    public boolean isEmpty() {
        return queue1.isEmpty() && queue2.isEmpty();
    }
}
```

* 1번 큐에 모든 요소를 추가한다.
* pop 연산이 들어오면, 큐1의 마지막 원소를 제외한 모든 원소를 큐 2에 옮긴다.
* 큐1의 요소를 popitem 에 삽입하고 출력. **(후입 선출)**
* 큐 1과 2를 swap

> queue1 {3, 5, 2}
> 
> pop
> - queue2 {3, 5}
> - '2' 반환
> 
> swap
> - queue1 {3, 5}
> - queue2 {}

#### Q2. 스택과 큐의 특성을 모두 사용해야 한다면?
> `데크(Deque)` 자료 구조를 사용할 수 있다. 삽입,삭제,조회 연산이 데크의 양 끝단에서만 이루어지는 구조이다.
