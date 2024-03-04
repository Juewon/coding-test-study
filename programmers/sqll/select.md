# 3월에 태어난 여성 회원 목록 출력하기
-- 코드를 입력하세요
SELECT MEMBER_ID, MEMBER_NAME, GENDER, DATE_FORMAT(DATE_OF_BIRTH, '%Y-%m-%d')
FROM MEMBER_PROFILE
WHERE MONTH(DATE_OF_BIRTH) = 3 AND TLNO IS NOT NULL AND GENDER = 'W'
ORDER BY MEMBER_ID ASC;

-- 생일이 3월인 여성 회원의 member_id, member_name, gender, date_of_birth를 작성
-- 전화번호가 null이면 제외
-- 회원 id를 기준 오름차순 정렬

# 인기있는 아이스크림
-- 코드를 입력하세요
SELECT FLAVOR
FROM FIRST_HALF
ORDER BY TOTAL_ORDER DESC, SHIPMENT_ID ASC;

-- 총주문량을 기준으로 내림차순 정렬, 총주문량이 같을 경우, 출하 번호를 기준으로 오름차순 정렬