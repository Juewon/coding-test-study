# NULL 처리하기
https://school.programmers.co.kr/learn/courses/30/lessons/59410

## 문제
입양 게시판에 동물 정보를 게시하려 합니다. 동물의 생물 종, 이름, 성별 및 중성화 여부를 아이디 순으로 조회하는 SQL문을 작성해주세요. 이때 프로그래밍을 모르는 사람들은 NULL이라는 기호를 모르기 때문에, 이름이 없는 동물의 이름은 "No name"으로 표시해 주세요.

**예시**   
예를 들어 `ANIMAL_INS` 테이블이 다음과 같다면

| ANIMAL_ID | ANIMAL_TYPE | DATETIME            | INTAKE_CONDITION | NAME  | SEX_UPON_INTAKE |
|-----------|-------------|---------------------|------------------|-------|-----------------|
| A350276   | Cat         | 2017-08-13 13:50:00 | Normal           | Jewel | Spayed Female   |
| A350375   | Cat         | 2017-03-06 15:01:00 | Normal           | Meo   | Neutered Male   |
| A368930   | Dog         | 2014-06-08 13:20:00 | Normal           | NULL  | Spayed Female   |

마지막 줄의 개는 이름이 없기 때문에, 이 개의 이름은 "No name"으로 표시합니다. 따라서 SQL문을 실행하면 다음과 같이 나와야 합니다.

|ANIMAL_TYPE|NAME|SEX_UPON_INTAKE|
|-|-|-|
|Cat|Jewel|Spayed Female|
|Cat|Meo|Neutered Male|
|Dog|No name|Spayed Female|

## Sol. 0
```sql
-- IFNULL : NULL을 대체할 문자열 삽입 가능
SELECT ANIMAL_TYPE, IFNULL(NAME, 'No name') AS NAME, SEX_UPON_INTAKE
FROM ANIMAL_INS;
```