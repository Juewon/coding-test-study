# 자동차 종류 별 특정 옵션이 포함된 자동차 수 구하기
https://school.programmers.co.kr/learn/courses/30/lessons/151137

## 문제
**문제 설명**    
`CAR_RENTAL_COMPANY_CAR` 테이블에서 '통풍시트', '열선시트', '가죽시트' 중 하나 이상의 옵션이 포함된 자동차가 자동차 종류 별로 몇 대인지 출력하는 SQL문을 작성해주세요. 이때 자동차 수에 대한 컬럼명은 `CARS`로 지정하고, 결과는 자동차 종류를 기준으로 오름차순 정렬해주세요.



## Sol. 0
`LIKE '%시트%'` 구문을 사용해 시트가 포함되어 있는 ROW들을 추출
```sql
SELECT CAR_TYPE, COUNT(CAR_ID) AS CARS
FROM CAR_RENTAL_COMPANY_CAR
WHERE OPTIONS LIKE '%시트%' 
GROUP BY CAR_TYPE
ORDER BY CAR_TYPE ASC;
```