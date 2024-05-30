# 연도별 대장균 크기의 편차 구하기
https://school.programmers.co.kr/learn/courses/30/lessons/299310

## 문제
분화된 연도(`YEAR`), 분화된 연도별 대장균 크기의 편차(`YEAR_DEV`), 대장균 개체의 ID(`ID`) 를 출력하는 SQL 문을 작성해주세요. 분화된 연도별 대장균 크기의 편차는 분화된 연도별 가장 큰 대장균의 크기 - 각 대장균의 크기로 구하며 결과는 연도에 대해 오름차순으로 정렬하고 같은 연도에 대해서는 대장균 크기의 편차에 대해 오름차순으로 정렬해주세요.

**예시**   
예를 들어 `ECOLI_DATA` 테이블이 다음과 같다면

| ID | PARENT_ID | SIZE_OF_COLONY | DIFFERENTIATION_DATE | GENOTYPE |
|----|-----------|----------------|----------------------|----------|
| 1  | NULL      | 10             | 2019/01/01           | 5        |
| 2  | NULL      | 2              | 2019/01/01           | 3        |
| 3  | 1         | 100            | 2020/01/01           | 4        |
| 4  | 2         | 10             | 2020/01/01           | 4        |
| 5  | 2         | 17             | 2020/01/01           | 6        |
| 6  | 4         | 101            | 2021/01/01           | 22       |

분화된 연도별 가장 큰 대장균의 크기는 다음과 같습니다.

2019 : 10   
2020 : 100   
2021 : 101

따라서 각 대장균의 분화된 연도별 대장균 크기의 편차는 다음과 같습니다.

ID 1 : 10 - 10 = 0   
ID 2 : 10 -2 = 8   
ID 3 : 100 - 100 = 0

ID 4 : 100 - 10 = 90   
ID 5 : 100 - 17 = 83   
ID 6 : 101 -101 - 0

이를 분화된 연도에 대해 오름차순으로 정렬하고 같은 연도에 대해서는 대장균 크기의 편차에 대해 오름차순으로 정렬하면 결과는 다음과 같아야 합니다.

| YEAR | YEAR_DEV | ID |
|------|----------|----|
| 2019 | 0        | 1  |
| 2019 | 8        | 2  |
| 2020 | 0        | 3  |
| 2020 | 83       | 5  |
| 2020 | 90       | 4  |
| 2021 | 0        | 6  |

## Sol. 0
```sql
-- 연도별 최대 대장균의 크기를 추출한 테이블과 조인해 연도에 맞는 대장균의 편차 구하기
SELECT YEAR(A.DIFFERENTIATION_DATE) AS YEAR, (B.YEAR_MAX_COLONY - A.SIZE_OF_COLONY) AS YEAR_DEV, A.ID
FROM ECOLI_DATA A
JOIN (SELECT YEAR(DIFFERENTIATION_DATE) AS YEAR, MAX(SIZE_OF_COLONY) AS YEAR_MAX_COLONY
        FROM ECOLI_DATA
        GROUP BY YEAR(DIFFERENTIATION_DATE)) B
WHERE YEAR(A.DIFFERENTIATION_DATE) = B.YEAR
ORDER BY 1, 2;
```
