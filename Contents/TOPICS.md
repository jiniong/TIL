# 주제별 지식 지도

- 분류: 색인
- 갱신일: 2026-10-29
- 출처: 저장소의 질문·실습·독서·영상 노트

## 목차

- [인프라와 Kubernetes](#인프라와-kubernetes)
- [AI와 모델 서빙](#ai와-모델-서빙)
- [백엔드와 요청 처리](#백엔드와-요청-처리)
- [아두이노와 임베디드](#아두이노와-임베디드)
- [데이터베이스와 검색](#데이터베이스와-검색)
- [네트워크와 콘텐츠 전송](#네트워크와-콘텐츠-전송)
- [개발 방법과 읽을거리](#개발-방법과-읽을거리)

날짜별 기록은 [전체 목차](README.md), 새 기록의 형식과 갱신 기준은 [템플릿](TEMPLATE.md)에서 확인합니다. 질문으로 원리를 이해하고 관련 실습에서 실제 동작을 확인합니다.

## 인프라와 Kubernetes

- [VM과 컨테이너의 실행 구조](Question/2026/1022-VM%EA%B3%BC%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88.md)
- [ECS·EKS와 EC2·Fargate의 역할](Question/2026/1022-AWS%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88%EA%B4%80%EB%A6%AC.md)
- [Cluster·Node·Namespace·Pod·Service의 관계](Question/2026/1022-Kubernetes%EA%B5%AC%EC%84%B1%EC%9A%94%EC%86%8C.md)
- [Ingress·Controller·ALB의 설정 흐름과 요청 흐름](Question/2026/1022-Ingress%EC%99%80Controller%EC%99%80ALB.md)
- [Kubernetes 인프라와 워크스페이스 서비스의 책임](Question/2026/1028-%EC%9D%B8%ED%94%84%EB%9D%BC%EC%99%80%EC%9B%8C%ED%81%AC%EC%8A%A4%ED%8E%98%EC%9D%B4%EC%8A%A4%EC%84%9C%EB%B9%84%EC%8A%A4.md)
- [VirtualBox 단일 Ubuntu VM과 K3s 에어갭 실습](Did/2026/1029-VirtualBox%EB%8B%A8%EC%9D%BCVM%EA%B3%BCK3s%EC%97%90%EC%96%B4%EA%B0%AD.md)

## AI와 모델 서빙

- [토큰·컨텍스트·파라미터 숫자의 의미](Question/2026/1029-%ED%86%A0%ED%81%B0%EC%BB%A8%ED%85%8D%EC%8A%A4%ED%8A%B8%EC%99%80%ED%8C%8C%EB%9D%BC%EB%AF%B8%ED%84%B0.md)
- [글자가 숫자로 계산되어 답변이 되는 과정](Question/2026/1029-%ED%86%A0%ED%81%B0%EC%83%9D%EC%84%B1%EC%88%AB%EC%9E%90%EC%98%88%EC%8B%9C.md)
- [역전파와 가중치 업데이트의 숫자 예시](Question/2026/1026-%EC%97%AD%EC%A0%84%ED%8C%8C%EC%99%80%EA%B8%B0%EC%9A%B8%EA%B8%B0.md)
- [이미 만들어진 LLM의 추가 학습과 LoRA](Question/2026/1026-LLM%ED%95%99%EC%8A%B5%EA%B3%BCLoRA.md)
- [추론 모델의 중간 계산과 자기 검산](Question/2026/1029-%EC%B6%94%EB%A1%A0%EB%AA%A8%EB%8D%B8%EA%B3%BC%EA%B2%80%EC%82%B0.md)
- [vLLM과 토큰 팩토리의 역할](Question/2026/1026-vLLM%EA%B3%BC%ED%86%A0%ED%81%B0%ED%8C%A9%ED%86%A0%EB%A6%AC.md)

## 백엔드와 요청 처리

- [Gateway 라우팅과 인증 필터 설정 읽기](Question/2026/1028-Gateway%EB%9D%BC%EC%9A%B0%ED%8C%85.md)
- [GatewayFilterFactory.apply의 실제 호출 체인](Question/2026/1028-GatewayFilter%EC%9D%98apply%ED%98%B8%EC%B6%9C%EC%B2%B4%EC%9D%B8.md)
- [S3 멀티파트 업로드](Did/2025/0802-%EB%A9%80%ED%8B%B0%ED%8C%8C%ED%8A%B8%EC%97%85%EB%A1%9C%EB%93%9C.md)
- [Node.js 비동기·병렬 처리](Did/2025/0804-nodejs%EB%B3%91%EB%A0%AC%EC%B2%98%EB%A6%AC.md)
- [Node.js 멀티코어 실험](Did/2025/0805-%EB%A9%80%ED%8B%B0%EC%BD%94%EC%96%B4.md)
- [WebSocket과 메시지 브로커](Question/2025/0807-%EB%A9%94%EC%8B%9C%EC%A7%80%EB%B8%8C%EB%A1%9C%EC%BB%A4.md)
- [토큰 저장과 탈취 대응](Question/2025/0715-%ED%86%A0%ED%81%B0.md)
- [리프레시 토큰 갱신](Question/2025/0717-%ED%86%A0%ED%81%B0.md)

## 아두이노와 임베디드

- [아두이노 입문: 전압·저항·GND·GPIO](Did/2026/1027-%EC%95%84%EB%91%90%EC%9D%B4%EB%85%B8%EA%B8%B0%EC%B4%88%EC%99%80GPIO.md)
- [ESP32 버튼·LED 전환과 디바운싱](Did/2026/1027-ESP32%EB%B2%84%ED%8A%BCLED%EC%99%80%EB%94%94%EB%B0%94%EC%9A%B4%EC%8B%B1.md)
- [Arduino 슬라이더 입력과 PWM 밝기 제어](Did/2026/1028-Arduino%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%8D%94%EC%99%80PWM.md)
- [Arduino Mega의 DHT22 온도와 OLED 표시](Did/2026/1028-ArduinoDHT22%EC%99%80OLED.md)

## 데이터베이스와 검색

- [팔로우·팔로잉 저장 방식](Question/2025/0714-%ED%8C%94%EB%A1%9C%EC%9E%89%EC%8A%A4%ED%82%A4%EB%A7%88.md)
- [본문 검색과 인덱스](Question/2025/0716-%EC%9D%B8%EB%8D%B1%EC%8A%A4.md)
- [UUID와 페이지네이션](Question/2025/0722-%ED%8E%98%EC%9D%B4%EC%A7%80%EB%84%A4%EC%9D%B4%EC%85%98.md)
- [정규화 범위](Question/2025/0724-%EC%A0%95%EA%B7%9C%ED%99%94.md)
- [Notification·MySQL 인덱스](Question/2025/0808-%EC%9D%B8%EB%8D%B1%EC%8A%A4.md)
- [Elastic Search Bible](Book/2026/Elastic%20Search%20Bible/README.md)

## 네트워크와 콘텐츠 전송

- [CSMA/CD와 CSMA/CA](Question/2025/0723-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC%EC%A0%84%EC%86%A1%EB%B0%A9%EC%8B%9D.md)
- [CDN](Question/2025/0725-CDN.md)
- [UDP의 지연과 신뢰성](Question/2025/0725-TCP%2CUDP.md)
- [넷플릭스 홈과 스트리밍](Question/2025/0822-%EB%84%B7%ED%94%8C%EB%A6%AD%EC%8A%A4.md)

## 개발 방법과 읽을거리

- [주니어 백엔드 실무 지식](Book/2025/%EC%A3%BC%EB%8B%88%EC%96%B4%20%EB%B0%B1%EC%97%94%EB%93%9C%20%EA%B0%9C%EB%B0%9C%EC%9E%90%EA%B0%80%20%EB%B0%98%EB%93%9C%EC%8B%9C%20%EC%95%8C%EC%95%84%EC%95%BC%20%ED%95%A0%20%EC%8B%A4%EB%AC%B4%20%EC%A7%80%EC%8B%9D.md)
- [바이브 코드 너머 개발자 생존법](Book/2026/%EB%B0%94%EC%9D%B4%EB%B8%8C%20%EC%BD%94%EB%93%9C%20%EB%84%88%EB%A8%B8%20%EA%B0%9C%EB%B0%9C%EC%9E%90%20%EC%83%9D%EC%A1%B4%EB%B2%95.md)
- [Claude Code 완전 가이드](Book/2026/Claude%20Code%20%EC%99%84%EC%A0%84%20%EA%B0%80%EC%9D%B4%EB%93%9C%20-%20%ED%95%B4%EC%BB%A4%ED%86%A4%20%EC%9A%B0%EC%8A%B9%EC%9E%90%EC%9D%98%2070%EA%B0%80%EC%A7%80%20%ED%8C%8C%EC%9B%8C%20%ED%8C%81.md)
- [Git fetch·pull과 rebase](Question/2025/0728-Git.md)
- [단위 테스트](Video/2025/0722-%EB%8B%A8%EC%9C%84%ED%85%8C%EC%8A%A4%ED%8A%B8.md)
- [개발자의 QA 책임](Video/2025/0730-QA.md)
- [인프런 아키텍처 변화](Video/2025/0723-%EC%9D%B8%ED%94%84%EB%9F%B0%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98%EB%B3%80%ED%99%94.md)
- [조직 구조와 레거시 개편](Video/2025/0727-%EC%9D%B8%ED%94%84%EB%9F%B0%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98%EB%B3%80%ED%99%94.md)
- [인프런 2023 아키텍처](Video/2025/0729-%EC%9D%B8%ED%94%84%EB%9F%B0%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98%EB%B3%80%ED%99%94.md)
