# 조건에 맞는 사용자 정보 조회하기
https://school.programmers.co.kr/learn/courses/30/lessons/164670

## 문제
`USED_GOODS_BOARD`와 `USED_GOODS_USER` 테이블에서 중고 거래 게시물을 3건 이상 등록한 사용자의 사용자 ID, 닉네임, 전체주소, 전화번호를 조회하는 SQL문을 작성해주세요. 이때, 전체 주소는 시, 도로명 주소, 상세 주소가 함께 출력되도록 해주시고, 전화번호의 경우 xxx-xxxx-xxxx 같은 형태로 하이픈 문자열(`-`)을 삽입하여 출력해주세요. 결과는 회원 ID를 기준으로 내림차순 정렬해주세요.

**예시**   
`USED_GOODS_BOARD` 테이블이 다음과 같고

| BOARD_ID | WRITER_ID  | TITLE            | CONTENTS                            | PRICE  | CREATED_DATE | STATUS | VIEWS |
|----------|------------|------------------|-------------------------------------|--------|--------------|--------|-------|
| B0001    | dhfkzmf09  | 칼라거펠트 코트 | 양모 70%이상 코트입니다.           | 120000 | 2022-10-14   | DONE   | 104   |
| B0002    | lee871201  | 국내산 볶음참깨 | 직접 농사지은 참깨입니다.          | 3000   | 2022-10-02   | DONE   | 121   |
| B0003    | dhfkzmf09  | 나이키 숏패팅    | 사이즈는 M입니다.                    | 40000  | 2022-10-17   | DONE   | 98    |
| B0004    | kwag98     | 반려견 배변패드 팝니다 | 정말 저렴히 판매합니다. 전부 미개봉 새상품입니다. | 12000 | 2022-10-01   | DONE   | 250   |
| B0005    | dhfkzmf09  | PS4             | PS5 구매로인해 팝니다.             | 250000 | 2022-11-03   | DONE   | 111   |

`USED_GOODS_USER` 테이블이 다음과 같을 때

| USER_ID  | NICKNAME   | CITY   | STREET_ADDRESS1      | STREET_ADDRESS2   | TLNO       |
|----------|------------|--------|----------------------|-------------------|------------|
| dhfkzmf09| 찐찐       | 성남시 | 분당구 수내로 13     | A동 1107호        | 01053422914|
| dlPcks90 | 썹썹       | 성남시 | 분당구 수내로 74     | 401호             | 01034573944|
| cjfwls91 | 점심만금식 | 성남시 | 분당구 내정로 185    | 501호             | 01036344964|
| dlfghks94| 희망       | 성남시 | 분당구 내정로 101    | 203동 102호       | 01032634154|
| rkdhs95  | 용기       | 성남시 | 분당구 수내로 23     | 501호             | 01074564564|

SQL을 실행하면 다음과 같이 출력되어야 합니다.

|USER_ID|NICKNAME|전체주소|전화번호|
|-|-|-|-|
|dhfkzmf09|찐찐|성남시 분당구 수내로 13 A동 1107호|010-5342-2914|

## Sol. 0
```sql
SELECT UGU.USER_ID, 
        UGU.NICKNAME, 
        -- CONCAT으로 각 컬럼값들을 합친 전체주소 출력
        CONCAT(UGU.CITY, ' ', UGU.STREET_ADDRESS1, ' ', UGU.STREET_ADDRESS2) AS '전체주소',
        -- CONCAT + SUBSTR로 전화번호에 하이폰(-)을 삽입해 출력
        -- SUBSTR(문자열, 시작문자, 출력할 길이)
        CONCAT(SUBSTR(UGU.TLNO, 1, 3), '-', SUBSTR(UGU.TLNO, 4, 4), '-', SUBSTR(UGU.TLNO, 8)) AS '전화번호'
-- 서브쿼리를 활용해 USED_GOODS_BOARD의 3번 이상 게시물을 작성한 USER(WRITER_ID) 추출 
FROM (SELECT WRITER_ID
        FROM USED_GOODS_BOARD
        GROUP BY WRITER_ID
        HAVING COUNT(*) >= 3) UGB
JOIN USED_GOODS_USER UGU ON UGB.WRITER_ID = UGU.USER_ID
ORDER BY UGU.USER_ID DESC;
```