# 개념

## 스택 stack

스택은 자료를 넣는 (push) 입구와 자료를 뽑는 (pop) 입구가 같아 제일 나중에 들어간 자료가 제일 먼저 나오는 (LIFO, Last in First out) 특성을 가지고 있다.



## 큐 Queue

큐는 스택과 마찬가지로 일종의 리스트로, 데이터 삽입은 한쪽 끝에서, 삭제는 반대쪽 끝에서만 일어난다.  
삽입이 일어나는 쪽을 rear, 삭제가 일어나는 쪽을 front라고 부른다.
FIFO(First-in, First-Out)



## 덱 Deque

Double-ended Queue의 약자로, 양쪽 끝에서 삽입과 삭제가 모두 가능한 자료구조다. 큐와 스택을 합친 형태다.



## python list와 queue & deque

- `list.pop(0), list.index, list.insert, list.count, x in list, list[:-1]` 등은 다 O(N)입니다. 이외에도 O(N)이 걸리는 list 연산이 굉장히 많습니다. https://wiki.python.org/moin/TimeComplexity
- 위의 이유로, **list를 큐 또는 덱으로 사용하면 절대, 절대, 절대, 절대, 절대 안 됩니다!! 반드시 `collections.deque`를 써야 합니다.**



```python
from collections import deque
dq = deque('love')
print(dq) # deque(['l','o','v','e'])
print(dq[2]) # 'v'
```
### stack
```python
# push
dq.append('U') # deque(['l','o','v','e','U']) 

# pop
dq.pop() # deque(['l','o','v','e'])
```

### queue
```python
# left push
dq.appendleft('I') # deque(['I','l','o','v','e'])

# right pop
dq.pop() # deque(['I','l','o','v'])

# left pop
dq.popleft() # deque(['l','o','v'])

```

### 그 외 기타 기능
```python
# extend
dq.extend('e') # deque(['l','o','v','e'])

# remove
dq.remove('v') # deque(['l','o','e'])

# change value
dq[1] = 't' # deque(['l','t','e'])

# reverse
dq.reverse() # deque(['e','t','l'])
```





## 우선순위 큐

우선순위의 개념을 큐에 도입한 자료구조.

배열, 연결리스트, 힙으로 구현이 가능한데 이 중 힙으로 구현하는 것이 가장 효율적이다.



## 힙

힙은 최댓값, 최솟값을 찾아내는 연산을 쉽게하기 위해 고안된 자료형이다.

힙(Heap)은 각 노드의 키(Key)값이 그 자식의 키값보다 작지않거나, 그 자식의 키값보다 크지않은 완전 이진트리(Complete Binary Tree)이다. 힙을 저장하는 자료구조는 배열이다. 

> 트리에 관한 용어정리 (트리에 대해서는 이후에 따로 다루기)
>
> Node(노드) : 트리를 구성하고있는 각각의 요소
>
> Edge(간선) : 트리를 구성하기위해 노드와 노드를 연결하는 선을 의미한다.
>
> Root Node(루트 노드) : 트리구조에서 최상위에 있는 노드
>
> Terminal Node(= leaf Node, 단말 노드) : 하위에 다른 노드가 연결되어있지않은 노드
>
> Internal Node(내부노드, 비단말 노드) : 단말노드를 제외한 모든 노드. 루트노드도 포함



### 삽입

삽입된 값은 완전 이진트리를 만족시키면서 최하단부에 노드를 추가하고 값을 부여해준다. 부모 노드의 값과 비교해 삽입된 값이 더 크다면 swap해준다. 이를 **반복한다**. swap하지않거나, 루트 노드에 도달하면 swap을 종료한다. O(logn)

![img](./heap_insert.png)





### 삭제

보통 최상단 노드를 삭제한다. 힙의 용도는 최대값(또는 최소값)을 루트 노드에 놓아 바로 꺼내쓸 수있도록 하는것이기때문이다. O(logn)

삭제한 후에 가장 최하단부 왼쪽에 위치한 노드(가장 마지막에 추가한 노드)를 루트 노드로 이동시킨다.

루트 노드값이 자식 노드보다 작을경우, 자식노드중 가장 큰 값을 루트노드와 swap한다. 이를 반복한다. swap하지않았거나, swap한 이후 노드가 터미널노드에 도달하면 swap을 종료한다.

![img](./heap_delete.png)

## python heap

heapq 모듈은 이진트리 기반의 최소힙 자료구조를 제공한다. 

heapq 모듈은 파이썬의 보통 리스트를 최소힙처럼 다룰수 있도록 도와준다. 별개의 자료구조가 아니다!

그래서 빈 리스트를 생성한다음 heapq 모듈의 함수를 호출할 때마다 이 리스트를 인자로 넘겨야한다.

결과적으로 heapq 모듈을통해 원소를 추가하거나 삭제한 리스트가 그냥 최소힙이다. 

```python
import heapq

heap = []

# 원소 삽입
heapq.heappush(heap, 4) # O(logN)
heapq.heappush(heap, 1)
heapq.heappush(heap, 7)
heapq.heappush(heap, 3)
print(heap) # [1, 3, 7, 4]

# 원소 삭제
print(heapq.heappop(heap)) # 1
print(heap) # [3, 4, 7]

# 삭제하지 않고 값 얻기
heap[0]

# 기존 리스트를 힙으로 변환
heap = [4,1,7,3,8,5]
heapq.heapify(heap)
print(heap) # [1,3,5,4,8,7]
```



최대 힙을 구하기 위해선 위 코드를 **응용**하면 된다. heap에 값을 삽입할 때 튜플을 원소로 추가하면, 튜플 내에서 맨 앞의 값 기준으로 최소힙이 구성되는 원리를 이용한다.

```python
import heapq

nums = [4, 1, 7, 3, 8, 5]
heap = []

for num in nums:
  heapq.heappush(heap, (-num, num))  # (우선 순위, 값)

while heap:
  print(heapq.heappop(heap)[1])  # index 1
```

이러면 최소힙이 -num으로 정렬되므로 최대힙이 된다!

값을 읽어올때는 튜플의 2번째 값을 읽어온다.







# 문제

## 스택 수열
[문제](https://www.acmicpc.net/problem/1874)  

1부터 n까지의 수를 스택에 넣었다가 뽑아 늘어놓음으로써, 하나의 수열을 만들 수 있다. 이때, 스택에 push하는 순서는 반드시 오름차순을 지키도록 한다고 하자. 임의의 수열이 주어졌을 때 스택을 이용해 그 수열을 만들 수 있는지 없는지, 있다면 어떤 순서로 push와 pop 연산을 수행해야 하는지를 알아낼 수 있다.

```python
from sys import stdin
n = int(stdin.readline())
in_ = map(lambda x : int(x.rstrip()), stdin.readlines())

def numeric():
    cnt, stack, result = 1, [], []
    for i in in_:
        while cnt <= i: #in_에서 가장 높은 값을 찾을 때까지만 while구문이 돌아감
            stack.append(cnt)
            result.append('+')
            cnt+=1
        if stack.pop() != i:
            return 'NO'
        else:
            result.append('-')
    return '\n'.join(result)

print(numeric())
```



## 회전하는 큐

[문제](https://www.acmicpc.net/problem/1021)

왼쪽 또는 오른쪽으로 미는 과정을 pop, append가 아니라 list slicing을 이용하여 표현할 수 있다.

좌측과 우측 어느방향으로 밀더라도 이동이 끝난 후의 모습은 같다는 점을 주목해야한다. 

```python
n, m = map(int, input().split())
dq = [i for i in range(1, n+1)]

ans = 0

for find in map(int, input().split()):
    ix = dq.index(find)
    ans += min(len(dq[ix:]), len(dq[:ix])) # 왼쪽으로 이동 또는 오른쪽으로 이동중에 더 짧은 횟수
    dq = dq[ix+1:] + dq[:ix]

print(ans)
```



## AC

[문제](https://www.acmicpc.net/problem/5430)

새로 만들어진 언어 AC는 두 가지 함수를 갖고 있다.

R : 배열 reverse

D : 첫 번째 숫자를 버리는 함수



### 핵심 IDEA

arr.pop(0)은 O(n)이다. 때문에 pop할 때마다 하나씩 지우는 것이 아니라 한번에 지울수 있는 방법을 생각해준다!

reverse도 O(n)이기때문에 실제로 행하지 않고 reverse된 상태만 기억해준다!!!

```python
from sys import stdin, stdout
def AC(com,n, li):
    com.replace('RR', '')
    l, r, d = 0, 0, True
    for c in com:
        if c == 'R': d = not d
        elif c == 'D':
            if d: l += 1
            else: r += 1
    if l+r <= n:
        res = li[l:n - r]
        if d: return '[' + ','.join(res) + ']\n'
        else: return '[' + ','.join(res[::-1]) + ']\n'
    else:
        return 'error\n'

T = int(stdin.readline())
for _ in range(T):
    com = stdin.readline()
    n = int(stdin.readline())
    li = stdin.readline().rstrip()[1:-1].split(',')
    if n == 0 : []
    stdout.write(AC(com, n, li))
```



## 최대 힙 / 최소 힙

[최대힙 문제](https://www.acmicpc.net/problem/11279)

heap 개념을 완벽하게 이해했다면 시간단축을 위해 heapq 라이브러리를 이용하자.

```python
# 최대힙
# 내 코드..는 https://www.acmicpc.net/source/21735293
from heapq import *
from sys import stdin

heap = []
input()
for x in map(int, stdin): # 숫자 한번에 받아보리기
    if x:
        heappush(heap, -x)
    else:
        print(-heappop(heap) if heap else 0)
```



[최소힙 문제](https://www.acmicpc.net/problem/1927)

```python
# 최소힙
import heapq
import sys

heap = []
int(input())
for i in map(int, sys.stdin):
    if i: 
        heapq.heappush(heap, i)
    else:
        if heap:
            print(heapq.heappop(heap))
        else:
            print(0)
```

