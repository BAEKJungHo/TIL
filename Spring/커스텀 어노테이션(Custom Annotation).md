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
