# 특정 기간동안 대여 가능한 자동차들의 대여비용 구하기
https://school.programmers.co.kr/learn/courses/30/lessons/157339

## 문제
`CAR_RENTAL_COMPANY_CAR` 테이블과 `CAR_RENTAL_COMPANY_RENTAL_HISTORY` 테이블과 `CAR_RENTAL_COMPANY_DISCOUNT_PLAN` 테이블에서 자동차 종류가 '세단' 또는 'SUV' 인 자동차 중 2022년 11월 1일부터 2022년 11월 30일까지 대여 가능하고 30일간의 대여 금액이 50만원 이상 200만원 미만인 자동차에 대해서 자동차 ID, 자동차 종류, 대여 금액(컬럼명: `FEE`) 리스트를 출력하는 SQL문을 작성해주세요. 결과는 대여 금액을 기준으로 내림차순 정렬하고, 대여 금액이 같은 경우 자동차 종류를 기준으로 오름차순 정렬, 자동차 종류까지 같은 경우 자동차 ID를 기준으로 내림차순 정렬해주세요.

**예시**   
예를 들어 `CAR_RENTAL_COMPANY_CAR` 테이블과 `CAR_RENTAL_COMPANY_RENTAL_HISTORY` 테이블과 `CAR_RENTAL_COMPANY_DISCOUNT_PLAN` 테이블이 다음과 같다면

| CAR_ID | CAR_TYPE | DAILY_FEE | OPTIONS                        |
|--------|----------|-----------|--------------------------------|
| 1      | SUV      | 25000     | 가죽시트,열선시트,후방카메라  |
| 2      | 세단     | 14000     | 스마트키,네비게이션,열선시트  |
| 3      | 트럭     | 32000     | 주차감지센서,후방카메라,가죽시트 |
| 4      | 세단     | 12000     | 열선시트,후방카메라            |
| 5      | 세단     | 22000     | 스마트키,주차감지센서          |

| HISTORY_ID | CAR_ID | START_DATE | END_DATE   |
|------------|--------|------------|------------|
| 1          | 1      | 2022-08-27 | 2022-09-02 |
| 2          | 1      | 2022-10-03 | 2022-10-04 |
| 3          | 2      | 2022-10-05 | 2022-10-20 |
| 4          | 2      | 2022-10-10 | 2022-11-12 |
| 5          | 3      | 2022-10-16 | 2022-10-17 |

| PLAN_ID | CAR_TYPE | DURATION_TYPE | DISCOUNT_RATE |
|---------|----------|---------------|---------------|
| 1       | 트럭     | 7일 이상      | 5%            |
| 2       | 트럭     | 30일 이상     | 7%            |
| 3       | 트럭     | 90일 이상     | 10%           |
| 4       | 세단     | 7일 이상      | 5%            |
| 5       | 세단     | 30일 이상     | 10%           |
| 6       | 세단     | 90일 이상     | 15%           |
| 7       | SUV      | 7일 이상      | 3%            |
| 8       | SUV      | 30일 이상     | 8%            |
| 9       | SUV      | 90일 이상     | 12%           |

자동차 종류가 '세단' 또는 'SUV' 인 자동차 중 2022년 11월 1일 부터 2022년 11월 30일까지 대여가능한 자동차는 자동차 ID가 1, 4, 5인 자동차입니다.

일일 대여 요금에 자동차 종류 별 대여기간이 30일 이상인 경우의 할인율을 적용하여 30일간의 대여 금액을 구하면,

- 자동차 ID가 1인 경우, 일일 대여 금액 25,000원에서 8% 할인율을 적용하고 30일을 곱하면 총 대여 금액은 690,000원
- 자동차 ID가 4인 경우, 일일 대여 금액 12,000원에서 10% 할인율을 적용하고 30일을 곱하면 총 대여 금액은 324,000원
- 자동차 ID가 5인 경우, 일일 대여 금액 22,000원에서 10% 할인율을 적용하고 30일을 곱하면 총 대여 금액은 621,000원이고, 대여 금액이 50만원 이상 200만원 미만인 경우에 대해서 대여 금액을 기준으로 내림차순, 자동차 종류를 기준으로 오름차순 및 자동차 ID를 기준으로 내림차순 정렬하면 다음과 같아야 합니다.
  
|CAR_ID|CAR_TYPE|FEE|
|-|-|-|
|5|세단|690000|
|1|SUV|621000|

**주의사항**   
`FEE`의 경우 예시처럼 정수부분만 출력되어야 합니다.

## Sol. 0
```sql
WITH 
-- 대여 기록이 없는 CAR_ID 추출
NO_RENTAL_HISTORY AS(
    SELECT C.CAR_ID
    FROM CAR_RENTAL_COMPANY_CAR C
    LEFT JOIN CAR_RENTAL_COMPANY_RENTAL_HISTORY H ON C.CAR_ID = H.CAR_ID
    WHERE H.CAR_ID IS NULL
),

-- 2022-11-01 ~ 2022-11-30에 대여가능한 CAR_ID 추출
AVAILABLE_RENT AS(
    SELECT DISTINCT CAR_ID
    FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY CRH
    WHERE NOT EXISTS (
            SELECT 1
            FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY H
            WHERE CRH.CAR_ID = H.CAR_ID
                AND H.START_DATE <= '2022-11-30'
                AND H.END_DATE >= '2022-11-01')
),

-- 대여 기록이 없는 CAR_ID와 대여 가능한 CAR_ID를 합친 임시 테이블 생성
COMBINE_AVAILABLE_RENT AS(
    SELECT CAR_ID FROM NO_RENTAL_HISTORY
    UNION
    SELECT CAR_ID FROM AVAILABLE_RENT
),

-- 대여 기간 할인 정책 테이블에서 '30일 이상'인 DURATION_TYPE만 필터링한 임시 테이블 생성 
DURATION AS(
    SELECT *
    FROM CAR_RENTAL_COMPANY_DISCOUNT_PLAN
    WHERE DURATION_TYPE = '30일 이상'
)

SELECT CAR.CAR_ID, 
        RCC.CAR_TYPE, 
        ROUND(((1 - (D.DISCOUNT_RATE * 0.01)) * RCC.DAILY_FEE) * 30, 0) AS FEE
FROM CAR_RENTAL_COMPANY_CAR RCC
JOIN COMBINE_AVAILABLE_RENT CAR ON RCC.CAR_ID = CAR.CAR_ID
JOIN DURATION D ON RCC.CAR_TYPE = D.CAR_TYPE
-- 대여 가능한 차 중 세단, SUV만 필터링
WHERE RCC.CAR_TYPE IN ('세단', 'SUV')
    -- 할인율을 적용한 일일 대여 요금의 30일치 비용이 50만원 이상 2백만원 미만인 데이터만 필터링
    AND ((1 - (D.DISCOUNT_RATE * 0.01)) * RCC.DAILY_FEE) * 30 >= 500000
    AND ((1 - (D.DISCOUNT_RATE * 0.01)) * RCC.DAILY_FEE) * 30 < 2000000
ORDER BY FEE DESC, RCC.CAR_ID ASC, CAR.CAR_ID DESC;
```