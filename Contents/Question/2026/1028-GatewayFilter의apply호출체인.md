# GatewayFilterFactory.apply의 실제 호출 체인

- 분류: 질문
- 기록일: 2026-10-28
- 갱신일: 2026-10-29
- 주제: Spring, Gateway, 호출 체인
- 출처: Codex 대화 「application.yaml gateway 설명」
- 상태: Gateway 4.3.0 소스 기준

## 목차

- [질문 1](#질문-1)
- [답변 1](#답변-1)
- [질문 2](#질문-2)
- [답변 2](#답변-2)
- [관련 지식](#관련-지식)
- [업데이트 기록](#업데이트-기록)

## 질문 1

직접 호출한 적이 없는 apply는 최종적으로 누가 호출할까?

## 답변 1

Spring Cloud Gateway 4.3.0의 RouteDefinitionRouteLocator.loadGatewayFilters가 필터 팩토리의 apply를 호출한다.

~~~text
RouteDefinitionRouteLocator.getRoutes()
  → convertToRoute()
    → getFilters()
      → loadGatewayFilters()
        → factory.apply(configuration)
          → 사용자 정의 GatewayFilterFactory.apply(config)
~~~

직접 호출문은 다음과 같다.

~~~java
GatewayFilter gatewayFilter = factory.apply(configuration);
~~~

설정의 Authentication 이름으로 등록된 AuthenticationGatewayFilterFactory를 찾고 설정 객체를 바인딩해 필터를 생성한다. [4.3.0 공식 소스](https://github.com/spring-cloud/spring-cloud-gateway/blob/v4.3.0/spring-cloud-gateway-server/src/main/java/org/springframework/cloud/gateway/route/RouteDefinitionRouteLocator.java)

## 질문 2

apply가 호출되는 시점과 실제 요청을 검사하는 시점은 같을까?

## 답변 2

- 라우트 구성 시점: apply(config)가 GatewayFilter를 만들어 반환
- 요청 처리 시점: 반환된 필터의 filter 메서드 또는 람다 본문 실행

~~~java
@Override
public GatewayFilter apply(Config config) {
    return (exchange, chain) -> {
        // 이 본문은 선택된 라우트의 요청을 처리할 때 실행
        return chain.filter(exchange);
    };
}
~~~

위 코드는 두 실행 시점을 보기 위한 축약 예시다. 라우트가 다시 구성되면 apply가 다시 호출될 수 있으므로 “애플리케이션에서 무조건 한 번만 실행”으로 외우지 않는다.

| 순서 설정 | 결정하는 대상 |
| --- | --- |
| 라우트의 order | 여러 라우트 중 어떤 라우트를 먼저 비교·선택할지 |
| OrderedGatewayFilter의 order | 선택된 라우트에서 필터를 어떤 순서로 실행할지 |

exchange는 요청·응답을 담는 객체이고 chain.filter(exchange)는 다음 처리로 이어가는 호출이다. 인증 실패 분기에서 오류를 반환하고 chain을 호출하지 않으면 후속 전달이 중단된다.

## 관련 지식

- [Gateway 라우팅 설정](1028-Gateway라우팅.md)
- 프레임워크 호출이 궁금할 때: 구현체 → 등록 이름 → 팩토리를 찾는 코드 → 실제 호출문 → 요청 시 실행 코드를 순서대로 추적

## 업데이트 기록

- 2026-10-29: 개념 설명에 더해 직접 호출 클래스·메서드와 버전 기록
