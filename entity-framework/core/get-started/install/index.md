---
title: "EF Core 설치"
author: divega
ms.author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
ms.technology: entity-framework-core
uid: core/get-started/install/index
ms.openlocfilehash: 31b96ebd0ae282b88be98988eff6263084dc5dd5
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2017
---
# <a name="installing-ef-core"></a>EF Core 설치

## <a name="prerequisites"></a>필수 구성 요소

.NET Core 2.0 응용 프로그램(.NET Core를 대상으로 하는 ASP.NET Core 2.0 응용 프로그램 포함)을 개발하기 위해서는 플랫폼에 맞는 [.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core)를 다운로드하여 설치해야 합니다. **Visual Studio 2017 15.3 버전을 설치한 경우에도 마찬가지입니다.**

EF Core 2.0 또는 기타 .NET Standard 2.0 라이브러리를 .NET Core 2.0 이외의 .NET 플랫폼(예: .NET Framework 4.6.1 이상)에서 사용하려면 .NET Standard 2.0을 인지하는 NuGet 버전과 그에 호환되는 프레임워크가 필요합니다. 이것은 몇 가지 방법으로 가져올 수 있습니다.

* Visual Studio 2017 15.3 버전 설치
* Visual Studio 2015를 사용할 경우 [NuGet 클라이언트 버전 3.6.0 다운로드 및 업그레이드](https://www.nuget.org/downloads)

이전 버전의 Visual Studio로 만들었고 .NET Framework를 대상으로 하는 프로젝트는 .NET Standard 2.0 라이브러리와의 호환을 위해 추가적인 수정이 필요할 수 있습니다. 

* 프로젝트 파일을 편집하고 최초 속성 그룹에 다음 항목이 표시되는지 확인합니다.
  ``` xml
  <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
  ```

* 테스트 프로젝트의 경우 다음 항목도 있는지 확인합니다.
  ``` xml
  <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
  ```

## <a name="getting-the-bits"></a>비트 가져오기
EF Core 런타임 라이브러리를 응용 프로그램에 추가하기 위해 권장되는 방법은 NuGet으로부터 EF Core 데이터베이스 공급자를 설치하는 것입니다.

런타임 라이브러리 외에도 마이그레이션 만들기 및 적용, 기존 데이터베이스 기반 모델 만들기 등과 같이, 설계 시점에 프로젝트의 여러 EF Core 관련 작업을 더 간편하게 수행할 수 있게 하는 도구를 설치할 수 있습니다.

> [!TIP]  
> 타사 데이터베이스 공급자를 사용하는 응용 프로그램을 업데이트해야 할 경우 항상 공급자의 업데이트가 사용할 EF Core 버전과 호환되는지 확인합니다. 예를 들어 이전 버전에 대한 데이터베이스 공급자는 EF Core 런타임 버전 2.0과 호환되지 않습니다.  

> [!TIP]  
> ASP.NET Core 2.0을 대상으로 하는 응용 프로그램은 추가적인 종속성 없이 타사 데이터베이스 공급자와 함께 EF Core 2.0을 사용할 수 있습니다. 이전 버전의 ASP.NET Core를 대상으로 응용 프로그램은 EF Core 2.0을 사용하려면 ASP.NET Core 2.0으로 업그레이드해야 합니다.

<a name="cli"></a>
### <a name="cross-platform-development-using-the-net-core-command-line-interface-cli"></a>.NET Core 명령줄 인터페이스(CLI)를 사용한 플랫폼 간 개발

[.NET Core](https://www.microsoft.com/net/download/core)를 대상으로 하는 응용 프로그램을 개발하기 위해 자주 사용하는 텍스트 편집기와 함께 [`dotnet` CLI 명령](https://docs.microsoft.com/dotnet/core/tools/)을 사용하거나, Visual Studio, Visual Studio for Mac 또는 Visual Studio Code 같은 IDE(Integrated Development Environment)를 사용할 수 있습니다.

> [!IMPORTANT]  
> .NET Core를 대상으로 하는 응용 프로그램은 특정 Visual Studio 버전이 필요합니다. 즉 .NET Core 1.x 개발에는 Visual Studio 2017이, .NET Core 2.0 개발에는 Visual Studio 2017 버전 15.3이 필요합니다.

플랫폼 간 .NET Core 응용 프로그램에서 SQL Server 공급자를 설치하거나 업그레이드하려면 응용 프로그램의 디렉터리로 전환한 다음 명령줄에서 다음을 실행합니다.

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

`dotnet add package` 명령에서 `-v` 한정자를 사용하여 특정 버전 설치를 지시할 수 있습니다. 예를 들어 EF Core 2.0 패키지를 설치하려면 명령에 `-v 2.0.0`을 추가합니다.

EF Core에는 `dotnet ef`로 시작하는 [`dotnet` CLI](../../miscellaneous/cli/dotnet.md)에 대한 추가 명령 집합이 있습니다. `dotnet ef` CLI 명령을 사용하려면 응용 프로그램의 `.csproj` 파일에 다음 항목이 있어야 합니다.

``` xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
</ItemGroup>
```

EF Core용.NET Core CLI 도구에도 Microsoft.EntityFrameworkCore.Design이라고 하는 별도의 패키지가 필요합니다. 다음을 통해 프로젝트에 추가하기만 하면 됩니다.

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Design
```

> [!IMPORTANT]  
> 항상 런타임 페키지의 주 버전에 부합하는 도구 패키지 버전을 사용합니다.

<a name="visual-studio"></a>
### <a name="visual-studio-development"></a>Visual Studio 개발 

Visual Studio를 사용하여 .NET Core, .NET Framework 또는 기타 EF Core에서 지원하는 플랫폼을 대상으로 하는 여러 다양한 응용 프로그램 유형을 개발할 수 있습니다.

두 가지 방법으로 Visual Studio에서 응용 프로그램에 EF Core 데이터베이스 공급자를 설치할 수 있습니다.

#### <a name="using-nugets-package-manager-user-interfacehttpsdocsmicrosoftcomnugettoolspackage-manager-ui"></a>NuGet의 [패키지 관리자 사용자 인터페이스](https://docs.microsoft.com/nuget/tools/package-manager-ui) 사용

* **프로젝트 > NuGet 패키지 관리** 메뉴 선택

* **찾아보기** 또는 **업데이트** 탭 클릭

* `Microsoft.EntityFrameworkCore.SqlServer` 패키지와 원하는 버전을 선택하고 확인

#### <a name="using-nugets-package-manager-console-pmchttpsdocsmicrosoftcomnugettoolspackage-manager-console"></a>NuGet의 [PMC(패키지 관리자 콘솔)](https://docs.microsoft.com/nuget/tools/package-manager-console) 사용

* **도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔** 메뉴 선택

* PMC에서 다음 명령 입력 및 실행

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* 이미 설치된 패키지를 더 최신 버전으로 업데이트하는 대신 `Update-Package` 명령을 사용할 수 있습니다.

* 특정 버전을 지정하려면 `-Version` 한정자를 사용합니다. 즉 EF Core 2.0 패키지를 설치하려면 명령에 `-Version 2.0.0`을 추가합니다.

#### <a name="tools"></a>도구

Visual Studio에서[ PMC 내부에서 실행되는 EF Core 명령](../../miscellaneous/cli/powershell.md)의 PowerShell 버전도 있습니다. 기능은 `dotnet ef` 명령과 유사합니다. 이 버전을 사용하려면 패키지 관리자 UI 또는 PMC를 사용하여 `Microsoft.EntityFrameworkCore.Tools` 패키지를 설치합니다.

> [!IMPORTANT]  
> 항상 런타임 페키지의 주 버전에 부합하는 도구 패키지 버전을 사용합니다.

> [!TIP]  
> Visual Studio의 PMC에서 `dotnet ef` 명령을 사용할 수 있으나 PowerShell 버전만큼 편리하지 않습니다.
> * 수동으로 디렉터리를 전환하지 않고 PMC에서 선택한 현재 프로젝트에서 자동으로 작동합니다.  
> * 명령을 완료하면 자동으로 Visual Studio에서 명령을 통해 생성된 파일을 엽니다.

> [!IMPORTANT]  
> **EF Core 2.0에서 사용되지 않는 패키지:** EF Core 2.0으로 기존 응용 프로그램을 업그레이드하면 구 버전 EF Core 패키지에 대한 몇 가지 참조를 수동으로 제거해야 할 수 있습니다. 특히, `Microsoft.EntityFrameworkCore.SqlServer.Design` 같은 데이터베이스 공급자 설계 시간 패키지는 EF Core 2.0에서 더 이상 지원되지 않거나 필요하지 않지만 다른 패키지를 업그레이드할 때 자동으로 제거되지도 않습니다.
