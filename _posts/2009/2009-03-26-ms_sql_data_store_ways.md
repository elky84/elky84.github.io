---
layout: post
title: MS-SQL 데이터 저장 방식
date: 2009-03-26 11:14:54
categories: [데이터베이스]
tags: [RDB, Database, 데이터베이스, MS-SQL, SQL Server]
comments: true
---

## Page
MS-SQL에서 다뤄지는 데이터가 저장되는 최소 단위는 Page 입니다.
Page의 크기는 8KB ( 1024 * 8 = 8192 btyes ) 이지만, 실제로 데이터를 저장할 수 있는 maximum row size 는 8090 bytes 입니다.

## Page의 구성
1. header (96 bytes)
   * 이전페이지와 다음페이지의 정보 저장
2. Data rows (8090 bytes)
   * 실제 데이터 저장부분
3. offset 부분
   * 각 행의 첫번째 바이트가 페이지의 시작부분에서 얼마나 멀리 떨어져 있는지를 저장

## Extent
페이지가 최대 8개가 모여 한개의 Extent를 이룹니다. 테이블, 인덱스가 물리적으로 저장되는 최소 단위라고 생각하시면 됩니다.
Extent의 크기는 64KB ( 8192 * 8 = 65536 bytes ) 입니다.
1MB당 16개의 extent 가 존재할 수 있다.

## Extent의 분류
1. single extent (단일)
   * 테이블면 테이블, 인덱스면 인덱스 하나의 단일한 데이터가 저장된 상태
2. mix extent (혼합)
   * 테이블과 인덱스등의 하나이상의 데이터베이스의 오브젝트들이 같이 저장된 상태
