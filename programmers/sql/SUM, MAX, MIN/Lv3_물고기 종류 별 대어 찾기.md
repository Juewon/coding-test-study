# 물고기 종류 별 대어 찾기
https://school.programmers.co.kr/learn/courses/30/lessons/293261

## 문제
물고기 종류 별로 가장 큰 물고기의 ID, 물고기 이름, 길이를 출력하는 SQL 문을 작성해주세요.

물고기의 ID 컬럼명은 `ID`, 이름 컬럼명은 `FISH_NAME`, 길이 컬럼명은 `LENGTH`로 해주세요.
결과는 물고기의 ID에 대해 오름차순 정렬해주세요.   
단, 물고기 종류별 가장 큰 물고기는 1마리만 있으며 10cm 이하의 물고기가 가장 큰 경우는 없습니다.

**예시**   
예를 들어 `FISH_INFO` 테이블이 다음과 같고

| ID | FISH_TYPE | LENGTH | TIME       |
|----|-----------|--------|------------|
| 0  | 0         | 30     | 2021/12/04 |
| 1  | 0         | 50     | 2020/03/07 |
| 2  | 0         | 40     | 2020/03/07 |
| 3  | 1         | 20     | 2022/03/09 |
| 4  | 1         | NULL   | 2022/04/08 |
| 5  | 2         | 13     | 2021/04/28 |
| 6  | 0         | 60     | 2021/07/27 |
| 7  | 0         | 55     | 2021/01/18 |
| 8  | 2         | 73     | 2020/01/28 |
| 9  | 1         | 73     | 2021/04/08 |
| 10 | 2         | 22     | 2020/06/28 |
| 11 | 2         | 17     | 2022/12/23 |

`FISH_NAME_INFO` 테이블이 다음과 같다면

| FISH_TYPE | FISH_NAME | 
|-----------|-----------| 
| 0         | BASS      | 
| 1         | SNAPPER   | 
| 2         | ANCHOVY   | 

'BASS' 중 가장 큰 물고기는 60cm 로 물고기 ID 가 6이고, 'SNAPPER' 중 가장 큰 물고기는 73cm 로 물고기 ID가 9입니다. 'ANCHOVY' 중 가장 큰 물고기는 73cm 로 물고기 ID가 8입니다. 따라서 물고기 ID(ID) 에 대해 오름차순 정렬한다면 결과는 다음과 같습니다.

| ID | FISH_NAME | LENGTH | 
|----|-----------|--------| 
| 6  | BASS      | 60     | 
| 8  | ANCHOVY   | 73     | 
| 9  | SNAPPER   | 73     |

## Sol. 0
```sql
SELECT A.ID, B.FISH_NAME, A.LENGTH
-- FISH_TYPE에 맞는 FISH_NAME을 불러오기 위해 내부조인
FROM FISH_INFO A
JOIN FISH_NAME_INFO B ON A.FISH_TYPE = B.FISH_TYPE
-- FISH_TYPE별 최대 길이에 해당하는 ROW를 불러오기
WHERE (A.FISH_TYPE, A.LENGTH) IN (SELECT FISH_TYPE, MAX(LENGTH) AS LENGTH
                                FROM FISH_INFO
                                GROUP BY FISH_TYPE)
ORDER BY A.ID;
```