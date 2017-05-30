---
layout: post
title: 데이터베이스 마이그레이션 with rails
date: 2013-04-15 11:14:54
categories: Ruby Rails
tags: Ruby Rails
comments: true
---

[참고 문서](http://rubykr.github.io/rails_guides/migrations.html)

한글 번역 아주 잘 되어 있군요.

Redmine도 루비를 쓰는 만큼, 같은 개념이라고 하네요. 

**모델 생성시**

    rails generate model Product name:string description:text

**독립적인 마이그레이션 만들기**

    rails generate migration AddPartNumberToProducts

**특정 버전으로 Migrate**
    
    rake db:migrate VERSION=20080906120000

**Rollback**

    rake db:rollback


간략하게 요약하자면 위와 같습니다.

독립적인 마이그레이션을 만들고 나면, db\migration 폴더 안에 현재 시간의 이름으로 된 rb 파일이 생성됩니다.
이 곳에 정의되어있는 **up 메소드는 버전을 올릴때, down 메소드는 버전을 내릴 때 불려지는 메소드**입니다.

이 과정을 통해서, 컬럼의 추가/삭제라던지, 인덱스의 추가/삭제 등의 버전별 기능을 유지할 수 있습니다.