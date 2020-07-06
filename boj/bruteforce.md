# 개념
Brute: 짐승, 동물  
Force: 힘

문제를 해결하기 위해서, 가능한 모든 경우에 대해 모두 직접 해 보는 방법입니다.
가장 비효율적이고 오래 걸리는 방법입니다.

# 문제
## 블랙잭
```python
# numbers에서 세 수의 조합을 모두 찾는 방법
n = len(numbers)
for i in range(n-2):
		for o in range(i+1,n-1):
			for p in range(o+1,n):
```