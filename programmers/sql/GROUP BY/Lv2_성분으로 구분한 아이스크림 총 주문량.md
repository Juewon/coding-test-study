# 성분으로 구분한 아이스크림 총 주문량
https://school.programmers.co.kr/learn/courses/30/lessons/133026

## 문제
상반기 동안 각 아이스크림 성분 타입과 성분 타입에 대한 아이스크림의 총주문량을 총주문량이 작은 순서대로 조회하는 SQL 문을 작성해주세요. 이때 총주문량을 나타내는 컬럼명은 TOTAL_ORDER로 지정해주세요.

**예시**   
예를 들어 `FIRST_HALF` 테이블이 다음과 같고

| SHIPMENT_ID | FLAVOR           | TOTAL_ORDER |
|-------------|------------------|-------------|
| 101         | chocolate        | 3200        |
| 102         | vanilla          | 2800        |
| 103         | mint_chocolate  | 1700        |
| 104         | caramel          | 2600        |
| 105         | white_chocolate  | 3100        |
| 106         | peach            | 2450        |
| 107         | watermelon       | 2150        |
| 108         | mango            | 2900        |
| 109         | strawberry       | 3100        |
| 110         | melon            | 3150        |
| 111         | orange           | 2900        |
| 112         | pineapple        | 2900        |


`ICECREAM_INFO` 테이블이 다음과 같다면

| FLAVOR          | INGREDIENT_TYPE |
|-----------------|------------------|
| chocolate       | sugar_based      |
| vanilla         | sugar_based      |
| mint_chocolate  | sugar_based      |
| caramel         | sugar_based      |
| white_chocolate | sugar_based      |
| peach           | fruit_based      |
| watermelon      | fruit_based      |
| mango           | fruit_based      |
| strawberry      | fruit_based      |
| melon           | fruit_based      |
| orange          | fruit_based      |
| pineapple       | fruit_based      |


상반기에 아이스크림의 주 성분이 설탕인 아이스크림들에 대한 총주문량을 구하면 3,200 + 2,800 + 1,700 + 2,600 + 3,100 = 13,400입니다. 아이스크림의 주 성분이 과일인 아이스크림들에 대한 총주문량을 구하면 3,100 + 2,450 + 2,150 + 2,900 + 3,150 + 2,900 + 2,900 = 19,550입니다. 따라서 총주문량이 작은 순서대로 조회하는 SQL 문을 실행하면 다음과 같이 나와야 합니다.

|INGREDIENT_TYPE|TOTAL_ORDER|
|-|-|
|sugar_based|13400|
|fruit_based|19550|

## Sol. 0
```sql
-- 조인을 통해 아이스크림 성분별 총 주문량 추출
SELECT B.INGREDIENT_TYPE, SUM(A.TOTAL_ORDER) AS TOTAL_ORDER
FROM FIRST_HALF A
JOIN ICECREAM_INFO B ON A.FLAVOR = B.FLAVOR
GROUP BY B.INGREDIENT_TYPE
ORDER BY TOTAL_ORDER;
```