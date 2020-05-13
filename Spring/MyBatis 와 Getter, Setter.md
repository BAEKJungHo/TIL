# MyBatis 와 Getter, Setter

MyBatis 사용할 때, select 혹은 insert 의 selectKey 로 seq 증가시키는 구문 등을 사용할 때, 내부적으로 자바의 getter 와 setter 를 이용한다.


예를 들어 PrivacyVo 에 있는 주민등록번호(RRN) 가 암호화된 값으로 `xxxxx` 로 저장되었다고 할 때, 만약 getter 가 아래와 같은경우

```java
public String getRrn() {
  this.rrn = this.생년월일 + this.주민등록번호뒷자리;
  return this.rrn;
}
```

- Example

생년월일과 주민등록번호 뒷자리가 암호화 되었다고 하더라고 DB 에서 조회할 때는 원하는 값이 안나올 것이다. 왜냐하면 DB 에는 주민등록번호 앞과 뒤를 합쳐서
암호화된 값을 저장하고 있기 때문이다. 

`따라서, getter, setter 들은 따로 변조하지 않는게 좋고, 위와 같이 특별한 경우엔 메서드를 따로 만들어 처리하는게 좋다.`
