# 대여 횟수가 많은 자동차들의 월별 대여 횟수 구하기
https://school.programmers.co.kr/learn/courses/30/lessons/151139

## 문제
`CAR_RENTAL_COMPANY_RENTAL_HISTORY` 테이블에서 대여 시작일을 기준으로 2022년 8월부터 2022년 10월까지 총 대여 횟수가 5회 이상인 자동차들에 대해서 해당 기간 동안의 월별 자동차 ID 별 총 대여 횟수(컬럼명: `RECORDS`) 리스트를 출력하는 SQL문을 작성해주세요. 결과는 월을 기준으로 오름차순 정렬하고, 월이 같다면 자동차 ID를 기준으로 내림차순 정렬해주세요. 특정 월의 총 대여 횟수가 0인 경우에는 결과에서 제외해주세요.

**예시**
예를 들어 `CAR_RENTAL_COMPANY_RENTAL_HISTORY` 테이블이 다음과 같다면

| HISTORY_ID | CAR_ID | START_DATE | END_DATE   |
|------------|--------|------------|------------|
| 1          | 1      | 2022-07-27 | 2022-08-02 |
| 2          | 1      | 2022-08-03 | 2022-08-04 |
| 3          | 2      | 2022-08-05 | 2022-08-05 |
| 4          | 2      | 2022-08-09 | 2022-08-12 |
| 5          | 3      | 2022-09-16 | 2022-10-15 |
| 6          | 1      | 2022-08-24 | 2022-08-30 |
| 7          | 3      | 2022-10-16 | 2022-10-19 |
| 8          | 1      | 2022-09-03 | 2022-09-07 |
| 9          | 1      | 2022-09-18 | 2022-09-19 |
| 10         | 2      | 2022-09-08 | 2022-09-10 |
| 11         | 2      | 2022-10-16 | 2022-10-19 |
| 12         | 1      | 2022-09-29 | 2022-10-06 |
| 13         | 2      | 2022-10-30 | 2022-11-01 |
| 14         | 2      | 2022-11-05 | 2022-11-05 |
| 15         | 3      | 2022-11-11 | 2022-11-11 |


대여 시작일을 기준으로 총 대여 횟수가 5회 이상인 자동차는 자동차 ID가 1, 2인 자동차입니다. 월 별 자동차 ID별 총 대여 횟수를 구하고 월 오름차순, 자동차 ID 내림차순으로 정렬하면 다음과 같이 나와야 합니다.

| MONTH | CAR_ID | RECORDS |
|-------|--------|---------|
| 8     | 2      | 2       |
| 8     | 1      | 2       |
| 9     | 2      | 1       |
| 9     | 1      | 3       |
| 10    | 2      | 2       |

## Sol.0
```sql
SELECT MONTH(START_DATE) AS MONTH, CAR_ID, COUNT(CAR_ID) AS RECORDS
FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
WHERE DATE_FORMAT(START_DATE, '%Y-%m') BETWEEN '2022-08' AND '2022-10'
    -- 2022년 8~10월의 CAR_ID 중 5번 이상 대여를 한 적 있는 CAR_ID를 필터링
    AND CAR_ID IN (SELECT CAR_ID 
                    FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
                    WHERE DATE_FORMAT(START_DATE, '%Y-%m') BETWEEN '2022-08' AND '2022-10'
                    GROUP BY CAR_ID
                    HAVING COUNT(CAR_ID) > 4)
-- MONTH, CAR_ID 그룹 별로 월 별 0회를 빌려간 CAR_ID를 제외한 데이터들을 추출
GROUP BY MONTH, CAR_ID
HAVING COUNT(CAR_ID) > 0
ORDER BY MONTH, CAR_ID DESC;
```
