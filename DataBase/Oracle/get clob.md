# get clob

클롭 데이터 가져오기 (전체 레코드)

```sql
SELECT
			DBMS_LOB.SUBSTR(A.CONTENT, DBMS_LOB.GETLENGTH(A.CONTENT), 1) AS CONTENT
FROM 
      ARTICLE A
```
