### 선형 자료구조(연결리스트)

### 연결리스트

- 연결리스트는 대표적인 선형 자료구조의 하나로 배열과 달리 크기가 정해져 있지 않은 `동적 자료구조`이다. 이는 연결 리스트가 여러개의 **노드**로 구성되기 때문이다. 노드는 데이터와 다음 노드가 저장된 주소 값을 가지고 있다.

### LinkedList 종류

#### 단방향 연결리스트

- 위에서 알 수 있듯이, 다음 노드를 가리키기 위한 포인터 필드 `next` 만을 가지고 있는 Linked List를 단방향 연결리스트라고 한다. 하지만 단일 연결 리스트는 현재 요소에서 이전 요소로 접근해야 할 때 매우 부적합한 경우가 있다.

- 예를들어 10,000개의 데이터가 있고, 9999번째 데이터에 접근하려면 9999번 노드를 이동해야 하기 때문에 비효율적일 수 있다는 것이다.

  ![image](https://github.com/gawgjiug/Eureka_CS_Study/assets/99489686/49fd9751-2f10-46cc-8e49-9c3bfed229a2)

#### 양방향 연결 리스트

![](https://velog.velcdn.com/images/gawgjiug/post/38b53c69-3656-4393-a716-d05fc07f6820/image.png)

- 이중 연결 리스트는 기존의 단일 연결 노드 객체에서 이전 노드 주소를 담고 있는 `prev` 가 추가된 형태이다.

- 단방향 연결리스트의 단점을 보완하기 위해 이전 노드 주소를 담는 필드를 추가한 것.

- 이중 연결 리스트에서 노드는 앞 노드의 주소 값과 다음 노드의 주소 값을 모두 저장하기 때문에 단순 연결리스트와 달리 양방향 탐색이 가능한 것이다.

- 한 노드당 주소 값 2개를 저장해야 하기 때문에 메모리를 많이 차지하는 단점이 있지만, 노드의 연결 순서와 무관하게 노드를 연속적으로 탐색해야 하는 경우에
  단방향 연결리스트보다 시간면에서 효율적이다

> 실제로 java 컬렉션 프레임 워크에 구현된 LinkedList 클래스는 doubly linked list로 구현되어있다.

#### 원형 연결리스트

- 원형 연결리스트는 마지막 노드가 NULL 값ㅣ 아니라 첫 번째 노드의 주소 값을 가리키는 구조이다.

![image](https://github.com/gawgjiug/Eureka_CS_Study/assets/99489686/ebebcc2e-96c0-4d18-acf5-977dcab9d4a3)

- 이러한 원형 연결리스트이 장점은 새로운 노드를 맨 마지막 또는 맨 앞에 삽입할 때 상수 시간이 소요된다는 점이다. 헤드가 마지막 노드를 가리키고 있어서 O(1) 에 마지막 노드에 접근 할 수 있다.
- 먼저 마지막 노드가 가리키던 주소 값을 새로운 노드가 가리키게 하고, 마지막 노드는 새로운 노드를 가리키게 한다. 그 다음 헤드가 새로운 노드를 가리키면 마지막에 노드를 추가하는 경우에도 상수시간에 수행을 완료할 수 있다, 또한 순환 구조라서 어느 노드든지 배열의 다른 노드에 모두 접근 할 수 있다.

#### LinkedList vs ArrayList 특징 비교

**데이터 접근 시간**

ArrayList : 모든 데이터 상수 시간 접근

LinkedList : 위치에 따라 이동시간 발생

**삽입 / 삭제 시간**

- 삽입/삭제 자체는 둘다 상수 시간을 갖는다, 하지만 ArrayList는 삽입/삭제 시 데이터 이동이 필요한 경우 추가시간이 발생하고 LinkedList도 그 위치까지 이동하는 시간이 발생한다.

**리사이징 필요성**

ArrayList : 공간이 부족할 경우 새로운 배열에 복사하는 추가 시간 발생한다

### LinkedList 객체 생성

- LinkedList 를 사용하기 위해선 상단에 패키지를 명시해야한다.

```java
import java.util.LinkedList;
```

- LinkedList 의 선언은 ArrayList와 동일하지만, ArrayList 처럼 초기 값을 미리 지정하는 기능은 제공되지 않음 배열의 경우 내부 데이터 구조가 미리 공간을 할당하는 방식을 사용하지만, LinkedList는 데이터가 추가 될 때 마다 노드 들이 생성되어 동적으로 추가되는 방식이기 때문임.

### LinkedList 요소 추가/삽입

ArrayList 와 달리 LinkedList에는 add 메서드가 4가지 있다.

- addFirst() : 맨 앞에 객체를 추가
- addLasg() : 맨 뒤에 객체를 추가

- boolean add() : LinkedList의 마지막에 객체를 추가함 (성공하면 true를 반환한다.)
- void add(int index, Object element) : 지정된 위치(index)에 객체를 저장한다.
- addAll(Collection c) : 주어진 컬렉션의 모든 객체를 저장한다. (마지막에 추가)\
- addAll(index,Collection c) : 지정한 위치부터 저장.

---

- LinkedList의 가장 큰 특징은 ArrayList와 다르게 중간에 데이터가 추가 되거나 중간에 있는 데이터가 삭제 되어도 앞으로 땡기거나 뒤로 미는 동작 없이 추가 되거나, 삭제될 노드 위치의 바로 앞 뒤쪽에 존재하는 노드의 참조를 변경하기만 하면 된다.

- 또, 배열과 달리 할당이 동적으로 이루어 졌기 때문에 메모리가 충분한 상황에서는 무한정으로 요소를 저장 할 수 있음.

### 요소 삭제

- Obejct removeFirst(): 첫번째 노드를 제거

- Object removeLast(): 마지막 노드를 제거

- Object remove(): LinkedList의 첫 번째 요소를 제거

- Object remove(int index): 지정된 위치(index)에 있는 객체를 제거한다.

- boolean remove(Object obj): 지정된 객체를 제거한다. (성공하면 true)

- boolean removeAll(Collection c): 지정한 컬렉션에 저장된 것과 동일한 노드들을 LinkedList에서 제거한다.

- boolean retainAll(Collection c): LinkedList에 저장된 객체 중에서 주어진 컬렉션과 공통된 것들만 남기고 제거한다.

- void clear(): LinkedList를 완전히 비운다.

---

### 요소 검색

- int size() : 저장된 객체의 개수를 반환함

- isEmpty() : 연결리스트가 비어있는지 확인.

- contains(object) : 객체가 연결리스트에 포함되어 있는지 확인한다.

- containsAll(Collection c) : 지정된 컬렉션의 모든 요소가 포함되었는지 알려줌 (모든 요소가 포함되어있어야 true를 반환함)

- indexOf() : 지정된 객체가 저장된 위치를 찾아 반환함.

- lastIndexOf() : 지정된 객체가 저장된 위치를 뒤에서 부터 역방향으로 찾아 반환한다.

---

### 요소 찾기

- 개별 단일 요소를 얻고자 하면 get 메서드로 얻어올 수 있다. 탐색 성능은 좋지 않은 편

1. Object get(int index) : 지정된 위치에 저장된 객체를 반환함.

2. List subList(firstIndex, tolIndex) : first ~ tol -1까지의 저장된 객체를 List로 반환함

---

### 요소 변경

- set 메서드를 사용해서 요소를 변경할 수 있다

```java

LinkedList<String> list = new LinkedList<>();
list1.add("10");
list1.add("20");
list1.add("30");

list1.set(1, "A"); // index 1번의 데이터를 문자열 "A"로 변경한다.
System.out.println(list1); // // [10, A, 30]
```

---

### 면접 예상 질문

- 연결리스트의 종류와 관련해서 설명해주세요.

- 단순 연결리스트를 역순으로 출력하는 방법을 설명해 보세요.

  > 단순 연결 리스트를 역순으로 출력하는 방법으로는 스택에 연결 리스트의 요소를 순차적으로 넣고 빼는 방법이 있지만, 이러한 방법은 스택을 사용하므로 추가적인 메모리가 필요함.
  > 때문에 포인터 3개를 사용해서 current에 해당하는 노드가 가리키는 주소 값을 previous 노드의 주소 값으로 바꾸는 로직도 알고 있으면 좋다.
