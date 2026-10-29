# Gateway 라우팅과 인증 필터 설정 읽기

- 분류: 질문
- 기록일: 2026-10-28
- 갱신일: 2026-10-29
- 주제: Spring, Gateway, 라우팅
- 출처: Codex 대화 「application.yaml gateway 설명」
- 상태: 당시 소스 확인 기반 개념 정리

## 목차

- [질문 1](#질문-1)
- [답변 1](#답변-1)
- [질문 2](#질문-2)
- [답변 2](#답변-2)
- [관련 지식](#관련-지식)
- [업데이트 기록](#업데이트-기록)

## 질문 1

application.yml에 추가된 Gateway는 어떤 역할일까?

## 답변 1

Gateway는 요청의 조건을 확인하고 목적지 서비스로 전달하는 입구다. 아래는 대화에서 확인한 설정 형태를 일반화한 예시다.

~~~yaml
- id: api-main
  uri: http://localhost:8082
  order: 100
  predicates:
    - Path=/apis/main/**
  filters:
    - PreserveHostHeader
    - StripPrefix=1
    - Authentication
~~~

| 항목 | 의미 |
| --- | --- |
| id | 라우트 이름 |
| uri | 요청 전달 목적지 |
| order | 라우트 비교 순서, 낮은 값 우선 |
| predicates | 라우트 선택 조건 |
| filters | 선택한 라우트에 적용할 요청·응답 처리 |

StripPrefix=1이면 /apis/main/example에서 맨 앞 경로 한 조각을 제거해 /main/example로 전달한다. 목적지 서비스의 context path와 함께 확인한다. PreserveHostHeader는 원래 Host 헤더를 유지하는 설정이다.

## 질문 2

공지처럼 공개 경로가 일반 경로와 겹치면 어느 설정이 적용될까?

## 답변 2

공지 전용 라우트의 order가 10이고 일반 라우트가 100이라면, 두 조건에 모두 맞는 요청은 우선순위가 높은 공지 라우트로 선택될 수 있다.

선택된 공개 라우트에 Authentication이 없다고 모든 업무 권한 검사가 없어지는 것은 아니다. Gateway의 통과 조건과 목적지 서비스의 데이터 접근·변경 권한은 함께 확인해야 한다.

대화에서 확인한 인증 구조는 전역 필터가 토큰을 확인해 사용자 정보를 전달하고, 경로별 Authentication 필터가 사용자 정보의 존재 여부를 검사하는 방식이었다. 이 흐름은 당시 프로젝트 구현의 사례이며 Gateway의 모든 프로젝트가 같은 구조를 쓰는 것은 아니다.

## 관련 지식

- [apply의 실제 호출자와 실행 시점](1028-GatewayFilter의apply호출체인.md)
- [Spring Cloud Gateway 공식 문서](https://docs.spring.io/spring-cloud-gateway/reference/)
- [Ingress·Controller·ALB의 역할](1022-Ingress와Controller와ALB.md)

## 업데이트 기록

- 2026-10-29: 10월 28일 소스 확인 결과에서 재사용할 라우팅 개념 추출
