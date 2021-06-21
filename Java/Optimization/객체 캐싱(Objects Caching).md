# 객체 캐싱(Objects Caching)

## Integer 클래스에서 사용하는 캐싱

```java
public final class Integer extends Number implements Comparable<Integer> {

// 생략

private static class IntegerCache {
        static final int low = -128;
        static final int high;
        static final Integer cache[];

        static {
            // high value may be configured by property
            int h = 127;
            String integerCacheHighPropValue =
                sun.misc.VM.getSavedProperty("java.lang.Integer.IntegerCache.high");
            if (integerCacheHighPropValue != null) {
                try {
                    int i = parseInt(integerCacheHighPropValue);
                    i = Math.max(i, 127);
                    // Maximum array size is Integer.MAX_VALUE
                    h = Math.min(i, Integer.MAX_VALUE - (-low) -1);
                } catch( NumberFormatException nfe) {
                    // If the property cannot be parsed into an int, ignore it.
                }
            }
            high = h;

            cache = new Integer[(high - low) + 1];
            int j = low;
            for(int k = 0; k < cache.length; k++)
                cache[k] = new Integer(j++);

            // range [-128, 127] must be interned (JLS7 5.1.7)
            assert IntegerCache.high >= 127;
        }

        private IntegerCache() {}
    }
    
    public static Integer valueOf(int i) {
        if (i >= IntegerCache.low && i <= IntegerCache.high)
            return IntegerCache.cache[i + (-IntegerCache.low)];
        return new Integer(i);
    }

}
```

IntegerCache 클래스의 코드를 보면 low ~ high(기본값은 -128~127) 사이의 Integer 인스턴스를 미리 생성하여 cache 배열에 저장하는 것을 볼 수 있다.

valueOf 메서드의 경우 IntegerCache 의 low 와 high 사이의 값일 경우 IntegerCache 의 cache 에 저장된 값을 반환하고 그 외의 경우 새로 Integer 인스턴스를 생성하여 반환한다.
즉 캐싱해둔 범위 내의 숫자일 경우 캐싱한 인스턴스를 반환하고 범위 밖의 수일 경우 새로 인스턴스를 생성하여 반환하는 방식이 Integer 의 캐싱이다.

## 로또

```java
public class LottoNumber {
    public static final int LOTTO_NUMBER_LOWER_BOUND = 1;
    public static final int LOTTO_NUMBER_UPPER_BOUND = 45;
    private static final List<LottoNumber> CACHE = new ArrayList<>();

    static {
        for (int i = LOTTO_NUMBER_LOWER_BOUND; i <= LOTTO_NUMBER_UPPER_BOUND; i++) {
            CACHE.add(new LottoNumber(i));
        }
    }

    private final int number;

    public LottoNumber(final int number) {
        validate(number);
        this.number = number;
    }

    public static LottoNumber valueOf(final int number) {
        LottoNumber lottoNumber = CACHE.get(number);

        if (Objects.isNull(lottoNumber)) {
            lottoNumber = new LottoNumber(number);
        }
        return lottoNumber;
    }

    private void validate(final int number) {
        if (number < LOTTO_NUMBER_LOWER_BOUND || number > LOTTO_NUMBER_UPPER_BOUND) {
            throw new IllegalArgumentException("LottoNumber가 유효하지 않습니다.");
        }
    }
}
```
```java
public class LottoNumberGenerator {
    private static final int VALID_SIZE = 6;

    public static List<LottoNumber> generate() {
        List<LottoNumber> lottoNumbers = new ArrayList<>(LottoNumber.values());
        Collections.shuffle(lottoNumbers);

        return lottoNumbers.subList(0, VALID_SIZE);
    }
}
```

## References

> https://woowacourse.github.io/javable/post/2020-06-24-caching-instance/
