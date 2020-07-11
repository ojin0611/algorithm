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

## 가장 긴 증가하는 부분수열 (Longest Increasing Subsequences)
[문제](https://www.acmicpc.net/problem/11053)  
수열 A가 주어졌을 때, 가장 긴 증가하는 부분 수열을 LIS라고 한다.

예를 들어, 수열 A = {10, 20, 10, 30, 20, 50} 인 경우에 가장 긴 증가하는 부분 수열은 A = {10, 20, 10, 30, 20, 50} 이고, 길이는 4이다.

이를 구하기 위해서는, 첫 번째 값부터 자기 자신까지의 LIS 길이를 저장하는 배열 DP를 만들어준다. i-1번째 값까지는 모두 LIS의 길이를 알고있으므로 이후에 i번째 값이 추가될때마다 이를 참고하여 DP[i] 값을 구해준다.

```python
N = int(input())
arr = list(map(int, input().split()))
dp = [1 for _ in range(N)] # 자기 자신만 고려하면 길이 1

for i in range(N): # 기준이 되는 값. arr[i]
    for j in range(i): 
        if arr[i] > arr[j] and dp[i] <= dp[j]:
        # 지금 갖고있는 LIS의 길이보다 더 길거나 같다면 (같으면 자기 자신을 추가할 수 있으므로 가능)
            dp[i] = dp[j] + 1 # 그 전의 길이 +1을 저장
            # 이 때 dp[i]가 의미하는 건 i번째 숫자까지의 LIS

print(max(dp))
```

## 최장 공통 부븐 수열 - Longest Common Subsequence
[문제](https://www.acmicpc.net/problem/9251) /
[풀이](http://melonicedlatte.com/algorithm/2018/03/15/181550.html)  

테이블의 값은 해당 값까지의 두 문자열의 LCS의 길이다.  

### LCS의 길이를 구하는 방법
윗줄부터 채우면, 윗줄은 이미 LCS라고 볼 수 있다.  
같은 문자를 만나면 좌상향대각+1 (두 문자열이 만나기 전의 LCS 길이를 찾아야하기 때문)   
다른 문자를 만나면 max(좌, 상향) 

### Example
|-|-|A|C|A|Y|K|P|
|-|-|-|-|-|-|-|-|
-|0|0|0|0|0|0|0|
C|**0**|0|**1**|0|0|0|0|
A|0|**1**|1|**2**|2|2|2
P|0|1|1|2|2|2|3
C|0|1|2|2|2|**2**|3
A|0|1|2|3|**3**|**3**|3
K|0|1|2|3|3|4|4
ex) A가 ACAYKP의 A와 만날때마다 좌상향대각+1을 한 것을 볼 수 있음  
ex) A,K는 다르므로 2,3중 큰 값인 3을선택

### LCS를 구하는 방법
가장 긴 수열을 찾는 방법은 역추적이다. 마지막 줄에서 max값 M을 찾고, M-1이 M이 된 지점(좌상향)을 계속 역추적하는 방법을 이용하면 최장수열을 찾을 수 있다.
|-|-|A|C|A|Y|K|P|
|-|-|-|-|-|-|-|-|
-|0|0|0|0|0|0|0|
C|<span style="color:red">0</span>|0|1|0|0|0|0|
A|0|**<span style="color:red">1</span>**|1|2|2|2|2
P|0|<span style="color:red">1</span>|1|2|2|2|3
C|0|1|**<span style="color:red">2</span>**|2|2|2|3
A|0|1|2|**<span style="color:red">3</span>**|<span style="color:red">3</span>|3|3
K|0|1|2|3|3|**<span style="color:red">4</span>**|4

굵은 빨간색 값들을 보아 ACAK가 LCS임을 알 수 있다.


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

## 점화식의 값을 특정 상수로 나눈 나머지
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
[문제](https://www.acmicpc.net/problem/1149)  
각 집마다 r,g,b로 칠했을 때의 비용을 저장하고 다음 집에서는 이전 집에서 칠하지 않은 색 중 더 낮은 값을 선택한다.

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
좌상향 우상향 대각 숫자 중에 더 큰 값을 선택해야한다. 편의를 위해 입력되는 숫자 배열 양 옆에 0을 추가해준다. 
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

## 1로 만들기
[문제](https://www.acmicpc.net/problem/1463)
```
n /= 3 if n % 3 == 0  
n /= 2 if n % 2 == 0  
n -= 1  
3가지 연산을 통해 n을 1로 만들기 위한 최소 횟수를 구하라.
```
  
bottom-up 방식으로 차곡차곡 쌓아서, 작은 숫자에서는 이미 최소 횟수를 구했다는 가정하에 더 큰 숫자에서 최소 횟수를 구하는 방법을 이용한다.

```python
n=int(input())

def one(n):
    if n<=4 or dp[n]: return dp[n]
    
    temp = [one(n-1)]
    if n%3 == 0: temp.append(one(n//3))
    if n%2 == 0: temp.append(one(n//2))
    return min(temp)+1

dp = [0,0,1,1,2] + [0 for _ in range(n)]
for i in range(5,n+1):
    dp[i] = one(i)

print(dp[n])
```

## 계단수
[문제](https://www.acmicpc.net/problem/10844)  
길이가 N-1인 계단수의 1의 자리 숫자를 이용하는 방법
```python
N = int(input()) # 숫자의 길이 

dp = [[0,1,1,1,1,1,1,1,1,1]] + [None for _ in range(N)]
for i in range(1, N+1):
    dp[i] = [dp[i-1][1] % 1000000000, 
    dp[i-1][0]+dp[i-1][2] % 1000000000,
    dp[i-1][1]+dp[i-1][3] % 1000000000, 
    dp[i-1][2]+dp[i-1][4] % 1000000000, 
    dp[i-1][3]+dp[i-1][5] % 1000000000, 
    dp[i-1][4]+dp[i-1][6] % 1000000000,
    dp[i-1][5]+dp[i-1][7] % 1000000000, 
    dp[i-1][6]+dp[i-1][8] % 1000000000, 
    dp[i-1][7]+dp[i-1][9] % 1000000000,
    dp[i-1][8] % 1000000000] 
    
print(sum(dp[N-1]) % 1000000000)
```

## 포도주 마시기
[문제]()

1~n번 잔에 포도주가 1~1000만큼 들어있을 때, 연속으로 3잔을 마시지 않고 포도주를 가장 많이 마시는 방법을 구하는 문제.  
```python
import sys

N=int(input())
stairs = [0]
for _ in range(N):
    stairs.append(int(sys.stdin.readline()))

dp = [None for _ in range(N+1)]
# dp[n] = ['n번째 마심 + n-1번째 마심', 'n번째 마심 + n-1번째 skip', 'n번째 skip' ]

for n in range(1,N+1):
    if n==1:
        dp[1] = [stairs[1], stairs[1], 0]
    elif n==2:
        dp[2] = [stairs[2]+dp[1][1], stairs[2] + dp[1][2], dp[1][0]]
    else:
        both = stairs[n] + max(dp[n-1][1], dp[n-1][2])
        now  = stairs[n] + max(dp[n-1][2], max(dp[n-2]))
        skip = max(dp[n-1])
        dp[n] = [both, now, skip]

print(max(dp[N]))
```

## 가장 긴 바이토닉 부분수열
[문제](https://www.acmicpc.net/problem/11054)

```python
N = int(input())
arr = list(map(int, input().split()))
dp = [1 for _ in range(N)] # 자기 자신만 고려하면 길이 1

for i in range(N): # 기준이 되는 값. arr[i]
    for j in range(i): 
        if arr[i] > arr[j] and dp[i] <= dp[j]:
        # 지금 갖고있는 LIS의 길이보다 더 길거나 같다면 (같으면 자기 자신을 추가할 수 있으므로 가능)
            dp[i] = dp[j] + 1 # 그 전의 길이 +1을 저장
            # 이 때 dp[i]가 의미하는 건 i번째 숫자까지의 LIS

# 위에서 구한 각 값까지의 LIS 길이를 시작으로, 각 값부터 감소하는 수열을 찾는다.
for i in range(N):
    for j in range(i):
        if arr[i] < arr[j] and dp[i] <= dp[j]:
            dp[i] = dp[j] + 1

print(max(dp))
```