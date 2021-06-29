# AutoIncrement 후 Insert 된 키값 받아오기

`useGeneratedKeys="true"` 설정을 한 다음 `keyProperty` 에 엔티티의 `pk 속성명`을 적으면 된다.

```xml
<insert id="saveBoard" parameterType="Board" useGeneratedKeys="true" keyProperty="id">
        insert into board (
            title,
            content
        ) values(
            #{title},
            #{content}
        )
    </insert>
```


## References

> https://bamdule.tistory.com/159
