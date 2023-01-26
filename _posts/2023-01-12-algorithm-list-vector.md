---
title: 리스트, 스택, 큐
categories: 
  - Algorithm
relative: true
comments: true
---

# 연속된 자료구조 (Contiguous data structures)

## 배열

```cpp
int arr[size]; // 정적 배열
int *arr = (int*) malloc (size * sizeof(int)); // C의 동적 배열
int *arr = new int[size]; // C++의 동적 배열
```

정적 배열은 Stack Memory에 할당되고, 동적 배열은 Heap에 할당된다. 두 유형 모두 다양한 연산에서 동일한 성능을 나타내고, 필요에 따라 선택하여 사용하면 된다.

> Cache Locality
> Contiguous data structures에서 하나의 원소에 접근할 때, 주변의 원소도 cache로 가져오므로, 성능이 향상됨. (cache locality)

### 제약사항

1. 메모리 할당과 해제를 수동으로 처리하므로, memory leak의 가능성 존재.
2. 범위를 벗어나는 참조를 검사하지 않음.
3. Deep copy가 동작하지 않음.

위 제약사항의 보완을 위해 std::array 를 사용할 수 있음.

## std::array

array는 메모리를 자동으로 할당하고 해제한다.

```cpp
using namespace std;

array<int, 10> arr1; // 크기가 10인 int 타입 배열 선언
arr1[0] = 1;

array<int, 4> arr2 = {1, 2, 3, 4};
for (int i = 0; i < arr2.size(); i++) {
    cout << arr2[i] << " ";
}
//output : 1 2 3 4
```

array는 ```at(index)``` method를 제공하여, index 값의 유효성을 검사할 수 있음. 유효하지 않으면 std:out_of_range exception 발생.

함수에 array 객체를 전달할 경우 기본적으로 Deep Copy가 동작. 

배열의 원소를 차례대로 접근하는 연산. 아래의 방법으로 배열의 크기를 정확하게 지정하지 않아도 탐색이 가능함.

```cpp
using namespace std;

array<int, 5> arr = {1, 2, 3, 4, 5};
for (auto element : arr) {
    cout << element << " ";
}
//output : 1 2 3 4 5
```

추가로, array는 아래의 반복자를 지원하며 deep comparison을 지원하여 배열간 direct 비교 가능.

```cpp
using namespace std;

array<int, 5> arr = {1, 2, 3, 4, 5};
arr.front(); // 1
arr.back(); // 5
// 배열 객체의 실제 포인터 반환
*(arr.data() + 1); // 2

array<int, 5> arr2 = {1, 2, 3, 4, 5};
arr == arr2 // true
```

## std::vector

array 에는 다음과 같은 단점이 존재함.

1. array의 크기는 컴파일 시간에 결정되는 상수이어야 함.
2. 크기가 고정되어 원소를 추가하거나 삭제할 수 없음.

vector는 가변 크기 배열로 데이터의 크기를 제공할 필요가 없음.

```cpp
#include <vector>
using namespace std;

vector<int> vec; // 크기가 0인 벡터 선언
vector<int> vec = {1, 2, 3, 4, 5}; // 크기가 5이고, 초깃값이 있는 벡터 선언
vector<int> vec(10); // 크기가 10인 벡터 선언
vector<int> vec(10, 5); // 크기가 10이고 모든 원소가 5인 벡터 선언
```

백터에 새로운 원소를 추가하려면 ```push_back()``` or ```insert()```를 이용.

```push_back()``` 은 벡터의 맨 뒤에 원소를 삽입하며, 공간이 남아 있다면 O(1)이 소요됨. 그러나 공간이 충분치 않으면, 모든 원소를 복사/이동하며 O(n)이 소요됨. 대부분 상황에서 벡터는 용량이 부족할 때마다 두배로 용량을 늘리기 때문에 ```push_back()```의 평균 시간 복잡도는 O(1)에 가까움.

```insert()```는 반복자를 인자로 받으며, 원하는 위치에 원소를 추가함. 때문에 O(n)의 시간이 소요됨.

```cpp
#include <vector>
using namespace std;

vector<int> vec; // 크기가 0인 벡터 선언
vec.push_back(1); // {1}
vec.insert(vec.begin(), 0); // {0, 1}
vec.insert(find(vec.begin(), vec.end(), 1), 4); // 1 앞에 4 추가 {0, 4, 1}
```

벡터에서 원소를 제거하려면 ```pop_back()``` or ```erase()```를 이용. 같은 원리로 ```pop_back()```은 O(1), ```erase()```는 O(n)이 소요됨.

```cpp
#include <vector>
using namespace std;

vector<int> vec = {1, 2, 3, 4, 5}; // {1, 2, 3, 4, 5}
vec.pop_back(); // {1, 2, 3, 4}
vec.erase(vec.begin()); // {2, 3, 4}
vec.erase(vec.begin() + 1, vec.begin() + 2); // {2, 4}
```

그밖의 유용한 함수

```emplace()```, ```emplace_back()``` : 새로운 원소가 추가될 위치에서 원소 생성

```clear()``` : 모든 원소를 제거

```reserve(capacity)``` : 벡터에서 사용할 용량을 지정

```shrink_to_fit()``` : 여분의 메모리 공간 해제

vector는 array에 대한 좋은 대안이고, 더욱 많은 유연성을 제공하면서도 성능면에서 큰 저하가 없기 때문에, 매우 자주 사용되는 STL임.
{: .notice}

# 연결된 자료구조 (Linked data structures)

## std::forward_list

연속된 자료 구조에서는 데이터 중간에 자료의 추가/삭제가 비효율적임. 예를 들어, 탭을 지원하는 브라우저는 언제든 새로운 탭을 임의의 위치에 옮길 수 있어야 함. 기본적인 연결 리스트는 포인터를 하나 가지고 있고, 메모리 할당/해제가 가능해야 함.

forward_list에서 삽입/삭제는 O(1)이며, 대신 전체 리스트의 크기 반환이나, 첫번째 원소 제외한 나머지 원소에 직접 접근등은 지원하지 않음.

```cpp
std::forward_list<int> fwd_list = {1, 2, 3};

fwd_list.push_front(0); // {0, 1, 2, 3}

auto it = fwd_list.begin();

fwd_list.insert_after(it, 5); // {0, 5, 1, 2, 3}

fwd_list.insert_after(it, 6); // {0, 6, 5, 1, 2, 3}

fwd_list.pop_front(); // {6, 5, 1, 2, 3}

fwd_list.erase_after(it); // {5, 1, 2, 3}

fwd_list.erase_after(it, fwd_list.end()); // {5}
```

추가로, 모든 원소값을 검삭하여 삭제하는 `remove()`와 `remove_if()`도 지원함.

`remove_if()`에서는 [Lambda Expression](https://learn.microsoft.com/en-us/cpp/cpp/lambda-expressions-in-cpp?view=msvc-170)을 통해 predicate를 전달할 수 있음.

## std::list

List는 doubly-linked list구조로 되어 있기에, forward_list에 비해 더 많은 기능이 지원되고 더 많은 메모리를 사용함.

`insert_after()` 대신 `insert()`가 지원되고, `pop_back()`, `push_back()`등이 추가로 지원됨.

### 반복자 무효화

자료구조에서 반복자는 메모리 주소를 가리키는 포인터인데, 컨테이너가 변경되면 반복자가 무효화되어 예상치 못한 결과가 일어날 수 있음.

vector에서는 경우에 따라 새로 메모리를 할당하고, 기존의 원소를 복사하는 동작이 발생함. 따라서 원소 삽입/삭제 이후에 반복자가 무효화될 수 있음.

반면에, list 또는 forward_list에서 삽입 동작은 반복자 유효성에 영향을 미치지 않음. 삭제 동작에서도 삭제된 원소를 제외한 나머지 반복자는 그대로 사용할 수 있음.

# 기타 자료구조

## std::deque

deque은 연결 / 연속의 두가지 방식이 섞여 있는 형태임. (deque은 double-ended queue의 약자) Deque은 아래를 만족해야 함.

1. `push_front()`, `pop_front()`, `push_back()`, `pop_back()` 동작이 O(1)의 시간복잡도를 가져야 함.
2. 모든 원소에 대한 random access가 O(1)의 시간복잡도를 가져야 함.
3. 덱 중간에서의 원소 삽입 및 삭제는 O(n)으로 동작해야 하고, 실제로는 n/2단계로 동작함.

Deque은 단일 메모리 청크를 사용하지 않고, 대신 여러 개의 메모리 청크를 사용함. 이 경우, 청크의 인덱스 및 크기를 이용해 어느 청크인지 특정할 수 있으므로 O(1)로 random access가 가능함. `push_front()`의 경우, 첫 번째 청크에 여유 공간이 없다면 새로운 청크를 할당하고, 이 주소를 첫번째 청크로 설정함. 이 경우 메모리 공간할당은 필요하지만, 원소를 이동시킬 필요가 없음.

## 컨테이너 어댑터

위의 컨테이너들은 매우 Low 개념의 자료구조를 바탕으로 만들어짐. C++에는 이런 컨테이너들을 기반으로 만들어지 컨테이너가 있는데, 대부분 유용한 기능을 제공하고 효율적임.

### Stack

Deque를 기반으로 만들어졌으며 LIFO구조를 사용 (`push()`, `pop()`). 모든 연산의 시간복잡도가 O(1)

### Queue

Deque를 기반으로 만들어졌으며 FIFO구조를 사용 (`push()`, `pop()`). 모든 연산의 시간복잡도가 O(1)

### Priority Queue

Heap의 자료구조를 제공. 최대/최소 원소에 접근하는 동작이 O(logn)으로 동작. Vector를 기반으로 만들어졌으며, 기본적으로 max heap이 생성됨.

# Reference

![코딩테스트를 위한 자료구조와 알고리즘 with C++](/assets/images/230112.png){: width="25%" height="25%"}

