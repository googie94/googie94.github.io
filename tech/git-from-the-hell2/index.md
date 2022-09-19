---
layout: post
author: "googie"
title:  "지옥에서 온 GIT(2)"
subtitle: "GIT의 원리"
type: "Git"
tech: true
text: true
post-header: true
header-img: "img/header.jpg"
portfolio: true
sentence: 이 글은 생활코딩 이고잉님의 '지옥에서 온 GIT' 강의를 보고 작성하였습니다.
source: https://opentutorials.org/
order: 7
comments: true
draft: false
---


`git init` 을 하게 되면, `.git` 이라는 숨김폴더가 생성된다.
`.git`을 파헤친다는 건 git을 이해하는 데에 핵심적인 요소가 된다.

```
>>> git init
>>> ls -al
total 48
drwxr-xr-x  13 googie  staff  416  9 19 08:48 .
drwxr-xr-x   5 googie  staff  160  9 19 08:19 ..
-rw-r--r--   1 googie  staff    2  9 19 08:16 COMMIT_EDITMSG
-rw-r--r--   1 googie  staff   23  9 19 07:22 HEAD
-rw-r--r--   1 googie  staff   41  9 19 08:19 ORIG_HEAD
-rw-r--r--   1 googie  staff  137  9 19 07:22 config
-rw-r--r--   1 googie  staff   73  9 19 07:22 description
drwxr-xr-x  13 googie  staff  416  9 19 07:22 hooks
-rw-r--r--   1 googie  staff  209  9 19 08:20 index
drwxr-xr-x   3 googie  staff   96  9 19 07:22 info
drwxr-xr-x   4 googie  staff  128  9 19 07:43 logs
drwxr-xr-x  18 googie  staff  576  9 19 08:16 objects
drwxr-xr-x   4 googie  staff  128  9 19 07:22 refs

--- git-from-the-hell
 |_ .git
      |- COMMIT_EDITMSG
      |- HEAD
      |- ORIG_HEAD
      |- config
      |- description
      |- hooks
      |- index
      |- info
      |- logs
      |- objects
      |_ refs

>>> f1.txt // 파일변경
>>> git add f1.txt

===
`git add`를 하게 되면, `index`와 `objects`파일에 변동이 생긴다.
<index>
100644 399f861302bf788241ec257bb2c1180db04d1c 0 f1.txt

<objects>
objects/39/399f861302bf788241ec257bb2c1180db04d1c
a
f1.txt
===



>> vim f2.txt // 파일 추가 및 변경
>> git add f2.txt

===
index objects 파일이 추가/변경 되었다.
<index>
100644 399f861302bf788241ec257bb2c1180db04d1c 0 f1.txt
100644 68a0e7f97cb9b365c1f731af9f88e6191f5627 0 f2.txt

<objects>
objects/39/68a0e7f97cb9b365c1f731af9f88e6191f5627
z
f2.txt
===


>>> cp f1.txt f3.txt // 파일 복사
>>> git add f3.txt

===
index objects 파일이 추가/변경 되었다.
파일의 이름이 달라도, 파일의 내용이 같다면 같은 오브젝트 파일을 가르킨다.
<index>
100644 399f861302bf788241ec257bb2c1180db04d1c 0 f1.txt
100644 68a0e7f97cb9b365c1f731af9f88e6191f5627 0 f2.txt
100644 399f861302bf788241ec257bb2c1180db04d1c 0 f3.txt

<objects>
objects/39/399f861302bf788241ec257bb2c1180db04d1c
a
f1.txt, f3.txt
===



```

- 파일을 추가한 것으로는 git은 아무런 동작을 하지 않는다.
- `git add`를 하게 되면, `index`와 `objects`파일에 변동이 생긴다.
- ***파일의 이름이 달라도, 파일의 내용이 같다면 같은 오브젝트 파일을 가르킨다.***


<br>

## objects 파일의 원리

`objects` 파일은 특이한 파일명을 가지고 있다.
이는 `sha1` 이라는 해쉬 알고리즘을 통과해 나오는 앞글자의 2글자를 따서 만들게 된다.


```
===
>> hello
>> aaf4c61ddcc5e8a2dabede0f3b482cd9aea9434d
===

--- objects
 |- aa 
 |   |- f4c61ddcc5e8a2dabede0f3b482cd9aea9434d - hello
 |- info
 |_ pack

```

## commit의 원리

```
>> git commit
===
다섯개의 파일에 변화가 생겼다.
- COMMIT_EDITMSG
- logs/HEAD
- log/refs/heads/master
- objects/aa/f4c61ddcc5e8a2dabede0f3b482cd9aea9434d
- refs/heads/master
===

# objects/aa/f4c61ddcc5e8a2dabede0f3b482cd9aea9434d
tree ae4dfc877e5bc7b05019a3ef86c3e4fa79e9e193
author googie94 <googie94@gamil.com> <시간정보>
committer googie94 <googie94@gamil.com> <시간정보>

[tree]ae4dfc877e5bc7b05019a3ef86c3e4fa79e9e193
100644 399f861302bf788241ec257bb2c1180db04d1c 0 f1.txt
100644 68a0e7f97cb9b365c1f731af9f88e6191f5627 0 f2.txt
100644 399f861302bf788241ec257bb2c1180db04d1c 0 f3.txt

>> vim f2.txt // 내용 변경
>> git commit

# objects/aa/f4c61ddcc5e8a2dabede0f3b482cd9aea9434d
tree 0d55fcb9ac29544ab31595695962e974ad2bd255
parent ae4dfc877e5bc7b05019a3ef86c3e4fa79e9e193 // 이전 커밋
author googie94 <googie94@gamil.com> <시간정보>
committer googie94 <googie94@gamil.com> <시간정보>

[tree]0d55fcb9ac29544ab31595695962e974ad2bd255
100644 399f861302bf788241ec257bb2c1180db04d1c 0 f1.txt
100644 b5b380c2eb5ef559d206a5b896a50df716ec13 0 f2.txt
100644 399f861302bf788241ec257bb2c1180db04d1c 0 f3.txt
```

- 각각의 버전마다 다른 트리값을 가지고 있다(스냅샷).
- objects 파일은 크게 blob, tree, commit 세 가지로 구분되어 있다.


<br>

## status의 원리

```
>>> f2.txt // 파일 내용 변경 1 -> 2

100644 b5b380c2eb5ef559d206a5b896a50df716ec13 0 f2.txt
1

===
index에 담겨진 f2.txt 의 내용과 현재 파일의 내용이 다르면 "수정되었다"라고 판단한다.
===


>>> git add f2.txt
<add 이후 index 파일과 내용>
*** 100644 b5b380c2eb5ef559d206a5b896a50df716ec13 0 f2.txt *** 
2

<최신 커밋의 f2.txt 파일과 내용>
*** 100644 b5b380c2eb5ef559d206a5b896a50df716ec13 0 f2.txt ***
1
===
최신 커밋의 f2.txt의 내용과 index의 내용이 다르면 "add 되었다" 라고 판단한다.
===

>>> git commit
tree 0d55fcb9ac29544ab31595695962e974ad2bd255
parent ae4dfc877e5bc7b05019a3ef86c3e4fa79e9e193 // 이전 커밋
author googie94 <googie94@gamil.com> <시간정보>
committer googie94 <googie94@gamil.com> <시간정보>

[tree]0d55fcb9ac29544ab31595695962e974ad2bd255
100644 399f861302bf788241ec257bb2c1180db04d1c 0 f1.txt
100644 b5b380c2eb5ef559d206a5b896a50df716ec13 0 f2.txt
> 2
100644 399f861302bf788241ec257bb2c1180db04d1c 0 f3.txt

<index>
100644 b5b380c2eb5ef559d206a5b896a50df716ec13 0 f2.txt
> 2

<로컬>
f2.txt
> 2

===
최신 커밋된 파일의 내용, index의 파일의 내용, 현재 로컬 파일의 내용이 같으면 "commit 할 것이 없다"라고 
===
```

- index에 담겨진 f2.txt 의 내용과 현재 파일의 내용이 다르면 "수정되었다"라고 판단한다.
- 최신 커밋의 f2.txt의 내용과 index의 내용이 다르면 "add 되었다" 라고 판단한다.
- 최신 커밋된 파일의 내용, index의 파일의 내용, 현재 로컬 파일의 내용이 같으면 "commit 할 것이 없다"라고 판단한다.



<br>

## The Working Space, Staging Area, and Local Repo

![작동원리](img/git_working.png)
- workspace: 프로젝트 폴더(git-from-the-hell 폴더)
- `add` 명령어를 실행하면 `index (staging area)` 파일을 생성한다.
- `commit` 명령어를 실행하면 `로컬 저장소` 에 `objects` 폴더에 트리와 파일이 저장된다.


