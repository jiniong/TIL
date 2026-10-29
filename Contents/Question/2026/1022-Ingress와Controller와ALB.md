# Ingress·Controller·ALB의 설정 흐름과 요청 흐름

- 분류: 질문
- 기록일: 2026-10-22
- 갱신일: 2026-10-29
- 주제: Kubernetes, Ingress, ALB, 네트워크
- 출처: Codex 대화 「서버 단위와 Pod 방식 비교 및 10월 26일 복습」
- 상태: 개념 정리

## 목차

- [질문 1](#질문-1)
- [답변 1](#답변-1)
- [질문 2](#질문-2)
- [답변 2](#답변-2)
- [관련 지식](#관련-지식)
- [업데이트 기록](#업데이트-기록)

## 질문 1

Ingress는 Nginx 같은 서버이고 AWS Load Balancer Controller가 실제 요청을 전달할까?

## 답변 1

Ingress는 호스트·경로에 따라 어느 Service로 보낼지 정의하는 규칙이다. 그 자체가 요청을 받는 프록시 프로세스는 아니다. Ingress Controller는 규칙을 읽고 실제 트래픽 처리 구성을 만든다.

AWS Load Balancer Controller를 사용하는 경우 설정과 요청의 흐름을 분리한다.

~~~text
설정 반영
Ingress / Service 설정
    → AWS Load Balancer Controller
    → ALB 리스너·규칙·대상 그룹 설정

실제 요청
사용자 → ALB → 등록된 대상 → 앱
~~~

Controller가 요청마다 ALB 앞에서 프록시하는 것은 아니다. ALB가 설정된 규칙에 따라 실제 요청을 전달한다. [Ingress 공식 개념](https://kubernetes.io/docs/concepts/services-networking/ingress/)

## 질문 2

실제 패킷은 항상 ALB → Service → Pod 순서로 갈까?

## 답변 2

Service는 라우팅 대상을 정의할 때 중요하지만, 항상 Service의 ClusterIP를 패킷이 거친다고 그리면 부정확하다.

| ALB 대상 방식 | 단순화한 요청 경로 |
| --- | --- |
| IP 대상 | 사용자 → ALB → Pod IP |
| Instance 대상 | 사용자 → ALB → Node의 NodePort → Pod |

배포 환경과 네트워크 설정에 따라 지원 방식이 달라진다. 실제 경로를 추적할 때는 target-type, Service 종류, 대상 그룹에 등록된 주소를 확인한다. [AWS Load Balancer Controller의 Ingress 설정](https://kubernetes-sigs.github.io/aws-load-balancer-controller/latest/guide/ingress/annotations/#target-type)

## 관련 지식

- [Kubernetes 구성요소](1022-Kubernetes구성요소.md)
- [Gateway 라우팅](1028-Gateway라우팅.md)

## 업데이트 기록

- 2026-10-29: Controller의 설정 반영과 ALB의 실제 요청 전달을 분리해 정리
