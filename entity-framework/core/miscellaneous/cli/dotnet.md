---
title: EF Core 도구 참조 (.NET CLI)-EF Core
author: bricelam
ms.author: bricelam
ms.date: 07/11/2019
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 7dc7a4404820a7c935648169cc6ff8d0f0118d87
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414226"
---
# <a name="entity-framework-core-tools-reference---net-cli"></a>Entity Framework Core 도구 참조-.NET CLI

Entity Framework Core에 대 한 CLI (명령줄 인터페이스) 도구는 디자인 타임 개발 작업을 수행 합니다. 예를 들어, [마이그레이션을](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0)만들고, 마이그레이션을 적용 하 고, 기존 데이터베이스를 기반으로 하는 모델에 대 한 코드를 생성 합니다. 명령은 [.NET Core SDK](https://www.microsoft.com/net/core)의 일부인 플랫폼 간 [dotnet](/dotnet/core/tools) 명령에 대 한 확장입니다. 이러한 도구는 .NET Core 프로젝트에서 작동 합니다.

Visual Studio를 사용 하는 경우 대신 [패키지 관리자 콘솔 도구](powershell.md) 를 사용 하는 것이 좋습니다.

* 수동으로 디렉터리를 전환 하지 않고도 **패키지 관리자 콘솔** 에서 선택한 현재 프로젝트와 함께 자동으로 작동 합니다.
* 명령이 완료 된 후 명령으로 생성 된 파일을 자동으로 엽니다.

## <a name="installing-the-tools"></a>도구 설치

설치 절차는 프로젝트 형식 및 버전에 따라 달라 집니다.

* EF Core 3(sp3)
* ASP.NET Core 버전 2.1 이상
* EF Core 2.x
* 1\.x EF Core

### <a name="ef-core-3x"></a>EF Core 3(sp3)

* `dotnet ef`는 전역 또는 로컬 도구로 설치 되어야 합니다. 대부분의 개발자는 다음 명령을 사용 하 여 `dotnet ef`를 전역 도구로 설치 합니다.

  ```dotnetcli
  dotnet tool install --global dotnet-ef
  ```

  `dotnet ef`를 로컬 도구로 사용할 수도 있습니다. 로컬 도구로 사용 하려면 [도구 매니페스트 파일](https://github.com/dotnet/cli/issues/10288)을 사용 하 여 도구를 도구 종속성으로 선언 하는 프로젝트의 종속성을 복원 합니다.

* [.NET Core SDK](https://www.microsoft.com/net/download/core)를 설치합니다.

* 최신 `Microsoft.EntityFrameworkCore.Design` 패키지를 설치 합니다.

  ```dotnetcli
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

### <a name="aspnet-core-21"></a>ASP.NET Core 2.1 이상

* 현재 [.NET Core SDK](https://www.microsoft.com/net/download/core)를 설치 합니다. 최신 버전의 Visual Studio 2017이 있는 경우에도 이 SDK를 설치해야 합니다.

  `Microsoft.EntityFrameworkCore.Design` 패키지가 [AspNetCore 메타 패키지](/aspnet/core/fundamentals/metapackage-app)에 포함 되어 있으므로이는 ASP.NET Core 2.1 이상에 필요 합니다.

### <a name="ef-core-2x-not-aspnet-core"></a>EF Core 2.x (ASP.NET Core 없음)

`dotnet ef` 명령은 .NET Core SDK에 포함 되어 있지만 명령을 사용 하도록 설정 하려면 `Microsoft.EntityFrameworkCore.Design` 패키지를 설치 해야 합니다.

* 현재 [.NET Core SDK](https://www.microsoft.com/net/download/core)를 설치 합니다. 최신 버전의 Visual Studio가 설치 되어 있는 경우에도 SDK를 설치 해야 합니다.

* 안정적인 최신 `Microsoft.EntityFrameworkCore.Design` 패키지를 설치 합니다.

  ```dotnetcli
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

### <a name="ef-core-1x"></a>1\.x EF Core

* .NET Core SDK 버전 2.1.200를 설치 합니다. 이후 버전은 EF Core 1.0 및 1.1에 대 한 CLI 도구와 호환 되지 않습니다.

* 2\.1.200 SDK 버전을 사용 하도록 응용 프로그램을 구성 하 여 [전역 json](/dotnet/core/tools/global-json) 파일을 수정 합니다. 이 파일은 일반적으로 솔루션 디렉터리 (프로젝트 위에 하나)에 포함 됩니다.

* 프로젝트 파일을 편집 하 고 `Microsoft.EntityFrameworkCore.Tools.DotNet`를 `DotNetCliToolReference` 항목으로 추가 합니다. 최신 1.x 버전을 지정 합니다 (예: 1.1.6). 이 섹션의 끝에 있는 프로젝트 파일 예제를 참조 하세요.

* `Microsoft.EntityFrameworkCore.Design` 패키지의 최신 1.x 버전을 설치 합니다. 예를 들면 다음과 같습니다.

  ```dotnetcli
  dotnet add package Microsoft.EntityFrameworkCore.Design -v 1.1.6
  ```

  두 패키지 참조가 모두 추가 되 면 프로젝트 파일은 다음과 같습니다.

  ``` xml
  <Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
      <OutputType>Exe</OutputType>
      <TargetFramework>netcoreapp1.1</TargetFramework>
    </PropertyGroup>
    <ItemGroup>
      <PackageReference Include="Microsoft.EntityFrameworkCore.Design"
                        Version="1.1.6"
                         PrivateAssets="All" />
    </ItemGroup>
    <ItemGroup>
       <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
                              Version="1.1.6" />
    </ItemGroup>
  </Project>
  ```

  `PrivateAssets="All"` 포함 된 패키지 참조가이 프로젝트를 참조 하는 프로젝트에 노출 되지 않습니다. 이 제한은 일반적으로 개발 중에만 사용 되는 패키지에 특히 유용 합니다.

### <a name="verify-installation"></a>설치 확인

다음 명령을 실행 하 EF Core CLI 도구가 올바르게 설치 되었는지 확인 합니다.

  ```dotnetcli
  dotnet restore
  dotnet ef
  ```

명령의 출력은 사용 중인 도구의 버전을 식별 합니다.

```console

                     _/\__
               ---==/    \\
         ___  ___   |.    \|\
        | __|| __|  |  )   \\\
        | _| | _|   \_/ |  //|\\
        |___||_|       /   \\\/\\

Entity Framework Core .NET Command-line Tools 2.1.3-rtm-32065

<Usage documentation follows, not shown.>
```

## <a name="using-the-tools"></a>도구 사용

도구를 사용 하기 전에 시작 프로젝트를 만들거나 환경을 설정 해야 할 수 있습니다.

### <a name="target-project-and-startup-project"></a>대상 프로젝트 및 시작 프로젝트

명령은 *프로젝트* 와 *시작 프로젝트*를 참조 합니다.

* *프로젝트* 는 명령이 파일을 추가 하거나 제거 하는 위치 이기 때문에 *대상 프로젝트가* 라고도 합니다. 기본적으로 현재 디렉터리에 있는 프로젝트는 대상 프로젝트입니다. <nobr>`--project`</nobr> 옵션을 사용 하 여 다른 프로젝트를 대상 프로젝트로 지정할 수 있습니다.

* *시작 프로젝트* 는 도구가 빌드하고 실행 하는 프로젝트입니다. 도구는 디자인 타임에 응용 프로그램 코드를 실행 하 여 데이터베이스 연결 문자열 및 모델 구성과 같은 프로젝트에 대 한 정보를 가져와야 합니다. 기본적으로 현재 디렉터리에 있는 프로젝트는 시작 프로젝트입니다. <nobr>`--startup-project`</nobr> 옵션을 사용 하 여 다른 프로젝트를 시작 프로젝트로 지정할 수 있습니다.

시작 프로젝트와 대상 프로젝트는 대개 동일한 프로젝트입니다. 개별 프로젝트의 일반적인 시나리오는 다음과 같습니다.

* EF Core 컨텍스트 및 엔터티 클래스는 .NET Core 클래스 라이브러리에 있습니다.
* .NET Core 콘솔 앱 또는 웹 앱은 클래스 라이브러리를 참조 합니다.

또한 [EF Core 컨텍스트와 별도의 클래스 라이브러리에 마이그레이션 코드](xref:core/managing-schemas/migrations/projects)를 추가할 수 있습니다.

### <a name="other-target-frameworks"></a>기타 대상 프레임 워크

CLI 도구는 .NET Core 프로젝트 및 .NET Framework 프로젝트에서 작동 합니다. .NET Standard 클래스 라이브러리에 EF Core 모델을 포함 하는 앱에는 .NET Core 또는 .NET Framework 프로젝트가 없을 수 있습니다. 예를 들어이는 Xamarin 및 유니버설 Windows 플랫폼 apps에서 적용 됩니다. 이러한 경우 도구에 대 한 시작 프로젝트 역할을 하는 용도로만 사용할 수 있는 .NET Core 콘솔 앱 프로젝트를 만들 수 있습니다. 프로젝트는 실제 코드가 없는 더미 프로젝트인 경우 도구에 대 한 대상을 제공 하기만 하면 됩니다 &mdash;.

더미 프로젝트가 필요한 이유는 무엇 인가요? 앞에서 설명한 대로 도구는 디자인 타임에 응용 프로그램 코드를 실행 해야 합니다. 이렇게 하려면 .NET Core 런타임을 사용 해야 합니다. EF Core 모델이 .NET Core 또는 .NET Framework을 대상으로 하는 프로젝트에 있는 경우 EF Core 도구는 프로젝트에서 런타임을 만듭니다. EF Core 모델이 .NET Standard 클래스 라이브러리에 있는 경우에는이 작업을 수행할 수 없습니다. .NET Standard은 실제 .NET 구현이 아닙니다. .NET 구현에서 지원 해야 하는 Api 집합의 사양입니다. 따라서 .NET Standard EF Core 도구를 통해 응용 프로그램 코드를 실행할 수 없습니다. 시작 프로젝트로 사용 하기 위해 만드는 더미 프로젝트는 도구가 .NET Standard 클래스 라이브러리를 로드 하는 데 사용할 수 있는 구체적인 대상 플랫폼을 제공 합니다.

### <a name="aspnet-core-environment"></a>ASP.NET Core 환경

ASP.NET Core 프로젝트에 대 한 환경을 지정 하려면 명령을 실행 하기 전에 **ASPNETCORE_ENVIRONMENT** 환경 변수를 설정 합니다.

## <a name="common-options"></a>일반 옵션

|                   | 옵션                            | 설명                                                                                                                                                                                                                                                   |
|:------------------|:----------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                   | `--json`                          | JSON 출력을 표시 합니다.                                                                                                                                                                                                                                             |
| <nobr>`-c`</nobr> | `--context <DBCONTEXT>`           | 사용할 `DbContext` 클래스입니다. 네임 스페이스를 포함 하거나 정규화 된 클래스 이름입니다.  이 옵션을 생략 하면 EF Core 컨텍스트 클래스가 검색 됩니다. 컨텍스트 클래스가 여러 개인 경우이 옵션이 필요 합니다.                                            |
| `-p`              | `--project <PROJECT>`             | 대상 프로젝트의 프로젝트 폴더에 대 한 상대 경로입니다.  기본값은 현재 폴더입니다.                                                                                                                                                              |
| `-s`              | `--startup-project <PROJECT>`     | 시작 프로젝트의 프로젝트 폴더에 대 한 상대 경로입니다. 기본값은 현재 폴더입니다.                                                                                                                                                              |
|                   | `--framework <FRAMEWORK>`         | [대상 프레임 워크](/dotnet/standard/frameworks)에 대 한 [대상 프레임 워크 모니커입니다](/dotnet/standard/frameworks#supported-target-framework-versions) .  프로젝트 파일이 여러 대상 프레임 워크를 지정 하 고 그 중 하나를 선택 하려는 경우에 사용 합니다. |
|                   | `--configuration <CONFIGURATION>` | 빌드 구성입니다 (예: `Debug` 또는 `Release`).                                                                                                                                                                                                   |
|                   | `--runtime <IDENTIFIER>`          | 패키지를 복원할 대상 런타임의 식별자입니다. RID(런타임 식별자) 목록은 [RID 카탈로그](/dotnet/core/rid-catalog)를 참조하세요.                                                                                                      |
| `-h`              | `--help`                          | 도움말 정보를 표시합니다.                                                                                                                                                                                                                                        |
| `-v`              | `--verbose`                       | 자세한 정보 출력을 표시합니다.                                                                                                                                                                                                                                          |
|                   | `--no-color`                      | 출력에 색을 표시 하지 않습니다.                                                                                                                                                                                                                                        |
|                   | `--prefix-output`                 | 수준으로 출력을 접두사로 사용 합니다.                                                                                                                                                                                                                                     |

## <a name="dotnet-ef-database-drop"></a>dotnet ef 데이터베이스 삭제

데이터베이스를 삭제합니다.

옵션:

|                   | 옵션                   | 설명                                              |
|:------------------|:-------------------------|:---------------------------------------------------------|
| <nobr>`-f`</nobr> | <nobr>`--force`</nobr>   | 확인 하지 않습니다.                                           |
|                   | <nobr>`--dry-run`</nobr> | 삭제할 데이터베이스를 표시 하지만 삭제할 데이터베이스를 표시 하지 않습니다. |

## <a name="dotnet-ef-database-update"></a>dotnet ef 데이터베이스 업데이트

데이터베이스를 마지막 마이그레이션 또는 지정 된 마이그레이션으로 업데이트 합니다.

인수:

| 인수      | 설명                                                                                                                                                                                                                                                     |
|:--------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<MIGRATION>` | 대상 마이그레이션입니다. 마이그레이션은 이름 또는 ID로 식별할 수 있습니다. 숫자 0은 *첫 번째 마이그레이션 이전* 을 의미 하는 특별 한 경우로, 모든 마이그레이션이 되돌려집니다. 마이그레이션이 지정 되지 않은 경우이 명령은 기본적으로 마지막 마이그레이션을 설정 합니다. |

다음 예에서는 데이터베이스를 지정 된 마이그레이션으로 업데이트 합니다. 첫 번째는 마이그레이션 이름을 사용 하 고 두 번째는 마이그레이션 ID를 사용 합니다.

```dotnetcli
dotnet ef database update InitialCreate
dotnet ef database update 20180904195021_InitialCreate
```

## <a name="dotnet-ef-dbcontext-info"></a>dotnet ef dbcontext 정보

`DbContext` 형식에 대 한 정보를 가져옵니다.

## <a name="dotnet-ef-dbcontext-list"></a>dotnet ef dbcontext 목록

사용 가능한 `DbContext` 유형을 나열 합니다.

## <a name="dotnet-ef-dbcontext-scaffold"></a>dotnet ef dbcontext 스 캐 폴드

데이터베이스에 대 한 `DbContext` 및 엔터티 형식에 대 한 코드를 생성 합니다. 이 명령이 엔터티 형식을 생성 하려면 데이터베이스 테이블에 기본 키가 있어야 합니다.

인수:

| 인수       | 설명                                                                                                                                                                                                             |
|:---------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<CONNECTION>` | 데이터베이스에 대한 연결 문자열입니다. ASP.NET Core 2.x 프로젝트의 경우 값은 *이름 =\<연결 문자열 > 이름일*수 있습니다. 이 경우 프로젝트에 대해 설정 된 구성 소스에서 이름이 제공 됩니다. |
| `<PROVIDER>`   | 사용할 공급자입니다. 일반적으로 NuGet 패키지의 이름입니다 (예: `Microsoft.EntityFrameworkCore.SqlServer`).                                                                                           |

옵션:

|                 | 옵션                                   | 설명                                                                                                                                                                    |
|:----------------|:-----------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>-d</nobr> | `--data-annotations`                     | 특성을 사용 하 여 모델을 구성 합니다 (가능한 경우). 이 옵션을 생략 하면 흐름 API만 사용 됩니다.                                                                |
| `-c`            | `--context <NAME>`                       | 생성할 `DbContext` 클래스의 이름입니다.                                                                                                                                 |
|                 | `--context-dir <PATH>`                   | `DbContext` 클래스 파일을 저장할 디렉터리입니다. 경로는 프로젝트 디렉터리에 상대적입니다. 네임 스페이스는 폴더 이름에서 파생 됩니다.                                 |
| `-f`            | `--force`                                | 기존 파일을 덮어씁니다.                                                                                                                                                      |
| `-o`            | `--output-dir <PATH>`                    | 엔터티 클래스 파일을 배치할 디렉터리입니다. 경로는 프로젝트 디렉터리에 상대적입니다.                                                                                       |
|                 | <nobr>`--schema <SCHEMA_NAME>...`</nobr> | 엔터티 형식을 생성할 테이블의 스키마입니다. 여러 스키마를 지정 하려면 각 스키마에 대해 `--schema`를 반복 합니다. 이 옵션을 생략 하면 모든 스키마가 포함 됩니다.          |
| `-t`            | `--table <TABLE_NAME>`...                | 엔터티 형식을 생성할 테이블입니다. 여러 테이블을 지정 하려면 각 항목에 대해 `-t` 또는 `--table`를 반복 합니다. 이 옵션을 생략 하면 모든 테이블이 포함 됩니다.                |
|                 | `--use-database-names`                   | 테이블 및 열 이름은 데이터베이스에 표시 된 대로 정확 하 게 사용 합니다. 이 옵션을 생략 하는 경우 데이터베이스 이름이 이름 스타일 규칙을 C# 더욱 잘 준수 하도록 변경 됩니다. |

다음 예에서는 모든 스키마 및 테이블을 스 캐 폴드 하 고 새 파일을 *모델* 폴더에 넣습니다.

```dotnetcli
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -o Models
```

다음 예에서는 선택한 테이블만 스 캐 폴드 지정 된 이름의 개별 폴더에 컨텍스트를 만듭니다.

```dotnetcli
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -o Models -t Blog -t Post --context-dir Context -c BlogContext
```

## <a name="dotnet-ef-migrations-add"></a>dotnet ef 마이그레이션 추가

새 마이그레이션을 추가 합니다.

인수:

| 인수 | 설명                |
|:---------|:---------------------------|
| `<NAME>` | 마이그레이션의 이름입니다. |

옵션:

|                   | 옵션                             | 설명                                                                                                      |
|:------------------|:-----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <nobr>`-o`</nobr> | <nobr>`--output-dir <PATH>`</nobr> | 사용할 디렉터리 및 하위 네임 스페이스입니다. 경로는 프로젝트 디렉터리에 상대적입니다. 기본값은 "migration"입니다. |

## <a name="dotnet-ef-migrations-list"></a>dotnet ef 마이그레이션 목록

사용 가능한 마이그레이션을 나열 합니다.

## <a name="dotnet-ef-migrations-remove"></a>dotnet ef 마이그레이션 제거

마이그레이션에 대해 수행 된 코드 변경 내용을 롤백하는 마지막 마이그레이션을 제거 합니다.

옵션:

|                   | 옵션    | 설명                                                                     |
|:------------------|:----------|:--------------------------------------------------------------------------------|
| <nobr>`-f`</nobr> | `--force` | 마이그레이션을 되돌립니다 (데이터베이스에 적용 된 변경 내용 롤백). |

## <a name="dotnet-ef-migrations-script"></a>dotnet ef 마이그레이션 스크립트

마이그레이션에서 SQL 스크립트를 생성 합니다.

인수:

| 인수 | 설명                                                                                                                                                   |
|:---------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<FROM>` | 마이그레이션을 시작 하는 중입니다. 마이그레이션은 이름 또는 ID로 식별할 수 있습니다. 숫자 0은 *첫 번째 마이그레이션 이전*을 의미 하는 특수 한 경우입니다. 기본값은 0입니다. |
| `<TO>`   | 종료 마이그레이션입니다. 마지막 마이그레이션에 대 한 기본값입니다.                                                                                                         |

옵션:

|                   | 옵션            | 설명                                                        |
|:------------------|:------------------|:-------------------------------------------------------------------|
| <nobr>`-o`</nobr> | `--output <FILE>` | 스크립트를 쓸 파일입니다.                                   |
| `-i`              | `--idempotent`    | 마이그레이션할 때 데이터베이스에서 사용할 수 있는 스크립트를 생성 합니다. |

다음 예에서는 InitialCreate migration에 대 한 스크립트를 만듭니다.

```dotnetcli
dotnet ef migrations script 0 InitialCreate
```

다음 예에서는 InitialCreate migration 후 모든 마이그레이션에 대 한 스크립트를 만듭니다.

```dotnetcli
dotnet ef migrations script 20180904195021_InitialCreate
```

## <a name="additional-resources"></a>추가 리소스

* [마이그레이션](xref:core/managing-schemas/migrations/index)
* [리버스 엔지니어링](xref:core/managing-schemas/scaffolding)
