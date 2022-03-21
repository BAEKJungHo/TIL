# Mongoose

Mongoose 는 Node.js 와 MongoDB 를 위한 `ODM(Object Data Mapping)` 라이브러리이다. 즉, 객체와 문서를 매핑해준다.

> JPA 는 ORM 으로서, 테이블과 객체를 매핑해준다.

- 필요에 따라 확장 및 변경이 가능한 자체 검증과 타입변환이 가능하며, express 와 함께 사용하면 MVC 패턴 구현이 용이하다는 장점이 있다.

## ODM(Object Data Mapping)

- ODM(Object Data Mapping) 은 말 그대로 객체와 문서를 1대1 매칭한다는 뜻이다.
- Object 는 자바스크립트의 객체이고, Document 는 몽고DB 의 문서이다.
- 즉, 문서를 DB 에서 조회할 때 자바스크립트 객체로 바꿔주는 역할이라고 생각하면 된다.
