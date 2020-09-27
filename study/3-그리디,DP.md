# 그리디 풀이
## 프로그래머스 Greedy 1번 : 체육복
```python
def solution(n, lost, reserve):
    # 체육복이 있는 학생은 True, 없으면 False
    students = [True for _ in range(n)]
    
    # 잃어버린 학생들 False 표시
    for l in lost:
        students[l-1] = False
    
    # 여분이 있는 학생중에 본인은 잃어버리지 않은 학생 저장
    real_reserve = []
    
    # 여분이 있는 학생들 중
    for r in reserve:
        # 자기꺼가 없으면
        if not students[r-1]:
            # 자기꺼 채우기
            students[r-1] = True
        else:
            # 자기꺼 있으면 진짜 여분있는 학생 리스트에 추가
            real_reserve.append(r)
    
    # 진짜 여분있는 학생들이 자기 앞사람부터(그리디) 나눠주자
    for r in real_reserve:
        if r==1:
            if not students[r]:
                students[r] = True
                continue
            
        elif r==n:
            if not students[r-2]:
                students[r-2] = True
                continue
            
        else:
            if not students[r-2]:
                students[r-2] = True
                continue
            # 앞사람 다음은 뒷사람
            elif not students[r]:
                students[r] = True
                continue
    
    # 체육복이 있는 학생 수 
    answer = sum(students)
    return answer
```

## 프로그래머스 Greedy 2번 : 큰 수 만들기
### index 옮겨가면서 찾기 (1개 케이스 시간초과)
```python
def solution(number, k):
    numbers = list(map(int, number))
    idx = 0
    del_idx = [False for _ in range(len(numbers))]
    while k:
        # 끝까지 갔는데 앞에 지울게 없으면 뒤에 다 지우기
        if idx+k >= len(numbers):
            del_idx[idx] = True
            idx += 1
            k -= 1
            continue
            
        # k번째 이내의 내 뒤에 나보다 큰 수가 있으면 날 지우고 가라 
        for i in range(1,k+1):
            if numbers[idx + i] > numbers[idx]:
                del_idx[idx] = True
                idx += 1
                k -= 1
                break
        
        # 없다면 나는 k번째 이내에서 가장 큰 수! 통과~
        else:
            idx += 1
    
    answer = ''
    for i in range(len(number)):
        if not del_idx[i]:
            answer += number[i]
    
    return answer
```

### Stack을 이용한 풀이
[참고풀이](https://hellominchan.tistory.com/345)
```python
def solution(number, k):
    stack = []
    
    for i in range(len(number)):
        if not stack:
            stack.append(number[i])
            continue
        
        isRemove = False
        
        while stack and int(stack[-1]) < int(number[i]):
            stack.pop()
            k -= 1
            if not k:
                isRemove = True
                stack += number[i:]
                break
        
        if isRemove:
            break
        
        stack.append(number[i])
        
    return "".join(stack[:len(number) - k]) if k else "".join(stack)
```

## 프로그래머스 Greedy 3번 : 조이스틱
[참고풀이](https://hellominchan.tistory.com/350)
```python
# 그리디, 즉 각 상황마다 좌 우 중에 더 가까운쪽으로 간다.
def solution(name):
    answer = 0
    name = list(name)
    
    shouldChange = 0
    for n in name:
        if n != 'A':
            shouldChange += 1
    
    if not shouldChange:
        return 0
    
    goLeft = 0
    goRight = 0
    index = 0
    
    while 1:
        if name[index] != 'A':
            answer += min(ord('Z') - ord(name[index]) + 1, ord(name[index]) - ord('A'))
            
            name[index] = 'A'
            shouldChange -= 1
        
        if not shouldChange:
            break
            
        goLeft = goRight = index
        move = 0
 
        while 1:
            goRight += 1
            goLeft -= 1
            move += 1
 
            if goLeft < 0:
                goLeft = len(name) - 1
            if goRight > len(name) - 1:
                goRight = 0
 
            if name[goRight] != 'A':
                answer += move
                index = goRight
                break
            elif name[goLeft] != 'A':
                answer += move
                index = goLeft
                break
            
    return answer
```

## 프로그래머스 Greedy 4번 : 구명보트
```python
def solution(people, limit) :
    people.sort()
    cnt = 0

    # index로 접근!
    i = 0 
    j = len(people)-1
    while i<=j :
        cnt+=1
        # 최대 2명 태울수있으니까, 가벼운사람 1명 태울수있으면 태우고 아님 말고
        if people[j]+people[i]<=limit :
            i+=1
        j-=1
    return cnt
```

## 프로그래머스 Greedy 5번 : 섬 연결하기
[최소비용 신장트리](https://dhpark1212.tistory.com/entry/%EC%B5%9C%EC%86%8C%EB%B9%84%EC%9A%A9-%EC%8B%A0%EC%9E%A5%ED%8A%B8%EB%A6%ACMinimum-Cost-Spanning-Tree)
```python
def solution(n, costs):
    answer = 0
    costs.sort(key=lambda x: x[2]) # Cost 기준 정렬
    mst = set([costs[0][0]]) # Set
    while len(mst) != n:
        for i, cost in enumerate(costs):
            # 사이클을 형성하는 경우 패스
            if (cost[0] in mst) and (cost[1] in mst):
                continue
            if (cost[0] in mst) or (cost[1] in mst):
                mst.update([cost[0], cost[1]])
                answer += cost[2]
                break
    return answer
```


## 프로그래머스 Greedy 6번 : 단속카메라
```python
def solution(routes):
    routes.sort()
    routes.reverse()

    # routes: -5 -3 / -14 -5 / -18 -13 / -20 15
    answer = 0
    
    # 각 route가 camera에 걸쳤는지 체크
    check = [0] * len(routes)
    for i in range(len(routes)):
        # 해당 route를 카메라가 통과한 적이 없다면, 해당 루트의 출발지점에 camera 설치
        if check[i] == 0:
            camera = routes[i][0] 
            answer += 1
        
        # 해당 route 뒤의 route들이 카메라를 통과했는지 확인하기~
        for j in range(i, len(routes)):
            if routes[j][0] <= camera <= routes[j][1]:
                check[j]=1
    return answer
```

## 백준 2352 : 반도체 설계
LIS 문제 (시간 통과하려면 O(NlogN)으로 풀어야함)
[참고 링크](https://codedrive.tistory.com/336)
[bisect 모듈 이용](https://suri78.tistory.com/204)
```python
# O(n^2)
n = int(input())
ports = list(map(int, input().split()))

dp = [0]*n
dp[0] = 1
for i in range(1,n): # index 1씩 증가
    for j in range(i): # 앞의 애들 값 탐색하면서 가장 큰 애의 값에 +1
        if ports[i] > ports[j] and dp[i] < dp[j]+1:
            dp[i] = dp[j]+1
print(dp[-1])

# O(nlogn)
from bisect import bisect_left 
n = input() 
dest = list(map(int, input().split())) 
link = [] 
for d in dest: 
    if not link or link[-1] < d: 
        link.append(d) 
    else: 
        link[bisect_left(link, d)] = d 
print(len(link))
'''
def bisect_left(arr, val): 
    s, e = 0, len(arr)-1 
    while s <= e: 
        m = (s+e)//2 
        if arr[m] > val: 
            e = m-1 
        else: 
            s = m+1 
    return s 
    
n = input() 
dest = list(map(int, input().split())) 
link = [] 
for d in dest: 
    if not link or link[-1] < d: 
        link.append(d) 
    else: 
        link[bisect_left(link, d)] = d 
        
print(len(link))
'''

```

## 백준 1946 : 신입 사원
[참고링크](https://suri78.tistory.com/196)
```python
import sys 
for _ in range(int(sys.stdin.readline())): 
    n = int(sys.stdin.readline()) 
    # score를 서류 순으로 오름차순 정렬. 면접 등수만 카운트
    score = sorted([tuple(map(int, sys.stdin.readline().split())) for _ in range(n)], key=lambda x:x[0]) 
    p, ans = n+1, 0 
    for s, e in score: 
        if p > e: 
            ans += 1 
            p = e 
    sys.stdout.write("{}\n".format(ans))
```



# 동적 계획법 풀이

## 프로그래머스 DP 1번 : N으로 표현
[참고풀이](https://gurumee92.tistory.com/164)
```python
def solution(N, number):
    # 8개 이내로 못만들면 -1
    answer = -1
    DP = []

    # 1개로 만들 수 있는 숫자~ 8개
    for i in range(1, 9):
        # 가능한 숫자들을 set에 담을 것
        numbers = set()

        # NNN은 미리 넣어둠
        numbers.add( int(str(N) * i) )
        
        # N개를 이용하는 방법은 I번 + N-I번 이용하는 것 (단, I<N)
        for j in range(0, i-1):
            for x in DP[j]:
                for y in DP[-j-1]:
                    numbers.add(x + y)
                    numbers.add(x - y)
                    numbers.add(x * y)
                    
                    if y != 0:
                        numbers.add(x // y)

        if number in numbers:
            answer = i
            break
        
        DP.append(numbers)

    return answer
```

## 프로그래머스 DP 2번 : 정수 삼각형
```python

def solution(triangle):
    second_line = triangle.pop()
    while triangle:
        sum_list = []
        first_line = triangle.pop()
        for i in range(len(second_line)-1):
            sum_list.append(first_line[i] + max(second_line[i], second_line[i+1]))
        
        second_line = sum_list
    answer = second_line[0]
    return answer
```

## 프로그래머스 DP 3번 : 등굣길
```python
def solution(m, n, puddles):
    # 메모리 절약을 위해 이전, 현재만 사용.
    dp_last = [0 for _ in range(m)]
    dp_last[0] = 1
    dp_pres = [0 for _ in range(m)]
    
    # puddles를 앞으로 쓸 matrix에 맞게 숫자 설정
    for i in range(len(puddles)):
        puddles[i] = [puddles[i][1]-1, puddles[i][0]-1]
        
    # 첫 줄에 puddles 여부        
    for j in range(1, m):
        if [0, j] not in puddles:
            dp_last[j] += dp_last[j-1]
        else:
            break
    
    # 둘째 줄부터
    for i in range(1, n):
        for j in range(m):
            # 왼쪽 칸이 puddle
            if [i, j-1] not in puddles:
                dp_pres[j] += dp_pres[j-1] % 1000000007
            # 윗칸이 puddle
            if [i-1, j] not in puddles:
                dp_pres[j] += dp_last[j] % 1000000007
            # 현재 칸이 puddle
            if [i, j] in puddles:
                dp_pres[j] = 0
        
        # 이전, 현재 교체
        dp_last = dp_pres
        dp_pres = [0 for _ in range(m)]
    answer = dp_last[-1] % 1000000007
    return answer
```

## 프로그래머스 DP 4번 : 도둑질
```python
def solution(money):
    
    # 첫 집을 털고 마지막 집을 안털때
    dp = [money[0]]
    # 첫집을 털면 두번째집은 못터는데, 왜 money[1]이 여기있는지 잘 모르겠음.
    dp.append(max(money[0], money[1]))
    for i in range(2, len(money)-1):
        dp.append(max(dp[i-2]+money[i], dp[i-1]))
        
    # 마지막 집을 털고 마지막 집을 털때
    dp2 = [0]
    dp2.append(money[1])
    for i in range(2, len(money)):
        dp2.append(max(dp2[i-2]+money[i], dp2[i-1]))
    
    answer = max(max(dp), max(dp2))
    return answer
```

## 백준 2098 : 외판원 순회
N <= 16이기때문에 비트마스킹 방법을 사용할 수 있다.


```python
import sys
 
N=int(sys.stdin.readline()) # 도시의 수
W=[] # 도시에서 도시로 가는 비용
INF=sys.maxsize
for _ in range(N):
    W.append(list(map(int,sys.stdin.readline().split())))
DP=[[None]*(1<<N) for _ in range(N)]


def find_path(last,visited):#현재위치
    if visited == (1<<N)-1:  # 모두 방문햇다면
        return W[last][0] or INF  # 0으로 가는방법 반환 없으면 INF반환
 
    if DP[last][visited] is not None:  # 이미 계산되어잇다면
        return DP[last][visited]  # 있는값 반환
 
    tmp=INF
    for city in range(N):#모든 도시에서
        if visited & (1 << city) == 0 and W[last][city] != 0:#아직 방문하지 않았고 cur->i로 가는길이 있다면
            tmp=min(tmp,
                    find_path(city,visited | (1<<city)) + W[last][city])
    DP[last][visited]=tmp
    return tmp
 

print(find_path(0,1<<0))


```

## 백준 17271 : 리그 오브 레전설 (Small)

```python
n,m = map(int, input().split())

dp = [0] * (n+1) 
dp[0] = 1
for i in range(1,n+1):
    dp[i] = dp[i-1]
    if i>=m: dp[i] += dp[i-m]
    dp[i] %= 1000000007

print(dp[-1])
```