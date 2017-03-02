---
layout: post
title: ASP.NET CORE 사용법
date: 2017-03-02 11:14:54
categories: 언어
comments: true
---
### ASP.NET CORE
* download
    * <https://www.microsoft.com/net/core#windowsvs2015>
* linux deploy
    * <https://www.microsoft.com/net/core#linuxcentos>
    * other version
        * <https://www.microsoft.com/net/download/linux> 
* getting started
    * <http://www.egocube.pe.kr/Translation/Content/asp-net-core-tutorial-1/201607220001#install-fiddler>
* server port & urls
    * <https://www.billbogaiv.com/posts/setting-host-uri-in-aspnet-core-rc2>
* perfomance profile
    * stop watch
        * <https://msdn.microsoft.com/ko-kr/library/system.diagnostics.stopwatch.elapsedticks(v=vs.110).aspx>
* Redirect
    * <https://support.microsoft.com/ko-kr/kb/307903>
* Session
    * <http://benjii.me/2016/07/using-sessions-and-<httpcontext-in-aspnetcore-and-mvc-core/>
### Database
* mysql .net core
    * connection string
        * <http://insidemysql.com/mysql-connector-net-for-net-core-1-0/>
    * win 10 SSL error
        * <http://stackoverflow.com/questions/32442056/how-to-connect-w10-universal-app-with-mysql-database>
* orm
    * EF Core
        * roadmap
            * <https://github.com/aspnet/EntityFramework/wiki/Roadmap>
        * new database
            * <https://docs.microsoft.com/en-us/ef/core/get-started/aspnetcore/new-db>
        * mysql provider
            * official
                * <https://docs.microsoft.com/en-us/ef/core/providers/mysql>
            * getting started
                * <http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/>
        * migration
            * official
                * <https://msdn.microsoft.com/ko-kr/data/jj591621.aspx>
                * update-database
                    * not implemented issue
                        * <http://howtoprogram.eu/question/n-a,25785>
                            * 아직 못 씀.
            * pomelo
                * <https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql>
                    * official과 같은 오류
            * SapientGuardian
                * <https://github.com/SapientGuardian/SapientGuardian.EntityFrameworkCore.MySql>
                    * 잘됨!!
                        * 쓰다가, official에서 버그 픽스되면 옮겨가자.
            * migration command not found
                * <http://stackoverflow.com/questions/35140341/ef7-the-term-add-migration-is-not-recognized-as-the-name-of-a-cmdlet>
                * Project.json에    "Microsoft.EntityFrameworkCore.SqlServer": "1.0.0", 있는지 확인
            * add-migration 오류
                * <https://github.com/aspnet/EntityFramework/issues/4825>
            * dotnet-ef [cli tool]
                * <https://docs.microsoft.com/en-us/ef/core/miscellaneous/cli/dotnet>
                * <http://benjii.me/2016/05/dotnet-ef-migrations-for-asp-net-core/>
                * Dotnet ef 커맨드 실행 오류
                    * <http://stackoverflow.com/questions/41960287/dotnet-ef-not-works-with-ef-tools-1-1-0-preview4-final>
            * Enum data type
                * <http://stackoverflow.com/questions/35298829/does-ef7-support-enums>
        * 
    * dapper.net
        * <https://github.com/StackExchange/dapper-dot-net>
            * create new database 기능이 없음.
* Redis
    * StackExchange
        * <https://www.nuget.org/packages/StackExchange.Redis/1.1.572-alpha>
    * ServiceStack
        * 유료라서 패스.
    
* Redis-cache
    * <https://docs.microsoft.com/en-us/aspnet/core/performance/caching/distributed>
* couchbase
    * <https://www.nuget.org/packages/CouchbaseNetClient/2.4.0-dp1>
### xlsx
* <http://cybershin.tistory.com/364>
### xUnit
* <https://xunit.github.io/docs/getting-started-dotnet-core.html>
### json
* newtonsoft.json
    * 저용량에서는 비슷, 대용량에서는 빠르고 복잡한 Nested 구조까지 가장 잘 처리했음.
        * <https://www.nuget.org/packages/newtonsoft.json/>