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

```reserve(capacity)``` : 벡터ㅔ서 사용할 용량을 지정

```shrink_to_fit()``` : 여분의 메모리 공간 해제

vector는 array에 대한 좋은 대안이고, 더욱 많은 유연성을 제공하면서도 성능면에서 큰 저하가 없기 때문에, 매우 자주 사용되는 STL임.
{: .notice}

# Reference

![코딩테스트를 위한 자료구조와 알고리즘 with C++](../_site/assets/images/230112.png)

