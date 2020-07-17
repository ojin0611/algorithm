# 개념

## 스택 stack

스택은 자료를 넣는 (push) 입구와 자료를 뽑는 (pop) 입구가 같아 제일 나중에 들어간 자료가 제일 먼저 나오는 (LIFO, Last in First out) 특성을 가지고 있다.

## 큐 Queue
큐는 스택과 마찬가지로 일종의 리스트로, 데이터 삽입은 한쪽 끝에서, 삭제는 반대쪽 끝에서만 일어난다.  
삽입이 일어나는 쪽을 rear, 삭제가 일어나는 쪽을 front라고 부른다.
FIFO(First-in, First-Out)

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