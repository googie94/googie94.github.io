---
layout: post
author: "googie"
title:  "Mac(맥)에서 업무 생산성을 책임질 앱과 단축키 모음"
subtitle: "맥북 기본 메모 앱 Notes | 단축키 생성 앱 HammerSpoon"
type: "Tool"
tech: true
text: true
post-header: false
order: 4
comments: false
draft: true
subjects: ["책상 위 움직임 최소화", "Notes(메모)"]
lastmod: 2023-03-16 23:00:00
---

<p>
	<a name="subject1"></a>
</p>

### 책상 위 움직임 최소화

이 글을 보는 당신은 단축키를 잘 활용하시나요?  아마 복사, 붙여넣기 같은 기본적인 기능만 쓰고 있진 않나요? 그마저도 마우스를 활용해서 하는 경우도 본 것 같아요.
마우스와 키보드를 오가기를 반복하는게 저는 왜 이렇게 번거로울까요?

그래서 준비했어요. Mac(맥)에서 유용한 맥의 단축키를 소개해요!
<br />


<p>
	<a name="subject2"></a>
</p>

### Notes(메모)


저는 메모 앱으로 하루를 시작해요. 하루 체크리스트를 만들고 하나씩 지워가는 맛, 아시나요?

많은 메모 앱 중에 가장 많이 활용하는 건 단연 Notes(메모) 앱이 아닐까 싶어요.
<br/>제가 가장 많이 사용하는 단축키를 정리할게요.

<br/>

| 행동 | 단축키 |
| :---: | :---: |
| 새 메모 생성 | `Command + N` |
| 새로운 메모 생성 | `Cmd + N` |
|새로운 폴더 생성 | `Shift + Cmd + N` |
|모든 메모 검색 | `Opt + Cmd +F` |
|사이드바, 메모 목록 이동 | `Ctl + Tab` |
|파일 첨부 | `Shift + Cmd + A` |
|링크 생성 | `Cmd + K` |
|표 생성 | `Opt + Cmd + T` |
|머리말 생성 | `Shift + Cmd + H` |
|부 머리말 생성 | `Shift + Cmd + J` |
|체크리스트 생성 | `Shift + Cmd + L` |
|체크리스트 항목 표시 해제 | `Shift + Cmd + U` |
|메모 콘텐츠 확대 | `Shift + Cmd + >` |
|메모 콘텐츠 초기화 | `Shift + Cmd + 0` |
|가운데 정렬 | `Shift + Cmd + |` |
|왼쪽 정렬 | `Shift + Cmd + {` |
|오른쪽 정렬 | `Shift + Cmd + }` |
|메모 목록 탭으로 가기 | `Cmd + Return` |

<br/>

---


<p>
	<a name="subject2"></a>
</p>

### HammerSpoon


기본 단축키로는 마우스로 이동을 반복해야만 했어요. 내가 단축키를 만들 순 없을까?
<br />'이런거 없을까?' 싶으면 항상 있어요. 사람 생각하는건 다 비슷한가봐요.
[기계인간 john-grib 블로그](https://johngrib.github.io/wiki/hammerspoon/)를 보고 처음 알게됐어요.


[Hammerspoon](http://www.hammerspoon.org/)을 소개할게요.
>- 맥에서 돌아가는 매크로 툴이예요.
- 윈도우 사이즈 변환이나, 단축키 등록이 편리하고 자유로워요.
- 특정 앱이 실행될 때 이어서 다른 작업을 수행하게 할 수도 있어요.

<br/>

**단축키 등록**


메모 앱을 단축키로 등록했는데요. 해머 스푼에는 맥 OS API가 래핑 되어 있어 간단히 앱을 매핑 시킬 수 있어요.
아래 코드를 확인해주세요. 간단하지 않나요?

```lua
function note()
    hs.application.launchOrFocus('Notes')
end

...

hs.hotkey.bind({'ctrl', 'cmd'}, 'N', note)
```

