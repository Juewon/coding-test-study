# 조회수가 가장 많은 중고거래 게시판의 첨부파일 조회하기
https://school.programmers.co.kr/learn/courses/30/lessons/164671

## 문제
`USED_GOODS_BOARD`와 `USED_GOODS_FILE` 테이블에서 조회수가 가장 높은 중고거래 게시물에 대한 첨부파일 경로를 조회하는 SQL문을 작성해주세요. 첨부파일 경로는 FILE ID를 기준으로 내림차순 정렬해주세요. 기본적인 파일경로는 /home/grep/src/ 이며, 게시글 ID를 기준으로 디렉토리가 구분되고, 파일이름은 파일 ID, 파일 이름, 파일 확장자로 구성되도록 출력해주세요. 조회수가 가장 높은 게시물은 하나만 존재합니다.

**예시**   
`USED_GOODS_BOARD` 테이블이 다음과 같고

| BOARD_ID | WRITER_ID | TITLE                   | CONTENTS                                           | PRICE  | CREATED_DATE | STATUS | VIEWS |
|----------|-----------|-------------------------|----------------------------------------------------|--------|--------------|--------|-------|
| B0001    | kwag98    | 반려견 배변패드 팝니다 | 정말 저렴히 판매합니다. 전부 미개봉 새상품입니다. | 12000  | 2022-10-01   | DONE   | 250   |
| B0002    | lee871201 | 국내산 볶음참깨         | 직접 농사지은 참깨입니다.                           | 3000   | 2022-10-02   | DONE   | 121   |
| B0003    | goung12   | 배드민턴 라켓           | 사놓고 방치만 해서 팝니다.                          | 9000   | 2022-10-02   | SALE   | 212   |
| B0004    | keel1990  | 디올 귀걸이             | 신세계강남점에서 구입. 정품 아닐시 백퍼센트 환불     | 130000 | 2022-10-02   | SALE   | 199   |
| B0005    | haphli01  | 스팸클래식 팔아요       | 유통기한 2025년까지에요                             | 10000  | 2022-10-02   | SALE   | 121   |

`USED_GOODS_FILE` 테이블이 다음과 같을 때

|FILE_ID|FILE_EXT|FILE_NAME|BOARD_ID|
|-|-|-|-|
|IMG_000001|.jpg|photo1|B0001|
|IMG_000002|.jpg|photo2|B0001|
|IMG_000003|.png|사진|B0002|
|IMG_000004|.jpg|사진|B0003|
|IMG_000005|.jpg|photo|B0004|

SQL을 실행하면 다음과 같이 출력되어야 합니다.

|FILE_PATH|
|-|
|/home/grep/src/B0001/IMG_000001photo1.jpg|
|/home/grep/src/B0001/IMG_000002photo2.jpg|

## Sol. 0
```sql
-- CONCAT함수를 활용해 출력할 문구 작성
SELECT CONCAT('/home/grep/src/', A.BOARD_ID, '/', B.FILE_ID, B.FILE_NAME, B.FILE_EXT) AS FILE_PATH
FROM USED_GOODS_BOARD A
JOIN USED_GOODS_FILE B ON A.BOARD_ID = B.BOARD_ID
-- 가장 많은 VIEWS를 가진 게시물의 ROW를 가져오기
WHERE A.VIEWS IN (SELECT MAX(VIEWS) AS VIEWS
                    FROM USED_GOODS_BOARD)
ORDER BY B.FILE_ID DESC;
```