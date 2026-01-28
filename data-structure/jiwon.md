## 📌 해시 (Hash)

### DS-005

해시 자료구조에 대해 설명해 주세요.

![Hash Structure](https://upload.wikimedia.org/wikipedia/commons/thumb/7/7d/Hash_table_3_1_1_0_1_0_0_SP.svg/960px-Hash_table_3_1_1_0_1_0_0_SP.svg.png)

> 해시 구조는 키(Key)와 값(Value)쌍으로 이루어진 데이터를 해시값으로 매핑하여 저장하는 자료구조

<동작 원리>

1. 해시 함수(Hash Function)를 사용하여 키를 고정된 길이 정수값으로 구성된 해시값으로 변환
2. 해시값을 이용해 해시 테이블(Hash Table) 내의 특정 인덱스에 접근
   1. 인덱스는 `index = hash(key) % table_size` 로 계산
3. 테이블의 해당 인덱스에 값을 저장하거나 검색

- 충돌(Collision)이 발생할 경우, 충돌 해결 기법(Linked List, Double Hashing 등)을 사용하여 데이터를 처리
- 해시 테이블의 크기가 일정 수준 이상으로 커지면, 리사이징(Resizing)을 통해 더 큰 테이블로 확장

*왜 해시 자료구조를 사용할까?*

- 검색
  - 평균: O(1)
  - 최악: O(n)
- 삽입
  - 평균: O(1)
  - 최악: O(n)
- 삭제
  - 평균: O(1)
  - 최악: O(n)

<종합>

- 빠른 속도 : 해당 키에 대한 해시값을 계산하여, 바로 해당 인덱스에 접근할 수 있기 때문에 빠르게 데이터를 검색, 삽입, 삭제할 수 있다.
  - 충동로 인해 최악의 경우 Linked List에 저장된 값들을 탐색하기에 O(n)의 시간이 소요될 수 있다.
- 효율적인 메모리 사용 : 해시 테이블은 필요한 만큼의 메모리를 동적으로 할당할 수 있어, 메모리 낭비를 줄일 수 있다.
- 유연성 : 다양한 데이터 타입을 키로 사용할 수 있어, 다양한 상황에 적용할 수 있다.

### DS-006

값이 주어졌을 때, 어떻게 하면 충돌이 최대한 적은 해시 함수를 설계할 수 있을까요?

1. 입력 데이터를 골고루 분배하기
   1. 데이터 삽입 시의 충돌 발생 확률을 최대한 줄이기 위해
2. 적절한 크기의 해시 테이블 사용
   1. 보통 소수(prime number) 크기의 테이블을 사용하여 충돌 확률 감소
   2. 로드 팩터(load factor)를 적절히 유지 (예: 0.7 이하)
      1. 로드 팩터 = (저장된 데이터의 수) / (해시 테이블의 크기)
3. 복잡한 연산 사용
   1. 단순한 연산보다 복잡한 연산을 사용하여 해시값을 생성

### DS-007

해시값이 충돌했을 때, 어떤 방식으로 처리할 수 있을까요?

크게 두 가지의 방법이 있다.

1. Separate Chaining (Open Hashing)
   1. 같은 인덱스에 여러 개의 값을 저장하기 위해, 각 인덱스에 연결 리스트(Linked List)나 트리(Tree)를 사용
   2. 충돌이 발생하면, 해당 인덱스의 리스트나 트리에 새로운 값을 추가
   3. 검색 시, 해당 인덱스의 리스트나 트리를 순회하여 값을 찾음
   4. 장점: 구현이 간단하고, 해시 테이블의 크기를 미리 정하지 않아도 됨
   5. 단점: 충돌이 많이 발생하면, 리스트나 트리의 길이가 길어져 성능이 저하될 수 있으며, 추가적인 메모리 사용이 필요
2. Open Addressing (Closed Hashing)
   충돌이 발생하면, 해시 테이블 내의 다른 빈 슬롯을 조사(probe)하여 키 값을 저장. 포인터를 사용하지 않기에 구현이 간편하고 검색이 좀 더 빠름
   1. Linear Probing
      1. 충돌이 발생하면, 다음 인덱스를 순차적으로 검사하여 빈 슬롯을 찾음
      2. 장점: 구현이 간단하고, 메모리 사용이 효율적
      3. 단점: 클러스터링(Clustering) 현상이 발생할 수 있어 성능이 저하될 수 있음(특정 구간에만 값이 몰리는 현상)
        - 왜 성능 저하인가?
           1. 클러스터링이 발생하면, 충돌이 발생할 확률이 높아지고, 탐사 과정에서 더 많은 슬롯을 검사해야 하기 때문에 성능이 저하됨
   2. Quadratic Probing
      1. 충돌이 발생하면, 제곱수 간격으로 슬롯을 검사하여 빈 슬롯을 찾음 (예: 1, 4, 9, 16, ...)
      2. 장점: Linear Probing보다 클러스터링 현상이 덜 발생
      3. 단점: 특정 테이블 크기에서 모든 슬롯을 검사하지 못할 수 있음
   3. Double Hashing
      1. 충돌이 발생하면, 원래 해시 함수와 다른 별개의 해시 함수를 추가로 사용하여 새로운 인덱스를 계산하고, 해당 인덱스를 검사하여 빈 슬롯을 찾음 (해시 함수 값이 같더라도 키가 다르면 다른 인덱스를 생성)
      2. 장점: 클러스터링 현상이 거의 발생하지 않음 (이전 방법들이 일정한 규칙을 사용함에 따라 발생하는 클러스터링을 방지)
      3. 단점: 구현이 복잡하고, 추가적인 해시 함수 연산이 있기에 함수의 성능 자체가 전체적인 해시 테이블의 성능에 영향을 미침

### DS-008

본인이 사용하는 언어에서는, 어떤 방식으로 해시 충돌을 처리하나요?

<파이썬>

- 파이썬은 해시를 딕셔너리 자료구조로 구현
- 충돌 해결 방식: Open Addressing

출처 : Cpython의 딕셔너리 구현 방식
  Cpython은 파이썬의 구현체 중 하나로, C 언어로 작성되어 있음

Objects/dictobject.c 파일의 1129번째 줄
> /*
The basic lookup function used by all operations.
This is based on Algorithm D from Knuth Vol. 3, Sec. 6.4.
**Open addressing** is preferred over chaining since the link overhead for
chaining would be substantial (100% with typical malloc overhead).

The initial probe index is computed as hash mod the table size. Subsequent
probe indices are computed as explained earlier.

All arithmetic on hash should ignore overflow.

_Py_dict_lookup() is general-purpose, and may return DKIX_ERROR if (and only if) a
comparison raises an exception.
When the key isn't found a DKIX_EMPTY is returned.
*/

### DS-009

Double Hashing 의 장점과 단점에 대해서 설명하고, 단점을 어떻게 해결할 수 있을지 설명해 주세요.

1. Double Hashing
      1. 충돌이 발생하면, 원래 해시 함수와 다른 별개의 해시 함수를 추가로 사용하여 새로운 인덱스를 계산하고, 해당 인덱스를 검사하여 빈 슬롯을 찾음 (해시 함수 값이 같더라도 키가 다르면 다른 인덱스를 생성)
      2. 장점: 클러스터링 현상이 거의 발생하지 않음 (이전 방법들이 일정한 규칙을 사용함에 따라 발생하는 클러스터링을 방지)
      3. 단점: 구현이 복잡하고, 추가적인 해시 함수 연산이 있기에 함수의 성능 자체가 전체적인 해시 테이블의 성능에 영향을 미침

<해결 방안>

1. 두번째 해시 값이 0이 되지 않도록 설계
   1. 두번째 해시 함수가 0을 반환하면, 무한 루프에 빠질 수 있음
2. 테이블 크기를 소수(prime number)로 설정
   1. 해시 충돌 확률을 줄이고, 두번째 해시 값이 테이블 크기와 서로소가 되도록 하여 모든 슬롯을 탐색할 수 있게 함

### DS-010

Load Factor에 대해 설명해 주세요. 본인이 사용하는 언어에서의 해시 자료구조는 Load Factor에 관련한 정책이 어떻게 구성되어
있나요?

- 로드 팩터 : 로드 팩터(load factor)는 해시 테이블의 성능을 평가하는 지표로, 저장된 데이터의 수를 해시 테이블의 크기로 나눈 값
  - 로드 팩터 = (저장된 데이터의 수) / (해시 테이블의 크기)
- 로드 팩터가 높을수록 충돌이 발생할 확률이 높아져 성능이 저하될 수 있음

파이썬의 load Factor 정책

> if ((size_t)so->fill*5 < mask*3)
    return 0;
return set_table_resize(so, so->used>50000 ? so->used*2 : so->used*4);

- 파이썬의 딕셔너리는 로드 팩터가 약 2/3 (약 0.66) 이상이 되면, 자동으로 리사이징(Resizing)을 수행
- 리사이징 시, 해시 테이블의

### DS-011

다른 자료구조와 비교하여, 해시 테이블은 멀티스레드 환경에서 심각한 수준의 Race Condition 문제에 빠질 위험이 있습니다. 성능 감소를 최소화 한 채로 해당 문제를 해결할 수 있는 방법을 설계해 보세요.

1. Segment Locking
   1. 해시 테이블을 여러 개의 세그먼트(segment)로 나누고, 각 세그먼트에 별도의 락(lock)을 적용
   2. 서로 다른 세그먼트에 접근하는 스레드들은 동시에 작업할 수 있어 성능 저하를 최소화
   3. 단점 : 세그먼트 크기를 적절히 설정하지 않으면, 락 경합(lock contention)이 발생할 수 있음
      1. 락 경합 : 여러 스레드가 동일한 락을 획득하려고 경쟁하는 상황
2. Lock Free Hash Table
   1. 락을 사용하지 않고, 원자적 연산(atomic operations)과 비교-교환(compare-and-swap) 기법을 사용하여 동시성 문제를 해결
   2. 성능 저하를 최소화하면서도 높은 동시성을 제공
   3. 단점 : 구현이 복잡하고, 특정 상황에서 성능이 저하될 수 있음
      1. 예) ABA 문제 : 한 스레드가 값을 읽고, 다른 스레드가 그 값을 변경한 후 다시 원래 값으로 되돌리는 상황에서 발생하는 문제
          1. 쓰레드 T1이 값 A를 읽는다
          2. 다른 쓰레드 T2가 값 A → B로 변경
          3. 또 T2가 B → 다시 A로 변경
          4. T1은 다시 CAS를 실행: "값이 A니까 나는 성공"
          5. 하지만 T1은 중간에 값이 B였다는 사실을 모른다
      2. 즉, 값이 동일하더라도 이후의 단계에 변경이 있었음을 인지하지 못하는 문제가 있음
   4. 해결 방안 : 태그(tag) 또는 버전 번호(version number)를 사용하여 값의 변경 여부를 추적

## 📌 힙 (Heap)

### DS-018

힙에 대해 설명해 주세요.

![heap](https://upload.wikimedia.org/wikipedia/commons/thumb/3/38/Max-Heap.svg/500px-Max-Heap.svg.png)

- 힙(Heap)은 완전 이진 트리(Complete Binary Tree)의 일종으로, 부모의 값이 자식의 값보다 크거나(최대 힙, Max Heap) 작아야(최소 힙, Min Heap) 하는 특성을 가진 자료구조
- 힙은 주로 우선순위 큐(Priority Queue)를 구현하는 데 사용되며, 힙 정렬(Heap Sort) 알고리즘에도 활용됨

### DS-019

힙을 배열로 구현한다고 가정하면, 어떻게 값을 저장할 수 있을까요?

아래와 같은 단계로 heap을 구현할 수 있다.

1. 배열(리스트) 끝에 새 값을 추가한다.
2. 추가한 원소의 인덱스를 구한다.
3. 부모 인덱스를 구하여 값을 비교한다.
4. 새 값이 부모의 값보다 작으면 값을 교환한다.
5. 인덱스를 갱신한다. (자리를 바꿨으므로, 새로 추가한 값의 인덱스가 변했다.)
6. 3번으로 돌아가서 같은 과정을 반복하되, 루트에 도달하면 종료한다.
7. 새 값이 부모의 값보다 크거나 같으면 종료한다.

```
def heapify(arr):
    # 가장 마지막 인덱스부터 탐색 시작
    current = start = len(arr)-1
    while start > 0:
        # 교환 여부 저장
        is_swaped = False
        while current > 0:
            parent = (current - 1) // 2
            if arr[parent] > arr[current]:
                arr[parent], arr[current] = arr[current], arr[parent]
                current = parent
                is_swaped = True
            else:
                break
        if is_swaped:
            current = start
        else:
            current = start = current - 1
```

### DS-020

힙의 삽입, 삭제 방식에 대해 설명하고, 왜 이진탐색트리와 달리 편향이 발생하지 않는지 설명해 주세요.

- BST의 편향 : 트리 자료구조에서 특정 방향으로 치우쳐서 균형이 무너진 상태
- 힙은 편향이 발생하지 않는 이유 : 삽입과 삭제에서 힙의 완전이진트리 조건을 충족하기 위해 트리를 재구성하기 때문

1. 삽입
   1. 가장 왼쪽에(가장 작은 수) 새로운 노드를 삽입하고, 부모 노드와 비교하며 Down-Top 방식으로 트리를 재구성
2. 삭제
   1. 힙의 삭제는 루트 노드의 삭제만 허용됨
   2. Top-Down 방식으로 자식 노드와의 조건을 충족하는지 확인하며 트리를 재구성

### DS-021

힙 정렬의 시간복잡도는 어떻게 되나요? Stable 한가요?

<힙 트리 시간복잡도>
- 삽입 시간 복잡도 : O(logn)
  - 최악의 경우 트리의 맨 아래부터 루트노드까지 이동하기에 트리 높이만큼의 시간복잡도를 지님
- 삭제 시간 복잡도 : O(longn)
  - 최악의 경우 트리의 루트노드부터 맨 아래까지 이동하기에 트리 높이만큼의 시간복잡도를 지님
- 참조 시간 복잡도 : O(1)
  - 힙은 기본적으로 루트 노드만 참조가 가능

정렬 시에는 여기에 n개의 노드를 확인하는 절차가 추가되기에 n * O(logn) = O(nlogn) 만큼의 시간이 걸리게 된다.

- 힙은 Unstable 하다.
  - 트리를 구성하며 값이 swap될 때, 이전 순서가 보장되지 않음 (특히 같은 값을 지는 노드가 있을 경우)

---
<출처>

- [Collision Resolution Techniques](https://www.geeksforgeeks.org/dsa/collision-resolution-techniques/)
- [[자료구조] 개방 주소법 (open addressing) - 선형 조사법, 이차 조사법, 이중 해시법](https://laurent.tistory.com/entry/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EA%B0%9C%EB%B0%A9-%EC%A3%BC%EC%86%8C%EB%B2%95-Open-Addressing-%EC%84%A0%ED%98%95-%EC%A1%B0%EC%82%AC%EB%B2%95-%EC%9D%B4%EC%B0%A8-%EC%A1%B0%EC%82%AC%EB%B2%95-%EC%9D%B4%EC%A4%91-%ED%95%B4%EC%8B%9C%EB%B2%95)
- [Hash Functions and Types of Hash functions](https://www.geeksforgeeks.org/dsa/hash-functions-and-list-types-of-hash-functions/)
- [해시(Hash)와 해시 충돌 해결 방법](https://ryu-e.tistory.com/87)
- [좌충우돌, 파이썬으로 자료구조 구현하기](https://wikidocs.net/194546)
