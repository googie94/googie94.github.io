---
layout: post
author: "googie"
title:  "프로그래머를 위한 파이썬(2)"
subtitle: "추상화와 캡슐화"
type: "Python"
tech: true
text: true
post-header: true
header-img: "img/python_pro.png"
order: 7
comments: false
draft: false
lastmod: 2023-07-06 22:00:00
---

역할: 심화지기


### 코드 리팩터링

팀리그 순위 부분을 관심사 분리하여 캡슐화 시키는 코드의 리팩터링을 요청했다.

```python
# 코드 리뷰 이후
def rank(self, request):
    from league.utils import RankService

    rank = RankService.get_rank()
    return Response(rank)


# 서비스 레이어로 분리했다.
# 1. 함수 내 쓰일 변수와 기본값을 정의한다.
# 2. 받아온 값을 정의한다.
# 3. redis를 조회한다.
#    a. redis에 저장된 값을 가져온다
# 4. DB에서 순위 테이블을 가져온다.
# 5. 순위 테이블을 필터링한다.
# 6. 순위 테이블을 가공한다.
# 7. 순위 테이블을 공식과 비공식 순위로 구분한다.
# 8. 결과값을 redis에 저장한다.
class RankServices:
    """
    리그 관련 순위
    """
    def __init__(
        self,
        variable=value
    ):
        self.variable = value

    @property
    def __get_redis_key(self):
        """get redis key"""
        return self._key

    @property
    def __redis_connect(self):
        """redis connect"""
        return redis.Redis(...)

    def __look_up_rank_in_redis(self) -> bool:
        """redis search"""
        if self.__redis_connect().zcount(...):
            return True
        return False

    def __filtered_ranks(self, rank_lst: Queryset) -> Queryset:
        """queryset filter"""
        return rank_lst.filter(...)

    def __ranks_with_point(self, rank_lst: Queryset) -> Queryset:
        """get useable rank"""
        return rank_lst.annotate()

    def __get_ranks_in_redis(self) -> list:
        """get rank in redis"""
        redis_lst = self.redis_connect().zrevrange(...)
        return redis_lst

    def __set_ranks_in_redis(self, Queryset: Queryset) -> None:
        """set rank in redis"""
        self.redis_connect().add(...)

    def __get_rank_in_db(self) -> list:
        """get rank in db"""
        return self.__ranks_with_season_point.filter(...)

    def get_rank(self) -> object:
        """get rank"""
        if self.__look_up_rank_in_redis:
            rank = self.get_ranks_in_redis()
        else:
            rank = self.__get_rank_in_db()
            self.__set_ranks_in_redis(rank)

        return rank
```

### 피드백
- 공개가 필요한 함수와 아닌 함수를 구분해야 한다 .
- 메모리 관련 추상화된 패키지가 필요하지 않을까?
    - 퍼블릭한 인터페이스에 저수준의 redis가 밀접하게 연결되어 있다. 변경에 취약하지 않을까
- @property의 사용은 두가지 경우가 있는 것 같다.
    - Getter Setter 개념
    - 간단히 쓰고 싶은
- classmethod에서는 self 말고 cls


### 심화: Getter와 Setter

Q. 게터/세터는 캡슐화를 방해하는가?
> 객체는 공용 인터페이스를 통해서만 외부와 소통하는데요 프라이빗 클래스 변수 선언이 자동으로 게터/세터를 만들어 버린다면
과연 캡슐화/정보 은닉이 되고 있다고 말할 수 있을까요? 게다가 마음이 바뀌어 클래스 변수를 제거할 때 게터/세터가 적용된 모든 곳의 코드 변경이 불가피해지는데, 강한 결합을 일으킨다는 반증이 아닐까요?

Q. 장고 models.py가 객체의 지위를 위태롭게 한다고 생각했어요.
> 플랩의 프로젝트 내 A모델은 모델 메서드로 B모델의 속성을 가져오는데요. 우리가 배운 객체(지향) 개념으로 보면
공용 인터페이스로 내부 속성의 공개 여부를 결정하는 주체는 B객체여야 하는데요. A가 그걸 결정해버린다는 거죠. 캡슐화를 위배한다고 느꼈다고 해요. 하지만 ORM(Object-Relation Mapping) 특성상 어쩔 수 없다는 의견이 있었어요<br /><br />
1) 엔티티의 연결성을 표현하는 RDBMS의 아이디어와<br />
2) 캡슐화를 지향하는 객체<br /><br />를 매칭시키다보니 태생적으로 개념적 충돌이 발생한다는 것인데요.
이 주제도 시간을 두고 좀 더 고민해보기로 했어요

