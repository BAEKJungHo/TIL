# RegularExpression

```java
public interface RegularExpression {

    Pattern YYYY_MM_DD = Pattern.compile("^\\d{4}-(0[1-9]|1[012])-(0[1-9]|[12][0-9]|3[0-1])$");
    Pattern PASSWORD = Pattern.compile("^(?=.*?[a-zA-Z])(?=.*?[0-9])(?!.*?[\\&><'])(?!.*(%2F))(?=.*?[#?!@$%^&*-]).{9,}$");
    Pattern PHONE = Pattern.compile("^\\d{2,3}-\\d{3,4}-\\d{4}$");
    Pattern EMAIL = Pattern.compile("^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,6}$");
    Pattern BIRTHDAY = Pattern.compile("^(19[0-9][0-9]|20\\d{2})(0[0-9]|1[0-2])(0[1-9]|[1-2][0-9]|3[0-1])$");
    Pattern ALPHABET = Pattern.compile("[a-zA-Z]+");
    Pattern NUMBER = Pattern.compile("[0-9]+");

}
```   
