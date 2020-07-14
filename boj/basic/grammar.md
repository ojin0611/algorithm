# 개념
## 입출력
### 입력  
1. input()
2. sys.stdin.readline()  
3. sys.stdin : 여러 줄 입력받기  
   ^Z를 입력받으면 종료해주기 때문에, 임의의 여러 줄을 입력받아야 하는 문제에 사용된다.
```python
import sys

T = int(input())

for i in range(T):
    a,b = map(int, sys.stdin.readline().split())
    print(a+b)

for line in sys.stdin:
    print(line)
```
### 출력  
1. print() # 한 줄에 하나씩 출력
2. print(something, end=' ') # 출력내용 뒤에 공백 출력, 이후 print하면 그 옆에 출력됨
3. sys.stdout.write()
```python
sys.stdout.write('\n'.join(map(str, arr)))
```

## 문자열
```python
print('\n')  # 줄바꿈
print('\\n') # `\`로 끝나고 줄바꿈 
print('\'')  # ' 
print('\\\ abc')  # `\\ abc`

'MISSISSIPI'.count('I')   # 3 (3개)
'MISSISSIPI'.find('S')    # 2 (S는 3번째에 처음으로 등장 / 없으면 -1)
set('MISSISSIPI')         # ('M','I','S','P')
'ABCCE'[::-1]             # 'ECCBA'
'ABCCE'.replace('CC','*') # 'AB*E'
'23'.zfill(4)             # '0023'
mystring.split()          # 공백, 탭, 엔터 모두 분리해서 리스트 반환
```

## 문자열 포맷팅
[다양한 포맷팅 옵션](https://brownbears.tistory.com/421) : 
`str.format()` & `f-string`  
```python
string = 'Case #{n}: {a} + {b} = {a_plus_b}'
print(string.format(n=i+1, a=a, b=b, a_plus_b=a+b)) # Case #1: 10 + 7 = 17
```

## 아스키 코드
ord()함수와 chr()함수를 사용하면 간단히 바꿀수 있습니다.  
[아스키 코드표](https://lsjsj92.tistory.com/201)
```python
ord('A') # 65
chr(75) # K
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

## 리스트
```python
# list
[10,20,30,40].index(30) # 2 (index of element)
max_idx = [idx for idx, c in enumerate(count) if c==max(count)] # 리스트 내 최대값의 인덱스 모두 찾기

print([1,2,3] + [4,5]) # [1,2,3,4,5]
```

## 딕셔너리
```python
for key, val in mydict.items():
    print('key :', key, ', value :', val)
```

## max
```python
if a>40: a=40
max(a,40)
```

## lambda
lambda 인자리스트 : 표현식
```python
(lambda x,y: x+y)(10,20) # 30
```

## map
map(function, iterable, ...)

첫 번째 인자 function 는 함수의 이름 입니다. 두 번째 인자 iterable은 한번에 하나의 멤버를 반환할 수 있는 객체 입니다.(list, str, tuple)  
map() 함수는 function을 iterable의 모든 요소에 대해 적용합니다. 그리고 function에 의해 변경된  iterator를 반환합니다.

두 개의 정수를 한 줄에 입력받았을 때 a,b로 사용하기 위해선
```python
list(map(int, '12345')) # [1,2,3,4,5]

a,b = map(int, input().split()) # 한 줄에 숫자 하나씩이면 a = int(input())
```  
map은 리스트의 요소를 지정된 함수로 처리해주는 함수입니다.  
map은 원본 리스트를 변경하지 않고 새 리스트를 생성합니다.  
위의 map은 input으로 입력받은 숫자를 (for문을 이용하지 않고 한 번에) int로 바꿔주는 역할입니다.

## filter
filter(function, iterable)

filter에 인자로 사용되는 function은 처리되는 각각의 요소에 대해 Boolean 값을 반환합니다. True를 반환하면 그 요소는 남게 되고, False 를 반환하면 그 요소는 제거 됩니다.
```python
foo = [2, 18, 9, 22, 17, 24, 8, 12, 27]
list( filter(lambda x: x % 3 == 0, foo) ) # [18, 9, 24, 12, 27]
```


## zip
`zip(*iterable)`은 동일한 개수로 이루어진 자료형을 묶어주는 역할을 하는 함수입니다.
```python
list(zip([1, 2, 3], [4, 5, 6], [7, 8, 9])) 
# [(1, 4, 7), (2, 5, 8), (3, 6, 9)]
[''.join(x) for x in zip(
    ['***','* *','***'], [' '*3]*3, ['***','* *','***']
    )] 
# ['***   ***','* *   * *','***   ***']
for a,b in zip(str1, str2): print(a==b)  # 두 문자열 char별로 비교
```

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


# 문제
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

