# 백준 알고리즘 (단계별)
단계별 풀이하다가 알게된 (알고있는 중요한) 정보들을 기록하는 공간   
https://www.acmicpc.net/step  
목차  
- 입출력과 사친연산
- if문
- for문


사용언어 : Python3

# 입출력과 사칙연산
## 문자열
문자열 줄바꿈은 `\n`  
문자열 출력 시, `\`로 끝나고 줄바꿈을 원하면 `\\n`  
문자열 출력 시, `\\`를 출력하고싶으면 `\\\`  

## map
두 개의 정수를 한 줄에 입력받았을 때 a,b로 사용하기 위해선
```python
a,b = map(int, input().split())
# 한 줄에 숫자 하나씩이면 a = int(input())
```  
map은 리스트의 요소를 지정된 함수로 처리해주는 함수입니다.  
map은 원본 리스트를 변경하지 않고 새 리스트를 생성합니다.  
위의 map은 input으로 입력받은 숫자를 (for문을 이용하지 않고 한 번에) int로 바꿔주는 역할입니다.

## Arithmetic Operator (Python)
```
+	더하기	a + b = 30
-	빼기	a - b = -10
*	곱하기	a * b = 200
/	나누기	b / a = 2.0
%	나머지	b % a = 0
**	제곱	a ** c = 1000
//	몫	a // c = 3
```

# if문
## 윤년이면 1, 아니면 0
```python
y=int(input())
print(int((y%400<1,y%4<1)[y%100>0]))
```
y%100>0이면 [True] (=[1]), 즉 앞의 tuple인 (y%400<1,y%4<1)에서 두 번째 원소를 선택한다. 

## 45분전 알람시계
```python
print((a-(b<45))%24,(b-45)%60)
```
b(분)가 45 이하면 a(시)에서 1을 빼준다.  
나머지를 이용해 음수를 처리한다.

# for문 / while문
## input() 대신 sys.stdin
빠른 속도를 위해 input 대신 다른 함수를 이용한다.
```python
import sys

T = int(input())
for i in range(T):
    a,b = map(int, sys.stdin.readline().split())
    print(a+b)
```

## 문자열 포맷팅
`str.format()` & `f-string`  
```python
string = 'Case #{n}: {a} + {b} = {a_plus_b}'
print(string.format(n=i+1, a=a, b=b, a_plus_b=a+b))
# Case #1: 10 + 7 = 17
```
[https://brownbears.tistory.com/421]

## EOF
EOF = End Of File. 파일의 끝  
파일 입력 시, 파일의 길이가 정해져있지 않는 경우 파일 끝을 알려주는 표시.  
Python으로 입력 파일을 받을 때는, try-except 구문을 통해 EOF 에러가 발생하면 파일이 끝난 것으로 간주한다.

[문제](https://www.acmicpc.net/problem/10951)  
[풀이](https://sozerodev.tistory.com/30)  
예외처리 안하는 방법
```python
import sys
 
for line in sys.stdin:
    a, b = map(int, line.split())
    print(a + b)
```

# 실습1
## max
```python
if a>40: a=40
max(a,40)
```

# 1차원 배열, 함수, 문자열
## 유용한 함수
```python
# list
[10,20,30,40].index(30) # 2 (index of element)
list(map(int, '12345')) # [1,2,3,4,5]

# string
'MISSISSIPI'.count('I') # 3 (number of string)
'MISSISSIPI'.find('S')  # 2 (1st place of string / 없으면 -1 return)
set('MISSISSIPI')       # ('M','I','S','P')
mystring.split()        # 공백, 탭, 엔터 모두 분리해줌
'ABCDE'[::-1]           # 'EDCBA'
'23'.zfill(4)           # '0023'

max_idx = [idx for idx, c in enumerate(count) if c==max(count)] # 리스트 내 최대값의 인덱스 모두 찾기
```
## 소수점 표시
```python
round(n,2) # 소수점 2번째 자리까지 반올림  
math.ceil(n) # 올림
math.ceil(n*10) / 10 # 소수점 컨트롤을 하려면 
math.floor(n) # 내림
math.pi # 3.141592...
print("%.2f" % n) # 소수점 2번째 자리까지 표시
```

# 문자열
## 아스키 코드
ord()함수와 chr()함수를 사용하면 간단히 바꿀수 있습니다.  
[아스키 코드표](https://lsjsj92.tistory.com/201)
```python
ord('A') # 65
chr(75) # K
```

## 크로아티아 문자
mystring에 string_list = ['aa','bb','cc']의 원소가 몇 개 있는지 확인하고싶을 때 : replace

```python
string_list = ['c=', 'c-', 'dz=', 'd-', 'lj', 'nj', 's=', 'z='] 
mystring = input() 
for s in string_list: 
    mystring = mystring.replace(s, '*') 
print(len(alpha)) # 전체 문자 개수
place(alpha.count('*')) # 치환된 문자 개수
```

## 정렬
key에 함수가 들어가면, list에 있는 각 원소에 함수를 씌우고 그 반환값을 기준으로 정렬한다.
```python
sorted(word, key=word.find)
```

# 수학 1, 2
## 소수 모두 찾기
```python
# 1 ~ 10000 중에서 소수를 모두 찾고싶으면
arr = [False, False] + [True] * 9999 # 0,1은 False
for i in range(2, 101):
    if arr[i]:
        for j in range(i * 2, len(arr), i):
            arr[j] = False
```

## 거리 비교
d^2 = r1^2 + r1^2 로 비교해주는 것이 편리하다.  
d = (r1^2 + r1^2)**0.5 로 계산하면 소수점때문에 비교가 힘들기때문이다

# 재귀
