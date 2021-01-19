# LocalDate

- 현재 날짜 구하기
  - ISO_DATE_TIME :	2019-03-22T23:56:36.4
  - ISO_LOCAL_DATE : 2019-03-22
  - ISO_LOCAL_TIME : 23:56:36.4
  - ISO_LOCAL_DATE_TIME : 2019-03-22T23:56:36.4
  - ISO_DATE : 2019-03-22
  - ISO_TIME : 23:56:36.4

```java
public static String getToday() {
        return LocalDate.now().format(DateTimeFormatter.ISO_DATE);
}
```
