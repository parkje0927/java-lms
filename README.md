# 학습 관리 시스템(Learning Management System)

## 레거시 코드 리팩터링

### 질문 삭제하기 요구사항

- 질문 데이터를 완전히 삭제하는 것이 아니라 데이터의 상태를 삭제 상태(deleted - boolean type)로 변경한다.
- 로그인 사용자와 질문한 사람이 같은 경우 삭제 가능하다.
- 답변이 없는 경우 삭제가 가능하다.
- 질문자와 답변글의 모든 답변자가 같은 경우 삭제가 가능하다.
- 질문을 삭제할 때 답변 또한 삭제해야 하며, 답변의 삭제 또한 삭제 상태(deleted)를 변경한다.
- 질문자와 답변자가 다른 경우 답변을 삭제할 수 없다.
- 질문과 답변 삭제 이력에 대한 정보를 DeleteHistory를 활용해 남긴다.

### 리팩터링 요구사항

- [x] `nextstep.qna.service.QnaService`의 `deleteQuestion()`는 앞의 질문 삭제 기능을 구현한 코드이다. 이 메서드는 단위 테스트하기 어려운 코드와 단위 테스트 가능한 코드가 섞여 있다.
- [x] QnaService의 `deleteQuestion()` 메서드에 단위 테스트 가능한 코드(핵심 비지니스 로직)를 도메인 모델 객체에 구현한다.
- [x] QnaService의 비즈니스 로직을 도메인 모델로 이동하는 리팩터링을 진행할 때 TDD로 구현한다.
- [x] 도메인 모델로 로직을 이동한 후에도 `QnaServiceTest`의 모든 테스트는 통과해야 한다.
  - QnaService의 `deleteQuestion()` 메서드에 대한 단위 테스트는 `src/test/java` 폴더의 `next.qna.service.QnaServiceTest`이다.

## 수강신청

### 수강 신청 기능 요구사항

- [x] 과정(Course)은 기수 단위로 여러 개의 강의(Session)를 가질 수 있다.
- [x] 강의는 시작일과 종료일을 가진다.
- [x] 강의는 강의 커버 이미지 정보를 가진다.
- [x] 강의는 무료 강의와 유료 강의로 나뉜다.
- [x] 강의 상태는 준비중, 모집중, 종료 3가지 상태를 가진다.
- [x] 강의 수강신청은 강의 상태가 모집중일 때만 가능하다.
- [x] 강의는 강의 최대 수강 인원을 초과할 수 없다.

### 프로그래밍 요구사항

- DB 테이블 설계 없이 도메인 모델부터 구현한다.
- 도메인 모델은 TDD로 구현한다.
  - 단, Service 클래스는 단위 테스트가 없어도 된다.

### 변경된 기능 요구사항

- [x] 강의가 진행 중인 상태에서도 수강신청이 가능해야 한다.
  - [x] 강의 진행 상태(준비중, 진행중, 종료)와 모집 상태(비모집중, 모집중)로 상태 값을 분리해야 한다.
- [x] 우아한테크코스(무료), 우아한테크캠프 Pro(유료)와 같이 선발된 인원만 수강 가능해야 한다.
  - [x] 강사는 수강신청한 사람 중 선발된 인원에 대해서만 수강 승인이 가능해야 한다.
  - [x] 강사는 수강신청한 사람 중 선발되지 않은 사람은 수강을 취소할 수 있어야 한다.

### 프로그래밍 요구사항

- 리팩터링할 때 컴파일 에러와 기존의 단위 테스트의 실패를 최소화하면서 점진적인 리팩터링이 가능하도록 한다.
- DB 테이블에 데이터가 존재한다는 가정하에 리팩터링해야 한다.
  - 즉, 기존에 쌓인 데이터를 제거하지 않은 상태로 리팩터링 해야 한다.
