---
title: Entity Framework Core 설치
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 455eccbb149f0980cefa250ef5db443c73e66603
ms.sourcegitcommit: ae399f9f3d1bae2c446b552247bd3af3ca5a2cf9
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48575641"
---
# <a name="installing-entity-framework-core"></a>Entity Framework Core 설치

## <a name="prerequisites"></a>전제 조건

* .NET Core 2.1 대상 앱을 개발하려면 [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core)를 설치합니다. 최신 버전의 Visual Studio 2017이 있는 경우에도 이 SDK를 설치해야 합니다.

* .NET Core 2.1 대상 앱 개발을 위해 Visual Studio를 사용하려면 Visual Studio 2017 버전 15.7 이상을 설치하십시오.

* ASP.NET Core 응용 프로그램에서 Entity Framework 2.1을 사용하려면 ASP.NET Core 2.1을 사용하십시오. 이전 버전의 ASP.NET Core를 사용하는 응용 프로그램은 2.1로 업그레이드해야 합니다.

* .NET Framework 4.6.1 이상을 대상으로 하는 앱에 Visual Studio 2015를 사용할 수 있습니다. 하지만 .NET Standard 2.0 및 호환 프레임워크를 인식하는 NuGet 버전이 필요합니다. Visual Studio 2015에서 이를 가져오려면, [NuGet 클라이언트를 버전 3.6.0으로 업그레이드하십시오](https://www.nuget.org/downloads).

## <a name="get-the-entity-framework-core-runtime"></a>Entity Framework Core 런타임 가져오기

EF Core 런타임 라이브러리를 응용 프로그램에 추가하려면 사용하려는 데이터베이스 공급자에 대한 NuGet 패키지를 설치하십시오. 지원되는 공급자 및 해당 NuGet 패키지 이름 목록은 [데이터베이스 공급자](../../providers/index.md)를 참조하십시오.

NuGet 패키지를 설치하거나 업데이트하려면 .NET Core CLI, Visual Studio 패키지 관리자 대화 상자 또는 Visual Studio 패키지 관리자 콘솔을 사용하십시오.

ASP.NET Core 2.1 응용 프로그램의 경우 메모리 내 및 SQL Server 공급자가 자동으로 포함되므로 이를 개별적으로 설치할 필요가 없습니다.

> [!TIP]  
> 타사 데이터베이스 공급자를 사용하는 응용 프로그램을 업데이트해야 할 경우 항상 공급자의 업데이트가 사용할 EF Core 버전과 호환되는지 확인합니다. 예를 들어 이전 버전에 대한 데이터베이스 공급자는 EF Core 런타임 버전 2.1과 호환되지 않습니다.  

### <a name="net-core-cli"></a>.NET Core CLI

다음 .NET Core CLI 명령은 SQL Server 공급자를 설치하거나 업데이트합니다.

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

`dotnet add package` 명령에서 `-v` 한정자를 사용하여 특정 버전을 나타낼 수 있습니다. 예를 들어 EF Core 2.1.0 패키지를 설치하려면 명령에 `-v 2.1.0`을 추가합니다.

### <a name="visual-studio-nuget-package-manager-dialog"></a>Visual Studio NuGet 패키지 관리자 대화 상자

* 메뉴에서 **프로젝트 > NuGet 패키지 관리** 선택

* **찾아보기** 또는 **업데이트** 탭 클릭

* SQL Server 공급자를 설치하거나 업데이트하려면 `Microsoft.EntityFrameworkCore.SqlServer` 패키지를 선택하고 확인합니다.

자세한 내용은 [NuGet 패키지 관리자 대화 상자](https://docs.microsoft.com/nuget/tools/package-manager-ui)를 참조하십시오.

### <a name="visual-studio-nuget-package-manager-console"></a>Visual Studio NuGet 패키지 관리자 콘솔

* 메뉴에서 **도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔** 선택

* SQL Server 공급자를 설치하려면 패키지 관리자 콘솔에서 다음 명령을 실행합니다.

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* 공급자를 업데이트하려면 `Update-Package` 명령을 사용합니다.

* 특정 버전을 지정하려면 `-Version` 한정자를 사용합니다. 예를 들어 EF Core 2.1.0 패키지를 설치하려면 명령에 `-Version 2.1.0`을 추가합니다.

자세한 내용은 [패키지 관리자 콘솔](https://docs.microsoft.com/nuget/tools/package-manager-console)을 참조하십시오.

## <a name="get-entity-framework-core-tools"></a>Entity Framework Core 도구

런타임 라이브러리 외에도 디자인 타임에 프로젝트에서 일부 EF Core 관련 작업을 수행할 수 있는 도구를 설치할 수 있습니다. 예를 들어 마이그레이션을 만들고, 마이그레이션을 적용하고, 기존 데이터베이스를 기준으로 모델을 만들 수 있습니다.

사용 가능한 두 가지 도구 집합:
* .NET Core [CLI(명령줄 인터페이스) 도구](../../miscellaneous/cli/dotnet.md)는 Windows, Linux 또는 macOS에서 사용할 수 있습니다. 이러한 명령은 `dotnet ef`로 시작합니다. 
* [패키지 관리자 콘솔 도구](../../miscellaneous/cli/powershell.md)는 Windows의 Visual Studio 2017에서 실행됩니다. 이러한 명령은 동사(예: `Add-Migration`, `Update-Database`)로 시작합니다.

패키지 관리자 콘솔에서 `dotnet ef` 명령을 사용할 수 있지만 Visual Studio를 사용할 경우 패키지 관리자 콘솔 도구를 사용하는 것이 더 편리합니다.
* 수동으로 디렉터리를 전환하지 않고 패키지 관리자 콘솔에서 현재 선택된 프로젝트에서 자동으로 작업합니다.  
* 명령을 완료하면 자동으로 Visual Studio에서 명령을 통해 생성된 파일을 엽니다.

<a name="cli"></a>

### <a name="get-the-cli-tools"></a>CLI 도구 가져오기

`dotnet ef` 명령은 .NET Core SDK에 포함되어 있지만, `Microsoft.EntityFrameworkCore.Design` 패키지를 설치하기 위해 필요한 명령을 지원합니다.

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

ASP.NET Core 2.1 앱의 경우 이 패키지는 자동으로 포함됩니다.

이전에 [필수 구성 요소](#prerequisites)에 설명된 것처럼 .NET Core 2.1 SDK도 설치해야 합니다.

> [!IMPORTANT]      
> 항상 런타임 패키지의 주 버전에 부합하는 도구 패키지 버전을 사용합니다.

### <a name="get-the-package-manager-console-tools"></a>패키지 관리자 콘솔 도구 가져오기

EF Core용 패키지 관리자 콘솔 도구를 가져오려면 `Microsoft.EntityFrameworkCore.Tools` 패키지를 설치합니다.

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Tools
``` 

ASP.NET Core 2.1 앱의 경우 이 패키지는 자동으로 포함됩니다.

## <a name="upgrading-to-ef-core-21"></a>EF Core 2.1로 업그레이드

기존 응용 프로그램을 EF Core 2.1로 업그레이드할 경우, 이전 EF Core 패키지에 대한 일부 참조를 수동으로 제거해야 합니다.

* `Microsoft.EntityFrameworkCore.SqlServer.Design` 같은 데이터베이스 공급자 디자인 타임 패키지는 EF Core 2.1에서 더 이상 지원되지 않거나 필요하지 않지만, 다른 패키지를 업그레이드할 때 자동으로 제거되지도 않습니다.

* 이제 .NET CLI 도구가 .NET SDK에 포함되므로 해당 패키지에 대한 참조를 *.csproj* 파일에서 제거할 수 있습니다.

  ```
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  ```

.NET Framework를 대상으로 하고 이전 버전의 Visual Studio로 제작된 응용 프로그램의 경우, .NET Standard 2.0 라이브러리와 호환되는지 확인하십시오.

  * 프로젝트 파일을 편집하고 최초 속성 그룹에 다음 항목이 표시되는지 확인합니다.

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * 테스트 프로젝트의 경우 다음 항목도 있는지 확인합니다.

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
