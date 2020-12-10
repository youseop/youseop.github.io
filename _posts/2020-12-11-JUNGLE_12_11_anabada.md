---
layout: post
title: JUNGLE#1 아나바다ㅓㅕ
subtitle: Python
cover-img: /assets/img/posting/mountain.jpg
thumbnail-img: /assets/img/posting/jungle.png
share-img: /assets/img/posting/mountain.jpg
tags: [기록🎉, Jungle, WEB]
comments: true
---

## 아나바다

<br>

[Go to git repository](https://github.com/youseop/ananbada_second/blob/main/README.md)

12월 9일부터, 12월 11일 13시까지 홍지님, 민규님과 함께 아나바다 서비스를 만들었다.

에브리타임에서 장터게시판에 계산기, 빔프로젝터 그리고 청소기 등을 빌린다는 게시글들을 보고 생각한 아이디어였다.

![Crepe](https://i.imgur.com/fYKBi1i.jpg)

<br>

- '빌릴지' 혹은 '빌려달라고 요구할지' 선택해 포스팅하고, 검색할 수 있다. 물건을 클릭하면 상세페이지를 띄우고 댓글을 통해 소통할 수 있다. 

- 또한, 카카오 API를 활용해 사용자의 위치 정보를 받고, 해당 사용자가 몇 km 떨어져 있는지 계산해 주었다.

- 게시글 오른쪽 하단에 있는 아이디를 클릭하면, 해당 유저의 페이지로 이동하도록 했다.

<br>

![Crepe](https://i.imgur.com/Giih0zY.jpg)

<br>

![Crepe](https://i.imgur.com/rEr0pJv.jpg)

<br>

![Crepe](https://i.imgur.com/0k1cHNA.jpg)

<br>

### 추가 사항

- 실시간 crud 기능
- 댓글 수정 & 삭제 기능
- Sold out 버튼

## Github

처음으로 다른 사람들과 함께 github를 이용해서 협업해 봤는데, git 사용이 생각보다 정말 어려웠다. 어제 저녁 프로젝트가 거의 끝나갈 때 내가 커밋을 잘못해서 원격 저장소의 파일들을 날려버렸다. 이후 revert하려 시도했지만, 실패했고 다른 repository를 다시 만들어야 했다.

다음부터는 pull, push 과정에서 충돌이 발생하는 부분들을 모두 해결하고 다시 commit하도록 하자...

<br>

### git memo

```
git init

git clone (git 저장소의 URL) - github 저장소 local로 복사

git status - 현재 저장소 내의 commit상태, branch 등을 확인할 수 있다.

<<변경사항 commit>>

git add . 

git status - add 영역 확인

git commit -m "memo" - 커밋 & 메모 작성

[최초 한 번만]
git remote add origin (원격 저장소 github URL) - 원격 저장소 연결

git push origin master - 커밋 내역을 master branch에 push(branch 정보를 확인하자)

git pull origin master

pull request에서 아래와 같이 충돌이 발생하면 이를 수정하고 다시 커밋한다.
'
<<<<<< HEAD
로컬 저장소
=======
remote 변경 내용
>>>>>> aab6d380aaf237a7c0aae28e00ea4607c8a7eec9
'
```

<br>



<br>
