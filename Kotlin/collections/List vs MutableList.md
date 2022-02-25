# List vs MutableList

- __List__
  - https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-list/#kotlin.collections.List
  - Methods in this interface support only `read-only` access to the list
- __MutableList__
  - https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-mutable-list/#kotlin.collections.MutableList
  - supports `adding and removing` elements 
  - val 을 써도 MutableList 의 add remove 기능을 쓸 수 있다. 단, 다른 MutableList 재할당은 불가능하다.
