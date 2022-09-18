---
layout: post
author: "googie"
title:  "코드를 보다가 문득"
subtitle: "사람이 만든 기술엔 사람이 보인다"
type: "Blog"
blog: true
text: true
post-header: true
header-img: "img/header.jpg"
order: 1
comments: true
draft: false
---

`Clean Code(클린 코드) - 로버트 C. 마틴` 에서는 중첩된 조건문의 사용을 지양하고 있다.
조건문이 중첩되어 길어지게 되면, 코드를 읽어 나가기가 쉽지 않기 때문이다.

```python
# 생각에 생각에 생각...
if thought:
  if thought:
    if deep_thought:
      ...
 ```

<br>
`"내가 무슨 생각을 하고 있었던 거지?" "그래서 어떻게 해야 하는건데?" "나 잘하고 있나?"`
물방울만한 고민이 결국 눈덩이처럼 커졌었다.

```python
# 생각 -> 행동, 생각 -> 행동...
if thought:
  return action

if thought:
  return action

if deep_thought:
  return action
 ```

클린코드 저자는 조건문은 return과 함께 사용하길 권장한다.
분기가 나눠져 읽기도 편하고, 결과를 예상하기도 쉽다.



고민하고 행동으로, 그리고 다시 고민하고 행동으로.
그렇게 반복하다보면 분명 어제보다 더 나은 오늘이 되어 있지 않을까 생각한다.
