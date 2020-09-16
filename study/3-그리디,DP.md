# 그리디 풀이
## 프로그래머스 1번 : 체육복
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

## 프로그래머스 2번 : 큰 수 만들기
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

## 프로그래머스 3번 : 조이스틱
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

# 동적 계획법 풀이

## 프로그래머스 1번 : N으로 표현
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

## 프로그래머스 2번 : 등굣길
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