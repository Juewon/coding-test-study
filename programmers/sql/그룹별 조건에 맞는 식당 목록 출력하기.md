# 그룹별 조건에 맞는 식당 목록 출력하기
https://school.programmers.co.kr/learn/courses/30/lessons/131124

## 문제
`MEMBER_PROFILE`와 `REST_REVIEW` 테이블에서 리뷰를 가장 많이 작성한 회원의 리뷰들을 조회하는 SQL문을 작성해주세요. 회원 이름, 리뷰 텍스트, 리뷰 작성일이 출력되도록 작성해주시고, 결과는 리뷰 작성일을 기준으로 오름차순, 리뷰 작성일이 같다면 리뷰 텍스트를 기준으로 오름차순 정렬해주세요.

**예시**   
`MEMBER_PROFILE` 테이블이 다음과 같고

| MEMBER_ID             | MEMBER_NAME | TLNO        | GENDER | DATE_OF_BIRTH |
|-----------------------|-------------|-------------|--------|---------------|
| jiho92@naver.com      | 이지호       | 01076432111 | W      | 1992-02-12    |
| jiyoon22@hotmail.com  | 김지윤       | 01032324117 | W      | 1992-02-22    |
| jihoon93@hanmail.net  | 김지훈       | 01023258688 | M      | 1993-02-23    |
| seoyeons@naver.com    | 박서연       | 01076482209 | W      | 1993-03-16    |
| yelin1130@gmail.com   | 조예린       | 01017626711 | W      | 1990-11-30    |

`REST_REVIEW` 테이블이 다음과 같을 때

| REVIEW_ID   | REST_ID | MEMBER_ID           | REVIEW_SCORE | REVIEW_TEXT               | REVIEW_DATE |
|-------------|---------|---------------------|--------------|---------------------------|-------------|
| R000000065  | 00028   | soobin97@naver.com | 5            | 부찌 국물에서 샤브샤브 맛이나고 깔끔 | 2022-04-12  |
| R000000066  | 00039   | yelin1130@gmail.com| 5            | 김치찌개 최곱니다.             | 2022-02-12  |
| R000000067  | 00028   | yelin1130@gmail.com| 5            | 햄이 많아서 좋아요             | 2022-02-22  |
| R000000068  | 00035   | ksyi0316@gmail.com | 5            | 숙성회가 끝내줍니다.           | 2022-02-15  |
| R000000069  | 00035   | yoonsy95@naver.com | 4            | 비린내가 전혀없어요.           | 2022-04-16  |

SQL을 실행하면 다음과 같이 출력되어야 합니다.

|MEMBER_NAME|REVIEW_TEXT|REVIEW_DATE|
|-|-|-|
|조예린|김치찌개 최곱니다.|2022-02-12|
|조예린|햄이 많아서 좋아요|2022-02-22|

## Sol. 0
```sql
SELECT A.MEMBER_NAME, B.REVIEW_TEXT, DATE_FORMAT(B.REVIEW_DATE, '%Y-%m-%d') AS REVIEW_DATE
FROM MEMBER_PROFILE A
JOIN REST_REVIEW B ON A.MEMBER_ID = B.MEMBER_ID
-- where절의 서브 쿼리를 통해 REST_REVIEW에서 리뷰를 가장 많이 남긴 
-- 사람의 행들을 필터링하기
WHERE B.MEMBER_ID = (SELECT MEMBER_ID
                        FROM REST_REVIEW
                        GROUP BY MEMBER_ID
                        -- 서브 쿼리 내에서는 ORDER BY절에서 COUNT 함수를 사용할 수 있음
                        ORDER BY COUNT(REVIEW_TEXT) DESC
                        LIMIT 1)
ORDER BY B.REVIEW_DATE, B.REVIEW_TEXT;
```
