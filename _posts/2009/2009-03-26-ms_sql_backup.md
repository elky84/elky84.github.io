---
layout: post
title: MS-SQL 백업
date: 2009-03-26 11:14:54
categories: [MS-SQL, SQL Server]
tags: [MS-SQL, SQL Server]
comments: true
---

## 백업시 참고 사항
- 기본적으로 model, northwind, Pubs, tempdb는 백업하지 않아도 됨.
- master테이블은 권한과 같은 정보들이 있고, msdb테이블은 스케쥴이나 패키지 작업이 저장되어 있습니다. 필요한 데이터일 경우에만 백업하면 됩니다.

## 전체 백업
- 전체 백업은 데이터의 변경 유무에 관여하지 않고 전체 데이터의 복사본을 만드는 백업 방식입니다.
- 전체 백업은 복구 과정이 다른 백업 방식보다 간편하고 다른 백업 방식보다 복구 시간이 적게 소요됩니다.

### 쿼리
Backup Database DB이름
To Disk='경로\파일명.bak'


## 차등 백업
- 차등 백업은 마지막 전체 백업 이후 변경된 모든 데이터를 백업하는 방식입니다. 이는 증분 백업과는 다르게 전체 백업 이후 파일이 변경될 경우 다음 전체 백업까지 계속 백업하는 방식입니다.
- 전체 백업 이미지와 가장 최근의 차등 이미지만 복구하면 되기 때문에 복구 시점에 따라 다르긴 하지만 대개 증분 백업보다 복구 속도가 빠릅니다.
- 파일이 변경될 때마다 파일 크기가 증가하게 되며, 다음 전체 백업 때까지 파일 크기가 점점 커지게 된다는 단점이 있습니다.

### 쿼리
Backup Database DB이름
To Disk='경로\파일명.bak' With Differential


## 로그 백업
- 로그 백업은, 데이터베이스를 백업 받는게 아니고 그동안 발생했던 트랜잭션 로그(지금까지 이루어졌던 모든 동작들의 로그)를 백업 받는 방법입니다.
- 전체 백업시에도 로그 백업은 삭제 되지 않습니다. 그렇기 때문에 전체 백업을 하기 전에 로그 백업을 모두 삭제하고 전체 백업을 하는 것이 좋습니다.
- 로그 백업은 시간 단위로 복원이 가능하다는 장점이있습니다. (부분 롤백 가능)
- 트랜잭션 로그 백업을 받으면 백업받은 로그가 제거되어 로그 파일의 사이즈를 줄여 줄 수 있게 됩니다. (증분 백업의 개념으로 사용 가능)

### 쿼리
>Backup Log DB이름 To Disk = '경로\파일명.bak' GO

### 백업시 옵션
- Init (Overwrite) : 덮어 쓰기 모드
>Backup Log DB이름 To Disk = '경로\파일명.bak'
>With Init

- NoInit (Append) : 뒤에 이어 붙이는 모드. (Default)
>Backup Log DB이름 To Disk = '경로\파일명.bak'
>With NoInit

- No_Truncate : 로그 백업을 하고나서, 로그를 지우지 말라는 뜻. (데이터 베이스가 비정상적인 상태에서도 가능)
> Backup Log DB이름 To Disk = '경로\파일명.bak'
> With No_Truncate GO -- 로그 백업후 로그를 지우지 말라는 의미.


### 백업 받은 파일 검사
한 파일에 여러번 백업할 수도 있고, 한 파일당 한번 백업할 수도 있다.
백업 받은 파일에 백업이 정상적으로 이루어졌는지 등의 정보를 확인하려면 아래와 같은 쿼리로 확인하면 된다.

- 백업 디바이스 헤더 검사
> Restore HeaderOnly From Disk = '경로\파일명.bak' GO

- 백업시 파일 리스트 검사
> Restore FileListOnly From Disk = '경로\파일명.bak' GO

- 백업 미디어 정보 검사
> Restore LabelOnly From Disk = '경로\파일명.bak' GO

- 백업 안정성 검사
> Restore VerifyOnly From Disk = '경로\파일명.bak' GO
