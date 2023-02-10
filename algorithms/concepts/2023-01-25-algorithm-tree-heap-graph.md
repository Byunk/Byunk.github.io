---
title: 트리, 힙, 그래프
comments: true
---

# 트리

## 구현

Binary Tree의 구현

```cpp
using namespace std;
struct node
{
  string position;
  node* first;
  node* second;
}

struct tree
{
  node* root;

  static tree create_tree_structure(const string& pos) {
    tree tree;
    tree.root = new node {pos, NULL, NULL};
    return tree;
  }

  static node* find(node* root, const string& value) {
    if (root == NULL) {
      return NULL;
    }

    if (root->position == value) {
      return root;
    }

    auto firstFound = find(root->first, value);

    if (firstFound != NULL) {
      return firstFound;
    }

    return find(root->second, value);
  }

  bool addSubordinate(const string& manager, const string& subordinate) {
    auto managerNode = find(root, manager);

    if (!managerNode) {
      return false;
    }

    if (managerNode->first && managerNode->second) {
      return false;
    }

    if (!managerNode->first) {
      managerNode->first = new node {subordinate, NULL, NULL};
    } else {
      managerNode->second = new node {subordinate, NULL, NULL};
    }
    return true;
  }
}
```

## 트리 순회

### 전위 순회(preorder traversal)

현재 노드 -> 왼쪽 노드 -> 오른쪽 노드 순으로 방문

```cpp
static void preOrder(node* start) {
  if (!start)
    return;

  // 출력 혹은 저장
  preOrder(start->first);
  preOrder(start->second);
}
```

### 중위 순회(inorder traversal)

왼쪽 노드 -> 현재 노드 -> 오른쪽 노드 순으로 방문

```cpp
static void inOrder(node* start) {
  if (!start)
    return;

  inOrder(start->first);
  // 출력 혹은 저장
  inOrder(start->second);
}
```

### 후위 순회(postorder traversal)

왼쪽 노드 -> 오른쪽 노드 -> 현재 노드 순으로 방문

```cpp
static void postOrder(node* start) {
  if (!start)
    return;

  postOrder(start->first);
  postOrder(start->second);
  // 출력 혹은 저장
}
```

## 다양한 트리 구조

### 이진 검색 트리(BST, Binary Search Tree)

BST는 다음의 조건을 만족한다.

- 부모 노드의 값 >= 왼쪽 자식 노드의 값
- 부모 노드의 값 <= 오른쪽 자식 노드의 값

BST의 검색 및 삽입은 O(log N)의 시간 복잡도를 가진다.

#### 검색

노드를 내려가면서 노드와 검색하는 값 사이의 대소관계를 비교하고 탐색 방향을 정함.

#### 삽입

노드를 내려가면서 노드와 삽입하는 값 사이의 대소관계를 비교하고 삽입할 곳을 정함.

#### 삭제

BST에서 원소를 삭제하는 작업은 삭제 후 전체 트리가 BST를 만족하도록 해야하므로, 조금 더 복잡함.

- *자식 노드가 없는 경우*: 해당 노드 삭제
- *자식 노드가 하나만 있는 경우*: 노드 삭제 후, 부모 노드의 포인터가 해당 자식 노드를 가리키도록 조정
- *자식 노드가 두개 있는 경우*: 노드 삭제 후, 후속 노드로 대체

*후속 노드

현재 노드 다음으로 큰 숫자를 가진 노드. (현재 원소보다 큰 원소들 중 가장 작은 원소)

1. 삭제하려는 값을 지우고, 후속 노드의 값으로 덮어씀.
2. 후속 노드를 삭제하고, 자식 노드로 대체.

![BST Deletion](https://media.geeksforgeeks.org/wp-content/uploads/deletion-in-binary-tree.png)

### 균형 트리

![Skewed BST](https://i.stack.imgur.com/Zhb5e.png)

편향된 이진 검색 트리의 검색은 원소의 개수, n에 가깝다. 이 문제를 해결하기 위해, 트리 회전, AVL 트리, 레드-블랙 트리와 같은 방법으로 균형잡힌 트리를 만들 수 있음.

### N-항 트리(N-ary Tree)

각 노드가 N개의 자식을 가질 수 있음.

```cpp
struct nTree
{
  int data;
  vector<nTree*> children;
}
```

일반적으로 N-항 트리는 그다지 유용하지 않지만, 적절한 요구사항이 있다면 유용하게 사용할 수 있음.

EX) 컴퓨터 파일 시스템, 컴파일러

### 관련 문제

- Baekjoon

[트리인가?(6416)](/baekjoon/baekjoon-6416)

[이진 검색 트리(5639)](/baekjoon/baekjoon-5639)

# 힙

힙은 다음과 같은 시간 복잡도를 만족해야 함.

- O(1): 최대(최소) 원소에 즉각적인 접근이 가능해야 함.
- O(log N): 원소 삽입
- O(log N): 최대 원소 삭제

힙은 위의 조건을 만족하기 위해 완전 이진 트리(complete binary tree)를 사용해야 함. 완전 이진 트리는 배열로 구현할 수 있음. 만약 부모 노드가 i번째 배열 원소라면, 자식 노드는 2*i+1또는 2*i+2번째 인덱스에 존재함.

힙의 불변성(invariants)을 만족해야하며, 항상 최대 원소가 루트에 있는 힙을 최대 힙(max heap), 최소 원소가 루트에 있는 힙을 최소 힙(min heap)이라고 함.

## 원소 삽입

![heap insertion](https://i.imgur.com/kx7DM60.png)

완전 이진 트리를 만족하도록 원소 삽입. 이후 부모 노드로 올라가면서 대소 비교하고, 필요하다면 swap.

## 원소 삭제

![heap deletion](https://i.imgur.com/FGNU5Ks.png)

힙에서는 루트 노드만을 삭제할 수 있음. 이후 어느 노드를 이용해 루트를 대체할 것인지를 결정해야 함. 따라서 이를 위해, 먼저 루트 노드와 트리의 맨 마지막 노드를 서로 교환한 후, 마지막 노드를 삭제. 이후 루트 노드에서 내려오면서 swap.

## 힙 초기화

가장 간단한 힙 초기화 방법은 하나씩 원소를 삽입하는 것임. 이 경우 O(N log N)이 소요됨. 그러나 Heapification Algorithm을 이용하면, O(N)시간에 힙을 초기화할 수 있음.

*Heapification

전체 트리의 아래쪽 서브 트리부터 힙 불변성을 만족하도록 힙을 업데이트.

## 관련 문제

- Baekjoon

[최소 힙(1927)](/baekjoon/baekjoon-1927)

[최대 힙(11279)](/baekjoon/baekjoon-11279)

[가운데를 말해요(1655)](/baekjoon/baekjoon-1655)

# 그래프

트리는 계층적 데이터를 표현하는 좋은 방법이지만, 하나의 노드에서 다른 노드로 이동하는 경로가 하나만 존재해야 함. 따라서 순환적인 종속성을 표현할 수 업승ㅁ.

## 구현

### 인접 행렬(Adjacency Matrix)

노드가 N개 있을 경우, N x N 크기의 2차원 배열로 표현.

### 인접 리스트

엣지의 id pair 만을 저장함.


# Reference

![코딩테스트를 위한 자료구조와 알고리즘 with C++](/assets/images/230112.png){: width="25%" height="25%"}

