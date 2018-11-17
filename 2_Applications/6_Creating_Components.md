## 네임스페이스 구성

클로저 함수, 레코드, 프로토콜 등 최상의 형식은 네임스페이스로 묶을 수 있다. 같은 이름을 다른 네임스페이스에서
쓸 수 있기 때문에 이름이 겹치는 것을 막을 수 있다.

### 네임스페이스 분류

#### 유틸리티

유틸리티 네임스페이스에는 일반 함수들을 정의한다. 예를 들면 문자열 함수나 파일 포멧 함수 같은 것들이 있다.
일반적으로 유틸리티 네임스페이스는 다른 네임스페이스에 디팬던시가 적다.

#### 데이터 정의

여기에는 커스텀 컬렉션이나 도메인 엔티티를 정의하고 그걸 사용하는 헬퍼 함수들을 정의한다.

#### 추상

프로토콜 같은 추상 레벨의 정의를 한다.

#### 구현

추상 네임스페이스에서 정의된 프로토콜이나 인터페이스를 구현한다. 이 구현은 어플리케이션에서 조립해서 사용한다.

#### 어셈블리

구현들이 어떻게 조립되는지에 대한 구현과 설정이 있다. 구현에는 프로토콜이나 데이터 구조를 직접사용한다?

#### 진입점

애플리케이션이 시작할 때 진입점이다. 여러개를 가질 수 있다. 설정을 초기화 하고 어셈블리를 초기화 하고
애플리케이션 라이프 사이클을 관리한다.

네임 스페이스 예제

```
myproject.util.string ;; utility
myproject.util.json   ;; utility
myproject.domain      ;; data - domain entities
myproject.config      ;; data - config data
myproject.services    ;; abstraction - service definitions
myproject.impl.xyz    ;; implementation of service abstraction
myproject.assembly    ;; assembly
myproject.main        ;; main entry point - command-line
```
