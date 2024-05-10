# 대여 기록이 존재하는 자동차 리스트 구하기
https://school.programmers.co.kr/learn/courses/30/lessons/157341

## 문제
`CAR_RENTAL_COMPANY_CAR` 테이블과 `CAR_RENTAL_COMPANY_RENTAL_HISTORY` 테이블에서 자동차 종류가 '세단'인 자동차들 중 10월에 대여를 시작한 기록이 있는 자동차 ID 리스트를 출력하는 SQL문을 작성해주세요. 자동차 ID 리스트는 중복이 없어야 하며, 자동차 ID를 기준으로 내림차순 정렬해주세요.

**예시**   
예를 들어 `CAR_RENTAL_COMPANY_CAR` 테이블이 다음과 같고

|CAR_ID|CAR_TYPE|DAILY_FEE|OPTIONS|
|-|-|-|-|
|1|세단|16000|가죽시트,열선시트,후방카메라|
|2|SUV|14000|스마트키,네비게이션,열선시트|
|3|세단|22000|주차감지센서,후방카메라,가죽시트|
|4|세단|12000|주차감지센서|

`CAR_RENTAL_COMPANY_RENTAL_HISTORY` 테이블이 다음과 같다면

| HISTORY_ID | CAR_ID | START_DATE | END_DATE   |
|------------|--------|------------|------------|
| 1          | 4      | 2022-09-27 | 2022-09-27 |
| 2          | 3      | 2022-10-03 | 2022-10-04 |
| 3          | 2      | 2022-10-05 | 2022-10-05 |
| 4          | 1      | 2022-10-11 | 2022-10-14 |
| 5          | 3      | 2022-10-13 | 2022-10-15 |

10월에 대여를 시작한 기록이 있는 '세단' 종류의 자동차는 자동차 ID가 1, 3 인 자동차이고, 자동차 ID를 기준으로 내림차순 정렬하면 다음과 같이 나와야 합니다.

|CAR_ID|
|-|
|3|
|1|

## Sol. 0
```sql
-- 중복제거한 CAR_ID를 내림차순으로 출력
SELECT DISTINCT(HISTORY.CAR_ID) AS CAR_ID
FROM CAR_RENTAL_COMPANY_CAR CAR
JOIN CAR_RENTAL_COMPANY_RENTAL_HISTORY HISTORY ON CAR.CAR_ID = HISTORY.CAR_ID
-- CAR_TYPE이 세단이고 START_DATE가 10월 이상인 CAR_ID만 출력
WHERE CAR.CAR_TYPE = '세단' AND MONTH(HISTORY.START_DATE) >= 10
ORDER BY CAR_ID DESC;
```