# 상품 별 오프라인 매출 구하기
https://school.programmers.co.kr/learn/courses/30/lessons/131533

## 문제
`PRODUCT` 테이블과 `OFFLINE_SALE` 테이블에서 상품코드 별 매출액(판매가 * 판매량) 합계를 출력하는 SQL문을 작성해주세요. 결과는 매출액을 기준으로 내림차순 정렬해주시고 매출액이 같다면 상품코드를 기준으로 오름차순 정렬해주세요.

**예시**
예를 들어 `PRODUCT` 테이블이 다음과 같고

|PRODUCT_ID|PRODUCT_CODE|PRICE|
|-|-|-|
|1|A1000011|15000|
|2|A1000045|8000|
|3|C3000002|42000|

`OFFLINE_SALE` 테이블이 다음과 같다면

|OFFLINE_SALE_ID|PRODUCT_ID|SALES_AMOUNT|SALES_DATE|
|-|-|-|-|
|1|1|2|2022-02-21|
|2|1|2|2022-03-02|
|3|3|3|2022-05-01|
|4|2|1|2022-05-24|
|5|1|2|2022-07-14|
|6|2|1|2022-09-22|

각 상품 별 총 판매량과 판매가는 다음과 같습니다.

- `PRODUCT_CODE` 가 `A1000011`인 상품은 총 판매량이 6개, 판매가가 15,000원
- `PRODUCT_CODE` 가 `A1000045`인 상품은 총 판매량이 2개, 판매가가 8,000원
- `PRODUCT_CODE` 가 `C3000002`인 상품은 총 판매량이 3개, 판매가가 42,000원

그러므로 각 상품 별 매출액을 계산하고 정렬하면 결과가 다음과 같이 나와야 합니다.

|PRODUCT_CODE|SALES|
|-|-|
|C3000002|126000|
|A1000011|90000|
|A1000045|16000|

## Sol. 0
```sql
-- PRODUCT_ID 별 총 판매 금액 추출 후 정렬하기
SELECT A.PRODUCT_CODE, SUM(A.PRICE * B.SALES_AMOUNT) AS SALES
FROM PRODUCT A
JOIN OFFLINE_SALE B ON A.PRODUCT_ID = B.PRODUCT_ID
GROUP BY B.PRODUCT_ID
ORDER BY SALES DESC, A.PRODUCT_CODE;
```