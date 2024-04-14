# 자동차 대여 기록에서 대여중 / 대여 가능 여부 구분하기
https://school.programmers.co.kr/learn/courses/30/lessons/157340

## 문제
**문제 설명**   
`CAR_RENTAL_COMPANY_RENTAL_HISTORY` 테이블에서 2022년 10월 16일에 대여 중인 자동차인 경우 '대여중' 이라고 표시하고, 대여 중이지 않은 자동차인 경우 '대여 가능'을 표시하는 컬럼(컬럼명: `AVAILABILITY`)을 추가하여 자동차 ID와 `AVAILABILITY` 리스트를 출력하는 SQL문을 작성해주세요. 이때 반납 날짜가 2022년 10월 16일인 경우에도 '대여중'으로 표시해주시고 결과는 자동차 ID를 기준으로 내림차순 정렬해주세요.

## Sol. 0
```sql
SELECT CAR_ID,
    -- START_DATE와 END_DATE 사이에 2022-10-16이 포함되어 있는 CAR_ID를 추출후 대여중이라고 출력
    -- 그렇지 않으면 대여 가능을 출력하는 AVAILABILITY 컬럼 생성
    CASE WHEN CAR_ID in 
        (SELECT CAR_ID 
         FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
         WHERE "2022-10-16" BETWEEN START_DATE AND END_DATE) THEN "대여중"
    ELSE "대여 가능"
    END  AVAILABILITY
    FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
    GROUP BY 1
    ORDER BY 1 desc
```