# Kotest

- [AccpetanceTest with FeatureSpec](https://github.com/mikelalvarezgo/CryptoSimulator/blob/0f4b9fd338c5680a564e0c567c1eb50433aba5fc/src/test/kotlin/com/mikelalvarezgo/crypto_wolf/infrastructure/AcceptanceTestCase.kt)
- Kotest에서는 테스트 간 격리 레벨에 대해 디폴트로 SingleInstance를 설정하고 있으며 이 경우 Mocking 등의 이유로 테스트 간 충돌이 발생할 수 있습니다. 따라서 테스트간 완전한 격리를 위해서는 아래와 같이 IsolationMode를 InstancePerLeaf로 지정해 사용해야 합니다.
  - https://kotest.io/docs/framework/isolation-mode.html

## Links

- https://kotest.io/docs/framework/testing-styles.html#feature-spec
- https://eclipse4j.tistory.com/360
- https://techblog.woowahan.com/5825/
