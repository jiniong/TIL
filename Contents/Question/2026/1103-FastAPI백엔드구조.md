# FastAPI 백엔드의 구조와 Spring의 대응 관계

- 분류: 질문
- 기록일: 2026-11-03
- 갱신일: 2026-11-03
- 주제: Python, FastAPI, 라우터, 서비스, SQLAlchemy, 배포
- 출처: PAAS 백엔드 구조 질문과 snuhai-paas의 app/main.py·app/routers/store_api.py·app/store/favorites.py·app/deps.py·pyproject.toml·Dockerfile 소스 확인
- 상태: 소스 확인과 개념 정리

## 목차

- [질문 1](#질문-1)
- [답변 1](#답변-1)
- [질문 2](#질문-2)
- [답변 2](#답변-2)
- [질문 3](#질문-3)
- [답변 3](#답변-3)
- [업데이트 기록](#업데이트-기록)

## 질문 1

Python 백엔드는 원래 main.py·routers·models 같은 구조인가?

## 답변 1

Python 언어가 이 폴더 구성을 강제하지 않는다. 이 프로젝트가 FastAPI·SQLAlchemy·Alembic을 조합해 선택한 구성이다.

| PAAS 구성 | 역할 | Spring에서 가까운 개념 |
| --- | --- | --- |
| main.py의 create_app | 앱 생성, 라우터·미들웨어·시작/종료 처리 연결 | 애플리케이션 진입점과 설정 |
| routers | HTTP 요청과 응답 처리 | Controller |
| store/favorites.py 등 기능 모듈 | 사용자별 규칙과 업무 처리 | Service |
| models.py | SQLAlchemy 테이블·ORM 모델 | Entity |
| schemas.py | Pydantic 요청·응답 모델 | DTO와 검증 |
| deps.py의 get_session | 요청에 쓸 DB 세션 공급 | 의존성 주입 |
| migrations | Alembic 스키마 변경 이력 | Flyway/Liquibase와 비슷한 역할 |

이는 개념을 비교한 것이며 각 프레임워크의 구현이 같다는 뜻은 아니다. 이 프로젝트에서는 별도 Repository 클래스 없이 기능 모듈이 SQLAlchemy Session으로 조회·저장하기도 한다. 모든 업무 처리를 Service 클래스로 감쌀 필요도 없다.

즐겨찾기 PUT의 실제 흐름은 main.py의 라우터 등록 → routers/store_api.py의 favorite_app → store/favorites.py의 add → UserFavoriteApp 저장 → 라우터의 session.commit이다.

## 질문 2

Python이라서 이미지 tar로 배포하는가?

## 답변 2

tar 반입은 Python 고유 방식이 아니라 폐쇄망으로 컨테이너 이미지를 전달하는 방식이다. 이 프로젝트의 Dockerfile은 Python 런타임·앱 소스·의존성을 이미지에 넣고 Uvicorn으로 FastAPI를 실행한다. Java 서버는 같은 이미지 전달 방식을 쓰면서 이미지 안에 JVM과 JAR 등을 넣을 수 있다. 언어의 실행 구성과 이미지의 전달 방식을 구분한다.

## 질문 3

Django 구조와도 다른가?

## 답변 3

Django는 프로젝트 설정(settings.py)·URL 설정(urls.py)과 기능별 앱을 중심으로 구성한다. Django의 view는 화면 HTML 파일이 아니라 URL에 연결된 요청 처리 함수나 클래스다. HTML 표현은 template이 맡는다. JSON 응답을 반환할 때는 template을 거칠 필요가 없다.

현재 PAAS의 라우터 핸들러는 Django view의 요청 처리 역할과 가깝다. Django에서도 업무 로직이 커지면 별도 services.py 같은 모듈로 분리할 수 있으므로 views.py에 모든 로직을 넣어야 하는 것은 아니다. 파일 배치와 ORM 도구가 달라도 HTTP 요청을 받아 권한·업무 규칙을 적용하고 DB를 읽거나 저장한 뒤 응답한다는 흐름은 같다.

- Django: urls.py → views.py → 업무 로직·Django ORM → JSON 또는 template 응답
- 현재 PAAS: 등록된 router → 요청 핸들러 → 기능 모듈·SQLAlchemy → JSON 또는 Jinja2 응답

근거: [Django 프로젝트·앱 구성 튜토리얼](https://docs.djangoproject.com/en/5.2/intro/tutorial01/), [Django의 view·template·MTV 설명](https://docs.djangoproject.com/en/5.2/faq/general/#django-appears-to-be-a-mvc-framework-but-you-call-the-controller-the-view-and-the-view-the-template-how-come-you-don-t-use-the-standard-names).

## 업데이트 기록

- 2026-11-03: PAAS의 실제 요청 흐름과 파일을 기준으로 FastAPI 구성 및 Spring 대응 관계를 정리.
- 2026-11-03: Django의 프로젝트·앱·view·template 구성을 공식 문서와 비교하고 요청 처리 역할의 대응 관계를 보완.
