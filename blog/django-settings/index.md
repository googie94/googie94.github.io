---
layout: post
title:  "TwoScoopsOfDjango - Django settings & requirements"
subtitle: "5장 장고 프로젝트 setting.py & requirements.txt 설정"
type: "TIL"
blog: true
text: true
author: "Googie"
post-header: true
header-img: "img/header.jpg"
order: 2
---

서버의 세팅들은 서버가 시작될 때 적용되며, 세팅값을 새로운 값으로 변경하기 위해서는 서버를 재시작 해야 한다. 개발자가 서비스를 운영중에 임의로 변경할 수 없다.

# 최선의 장고 세팅 가이드

> 프로젝트가 커지며, 장고의 세팅 항목들도 점차 복잡해지기 시작한다.

* 모든 설정 파일은 버전 컨트롤 시스템으로 관리되어야 한다.
* 반복되는 설정들을 없애야 한다.
* 암호나 비밀 키는 안전히 보관해야 한다.

## 모든 설정 파일은 버전 컨트롤 시스템으로 관리되어야 한다.

> 특히 운영환경에서 중요. 날짜, 시간 등의 세팅 변화에 대한 기록이 반드시 문서화 되어야 한다.

개발을 위해 개발자에게만 필요한 환경이 존재한다. 스테이징과 운영환경에 따라 다른 설정들이 비활성화되어 있거나 아예 설치조차 되지 않은 디버그 도구(debug_tool_bar)가 그렇다.
또한 SECRET_KEY 같은 비밀 정보는 보안을 위해서 저장소에서 빼야 한다.

일반적 해결은 local_settings.py와 같은 모듈을 만들어 해당 서버나 개발머신에 위치 시킴으로써, 이 파일을 버전 관리 시스템에서 제외시키는 방법이 있다.
하지만,
* 모든 머신의 버전 컨트롤에 등록되지 않는 코드가 존재할 수 있다.
* 운영 환경과 개발 환경이 벌어지게 되어, 문제의 원인을 찾기 힘들다.
* 여러 팀이 서로의 local_settings.py를 공유하는 경우, 이는 같은 일을 반복하게 되는 문제와 서로 다른 환경에서 작업하게 될 수 있다.

우리는 이러한 문제를 해결하기 위해, 개발 환경, 스테이징 환경, 테스트 환경, 운영 환경 설정을 공통되는 객체로부터 상속받아 구성된 서로 다른 세팅 파일로 구분지어 관리한다.
그런 다음 서버의 암호 정보 등을 버전 컨트롤에서 빼내어 관리한다.

## 여러 개의 settings 파일 이용하기

처음 장고의 기본 설정은 configuration_root에 settings.py가 세팅되어 있다.
한 개의 settings 를 이용하는 것이 아닌, settings 디렉터리를 생성해, 아래에 여러 개의 셋업 파일을 구성해 사용하는 것을 추천한다.

```python
settings/
 ├── __init__.py
 ├── base.py # 프로젝트의 모든 인스턴스에 적용되는 공용 세팅 파일
 ├── local.py  # 로컬 환경에서 작업할 때 쓰이는 파일. 디버그모드, 로그 레벨, 디버그 도구 활성화 등이 설정되어있다
 ├── staging.py # 스테이징 서버는 운영 서버에서 구동되는 세미 프라이빗 버전이다. 운영 환경으로 코드가 이동되기 전에 관리자 및 고객들의 확인을 위한 시스템이다
 ├── test.py  # 테스트 실행기, 인메모리 데이터베이스 정의, 로그 세팅 등을 포함한 테스트를 위한 세팅
 ├── production.py # 운영 서버에서 실제로 운영되는 세팅 파일
 ├── ci.py # 지속적 통합에 쓰이는 ci.py 파일이 필요할 수 있다. 큰 큐모에서 특수한 목적을 가진 서버가 있을 경우엔 각 목적에 맞는 커스텀 세팅 파일을 만들어쓰면 된다\
 ```

이렇게 나뉜 설정 파일을 이용해, shell 혹은 runserver를 해보면,
```python
python manage.py shell --settings=twoscoops.settings.local # 장고/파이썬 shell 실행
python manage.py runserver --settings=twoscoops.settings.local # 로컬 개발 서버 구동
```






## 오늘 있었던 일

로컬에서 작업할 때, DB를 Dump해서 로컬DB에 연결해 테스트 환경을 만들곤 하는데, 연결한 것으로 착각하고 지금까지 작업 내용을 Migrate까지 해버렸다.
구글링을 한 결과, 방법을 정리할 수 있었다.

상황1. DB가 없거나 삭제해도 괜찮은 경우


- 마이그레이션을 초기화하고 다시 재생성한다.


상황2. 중요한 DB가 있어, 삭제할 수 없는 경우

python manage.py migrate app-name zero
Django 모델 - django_migration 에서 삭제


-  마이그레이션 적용을 해제한 후, DB를 적용한다

python manage.py migrate --fake app-name 0001


## 주의하자!

마이그레이션을 할 때에는 `app-name` 을 쓰는 습관을 들이자  원치 않는 마이그레이션이 적용될 수 있기 때문이다


## 레퍼런스

작성중입니다
