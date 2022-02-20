# @CsvSource

```java
class FareTest {

    @CsvSource({"10, 1250", "16, 1450", "59, 1950"})
    @ParameterizedTest
    void 요금_계산(int distance, int expectedFare) {
        Fare fare = new Fare(distance);
        assertThat(fare.findCost()).isEqualTo(expectedFare);
    }
}
```
