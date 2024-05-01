# 저자 별 카테고리 별 매출액 집계하기
https://school.programmers.co.kr/learn/courses/30/lessons/144856

## 문제
**문제 설명**   
`2022년 1`월의 도서 판매 데이터를 기준으로 저자 별, 카테고리 별 매출액(`TOTAL_SALES = 판매량 * 판매가`) 을 구하여, 저자 ID(`AUTHOR_ID`), 저자명(`AUTHOR_NAME`), 카테고리(`CATEGORY`), 매출액(`SALES`) 리스트를 출력하는 SQL문을 작성해주세요.   
결과는 저자 ID를 오름차순으로, 저자 ID가 같다면 카테고리를 내림차순 정렬해주세요.

## Sol. 0
```sql
-- 4. 출력할 컬럼 설정, 조건에 맞는 ROW들의 PRICE와 SALES를 곱한 새로운 TOTAL_SALES 컬럼 생성
SELECT B.AUTHOR_ID, B.AUTHOR_NAME, A.CATEGORY, SUM(A.PRICE * C.SALES) AS TOTAL_SALES
-- 1. 세개 테이블들을 조인
FROM BOOK A
JOIN AUTHOR B ON A.AUTHOR_ID = B.AUTHOR_ID
JOIN BOOK_SALES C ON A.BOOK_ID = C.BOOK_ID
-- 2. 2022년 1월에 판매된 도서 데이터만 불러오기
WHERE C.SALES_DATE BETWEEN '2022-01-01' AND '2022-01-31'
-- 3. 저자별, 카테고리별로 그룹핑
GROUP BY B.AUTHOR_ID, A.CATEGORY
ORDER BY B.AUTHOR_ID, A.CATEGORY DESC;
```