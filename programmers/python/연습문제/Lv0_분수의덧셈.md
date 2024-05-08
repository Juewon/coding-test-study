# 분수의 덧셈
https://school.programmers.co.kr/learn/courses/30/lessons/120808

## 문제 설명
첫 번째 분수의 분자와 분모를 뜻하는 `numer1`, `denom1`, 두 번째 분수의 분자와 분모를 뜻하는 `numer2`, `denom2`가 매개변수로 주어집니다. 두 분수를 더한 값을 **기약 분수**로 나타냈을 때 분자와 분모를 순서대로 담은 배열을 return 하도록 solution 함수를 완성해보세요.

- 제한 사항
  - 0 < `numer1`, `denom1`, `numer2`, `denom2` < 1,000
- 입출력 예
  - |numer1|denom1|numer|denom2|result|
    |---|---|---|---|---|
    |1|2|3|4|[5, 4]|
    |9|2|1|3|[29, 6]|
  

## Solution. 0
```python
def solution(numer1, denom1, numer2, denom2):
    answer = []
    # 두 분수를 더한 분자 값
    numer = (numer1*denom2) + (numer2*denom1)
    # 두 분수를 더한 분모 값 
    denom = denom1 * denom2
    
    # 분자, 분모 모두 2로 나눠질때까지 나눠주기
    # 더이상 안나눠질 시, 결과 return
    while True:
        if numer % 2 == 0 and denom % 2 == 0:
            numer //= 2
            denom //= 2
            continue
        
        else:  
            answer.append(numer)
            answer.append(denom)
            break
        
    return answer
```

### Sol. 0 - 결과
> **33/100 점** (test case 10개 fail)

## Solution. 1
- `Sol. 0 오답요인` : 2로 나눴을 때 더이상 안나눠지는 분수는 당연히 기약분수 일 것이란 단순한 생각을 함
- 기약분수란 ?
  - 분모, 분자의 공약수가 1뿐인 분수를 기약분수라고 함
  - 기약분수로 나타내는 방법
    - 분모와 분자의 공약수가 1이 될 때까지 나누기
    - 분모와 분자를 최대공약수로 나누기

```python
def solution(numer1, denom1, numer2, denom2):
    # 두 분수를 더한 분모 값 
    numer = (numer1*denom2) + (numer2*denom1)
    # 두 분수를 더한 분자 값
    denom = denom1 * denom2
    
    answer = []
    
    # 분자의 약수를 담을 리스트 생성
    divisor_numer = []
    # 분모의 약수를 담을 리스트 생성
    divisor_denom = []
    # 분자,분모의 공약수를 담을 리스트 생성
    divisor_common = []
    
    # 분자의 약수를 찾아내 추가
    for i in range(1, numer+1):
        if numer % i == 0:
            divisor_numer.append(i)
    
    # 분모의 약수를 찾아내 추가
    for j in range(1, denom+1):
        if denom % j == 0:
            divisor_denom.append(j)
    
    # 분자, 분모의 약수들을 비교해 공약수를 추가
    for x in divisor_numer:
        if x in divisor_denom:
            divisor_common.append(x)
    
    # 최대공약수가 1이 아닐 시, 최대공약수로 나눠주고 return, 최대공약수가 1일 경우 바로 return
    if max(divisor_common) != 1:
        numer //= max(divisor_common)
        denom //= max(divisor_common)
    
    answer.append(numer)
    answer.append(denom)
                        
    return answer
```

### Sol. 1 - 결과
> **100/100 점** (test case 0개 fail)
