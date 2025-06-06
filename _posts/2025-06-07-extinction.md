---
title: "멸종위기 대장균"
date: 2025-06-07
categories: [sql coding]
tags: []
toc: true
toc_label: "Table of contents"
toc_icon: "cog"
---

## 문제

문제 설명은 다음과 같다

문제 설명

대장균들은 일정 주기로 분화하며, 분화를 시작한 개체를 부모 개체, 분화가 되어 나온 개체를 자식 개체라고 합니다.
다음은 실험실에서 배양한 대장균들의 정보를 담은 ECOLI_DATA 테이블입니다. ECOLI_DATA 테이블의 구조는 다음과 같으며, ID, PARENT_ID, SIZE_OF_COLONY, DIFFERENTIATION_DATE, GENOTYPE 은 각각 대장균 개체의 ID, 부모 개체의 ID, 개체의 크기, 분화되어 나온 날짜, 개체의 형질을 나타냅니다.

| Column name          | Type    | Nullable |
|----------------------|---------|----------|
| ID                   | INTEGER | FALSE    |
| PARENT_ID            | INTEGER | TRUE     |
| SIZE_OF_COLONY       | INTEGER | FALSE    |
| DIFFERENTIATION_DATE | DATE    | FALSE    |
| GENOTYPE             | INTEGER | FALSE    |

최초의 대장균 개체의 PARENT_ID 는 NULL 값입니다.
문제
각 세대별 자식이 없는 개체의 수(COUNT)와 세대(GENERATION)를 출력하는 SQL문을 작성해주세요. 이때 결과는 세대에 대해 오름차순 정렬해주세요. 단, 모든 세대에는 자식이 없는 개체가 적어도 1개체는 존재합니다.

## 초기 시도

초기에는 Recursive 한 테이블에 대한 컨셉이 익숙치 않다보니 한 단계마다 서브쿼리로 만들어 그 단계에 있는 
ID를 추출해야 되나 라는 생각을 했지만 문제는 몇 세대에 다다러야 이를 멈추는지 제약 조건을 달 수 없다는 것이 한계로 다가왔다

## 정답

~~~sql
WITH RECURSIVE gens AS (
    -- 1세대: PARENT_ID가 NULL인 개체
    SELECT
        ID,
        1 AS generation
    FROM ECOLI_DATA
    WHERE PARENT_ID IS NULL

    UNION ALL

    -- 재귀: 부모세대(g)에서 ID를 꺼내서, 그 자식(e)의 세대를 “부모세대 + 1”로 설정
    SELECT
        e.ID,
        g.generation + 1
    FROM ECOLI_DATA e
    JOIN gens g
      ON e.PARENT_ID = g.ID
)
SELECT
    COUNT(*)        AS COUNT
    ,g.generation AS GENERATION
    
FROM gens g
-- “자식이 없는 개체”만 남기기: ECOLI_DATA에 자기 자신을 PARENT_ID로 갖는 행이 없는 경우
WHERE NOT EXISTS (
    SELECT 1
      FROM ECOLI_DATA e2
     WHERE e2.PARENT_ID = g.ID
)
GROUP BY
    g.generation
ORDER BY
    g.generation;
~~~

재귀 테이블 정의가 익숙치 않다 보니 챗지피티의 힘을 빌려 완성한 답변이다. 스텝마다 설명하자면
1단계는 먼저 재귀 테이블 gens 정의이다. Base 단계는 먼저 Parent_ID가 Null인 말 그대로
1세대인 ID를 ECOLI_DATA 테이블로부터 가지고 온다. 재귀 단계는 이후 gens 테이블에서 ID를 꺼내
이를 ECOLI_DATA 테이블에서 PARENT_ID와 대조하여 자식 세대 ID를 가져오고 generation을 기록한다.

이로써 재귀 테이블 gens가 완성됐고 이제 generation으로 group by 해서 수를 세면 되는데 아직 제약조건인
자식이 없는 개체만 남기기가 완성 되지 않았다. 자식이 없기 때문에 ECOLI_DATA에서 특정 ID를 갖는 행이 없는
경우의 ID를 추출하면 되므로 WHERE NOT EXISTS 구문으로 이를 추출해낸다.






