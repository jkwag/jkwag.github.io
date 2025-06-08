---
title: "특정 기간동안 대여 가능한 자동차 대여 비용 구하기"
date: 2025-06-08
categories: [sql_coding]
tags: []
toc: true
toc_label: "Table of contents"
toc_icon: "cog"
---

## 문제

다음은 어느 자동차 대여 회사에서 대여 중인 자동차들의 정보를 담은 CAR_RENTAL_COMPANY_CAR 테이블과 자동차 대여 기록 정보를 담은 CAR_RENTAL_COMPANY_RENTAL_HISTORY 테이블과 자동차 종류 별 대여 기간 종류 별 할인 정책 정보를 담은 CAR_RENTAL_COMPANY_DISCOUNT_PLAN 테이블 입니다.
CAR_RENTAL_COMPANY_CAR 테이블은 아래와 같은 구조로 되어있으며, CAR_ID, CAR_TYPE, DAILY_FEE, OPTIONS 는 각각 자동차 ID, 자동차 종류, 일일 대여 요금(원), 자동차 옵션 리스트를 나타냅니다.

| Column name | Type           | Nullable |
|-------------|----------------|----------|
| CAR_ID      | INTEGER        | FALSE    |
| CAR_TYPE    | VARCHAR(255)   | FALSE    |
| DAILY_FEE   | INTEGER        | FALSE    |
| OPTIONS     | VARCHAR(255)   | FALSE    |


자동차 종류는 '세단', 'SUV', '승합차', '트럭', '리무진' 이 있습니다. 자동차 옵션 리스트는 콤마(',')로 구분된 키워드 리스트(예: ''열선시트,스마트키,주차감지센서'')로 되어있으며, 키워드 종류는 '주차감지센서', '스마트키', '네비게이션', '통풍시트', '열선시트', '후방카메라', '가죽시트' 가 있습니다.
CAR_RENTAL_COMPANY_RENTAL_HISTORY 테이블은 아래와 같은 구조로 되어있으며, HISTORY_ID, CAR_ID, START_DATE, END_DATE 는 각각 자동차 대여 기록 ID, 자동차 ID, 대여 시작일, 대여 종료일을 나타냅니다.

| Column name | Type    | Nullable |
|-------------|---------|----------|
| HISTORY_ID  | INTEGER | FALSE    |
| CAR_ID      | INTEGER | FALSE    |
| START_DATE  | DATE    | FALSE    |
| END_DATE    | DATE    | FALSE    |

CAR_RENTAL_COMPANY_DISCOUNT_PLAN 테이블은 아래와 같은 구조로 되어있으며, PLAN_ID, CAR_TYPE, DURATION_TYPE, DISCOUNT_RATE 는 각각 요금 할인 정책 ID, 자동차 종류, 대여 기간 종류, 할인율(%)을 나타냅니다.

| Column name    | Type           | Nullable |
|----------------|----------------|----------|
| PLAN_ID        | INTEGER        | FALSE    |
| CAR_TYPE       | VARCHAR(255)   | FALSE    |
| DURATION_TYPE  | VARCHAR(255)   | FALSE    |
| DISCOUNT_RATE  | INTEGER        | FALSE    |

할인율이 적용되는 대여 기간 종류로는 '7일 이상' (대여 기간이 7일 이상 30일 미만인 경우), '30일 이상' (대여 기간이 30일 이상 90일 미만인 경우), '90일 이상' (대여 기간이 90일 이상인 경우) 이 있습니다. 대여 기간이 7일 미만인 경우 할인정책이 없습니다.

문제
CAR_RENTAL_COMPANY_CAR 테이블과 CAR_RENTAL_COMPANY_RENTAL_HISTORY 테이블과 CAR_RENTAL_COMPANY_DISCOUNT_PLAN 테이블에서 자동차 종류가 '세단' 또는 'SUV' 인 자동차 중 2022년 11월 1일부터 2022년 11월 30일까지 대여 가능하고 30일간의 대여 금액이 50만원 이상 200만원 미만인 자동차에 대해서 자동차 ID, 자동차 종류, 대여 금액(컬럼명: FEE) 리스트를 출력하는 SQL문을 작성해주세요. 결과는 대여 금액을 기준으로 내림차순 정렬하고, 대여 금액이 같은 경우 자동차 종류를 기준으로 오름차순 정렬, 자동차 종류까지 같은 경우 자동차 ID를 기준으로 내림차순 정렬해주세요.

## 내 답안

~~~sql
WITH cte as (SELECT  a.CAR_ID
        ,a.CAR_TYPE
        ,a.DAILY_FEE
  FROM  CAR_RENTAL_COMPANY_CAR a      
  JOIN  CAR_RENTAL_COMPANY_RENTAL_HISTORY b
    ON  a.CAR_ID = b.CAR_ID
 GROUP
    BY  a.CAR_ID
HAVING  a.CAR_TYPE IN ('SUV','세단') and SUM(CASE WHEN EXTRACT(YEAR_MONTH FROM b.START_DATE) = 202211 THEN 1 WHEN EXTRACT(YEAR_MONTH FROM b.END_DATE) = 202211 THEN 1 ELSE 0 END) = 0 AND a.DAILY_FEE*30 >= 500000 AND a.DAILY_FEE*30 <= 2000000)

SELECT  a.CAR_ID
        ,a.CAR_TYPE
        ,ROUND(a.DAILY_FEE*30 - a.DAILY_FEE*30*b.DISCOUNT_RATE/100,0) AS FEE
  FROM  cte a
  JOIN  (SELECT CAR_TYPE
                ,DISCOUNT_RATE
           FROM CAR_RENTAL_COMPANY_DISCOUNT_PLAN
          WHERE DURATION_TYPE = '30일 이상')b
    ON  a.CAR_TYPE = b.CAR_TYPE
 ORDER
    BY  3 DESC, 2 ASC, 1 DESC;
~~~

## 정답

~~~sql
SELECT
    c.CAR_ID,
    c.CAR_TYPE,
    -- 30일간 총요금 = daily_fee * 30 * (100 - discount_rate) / 100
    ROUND(c.DAILY_FEE * 30 * (100 - p.DISCOUNT_RATE) / 100) AS FEE
FROM CAR_RENTAL_COMPANY_CAR AS c
-- 30일 이상 대여 때의 할인율만 가져오기
JOIN CAR_RENTAL_COMPANY_DISCOUNT_PLAN AS p
  ON p.CAR_TYPE     = c.CAR_TYPE
 AND p.DURATION_TYPE = '30일 이상'
WHERE
    -- (1) 세단 또는 SUV
    c.CAR_TYPE IN ('세단', 'SUV')
    -- (2) 2022-11-01 ~ 2022-11-30 기간과 겹치는 대여 기록이 없어야 함
    AND NOT EXISTS (
      SELECT 1
      FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY AS r
      WHERE r.CAR_ID    = c.CAR_ID
        AND r.START_DATE <= DATE '2022-11-30'
        AND r.END_DATE   >= DATE '2022-11-01'
    )
    -- (3) 할인 적용 후 총요금이 500,000 이상, 2,000,000 미만
    AND ROUND(c.DAILY_FEE * 30 * (100 - p.DISCOUNT_RATE) / 100) BETWEEN 500000 AND 1999999
ORDER BY
    FEE DESC,
    c.CAR_TYPE ASC,
    c.CAR_ID DESC;
~~~
