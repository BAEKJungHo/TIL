# Version History

## Kotlin 1.6.0

### 코틀린 그래들 플러그인

Kotlin 1.6.0에서는 `KotlinGradleSubplugin` 클래스의 deprecation 수준을 `ERROR` 로 변경했습니다.
이 클래스는 컴파일러 플러그인을 작성하는 데 사용되었습니다. 
다음 릴리스에서는 이 클래스를 제거합니다. 
대신 클래스 를 사용하십시오 `KotlinCompilerPluginSupportPlugin.`

kotlin.useFallbackCompilerSearch 빌드 옵션 noReflect 과 includeRuntime 컴파일러 옵션을 제거했습니다 . 컴파일러 옵션이 useIR 숨겨져 있으며 향후 릴리스에서 제거될 예정입니다.
