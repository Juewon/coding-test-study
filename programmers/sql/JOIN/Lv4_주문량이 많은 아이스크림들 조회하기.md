# 주문량이 많은 아이스크림들 조회하기
https://school.programmers.co.kr/learn/courses/30/lessons/133027

## 문제
7월 아이스크림 총 주문량과 상반기의 아이스크림 총 주문량을 더한 값이 큰 순서대로 상위 3개의 맛을 조회하는 SQL 문을 작성해주세요.

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

`JULY`테이블이 다음과 같다면

| SHIPMENT_ID | FLAVOR           | TOTAL_ORDER |
|-------------|------------------|-------------|
| 101         | chocolate        | 520         |
| 102         | vanilla          | 560         |
| 103         | mint_chocolate   | 400         |
| 104         | caramel          | 460         |
| 105         | white_chocolate  | 350         |
| 106         | peach            | 500         |
| 107         | watermelon       | 780         |
| 108         | mango            | 790         |
| 109         | strawberry       | 520         |
| 110         | melon            | 400         |
| 111         | orange           | 250         |
| 112         | pineapple        | 200         |
| 208         | mango            | 110         |
| 209         | strawberry       | 220         |

7월 아이스크림 총주문량과 상반기의 아이스크림 총 주문량을 더한 값이 큰 순서대로 상위 3개의 맛을 조회하면 strawberry(520 + 220 + 3,100 = 3,840), mango(790 + 110 + 2,900 = 3,800), chocolate(520 + 3,200 = 3,720) 순입니다. 따라서 SQL 문을 실행하면 다음과 같이 나와야 합니다.

|FLAVOR|
|-|
|strawberry|
|mango|
|chocolate|

## Sol. 0
```sql
SELECT A.FLAVOR
FROM FIRST_HALF A
-- JULY 테이블의 FLAVOR 별 총 TOTAL_ORDER를 구한 서브 쿼리 생성
-- JULY 테이블의 FLAVOR 열은 중복행이 있어 서브쿼리를 따로 생성해 각 FLAVOR에 대한 합계를 구하는 방식
-- 생성 후 FIRST_HALF 테이블과 INNER JOIN
JOIN (SELECT FLAVOR, SUM(TOTAL_ORDER) AS TOTAL_ORDER
        FROM JULY
        GROUP BY FLAVOR) B
ON A.FLAVOR = B.FLAVOR
-- 두 테이블의 TOTAL_ORDER를 합한 값을 기준으로 내림차순 정렬 후
ORDER BY A.TOTAL_ORDER + B.TOTAL_ORDER DESC
-- 상위 3개만 추출
LIMIT 3;
```