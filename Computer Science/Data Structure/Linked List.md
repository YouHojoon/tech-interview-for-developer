### Linked List

---

![img](https://www.geeksforgeeks.org/wp-content/uploads/gq/2013/03/Linkedlist.png)

연속적인 메모리 위치에 저장되지 않는 선형 데이터 구조

(포인터를 사용해서 연결된다)

각 노드는 **데이터 필드**와 **다음 노드에 대한 참조**를 포함하는 노드로 구성

<br/>

**왜 Linked List를 사용하나?**

> 배열은 비슷한 유형의 선형 데이터를 저장하는데 사용할 수 있지만 제한 사항이 있음
>
> 1) 배열의 크기가 고정되어 있어 미리 요소의 수에 대해 할당을 받아야 함
>
> 2) 새로운 요소를 삽입하는 것은 비용이 많이 듬 (공간을 만들고, 기존 요소 전부 이동)

**장점**

> 1) 동적 크기
>
> 2) 삽입/삭제 용이

**단점**

> 1) 임의로 액세스를 허용할 수 없음. 즉, 첫 번째 노드부터 순차적으로 요소에 액세스 해야함 (이진 검색 수행 불가능)
>
> 2) 포인터의 여분의 메모리 공간이 목록의 각 요소에 필요



노드 구현은 아래와 같이 데이터와 다음 노드에 대한 참조로 나타낼 수 있다

```
// A linked list node 
struct Node 
{ 
  int data; 
  struct Node *next; 
}; 
```



**Single Linked List**

노드 3개를 잇는 코드를 만들어보자

```
      head         second         third 
        |             |             | 
        |             |             | 
    +---+---+     +---+---+     +----+----+ 
    | 1  | o----->| 2 | o-----> |  3 |  # | 
    +---+---+     +---+---+     +----+----+
```

[소스 코드]()



<br/>

<br/>

**노드 추가**

- 앞쪽에 노드 추가

```
void push(struct Node* head_ref, int new_data){
    struct Node* new_node = (struct Node*) malloc(sizeof(struct Node));

    new_node->data = new_data;

    new_node->next = (*head_ref);

    (*head_ref) = new_node;
}
```

</br>

- 특정 노드 다음에 추가

```
void insertAfter(struct Node* prev_node, int new_data){
    if (prev_node == NULL){
        printf("이전 노드가 NULL이 아니어야 합니다.");
        return;
    }

    struct Node* new_node = (struct Node*) malloc(sizeof(struct Node));

    new_node->data = new_data;
    new_node->next = prev_node->next;

    prev_node->next = new_node;
    
}
```

</br>

- 끝쪽에 노드 추가

```
void append(struct Node** head_ref, int new_data){
    struct Node* new_node = (struct Node*)malloc(sizeof(struct Node));

    struct Node *last = *head_ref;

    new_node->data = new_data;

    new_node->next = NULL;

    if (*head_ref == NULL){
        *head_ref = new_node;
        return;
    }

    while(last->next != NULL){
        last = last->next;
    }

    last->next = new_node;
    return;

}
```

**원형 Linked List**
- 원형 연결 리스트란 마지막 노드가 첫 번째 노드를 가리키는 리스트이다.
- 연결 리스트가 유용한 경우는 리스트의 끝에 노드를 삽입하는 연산이 효율적일 수 있다.
> 단순 연결 리스트에서 리스트의 끝에 노드를 삽입하기 위해서는 마지막 노드까지 가야한다.
> 
> 원형 연결 리스트에서는 헤드 포인터가 마지막 노드를 가리키도록 구현한다면 상수시간으로 삽입 연산이 가능하다.
- 앞쪽에 노드 추가

```
void push(struct Node* head_ref, int new_data){
    struct Node* new_node = (struct Node*) malloc(sizeof(struct Node));

    new_node->data = new_data;

    new_node->next = (*head_ref)->next; 

    (*head_ref)->next = new_node;
}
```
- 뒤쪽에 노드 추가
원형이라 헤드 포인터가 가리키는 위치만 바꿔주면 된다.

**이중 연결 리스트**
- 단순 연결 리스트에서 후속 노드를 찾기는 쉽지만 선행 노드를 찾으려면 구조상 아주 어렵다.
> 원형 리스트라 하더라도 전체 노드를 거쳐서 돌아와야됨
-  이러한 문제를 해결하기 위해 만들어진 것이 이중 연결 리스트
-  단점으로는 공간을 많이 차지한다.
-  실제 응용에서는 이중 연결 리스트와 원형 연결 리스트를 혼합한 형태가 많이 사용된다.
-  헤드 노드라는 특별한 노드를 추가하는 경우가 많다.
> 헤드 노드는 데이터 필드에는 아무런 정보가 없고 삽입과 삭제 알고리즘을 간편하게 하기 위해 존재한다.
