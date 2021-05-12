---
layout:     post
title:      "Part 1-2. 자료구조"
date:       2021-05-11
categories: CS

---

[목차]   

1. **Array vs. Linked List**   
   1.1. Array
   1.2. Linked List  
2. **Stack and Queue**   
   2.1. Stack   
   2.2. Queue  
3. **Tree**   
   3.1. Binary Tree(이진 트리)  
   3.2. 장기 스테줄러   
   3.3. 단기 스케줄러   
   3.4. 중기 스케줄러     
4.

- - -

#### **1.  Array vs. Linked List**

##### **1.1. Array**

(1) 특징

 - 가장 기본적인 자료구조
 - 논리적 저장 순서와 물리적 저장 순서가 일치
   + 인덱스(index)로 해당 원소(element)에 접근할 수 있다.
   + random access 가능: 찾고자 하는 원소의 인덱스 값을 알고 있으면, `Big-O(1)`에 해당 원소로 접근할 수 있다.

(2) 단점

 - 삭제 또는 삽입의 과정에서는 해당 원소에 접근하여 작업을 완료한 뒤(`o(n)`), 또 한 가지의 작업을 추가적으로 해 줘야 하기 때문에, 시간이 더 걸린다.

   + 삭제하는 경우
     + 만약 배열의 원소 중 어느 원소를 삭제했다고 했을 때, 배열의 연속적인 특징이 깨지게 된다(빈 공간이 발생).
     + 삭제한 원소보다 큰 인덱스를 갖는 원소들을 `shift` 해 줘야 하는 비용(cost)이 발생하고, 이 경우의 시간 복잡도는 `O(n)`이 된다.

   + 삽입하는 경우
     + 첫 번째 자리에 새로운 원소를 추가하고자 한다면, 모든 원소들의 인덱스를 1씩 shift 해 줘야 하므로, 이 경우에도 `O(n)` 의 시간을 요구하게 된다.



##### **1.2. Linked List**

(1) 특징

 -  Array를 삭제 또는 삽입하는 과정에서 발생하는 문제를 해결하기 위한 자료구조
 - 각각의 원소들은 자기 자신 다음에 어떤 원소가 있는지 만을 기억하고 있다.
   + 이 부분만 다른 값으로 바꿔주면 삭제와 삽입을 `O(n)` 만에 해결할 수 있는 것이다.

(2) 문제   

 - 원하는 위치에 삽입을 하고자 하면 원하는 위치를 Search 해야 하고, 이 과정에서 첫 번째 원소부터 모두 다 확인해봐야 한다.
   + Array 와는 달리 논리적 저장 순서와 물리적 저장 순서가 일치하지 않기 때문
   + 삽입하고 정렬하는 것과 마찬가지이다.
   + 어떠한 원소를 삭제 또는 추가하고자 했을 때, 그 원소를 찾기 위해서 `O(n)` 의 시간이 추가적으로 발생하게 된다.
 - 검색/삽입/삭제 에 대해서 `O(n)` 의 시간 복잡도를 갖는다.
 - Tree의 근간이 되는 자료구조



#### **2. Stack and Queue**

##### **2.1. Stack**

(1) 특징

- 선형 자료구조의 일종
- LIFO(Last In First Out



##### **2.2. Queue**

- 선형 자료구조의 일종
- FIFO(First In First Out)



#### **3. Tree**

(1) 정의

- 스택이나 큐와 같은 선형 구조가 아닌 비선형 자료구조
- 계층적 관계(Relationship)을 표현하는 자료구조

(2) 트리를 구성하고 있는 요소들(용어)

- Node(노드): 트리를 구성하고 있는 각각의 요소
- Edge(간선): 트리를 구성하기 위해 노드와 노드를 연결하는 선
- Root Node(루트 노드): 트리 구조에서 최상위에 있는 노드
- Terminal Node(단말 노드, leaf Node): 하위에 다른 노드가 연결되어 있지 않는 노드
- Internal Node(내부 노드, 비단말 노드): 단말 노드를 제외한 모든 노드, 루트 노드를 포함



##### **3.1. Binary Tree(이진 트리)**

(1) 특징

- 루트 노드를 중심으로 두 개의 서브 트리(큰 트리에 속하는 작은 트리)로 나뉘어 진다.
  + 나뉘어진 두 서브 트리도 모두 이진 트리여야 한다.
  + 공집합도 이진트리로 포함시켜야 한다.
  + 각 층별로 숫자를 매겨서 이를 트리의 Level 이라 한다.
    + 레벨의 값은 0부터 시작하며, 따라서 루트 노드의 레벨은 0이다.
    + height: 트리의 최고 레벨

- 배열로 구성된 Binary Tree는 노드의 개수가 n이고 root가 0이 아닌 1에서 시작할 때, i번째 노드에 대해서 아래의 index를 갖는다.

  + parent(i) = i/2
  + left_child(i) = 2i
  + right_child(i) = 2i+1



##### **3.2. Binary Tree(이진 트리)의 종류**

(1) Perfect Binary Tree(포화 이진 트리)
+ 모든 레벨이 꽉 찬 이진 트리
(2) Complete Binary Tree(완전 이진 트리)
+ 위에서 아래로, 왼쪽에서 오른쪽으로 순서대로 차곡차곡 채워진 이진 트리
(3)Full Binary Tree(정 이진 트리)
+ 모든 노드가 0개 혹은 2개의 자식 노드만을 갖는 이진 트리
(4) BST(Binary Search Tree)



##### **3.3. BST(Binary Search Tree)**

- 이진 탐색 트리에는 데이터를 저장하는 규칙이 존재
  : 이 규칙은 특정 데이터의 위치를 찾는 데 사용할 수 있다.
  + 규칙 1. 이진 탐색 트리의 노드에 저장된 키는 유일하다.
  + 규칙 2. 부모의 키가 왼쪽 자식 노드의 키보다 크다.
  + 규칙 3. 부모의 키가 오른쪽 자식 노드의 키보다 작다.
  + 규칙 4. 왼쪽과 오른쪽 서브트리도 이진 탐색 트리이다.
- 시간 복잡도: `O(n)`
  + 정확하게 말하면 `O(h)`
    + 트리의 높이를 하나씩 더해갈 수록 추가할 수 있는 노드의 수가 두 배씩 증가하기 때문
      하지만 이러한 이진 탐색 트리는 Skewed Tree(편향 트리)가 될 수 있다.
    + 이럴 경우 성능에 영향을 미치게 되며, 탐색의 Worst case가 되며, 시간 복잡도는 `O(n)`이 된다.
- Rebalancing 기법
  + 배열보다 많은 메모리를 사용하며 데이터를 저장했지만 탐색에 필요한 시간 복잡도가 같게 되는 비효율적인 문제를 해결하기 위한 기법
  + 균형을 잡기 위한 트리 구조의 재조정
  + 구현: Red-Black Tree



#### **4. Binary Heap**

(1) 정의

- 자료 구조의 일종으로, Tree의 형식
  + Tree 중에서도 배열의 기반한 Complete Binary Tree
- 배열에 트리의 값을 넣어줄 때, 0번째는 건너뛰고 1번 index 부터 루트 노드가 시작된다.
  + 노드의 고유번호 값과 배열의 index를 일치시켜 혼동을 줄이기 위함

(2) 종류

- 최대힙(max heap)
  - 각 노드의 값이 해당 children의 값보다 크거나 같은 complete binary tree
  - 최댓값을 찾는데 필요한 시간복잡도: `O(1)`
    + 루트 노드에 있는 값이 가장 크기 때문
  - complete binary tree 이므로 배열을 사용하여 효율적으로 관리할 수 있다.
    + random access가 가능하다.
- 최소힙(min heap)
  + 각 노드의 값이 해당 children의 값보다 작거나 같은 complete binary tree
  + 최솟값을 찾는데 필요한 시간복잡도: `O(1)`

(3) 시간 복잡도

- heap의 구조를 계속 유지하기 위해서는 제거된 루트 노드를 대체할 다른 노드가 필요하다.

- - 여기서 heap은 맨 마지막 노드를 루트 노드로 대체시킨 후, 다시 heapify 과정을 거쳐 heap 구조를 유지한다.
  - 결국, `O(logn)` 의 시간 복잡도로 최댓값 또는 최솟값이 접근할 수 있게 된다.



#### **5. Red Black Tree(RBT)**

- BST를 기반으로 하는 트리 형식의 자료 구조

- BST의 삽입, 삭제 연산 과정에서 발생할 수 있는 문제점을 해결하기 위해 만들어진 자료구조

- 검색/삽입/삭제에 대한 시간 복잡도: `O(logn)`
  + 동일한 노드 개수일 때, depth를 최소화하여 시간 복잡도를 줄이는 것이 핵심 아이디어
    + depth가 최소가 되는 경우는 tree가 complete binary tree 인 경우



##### **5.1. Red Black Tree의 정의**

- 각 노드는 Red or Black 이라는 색깔을 갖는다.

- Root node의 색깔은 Black 이다.

- 각 leaf node는 Black 이다.

- 어떤 노드의 색깔이 Red 라면, 두 개의 children의 색깔은 모두 black 이다.

- 각 노드에 대해서 노드로부터 descendant leaves 까지의 단순 경로는 모두 같은 수의 black nodes 들을 포함하고 있다.
  + Black-Height: 노드 x로 부터 노드 x를 포함하지 않은 leaf node 까지의 simple path 상에 있는 black nodes 들의 개수



##### **5.2. Red Black Tree의 특징**

- Binary Search Tree 이므로, BST의 특징을 모두 갖는다.
- Root node 부터 leaf node 까지의 모든 경로 중 최소 경로와 최대 경로의 크기 비율은 2보다 크지 않다.
  + Balanced 상태
- 노드의 child가 없을 경우 child를 가리키는 포인터는 NIL 값을 저장한다. 이러한 NIL 들은 leaf node로 간주한다.
- Java Collection에서 ArrayList도 내부적으로 RBT로 이루어져 있고, HashMap에서의 Seperation Chaining에서도 사용된다.



##### **5.3. Red Black Tree의 삽입**

- BST의 특성을 유지하면서 노드를 삽입

- 삽입된 노드의 색깔을 Red로 지정
  + Red로 지정하는 이유: Black-Height 변경을 최소화하기 위함

- 삽입 결과 RBT의 특성을 위배(violation)하는 경우 노드의 색깔을 조정하고, Black-Height가 위배되었다면 rotation을 통해 height를 조정한다.
  + RBT의 동일한 height에 존재하는 internal node 들의 Black-height가 같아지게 되고, 최소 경로와 최대 경로의 크기 비율이 2 미만으로 유지된다.



##### **5.4. Red Black Tree의 삭제**

- BST의 특성을 유지하면서 노드를 삭제
- 삭제될 노드의 개수에 따라 rotation 방법이 달라지게 된다.
  + 지워진 노드의 색깔이 Black 이라면 Black-Height가 1 감소한 경로에 black node가 1개 추가되도록 rotation하고 노드의 색깔을 조정한다.
  + 지워진 노드의 색깔이 Red 라면 violation이 발생하지 않으므로 RBT가 유지된다.



#### **6. Hash Table**

- 내부적으로 배열을 사용하여 데이터를 저장하기 때문에 빠른 검색 속도를 갖는다.
- average case에 대한 시간 복잡도: `O(1)`
  + 특정한 값을 검색하는 데 데이터 고유의 인덱스로 접근하게 되기 때문
  + 항상 `O(1)`인 것은 아니며, average case에 대해서 인 것은 collision 때문
- 문제점
  - 인덱스로 저장되는 key 값이 불규칙
  - 해결책
    + 특별한 알고리즘을 이용하여 저장할 데이터와 연관된 고유한 숫자를 만들어 낸 뒤 이를 인덱스로 사용한다.
    + 특정 데이터가 저장되는 인덱스는 그 데이터만의 고유한 위치이기 때문에, 삽입 연산 시 다른 데이터의 사이에 끼어들거나, 삭제 시 다른 데이터로 채울 필요가 없으므로 연산에서 추가적인 비용이 없도록 만들어진 구조

##### **6.1. Hash Function**

(1) 특징

- hash method
- 인덱스로 사용할 저장할 데이터와 연관된 고유한 숫자를 만들어 내는 해시 함수 또는 hash method
  + hash code: 해시 함수를 통해 반환된 데이터의 고유 숫자 값
    + 저장되는 값들의 key 값을 hash function을 통해서 작은 범위의 값들로 바꿔준다.
- 어설픈 해시 함수를 통해 key 값들을 결정하는 경우 동일한 값이 도출될 수 있다.
  + collision 발생
    + 동일한 key 값에 복수 개의 데이터가 하나의 테이블에 존재하게 되는 것
    + 서로 다른 두 개의 키가 같은 인덱스로 hashing(hash 함수를 통해 계산됨을 의미)되면 같은 곳에 저장할 수 없게 된다.
  + collision이 많아 질수록 검색에 필요한 시간복잡도가 `O(1)` 에서 `O(n)` 에 가까워 진다.

(2) 해시 함수가 갖추어야 하는 조건

- 일반적으로 좋은 해시 함수: 키의 일부분을 참조하여 해시 값을 만들지 않고, 키 전체를 참조하여 생성
  + 좋은 해시 함수는 키가 어떤 특성을 갖고 있느냐에 따라 달라진다.
- 무조건 1:1로 만드는 것보다 collision을 최소화 하는 방향으로 설계하며, 이 collision에 대비하여 어떻게 대응할 것인가가 더 중요
  + 1:1 대응이 되도록 만드는 것이 거의 불가능하기도 하다.
    + 이러한 해시함수를 만든다 하더라도 이는 array와 다를 바 없고, 메모리도 너무 많이 차지하게 된다.

(3) hashing 된 인덱스에 이미 다른 값이 들어 있다면, 새 데이터를 저장할 다른 위치를 찾은 뒤에야 저장할 수 있다.



##### **6.1. Resolve Conflict**

(1) Open Conflict

- 해시 충돌이 발생하면(즉, 삽입하려는 해시 커밋이 이미 사용 중인 경우), 다른 해시 버킷에 해당 자료를 삽입하는 방식
  + 버킷: 바구니와 같은 개념으로, 데이터를 저장하기 위한 공간
- 공개 주소 방식이라고도 불리며, collision이 발생하면 데이터를 저장할 장소를 찾아 헤맨다.
  + 최악의 경우: 비어있는 버킷을 찾지 못하고 탐색을 시작한 위치까지 되돌아 오는 경우
    + Linear Probing: 순차적으로 탐색하며, 비어있는 버킷을 찾을 때까지 계속 진행된다.
    + Quandratic probing: 2차 함수를 이용해 탐색할 위치를 찾는다.
    + Double hashing probing: 하나의 해시 함수에서 충돌이 발생하면, 2차 해시 함수를 이용해 새로운 주소를 할당한다. 위 두 가지 방법에 비해 많은 연산량을 요구하게 된다.



참고: <https://github.com/JaeYeopHan/Interview_Question_for_Beginner>