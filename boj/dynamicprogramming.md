# 개념
## 동적 계획법 - Dynamic Programming
**동적 계획법(DP)**은 큰 문제를 작은 문제로 나눠서 푸는 알고리즘이다. Dynamic은 DP라는 말을 처음 사용한 벨만이 멋있어서 선택한 단어다.  
분할 정복과 비슷한데, DP와 달리 분할 정복은 계산한 부분문제를 한 번만 쓰고 더 이상 쓰지 않기 때문에 `메모이제이션`이 필요하지 않다.

## 메모이제이션 - Memoization
DP에서는 작은 문제들이 반복되고, 이 작은 문제들의 결과값이 항상 같다. 이를 이용해 한 번 계산한 작은 문제를 저장해놓고 다시 사용하는데 이것을 메모이제이션이라고 한다.

## Top-Down vs Bottom-Up
**Top-Down 방법**은 재귀와 같은 방식으로 위에서 아래로 내려오는 방식이다.
함수 호출을 줄이기 위해 메모이제이션을 사용한다.

```python
def fibonacci_top_down(n):
    if memo[n] > 0:
        return memo[n]
    
    if n <= 1: # 0,1
        memo[n] = n
        return memo[n]
    else:
        # 바로 return하지 않고, memo에 저장한다는 것이 단순 재귀와 다르다.
        memo[n] = fibonacci_top_down(n-1) + fibonacci_top_down(n-2)
        return memo[n]

memo = [0 for i in range(100)]
```

**Bottom-Up 방법**은 작은 문제부터 차근차근 구해나가는 방법이다. for문을 이용해 처음부터 다음값을 계산해나간다.

```python
def fibonacci_bottom_up(n):
    if n<=1: return n
    first = 0
    second= 1
    for i in range(0, n-1):
        next = first + second
        first = second
        second = next
    return next
```


# 문제
## 단순 재귀로 피보나치 함수를 구하면 함수 호출 개수가 기하급수적으로 늘어납니다.
[문제](https://www.acmicpc.net/problem/1003)

```python
import sys
T = int(input())
q = [int(sys.stdin.readline()) for _ in range(T)]

dp = [[1,0], [0,1]] # fibo(0) > 0만 1번 호출, fibo(1) > 1만 1번 호출
for i in range(2,max(q)+1):
    # 호출되는 횟수가 피보나치 수열로 증가
    dp.append([dp[i-2][0]+dp[i-1][0], dp[i-2][1]+dp[i-1][1]])
for i in q:
    print(dp[i][0], dp[i][1])
```

## 점화식의 값을 특정 상수로 나눈 나머지를 구하는 문제
[문제](https://www.acmicpc.net/problem/1904)
```python
# 큰 값끼리 더하면 시간이 오래걸리므로, 나머지끼리 더해준다.
def tile(n):
    if n<=2: return n

    temp1 = 1
    temp2 = 2
    for i in range(0,n-2):
        next = temp1 + temp2
        temp1 = temp2 % 15746
        temp2 = next % 15746
    
    return next % 15746

print(tile(int(input())))
```

## 옆집과 다른 색
```python
import sys
N = int(input())

# 각 집의 r,g,b 가격을 arr에 저장
arr = []
for _ in range(N):
    r,g,b = map(int, sys.stdin.readline().split())
    arr.append([r,g,b])

# i번째집을 r,g,b로 색칠했을 때 비용 총합을 bottom-up 방법으로 dp[i-1]에 저장
dp = []
for i in range(len(arr)):
    if i==0: dp.append(arr[i])
    else:
        r = min(dp[i-1][1], dp[i-1][2]) + arr[i][0]
        g = min(dp[i-1][0], dp[i-1][2]) + arr[i][1]
        b = min(dp[i-1][0], dp[i-1][1]) + arr[i][2]
        dp.append([r,g,b])
print(min(dp[N-1]))
```

## 정수 삼각형
[문제](https://www.acmicpc.net/problem/1932)  
```python
import sys
N = int(input())

triangle = []
for _ in range(N):
    input_list = list(map(int, sys.stdin.readline().split()))
    triangle.append([0] + input_list + [0])
    
for i, arr in enumerate(triangle):
    if i==0: before = arr
    else:
        next = [0]
        for j in range(1, len(arr)-1):
            next.append(arr[j] + max(before[j-1], before[j]))
        next.append(0)
        before = next
print(max(next))
```