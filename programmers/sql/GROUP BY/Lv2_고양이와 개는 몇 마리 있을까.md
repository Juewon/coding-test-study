# 고양이와 개는 몇 마리 있을까
https://school.programmers.co.kr/learn/courses/30/lessons/59040

## 문제
동물 보호소에 들어온 동물 중 고양이와 개가 각각 몇 마리인지 조회하는 SQL문을 작성해주세요. 이때 고양이를 개보다 먼저 조회해주세요.

**예시**   
예를 들어 `ANIMAL_INS` 테이블이 다음과 같다면

|ANIMAL_ID|ANIMAL_TYPE|DATETIME|INTAKE_CONDITION|NAME|SEX_UPON_INTAKE|
|-|-|-|-|-|-|
|A373219|Cat|2014-07-29 11:43:00|Normal|Ella|Spayed Female|
|A377750|Dog|2017-10-25 17:17:00|Normal|Lucy|Spayed Female|
|A354540|Cat|2014-12-11 11:48:00|Normal|Tux	|Neutered Male|

고양이는 2마리, 개는 1마리 들어왔습니다. 따라서 SQL문을 실행하면 다음과 같이 나와야 합니다.

|ANIMAL_TYPE|count|
|-|-|
|Cat|2|
|Dog|1|

## Sol. 0
```sql
-- ORDER BY절을 활용해 Dog보다 Cat이 먼저 출력
SELECT ANIMAL_TYPE, COUNT(ANIMAL_TYPE) AS count
FROM ANIMAL_INS
WHERE ANIMAL_TYPE IN ('Cat', 'Dog')
GROUP BY ANIMAL_TYPE
ORDER BY ANIMAL_TYPE;
```