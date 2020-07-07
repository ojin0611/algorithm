# 개념
0. 내장 함수 - sort / sorted
```python
arr.sort() # return None. arr가 정렬됨


arr = [(1, 2), (0, 1), (5, 1), (5, 2), (3, 0)] # 인자없이 그냥 sorted()만 쓰면, 리스트 아이템의 각 요소 순서대로 정렬을 한다. 
sorted(arr) # [(0, 1), (1, 2), (3, 0), (5, 1), (5, 2)] 

sorted(arr, key=function)
# key에 함수가 들어가면, list에 있는 각 원소에 함수를 씌우고 그 반환값을 기준으로 정렬한다.

sorted(word, key=word.find) 
# word의 처음에 등장한 알파벳부터 순서대로 정렬한다. (ex. banana > baaann)

sorted(arr, key = lambda x : x[0]) # [(0, 1), (1, 2), (3, 0), (5, 1), (5, 2)] 
sorted(arr, key = lambda x : x[1]) # [(3, 0), (0, 1), (5, 1), (1, 2), (5, 2)] 

# 아이템 첫 번째 인자를 기준으로 오름차순으로 먼저 정렬 
# 그 안에서 다음 두 번째 인자를 기준으로 내림차순으로 정렬
arr = [(1, 3), (0, 3), (1, 4), (1, 5), (0, 1), (2, 4)] 
sorted(arr, key = lambda x : (x[0], -x[1])) # [(0, 3), (0, 1), (1, 5), (1, 4), (1, 3), (2, 4)]

```


1. 선택 정렬 (Selection Sort) - O(n^2)  
가장 작은 것을 선택해서 제일 앞으로 보내는 정렬  
한 번 스캔해서 가장 작은 값과 맨 앞의 값을 바꿔줌  
그 다음 루프에서는 맨 앞 빼고 돌리기
```python
def selection_sort(arr):
    for i in range(len(arr) - 1):
        min_idx = i
        for j in range(i + 1, len(arr)):
            if arr[j] < arr[min_idx]:
                min_idx = j
        arr[i], arr[min_idx] = arr[min_idx], arr[i]
```

1. 거품 정렬 (Bubble Sort) - O(n^2)  
바로 뒤에 있는 값과 비교해 더 크면 자리를 바꾸는(뒤로 보내는) 정렬  
결과적으로 한 루프마다 가장 큰 값이 맨 뒤로 이동
그 다음 루프에서는 맨 뒤 빼고 돌리기  
일반적으로 가장 느린 정렬 방법
```python
def bubble_sort(arr):
    for i in range(len(arr) - 1, 0, -1):
        for j in range(i):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
```

1. 삽입 정렬 (Insertion Sort) - O(n^2)  
앞의 숫자들이 정렬이 돼있다는 가정하에, 바로 앞 숫자와 비교하여 자신이 더 작으면 자리를 바꾸는(앞으로 보내는) 정렬  
한 루프를 돌면, 해당 값이 자기가 있어야하는 자리에 삽입됨  
바깥 쪽 루프는 순방향, 안 쪽 루프는 역방향으로 진행  
데이터가 이미 거의 정렬된 상태에 한해서 어떤 알고리즘보다 빠름  
```python
def insertion_sort(arr):
    for end in range(1, len(arr)):
        for i in range(end, 0, -1):
            if arr[i - 1] > arr[i]:
                arr[i - 1], arr[i] = arr[i], arr[i - 1]
```

1. 병합 정렬 - O(nlogn)  
[동영상](https://i.stack.imgur.com/YlHqG.gif)  
분할 정복 (Divide and Conquer) 기법과 재귀 알고리즘을 이용한 정렬
기존 배열(arr)을 반으로 나눈 뒤에, 두 배열(low_arr, high_arr)에서 가장 작은 값을 뽑아내어 새로운 배열(merged_arr)을 만드는 방법  
```python
# index slicing. 메모리 많이 사용
def merge_sort(arr):
    if len(arr) < 2:
        return arr

    mid = len(arr) // 2
    low_arr = merge_sort(arr[:mid])
    high_arr = merge_sort(arr[mid:])

    merged_arr = []
    l = h = 0
    while l < len(low_arr) and h < len(high_arr):
        if low_arr[l] < high_arr[h]:
            merged_arr.append(low_arr[l])
            l += 1
        else:
            merged_arr.append(high_arr[h])
            h += 1
    merged_arr += low_arr[l:]
    merged_arr += high_arr[h:]
    return merged_arr

# 인덱스 접근을 이용해 메모리 최적화
def merge_sort(arr):
    def sort(low, high):
        if high - low < 2:
            return
        mid = (low + high) // 2
        sort(low, mid)
        sort(mid, high)
        merge(low, mid, high)

    def merge(low, mid, high):
        temp = []
        l, h = low, mid

        while l < mid and h < high:
            if arr[l] < arr[h]:
                temp.append(arr[l])
                l += 1
            else:
                temp.append(arr[h])
                h += 1

        while l < mid:
            temp.append(arr[l])
            l += 1
        while h < high:
            temp.append(arr[h])
            h += 1

        for i in range(low, high):
            arr[i] = temp[i - low]

    return sort(0, len(arr))
```

1. 퀵 소트 (Quick Sort) - O(nlogn)  
배열을 pivot 값 기준으로 더 작은 값과 큰 값으로 반복적으로 분할하여 정렬  
일반적으로 가장 빠른 알고리즘  
이미(혹은 거의) 정렬되어있는 경우 O(N^2)  
[자세한 설명](https://www.daleseo.com/sort-quick/)
```python
# 간결한 코드
def quick_sort(arr):
    if len(arr) <= 1:
        return arr
    pivot = arr[len(arr) // 2]
    lesser_arr, equal_arr, greater_arr = [], [], []
    for num in arr:
        if num < pivot:
            lesser_arr.append(num)
        elif num > pivot:
            greater_arr.append(num)
        else:
            equal_arr.append(num)
    return quick_sort(lesser_arr) + equal_arr + quick_sort(greater_arr)

# 메모리 최적화
def quick_sort(arr):
    def sort(low, high):
        if high <= low:
            return

        mid = partition(low, high)
        sort(low, mid - 1)
        sort(mid, high)

    def partition(low, high):
        pivot = arr[(low + high) // 2]
        while low <= high:
            while arr[low] < pivot:
                low += 1
            while arr[high] > pivot:
                high -= 1
            if low <= high:
                arr[low], arr[high] = arr[high], arr[low]
                low, high = low + 1, high - 1
        return low

    return sort(0, len(arr) - 1)
```

1. 계수 정렬 (Counting Sort) - O(n)  
입력된 값 아래에 몇개의 정수들이 존재하는지에 따라 X의 위치가 결정되는 알고리즘으로 비교 정렬이 아님  
입력 요소의 범위가 작을 때 효율적인 정렬 방법  
  

    1. 입력 요소들이 1부터 K까지의 정수라고 가정  
    2. 각 요소가 몇개씩 들어있는 지를 담을 카운팅 배열(c)을 K 크기로 초기화  
    3. 입력된 배열(arr)의 각 요소들을 세아려 c에 기록  
    4. c는 기록된 값을 이전 요소들의 누적합으로 다시 할당  
    5. c값을 기준으로 새로운 배열(sorted_arr)에 재배치하면 완료

```python
def counting_sorted(arr, K):
    counter = [0] * K
    
    # 각 요소가 몇 개 있는지 Count
    for i in arr:
        counter[i] += 1
    
    # 누적합으로 다시 할당
    for i in range(1,K):
        counter[i] += counter[i-1]
   
    sorted_arr = [0] * len(arr)
    for i in range(len(arr)):
        sorted_arr[c[arr[i]]-1] = arr[i]
        counter[arr[i]] -= 1
​
    return sorted_arr

# 메모리 제한이 있는 경우, 카운팅 배열에 바로 +1 해준다.
counter = [0] * K
for _ in range(N):
    counter[int(sys.stdin.readline())] += 1

for i,c in enumerate(counter):
    if c!=0:
        for _ in range(c):
            print(i)

# 딕셔너리 사용도 가능
```
1. TimSort

1. 힙 정렬 - O(nlogn)

1. 기수 정렬



# 문제
## 최빈값
```python
from collection import Counter

arr = [...]
Counter(arr).most_common()
```

## 단어 정렬
```python
import sys
N = int(input())

arr = []
for _ in range(N):
    arr.append(sys.stdin.readline().rstrip())

# 중복 단어 제거
# 길이로 정렬 > len(x), 이후에는 알파벳 > x
a = sorted(list(set(arr)), key=lambda x: (len(x), x))
for x in a:
    print(x)
```