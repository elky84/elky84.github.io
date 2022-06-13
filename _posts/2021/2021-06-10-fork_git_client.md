---
layout: post
title: Fork - Git Client 추천
date: 2021-06-10 00:00:00
categories: [개발 도구]
tags: [Git, Git Client]
comments: true
---

Git Client는 셀 수도 없이 많이 존재한다.
git의 수 많은 command line 기능을 GUI화 했을 때 직관적이게 녹여내는 것이 쉽지 않기도하고, 개발자의 의도나 용법이 묻어나기도 하다보니 그 결과물은 매우 달라진다.

그 중에서도 개인적으로 검토하고 꽤 오랜 시간 (3개월 이상씩은 다 사용했다) Git Client를 소개하고 추천해보고자 한다.

비교 대상은 Git Kraken, Github Desktop, SourceTree, fork로 정했다. TortoiseGit은 SVN버전과 달리 Git버전은 마이너인데다가, SVN보다 좀 더 브랜치나 Interactive Rebase와 같은 commit 내역에 대한 처리가 중요한데 이 부분이 아쉬워 길게 사용 안해서 평가 대상에서 제외했다.

# 주요 판단 기준

내가 Git Client 사용시 중요하게 생각하는 기능은 다음과 같다.

1. Private Repository 사용 가능
2. 서비스 연동 지원 (Github, Github Enterprise, Gitlab, Bitbucket 등)
    * 특정 서비스 연동이 반 강제시에는 감점
3. 브랜치 그래프 시각화
4. Interactive Rebase 기능, Gitflow 지원
5. UI, UX (주관적)

# 1. [Git Kraken](https://www.gitkraken.com/)

무료 플랜이 있긴하지만, 제대로 사용하려면 유료다. 기능이 많긴하고 통합도 여러 메이저 서비스들은 다 연동된다.

![GitKraken](/img/2021/gitkraken.png)

## 1번 기준

아쉽게도 미충족이다. private repository에 대해서는 무료 사용이 불가능한 점이 아쉽다.

| ![GitKraken](/img/2021/gitkraken_private_repository.png) |
|:--:| 
| Private Repository는 무료 플랜에선 사용 불가능하다 |

| ![GitKraken](/img/2021/gitkraken_pricing.png) |
|:--:| 
| 물론 매달 4.5달러는 아주 큰돈은 아니지만, 다른 좋은 클라이언트가 없다면 모를까, 이 부분이 집에서 사용할 때 아쉬운 점이 됐다. |

## 2번 기준

이 부분은 아주 큰 강점이다.
비교하는 4개 제품군중 가장 다양하게 연동을 지원한다.
그래서 유료인 거긴 하지만

## 3번 기준

브랜치 그래프가 아주 눈에 잘 들어온다

## 4번 기준

### Interactive Rebase

Context 메뉴를 통한 Interactive 
![GitKraken](/img/2021/gitkraken_context_menu.png)

### Git Flow

문제 없음

## 5번 기준

전체적으로 직관적이고, 높은 학습 코스트 없이 사용 가능했다.

## 최종 점수

4점 (1번 항목에서 감점)


# 2. [Github Desktop](https://desktop.github.com/)

github에서 제공하는 무료 클라이언트다.

branch가 표시도 안되고, 브랜치별 현황을 보는 것이 비직관적이었다.

![Github](/img/2021/github_desktop.png)

## 1번 기준

문제 없다

## 2번 기준

Github와는 아주 찰떡이다.
이외에 저장소를 사용 할 경우, 연동이 잘 안되므로 굳이 선택할 필요 없다.

## 3번 기준

브랜치 그래프에 대해서 시각화가 부족하다

## 4번 기준

### Interactive Rebase
Context 메뉴를 통한 기능이 2개 밖에 지원이 안된다. 
![Github](/img/2021/github_desktop_context_menu.png)

### Git Flow

미지원

## 5번 기준

아쉽게도 사용한 클라이언트 중 가장 비직관적이었다.

## 최종 점수

1점 (처음 쓴지 한참 지나도 개선이 안됨. 무료 버전이고 목표가 뚜렷하지 않아서인지도?)

# 3. [SourceTree](https://www.sourcetreeapp.com/)

Atlassian에서 제공하는 무료 클라이언트다.

![SourceTree](/img/2021/sourcetree.png)

## 1번 기준

문제 없음

## 2번 기준

Bitbucket은 물론이며, Github를 포함한 Gitlab 등에서의 사용도 무난하다.
조금의 설정 세팅을 해주어야 할 때가 있으며, Github나 Gitlab 사용시 Remote 버튼에 빨간 Alert이 뜨는 것이 좀 거슬리는게 단점

초기 세팅시 꼭 Bitbucket을 요구하는 데다가, Mercurial 설치를 반 강제하는 점도 아쉬움

## 3번 기준

브랜치 그래프가 아주 잘 보인다.

## 4번 기준

### Interactive Rebase

꽤 많은 기능이 지원된다.
![Github](/img/2021/sourcetree_context_menu.png)

### Git Flow

문제 없음

## 5번 기준

무난하다

## 최종 점수

4.5점 (Bitbucket이 아닐 때 여기저기 거슬리는 부분이 존재하는 것이 매우 아쉽다)

# 4. [Fork (git-fork)](https://git-fork.com/)

Dan Pristupov (MAC 버전), Tanya Pristupova (.NET, WPF)라는 두 명의 개발자가 제공하는, 조건부 무료 클라이언트다.

![fork](/img/2021/fork.png)

유료 $49.99인데, 강제도 아니며, 광고 제거 및 후원에 목적을 두고 있다.

두 개발자가 spare time 개발자였는데, 최근 full-time 개발자로 전향했다.

## 1번 기준

문제 없음

## 2번 기준

기본적으로 연동 기능은 거의 없다고 봐도 무방하다.
아쉬울 수도 있지만, 기본 기능엔 충실하고, 대신 UX가 심플해진 측면도 있다고 볼 수 있다.

## 3번 기준

브랜치 그래프가 시각적으로 잘 보였다.

## 4번 기준

### Interactive Rebase

아주 많은 기능이 지원된다.

![fork](/img/2021/fork_context_menu.png)

### Git Flow

문제 없음

## 5번 기준

무난하다
기본 기능에 충실하며, 메뉴가 많지 않아 쉽게 익숙해 질 수 있다.

## 최종 점수

5점 (유레카! 심지어 구독 모델이 아니고, 유료 구매시에도 $49.99다)

# 요약

## 점수 정리
fork (5) > SourceTree (4.5) > Git Kraken (4) > Github Desktop (1)

## 회사에서 구매해서 사용한다면?

fork, Git Kraken (5) > SourceTree (4.5) > Github Desktop (1)

## 그래서 무엇을 쓰면 좋을까?

나는 fork를 추천한다.

3년여간 잘 사용하고 있고, 연동은 좀 아쉽지만 github 사이트 접속해서 이용하면 그만인 기능도 많다.