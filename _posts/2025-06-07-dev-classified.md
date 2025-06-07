---
title: "개발자 분류"
date: 2025-06-07
categories: [sql_coding]
tags: []
toc: true
toc_label: "Table of contents"
toc_icon: "cog"
---

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
