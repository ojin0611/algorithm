# dfs, bfs 풀이

## 프로그래머스 그래프 1. 가장 먼 노드

BFS를 이용해 루트 노드에서부터 거리를 구한다.

```python
from collections import deque,defaultdict 

def solution(n, edge):
    visited = [False] * (n+1)
    graph = defaultdict(set)
    
    for e in edge:
        graph[e[0]].add(e[1])        
        graph[e[1]].add(e[0])        
    
    def bfs(graph, start, visited): 
        queue = deque([start]) 
        visited[start] = 1
        while queue: 
            v = queue.popleft()
            length = visited[v] 
            for i in graph[v]: 
                if not visited[i]: 
                    queue.append(i) 
                    visited[i] = length + 1
        
    bfs(graph, 1, visited)
    max_length = max(visited)
    
    answer = 0
    for a in visited:
        if a==max_length:
            answer += 1
    return answer
```

## 프로그래머스 그래프 2. 순위

dfs를 이용해, 나한테 진 애들한테 진 애들한테 진 애들한테 진 애들을 탐색하자

```python
from collections import defaultdict

def solution(n, results):
    graph = [[0] * n for _ in range(n)] # 승패표
    WIN, LOSE = 1, -1
    for i, j in results: # 내입장 wind = 상대방 lose
        graph[i-1][j-1], graph[j-1][i-1] = WIN, LOSE
    
    # 0번부터 1명씩 꺼내서, graph를 다 채울거야
    for me in range(n):
        # 지금까지 기록돼있는, 나한테 진 애들을 스택에 저장
        stack = [opp for opp, rst in enumerate(graph[me]) if rst== WIN]
        
        # 나한테 진 애들을 하나씩 탐색
        while stack:
            loser = stack.pop()
            # 나한테 진 애한테 진 애들을 탐색 (Deeper)
            for opp, rst in enumerate(graph[loser]):
                # 나한테 진 애한테 졌으면서, 나와의 대전기록이 아직 없다면
                if rst == WIN and graph[me][opp] == 0:
                    # graph에 채워
                    graph[me][opp], graph[opp][me] = WIN, LOSE
                    # 나한테 진 애한테 진 애한테 진 애들을 탐색 (Deeper)
                    stack.append(opp)
    
    # 나와 대전기록을 봤을 때, 나 자신을 제외한 모두와 대전기록이 있다면 know
    return len(['know' for x in graph if x.count(0) == 1])

```

## 프로그래머스 그래프 3. 방의 개수

[참고할 테스트케이스](https://www.leejg.me/algorithm-test/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EA%B3%A0%EB%93%9D%EC%A0%90-kit-%ED%8C%8C%EC%9D%B4%EC%8D%AC-%ED%92%80%EC%9D%B4/3-%EB%B0%A9%EC%9D%98-%EA%B0%9C%EC%88%98)

방문했던 노드를 다시 방문할 때 방이 1개씩 추가된다. 따라서 노드별로 방문여부(visit)를 기록하고, 이미 방문했던 기록이 있으면 방의 개수에 1을 더해준다.

더불어 아래 내용을 고려해야한다.

1. 왔던 경로 다시 가는것 (반대방향 포함)
    > 방문여부 뿐만 아니라 노드별로 지나왔던 경로들을 저장해야한다.
    > 저장하는 방법은 여러가지인데, 각 노드와 노드를 통과한 arrow들을 저장하는 방법이 있고, 두 노드를 저장하는 방법이 있다. 아래 풀이에서는 두 노드의 위치를 저장했다.
2. X자 교차 고려
    > 기존 노드를 재방문하는 경우 외에도 이동중에 X자로 교차하는 경우에 방이 생길 가능성이 있다. 따라서 한 번 이동할 때 2번 이동하면서 노드를 2개 방문한다고 설계한다.

```python
from collections import defaultdict
def solution(arrows):
    dx = [0, 1, 1, 1, 0,-1,-1,-1]
    dy = [1, 1, 0,-1,-1,-1, 0, 1]
    
    start = (0,0)
    
    # graph는 해당 node를 지나간 적 있는 arrow들을 set으로 저장한다.
    passed = defaultdict(int)
    visit = defaultdict(int)

    visit[start] = 1

    prev = start
    answer = 0
    
    for arrow in arrows:
        for _ in range(2):
            now = (prev[0] + dx[arrow], prev[1] + dy[arrow])
            
            if visit[now] and (prev,now) not in passed:
                answer += 1 

            visit[now] = 1
            passed[(prev, now)] = 1
            passed[(now, prev)] = 1
            
            prev = now
    return answer
```