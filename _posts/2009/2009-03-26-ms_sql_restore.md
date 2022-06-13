---
layout: post
title: MS-SQL 복원
date: 2009-03-26 11:14:54
categories: [데이터베이스]
tags: [RDB, Database, 데이터베이스, MS-SQL, SQL Server]
comments: true
---

# 복원
- 백업한 데이터를 복구하는 것을 말한다.

## 쿼리
- Restore Database DB이름 From Disk='경로\파일명.bak'

## 복원시 옵션
1. 일반 옵션
   - File : 한 파일 내에 여러개의 백업 존재 시, 백업 디바이스 헤더 검사에서의 Position 값을 넘겨주면 해당 백업만 복원합니다.
    >Restore Database DB이름 From Disk='경로\파일명.bak' 
    >With File = 3 -- 해당 파일의 3번 Position의 백업만 복원
   - DBO_ONLY : 복원 이후 권한을 db_owner 에게만 준다
   >Restore Database DB이름 From Disk='경로\파일명.bak' 
   >With Dbo_Only
2. 복구 이후 상태 설정용 옵션
   - Recovery : 복원이 끝났으니 사용하게 하라는 의미.
   >Restore Database DB이름 From Disk='경로\파일명.bak' 
   >With Recovery -- 복원을 끝내고 사용할 수 있도록 설정
   - NoRecovery : 추가적인 복구 작업이 있을 것이기에 사용하게 하지 말라는 의미.
   >Restore Database DB이름 From Disk='경로\파일명.bak' 
   >With Recovery -- 복원을 끝내도 사용은 할 수 없게 설정
   - Standby : 추가적인 복구가 있을 예정이지만, 사용은 하고 싶기에 읽기 전용 상태로 설정.
   >Restore Database DB이름 From Disk='경로\파일명.bak' 
   >With Standby -- 복원을 끝내고 읽기 전용으로 설정
3. 범위 지정 복구 관련 옵션
   - StopAt : 복원 시점을 시간으로 지정
   >Restore Database DB이름 From Disk='경로\파일명.bak'
   >StopAt = '2001-11-19 01:09' --2001년 11월 19일 1시 9분 데이터까지 복원
4. 마크 이용 복구 관련 옵션
   - 트랜잭션에 마크 설정
   >Begin Tran myTran With Mark --트랜잭션에 마크를 설정
   >-- 여러가지 처리
   >Commit
   - StopAtMark : 지정한 마크의 트랜잭션까지 복원
   >Restore Database DB이름 From Disk='경로\파일명.bak'
   >StopAtMark = 'myTran' -- myTran 이라는 Transaction 까지 복원
   - StopBeforeMark : 해당 트랜잭션 직전(해당 트랜잭션을 포함하지 않음.) 에서 복원을 중지한다.
   >Restore Database DB이름 From Disk='경로\파일명.bak'
   >StopBeforeMark = 'myTran' --myTran 이전 까지의 Transaction 까지 복원
   - After : 날짜 지정 기능 (StopAtMark나, StopAtBeforeMark 과 함께 사용 가능)
   >Restore Database DB이름 From Disk='경로\파일명.bak'
   >StopAtMark = 'myTran'
   >AFTER '2001-11-19 01:09' --2001년 11월 19일 1시 9분 데이터까지 복원
5. 현재 상태를 파일로 저장시 옵션
   - MOVE A TO B : 복원시 현재 상태를 파일로 저장할 파일 경로 재정의 명령
   >Restore Database DB이름 From Disk='경로\파일명.bak' 
   >With Move '논리 파일 이름' To '경로\파일명.mdf' -- 해당 논리 파일을 지정한 곳으로 옮겨라
   - Replace : 복원시 현재 상태를 저장할 때 같은 파일 존재시 덮어 써라. (Move 명령과 함께 옵션 지정 가능)
   >Restore Database DB이름 From Disk='경로\파일명.bak' 
   >With Replace-- 파일명.bak이 이미 있을 경우 덮어 쓰게 설정

## 복구 모델
1. 전체 복구 모델 (FULL)
   - 데이터 백업과 로그 백업을 사용하여, 손실 없이 복구하는 모델.
   - 시간 지정 복구가 가능하다.
   - 전체, 차등, 로그, 파일 백업을 할 수 있음.
2. 대량 로그 복구 모델 (Bulk_Logged)
   - 최소의 로그와 데이터 페이지를 저장하고 복구하는 모델.
   - 지정된 시점까지의 복구(Stopat 옵션)는 허락 안된다.
   - 전체, 차등, 로그, 파일 백업을 할 수 있음.
3. 단순 모델 (Simple)
   - 로그를 가장 적게 사용한다. trunc.log on chkpt. 데이터 베이스 옵션과 동일.
   - 로그를 주기적으로 비우기때문에 로그가 가득 차지 않는다.
   - 지정된 시점까지의 복구(Stopat 옵션)는 허락 안된다.
   - 전체, 차등 백업만 할 수 있음.