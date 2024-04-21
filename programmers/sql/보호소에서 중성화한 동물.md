# 보호소에서 중성화한 동물
https://school.programmers.co.kr/learn/courses/30/lessons/59045

## 문제
보호소에서 중성화 수술을 거친 동물 정보를 알아보려 합니다. 보호소에 들어올 당시에는 중성화1되지 않았지만, 보호소를 나갈 당시에는 중성화된 동물의 아이디와 생물 종, 이름을 조회하는 아이디 순으로 조회하는 SQL 문을 작성해주세요.

**예시**
예를 들어, `ANIMAL_INS` 테이블과 `ANIMAL_OUTS` 테이블이 다음과 같다면

`ANIMAL_INS`

| ANIMAL_ID | ANIMAL_TYPE | DATETIME             | INTAKE_CONDITION | NAME      | SEX_UPON_INTAKE |
|-----------|-------------|----------------------|------------------|-----------|-----------------|
| A367438   | Dog         | 2015-09-10 16:01:00  | Normal           | Cookie    | Spayed Female   |
| A382192   | Dog         | 2015-03-13 13:14:00  | Normal           | Maxwell 2 | Intact Male     |
| A405494   | Dog         | 2014-05-16 14:17:00  | Normal           | Kaila     | Spayed Female   |
| A410330   | Dog         | 2016-09-11 14:09:00  | Sick             | Chewy     | Intact Female   |

`ANIMAL_OUTS`

| ANIMAL_ID | ANIMAL_TYPE | DATETIME             | NAME      | SEX_UPON_OUTCOME |
|-----------|-------------|----------------------|-----------|------------------|
| A367438   | Dog         | 2015-09-12 13:30:00  | Cookie    | Spayed Female   |
| A382192   | Dog         | 2015-03-16 13:46:00  | Maxwell 2 | Neutered Male    |
| A405494   | Dog         | 2014-05-20 11:44:00  | Kaila     | Spayed Female   |
| A410330   | Dog         | 2016-09-13 13:46:00  | Chewy     | Spayed Female   |

- Cookie는 보호소에 들어올 당시에 이미 중성화되어있었습니다.
- Maxwell 2는 보호소에 들어온 후 중성화되었습니다.
- Kaila는 보호소에 들어올 당시에 이미 중성화되어있었습니다.
- Chewy는 보호소에 들어온 후 중성화되었습니다.

따라서 SQL문을 실행하면 다음과 같이 나와야 합니다.

|ANIMAL_ID|ANIMAL_TYPE|NAME|
|-|-|-|
|A382192|Dog|Maxwell 2|
|A410330|Dog|Chewy|

## Sol. 0
```sql
SELECT B.ANIMAL_ID, B.ANIMAL_TYPE, B.NAME
FROM ANIMAL_INS A
JOIN ANIMAL_OUTS B ON A.ANIMAL_ID = B.ANIMAL_ID
-- ANIMAL_INS 와 ANIMAL_OUTS의 SEX_UPON 컬럼의 내용이 달라진 내용을 추출
WHERE A.SEX_UPON_INTAKE != B.SEX_UPON_OUTCOME;
```