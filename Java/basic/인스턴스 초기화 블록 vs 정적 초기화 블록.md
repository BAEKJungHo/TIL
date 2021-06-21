# 인스턴스 초기화 블록

인스턴스 초기화 블록은 객체를 생성할때 생성자가 호출되기 전에 호출된다. 

```java
public class GFG
{
    // Initializer block starts..
    {
        // This code is executed before every constructor.
        System.out.println("Common part of constructors invoked !!");
    }
    // Initializer block ends
 
    public GFG()
    {
        System.out.println("Default Constructor invoked");
    }
    public GFG(int x)
    {
        System.out.println("Parameterized constructor invoked");
    }
    public static void main(String arr[])
    {
        GFG obj1, obj2;
        obj1 = new GFG();
        obj2 = new GFG(0);
    }
}
```

# 정적 초기화 블록

```java
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
 ```

- 클래스가 로딩될 때 호출되며 따라서 각 클래스당 최초 1회만 실행된다. 즉, 정적 초기화 블록은 단 한번만 호출된다. 
- 스태틱 블럭은 객체가 생성되기 이전에 수행되므로 인스턴스 멤버에 접근할 수 없다.
- 클래스가 로딩 될 때 복잡한 초기화 과정을 수행해야 할 때 사용할 수 있다.

