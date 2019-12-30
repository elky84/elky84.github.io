---
layout: post
title: .NET CORE 3.0 릴리즈
date: 2019-09-25 00:00:00
categories: [.NET CORE 3, .NET CORE, C#]
tags: [.NET CORE 3, .NET CORE, C#]
comments: true
---

[Announcing .NET Core 3.0](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-0/)

이와 동시에 ASP NET CORE 3.0, EF CORE 3.0도 함께 릴리즈 되었다.

이미 2.X 버전 대에 와서는 쓸만한 상태가 됐었지만, 부족한 부분들을 채워주겠다는 포부를 들고 왔다.

몇가지 신규 기능 나열하자면 다음과 같다.
1. Winform, WPF 지원.
    * 윈도우에서만 가능하다.
2. 새 JSON API 추가
    * JSON을 다루기 좀 더 편리하고, 성능도 훌륭하다.
3. ARM64 지원.
    * 즉 모바일 기기나, SBC 등에서 유용하다는 뜻이다.
4. C# 8.0 지원
    * <https://docs.microsoft.com/ko-kr/dotnet/csharp/whats-new/csharp-8>
5. 비동기 스트림 지원
    * C#은 이미 async/await를 아주 유연하게 지원하므로, 스트림 처리에서도 비동기 처리를 아주 손쉽게 녹여낼 수 있다.
6. Build/Publish 옵션들의 추가
    * <https://dotnetcoretutorials.com/2019/06/20/publishing-a-single-exe-file-in-net-core-3-0/> 참조
7. LINQ 확장 추가
    * DataTable등과 같은 기본 자료형에 다양한 API 추가로 데이터를 다루는 편의성 강화.
8. 성능 개선
    * 새 버전이 나올수록 느려지는 모 언어도 있으니...장점 맞는걸로…
9. 가비지 컬렉터 메모리 사용량 개선
10. Docker 지원 강화
    * 진짜 아주 편해졌다.
    * 6번 항목과 어우러져서, 어떻게든 배포 스트레스를 줄여주겠다는 의지의 표명으로 보여짐.
    * Docker-Compose도 기본 지원
    * Visual Studio 사용시 Docker Desktop과 아주 밀접하게 연동 된 것을 알 수 있는데, 이는 궁극적으로 Kubernetis와의 연동에서도 장점을 가지고 있음을 말한다.

.NET 5를 통한 .NET CORE와 .NET Framework의 대통합을 노리는 MS의 마지막 .NET Core 메이저 버전인 만큼 어떠한 형태로 통합 될 것인지를 미리 볼 수 있는 버전이었다.

[Introducing .NET 5](https://devblogs.microsoft.com/dotnet/introducing-net-5/)

실제로 .NET CORE 2.X 에서도 충분히 좋았지만, 3.0으로 오면서 그 만족도는 조금 지쳐 있던 개발이 즐거워질 정도였다.
리눅스 환경에서의 매끄러운 동작은 2.X에서도 충분했는데, Docker 지원의 강화는 더 말할 것 없는 편안함을 가져다 주었다.

나 처럼 1.X 버전의 .NET CORE로 고통 받았던 사람이나, 아직도 리눅스에서 C#은 Mono로 구동해야 된다고 생각하는 분들이 있다면 조금 시간을 내어 간단한 프로젝트를 구동/배포 해본다면 C#의 매력에, .NET의 매력에 빠질 수 있지 않을까 생각한다. 특히 리눅스 서버를 써야 하는 선택지만 있는 분들일 수록 더욱 추천하고 싶다.