# 업그레이드 할 수 없는 아이템 구하기
https://school.programmers.co.kr/learn/courses/30/lessons/273712

## 문제
문제
더 이상 업그레이드할 수 없는 아이템의 아이템 ID(ITEM_ID), 아이템 명(ITEM_NAME), 아이템의 희귀도(RARITY)를 출력하는 SQL 문을 작성해 주세요. 이때 결과는 아이템 ID를 기준으로 내림차순 정렬해 주세요.

예시
예를 들어 `ITEM_INFO` 테이블이 다음과 같고

|ITEM_ID|ITEM_NAME|RARITY|PRICE|
|-|-|-|-|
|0|ITEM_A|RARE|10000|
|1|ITEM_B|RARE|9000|
|2|ITEM_C|LEGEND|11000|
|3|ITEM_D|RARE|10000|
|4|ITEM_E|RARE|12000|

`ITEM_TREE` 테이블이 다음과 같다면

|ITEM_ID|PARENT_ITEM_ID|
|-|-|
|0|NULL|
|1|0|
|2|0|
|3|1|
|4|1|

'ITEM_A' 는 'ITEM_B', 'ITEM_C' 로 업그레이드가 가능하며 'ITEM_B' 는 'ITEM_D', 'ITEM_E' 로 업그레이드가 가능합니다. 'ITEM_C', 'ITEM_D', 'ITEM_E' 는 더 이상 업그레이드가 가능하지 않으므로 결과는 다음과 같이 나와야 합니다.

|ITEM_ID|ITEM_NAME|RARITY|
|-|-|-|
|4|ITEM_E|RARE|
|3|ITEM_D|RARE|
|2|ITEM_C|LEGEND|

## Sol. 0
```sql
SELECT ITEM_ID, ITEM_NAME, RARITY
FROM ITEM_INFO
-- PARENT_ITEM_ID가 NULL(부모 아이템)이 아니고, 부모 아이템 ID에 포함되지 않는 ITEM_ID가  
-- 더 이상 업그레이드 할 수 없는 최종 아이템 인것을 확인할 수 있음
WHERE ITEM_ID NOT IN (SELECT DISTINCT(PARENT_ITEM_ID)
                        FROM ITEM_TREE
                        WHERE PARENT_ITEM_ID IS NOT NULL)
ORDER BY ITEM_ID DESC;
```