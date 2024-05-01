# 즐겨찾기가 가장 많은 식당 정보 출력하기
https://school.programmers.co.kr/learn/courses/30/lessons/131123

## 문제
**문제 설명**   
`REST_INFO` 테이블에서 음식종류별로 즐겨찾기수가 가장 많은 식당의 음식 종류, ID, 식당 이름, 즐겨찾기수를 조회하는 SQL문을 작성해주세요. 이때 결과는 음식 종류를 기준으로 내림차순 정렬해주세요.

## Sol. 0
```sql
SELECT FOOD_TYPE, REST_ID, REST_NAME, MAX(FAVORITES) AS FAVORITES
FROM REST_INFO
GROUP BY FOOD_TYPE
ORDER BY 1 DESC;
```

위 구문 작성 시, `FOOD_TYPE`의 그룹별 `MAX FAVORITES`은 추출이 되지만   
`MAX FAVORITES`에 해당하는 행의 `REST_ID`와 `REST_NAME`을 불러오지 못하는 현상이 발생해 오답처리됨  

## Sol. 1
```sql
-- FOOD_TYPE 그룹별 최대 좋아요 수를 받은 행을 WHERE절을 통해 불러와 
-- SELECT을 통한 최대 좋아요수에  맞는 컬럼값들 불러오기
SELECT FOOD_TYPE, REST_ID, REST_NAME, FAVORITES
FROM REST_INFO
WHERE (FOOD_TYPE, FAVORITES) IN (    
    SELECT FOOD_TYPE, MAX(FAVORITES) 
    FROM REST_INFO
    GROUP BY FOOD_TYPE 
)
ORDER BY 1 DESC;
```

`WHERE ~ IN` 구문을 써서 서브쿼리 내 그룹별 `MAX(FAVORITES)`에 해당하는 행들을 불러오는 방식을 채택