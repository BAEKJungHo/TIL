# 커스텀 어노테이션

- @RequestMapping 어노테이션을 메타 어노테이션으로 사용할 수 있다.
  - @GetMapping 같은 커스텀한 어노테이션을 만들 수 있다.
  
- 메타(Meta) 어노테이션
  - 어노테이션에 사용할 수 있는 어노테이션
  - 스프링이 제공하는 대부분의 어노테이션은 메타 어노테이션으로 사용할 수 있다.
  
- 조합(Composed) 어노테이션
  - 한개 혹은 여러 메타 어노테이션을 조합해서 만든 어노테이션
  - 코드를 간결하게 줄일 수 있다.
  - 보다 구체적인 의미를 부여할 수 있다.
  - ex) @GetMapping, @PostMapping 등

- @Retention
  - 해당 어노테이션 정보를 언제까지 유지할 것인가
  - Source : 소스 코드 까지만 유지. 즉, 컴파일 하면 해당 어노테이션 정보는 사라짐
    - 어노테이션을 주석처럼 사용하고 싶은 경우
  - Class : 컴파일 한 class 파일에도 유지. 즉, 런타임 시 클래스를 메모리로 읽어오면 해당 정보는 사라진다.
  - Runtime : 클래스를 메모리에 읽어왔을 때까지 유지. 코드에서 이 정보를 바탕으로 특정 로직을 실행할 수 있다.
    - 만약 핸들러 메서드 위에 사용하는 경우 무조건 Runtime 으로 설정
 
 - @Target
  - 해당 어노테이션을 어디에 사용할 수 있는지 결정
  
- @Documented
  - 해당 어노테이션을 사용한 코드의 문서에 그 어노테이션에 대한 정보를 표기할지 결정한다.
  - 해당 어노테이션을 사용한 JavaDoc 에 남는다.
  - @AliasFor 가 생성됨.
  - 즉, 어노테이션을 문서화 한다.
  
## Example

```java
/*
 * Copyright (c) 2003, 2013, Oracle and/or its affiliates. All rights reserved.
 * DO NOT ALTER OR REMOVE COPYRIGHT NOTICES OR THIS FILE HEADER.
 *
 * This code is free software; you can redistribute it and/or modify it
 * under the terms of the GNU General Public License version 2 only, as
 * published by the Free Software Foundation.  Oracle designates this
 * particular file as subject to the "Classpath" exception as provided
 * by Oracle in the LICENSE file that accompanied this code.
 *
 * This code is distributed in the hope that it will be useful, but WITHOUT
 * ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or
 * FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License
 * version 2 for more details (a copy is included in the LICENSE file that
 * accompanied this code).
 *
 * You should have received a copy of the GNU General Public License version
 * 2 along with this work; if not, write to the Free Software Foundation,
 * Inc., 51 Franklin St, Fifth Floor, Boston, MA 02110-1301 USA.
 *
 * Please contact Oracle, 500 Oracle Parkway, Redwood Shores, CA 94065 USA
 * or visit www.oracle.com if you need additional information or have any
 * questions.
 */

package java.lang;

import java.lang.annotation.*;
import static java.lang.annotation.ElementType.*;

/**
 * A program element annotated &#64;Deprecated is one that programmers
 * are discouraged from using, typically because it is dangerous,
 * or because a better alternative exists.  Compilers warn when a
 * deprecated program element is used or overridden in non-deprecated code.
 *
 * @author  Neal Gafter
 * @since 1.5
 * @jls 9.6.3.6 @Deprecated
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(value={CONSTRUCTOR, FIELD, LOCAL_VARIABLE, METHOD, PACKAGE, PARAMETER, TYPE})
public @interface Deprecated {
}
```

## @Retention 어노테이션 범위

- SOURCE 
  - 컴파일러가 사용하고 클래스 파일에는 포함되지 않음 (단순 주석용, 컴파일러용)
- CLASS
  - 컴파일시 클래스 파일 안에 포함되나 JVM 에서 무시함
- RUNTIME
  - 컴파일시 포함되고 JVM 에서 인식함

## 주석용 어노테이션

- 목록
- 상세
- 등록
- 등록폼
- 수정
- 수정폼
- 삭제
- 복원
- 완전삭제
- 엑셀다운로드 

등등 주석용 어노테이션 생각해보기

```java
/**
 * 목록 처리를 나타내는 어노테이션
 * @author  BAEKJH;
 */
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Inventory {
}


/**
 * 등록 처리를 나타내는 어노테이션
 * @author  BAEKJH;
 */
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Create {
}

/**
 * 등록폼 이동을 나타내는 어노테이션
 * @author  BAEKJH;
 */
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface CreateForm {
}

/**
 * 등록 처리를 나타내는 어노테이션
 * @author  BAEKJH;
 */
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Create {
}

/**
 * 상세 페이지 처리를 나타내는 어노테이션
 * @author  BAEKJH;
 */
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Detail {
}

/**
 * 수정폼 이동을 나타내는 어노테이션
 * @author  BAEKJH;
 */
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface EditForm {
}

/**
 * 수정 처리를 나타내는 어노테이션
 * @author  BAEKJH;
 */
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Edit {
}

/**
 * 삭제 처리를 나타내는 어노테이션
 * 삭제 상태여부인 Delck 를 Update 함
 * @author  BAEKJH;
 */
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Delck {
}

/**
 * 완전 삭제 처리를 나타내는 어노테이션
 * 실제 DB 에서도 삭제 처리를 함
 * @author  BAEKJH;
 */
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Remove {
}

/**
 * 엑셀 다운로드를 나타내는 어노테이션
 * @author  BAEKJH;
 */
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface ExcelDownLoad {
}

/**
 * 복원을 나타내는 어노테이션
 * @author  BAEKJH;
 */
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Recover {
}
```
