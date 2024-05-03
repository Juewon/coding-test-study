# 조건에 맞는 아이템들의 가격의 총합 구하기
https://school.programmers.co.kr/learn/courses/30/lessons/273709

## 문제
`ITEM_INFO` 테이블에서 희귀도가 'LEGEND'인 아이템들의 가격의 총합을 구하는 SQL문을 작성해 주세요.   
 이때 컬럼명은 'TOTAL_PRICE'로 지정해 주세요.

**예시**   
예를 들어 `ITEM_INFO` 테이블이 다음과 같다면

| ITEM_ID | ITEM_NAME | RARITY | PRICE |
|---------|-----------|--------|-------|
| 0       | ITEM_A    | COMMON | 10000 |
| 1       | ITEM_B    | LEGEND | 9000  |
| 2       | ITEM_C    | LEGEND | 11000 |
| 3       | ITEM_D    | UNIQUE | 10000 |
| 4       | ITEM_E    | LEGEND | 12000 |

조건에 해당되는 아이템의 아이템 ID는 1, 2, 4이며 각 아이템들의 가격은 9000, 11000, 12000 이므로 조건에 해당되는 아이템들의 가격의 합은 다음과 같습니다.

|TOTAL_PRICE|
|-|
|32000|

## Sol. 0
```sql
SELECT SUM(PRICE) AS TOTAL_PRICE
FROM ITEM_INFO
WHERE RARITY = 'LEGEND';
```