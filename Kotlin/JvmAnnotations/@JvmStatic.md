# @JvmStatic

- Java 에서 Kotlin 으로 작성된 함수와 프로퍼티에 static 하게 접근할 수 있도록 추가적인 메서드 또는 getter / setter 를 생성한다.
- Kotlin 만 사용한다면 필요 없다.

```kotlin
class DistancePolicy private constructor(private val distance: Int) : FarePolicy {
    override fun calculate(fare: Fare) {
        // Kotlin 내부에서 Static 함수에 접근
        fare.add(Policy.getOverFare(distance))
    }

    private enum class Policy(private val predicate: Predicate<Int>, private val criteria: Int) {
        STANDARD(
            Predicate { distance: Int -> distance <= 10 },
            0
        ),
        NORMAL(
            Predicate { distance: Int -> distance in 11..50 },
            5
        ),
        LONGEST(
            Predicate { distance: Int -> distance > 50 },
            8
        );

        private fun match(distance: Int): Boolean {
            return predicate.test(distance)
        }

        companion object {
            private const val NOT_EXISTS_OVER_FARE = 0

            fun getOverFare(distance: Int): Int {
                val policy = findType(distance)
                return if (isStandard(distance)) NOT_EXISTS_OVER_FARE else calculateOverFare(distance, policy.criteria)
            }

            private fun isStandard(distance: Int): Boolean {
                return findType(distance) == STANDARD
            }

            private fun calculateOverFare(distance: Int, criteria: Int): Int {
                return ((ceil(((distance - 11) / criteria).toDouble()) + 1) * 100).toInt()
            }

            private fun findType(distance: Int): Policy {
                return Arrays.stream(values())
                    .filter { policy: Policy -> policy.match(distance) }
                    .findFirst()
                    .orElse(STANDARD)
            }
        }
    }

    companion object {
        @JvmStatic
        fun from(distance: Int): DistancePolicy {
            return DistancePolicy(distance)
        }
    }
}
```

```java
public class FareCalculator {

    private final List<FarePolicy> farePolicies;

    public FareCalculator(final Path path, final int age) {
        farePolicies = createFarePolicies(path, age);
    }

    private List<FarePolicy> createFarePolicies(final Path path, final int age) {
        return Arrays.asList(
                // 자바에서 Kotlin 의 Static 함수에 접근
                DistancePolicy.from(path.extractDistance()),
                LinePolicy.from(path.extractExpensiveExtraCharge()),
                AgePolicy.from(age));
    }
    
    ...
}
```
