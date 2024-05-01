# 특정 조건을 만족하는 물고기별 수와 최대 길이 구하기
https://school.programmers.co.kr/learn/courses/30/lessons/298519

## 문제
`FISH_INFO`에서 평균 길이가 33cm 이상인 물고기들을 종류별로 분류하여 잡은 수, 최대 길이, 물고기의 종류를 출력하는 SQL문을 작성해주세요. 결과는 물고기 종류에 대해 오름차순으로 정렬해주시고, 10cm이하의 물고기들은 10cm로 취급하여 평균 길이를 구해주세요.

컬럼명은 물고기의 종류 'FISH_TYPE', 잡은 수 'FISH_COUNT', 최대 길이 'MAX_LENGTH'로 해주세요.

**예시**    
예를 들어 `FISH_INFO` 테이블이 다음과 같다면

| ID | FISH_TYPE | LENGTH | TIME       |
|----|-----------|--------|------------|
| 0  | 0         | 30     | 2021/12/04 |
| 1  | 0         | 50     | 2020/03/07 |
| 2  | 0         | 40     | 2020/03/07 |
| 3  | 1         | 30     | 2022/03/09 |
| 4  | 1         | NULL   | 2022/04/08 |
| 5  | 2         | 32     | 2020/04/28 |

물고기 종류가 0인 물고기들의 평균 길이는 (30 + 50 + 40) / 3 = 40cm 이고 물고기 종류가 1인 물고기들의 평균 길이는 (30 + 10) / 2 = 20cm 이며, 물고기 종류가 2인 물고기들의 평균 길이는 (32) / 1 = 32cm 입니다. 따라서 평균길이가 33cm 인 물고기 종류는 0 이므로, 총 잡은 수는 3마리, 최대 길이는 50cm 이므로 결과는 다음과 같아야 합니다.

|FISH_COUNT|MAX_LENGTH|FISH_TYPE|
|-|-|-|
|3|50|0|

## Sol. 0
```sql
-- HAVING 절에 포함되는 ROW들 COUNT, 최대 길이값과 그에 FISH_TYPE 추출하기
SELECT COUNT(*) AS FISH_COUNT, MAX(LENGTH) AS MAX_LENGTH, FISH_TYPE
FROM FISH_INFO
GROUP BY FISH_TYPE
-- NULL값을 10으로 대체한후 잡은 물고기의 평균값이 33cm 이상인 경우 출력대상에 포함
HAVING AVG(IFNULL(LENGTH, 10)) >= 33
ORDER BY FISH_TYPE;
```