---
title: "명령줄 참조 - EF Core"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 076e9251850ba10df323cd25922aa8b95b3a5491
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2017
---
<a name="entity-framework-core-tools"></a>Entity Framework Core 도구
===========================
Entity Framework Core 도구를 사용하면 EF Core 앱 개발 과정에 도움이 됩니다. 주로 데이터베이스의 스키마에 대한 리버스 엔지니어링을 통해 DbContext와 엔터티 형식을 스캐폴드하고 마이그레이션을 관리합니다.

[EF Core PMC(Package Manager Console) 도구][1]는 Visual Studio 내부에서 뛰어난 환경을 제공합니다. NuGet의 [PMC(Package Manager Console)][2]를 사용하여 실행합니다. 이러한 도구는 .NET Core와 .NET Framework 프로젝트 모두에서 작동합니다.

[EF Core .NET 명령줄 도구][3]는 [.NET Core CLI(명령줄) 도구][4]의 확장으로, 여러 플랫폼에서 사용하고 Visual Studio 외부에서 실행할 수 있습니다. 이러한 도구에는 .NET Core SDK 프로젝트(`Sdk="Microsoft.NET.Sdk"` 또는 프로젝트 파일에서 유사 항목)가 필요합니다.

두 도구 모두 동일한 기능을 제공합니다. Visual Studio에서 개발할 경우 더 통합적인 환경을 제공하는 PMC 도구를 사용하는 것이 좋습니다.

<a name="frameworks"></a>프레임워크
----------
이 도구는 .NET Framework 또는 .NET Core 대상 프로젝트를 지원합니다.

다른 프레임워크(예: 유니버설 Windows 또는 Xamarin)를 대상으로 하는 프로젝트의 경우 별도의 .NET Standard 프로젝트를 만들고, 지원되는 프레임워크 중 하나에 대한 교차 대상을 지정하는 것이 좋습니다.

.NET Core에서 교차 대상을 지정하려면 프로젝트를 마우스 오른쪽 단추로 클릭하고 **\*.csproj 편집** 등을 클릭합니다. 다음과 같이 `TargetFramework` 속성을 업데이트합니다. 단, 속성 이름 복수가 됩니다.

``` xml
<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>
```

.NET Standard 클래스 라이브러리를 사용할 경우 시작 프로젝트가 .NET Framework 또는 .NET Core를 대상으로 할 때 교차 대상을 지정할 필요가 없습니다.

<a name="startup-and-target-projects"></a>시작 및 대상 프로젝트
---------------------------
명령을 호출할 때마다 대상 프로젝트와 시작 프로젝트 등의 두 프로젝트가 관련됩니다.

대상 프로젝트에는 모든 파일이 추가됩니다(또는 일부 경우 제거됨).

시작 프로젝트는 프로젝트 코드를 실행할 때 도구가 에뮬레이트하는 프로젝트입니다.

대상 프로젝트와 시작 포르젝트가 동일할 수 있습니다.


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
