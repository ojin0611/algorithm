# 이분탐색

# 개념







# 문제

## 수 찾기

[문제](https://www.acmicpc.net/problem/1920)

이분 탐색을 이용해 수를 찾는 문제다.

arr[:mid], arr[mid+1:] 를 이용해서 재귀적으로 풀어도 되지만 일반적으로는 `start`와 `end`를 이용해 문제를 푼다.

#### binary search 풀이

```python
import sys

n = int(sys.stdin.readline().rstrip())
arr = list(map(int, sys.stdin.readline().split()))

m = int(sys.stdin.readline().rstrip())
nums = list(map(int, sys.stdin.readline().split()))

arr.sort()

def isInArr(x, arr, start, end):
    if start > end: return 0
    mid = (start + end) // 2
    
    if x < arr[mid]:
        return isInArr(x, arr, start, mid-1)
    elif x > arr[mid]:
        return isInArr(x, arr, mid+1, end)
    else:
        return 1
    
for a in nums:
    print(isInArr(a, arr, 0, len(arr)-1))
```



번외로 list와 set의 자료구조에 대한 이해가 있다면 이분탐색보다도 더 빨리 문제를 풀 수 있다.

[관련 링크](https://tkql.tistory.com/37)

Python의 자료형별로 쓰이는 경우

| list                      | tuple                       | set                  | dict          |
| ------------------------- | --------------------------- | -------------------- | ------------- |
| 데이터 수정이 필요한 경우 | 데이터의 읽기만 필요한 경우 | 중복을 불허하는 경우 | 상황에 따라.. |
| 순서가 필요한 경우        |                             | 순서가 불필요한 경우 |               |



특히 set의 이점은 탐색에 있다. 

예를 들어, x가 배열 arr에 포함됐는지 확인하기 위해 보통 `x in arr`를 사용한다.

리스트는 평균적으로 O(n)의 시간이 들지만 set은 O(1)의 시간으로, 자료구조가 클수록 엄청난 시간적 이점이 있다.



#### set을 이용한 풀이

```python
import sys
input = sys.stdin.readline

def BOJ_1920():
    n,A,m = input(),set(input().split()),input()
    res=""
    for i in input().split():
        res += "1\n" if i in A else "0\n"
    print(res)
BOJ_1920()
```

