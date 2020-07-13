# 개념
[참고:그리디 알고리즘](https://m.blog.naver.com/ndb796/221242106787)  
그리디 알고리즘(Greedy Algorithm)은 문제를 해결하는 과정에서 그 순간순간마다 최적이라고 생각되는 결정을 하는 방식으로 진행하여 최종 해답에 도달하는 문제 해결 방식이다.  
이는 항상 최적의 결과를 도출하는 것은 아니지만 어느정도 최적의 해에 근사한 값을 **빠르게** 구할 수 있다는 장점이 있다.

기본적으로 그리디 알고리즘은 무조건 큰 경우대로, 무조건 작은 경우대로, 무조건 긴 경우대로, 무조건 짧은 경우대로 등 극단적으로 문제에 접근한다는 점에서 `정렬 기법과 함께 사용`되는 경우가 많다.

# 문제
## 회의실배정
[문제](https://www.acmicpc.net/problem/1931)

일찍 끝나는 회의를 먼저 넣어야 다른 회의를 더 많이 할 수 있다.  
또한 일찍 시작하는 회의를 먼저 넣어줘야한다.  

```python
import sys
n = int(input())
arr = [None for _ in range(n)]
for i in range(n):
    x,y = map(int, sys.stdin.readline().split())
    arr[i] = (x,y)
arr = sorted(arr, key=lambda x: (x[1],x[0]))

ans = [arr.pop(0)]
for a in arr:
    if a[0]>=ans[-1][1]:
        ans.append(a)

print(len(ans))
```

## ATM
[문제](https://www.acmicpc.net/problem/11399)

ATM이 1대밖에 없고, 각 사람이 돈을 인출하는데 걸리는 시간이 정해져있다면, 각 사람이 돈을 인출하는데 필요한 시간의 합의 최솟값을 구하기 위해서는
`인출시간이 짧은 사람이 먼저 뽑으면 된다.`

```python
n = int(input())
arr = list(map(int, input().split()))

arr.sort()
s = 0
all_sum = 0
for a in arr:
    s += a 
    all_sum += s 

print(all_sum)
```

## 잃어버린 괄호
[문제](https://www.acmicpc.net/problem/1541)

-로 split한 후 +연산을 해준다.(+를 괄호로 묶는다는 뜻)

```python
e = [sum(map(int, x.split('+'))) for x in input().split('-')]
print(e[0]-sum(e[1:]))
```
