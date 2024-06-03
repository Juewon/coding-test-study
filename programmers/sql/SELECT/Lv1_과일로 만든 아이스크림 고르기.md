# 과일로 만든 아이스크림 고르기
https://school.programmers.co.kr/learn/courses/30/lessons/133025

## 문제
상반기 아이스크림 총주문량이 3,000보다 높으면서 아이스크림의 주 성분이 과일인 아이스크림의 맛을 총주문량이 큰 순서대로 조회하는 SQL 문을 작성해주세요.

**예시**   
예를 들어 `FIRST_HALF` 테이블이 다음과 같고

| SHIPMENT_ID | FLAVOR           | TOTAL_ORDER |
|-------------|------------------|-------------|
| 101         | chocolate        | 3200        |
| 102         | vanilla          | 2800        |
| 103         | mint_chocolate   | 1700        |
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

| FLAVOR           | INGREDIENT_TYPE |
|------------------|-----------------|
| chocolate        | sugar_based     |
| vanilla          | sugar_based     |
| mint_chocolate   | sugar_based     |
| caramel          | sugar_based     |
| white_chocolate  | sugar_based     |
| peach            | fruit_based     |
| watermelon       | fruit_based     |
| mango            | fruit_based     |
| strawberry       | fruit_based     |
| melon            | fruit_based     |
| orange           | fruit_based     |
| pineapple        | fruit_based     |

상반기 아이스크림 총주문량이 3,000보다 높은 아이스크림 맛은 chocolate, strawberry, melon, white_chocolate입니다. 이 중에 아이스크림의 주 성분이 과일인 아이스크림 맛은 strawberry와 melon이고 총주문량이 큰 순서대로 아이스크림 맛을 조회하면 melon, strawberry 순으로 조회되어야 합니다. 따라서 SQL 문을 실행하면 다음과 같이 나와야 합니다.

|FLAVOR|
|-|
|melon|
|strawberry|

## Sol. 0
```sql
-- 아이스크림의 주 성분이 과일이고 총 주문량이 3000보다 높은 아이스크림 맛 추출하기
SELECT A.FLAVOR
FROM FIRST_HALF A
JOIN ICECREAM_INFO B ON A.FLAVOR = B.FLAVOR
WHERE B.INGREDIENT_TYPE = 'fruit_based' 
        AND A.TOTAL_ORDER > 3000
ORDER BY A.TOTAL_ORDER DESC;
```