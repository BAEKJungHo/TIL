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

- 날짜 더하고 빼기

```java
LocalDateTime now = LocalDateTime.now(); // 현재시간
LocalDateTime threeYearsAfter = now.plusYears(3); // 3년 뒤 - now는 계속 현재시간
LocalDateTime twoDaysAgo = now.minusDays(2); // 2일 전
LocalDateTime twoDaysAndThreeHoursAgo = now.minusDays(2).minusHours(3); // 2일 3시간 전
```

```java
/**
 * 현재 날짜에서 n 개월 뒤
 * @return
 */
public static String getAfterMonth(int month) {
    return LocalDate.now().plusMonths(month).format(DateTimeFormatter.ISO_DATE);
}

/**
 * 현재 날짜에서 n 년 뒤
 * @return
 */
public static String getAfterYear(int year) {
    return LocalDate.now().plusYears(year).format(DateTimeFormatter.ISO_DATE);
}
```
