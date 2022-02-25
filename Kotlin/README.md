# Kotlin

> https://kotlinlang.org/docs/home.html

## Table of Contents

- [Version History](https://github.com/BAEKJungHo/TIL/blob/master/Kotlin/contents/Version%20History.md)
- [Packages](https://github.com/BAEKJungHo/TIL/blob/master/Kotlin/concepts/Packages.md)
- [Program Entry Point](https://github.com/BAEKJungHo/TIL/blob/master/Kotlin/concepts/Program%20entry%20point.md)
- [Print to the standard output](https://github.com/BAEKJungHo/TIL/blob/master/Kotlin/concepts/Print%20to%20the%20standard%20output.md)
- [Functions](https://github.com/BAEKJungHo/TIL/blob/master/Kotlin/concepts/Functions.md)
- [val vs var](https://github.com/BAEKJungHo/TIL/blob/master/Kotlin/concepts/val%2C%20var.md)
- [DTO(Data classes)](https://github.com/BAEKJungHo/TIL/blob/master/Kotlin/concepts/DTO(Data%20classes).md)
- [Classes](https://github.com/BAEKJungHo/TIL/blob/master/Kotlin/concepts/Classes.md)
- [Visibility Modifier](https://github.com/BAEKJungHo/TIL/blob/master/Kotlin/concepts/Visibility%20Modifier(Access%20Modifier).md)

## 코틀린이 추구하는 철학

- __Java 를 대체하고, 더 간결하고 더 생산적이며 안전한 언어를 제공하는 것__
- __Java 와의 상호운용성에 초점을 맞춘 언어__
- 어떻게 Java 와 100% 호환이 되는 것일까?
  - 자바는 컴파일러에 의해 Bytecode 가 생성된다.
  - 코틀린 또한 그렇다. Bytecode 가 생성되며, JVM 위에서 돌아갈 수 있다.

## 정적 타입 지정 언어

- 자바와 마찬가지로 코틀린도 `정적 타입(statically typed)` 지정 언어이다.
- 정적 타입 지정 언어는 모든 프로그램 구성 요소의 타입을 컴파일 시점에 알 수 있고, 프로그램 안에서 객체의 필드나 메서드를 사용할 때마다 컴파일러가 타입을 검증해준다는 뜻이다.
- 코틀린은 모든 변수의 타입을 프로그래머가 직접 명시할 필요가 없다.
  - `var` 키워드를 사용하여 변수를 나타내면 컴파일러가 문맥을 고려해 변수 타입을 결정한다.
  - 이것을 `타입 추론(Type Inference)` 이라고 한다.

### 장점

- __성능__
  - 실행 시점에 어떤 메서드를 호출할지 알아내는 과정이 필요 없으므로, 메서드 호출이 빠르다.
- __신뢰성__
  - 컴파일러가 프로그램의 정확성(correctness)을 검증하기 때문에 실행 시 프로그램이 오류로 중단될 가능성이 더 적어진다.
- __유지보수성__
  - 코드에서 다루는 객체가 어떤 타입에 속하는지 알 수 있기 때문에 처음 보는 코드를 다룰 때도 더 쉽다.
- __도구 지원__
  - 정적 타입 지정을 활용하면 더 안전하게 리팩토링할 수 있고, 도구는 더 정확한 코드 완성 기능을 제공할 수 있다.

## 코틀린이 자바와 다르게 프로그램의 신뢰성을 높이는 방법

- 코틀린은 `널이 될 수 있는 타입(nullable type)`을 지원한다.
- 따라서, NPE 가 발생할 수 있는 지를 컴파일타임에 검사할 수 있어서, 신뢰성을 높일 수 있다.

## 코틀린은 함수형 프로그래밍을 풍부하게 지원한다.

- 함수형 프로그래밍을 강제하진 않지만, 함수형 프로그래밍을 할 수 있도록 풍부하게 지원한다.
- __함수형 프로그래밍의 특징__
  - `일급 시민(First class) 함수`
    - 함수를 일반 값처럼 다룰 수 있다. 변수에 저장할 수 있고, 다른 함수의 인자로 전달할 수 있으며, 함수에서 새로운 함수를 만들어서 반환할 수 있다.
  - `불변성(immutablility)`
    - 일단 만들어지면 내부 상태가 변하지 않는다.
  - `부수 효과(side effect) 없음`
    - 함수형 프로그래밍에서는 입력이 같으면 항상 같은 출력을 내놓고, 객체의 상태를 변경하지 않으며, 함수 외부나 다른 바깥 환경과 상호작용하지 않는 `순수 함수(pure function)`를 사용한다.
- __함수형 프로그래밍으로 인한 장점__
  - 간결성, 가독성
  - 함수를 값 처럼 활용하기 때문에 강력한 `추상화(abstraction)` 제공
  - 멀티 스레드에서 안전하다.
  - 테스트 코드 작성이 쉽다.

## Kotlin 다중 플랫폼 작동 방식

![kotlin](https://user-images.githubusercontent.com/47518272/155450938-cca09507-ce48-4ae2-9e6e-65cd1da49176.png)

## [Kotlin for Server Side](https://kotlinlang.org/docs/server-overview.html)

- Kotlin을 사용한 서버 측 개발을 위한 프레임워크
- Kotlin 서버 측 애플리케이션 배포
- 등을 알 수있다.

## Kotlin/Native

- Kotlin 코드를 가상 머신 없이 실행할 수 있는 네이티브 바이너리로 컴파일하는 기술
- Kotlin/Native는 기본적으로 임베디드 장치 또는 IOS 와 같이 가상 머신 이 바람직하지 않거나 가능하지 않은 플랫폼에 대한 컴파일을 허용하도록 설계되었다.
- 개발자가 추가 런타임이나 가상 머신이 필요하지 않은 독립형 프로그램을 생성해야 하는 상황에 이상적입니다

