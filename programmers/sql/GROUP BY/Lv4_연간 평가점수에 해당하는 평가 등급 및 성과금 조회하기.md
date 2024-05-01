# 연간 평가점수에 해당하는 평가 등급 및 성과금 조회하기
https://school.programmers.co.kr/learn/courses/30/lessons/284528

## 문제
`HR_EMPLOYEES`, `HR_GRADE` 테이블을 이용해 사원별 성과금 정보를 조회하려합니다. 평가 점수별 등급과 등급에 따른 성과금 정보가 아래와 같을 때, 사번, 성명, 평가 등급, 성과금을 조회하는 SQL문을 작성해주세요.

평가등급의 컬럼명은 `GRADE`로, 성과금의 컬럼명은 `BONUS`로 해주세요.
결과는 사번 기준으로 오름차순 정렬해주세요.

| 기준 점수 | 평가 등급 | 성과금(연봉 기준) |
|----------|----------|---------------|
| 96 이상   | S        | 20%           |
| 90 이상   | A        | 15%           |
| 80 이상   | B        | 10%           |
| 이외      | C        | 0%            |

**예시**    
`HR_EMPLOYEES` 테이블이 다음과 같고

| EMP_NO   | EMP_NAME | DEPT_ID | POSITION | EMAIL                   | COMP_TEL     | HIRE_DATE  | SAL       |
|----------|----------|---------|----------|-------------------------|--------------|------------|-----------|
| 2017002  | 정호식   | D0001   | 팀장     | hosick_jung@grep.com    | 031-8000-1101| 2017-03-01 | 65000000  |
| 2018001  | 김민석   | D0001   | 팀원     | minseock_kim@grep.com   | 031-8000-1102| 2018-03-01 | 60000000  |
| 2019001  | 김솜이   | D0002   | 팀장     | somi_kim@grep.com       | 031-8000-1106| 2019-03-01 | 60000000  |
| 2020002  | 김연주   | D0002   | 팀원     | yeonjoo_kim@grep.com    | 031-8000-1107| 2020-03-01 | 53000000  |
| 2020005  | 양성태   | D0003   | 팀원     | sungtae_yang@grep.com   | 031-8000-1112| 2020-03-01 | 53000000  |

`HR_GRADE` 테이블이 다음과 같을 때

| EMP_NO   | YEAR | HALF_YEAR | SCORE |
|----------|------|-----------|-------|
| 2017002  | 2022 | 1         | 92    |
| 2018001  | 2022 | 1         | 89    |
| 2019001  | 2022 | 1         | 94    |
| 2020002  | 2022 | 1         | 90    |
| 2020005  | 2022 | 1         | 92    |
| 2017002  | 2022 | 2         | 84    |
| 2018001  | 2022 | 2         | 89    |
| 2019001  | 2022 | 2         | 81    |
| 2020002  | 2022 | 2         | 91    |
| 2020005  | 2022 | 2         | 81    |

다음과 같이 사원별 성과금 정보를 출력해야 합니다.

| EMP_NO   | EMP_NAME | GRADE | BONUS  |
|----------|----------|-------|--------|
| 2017002  | 정호식   | B     | 6500000|
| 2018001  | 김민석   | B     | 6000000|
| 2019001  | 김솜이   | B     | 6000000|
| 2020002  | 김연주   | A     | 7950000|
| 2020005  | 양성태   | B     | 5300000|

## Sol. 0
```sql
-- 서브쿼리 테이블과 비교해 등급별 SAL에 대한 BONUS 계산하기
SELECT B.EMP_NO, A.EMP_NAME, B.GRADE,
            CASE WHEN B.GRADE = 'S' THEN A.SAL * 0.2
                WHEN B.GRADE = 'A' THEN A.SAL * 0.15
                WHEN B.GRADE = 'B' THEN A.SAL * 0.1
            ELSE A.SAL * 0
            END AS BONUS
FROM HR_EMPLOYEES A
-- 서브쿼리로 평균 점수별로 등급을 나눈 테이블을 생성 후 조인시키기
JOIN (SELECT EMP_NO,
        CASE WHEN AVG(SCORE) >= 96 THEN 'S'
            WHEN AVG(SCORE) >= 90 THEN 'A'
            WHEN AVG(SCORE) >= 80 THEN 'B'
        ELSE 'C'
        END AS GRADE
FROM HR_GRADE
GROUP BY EMP_NO) B ON A.EMP_NO = B.EMP_NO
ORDER BY B.EMP_NO;
```