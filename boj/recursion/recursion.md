# 개념
재귀함수란 자기자신을 다시 호출하는 함수를 의미합니다
함수가 끝나는 조건이 있어야 합니다.



# 문제
## 별 찍기 - 10
[문제](https://www.acmicpc.net/problem/2447)  
가운데가 비어있는 프랙탈 만들기
```python
N = int(input())

# 1. 빈 칸의 인덱스를 미리 구하기
div = N
k = 0
while div > 1:
    div //= 3
    k += 1

for n in range(k):
    # 1. 3**n으로 나눈 몫 구하기
    # 2. 그 몫을 3으로 나눈 나머지가 1
    idx = [i for i in range(N) if (i // 3 ** n) % 3 == 1]
    for i in idx:
        for j in idx:
            star[i][j] = " "

for line in star:
    print(''.join(line))

# 2. 가장 빠른 풀이
def concat(r1, r2):
    return [''.join(x) for x in zip(r1, r2, r1)]

def stars(n):
    if n == 1:
        return ['*']
    n //= 3
    x = stars(n)
    t = concat(x, x)
    m = concat(x, [' '*n]*n)
    return t + m + t


print('\n'.join(stars(N)))
```

## 하노이탑 동선
[문제](https://www.acmicpc.net/problem/11729)  
1에 놓여있는 원판을 3으로 모두 옮기는 과정 출력하기

[풀이](https://pacific-ocean.tistory.com/119)  
print(a,c) : a에서 c로 옮긴다.
n개를 모두 옮기기 위해서는 n-1개를 b로 옮긴 후, 맨 아래 원판을 c로 옮기고 다시 n-1개를 c로 옮긴다.
```python
n = int(input())
def hanoi(n, a, b, c):
    if n == 1:
        print(a, c)
    else:
        hanoi(n - 1, a, c, b)
        print(a, c)
        hanoi(n - 1, b, a, c)
sum = 1
for i in range(n - 1):
    sum = sum * 2 + 1
print(sum)
hanoi(n, 1, 2, 3)
```