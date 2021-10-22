---
title: "중첩 클래스 Nested Classes: 기본"
excerpt: "중첩 클래스 종류, 특징, 사용하는 이유 등 기본적인 내용 정리"
date: 2021-04-27
---


이 내용은 대체적으로 java 1.8 기준으로 작성됐다


# Nested Classes란?

- 다른 클래스 내에 정의한 중첩된 클래스

# 종류
![Nested classes 종류](/assets/images/post/2021-04-27-nested-classes-basic_1.png)

- nested class는 크게 두 가지로 분류된다
    - `non-static`
    - `static`
- nested class는 총 네 가지가 종류가 있으며 아래와 같이 다양하게 불린다
    1. member class
        - 인스턴스 클래스(instance class), instance member class, member class, inner class, ...
    2. static nested classes
        - 정적 클래스(static class), static member class, static inner class,  ...
    3. local classe 지역 클래스
    4. anonymous classes 익명 클래스
- `member`라는 키워드는 member 자리에 선언되기 때문이다

# 특징

- 접근 제어자를 4개 다 붙일 수 있음
    - 기존: `public`, `default`
- 제어자 사용 가능
    - `abstract`, `final`, ...
- `iv`와 `cv`간 규칙이 동일하게 적용됨

# 컴파일 시 파일 형식

- 외부 클래스명$**내부 클래스명**.class
    - 익명클래스는 외부 클래스명$**숫자**.class

# 사용하는 이유

> 두 클래스가 서로 긴밀한 관계에 있기 때문

1. 논리적으로 그룹화
    - **한 곳에서만 사용되는 클래스인 경우** 해당 클래스에 포함시켜 둘을 함께 유지하는게 논리적이다
        - 이러한 `helper classes`를 중첩시키면 패키지가 더욱 간소화됨
2. 캡슐화, 접근성 증가
    - A와 B라는 최상위 클래스가 있고, B가 `private`로 선언된 A의 `member`에 접근해야 하는 경우
        - A에 B를 숨기면 A의 `member`를 `private`으로 유지할 수 있고 B는 A에 접근이 가능해짐
        - B 자체를 외부에 숨길 수 있음
3. 더 읽기 쉽고 관리하기 쉬운 코드
    - 코드가 사용되는 곳에 더 가깝게 위치하게 되므로

# Serialization

> local 및 anonymous classes를 포함한 inner 클래스의 직렬화는 권장되지 않음

- java compiler가 inner class와 같은 특정 구조는 합성 구조로 컴파일 함
    - 합성 구조는 컴파일러 구현에 따라 다를 수 있음 → `.class` 파일은 컴파일러 구현에 따라 다를 수 있음
    - ⇒ 내부 클래스를 `serialize`하고 다른 JRE 구현으로 `deserialize` 할 경우 호환성 문제가 발생할 수 있다

---

# 참고 링크

- Effective java(2판)
- 자바의 정석(3판)
- [https://docs.oracle.com/javase/tutorial/java/javaOO/nested.html](https://docs.oracle.com/javase/tutorial/java/javaOO/nested.html)




{% viiviii/bbc737fe6e2f0fc5b3ce51f6db63b96d %}


{% gist viiviii/bbc737fe6e2f0fc5b3ce51f6db63b96d %}

