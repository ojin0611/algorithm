# 개념
## 최대공약수(Greatest Common Divisor)와 최대공배수(Least Common Multiple)
GCD를 구하기 위하는 방법은 여러 가지인데, 유클리드 알고리즘을 이용한 방법은 아래와 같다.  
1. 두 수 a,b 가 있다. (a>b)
2. a % b == 0 이면 b가 GCD
3. a % b != 0 이면 a, b = b, a % b 을 해주고 다시 2로 돌아간다.

LCM = a * b / gcd
```python
def gcd(a,b):
    if a%b==0:
        return b
    else:
        return gcd(b,a%b)

def lcm(a,b):
    return a*b//gcd(a,b)

```

## 약수 구하기
```python
def getDivisor(x):
    divisor = [x] # 1 또는 자기 자신 넣고 시작하면 됨. 
    for i in range(2, int(x**0.5)+1):
        if x%i == 0:
            divisor.append(i)
            divisor.append(x//i)
    return sorted(list(set(divisor)))

```


# 문제
## 소수 모두 찾기
```python
# 1 ~ 10000 중에서 소수를 모두 찾고싶으면
arr = [False, False] + [True] * 9999 # 0,1은 False
for i in range(2, 101):
    if arr[i]:
        for j in range(i * 2, len(arr), i):
            arr[j] = False
```

## Fly me to the Alpha Centauri
[문제](https://www.acmicpc.net/problem/1011)

```python
for _ in range(int(input())):
    x,y=map(int, input().split())
    
    distance = y-x
    n = int(distance**0.5)
    if n**2 + n < distance:
        print(2*n+1)
    elif n**2 < distance:
        print(2*n)
    else:
        print(2*n-1)
```

## 터렛
[문제](https://www.acmicpc.net/problem/1002)

거리 비교 시, d^2 = r1^2 + r1^2 로 비교해주는 것이 편리하다.  
d = (r1^2 + r1^2)**0.5 로 계산하면 소수점때문에 비교가 힘들기때문이다

```python
for _ in range(int(input())):
    x1,y1,r1,x2,y2,r2 = map(int, input().split())
    
    d = (x1-x2)**2 + (y1-y2)**2
    add = (r1+r2)**2
    sub = (r1-r2)**2
    if d==0:
        if r1==r2:
            print(-1)
        else:
            print(0)
    else:
        if add < d:
            print(0)
        elif add == d:
            print(1)
        else:
            if sub==d:
                print(1)
            elif sub > d:
                print(0)
            else:
                print(2)
```

## 소인수분해
[문제](https://www.acmicpc.net/problem/11653)

```python
v = int(input()); i=2
while v!=1 :
    if v%i == 0:
        v = v/i
        print(i)
    else : i+=1
```

## 팩토리얼 0의 개수
[문제](https://www.acmicpc.net/problem/1676)  
숫자 1부터 N까지 곱할 때, 0을 만드는 숫자는 5의 거듭제곱들이다. (짝수는 그보다 훨씬 많으므로 고려하지 않아도 됨)
```python
N = int(input()) # N < 500
print(N//5 + N//25 + N//125)
```