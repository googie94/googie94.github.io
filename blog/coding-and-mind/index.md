---
layout: post
author: "googie"
title:  "코딩과 마음"
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

### 코딩과 조건과 마음

`Clean Code(클린 코드) - 로버트 C. 마틴`를 보면 중첩된 조건문의 사용을 지양하고 있습니다.
조건문이 중첩되어 길어지게 되면, 코드를 읽어 나가기가 쉽지 않기 때문입니다.

```python
# 생각에 생각에 생각...
if thought:
  if thought:
    if deep_thought:
      ...
 ```

<br>
생각에 생각에 생각... `"내가 무슨 생각을 하고 있었던 거지?"` `"그래서 어떻게 해야 하는건데?"` `"나 잘하고 있나?"` 
물방울만한 고민이 생각에 생각을 거듭해 결국 눈덩이처럼 커져 있곤 합니다.

```python
# 생각 -> 행동, 생각 -> 행동...
if thought:
  return action

if thought:
  return action

if deep_thought:
  return action
 ```

클린코드 저자는 `조건문은 return과 함께` 사용하길 권장합니다.
분기가 나눠져 읽기도 편하고, 결과를 예상하기도 쉽습니다.

<br>
지나서 생각해보면 고민하기보단 일단 행동해보면 한 발 나아가 있는 나를 발견할 수 있었습니다.
고민하고 행동으로, 그리고 다시 고민하고 행동으로. 
그렇게 반복하다보면 분명 어제보다 더 나은 오늘이 되어 있지 않을까 생각합니다.
