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

