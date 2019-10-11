---
title: EF Core 도구 참조 (패키지 관리자 콘솔)-EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/18/2018
uid: core/miscellaneous/cli/powershell
ms.openlocfilehash: 45370a82131da9db8b724fe395d41b1e3641fcf8
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181339"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a>Entity Framework Core 도구 참조-Visual Studio의 패키지 관리자 콘솔

Entity Framework Core 용 패키지 관리자 콘솔 (PMC) 도구는 디자인 타임 개발 작업을 수행 합니다. 예를 들어, [마이그레이션을](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0)만들고, 마이그레이션을 적용 하 고, 기존 데이터베이스를 기반으로 하는 모델에 대 한 코드를 생성 합니다. 명령은 [패키지 관리자 콘솔](/nuget/tools/package-manager-console)을 사용 하 여 Visual Studio 내부에서 실행 됩니다. 이러한 도구는 .NET Core와 .NET Framework 프로젝트 모두에서 작동합니다.

Visual Studio를 사용 하지 않는 경우 [EF Core 명령줄 도구](dotnet.md) 를 대신 사용 하는 것이 좋습니다. CLI 도구는 플랫폼 간 이며 명령 프롬프트 내에서 실행 됩니다.

## <a name="installing-the-tools"></a>도구 설치

도구를 설치 하 고 업데이트 하는 절차는 ASP.NET Core 2.1 이상 버전과 이전 버전 또는 다른 프로젝트 형식 간에 다릅니다.

### <a name="aspnet-core-version-21-and-later"></a>ASP.NET Core 버전 2.1 이상

@No__t-0 패키지가 [AspNetCore 메타 패키지](/aspnet/core/fundamentals/metapackage-app)에 포함 되어 있으므로 도구는 ASP.NET Core 2.1 이상 프로젝트에 자동으로 포함 됩니다.

따라서 도구를 설치 하기 위해 아무것도 수행할 필요가 없지만 다음을 수행 해야 합니다.
* 새 프로젝트에서 도구를 사용 하기 전에 패키지를 복원 합니다.
* 패키지를 설치 하 여 최신 버전으로 도구를 업데이트 합니다.

최신 버전의 도구를 사용 하 고 있는지 확인 하려면 다음 단계를 수행 하는 것이 좋습니다.

* *.Csproj* 파일을 편집 하 고 최신 버전의 [microsoft.entityframeworkcore.tools.dotnet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) 패키지를 지정 하는 줄을 추가 합니다. 예를 들어 *.csproj* 파일에는 다음과 같은 `ItemGroup`이 포함 될 수 있습니다.

  ```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="2.1.3" />
    <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="2.1.1" />
  </ItemGroup>
  ```

다음 예제와 같은 메시지가 표시 되 면 도구를 업데이트 합니다.

> EF Core 도구 버전 ' 2.1.1-30846 '은 (는) 런타임 ' 2.1.3-32065 ' 보다 이전 버전입니다. 최신 기능 및 버그 수정에 대 한 도구를 업데이트 합니다.

도구를 업데이트 하려면:
* 최신 .NET Core SDK를 설치 합니다.
* Visual Studio를 최신 버전으로 업데이트 합니다.
* 위와 같이 최신 도구 패키지에 대 한 패키지 참조를 포함 하도록 *.csproj* 파일을 편집 합니다.

### <a name="other-versions-and-project-types"></a>기타 버전 및 프로젝트 형식

**패키지 관리자**콘솔에서 다음 명령을 실행 하 여 패키지 관리자 콘솔 도구를 설치 합니다.

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

**패키지 관리자 콘솔**에서 다음 명령을 실행 하 여 도구를 업데이트 합니다.

``` powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a>설치 확인

다음 명령을 실행 하 여 도구가 설치 되어 있는지 확인 합니다.

``` powershell
Get-Help about_EntityFrameworkCore
```

출력은 다음과 같이 표시 됩니다. 사용 중인 도구의 버전을 알 수 없습니다.

```console

                     _/\__
               ---==/    \\
         ___  ___   |.    \|\
        | __|| __|  |  )   \\\
        | _| | _|   \_/ |  //|\\
        |___||_|       /   \\\/\\

TOPIC
    about_EntityFrameworkCore

SHORT DESCRIPTION
    Provides information about the Entity Framework Core Package Manager Console Tools.

<A list of available commands follows, omitted here.>
```

## <a name="using-the-tools"></a>도구 사용

도구를 사용 하기 전에:
* 대상과 시작 프로젝트의 차이점을 이해 합니다.
* .NET Standard 클래스 라이브러리와 함께 도구를 사용 하는 방법을 알아봅니다.
* ASP.NET Core 프로젝트의 경우 환경을 설정 합니다.

### <a name="target-and-startup-project"></a>대상 및 시작 프로젝트

명령은 *프로젝트* 와 *시작 프로젝트*를 참조 합니다.

* *프로젝트* 는 명령이 파일을 추가 하거나 제거 하는 위치 이기 때문에 *대상 프로젝트가* 라고도 합니다. 기본적으로 **패키지 관리자 콘솔** 에서 선택한 **기본 프로젝트** 는 대상 프로젝트입니다. <nobr>@No__t-1</nobr> 옵션을 사용 하 여 다른 프로젝트를 대상 프로젝트로 지정할 수 있습니다.

* *시작 프로젝트* 는 도구가 빌드하고 실행 하는 프로젝트입니다. 도구는 디자인 타임에 응용 프로그램 코드를 실행 하 여 데이터베이스 연결 문자열 및 모델 구성과 같은 프로젝트에 대 한 정보를 가져와야 합니다. 기본적으로 **솔루션 탐색기** 의 **시작 프로젝트** 는 시작 프로젝트입니다. <nobr>@No__t-1</nobr> 옵션을 사용 하 여 다른 프로젝트를 시작 프로젝트로 지정할 수 있습니다.

시작 프로젝트와 대상 프로젝트는 대개 동일한 프로젝트입니다. 개별 프로젝트의 일반적인 시나리오는 다음과 같습니다.

* EF Core 컨텍스트 및 엔터티 클래스는 .NET Core 클래스 라이브러리에 있습니다.
* .NET Core 콘솔 앱 또는 웹 앱은 클래스 라이브러리를 참조 합니다.

또한 [EF Core 컨텍스트와 별도의 클래스 라이브러리에 마이그레이션 코드](xref:core/managing-schemas/migrations/projects)를 추가할 수 있습니다.

### <a name="other-target-frameworks"></a>기타 대상 프레임 워크

패키지 관리자 콘솔 도구는 .NET Core 또는 .NET Framework 프로젝트에서 작동 합니다. .NET Standard 클래스 라이브러리에 EF Core 모델을 포함 하는 앱에는 .NET Core 또는 .NET Framework 프로젝트가 없을 수 있습니다. 예를 들어이는 Xamarin 및 유니버설 Windows 플랫폼 apps에서 적용 됩니다. 이러한 경우 도구에 대 한 시작 프로젝트 역할을 하는 용도로만 사용할 수 있는 .NET Core 또는 .NET Framework 콘솔 앱 프로젝트를 만들 수 있습니다. 프로젝트는 실제 코드가 없는 더미 프로젝트 일 수 있습니다 &mdash;은 도구에 대 한 대상을 제공 하는 데만 필요 합니다.

더미 프로젝트가 필요한 이유는 무엇 인가요? 앞에서 설명한 대로 도구는 디자인 타임에 응용 프로그램 코드를 실행 해야 합니다. 이렇게 하려면 .NET Core 또는 .NET Framework 런타임을 사용 해야 합니다. EF Core 모델이 .NET Core 또는 .NET Framework을 대상으로 하는 프로젝트에 있는 경우 EF Core 도구는 프로젝트에서 런타임을 만듭니다. EF Core 모델이 .NET Standard 클래스 라이브러리에 있는 경우에는이 작업을 수행할 수 없습니다. .NET Standard은 실제 .NET 구현이 아닙니다. .NET 구현에서 지원 해야 하는 Api 집합의 사양입니다. 따라서 .NET Standard EF Core 도구를 통해 응용 프로그램 코드를 실행할 수 없습니다. 시작 프로젝트로 사용 하기 위해 만드는 더미 프로젝트는 도구가 .NET Standard 클래스 라이브러리를 로드 하는 데 사용할 수 있는 구체적인 대상 플랫폼을 제공 합니다.

### <a name="aspnet-core-environment"></a>ASP.NET Core 환경

ASP.NET Core 프로젝트에 대 한 환경을 지정 하려면 명령을 실행 하기 전에 **env: ASPNETCORE_ENVIRONMENT** 를 설정 합니다.

## <a name="common-parameters"></a>일반 매개 변수

다음 표에서는 모든 EF Core 명령에 공통적인 매개 변수를 보여 줍니다.

| 매개 변수                 | 설명                                                                                                                                                                                                          |
|:--------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -Context \< 문자열 >        | 사용할 `DbContext` 클래스입니다. 네임 스페이스를 포함 하거나 정규화 된 클래스 이름입니다.  이 매개 변수를 생략 하면 EF Core 컨텍스트 클래스를 찾습니다. 컨텍스트 클래스가 여러 개인 경우이 매개 변수는 필수입니다. |
| -Project \<String>        | 대상 프로젝트입니다. 이 매개 변수를 생략 하면 **패키지 관리자 콘솔** 의 **기본 프로젝트가** 대상 프로젝트로 사용 됩니다.                                                                             |
| -StartupProject \<String> | 시작 프로젝트입니다. 이 매개 변수를 생략 하면 **솔루션 속성** 의 **시작 프로젝트가** 대상 프로젝트로 사용 됩니다.                                                                                 |
| -자세한 정보                  | 자세한 정보 출력을 표시합니다.                                                                                                                                                                                                 |

명령에 대 한 도움말 정보를 표시 하려면 PowerShell의 `Get-Help` 명령을 사용 합니다.

> [!TIP]
> Context, Project 및 StartupProject 매개 변수는 탭 확장을 지원 합니다.

## <a name="add-migration"></a>추가-마이그레이션

새 마이그레이션을 추가 합니다.

매개 변수:

| 매개 변수                         | 설명                                                                                                             |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| @no__t 64,--Name \< 문자열 > <nobr>       | 마이그레이션의 이름입니다. 이 매개 변수는 위치 매개 변수 이며 필수입니다.                                              |
| <nobr>-OutputDir \<String></nobr> | 사용할 디렉터리 및 하위 네임 스페이스입니다. 경로는 대상 프로젝트 디렉터리를 기준으로 합니다. 기본값은 "migration"입니다. |

## <a name="drop-database"></a>Drop Database

데이터베이스를 삭제 합니다.

매개 변수:

| 매개 변수 | 설명                                              |
|:----------|:---------------------------------------------------------|
| -WhatIf   | 삭제할 데이터베이스를 표시 하지만 삭제할 데이터베이스를 표시 하지 않습니다. |

## <a name="get-dbcontext"></a>Get-DbContext

0 @no__t 형식에 대 한 정보를 가져옵니다.

## <a name="remove-migration"></a>마이그레이션 제거

마이그레이션에 대해 수행 된 코드 변경 내용을 롤백하는 마지막 마이그레이션을 제거 합니다.

매개 변수:

| 매개 변수 | 설명                                                                     |
|:----------|:--------------------------------------------------------------------------------|
| -Force    | 마이그레이션을 되돌립니다 (데이터베이스에 적용 된 변경 내용 롤백). |

## <a name="scaffold-dbcontext"></a>스 캐 폴드-DbContext

데이터베이스에 대 한 `DbContext` 및 엔터티 형식에 대 한 코드를 생성 합니다. @No__t-0에서 엔터티 형식을 생성 하려면 데이터베이스 테이블에 기본 키가 있어야 합니다.

매개 변수:

| 매개 변수                          | 설명                                                                                                                                                                                                                                                             |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>-Connection \< 문자열 ></nobr> | 데이터베이스에 대 한 연결 문자열입니다. ASP.NET Core 2.x 프로젝트의 경우 값은 *이름 = \< 연결 문자열 > 이름일*수 있습니다. 이 경우 프로젝트에 대해 설정 된 구성 소스에서 이름이 제공 됩니다. 이 매개 변수는 위치 매개 변수 이며 필수입니다. |
| <nobr>-공급자 \< 문자열 ></nobr>   | 사용할 공급자입니다. 일반적으로 NuGet 패키지의 이름입니다 (예: `Microsoft.EntityFrameworkCore.SqlServer`). 이 매개 변수는 위치 매개 변수 이며 필수입니다.                                                                                           |
| -OutputDir \<String>               | 파일을 저장할 디렉터리입니다. 경로는 프로젝트 디렉터리에 상대적입니다.                                                                                                                                                                                             |
| -ContextDir \<String >              | @No__t-0 파일을 저장할 디렉터리입니다. 경로는 프로젝트 디렉터리에 상대적입니다.                                                                                                                                                                              |
| -Context \< 문자열 >                 | 생성할 `DbContext` 클래스의 이름입니다.                                                                                                                                                                                                                          |
| -스키마 \<String [] >               | 엔터티 형식을 생성할 테이블의 스키마입니다. 이 매개 변수를 생략 하면 모든 스키마가 포함 됩니다.                                                                                                                                                             |
| -Tables \<String [] >                | 엔터티 형식을 생성할 테이블입니다. 이 매개 변수를 생략 하면 모든 테이블이 포함 됩니다.                                                                                                                                                                         |
| -DataAnnotations                   | 특성을 사용 하 여 모델을 구성 합니다 (가능한 경우). 이 매개 변수를 생략 하면 흐름 API만 사용 됩니다.                                                                                                                                                      |
| -UseDatabaseNames                  | 테이블 및 열 이름은 데이터베이스에 표시 된 대로 정확 하 게 사용 합니다. 이 매개 변수를 생략 하는 경우 데이터베이스 이름이 이름 스타일 규칙을 C# 더욱 잘 준수 하도록 변경 됩니다.                                                                                       |
| -Force                             | 기존 파일을 덮어씁니다.                                                                                                                                                                                                                                               |

예:

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

예를 들어 선택한 테이블만 스 캐 폴드 지정 된 이름의 개별 폴더에 컨텍스트를 만듭니다.

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext
```

## <a name="script-migration"></a>스크립트-마이그레이션

선택한 마이그레이션의 모든 변경 내용을 선택한 다른 마이그레이션에 적용 하는 SQL 스크립트를 생성 합니다.

매개 변수:

| 매개 변수                | 설명                                                                                                                                                                                                                |
|:-------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| *-From* \<String>        | 마이그레이션을 시작 하는 중입니다. 마이그레이션은 이름 또는 ID로 식별할 수 있습니다. 숫자 0은 *첫 번째 마이그레이션 이전*을 의미 하는 특수 한 경우입니다. 기본값은 0입니다.                                                              |
| *-To* \<String>          | 종료 마이그레이션입니다. 마지막 마이그레이션에 대 한 기본값입니다.                                                                                                                                                                      |
| <nobr>-Idempotent</nobr> | 마이그레이션할 때 데이터베이스에서 사용할 수 있는 스크립트를 생성 합니다.                                                                                                                                                         |
| -Output \< 문자열 >        | 결과를 쓸 파일입니다. 이 매개 변수를 생략 하면 응용 프로그램의 런타임 파일이 생성 되는 폴더와 동일한 폴더에 생성 된 이름으로 파일이 생성 됩니다 (예: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/* ). |

> [!TIP]
> To, From 및 Output 매개 변수는 탭 확장을 지원 합니다.

다음 예에서는 마이그레이션 이름을 사용 하 여 InitialCreate migration에 대 한 스크립트를 만듭니다.

```powershell
Script-Migration -To InitialCreate
```

다음 예에서는 마이그레이션 ID를 사용 하 여 InitialCreate migration 이후의 모든 마이그레이션에 대 한 스크립트를 만듭니다.

```powershell
Script-Migration -From 20180904195021_InitialCreate
```

## <a name="update-database"></a>업데이트-데이터베이스

데이터베이스를 마지막 마이그레이션 또는 지정 된 마이그레이션으로 업데이트 합니다.

| 매개 변수                           | 설명                                                                                                                                                                                                                                                     |
|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr> *-Migration* \< 문자열 ></nobr> | 대상 마이그레이션입니다. 마이그레이션은 이름 또는 ID로 식별할 수 있습니다. 숫자 0은 *첫 번째 마이그레이션 이전* 을 의미 하는 특별 한 경우로, 모든 마이그레이션이 되돌려집니다. 마이그레이션이 지정 되지 않은 경우이 명령은 기본적으로 마지막 마이그레이션을 설정 합니다. |

> [!TIP]
> Migration 매개 변수는 탭 확장을 지원 합니다.

다음 예에서는 모든 마이그레이션을 되돌립니다.

```powershell
Update-Database -Migration 0
```

다음 예에서는 데이터베이스를 지정 된 마이그레이션으로 업데이트 합니다. 첫 번째는 마이그레이션 이름을 사용 하 고 두 번째는 마이그레이션 ID를 사용 합니다.

```powershell
Update-Database -Migration InitialCreate
Update-Database -Migration 20180904195021_InitialCreate
```

## <a name="additional-resources"></a>추가 자료

* [마이그레이션](xref:core/managing-schemas/migrations/index)
* [리버스 엔지니어링](xref:core/managing-schemas/scaffolding)
