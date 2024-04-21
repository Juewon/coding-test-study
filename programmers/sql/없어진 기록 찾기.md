# 없어진 기록 찾기
https://school.programmers.co.kr/learn/courses/30/lessons/59042

## 문제
천재지변으로 인해 일부 데이터가 유실되었습니다. 입양을 간 기록은 있는데, 보호소에 들어온 기록이 없는 동물의 ID와 이름을 ID 순으로 조회하는 SQL문을 작성해주세요.

**예시**
예를 들어, `ANIMAL_INS` 테이블과 `ANIMAL_OUTS` 테이블이 다음과 같다면

`ANIMAL_INS`

|ANIMAL_ID|ANIMAL_TYPE|DATETIME|INTAKE_CONDITION|NAME|SEX_UPON_INTAKE|
|-|-|-|-|-|-|
|A352713|Cat|2017-04-13|16:29:00|Normal|Gia|Spayed Female|
|A350375|Cat|2017-03-06|15:01:00|Normal|Meo|Neutered Male|

`ANIMAL_OUTS`

|ANIMAL_ID|ANIMAL_TYPE|DATETIME|NAME|SEX_UPON_OUTCOME|
|-|-|-|-|-|
|A349733|Dog|2017-09-27|19:09:00|Allie|Spayed Female|
|A352713|Cat|2017-04-25|12:25:00|Gia|Spayed Female|
|A349990|Cat|2018-02-02|14:18:00|Spice|Spayed Female|

`ANIMAL_OUTS` 테이블에서
- Allie의 ID는 ANIMAL_INS에 없으므로, Allie의 데이터는 유실되었습니다.
- Gia의 ID는 ANIMAL_INS에 있으므로, Gia의 데이터는 유실되지 않았습니다.
- Spice의 ID는 ANIMAL_INS에 없으므로, Spice의 데이터는 유실되었습니다.   
따라서 SQL문을 실행하면 다음과 같이 나와야 합니다.

|ANIMAL_ID|NAME|
|-|-|
|A349733|Allie|
|A349990|Spice|

## Sol. 0
`ANIMAL_OUTS` 테이블엔 있지만, `ANIMAL_INS` 테이블엔 없는 데이터 찾기   
외부조인을 활용해 NULL이 아닌 값 찾기
```sql
SELECT B.ANIMAL_ID, B.NAME
FROM ANIMAL_INS A
RIGHT OUTER JOIN ANIMAL_OUTS B ON A.ANIMAL_ID = B.ANIMAL_ID
WHERE A.ANIMAL_ID IS NULL;
```