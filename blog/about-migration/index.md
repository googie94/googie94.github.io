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
python manage.py <명령어> + <app name> + <number>


예시) python manage.py migrate coupon 0001
```

### 오늘 있었던 일과 해결방법을 적어보자




## 레퍼런스

검색하다가 자주 봤던 블로그인데, 역시나 TIL을 실천 중이었고, TIL의 회고에 대한 글도 있다.

[초보몽키의 개발블로그 - 6개월 간의 회고 🔗](https://wayhome25.github.io/til/2017/08/14/TIL-for-6-months/)
