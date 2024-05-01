# 특정 세대의 대장균 찾기
https://school.programmers.co.kr/learn/courses/30/lessons/301650

## 문제
3세대의 대장균의 ID(`ID`) 를 출력하는 SQL 문을 작성해주세요. 이때 결과는 대장균의 ID 에 대해 오름차순 정렬해주세요.

**예시**    
예를 들어 `ECOLI_DATA` 테이블이 다음과 같다면

| ID | PARENT_ID | SIZE_OF_COLONY | DIFFERENTIATION_DATE | GENOTYPE |
|----|-----------|----------------|----------------------|----------|
| 1  | NULL      | 10             | 2019/01/01           | 5        |
| 2  | NULL      | 2              | 2019/01/01           | 3        |
| 3  | 1         | 100            | 2020/01/01           | 4        |
| 4  | 2         | 16             | 2020/01/01           | 4        |
| 5  | 2         | 17             | 2020/01/01           | 6        |
| 6  | 4         | 101            | 2021/01/01           | 22       |
| 7  | 3         | 101            | 2022/01/01           | 23       |
| 8  | 6         | 1              | 2022/01/01           | 27       |

PARENT ID 가 NULL 인 ID 1, ID 2가 1 세대이며 ID 1에서 분화된 ID 3, ID 2에서 분화된 ID 4, ID 5 가 2 세대입니다. ID 4 에서 분화된 ID 6, ID 3에서 분화된 ID 7 이 3 세대이며 ID 6에서 분화된 ID 8은 4 세대입니다.

따라서 결과를 ID 에 대해 오름차순 정렬하면 다음과 같아야 합니다.

|ID|
|-|
|6|
|7|

## Sol. 0
**3세대 대장균 부모 -> 2세대 대장균 부모 -> 1세대 대장균 부모가 NULL**이면 3세대 대장균 부모의 ID를 출력

```sql
-- 3세대 대장균의 부모로부터 거슬러 올라가 순서에 맞게 3세대부터 1세대까지 간다면(C.PARENT_ID IS NULL)
-- 3세대 대장균을 출력
SELECT A.ID
FROM ECOLI_DATA A
JOIN ECOLI_DATA B ON A.PARENT_ID = B.ID
JOIN ECOLI_DATA C ON B.PARENT_ID = C.ID
WHERE C.PARENT_ID IS NULL
ORDER BY A.ID;
```