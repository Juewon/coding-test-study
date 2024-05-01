# 베스트앨범
https://school.programmers.co.kr/learn/courses/30/lessons/42579

## 문제
**문제 설명**     
스트리밍 사이트에서 장르 별로 가장 많이 재생된 노래를 두 개씩 모아 베스트 앨범을 출시하려 합니다. 노래는 고유 번호로 구분하며, 노래를 수록하는 기준은 다음과 같습니다.

1. 속한 노래가 많이 재생된 장르를 먼저 수록합니다.
2. 장르 내에서 많이 재생된 노래를 먼저 수록합니다.
3. 장르 내에서 재생 횟수가 같은 노래 중에서는 고유 번호가 낮은 노래를 먼저 수록합니다.    

노래의 장르를 나타내는 문자열 배열 genres와 노래별 재생 횟수를 나타내는 정수 배열 plays가 주어질 때, 베스트 앨범에 들어갈 노래의 고유 번호를 순서대로 return 하도록 solution 함수를 완성하세요.

**제한사항**
- genres[i]는 고유번호가 i인 노래의 장르입니다.
- plays[i]는 고유번호가 i인 노래가 재생된 횟수입니다.
- genres와 plays의 길이는 같으며, 이는 1 이상 10,000 이하입니다.
- 장르 종류는 100개 미만입니다.
- 장르에 속한 곡이 하나라면, 하나의 곡만 선택합니다.
- 모든 장르는 재생된 횟수가 다릅니다.

**입출력 예**   
|genres|plays|return|
|-|-|-|
|["classic", "pop", "classic", "classic", "pop"]|[500, 600, 150, 800, 2500]|[4, 1, 3, 0]|

**입출력 예 설명**   
classic 장르는 1,450회 재생되었으며, classic 노래는 다음과 같습니다.

- 고유 번호 3: 800회 재생
- 고유 번호 0: 500회 재생
- 고유 번호 2: 150회 재생   

pop 장르는 3,100회 재생되었으며, pop 노래는 다음과 같습니다.

- 고유 번호 4: 2,500회 재생
- 고유 번호 1: 600회 재생

따라서 pop 장르의 [4, 1]번 노래를 먼저, classic 장르의 [3, 0]번 노래를 그다음에 수록합니다.

- 장르 별로 가장 많이 재생된 노래를 최대 두 개까지 모아 베스트 앨범을 출시하므로 2번 노래는 수록되지 않습니다.

## Solution. 0
```python
# defaultdict 함수를 사용
from collections import defaultdict

def solution(genres, plays):
  answer =  []
  # 초기값이 리스트 형식으로 구성되어있는 genre_dict 딕셔너리 생성
  genre_dict = defaultdict(list)
  
  # 장르별로 각 곡의 재생횟수와 인덱스를 리스트로 묶어 append 시키기
  # ex) defaultdict(<class 'list'>, {'classic': [[500, 0], [150, 2], [800, 3]], 'pop': [[600, 1], [2500, 4]]})
  for idx in range(len(genres)):
    genre_dict[genres[idx]].append([plays[idx], idx])

  # genre_dict에 값이 존재하는 동안 반복
  while genre_dict:
    # lambda 함수를 이용해 각 장르별 곡들의 총 재생횟수를 더한 후
    # max 함수를 이용해 가장 높은 재생횟수를 가진 장르를 추출
    max_genre = max(genre_dict, key=lambda x : sum(item[0] for item in genre_dict[x]))
    
    # 제한 사항(장르에 속한 곡이 하나라면, 하나의 곡만 선택합니다)에 맞게 if문 구성 
    if len(genre_dict[max_genre]) == 1:
      answer.append(genre_dict[max_genre][0][1])
    
    else:
      # 가장 많이 재생된 장르의 노래 두 곡을 선별하기 위해 for문 사용
      for _ in range(2):
        # 제한 사항 : 장르 내에서 재생 횟수가 같은 노래 중에서는 고유 번호가 낮은 노래를 먼저 수록합니다.
        # max 함수를 활용하면 두 값이 같을경우, 인덱스가 낮은 값을 최대값으로 불러옴
        # 가장 많이 재생된 장르의 가장 많이 재생된 노래의 인덱스를 answer에 append
        max_idx = max(genre_dict[max_genre], key=lambda x : x[0])[1]
        answer.append(max_idx)
        # 두번째 곡을 담기 위해 베스트앨범에 선정된 곡은 remove시키기
        genre_dict[max_genre].remove(max(genre_dict[max_genre], key=lambda x : x[0]))

    # while문을 종료시키기 위해 베스트앨범 선정이 끝난 장르는 delete시키기
    del(genre_dict[max_genre])

  return answer
```

### Sol. 0 - 결과
> **100/100 점**