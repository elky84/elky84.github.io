---
layout: post
title: Django to ASP.NET CORE
date: 2017-03-01 11:14:54
categories: [웹 서버]
tags: [Django, ASP.NET CORE]
comments: true
---
### django lobby 서버 기능 이전.
* controller 맞추기.
    * routes 관리가 따로 필요 없음.
        * 컨트롤러 url 설정 규격
            * <http://www.strathweb.com/2016/09/required-query-string-parameters-in-asp-net-core-mvc/>

* models 동일하게 구현하고 migrate
    * add-migration error
        * <http://stackoverflow.com/questions/38162227/asp-net-core-ef-add-migration-command-not-working>
    * data type 설정
        * <https://docs.microsoft.com/en-us/ef/core/modeling/relational/data-types>
    * container type
        * List로는 primitive type은 안되고, 다른 model만 지정할 수 있다.
            * ER관계로 표현 가능한 것만 List로 담을 수 있게 규정함.
        * JsonField같은 것이 없음.
            * string을 JSON형식으로 사용해서 컨테이너 타입을 구현한 방식.
                * <http://stackoverflow.com/questions/20711986/entity-framework-code-first-cant-store-liststring>

* 컨텐츠 코딩 하위호환.
    * 기본적으로 기존 룰 (서버가 SendMessage를 통해 클라이언트 제어)을 따름.
    * naming을 Camel 표기법으로 변경.
    * JSON 규격도 네이밍 제외하곤 기존 룰 따라감.
        * 일부 클래스는 멤버 제거 및 보강 작업을 추후 진행.