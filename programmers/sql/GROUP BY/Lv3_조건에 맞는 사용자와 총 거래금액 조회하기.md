# 조건에 맞는 사용자와 총 거래금액 조회하기
https://school.programmers.co.kr/learn/courses/30/lessons/164668

## 문제
**문제 설명**   
`USED_GOODS_BOARD`와 `USED_GOODS_USER`테이블에서 완료된 중고 거래의 총금액이 70만 원 이상인 사람의 회원 ID, 닉네임, 총거래금액을 조회하는 SQL문을 작성해주세요. 결과는 총거래금액을 기준으로 오름차순 정렬해주세요.

## Sol. 0
```sql
-- 4. 출력한 컬럼명 나열, 판매한 금액을 SUM한 TOTAL_SALES 출력
SELECT B.USER_ID, B.NICKNAME, SUM(A.PRICE) AS TOTAL_SALES
-- 1. GOODS_BOARD 와 GOODS_USER 테이블을 ID 컬럼으로 조인
FROM USED_GOODS_BOARD A
JOIN USED_GOODS_USER B ON A.WRITER_ID = B.USER_ID
-- 2. 판매가 완료된 상품만 가져오기 위해 조건 추가
WHERE A.STATUS = 'DONE'
-- 3. 판매가 완료된 상품의 가격이 70만원 이상인 ID들만 그룹화하기
GROUP BY A.WRITER_ID
HAVING SUM(A.PRICE) >= 700000
-- 5.TOTAL_SALES를 기준으로 오름차순하기
ORDER BY 3;
```