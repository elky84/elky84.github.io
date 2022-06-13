---
layout: post
title: Ruby가 모듈을 찾는 장소.
date: 2013-05-09 11:14:54
categories: [Ruby]
tags: [Ruby, 루비]
comments: true
---

### Ruby에서 load나 require시에 참조하는 폴더
* 쉘 현재 경로 
* RUBYLIB 환경 변수 경로

이 경로를 알고 싶을땐 아래와 같은 구문으로도 가능합니다.

    % ruby -e 'puts $:'

이 경로가 아닌 다른 경로를 지정하기 위해서는, -I 경로 (대문자 I입니다)를 지정하거나, RUBYLIB 환경 변수에 추가해주시면됩니다.

예를 들어,

    ruby Util/StartServers.rb filename

라는 구문이 있을때, -I 구문이 없다면 rb파일이 참조하고 있는 다른 파일들은 제대로 로드 되지 못합니다.

    load 'XML_Util.rb'
    load 'ShellExecute_Util.rb'
    load 'ServerConstants.rb'

이 load 구문들에서 XML_Util.rb을 찾지 못해, 오류를 발생하는 것이죠.

그래서, 

    ruby -I Util Util/StartServers.rb filename

이렇게 모듈 검색 경로를 설정해, 해당 모듈들의 위치를 지정함으로써, 해결 가능합니다.

단순한 문제지만, 구글링으로 원하는 답변을 찾다가, 프로그래밍 루비 (곡괭이책)에서 원하는 답변을 찾게되 올립니다. 저처럼 헷갈리신분 참고하세요~.