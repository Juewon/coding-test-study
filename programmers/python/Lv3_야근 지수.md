# 야근 지수
https://school.programmers.co.kr/learn/courses/30/lessons/12927

## 문제
**문제 설명**   
회사원 Demi는 가끔은 야근을 하는데요, 야근을 하면 야근 피로도가 쌓입니다. 야근 피로도는 야근을 시작한 시점에서 남은 일의 작업량을 제곱하여 더한 값입니다. Demi는 N시간 동안 야근 피로도를 최소화하도록 일할 겁니다.Demi가 1시간 동안 작업량 1만큼을 처리할 수 있다고 할 때, 퇴근까지 남은 N 시간과 각 일에 대한 작업량 works에 대해 야근 피로도를 최소화한 값을 리턴하는 함수 solution을 완성해주세요.

**제한 사항**
- works는 길이 1 이상, 20,000 이하인 배열입니다.
- works의 원소는 50000 이하인 자연수입니다.
- n은 1,000,000 이하인 자연수입니다.

**입출력 예**
|works|n|result|
|-|-|-|
|[4, 3, 3]|4|12|
|[2, 1, 2]|1|6|
|[1,1]|3|0|

## Solution. 0
```python
def solution(n, works):
    answer = 0
    # 작업량이 퇴근까지 남은 시간보다 적으면 0을 리턴
    if sum(works) < n:
        return answer 
    
    # 퇴근까지 남은 N시간만큼 각 작업량에 -1씩 빼주기
    # N이 0이 되면 while문 break
    while True :
        for i in range(len(works)):
            works[i] -= 1
            n -= 1
            if n == 0:
                break
        if n == 0:
            break
    
    # 남은 작업량에 대한 피로도 계산
    for i in range(len(works)):
        answer += works[i]**2
    
    return answer
```

### Sol. 0 - 결과
> **6.7 / 100** - 입출력 예시에 대한 기댓값은 정확히 나왔지만, 여러 테스트 케이스들에 실패

## Solution. 1
작업량이 가장 높은 작업부터 더 효율적으로 작업시간을 빼주기 위해 **최대 힙**(max heap) 알고리즘으로 풀어보았다.

```python
import heapq

def solution(n, works):
    # 총 작업시간이 퇴근까지 남은 시간(N) 보다 작으면 0을 리턴
    if sum(works) < n:
        return 0 
    
    # 리스트를 최대힙으로 만들기 위해 각 작업량에 대한 부호 변환
    works = [-work for work in works]
    # 리스트를 힙 자료구조로 변환
    heapq.heapify(works)
    
    # 남은 시간(N)만큼 각 작업에 대해 1씩 빼주기 위해 
    # 부호가 변환된 각 작업량에 대해 +1 해서 다시 heappush 해주기
    for _ in range(n):
        work = heapq.heappop(works) + 1
        heapq.heappush(works, work)
    
    # 최소화된 야근 피로도 계산
    answer = sum([work**2 for work in works])

    return answer
```

### Sol. 1 - 결과
> **100 / 100**


