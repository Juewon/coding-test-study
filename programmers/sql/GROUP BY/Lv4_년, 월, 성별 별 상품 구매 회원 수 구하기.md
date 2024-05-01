# 년, 월, 성별 별 상품 구매 회원 수 구하기
https://school.programmers.co.kr/learn/courses/30/lessons/131532

## 문제
`USER_INFO` 테이블과 `ONLINE_SALE` 테이블에서 년, 월, 성별 별로 상품을 구매한 회원수를 집계하는 SQL문을 작성해주세요. 결과는 년, 월, 성별을 기준으로 오름차순 정렬해주세요. 이때, 성별 정보가 없는 경우 결과에서 제외해주세요.

**예시**   
예를 들어 `USER_INFO` 테이블이 다음과 같고

| USER_ID | GENDER | AGE | JOINED    |
|---------|--------|-----|-----------|
| 1       | 1      | 26  | 2021-06-01|
| 2       | NULL   | NULL| 2021-06-25|
| 3       | 0      | NULL| 2021-06-30|
| 4       | 0      | 31  | 2021-07-03|
| 5       | 1      | 25  | 2021-07-09|
| 6       | 1      | 33  | 2021-07-14|

`ONLINE_SALE` 테이블이 다음과 같다면

| ONLINE_SALE_ID | USER_ID | PRODUCT_ID | SALES_AMOUNT | SALES_DATE |
|----------------|---------|------------|--------------|------------|
| 1              | 1       | 54         | 1            | 2022-01-01 |
| 2              | 1       | 3          | 2            | 2022-01-25 |
| 3              | 4       | 34         | 1            | 2022-01-30 |
| 4              | 6       | 253        | 3            | 2022-02-03 |
| 5              | 2       | 31         | 2            | 2022-02-09 |
| 6              | 5       | 35         | 1            | 2022-02-14 |
| 7              | 5       | 57         | 1            | 2022-02-18 |

2022년 1월에 상품을 구매한 회원은 `USER_ID` 가 1(`GENDER=1`), 4(`GENDER=0`)인 회원들이고,
2022년 2월에 상품을 구매한 회원은 `USER_ID` 가 2(`GENDER=NULL`), 5(`GENDER=1`), 6(`GENDER=1`)인 회원들 이므로,

년, 월, 성별 별로 상품을 구매한 회원수를 집계하고, 년, 월, 성별을 기준으로 오름차순 정렬하면 다음과 같은 결과가 나와야 합니다.

|YEAR|MONTH|GENDER|USERS|
|-|-|-|-|
|2022|1|0|1|
|2022|1|1|1|
|2022|2|1|2|

## Sol. 0
```sql
-- DISTINCT를 사용해 중복 구매한 USER_ID를 제외한 후 COUNT
SELECT YEAR(B.SALES_DATE) AS YEAR, MONTH(B.SALES_DATE) AS MONTH, A.GENDER, COUNT(DISTINCT(B.USER_ID)) AS USERS 
FROM USER_INFO A
JOIN ONLINE_SALE B ON A.USER_ID = B.USER_ID
-- GENDER가 NULL인 데이터는 출력 제외
WHERE A.GENDER IS NOT NULL
GROUP BY YEAR, MONTH, A.GENDER
ORDER BY 1, 2, 3;
```