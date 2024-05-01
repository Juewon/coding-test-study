# 완주하지 못한 선수
https://school.programmers.co.kr/learn/courses/30/lessons/42576

## 문제
수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.

마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.

**제한사항**    
- 마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다.
- completion의 길이는 participant의 길이보다 1 작습니다.
- 참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.
- 참가자 중에는 동명이인이 있을 수 있습니다.

**입출력 예**
|participant|completion|return|
|-|-|-|
|["leo", "kiki", "eden"]|["eden", "kiki"]|"leo"|
|["marina", "josipa", "nikola", "vinko", "filipa"]|["josipa", "filipa", "marina", "nikola"]|"vinko"|
|["mislav", "stanko", "mislav", "ana"]|["stanko", "ana", "mislav"]|"mislav"|

**입출력 예 설명**    
예제 #1   
"leo"는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.

예제 #2   
"vinko"는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.

예제 #3   
"mislav"는 참여자 명단에는 두 명이 있지만, 완주자 명단에는 한 명밖에 없기 때문에 한명은 완주하지 못했습니다.

## Sol. 0
```python
# collections의 Counter 함수를 써서 각 리스트별 요소 간 개수를 카운트한 딕셔너리를 생성 후
# 참가자와 완료자의 차집합을 구해 남아있는 참가자를 출력해 완주하지 못한 선수를 출력
from collections import Counter

# ex) participant = ["mislav", "stanko", "mislav", "ana"]
#     completion = ["stanko", "ana", "mislav"] 
def solution(participant, completion):
    participant_count = Counter(participant)
    # participant_count = {'mislav' : 2, 'stanko' : 1, 'ana' : 1}
    completion_count = Counter(completion)
    # completion_count = {'stanko' : 1, 'ana' : 1, 'mislav' : 1}

    not_completed = participant_count - completion_count
    # not_completed = {'mislav' : 1}

    # key의 값을 문자열로 변환 후 저장
    not_completed_str = ', '.join(list(not_completed.keys()))

    return not_completed_str  # 'mislav'
```

