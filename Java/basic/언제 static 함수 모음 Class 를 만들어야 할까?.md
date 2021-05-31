# 언제 static 함수 모음 Class 를 만들어야 할까?

static 함수 모음 class란  Apache Commons Lang StringUtils 처럼 순전히 static 함수들만을 가지고 있고, 객체를 생성하지 않고 사용하는 클래스를 의미한다.

static 함수 모음 클래스의 모든 함수는 인자가 동일할 경우 항상 동일한 결과를 리턴해야 한다. 이 규칙을 지킬 수 없으면 POJO Bean으로 만들라.

이것이 이뤄지려면 함수 안에서는 `외부 자원(Resource)`에 대해 하나도 의존하면 안된다는 선결 조건을 충족해야 한다. 외부 자원은 그 실행 결과의 일관성을 보장할 수 없기 때문이다.

이에 가장 잘 들어맞는 예는 StringUtils, CollectionUtils 같은 것들이다.

예를 들면, StringUtils.contains("hello world", "hello")는 어떠한 상황에서도 동일한 결과를 리턴한다.

마지막으로 static 함수 모음 Class를 만들 때 내가 사용하는 방법 두 가지 정도를 소개한다.

- 프로젝트 전용 StringUtils 같은게 필요한 경우가 있다. 이 때 그냥 StringUtils라고 만들지 말고, Apache Commons나 Spring Framework 등에 존재하는 StringUtils 등을 상속해서 ProjectSpecificStringUtils를 만든다. 그리고서 원하는 기능을 하는 함수를 ProjectSpecificStringUtils에 추가하면 다른 프로그래머들은 일관성있게 ProjectSpecificStringUtils 만 사용하면 되게 된다. 이미 세상에는 대부분의 기능에 관한 유틸리티성 static 함수 모음 클래스들이 존재한다.
- static import 사용성을 높이려면 지나치게 보편적인 이름을 사용하는 것은 피하는게 낫다. 다시 말해 나는 StringUtils.contains() 보다는 StringUtils.strContains()를 선호한다.

## References

> http://kwon37xi.egloos.com/4844149
