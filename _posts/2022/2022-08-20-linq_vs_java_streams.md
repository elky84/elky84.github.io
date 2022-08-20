---
layout: post
title: C# LINQ 함수들의 작동 방식 (with skeches)
date: 2022-08-20 00:00:00
categories: [C#]
tags: [C#, LINQ]
comments: true
---

최근에 읽게된 글 중에서, LINQ의 편리한 메소드들을 시각화 해 준 글이 있었다.

# LINQ의 함수들의 작동 방식

* 💡 [LINQ explained with sketches](https://steven-giesel.com/blogPost/d65c5411-a69b-489f-b73f-18ce0ed8678d)

![Linq](/img/2022/LinqPart1.webp)
* Select : 특정 값을 선택 (또는 변환 역할도 가능) like Projection
* Where : 추출
* SelectMany : 병합
* Zip : 두 목록을 병합. 양쪽다 원소가 있는 수만큼만 병합함.
* OrderBy : 정렬
* Distinct : 중복 제거

![Linq](/img/2022/LinqPart2.webp)
* Aggregate : 집계
* Chunk: 특정 갯수 단위로 묶음
* Union : 두 목록의 고유한 요소만 추출. (병합+Distinct)
* Intersect: 두 목록에 모두 존재하는 요소만 추출
* Any : 특정 원소가 하나라도 만족하면 됨
* All : 모든 요소가 조건을 다 만족해야 함
* Append: 뒤에 붙이기
* Prepend : 앞에 붙이기

![Linq](/img/2022/LinqPart3.webp)

* MaxBy : 가장 큰 객체 찾기
* DistinctBy : Distinct와 유사하지만, Select처럼 값의 형태를 변환할 수 있음
* ToLookup : 특정 조건이 맞는 데이터를 묶을 때 사용됨.
* ToDictionary : 특정 값을 Key, Value로 변환해서 Dictionary로 변환함. 키를 잘못 지정할 경우 중복 키 예외가 발생할 수 있음에 주의
* Join : SQL Left Join과 유사함. 한 목록을 기준으로, 다른 목록에서 조건이 맞는 데이터를 추출하고, 결과를 바탕으로 새 형태로 변환 가능


![Linq](/img/2022/LinqPart4.webp)
* Take : 특정 갯수만큼 꺼내기
* Skip : 특정 갯수만큼 건너뛰기
* OfType : 해당 타입의 일종인 원소만 추출
* GroupBy : 특정 값을 기준으로, 그룹화
* Reverse : 순서 반전
* First : 첫번째 원소 반환
* Single : 원소가 하나여야만 함. 두개 이상이거나, 하나도 없을 경우 예외 발생에 주의
* FirstOrDefault : 첫번째 원소를 반환하거나, 원소가 하나도 없으면 null 반환
* SingleOrDefault : 원소가 하나일 경우에만 첫번째 원소를 반환하고, 둘 이상이거나 원소가 하나도 없으면 null 반환

원본 글이 워낙 잘 정리되어있다. 간단한 요약+번역글로 봐주면 감사하겠다.