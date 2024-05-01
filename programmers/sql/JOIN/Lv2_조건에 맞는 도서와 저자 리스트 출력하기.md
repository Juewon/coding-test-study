# 조건에 맞는 도서와 저자 리스트 출력하기
https://school.programmers.co.kr/learn/courses/30/lessons/144854

## 문제
`'경제'` 카테고리에 속하는 도서들의 도서 ID(`BOOK_ID`), 저자명(`AUTHOR_NAME`), 출판일(`PUBLISHED_DATE`) 리스트를 출력하는 SQL문을 작성해주세요.
결과는 출판일을 기준으로 오름차순 정렬해주세요.

**예시**
예를 들어 `BOOK` 테이블과 `AUTHOR` 테이블이 다음과 같다면

|BOOK_ID|CATEGORY|AUTHOR_ID|PRICE|PUBLISHED_DATE|
|-|-|-|-|-|
|1|인문|1|10000|2020-01-01|
|2|경제|1|9000|2021-04-11|
|3|경제|2|11000|2021-02-05|

|AUTHOR_ID|AUTHOR_NAME|
|-|-|
|1|홍길동|
|2|김영호|

`'경제'` 카테고리에 속하는 도서는 도서 ID가 2, 3인 도서이고, 출판일을 기준으로 오름차순으로 정렬하면 다음과 같은 결과가 나와야 합니다.

|BOOK_ID|AUTHOR_NAME|PUBLISHED_DATE|
|-|-|-|
|3|김영호|2021-02-05|
|2|홍길동|2021-04-11|

**주의사항**   
`PUBLISHED_DATE`의 데이트 포맷이 예시와 동일해야 정답처리 됩니다.

## Sol. 0
```sql
SELECT A.BOOK_ID, B.AUTHOR_NAME, DATE_FORMAT(A.PUBLISHED_DATE, '%Y-%m-%d')
FROM BOOK A
JOIN AUTHOR B ON A.AUTHOR_ID = B.AUTHOR_ID
WHERE CATEGORY = '경제'
ORDER BY A.PUBLISHED_DATE ASC;
```