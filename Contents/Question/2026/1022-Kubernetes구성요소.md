# Cluster·Node·Namespace·Pod·Service의 관계

- 분류: 질문
- 기록일: 2026-10-22
- 갱신일: 2026-10-29
- 주제: Kubernetes, Namespace, Service
- 출처: Codex 대화 「서버 단위와 Pod 방식 비교 및 10월 26일 복습」
- 상태: 개념 정리

## 목차

- [질문 1](#질문-1)
- [답변 1](#답변-1)
- [질문 2](#질문-2)
- [답변 2](#답변-2)
- [질문 3](#질문-3)
- [답변 3](#답변-3)
- [관련 지식](#관련-지식)
- [업데이트 기록](#업데이트-기록)

## 질문 1

Cluster, Node, Control Plane, etcd는 각각 무엇일까?

## 답변 1

- Cluster: 함께 관리되는 Kubernetes 전체 시스템
- Node: Pod가 실행되는 물리 서버 또는 VM
- Control Plane: API 처리, 상태 유지, 배치 결정 등 클러스터를 관리하는 구성요소
- etcd: Kubernetes의 클러스터 상태를 보관하는 키·값 저장소

etcd가 직접 Pod를 실행하거나 일반 애플리케이션의 업무 DB 역할을 맡는 것은 아니다. 스케줄러는 아직 배치되지 않은 Pod를 어느 Node에 놓을지 결정하고, 해당 Node의 kubelet과 컨테이너 런타임이 실행을 담당한다.

## 질문 2

prod와 dev Namespace를 나누면 같은 Node에서 실행할 수 없을까?

## 답변 2

Namespace는 리소스의 논리적인 관리 범위다. Node를 물리적으로 나누는 기준이 아니다.

~~~text
하나의 Node
├─ prod Namespace의 API Pod
└─ dev Namespace의 API Pod
~~~

위 배치는 가능하다. 특정 Node에만 배치하려면 nodeSelector, affinity, taint/toleration 등 별도의 스케줄링 조건이 필요하다. Namespace만 나눈다고 네트워크 통신이 자동 차단되는 것도 아니므로 NetworkPolicy·권한·할당량을 별도로 본다.

복습에서 “Namespace가 Node를 나누므로 같은 Node에서 실행 불가”라고 답했던 부분을 수정했다. [Namespace 공식 개념](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/)

## 질문 3

Pod 하나에 Service 하나가 들어가는 구조일까?

## 답변 3

Pod는 컨테이너 실행 단위이고 Service는 Pod 집합에 접근할 안정적인 연결점을 제공하는 리소스다. Service가 Pod 안에서 실행되는 앱 프로세스는 아니다.

- 하나의 Service가 여러 Pod를 대상으로 삼을 수 있음
- 여러 Service가 같은 Pod를 대상으로 삼을 수도 있음
- 모든 Pod가 Service를 반드시 가져야 하는 것은 아님
- Pod가 교체되어도 클라이언트는 Service의 이름·주소를 사용할 수 있음

Service selector와 Pod label을 맞추면 대상 EndpointSlice가 관리된다. 클러스터에는 여러 Service가 존재할 수 있다. [Service 공식 개념](https://kubernetes.io/docs/concepts/services-networking/service/)

## 관련 지식

- [Ingress·Controller·ALB](1022-Ingress와Controller와ALB.md)
- [인프라 플랫폼과 워크스페이스 앱](1028-인프라와워크스페이스서비스.md)
- [단일 VM K3s 실습](../../Did/2026/1029-VirtualBox단일VM과K3s에어갭.md)

## 업데이트 기록

- 2026-10-29: 10월 22일 개념 학습과 10월 26일 Namespace 오답 수정 반영
