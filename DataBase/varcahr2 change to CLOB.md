## varchar2 change to CLOB

```sql
-- CLOB 임시 컬럼 추가
ALTER TABLE 테이블명 ADD(NEW 컬럼명 CLOB);

-- 임시컬럼에 있던 값을 CLOB 타입으로 복사
UPDATE 테이블명 SET NEW 컬럼명 = 기존 컬럼명;

-- 기존 컬럼 삭제
ALTER 테이블명 DROP COLUMN 기존 컬럼명;

-- 임시컬럼명을 기존 컬럼명으로 변경
ALTER 테이블명 RENAME COLUMN NEW 컬럼명 TO 기존 컬럼명;
```
