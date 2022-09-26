# IntelliJ Settings

- __Appearance & Behavior__
  - Theme: `Super Dark Theme`
  - Progress Bar: Mario Progress Bar
  - Use custom font : `Malgun Gothic` size : 12
  - Presentation Mode : Font size : 24
- __Editor > Font__
  - ![fontsetting](https://user-images.githubusercontent.com/47518272/155870655-ec52dcbb-5d9f-4567-95b8-d269e25ddd8a.png)
- __Editor > File Encodings__
  - `UTF-8`
- __Editor > General > Auto import__
  - ![optimizeimport](https://user-images.githubusercontent.com/47518272/156608752-4bad388f-a867-49e9-9514-0ccaac4b7e89.png)
- __Editor > Code Style > Java__
  - Use single class import
  - Class count to use import with `*`, Names count to use static import with `*` : 100 으로 설정
- __Editor > Code Style > Kotlin__
  - Use single name import
  - Packages to Use import with `*` 에서 `java.util.*` 제거
- __Editor > Live Templates > custom__
  - `atdd`
    - ```
      // when
      java.util.Map<String, String> params = new java.util.HashMap<>();
      io.restassured.response.ExtractableResponse<io.restassured.response.Response> response = io.restassured.RestAssured
              .given().log().all()
              .body(params)
              .contentType(org.springframework.http.MediaType.APPLICATION_JSON_VALUE)
              .when().$METHOD$("$URI$")
              .then().log().all().extract();

      // then
      org.assertj.core.api.Assertions.assertThat(response.statusCode()).isEqualTo(org.springframework.http.HttpStatus.$STATUS$.value());
      ```
  - `tdd`
    - ```
      /**
       * Given
       * When
       * Then
       */
      @DisplayName("Scenario")
      @Test
      void $NAME$() {
          $END$
      }
    ```
- __Plugins__
  - AsciiDoc
  - Class Decompile
  - Java Decompiler
  - Kotlin to Java Decompiler 
  - Kotlin
  - Key Promoter X
  - Live Edit
  - CodeMetrics
  - Commit Message Template
  - Database Navigator
  - Duck Progress Bar
  - Extra Icons
  - Translator
    - Tools > Translator > DEFAULT
  - SonarLint
  - String Manipulation
  - .ignore

# Other Settings

- Talend API Tester
- Octotree
- JSON Viewer
