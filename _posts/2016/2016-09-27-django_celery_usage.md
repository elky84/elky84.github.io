---
layout: post
title: Django Celery 사용법 [with RabbitMQ]
date: 2016-09-27 11:14:54
categories: [Python, Django, Celery, RabbitMQ]
tags: [Python, Django, Celery, RabbitMQ]
comments: true
---

# Django + Celery 사용법

## 설치

* pip install -U Celery
* pip install django-celery
* 데이터베이스 migration 필요. [djcelery를 위해]

## Django 설정

* settings.py에 아래 내용을 추가한다.

>import djcelery
>djcelery.setup_loader()
>BROKER_URL = "amqp://guest:guest@localhost:5672//"

* settings.py 파일에 추가

>INSTALLED_APPS = (
>'djcelery',
>'myapp',
>)

* 두 가지를 추가해야 한다. myapp은 개발하고 있는 app의 이름이 되겠다.

## Task 생성

    from djcelery import celery
    @celery.task(name='tasks.add')
    def add(x,y):
     return x + y

    @celery.task(name='tasks.sleeptask')
    def sleeptask(i):
     from time import sleep
     sleep(i)
     return i

테스트 목적으로 두 개의 task를 만들었다. 하나는 즉시 값을 리턴하는 add함수와 10초 뒤에 task를 반환하는 sleeptask 함수이다.

## View 만들기

    # django app의 views.py에 아래의 내용을 추가
    from django.http import HttpResponse
    from myapp import tasks

    def test_celery(request):
       result = tasks.sleeptask.delay(2)
       result2 = tasks.add.delay(2,5)
       return HttpResponse("this is task test (id : %s)" % result.id)


    # 이렇게 view를 만들어놓고 url에 이 view를 호출할 수 있도록 해야한다. urlpattern에 아래 내용을 추가해준다.

    import views as taskview
    urlpatterns = patterns(
                          ...
                          url(r'^test$',taskview.test_celery),
                          )

### 가동

    python manage.py celeryd -l info

### 직렬화

* <http://stackoverflow.com/questions/16040039/understanding-celery-task-prefetching>
* python manage.py celeryd --concurrency=1 -Ofair
* settings.py에 추가.
    >CELERYD_PREFETCH_MULTIPLIER = 1

### Consumer

* 주의 사항
    * 15672는 웹 포트다. 5672 포트로 연결해야 한다.
    * 연결이 RabbitMQ에서 끊어질 시에는 RabbitMQ 관리 웹 페이지에서 Can access virtual hosts가 설정되어있는 계정인지 확인해볼 것.
    * tasks로 만들어 던지는 함수에 req를 바로 던질 수 없다. pickle 가능한 object만 던져야 한다. 즉 파라미터들을 꺼내서 던져야 한다는 의미.
        >user_no = int(req.session['user_no'])  
        >return tasks.celery.delay(user_no)

### Pycharm Celery 디버깅 설정

* <http://stackoverflow.com/questions/37150910/how-can-i-use-pycharm-to-locally-debug-a-celery-worker>

![Pycharm Celery 디버깅 설정](/images/celery_debugging.png)

### 참고

* <http://ngee.tistory.com/548>
* <http://ngee.tistory.com/580>
* <http://celery.readthedocs.io/en/latest/getting-started/first-steps-with-celery.html>
* <http://ahnseungkyu.com/110>
* <http://abipictures.tistory.com/895>
* <http://febdy.tistory.com/9>