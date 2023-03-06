---
layout: post
author: "googie"
title:  "클린코드"
subtitle: "소개, 코드 포매팅과 도구"
type: "python"
tech: true
text: true
post-header: true
header-img: "img/header.jpg"
order: 1
comments: true
draft: false
lastmod: 2023-03-04 2:00:00
---

클린코드는 포매팅 이상의 훨씬 중요한 것을 의미한다. 때문에 표준 포매팅을 유지하는 것이 유지 보수성의 핵심 유의 사항이다.
파이썬이 제공하는 기능을 사용하여 자체 문서화된 코드를 작성하는 방법과 코드의 레이아웃을 일정하게 유지하는 도구를 설정하는 방법에 대해 알아본다.

### 클린코드

본질은 "프로그래밍 언어의 진정한 의미는 아이디어를 다른 개발자에게 전달하는것"
다른 엔지니어가 코드를 읽고 유지 관리할 수 있는지의 여부에 달려있다.


### 클린코드는 PEP-8?

클린코드를 생각하면 기본 포맷팅, 린팅도구 혹은 다른 검사 도구를 사용한 레이아웃의 통일을 생각하는 경우가 있다. 코드를 올바르게 포매팅하는 것은 물론 중요하지만 클린코드는 그 이상의 의미가 있다.
클린코드는 품질좋은 소프트웨어를 개발하고, 견고하고 유지보수가 쉬운 시스템을 만들고, 기술부채를 회피하는 것에 의미가 있다.

포매팅은 코드의 '검색을 쉽게', '읽기 쉽게' 만들어 코드의 품질을 높이는 데에 도움을 준다.


### Docstring과 어노테이션

```python
def googie(params):
	"""
	Docstring
	params -> int or None
	"""
	if params == 30:
		# 주석
		# params는 나이
		# 20대를 반환
		return 20
	return None
```

```python
>>> help(googie)
```
```python
Help on function my:

googie(params)
	Docstring
	params -> int or None
```

docstring은 코드의 일부로, 객체에 docstring이 정의되어 있으면 doc속성을 통해 접근이 가능하다.
```python
>>> googie.__doc__
```
```python
>>> Docstring
>>> params -> int or None
```


주석과 Docstring 은 다르다.
Docstring은 코드의 특정 컴포넌트(모듈, 클래스, 메서드)에 대한 문서화이다.

코드에 주석을 다는 행위는 여러 이유로 나쁜 습관이다.
결국, 코드로 아이디어를 제대로 표현하지 못했음을 나타내며, 주석이 현재와 일치하지 않을 경우 또 다른 위험을 초래할 수 있다.

docstring은 지속적으로 수작업으로 업데이트해줘야 한다는 단점이 있다. 소프트웨어는 단순한 코드가 아니기 때문에, 문서는 산출물에 함께 포함되어 있어야 한다.


### 어노테이션

코드 사용자에게 함수 인자로 어떤것을 주어야하고, 받을 수 있는지 힌트를 주는 행위이다.
힌트는 힌트일뿐, 강제하지 않는다.

```python
class Point:
	def __init__(self, lat, long):
		self.lat = lat
		self.long = long

def locate(latitude: float, longitude: float) -> Point:
	"""맵에서 좌표에 해당하는 객체 검색"""
	pass
```
인자로 float 타입으로 받고, Point 클래스 객체를 반환한다.

```python
>>> locate.__annotations__
{'latitude': <class 'float'>, 'longitude': <class 'float'>, 'return': <class 'Point'>}
```

__annotation__ 속성을 이용해 문서 생성, 유효성 검증, 타입 체크 등을 할 수 있다. 이외에도 변수의 의도를 설명하는 문자열, 콜백이나 유효성 검사 함수로 사용할 수 있는 callable 등을 사용할 수 있다.


### docstring을 사용한 더 나은 설명

```python
def data_from_response(response: dict) -> dict:
	if response["status"] != 200:
		raise ValueError
	return {"data": response["payload"]}
```

```python
def data_from_response(response: dict) -> dict:
	"""response에 문제가 없다면 response의 payload를 반환
	-response 사전의 예제:
	{
		"status": 200, # <int>
		"timestamp: "....", # 현재 시간 ISO 포맷 문자열
		"payload": { ... } # 반환하려는 사전 데이터
	}

	- 반환 사전의 예제:
	{
		"data": { .. }
	}

	- 발생 가능한 예외:
		HTTP status가 200이 아닌 경우 ValueError 발생
	"""
	if response["status"] != 200:
		raise ValueError
	return {"data": response["payload"]}
```

이를 통해서 입출력 값을 더 잘 이해하게 되고, 단위테스트에서도 유용한 정보로 사용될 수 있다.
효과적인 문서가 되려면 보다 상세한 정보가 필요하다.


### 도구 설정

코딩 스타일이나 가이드라인을 준수하는 것은 여러가지로 중요하다. 이는 필수적이지만 충분하지는 않은 조건이다.
팀 프로젝트가 준수해야하는 최소한의 요구사항으로 도구를 활용하는 것이 좋다.

Black, Pylint, Mypy 와 같은 도구들을 에디터나 IDE에 통합하여 작업을 수월하게 만들 수 있다.
파일을 저장함과 동시에 수정작업을 설정시키게 할 수 있다.
