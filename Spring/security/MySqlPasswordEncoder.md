# MySqlPasswordEncoder

DB 패스워드 암호화를 MySQL 의 password() 함수를 사용하는 경우, 스프링 시큐리티를 사용할 때 `MySqlPasswordEncoder` 클래스를 만들어서 빈으로 등록해줘야 한다.

1. 스프링 시큐리티는 입력한 ID 를 통해서 DB 에서 정보를 조회해오고 (암호화된 패스워드가 vo 에 담겨있다.)
2. credenctials 에 들어있는 (자신이 입력한 패스워드, 암호화 되지 않은 패스워드) 패스워드와, 시큐리티의 PasswordEncoder 를 사용하여 암호화 한 다음 패스워드를 비교한다.

```java
public class MySqlPasswordEncoder implements PasswordEncoder {

    @Override
    public String encode(CharSequence rawPassword) {
        if (rawPassword == null) {
            throw new NullPointerException();
        }
        byte[] bpara = new byte[rawPassword.length()];
        byte[] rethash;
        int i;

        for (i = 0; i < rawPassword.length(); i++)
            bpara[i] = (byte) (rawPassword.charAt(i) & 0xff);

        try {
            MessageDigest sha1er = MessageDigest.getInstance("SHA1");
            rethash = sha1er.digest(bpara); // stage1
            rethash = sha1er.digest(rethash); // stage2
        } catch (GeneralSecurityException e) {
            throw new RuntimeException(e);
        }

        StringBuffer r = new StringBuffer(41);
        r.append("*");

        for (i = 0; i < rethash.length; i++) {
            String x = Integer.toHexString(rethash[i] & 0xff).toUpperCase();
            if (x.length() < 2) r.append("0"); r.append(x);
        }
        return r.toString();
    }

    @Override
    public boolean matches(CharSequence rawPassword, String encodedPassword) {
        if (encodedPassword == null || rawPassword == null) { return false; }
        if (!encodedPassword.equals(encode(rawPassword))) { return false; }
        return true;
    }

}
```

- 빈으로 등록(SecurityConfig)

```java
...
@Bean
public PasswordEncoder passwordEncoder() {
    return new MySqlPasswordEncoder();
}
...
```
