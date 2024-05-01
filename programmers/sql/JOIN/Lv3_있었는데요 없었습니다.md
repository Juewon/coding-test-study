# 있었는데요 없었습니다
https://school.programmers.co.kr/learn/courses/30/lessons/59043

## 문제
관리자의 실수로 일부 동물의 입양일이 잘못 입력되었습니다. 보호 시작일보다 입양일이 더 빠른 동물의 아이디와 이름을 조회하는 SQL문을 작성해주세요. 이때 결과는 보호 시작일이 빠른 순으로 조회해야합니다.

예시
예를 들어, `ANIMAL_INS` 테이블과 `ANIMAL_OUTS` 테이블이 다음과 같다면

`ANIMAL_INS`

|ANIMAL_ID|ANIMAL_TYPE|DATETIME|INTAKE_CONDITION|NAME|SEX_UPON_INTAKE|
|-|-|-|-|-|-|
|A350276|Cat|2017-08-13 13:50:00|Normal|Jewel|Spayed Female|
|A381217|Dog|2017-07-08 09:41:00|Sick|Cherokee|Neutered Male|

`ANIMAL_OUTS`

|ANIMAL_ID|ANIMAL_TYPE|DATETIME|NAME|SEX_UPON_OUTCOME|
|-|-|-|-|-|
|A350276|Cat|2018-01-28 17:51:00|Jewel|Spayed Female|
|A381217|Dog|2017-06-09 18:51:00|Cherokee|Neutered Male|

SQL문을 실행하면 다음과 같이 나와야 합니다.

|ANIMAL_ID|NAME|
|-|-|
|A381217|Cherokee|

## Sol. 0
입양 일시가 보호 일시보다 작으면 결과를 출력
```sql
SELECT A.ANIMAL_ID, A.NAME
FROM ANIMAL_INS A
JOIN ANIMAL_OUTS B ON A.ANIMAL_ID = B.ANIMAL_ID
WHERE B.DATETIME < A.DATETIME
ORDER BY A.DATETIME;
```