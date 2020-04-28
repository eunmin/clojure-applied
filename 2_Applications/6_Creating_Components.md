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

```clojure
myproject.util.string ;; utility
myproject.util.json   ;; utility
myproject.domain      ;; data - domain entities
myproject.config      ;; data - config data
myproject.services    ;; abstraction - service definitions
myproject.impl.xyz    ;; implementation of service abstraction
myproject.assembly    ;; assembly
myproject.main        ;; main entry point - command-line
```

### public  또는 private 함수



## 컴포넌트 API 설계

컴포넌트를 사용하는 방법은 크게 두가지로 나눌 수 있다. 하나는 함수 호출로 사용하는 방법과 다른 하나는 큐나 채널로 메시지를 전달 하는 방법이 있다.

### 함수로 컴포넌트 데이터 사용하기

컴포넌트 불변 데이터는 외부에 바로 공개해도 안전하다.

`ke`라는 컴포넌트에 아래와 같은 컴포넌트 함수가 필요하다고 가정해보자. 

```clojure
;; Read interface
(defn get-rules [ke])
(defn find-rules [ke criteria])

;; Update interface
(defn add-rule [ke rule])
(defn replace-rule [ke old-rule new-rule]) 
(defn delete-rule [ke rule])

;; Processing interface
(defn fire-rules [ke request])
```

여기서 `find-rules`는 `get-rules` 를 `filter`해서 만들 수 있고 업데이트 함수들은 `transform-rules` 라는 공통 기능으로 만들 수 있기 때문에 기본이 되는 함수는 아래와 같다.

```clojure
;; Get the rule-set
(defn get-rules [ke])

;; Transform from one rule-set to another
(defn transform-rules [ke update-fn])

;; Produce a response from a request
(defn fire-rules [ke request])
```

기본 함수들은 컴포넌트가 제공해야할 함수기 때문에 프로토콜로 정의하고 나머지는 일반 함수로 만들면 된다.

```clojure
(ns components.ke)
;; SPI protocol

(defprotocol KE
  (get-rules [ke] 
             "Get full rule set") 
  (transform-rules [ke update-fn]
                   "Apply transformation function to rule set. Return new KE.") 
  (fire-rules [ke request]
              "Fire the rules against the request and return a response"))

;; private helper functions
(defn- transform-criteria [criteria] ;; ...
)

;; api fns built over the protocol
(defn find-rules [ke criteria]
  (filter (transform-criteria criteria) (get-rules ke)))

(defn add-rule [ke rule]
  (transform-rules ke #(conj % rule)))

(defn replace-rule [ke old-rule new-rule]
  (transform-rules ke #(-> % (disj old-rule) (conj new-rule))))

(defn delete-rule [ke rule]
  (transform-rules ke #(-> % (disj rule))))
```





## 채널로 컴포넌트 연결하기

## 컴포넌트 구현

