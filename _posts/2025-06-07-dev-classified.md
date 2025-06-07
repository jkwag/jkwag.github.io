---
title: "개발자 분류"
date: 2025-06-07
categories: [sql_coding]
tags: []
toc: true
toc_label: "Table of contents"
toc_icon: "cog"
---

## 문제


SKILLCODES 테이블은 개발자들이 사용하는 프로그래밍 언어에 대한 정보를 담은 테이블입니다. SKILLCODES 테이블의 구조는 다음과 같으며, NAME, CATEGORY, CODE는 각각 스킬의 이름, 스킬의 범주, 스킬의 코드를 의미합니다. 스킬의 코드는 2진수로 표현했을 때 각 bit로 구분될 수 있도록 2의 제곱수로 구성되어 있습니다.

| 컬럼명    | 타입         | UNIQUE | NULLABLE |
| --------- | ------------ | :----: | :------: |
| NAME      | VARCHAR(N)   |   Y    |    N     |
| CATEGORY  | VARCHAR(N)   |   N    |    N     |
| CODE      | INTEGER      |   Y    |    N     |

DEVELOPERS 테이블은 개발자들의 프로그래밍 스킬 정보를 담은 테이블입니다. DEVELOPERS 테이블의 구조는 다음과 같으며, ID, FIRST_NAME, LAST_NAME, EMAIL, SKILL_CODE는 각각 개발자의 ID, 이름, 성, 이메일, 스킬 코드를 의미합니다. SKILL_CODE 컬럼은 INTEGER 타입이고, 2진수로 표현했을 때 각 bit는 SKILLCODES 테이블의 코드를 의미합니다.

| 컬럼명     | 타입         | UNIQUE | NULLABLE |
| ---------- | ------------ | :----: | :------: |
| ID         | VARCHAR(N)   |   Y    |    N     |
| FIRST_NAME | VARCHAR(N)   |   N    |    Y     |
| LAST_NAME  | VARCHAR(N)   |   N    |    Y     |
| EMAIL      | VARCHAR(N)   |   Y    |    N     |
| SKILL_CODE | INTEGER      |   N    |    N     |


예를 들어 어떤 개발자의 SKILL_CODE가 400 (=b'110010000')이라면, 이는 SKILLCODES 테이블에서 CODE가 256 (=b'100000000'), 128 (=b'10000000'), 16 (=b'10000') 에 해당하는 스킬을 가졌다는 것을 의미합니다.

## 내 답안

~~~sql
-- 코드를 작성해주세요
  WITH  cte AS(
    SELECT  a.*
            ,b.*
      FROM  DEVELOPERS a
      JOIN  (
    SELECT  NAME
            ,CATEGORY
            ,BIN(CODE) AS binary_code
      FROM  SKILLCODES)b
        ON  SUBSTRING(BIN(a.SKILL_CODE),-LENGTH(b.binary_code),1) = 1
            
  )
SELECT  'A' as GRADE
        ,ID
        ,EMAIL
  FROM  cte
 WHERE  ID IN (SELECT DISTINCT ID FROM cte WHERE CATEGORY = 'Front End') and ID IN (SELECT DISTINCT ID FROM cte WHERE NAME = 'Python')
 
 UNION
     
SELECT  'B' as GRADE
        ,ID
        ,EMAIL
  FROM  cte
  WHERE  ID IN (SELECT DISTINCT ID FROM cte WHERE NAME = 'C#')
  UNION
      
SELECT  'C' as GRADE
        ,ID
        ,EMAIL
  FROM  cte
  
  WHERE  ID NOT IN (SELECT DISTINCT ID FROM cte WHERE NAME = 'Python') and ID IN (SELECT DISTINCT ID FROM cte WHERE CATEGORY = 'Front End');
~~~

내가 원래 생각했던 풀이는 substring 함수를 이용해서 비트 단위로 바꾼 code를 skill 코드와 비교하여 개발자들마다 사용할 수 있는 언어를 리스트화 시키는데는 성공했다.

그러나, cte이후 본 단계에서 A, B, C 로 나누는 기준이 상호독립적이지 않기 때문에 예제 코드를 실행했을 떄 A에 있는 ID가 C에도 포함되는 불상사가 발생되었다.

## 정답

~~~sql
SELECT
  CASE
    -- A: Python + Front End
    WHEN SUM(CASE WHEN s.NAME     = 'Python'     THEN 1 ELSE 0 END) >= 1
     AND SUM(CASE WHEN s.CATEGORY = 'Front End' THEN 1 ELSE 0 END) >= 1
    THEN 'A'
    -- B: C#
    WHEN SUM(CASE WHEN s.NAME     = 'C#'         THEN 1 ELSE 0 END) >= 1
    THEN 'B'
    -- C: Front End (A, B 모두 아닌 Front End 보유자)
    WHEN SUM(CASE WHEN s.CATEGORY = 'Front End' THEN 1 ELSE 0 END) >= 1
    THEN 'C'
    ELSE NULL
  END AS GRADE,
  d.ID,
  d.EMAIL
FROM DEVELOPERS AS d
  -- SKILL_CODE 비트마스크와 SKILLCODES.CODE 간 비트 AND 로 “보유 여부”를 판별
  JOIN SKILLCODES AS s
    ON (d.SKILL_CODE & s.CODE) = s.CODE
GROUP BY
  d.ID,
  d.EMAIL
HAVING
     -- A, B, C 중 하나에 걸려야만 결과에 나옴
     (
        SUM(CASE WHEN s.NAME     = 'Python'     THEN 1 ELSE 0 END) >= 1
      AND SUM(CASE WHEN s.CATEGORY = 'Front End' THEN 1 ELSE 0 END) >= 1
     )
  OR ( SUM(CASE WHEN s.NAME     = 'C#'         THEN 1 ELSE 0 END) >= 1 )
  OR ( SUM(CASE WHEN s.CATEGORY = 'Front End' THEN 1 ELSE 0 END) >= 1 )
ORDER BY
  GRADE ASC,
  d.ID   ASC;
~~~

정답은 다음과 같다. 먼저, 달라진 점은 (d.SKILL_CODE & s.CODE) = s.CODE와 같은 비트 AND 방식으로 보유 여부를 판별하는 방식을 사용한다.
또한, ID와 EMAIL로 Group BY를 시도하면서 CASE 구문을 적극적으로 활용하는데 SUM(CASE WHEN) >= 1 과 같은 방식으로 내가 가지고 있었던 고민인
숫자가 아닌 유형으로 나누려 할 떄 Group By를 어떻게 활용할 수 있을까? 라는 의문을 깔끔하게 해결한다. HAVING절은 A,B,C 기준에 충족하지 못하는 유저들을 
거르는 필터망 역할을 한다
