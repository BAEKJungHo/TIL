# ConversionService

코틀린을 사용하다가 dto 에 type 이라는 enum 변수를 만들어두고, payload 에 담긴 String 타입의 문자열을 enum 으로 변환하는 작업에서 버그가 발생했다.

코틀린은 Decompile 시 `Elvis Operator` 를 사용하지 않으면 기본으로 `@NotNull` 이 붙는다.

따라서, 변환 과정에서 `ConversionFailedException` 를 기대했지만, NullPointerException 이 발생하는 것이었다.

## Code

```kotlin
enum class DeliveryType {
    IMMEDIATELY,
    BOOKING,
    ;
}

class StringToDeliveryTypeConverter: Converter<String, DeliveryType> {
    override fun convert(source: String): PickupType {
        val valid = DeliveryType.values().filter { it.name == source }.toList().isNotEmpty()
        if (!valid) throw NotFoundDeliveryTypeException()
        return DeliveryType.valueOf(source)
    }
}
```

```kotlin
data class FindProductsRequest(
    @DateTimeFormat(pattern = "yyyy-MM-dd'T'HH:mm:ss") val receiveTime: LocalDateTime?,
    val deliveryType: DeliveryType
)
```

```kotlin
// Controller 
fun find(
    @ModelAttribute request: FindProductsRequest,
    bindingResult: BindingResult
) { ... }
```

## Debugging

- Request 요청
- CustomConverter 동작
- ConversionUitls 동작
  - ```kotlin
    abstract class ConversionUtils {
      @Nullable
      public static Object invokeConverter(GenericConverter converter, @Nullable Object source,
          TypeDescriptor sourceType, TypeDescriptor targetType) {

        try {
          return converter.convert(source, sourceType, targetType);
        }
        catch (ConversionFailedException ex) {
          throw ex;
        }
        catch (Throwable ex) {
          throw new ConversionFailedException(sourceType, targetType, source, ex);
        }
      }

      ...
    }
    ```
- TypeConverterDelegate 동작
  - ```kotlin
      if (convertedValue == currentConvertedValue) {
			try {
        // 이 부분에서 예외 발생 > 내부적으로 리플렉션을 사용하여 컨버팅 함
				Field enumField = requiredType.getField(trimmedValue);
				ReflectionUtils.makeAccessible(enumField);
				convertedValue = enumField.get(null);
			}
			catch (Throwable ex) {
				if (logger.isTraceEnabled()) {
					logger.trace("Field [" + convertedValue + "] isn't an enum value", ex);
				}
			}
		}
    ```
