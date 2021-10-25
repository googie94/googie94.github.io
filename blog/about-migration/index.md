---
layout: post
title:  "Django Migration에 대해서"
subtitle: "DB를 스키마에 적용시키는 수단"
type: "TIL"
blog: true
text: true
author: "Googie"
post-header: true
header-img: "img/header.jpg"
order: 2
---

장고 공식문서와 여러 블로그를 참고했습니다.

# Migration이 뭐야?

> 장고의 공식문서에 따르면 `migration`은 모델에 대한 변경 사항(추가, 삭제, 변경 등)을 데이터베이스 스키마로 전달해주는 Django의 수단이라고 적혀있습니다.

migration은 장고에서 DB와 직접적으로 연결되는 중요한 파일입니다.
models.py에서 선언한 모델을 migration으로 바꿔주고 migrate할 때 sql로 바꿔 실제 DB에 적용합니다.
파이썬으로 선언된 모델을 sql쿼리로 변경하기 위한 중간다리라고 생각하면 될 것 같습니다.

## 명령어

- migrate: 마이그레이션 적용 및 적용 취소
- makemigrations: 모델에 대한 변경 사항을 기반으로 새 마이그레이션 생성을 담당
- sqlmigrate: 마이그레이션에 대한 SQL 문을 표시
- showmigrations: 프로젝트의 마이그레이션 및 상태를 나열

```
python manage.py <명령어> + <app-name> + <migration-name>


예시) python manage.py migrate coupon 0001
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

작성중입니다.
