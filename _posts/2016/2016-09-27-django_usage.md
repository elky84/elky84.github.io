---
layout: post
title: Django 사용법
date: 2016-09-27 11:14:54
categories: [Django]
tags: [Python, Django]
comments: true
---

* Django 웹 서버 환경 설정.
    * <http://tutorial.djangogirls.org/ko/django_start_project/> 
* django-excel 설치 오류
    * <http://django-excel.readthedocs.io/en/latest/>
    * pip install --upgrade setuptools
* pip 설치 방법
    * <https://pip.pypa.io/en/stable/>
* rails와 대응되는 기능
    * config.rb + database.yml [settings.py]
    * urls.py [routes.rb]
    * pip [gemfile]
        * <https://pip.pypa.io/en/stable/user_guide/>
    * requirements.txt
        * <http://stackoverflow.com/questions/19280249/python-equivalent-of-a-ruby-gem-file>
    * pip manage [gem update]
* 실행
    * pip install -r requirements.txt
* 버전 고정
    * pip freeze > requirements.txt
* admin 기능 활성화
    * <http://django-doc-pootle-test.readthedocs.io/en/latest/intro/tutorial02.html>
* admin 계정 생성
    * python manage.py createsuperuser
* model 생성 기능
    * <http://tutorial.djangogirls.org/ko/django_models/>
        >python manage.py startapp [앱이름]
    * <http://pythonstudy.xyz/python/article/308-Django-%EB%AA%A8%EB%8D%B8-Model>
* model reload
    * <http://stackoverflow.com/questions/4377861/reload-django-object-from-database>
* model api
    * <http://pythonstudy.xyz/python/article/310-Django-%EB%AA%A8%EB%8D%B8-API>
* model migrate 과정
    * migrate 파일 생성
        * python manage.py makemigrations
    * 실행
        * python manage.py migrate
    * migrate 실패시
        * 오류 확인 후, 오류가 나지 않도록 migrate 파일을 수동 수정하거나 삭제 해주면 된다.
        * 주로 default값 오류임.
            * default 값 넣는 법
                * <http://stackoverflow.com/questions/12649659/how-to-set-a-django-model-fields-default-value-to-a-function-call-callable-e>
    * models.field types
        * <https://docs.djangoproject.com/en/1.9/ref/models/fields/>
* redis
    * <https://niwinz.github.io/django-redis/latest/>
* request
    * <http://nemesisdesign.net/blog/coding/django-how-retrieve-query-string-parameters/>
* cookie 유지
    * <http://blog.readiz.com/4>
* datetime
* compare
    * <http://stackoverflow.com/questions/16016002/django-how-to-get-a-time-difference-from-the-time-post>
* background run
    * 설치
        * yum install nohup
    * 권한 부여
        * chmod 755 shell.sh
    * 가동
        * nohup sh -- ./shell.sh &
    * 중지
        * killall
            * <http://www.linuxadmin.lt/centos-7-killall-command-not-found/>
* File upload
    * <https://docs.djangoproject.com/ja/1.9/topics/<http /file-uploads/>
* multifile upload
    * <http://gorakgarak.tistory.com/621>
    * <https://docs.djangoproject.com/ja/1.9/topics/<http /file-uploads/#uploading-multiple-files>
* inspect
    * 함수와 앱 이름 알아내기
        * <https://www.isotoma.com/blog/2009/12/09/getting-the-name-of-the-current-view-in-a-django-template/>

## Django package
* django
    * <http://stackoverflow.com/questions/24057015/manage-py-importerror-no-module-named-django>
        * pip install django
* redis 계열
    * redis
        * <https://github.com/MSOpenTech/redis/releases>
    * django-redis-sessions
        * <https://pypi.python.org/pypi/django-redis-sessions/0.5.6>
    * django-redis
        * <https://niwinz.github.io/django-redis/latest/>
* django-jsonfield
    * <https://pypi.python.org/pypi/django-jsonfield>
* django-excel
    * <http://django-excel.readthedocs.io/en/latest/>

## python
* dictionary 
    * 사용법
        * <https://wikidocs.net/16>
* merge
    * <http://stackoverflow.com/questions/11011756/is-there-any-pythonic-way-to-combine-two-dicts-adding-values-for-keys-that-appe>
* list
    * 원소 포함 여부
        * <http://stackoverflow.com/questions/12934190/is-there-a-short-contains-function-for-lists-in-python>
* loop
    * <http://www.tutorialspoint.com/python/python_for_loop.htm>
* 연산자 정리
    * 연산자, 식, 그리고 프로그램 흐름
        * <http://jythonbook-ko.readthedocs.io/en/latest/OpsExpressPF.html>
    * 3항 연산자
        * <http://infitude.tistory.com/3>
* null 검사
    * <http://stackoverflow.com/questions/3289601/null-object-in-python>
* to_i
    * <http://stackoverflow.com/questions/642154/how-to-convert-strings-into-integers-in-python>
* json
    * dictionary <-> json and django
        * <https://godjango.com/blog/working-with-json-and-django/>
    * dictionary <-> json
        * <http://bluese05.tistory.com/37>
* 람다
    * <https://wikidocs.net/64>
* 클로저
    * <http://jonnung.blogspot.kr/2014/09/python-easy-closure.html>
* 모듈
    * <https://wikidocs.net/29>
* command line argument
    * <http://devluna.blogspot.kr/2015/10/python-command-line-argument-calling.html>
* file io
    * <https://wikidocs.net/26>
* tuple
    * <https://wikidocs.net/71>
* convert
    * <http://www.dotnetperls.com/convert-python>
* static method, class method
    * <http://blog.naver.com/PostView.nhn?blogId=dudwo567890&logNo=130164152571>


## python package
* excel
    * 설치
        * <https://github.com/pyexcel/pyexcel>
* 사용법
    * <http://pythonhosted.org/pyexcel/tutorial.html>
    * <https://github.com/pyexcel/pyexcel-xls>
* console color
    * <https://pypi.python.org/pypi/colorama>