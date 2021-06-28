# MySQL 에서 Sequence 흉내내기

```sql
-- 시퀀스 테이블 생성
create table SEQUENCES (
	NAME VARCHAR(32),
	CURRVAL bigint unsigned 
) 

-- 시퀀스를 생성 할 프로시저 생성
DELIMITER $$
	create procedure `create_sequence` (in THE_NAME TEXT)
	modifies sql data 
	deterministic 
	begin 
			delete from sequences where NAME = THE_NAME;
			insert into sequences VALUES(THE_NAME, 0);
	end
	
	
-- 생성한 시퀀스의 다음 값을 가져오는 함수 생성
DELIMITER $$
	create FUNCTION `NEXTVAL` (the_name VARCHAR(32))
	returns bigint unsigned
	modifies sql data
	deterministic
	begin
		declare RET bigint unsigned;
		update sequences set CURRVAL = CURRVAL + 1 where NAME = THE_NAME;
		select CURRVAL into RET from sequences where NAME = THE_NAME limit 1;
		return RET;
	end
	
	
-- 시퀀스 생성
call CREATE_SEQUENCE('FILE_SEQ');

-- 호출해서 사용
select user.NEXTVAL('FILE_SEQ') from dual;
```

## References 

> https://proudin.tistory.com/28
> 
> https://imover.tistory.com/124
