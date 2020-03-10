---
title: Entity Framework Core 설치 - EF Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 987b6f38954c291f88b5167fa9b061853b15a6cb
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78412858"
---
# <a name="installing-entity-framework-core"></a>Entity Framework Core 설치

## <a name="prerequisites"></a>사전 요구 사항

* EF Core는 [.NET Standard 2.1](/dotnet/standard/net-standard) 라이브러리입니다. 따라서 EF Core는 .NET Standard 2.1을 지원하는 .NET 구현이 필요합니다. EF Core는 다른 .NET Standard 2.1 라이브러리에서도 참조할 수 있습니다.

* 예를 들어 .NET Core를 사용하여 EF Core를 대상으로 하는 앱을 개발할 수 있습니다. .NET Core 앱을 빌드하려면 [.NET Core SDK](https://dotnet.microsoft.com/download)가 필요합니다. 필요에 따라 [Visual Studio](https://visualstudio.microsoft.com/vs), [Mac용 Visual Studio](https://visualstudio.microsoft.com/vs/mac) 또는 [Visual Studio Code](https://code.visualstudio.com)와 같은 개발 환경을 사용할 수도 있습니다. 자세한 내용은 [.NET Core 시작](/dotnet/core/get-started)을 확인하세요.

* EF Core를 사용하면 Windows에서 Visual Studio를 사용하여 애플리케이션을 개발할 수 있습니다. [Visual Studio](https://visualstudio.microsoft.com/vs)의 최신 버전을 사용하는 것이 좋습니다.

* EF Core는 [Xamarin](https://dotnet.microsoft.com/apps/xamarin) 및 .NET 네이티브와 같은 다른 .NET 구현에서도 실행할 수 있습니다. 하지만 실제로 해당 구현에는 앱에서 EF Core의 작동 방식에 영향을 미칠 수 있는 런타임 제한이 있습니다. 자세한 내용은 [EF Core에서 지원되는 .NET 구현](xref:core/platforms/index)을 참조하세요.

* 마지막으로, 다른 데이터베이스 공급자는 특정 데이터베이스 엔진 버전, .NET 구현 또는 운영 체제가 필요할 수 있습니다. 애플리케이션에 적합한 환경을 지원하는 [EF Core 데이터베이스 공급자](xref:core/providers/index)를 사용할 수 있는지 확인하세요.

## <a name="get-the-entity-framework-core-runtime"></a>Entity Framework Core 런타임 가져오기

EF Core를 애플리케이션에 추가하려면 사용할 데이터베이스 공급자에 대한 NuGet 패키지를 설치하세요.

ASP.NET Core 애플리케이션을 빌드하는 경우 메모리 내 및 SQL Server 공급자를 설치할 필요가 없습니다. 이러한 공급자는 EF Core 런타임와 함께 ASP.NET Core의 현재 버전에 포함되어 있습니다.  

NuGet 패키지를 설치하거나 업데이트하려면 .NET Core CLI(명령줄 인터페이스), Visual Studio 패키지 관리자 대화 상자 또는 Visual Studio 패키지 관리자 콘솔을 사용할 수 있습니다.

### <a name="net-core-cli"></a>.NET Core CLI

* 운영 체제의 명령줄에서 다음 .NET Core CLI 명령을 사용하여 EF Core SQL Server 공급자를 설치하거나 업데이트합니다.

  ```dotnetcli
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
  ```

* `dotnet add package` 명령에서 `-v` 한정자를 사용하여 특정 버전을 나타낼 수 있습니다. 예를 들어 EF Core 2.2.0 패키지를 설치하려면 명령에 `-v 2.2.0`을 추가합니다.

자세한 내용은 [.NET CLI(명령줄 인터페이스 도구)](/dotnet/core/tools/)를 참조하세요.

### <a name="visual-studio-nuget-package-manager-dialog"></a>Visual Studio NuGet 패키지 관리자 대화 상자

* Visual Studio 메뉴에서 **프로젝트 > NuGet 패키지 관리** 선택

* **찾아보기** 또는 **업데이트** 탭 클릭

* SQL Server 공급자를 설치하거나 업데이트하려면 `Microsoft.EntityFrameworkCore.SqlServer` 패키지를 선택하고 확인합니다.

자세한 내용은 [NuGet 패키지 관리자 대화 상자](/nuget/tools/package-manager-ui)를 참조하십시오.

### <a name="visual-studio-nuget-package-manager-console"></a>Visual Studio NuGet 패키지 관리자 콘솔

* Visual Studio 메뉴에서 **도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔** 선택

* SQL Server 공급자를 설치하려면 패키지 관리자 콘솔에서 다음 명령을 실행합니다.

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```

* 공급자를 업데이트하려면 `Update-Package` 명령을 사용합니다.

* 특정 버전을 지정하려면 `-Version` 한정자를 사용합니다. 예를 들어 EF Core 2.2.0 패키지를 설치하려면 명령에 `-Version 2.2.0`을 추가합니다.

자세한 내용은 [패키지 관리자 콘솔](/nuget/tools/package-manager-console)을 참조하십시오.

## <a name="get-the-entity-framework-core-tools"></a>Entity Framework Core 도구 가져오기

프로젝트에서 EF Core 관련 작업(예: 데이터베이스 마이그레이션 생성 및 적용)을 수행하거나 기존 데이터베이스를 기반으로 EF Core 모델을 생성하는 도구를 설치할 수 있습니다.

사용 가능한 두 가지 도구 집합:

* [.NET Core CLI(명령줄 인터페이스) 도구](xref:core/miscellaneous/cli/dotnet)는 Windows, Linux 또는 macOS에서 사용할 수 있습니다. 이러한 명령은 `dotnet ef`로 시작합니다.

* [PMC(패키지 관리자 콘솔 도구)](xref:core/miscellaneous/cli/powershell)는 Windows의 Visual Studio에서 실행됩니다. 이러한 명령은 동사(예: `Add-Migration`, `Update-Database`)로 시작합니다.

패키지 관리자 콘솔에서 `dotnet ef` 명령을 사용할 수도 있지만 Visual Studio를 사용할 경우 패키지 관리자 콘솔 도구를 사용하는 것이 좋습니다.

* 수동으로 디렉터리를 전환하지 않고 Visual Studio의 PMC에서 선택한 현재 프로젝트에서 자동으로 작동합니다.  

* 명령을 완료하면 자동으로 Visual Studio에서 명령을 통해 생성된 파일을 엽니다.

<a name="cli"></a>

### <a name="get-the-net-core-cli-tools"></a>.NET Core CLI 도구 가져오기

.NET core CLI 도구에는 [필수 구성 요소](#prerequisites)에서 앞서 언급한 .NET Core SDK를 필요로 합니다.

`dotnet ef` 명령은 .NET Core SDK의 최신 버전에 포함되어 있지만, 특정 프로젝트에서 명령을 사용하려면 `Microsoft.EntityFrameworkCore.Design` 패키지를 설치해야 합니다.

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Design
```

> [!IMPORTANT]
> 항상 런타임 패키지의 주 버전에 부합하는 도구 패키지 버전을 사용합니다.

### <a name="get-the-package-manager-console-tools"></a>패키지 관리자 콘솔 도구 가져오기

EF Core용 패키지 관리자 콘솔 도구를 가져오려면 `Microsoft.EntityFrameworkCore.Tools` 패키지를 설치합니다. 예를 들어 Visual Studio에서:

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

ASP.NET Core 앱의 경우 이 패키지는 자동으로 포함됩니다.

## <a name="upgrading-to-the-latest-ef-core"></a>최신 EF Core로 업그레이드

* 새로운 버전의 EF Core를 릴리스할 때마다 Microsoft.EntityFrameworkCore.SqlServer, Microsoft.EntityFrameworkCore.Sqlite 및 Microsoft.EntityFrameworkCore.InMemory와 같은 EF Core 프로젝트의 일부인 새 버전의 공급자도 릴리스합니다. 모든 개선 사항을 가져오려면 새 버전의 공급자로 업그레이드하면 됩니다.

* EF Core는 SQL Server 및 메모리 내 공급자와 함께 ASP.NET Core의 현재 버전에 포함되어 있습니다. 기존 ASP.NET Core 애플리케이션을 최신 버전의 EF Core로 업그레이드하려면 항상 ASP.NET Core 버전을 업그레이드합니다.

* 타사 데이터베이스 공급자를 사용하는 애플리케이션을 업데이트해야 할 경우 항상 공급자의 업데이트가 사용할 EF Core 버전과 호환되는지 확인합니다. 예를 들어 이전 버전에 대한 데이터베이스 공급자는 EF Core 런타임 버전 2.0과 호환되지 않습니다.

* EF Core용 타사 공급자는 일반적으로 EF Core 런타임과 함께 패치 버전을 릴리스하지 않습니다. 타사 공급자를 사용하는 애플리케이션을 EF Core의 패치 버전으로 업그레이드하려면 Microsoft.EntityFrameworkCore 및 Microsoft.EntityFrameworkCore.Relational과 같은 개별 EF Core 런타임 구성 요소에 직접 참조를 추가해야 할 수 있습니다.

* 기존 애플리케이션을 최신 버전의 EF Core로 업그레이드할 경우, 이전 EF Core 패키지에 대한 일부 참조를 수동으로 제거해야 할 수 있습니다.

  * `Microsoft.EntityFrameworkCore.SqlServer.Design`과 같은 데이터베이스 공급자 디자인 타임 패키지는 EF Core 2.0 이상에서 더 이상 필요하거나 지원되지 않지만, 다른 패키지를 업그레이드할 때 자동으로 제거되지는 않습니다.

  * .NET CLI 도구는 버전 2.1부터 .NET SDK에 포함되므로 해당 패키지에 대한 참조를 프로젝트 파일에서 제거할 수 있습니다.

    ``` xml
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
    ```
