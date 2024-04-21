# 의상
https://school.programmers.co.kr/learn/courses/30/lessons/42578

## 문제 설명
코니가 가진 의상들이 담긴 2차원 배열 clothes가 주어질 때 서로 다른 옷의 조합의 수를 return 하도록 solution 함수를 작성해주세요.

**제한사항**    
- clothes의 각 행은 [의상의 이름, 의상의 종류]로 이루어져 있습니다.
- 코니가 가진 의상의 수는 1개 이상 30개 이하입니다.
- 같은 이름을 가진 의상은 존재하지 않습니다.
- clothes의 모든 원소는 문자열로 이루어져 있습니다.
- 모든 문자열의 길이는 1 이상 20 이하인 자연수이고 알파벳 소문자 또는 '_' 로만 이루어져 있습니다.

**입출력 예**
|clothes|return|
|-|-|
|[["yellow_hat", "headgear"], ["blue_sunglasses", "eyewear"], ["green_turban", "headgear"]]|5|

**입출력 예 설명**   
예제 #1   
headgear에 해당하는 의상이 yellow_hat, green_turban이고 eyewear에 해당하는 의상이 blue_sunglasses이므로 아래와 같이 5개의 조합이 가능합니다.
```
1. yellow_hat
2. blue_sunglasses
3. green_turban
4. yellow_hat + blue_sunglasses
5. green_turban + blue_sunglasses
```

## Sol. 0
```python
from collections import defaultdict

def solution(clothes):
    answer = 1
    # 같은 종류의 옷을 담아둘 딕셔너리 생성
    dic = defaultdict(list)
    
    # 같은 종류의 옷들을 담아줌
    for clo in clothes:
        dic[clo[1]].append(clo[0])
    
    # 각 부위가 가지는 경우의 수 : (각 부위 배열의 길이 + 1(해당 부위의 옷을 안쓰는 경우))
    # 각 부위의 배열의 길이 + 1을 모두 곱해 옷을 입거나 입지 않는 모든 경우의 수를 추출   
    for k in dic:
        answer *= len(dic[k]) + 1 
    
    # 아무것도 입지 않는 경우의 수 1을 빼주기
    answer -= 1

    return answer
```