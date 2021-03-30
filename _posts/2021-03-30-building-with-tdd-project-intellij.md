---
title: "테스트 프로젝트 IntelliJ로 빌드하기"
excerpt: "src만 존재하는 TDD 연습 프로젝트 인텔리제이로 옮기는 과정"
date: 2021-03-30
---


# 시작하기

이클립스로 작업하던 프로젝트를 인텔리제이로 옮기는 과정을 포스팅해봄

아무런 설정 없이 src만 있는 프로젝트를 열어보면 아래 상태와 같음

![프로젝트를 처음 열었을 때 모습 이미지](/assets/images/post/2021-03-30-building-with-tdd-project-intellij_1.png)

## 목표

- 빨간 에러 없애기
- 인텔리제이 문서에서 소개된 아래처럼 바꾸기

![인텔리제이 문서 중 디렉터리 구조 스크린샷](/assets/images/post/2021-03-30-building-with-tdd-project-intellij_2.png)

# 설정하기

## STEP 1

🚩 src 폴더에 설정 된 소스 루트 해제하기

- src 폴더 우클릭 → Mark Directory as → Unmark as Sources Root

![(좌)해제 전, (우)해제 후 이미지](/assets/images/post/2021-03-30-building-with-tdd-project-intellij_3.png)

## STEP 2

🚩 test 아래 java 폴더를 테스트 소스 루트로 변경하기

- java 폴더 우클릭 → Mark Directory as → Test Sources Root

![(좌)변경 전, (우)변경 후 이미지](/assets/images/post/2021-03-30-building-with-tdd-project-intellij_4.png)


## STEP 3

🚩 Package path 변경

아래와 같이 package path를 `tdd`로 변경하라는 에러메세지가 뜨므로 변경

![package path 에러 내용 스크린샷](/assets/images/post/2021-03-30-building-with-tdd-project-intellij_5.png)

⇒ 이유: (2)에서 소스 루트를 변경했으므로

- 처음 → 소스 루트 `src`, package path는 하위인 test.java.tdd
- 변경 → 테스트 소스 루트 `java`, package path는 하위인 tdd

## STEP 4

🚩 junit 다운로드

![Add JUnit classpath 창 스크린샷](/assets/images/post/2021-03-30-building-with-tdd-project-intellij_6.png)

![Download library from Maven Repository 창 스크린샷](/assets/images/post/2021-03-30-building-with-tdd-project-intellij_7.png)

🎈완료🎈

[인텔리제이 테스트 설정 문서](https://www.jetbrains.com/help/idea/testing.html#add-testing-libraries)를 참고하여 작성하였음

# 추가로

## 파일 생성 시 Java Class 파일 타입 등이 안보일 때

![(상)파일타입이 안보이는 스크린샷, (하)파일 타입이 보이는 스크린샷](/assets/images/post/2021-03-30-building-with-tdd-project-intellij_8.png)

상위 디렉토리 중 어느것도 소스 루트가 아닐 때 이럴 수 있음

우클릭 → Mark Directory As → Sources Root 혹은 Test Sources Root 설정


## 폴더 색깔의 의미

- 회색 폴더 → 컨텐츠 루트, 콘텐츠의 최상위 폴더
- 하늘색 폴더 → 소스 루트: 컴파일해야하는 프로덕션 코드 보관
- 초록색 폴더 → 테스트 소스 루트: 테스트와 관련된 코드 보관
    - 테스트 코드를 프로덕션 코드와 별도로 보관

    [Content roots | IntelliJ IDEA](https://www.jetbrains.com/help/idea/content-roots.html#folder-categories)


## 컴파일 저장 위치

일반적으로 소스 및 테스트 소스에 대한 컴파일 결과도 다른 폴더로 설정함

- 기본설정일 때

![(좌)소스 파일 저장위치 구조, (우)컴파일 저장위치 구조 스크린샷](/assets/images/post/2021-03-30-building-with-tdd-project-intellij_9.png)

- 컴파일 폴더 수동 설정
    - 프로젝트 우클릭 → Open Module Settings → Modules → Paths → Use module complie output path


## JUnit5 패키지 명의 jupiter는 무엇일까?

> 요약: JUnit5의 서브 패키지이다.

JUnit5: Spring Boot 2.2.x 이후 기본적으로 제공되는 버전

jupiter: JUnit5에서 등장한 새로운 모델 및 TestEngine을 제공하는 서브 패키지

- [참고링크](https://sabarada.tistory.com/79)
- [JUnit5 docs](https://junit.org/junit5/docs/current/user-guide/)


## directory 구조에 대해

![(좌)src 밑 main, test가 있는 구조, (우)src와 test가 같은 레벨인 구조](/assets/images/post/2021-03-30-building-with-tdd-project-intellij_10.png)

내가 왼쪽 선택한 이유

- [Apache Software Foundation의 표준 디렉토리 구조](http://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html)라고 함

그리고 ~~왼쪽 구조를 사용할 때 코드와 테스트 코드를 동일한 패키지에 배치할 경우 protected 메서드도 테스트 가능하다, 소스에서 코드와 리소스 파일을 쉽게 분리할 수 있는 구조다~~라고 하는데 이 부분은 직접 확인하거나 지금 이해가는 부분이 아니라 *이런 것도 고려 대상이구나~*정도로 참고 하였음 


## JUnit4.0 test classes found in package '<default package>' 에러

해결방법: 테스트 클래스 및 메서드가 public이어야 함

[IntelliJ JUnit4 - "0 test classes found"](https://www.reddit.com/r/javahelp/comments/4ca825/intellij_junit4_0_test_classes_found/?st=ixec25w2&sh=8e1fa458)

~~왜 5 버전을 쓰다가 4 내용이 나오냐? 저도 알고싶지 않았어요....~~


# 마치며

이 삽질을 하게 된 그리고 돌고 돌은 원인

- 소스 루트와 설정 자체를 몰랐다
- 인텔리제이를 몰랐다
- JUnit5 및 jupiter를 몰랐다
- 테스트 코드 컴파일 위치에 대해 생각해본적이 없다
- 테스트 코드 분리에 대해 고민해본적이 없다

소스 루트, 테스트 소스 루트 지정은 "내 소스 파일은 여기 아래 있어"라고 설정해주는 것이다.

처음에 그냥 단어의 뜻도 생각하지 않고 변경하다가 왜 path가 바뀌지?하며 브레이크가 걸렸다.

이클립스에서 세팅할 때도 인텔리제이와 명칭만 다를 뿐 Build path > Configure Build Path...에서 같은 맥락으로 설정할 수 있다. (test sources folder, output folder 지정 등)
