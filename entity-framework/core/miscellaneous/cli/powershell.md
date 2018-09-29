---
title: EF Core 도구 참조 (패키지 관리자 콘솔)-EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/18/2018
uid: core/miscellaneous/cli/powershell
ms.openlocfilehash: cde3c1f75d33808259654dfd9e1de51e662e8092
ms.sourcegitcommit: ad1bdea58ed35d0f19791044efe9f72f94189c18
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47447198"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a>Entity Framework Core 도구 참조-Visual Studio에서 패키지 관리자 콘솔

Entity Framework Core에 대 한 패키지 관리자 콘솔 (PMC) 도구는 디자인 타임 개발 작업을 수행 합니다. 만드는 예를 들어 [마이그레이션을](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations)마이그레이션을 적용 하 고 기존 데이터베이스를 기반으로 모델에 대 한 코드를 생성 합니다. 명령을 사용 하 여 Visual Studio 내부에서 실행 합니다 [패키지 관리자 콘솔](/nuget/tools/package-manager-console)합니다. 이러한 도구는 .NET Core와 .NET Framework 프로젝트 모두에서 작동합니다.

권장 하는 경우에 Visual Studio를 사용 하지 않는, 합니다 [EF Core 명령줄 도구](dotnet.md) 대신 합니다. CLI 도구는 크로스 플랫폼 및 명령 프롬프트 내에서 실행 됩니다.

## <a name="installing-the-tools"></a>도구 설치

설치 및 업데이트 된 도구에 대 한 절차에서는 ASP.NET Core 2.1 이상 및 이전 버전 또는 기타 프로젝트 형식에 따라 다릅니다.

### <a name="aspnet-core-version-21-and-later"></a>ASP.NET Core 2.1 이상 버전

때문에 ASP.NET Core 2.1 이상 프로젝트에서 도구를 자동으로 포함 됩니다는 `Microsoft.EntityFrameworkCore.Tools` 패키지에 포함 됩니다 합니다 [Microsoft.AspNetCore.App 메타 패키지](/aspnet/core/fundamentals/metapackage-app)합니다.

따라서 tools를 설치 하려면 아무 작업도 수행할 필요가 없습니다 있지만 필요가 있습니다.
* 새 프로젝트의 도구를 사용 하기 전에 패키지를 복원 합니다.
* 도구를 최신 버전으로 업데이트 하는 패키지를 설치 합니다.

를 최신 버전의 도구를 보시기 했는지 확인 하려면 다음 단계를 수행할 수도 하는 것이 좋습니다.

* 편집 하 *.csproj* 파일을 최신 버전을 지정 하는 줄을 추가 합니다 [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) 패키지 합니다. 예를 들어, 합니다 *.csproj* 파일이 포함 될 수 있습니다는 `ItemGroup` 다음과 유사 합니다.

  ```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="2.1.3" />
    <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="2.1.1" />
  </ItemGroup>
  ```

다음 예제와 같이 메시지가 표시 되 면 도구를 업데이트 합니다.

> EF Core 도구 버전 '2.1.1-rtm-30846' 런타임 '2.1.3-rtm-32065' 보다 오래 되었습니다. 최신 기능 및 버그 수정에 대 한 도구를 업데이트 합니다.

도구를 업데이트 합니다.
* 최신.NET Core SDK를 설치 합니다.
* Visual Studio 최신 버전으로 업데이트 합니다.
* 편집 된 *.csproj* 앞에 표시 된 것 처럼 최신 도구 패키지에 대 한 패키지 참조를 포함 하도록 파일입니다.

### <a name="other-versions-and-project-types"></a>다른 버전 및 프로젝트 형식

다음 명령을 실행 하 여 패키지 관리자 콘솔 도구를 설치 **패키지 관리자 콘솔**:

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

다음 명령을 실행 하 여 도구를 업데이트 **패키지 관리자 콘솔**합니다.

``` powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a>설치 확인

이 명령을 실행 하 여 도구 설치 되어 있는지 확인 합니다.

``` powershell
Get-Help about_EntityFrameworkCore
```

출력은 다음과 같습니다 (이 알 수 없는 버전을 사용 하는 도구):

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

## <a name="using-the-tools"></a>도구를 사용 하 여

도구 사용 하기 전에:
* 대상 및 시작 프로젝트 간의 차이점을 이해 합니다.
* .NET Standard 클래스 라이브러리와 도구를 사용 하는 방법에 알아봅니다.
* ASP.NET Core 프로젝트에 대 한 환경을 설정 합니다.

### <a name="target-and-startup-project"></a>대상 및 시작 프로젝트

명령 참조는 *프로젝트* 와 *시작 프로젝트*합니다.

* 합니다 *프로젝트* 이 라고도 합니다 *대상 프로젝트* 명령을 추가 하거나 파일을 제거할 위치 이기 때문에 합니다. 기본적으로 **기본 프로젝트** 에서 선택한 **패키지 관리자 콘솔** 대상 프로젝트입니다. 사용 하 여 대상 프로젝트로 다른 프로젝트를 지정할 수 있습니다 합니다 <nobr> `--project` </nobr> 옵션입니다.

* 합니다 *시작 프로젝트* 도구 빌드 및 실행 하는 것입니다. 도구는 데이터베이스 연결 문자열 및 모델의 구성 등 프로젝트에 대 한 정보를 가져오려면 디자인 타임에 응용 프로그램 코드를 실행 해야 합니다. 기본적으로 **시작 프로젝트** 에 **솔루션 탐색기** 시작 프로젝트 인지 합니다. 사용 하 여 시작 프로젝트로 다른 프로젝트를 지정할 수 있습니다 합니다 <nobr> `--startup-project` </nobr> 옵션입니다.

시작 프로젝트 및 대상 프로젝트에서 동일한 프로젝트 많습니다. 일반적인 시나리오를 별도 프로젝트 되는 경우:

* EF Core 컨텍스트 및 엔터티 클래스는.NET Core 클래스 라이브러리에는.
* .NET Core 콘솔 앱 또는 웹 앱을 클래스 라이브러리를 참조 합니다.

수 이기도 [EF Core 컨텍스트를 별도 클래스 라이브러리에 마이그레이션 코드를 배치](xref:core/managing-schemas/migrations/projects)합니다.

### <a name="other-target-frameworks"></a>다른 대상 프레임 워크

패키지 관리자 콘솔 도구는.NET Core 또는.NET Framework 프로젝트를 사용 하 여 작동합니다. .NET Core 또는.NET Framework 프로젝트를.NET Standard 클래스 라이브러리는 EF Core 모델에 있는 앱 없을 수도 있습니다. 예를 들어, Xamarin 및 유니버설 Windows 플랫폼 앱의 마찬가지입니다. 이러한 경우를 도구용 시작 프로젝트 역할을 위해서만 사용 되는.NET Core 또는.net 콘솔 앱 프로젝트를 만들 수 있습니다. 프로젝트는 실제 코드가 없는 임시 프로젝트 일 수 &mdash; 는 도구에 대 한 대상을 제공에 필요 합니다.

필요한 더미 프로젝트 이유는 무엇입니까? 앞에서 설명한 대로 도구는 디자인 타임에 응용 프로그램 코드를 실행 해야 합니다. 이렇게 하려면.NET Core 또는.NET Framework 런타임을 사용 해야 합니다. EF Core 모델 프로젝트를.NET Core 또는.NET Framework를 대상으로 하는 경우 EF Core 도구는 프로젝트에서 런타임에 차용 합니다. EF Core 모델은.NET Standard 클래스 라이브러리에 해당 하는 수행할 수 없습니다. .NET Standard는 실제.NET 구현이 아닙니다. 이.NET 구현에서 지원 해야 하는 Api 집합의 사양. 따라서.NET Standard 부족 응용 프로그램 코드를 실행 하는 EF Core 도구에 대 한 합니다. 시작 프로젝트로 사용 하기 위해 만든 더미 프로젝트 도구는.NET Standard 클래스 라이브러리를 로드할 수 구체적인 대상 플랫폼을 제공 합니다. 

### <a name="aspnet-core-environment"></a>ASP.NET Core 환경

ASP.NET Core 프로젝트에 대 한 환경에 지정 하려면 **env:ASPNETCORE_ENVIRONMENT** 명령을 실행 하기 전에 합니다.

## <a name="common-parameters"></a>일반 매개 변수

다음 표에서 EF Core 명령은 모두에 공통 되는 매개 변수를 보여 줍니다.

| 매개 변수                 | 설명                                                                                                                                                                                                          |
|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 상황에 맞는 \<문자열 >        | `DbContext` 클래스를 사용 합니다. 유일한 또는 정규화 된 네임 스페이스의 클래스 이름입니다.  이 매개 변수를 생략 하면 EF Core 컨텍스트 클래스를 찾습니다. 여러 컨텍스트 클래스의 경우이 매개 변수는 필수입니다. |
| -프로젝트 \<문자열 >        | 대상 프로젝트입니다. 이 매개 변수를 생략 합니다 **기본 프로젝트** 에 대 한 **패키지 관리자 콘솔** 대상 프로젝트로 사용 됩니다.                                                                             |
| -StartupProject \<문자열 > | 시작 프로젝트입니다. 이 매개 변수를 생략 합니다 **시작 프로젝트** 에 **솔루션 속성** 대상 프로젝트로 사용 됩니다.                                                                                 |
| -Verbose                  | 자세한 정보 출력을 표시 합니다.                                                                                                                                                                                                 |

명령에 대 한 도움말 정보를 표시 하려면 PowerShell의 사용 하 여 `Get-Help` 명령입니다.

> [!TIP]
> 상황에 맞는, 프로젝트 및 StartupProject 매개 변수 탭 확장을 지원 합니다.

## <a name="add-migration"></a>추가 마이그레이션

새 마이그레이션을 추가합니다.

매개 변수:

| 매개 변수                         | 설명                                                                                                             |
|-----------------------------------|-------------------------------------------------------------------------------------------------------------------------|
| <nobr>-Name \<문자열 ><nobr>       | 마이그레이션 이름입니다. 위치 매개 변수 이며 필수입니다.                                              |
| <nobr>-OutputDir \<String></nobr> | 사용 하 여 디렉터리 (및 하위 네임 스페이스). 대상 프로젝트 디렉터리를 기준으로 경로입니다. 기본값은 "마이그레이션"입니다. |

## <a name="drop-database"></a>데이터베이스 삭제

데이터베이스를 삭제합니다.

매개 변수:

| 매개 변수 | 설명                                                |
|-----------|------------------------------------------------------------|
| -WhatIf   | 데이터베이스는 삭제할 수 있지만 삭제 하지는 마세요 보여 줍니다.   |

## <a name="get-dbcontext"></a>Get-DbContext

사용 가능한 목록 `DbContext` 형식입니다.

## <a name="remove-migration"></a>마이그레이션 제거

마지막으로 마이그레이션 (마이그레이션에 대해 수행 된 코드 변경 내용을 롤백)를 제거 합니다. 

매개 변수:

| 매개 변수 | 설명                                                                     |
|-----------|---------------------------------------------------------------------------------|
| -Force    | 마이그레이션 되돌리기 (데이터베이스에 적용 된 변경 내용 롤백). |

## <a name="scaffold-dbcontext"></a>DbContext 스 캐 폴드

에 대 한 코드 생성을 `DbContext` 및 데이터베이스에 대 한 엔터티 형식입니다.

매개 변수:

| 매개 변수                                  | 설명                                                                                                                                                                                                                                                             |
|--------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>연결 \<문자열 ></nobr>         | 데이터베이스에 연결 문자열입니다. ASP.NET Core 2.x 프로젝트에 대 한 값이 될 수 있습니다 *이름 =\<연결 문자열의 이름 >* 합니다. 이 경우 이름을 프로젝트에 대해 설정 된 구성 소스에서 제공 됩니다. 위치 매개 변수 이며 필수입니다. |
| <nobr>공급자 \<문자열 ></nobr>           | 공급자에서 사용 합니다. 일반적으로이 NuGet 패키지의 이름 예: `Microsoft.EntityFrameworkCore.SqlServer`합니다. 위치 매개 변수 이며 필수입니다.                                                                                           |
| -OutputDir \<문자열 >                       | 디렉터리에 파일 보관입니다. 프로젝트 디렉터리를 기준으로 경로입니다.                                                                                                                                                                                             |
| -ContextDir \<문자열 >                      | 적용할 디렉터리는 `DbContext` 파일입니다. 프로젝트 디렉터리를 기준으로 경로입니다.                                                                                                                                                                              |
| 상황에 맞는 \<문자열 >                         | 이름을 합니다 `DbContext` 클래스를 생성 합니다.                                                                                                                                                                                                                          |
| -스키마 \<String >                       | 에 대 한 엔터티 형식을 생성 하는 테이블의 스키마입니다. 이 매개 변수를 생략 하면 모든 스키마 포함 됩니다.                                                                                                                                                             |
| -테이블 \<String >                        | 에 대 한 엔터티 형식을 생성 하는 테이블입니다. 이 매개 변수를 생략 하면 테이블을 모두 포함 됩니다.                                                                                                                                                                         |
| -DataAnnotations                           | (가능한 한) 경우 모델을 구성 하려면 특성을 사용 합니다. 이 매개 변수를 생략 하면 fluent API만 사용 됩니다.                                                                                                                                                      |
| -UseDatabaseNames                          | 데이터베이스에 표시 된 대로 테이블 및 열 이름을 사용 합니다. 이 매개 변수를 생략 하면 더욱 긴밀 하 게 C# 이름 스타일 규칙에 맞게 데이터베이스 이름이 변경 됩니다.                                                                                       |
| -Force                                     | 기존 파일을 덮어씁니다.                                                                                                                                                                                                                                               |

예제:

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

지정한 이름 가진 별도 폴더에 컨텍스트를 만들고 선택한 테이블만 스 캐 폴딩 하는 예제:

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext
```

## <a name="script-migration"></a>스크립트 마이그레이션

에 적용 되는 모든 변경 내용을 하나 선택한 마이그레이션에서 선택한 마이그레이션 다른 SQL 스크립트를 생성 합니다.

매개 변수:

| 매개 변수           | 설명                                                                                                                                                                                                                |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  \<문자열 >   | 마이그레이션을 시작 합니다. 마이그레이션은 식별 이름 또는 id입니다. 숫자 0는 특별 한 의미 *첫 번째 마이그레이션 전에*입니다. 기본값은 0입니다.                                                              |
| *-에* \<문자열 >     | 끝 마이그레이션입니다. 기본값 마지막 마이그레이션입니다.                                                                                                                                                                      |
| <nobr>멱 등</nobr>         | 모든 마이그레이션 데이터베이스에서 사용할 수 있는 스크립트를 생성 합니다.                                                                                                                                                         |
| -출력 \<문자열 >   | 결과를 쓸 파일입니다. 예를 들어 앱의 런타임 파일이 생성 될 때 파일이 동일한 폴더에 생성 된 이름으로 만들어집니다이 매개 변수를 생략 합니다. */obj/Debug/netcoreapp2.1/ghbkztfz.sql/* 합니다. |

> [!TIP]
> 및 출력 매개 변수 탭 확장을 지원 합니다.

다음 예제에서는 마이그레이션 이름을 사용 하 여 InitialCreate 마이그레이션에 대 한 스크립트를 만듭니다.

```powershell
Script-Migration -To InitialCreate
```

다음 예제에서는 마이그레이션 ID를 사용 하 여 InitialCreate 마이그레이션 후 모든 마이그레이션에 대 한 스크립트를 만듭니다.

```powershell
Script-Migration -From 20180904195021_InitialCreate
```

## <a name="update-database"></a>데이터베이스 업데이트

지정 된 마이그레이션 또는 마지막 마이그레이션을 위해 데이터베이스를 업데이트합니다.

| 매개 변수                             | 설명                                                                                                                                                                                                                                                     |
|---------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>*마이그레이션* \<문자열 ></nobr>   | 대상 마이그레이션입니다. 마이그레이션은 식별 이름 또는 id입니다. 숫자 0는 특별 한 의미 *첫 번째 마이그레이션을 시작 하기 전에* 하 고 모든 마이그레이션에 되돌릴 수 있습니다. 마이그레이션 없음이 지정 된 경우 명령을 기본값은 마지막 마이그레이션입니다. |

> [!TIP]
> 마이그레이션 매개 변수 탭 확장을 지원합니다.

다음 예제에서는 모든 마이그레이션 되돌립니다.

```powershell
Update-Database -Migration 0
```

다음 예제에서는 지정 된 마이그레이션에 데이터베이스를 업데이트 합니다. 첫 번째 마이그레이션 이름을 사용 하 고 마이그레이션 ID를 사용 하 여 두 번째 키를 누릅니다.

```powershell
Update-Database -Migration InitialCreate
Update-Database -Migration 20180904195021_InitialCreate
```