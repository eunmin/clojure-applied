# 도메인 모델링

## 엔티티 모델링

클로저는 맵과 레코드로 도메인 엔티티를 표현할 수 있음

### 맵

태양계 행성 엔티티를 만든다면 다음과 같이 만들 수 있음 엔티티 타입은 `:type` 키에 `:Plant`라고 지정 하지만
좋은 방법은 아님

```clojure
(def earth {
  :name "Earth"
  :moon 1
  :volume 1.08321e12 ;; km^3
  :mass 5.97219e24 ;; kg
  :aphelion 152098232 ;; km, farthest from sun
  :perihelion 147098290 ;; km, closest to sun
  :type :Plant ;; 엔티티 타입
})
```

### 레코드

태양계 행성 엔티티를 레코드로 표현하면 타입과 생성자 함수를 가질 수 있기 때문에 더 좋다.

```clojure
(defrecord Planet [name
                   moons
                   volume ;; km^3
                   mass ;; kg
                   aphelion ;; km, farthest from sun
                   perihelion ;; km, closest to sun
                   ])
```

### 맵과 레코드 중에 선택하기

- 엔티티 모델에는 대부분 레코드가 더 적합한 선택이다.
- 공개 API를 만드는 경우 레코드 보다 더 유연한 맵을 선택하는 것이 더 적합하다.


## 엔티티 생성

### 생성자의 옵션 처리

### 위치 기반 디스트럭처링

### 맵 디스트럭처링

### 생성자에서 계산하기

### 부수 효과가 있는 생성자

### 기본 엔티티

## 관계 모델링

## 엔티티 유효성 검증

##

- 타입 기반의 디스패칭은 프로토콜을 사용하는 것이 더 좋다. (Protocols are usually preferred for type-based dispatch.)
- 프로토콜과 멀티메서드는 자바 타입 계층 구조를 지원하는데 멀티메서드는 자체 타입 계층 구조를 만들 수 있다.
