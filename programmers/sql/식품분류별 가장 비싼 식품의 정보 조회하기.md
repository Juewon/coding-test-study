# 식품분류별 가장 비싼 식품의 정보 조회하기
https://school.programmers.co.kr/learn/courses/30/lessons/131116

## 문제
`FOOD_PRODUCT` 테이블에서 식품분류별로 가격이 제일 비싼 식품의 분류, 가격, 이름을 조회하는 SQL문을 작성해주세요. 이때 식품분류가 '과자', '국', '김치', '식용유'인 경우만 출력시켜 주시고 결과는 식품 가격을 기준으로 내림차순 정렬해주세요.

**예시**   
`FOOD_PRODUCT` 테이블이 다음과 같을 때

| PRODUCT_ID | PRODUCT_NAME     | PRODUCT_CD | CATEGORY | PRICE |
|------------|------------------|------------|----------|-------|
| P0018      | 맛있는고추기름   | CD_OL00008 | 식용유   | 6100  |
| P0019      | 맛있는카놀라유   | CD_OL00009 | 식용유   | 5100  |
| P0020      | 맛있는산초유     | CD_OL00010 | 식용유   | 6500  |
| P0021      | 맛있는케첩       | CD_SC00001 | 소스     | 4500  |
| P0022      | 맛있는마요네즈   | CD_SC00002 | 소스     | 4700  |
| P0039      | 맛있는황도       | CD_CN00008 | 캔       | 4100  |
| P0040      | 맛있는명이나물   | CD_CN00009 | 캔       | 3500  |
| P0041      | 맛있는보리차     | CD_TE00010 | 차       | 3400  |
| P0042      | 맛있는메밀차     | CD_TE00001 | 차       | 3500  |
| P0099      | 맛있는맛동산     | CD_CK00002 | 과자     | 1800  |

SQL을 실행하면 다음과 같이 출력되어야 합니다.

| CATEGORY | MAX_PRICE | PRODUCT_NAME |
|----------|-----------|--------------|
| 식용유   | 6500      | 맛있는산초유 |
| 과자     | 1800      | 맛있는맛동산 |

## Sol. 0
- GROUP BY 사용 시, 그룹화된 열의 각 그룹에 대해 하나의 행만 반환
- 이러한 경우 MAX(PRICE)는 각 카테고리 별 최대 가격을 의미하지만, 그에 해당하는 PRODUCT_NAME을 어떻게 선택해야 할지 명확하지 않기 때문에 결과가 이상하게 나옴
- 가격이 최대인 제품의 이름을 가져오기 위해 서브쿼리를 활용해 원하는 조건에 맞는 ROW자체를 불러와 그에 맞게 PRODUCT_NAME을 출력

```sql
-- 가져온 ROW에서 PRODUCT_NAME을 불러오기
SELECT CATEGORY, PRICE AS MAX_PRICE, PRODUCT_NAME
FROM FOOD_PRODUCT
-- WHERE 절 서브쿼리를 활용해 조건에 맞는 ROW들을 불러옴
WHERE (CATEGORY, PRICE) IN 
        (SELECT CATEGORY, MAX(PRICE) AS PRICE
            FROM FOOD_PRODUCT
            WHERE CATEGORY IN ('과자', '국', '김치', '식용유')
            GROUP BY CATEGORY)
ORDER BY MAX_PRICE DESC;
```