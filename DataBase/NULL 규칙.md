# Null 규칙

## 비교 조건

- 첫 번째 쿼리

```
SELECT APPLIER_NAME, DISCRHASH, COMMENTATOR_SEQ
 FROM COMMENTATOR
WHERE 1=1 AND TOURISM_YEAR = '2021'
  AND TOURISM_MONTH = '9'
  AND TOURISM_DAY = '2'
  AND TOURISM_TIME = '3'
  AND RESERVATION_SEQ = 6
  AND DEL_STS = 'N'
``` 

쿼리결과가 다음과 같다고 하겠다.
 
`DISCRHASH : '123', NULL, 'ABC'`

- 두 번째 쿼리

```sql
SELECT APPLIER_NAME, DISCRHASH, COMMENTATOR_SEQ
 FROM COMMENTATOR
WHERE 1=1 AND TOURISM_YEAR = '2021'
  AND TOURISM_MONTH = '9'
  AND TOURISM_DAY = '2'
  AND TOURISM_TIME = '3'
  AND RESERVATION_SEQ = 6
  AND DEL_STS = 'N'
  AND DISCRHASH NOT IN '123'
```

두 번째 쿼리의 결과는 DISCRHASH 값이 123 과 NULL 이 아닌 애들만 출력된다.

그 이유는 다음과 같다.

- 보통 NULL값은 무시된다.
- FALSE 가 아니라 무시된다.
- NULL 조건은 IS NULL, IS NOT NULL로 처리해야된다.
- `=` 이나 `!=` `<>` 에 NULL을 붙이면 항상 `FALSE` 가 나온다.
  - Ex. DISCRHASH = NULL ... FALSE
- `모든 DB 벤더에 상관없이 동일하게 적용 된다.`

NULL 을 포함된 결과를 출력하고 싶으면 다음과 같이 해야한다.

- 세 번째 쿼리

```sql
SELECT APPLIER_NAME, DISCRHASH, COMMENTATOR_SEQ
 FROM COMMENTATOR
WHERE 1=1 AND TOURISM_YEAR = '2021'
  AND TOURISM_MONTH = '9'
  AND TOURISM_DAY = '2'
  AND TOURISM_TIME = '3'
  AND RESERVATION_SEQ = 6
  AND DEL_STS = 'N'
  AND (DISCRHASH NOT IN '123' OR DISCRHASH IS NULL)
```

__쿼리 WHERE 조건에 NOT 연산(NOT IN, `<>`, `!=`)이 들어가는 경우 그 컬럼이 NOT NULL 제약 조건이 걸려있는지 꼭 확인해야 한다. 만약, NULL 허용 컬럼이라면 NULL 결과를 포함 시킬껀지, 아닌지의 여부에 따라 조건 쿼리가 달라진다.__
