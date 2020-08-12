# 이분탐색

# 개념

둘로 나눠 탐색하는 알고리즘이다. 첫 항부터 탐색하면 O(n)인데, 배열이 정렬돼있다면 중앙부터 탐색하여 탐색범위를 절반씩 줄일 수 있으므로 O(log2n)으로 탐색할 수 있다. 





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



## 숫자 카드 2

[문제](https://www.acmicpc.net/problem/10816)

이분탐색의 대상에 **중복**이 있는 경우다. 중복될 경우, 각 항이 몇 개씩 있는지 세는 것이 관건이다.

이분 탐색 알고리즘의 핵심 부분은 중앙값과 비교하는 부분인데, 중복이 없을 경우 어떤 조건문이 먼저 오든 상관없었지만 중복이 있는 경우 **항상** 같은지 먼저 비교해줘야한다.

같을 경우 해당 범위에서 count로 같은 개수를 셀 수 있는데, 만약 작거나 큰것을 먼저 조건문으로 걸어버리면 자칫하면 몇 개 빼먹을 수 있기때문이다.

추가로, 이 문제는 탐색의 대상뿐만 아니라 탐색하고자하는 숫자들도 중복이 허용되는데 이때는 시간의 효율성을 위해 dictionary를 이용해준다. 이미 찾았던 적 있는 숫자라면 바로 값을 얻어올 수 있다.

```python
import sys
N = int(sys.stdin.readline().rstrip())
N_CARDS = list(map(int, sys.stdin.readline().split()))

M = int(sys.stdin.readline().rstrip())
M_CARDS = list(map(int, sys.stdin.readline().split()))

N_CARDS.sort()

def binarySearch(x, arr, start, end):
    if start > end: 
        return 0
    mid = (start + end) // 2
    
    if x==arr[mid]: # 얘 먼저해줘야한다.
        return arr[start:end+1].count(x)
        
    elif x < arr[mid]:
        return binarySearch(x, arr, start, mid-1)
    
    else:
        return binarySearch(x, arr, mid+1, end)
    

ans = {}
for m in M_CARDS:
    if m not in ans.keys():
        ans[m] = binarySearch(m, N_CARDS, 0, len(N_CARDS)-1) 
    print(ans[m], end=" ")

```

