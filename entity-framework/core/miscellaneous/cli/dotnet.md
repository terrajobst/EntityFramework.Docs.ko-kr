---
title: EF Core 도구 참조 (.NET CLI)-EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/20/2018
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 87b9c73e32eddbf48cd3408de93245d9974efdce
ms.sourcegitcommit: 7a7da65404c9338e1e3df42576a13be536a6f95f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/06/2018
ms.locfileid: "48834788"
---
# <a name="entity-framework-core-tools-reference---net-cli"></a>Entity Framework Core 참조-.NET CLI 도구

Entity Framework Core에 대 한 명령줄 인터페이스 (CLI) 도구는 디자인 타임 개발 작업을 수행 합니다. 만드는 예를 들어 [마이그레이션을](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations)마이그레이션을 적용 하 고 기존 데이터베이스를 기반으로 모델에 대 한 코드를 생성 합니다. 플랫폼 간 확장 명령이 [dotnet](/dotnet/core/tools) 명령에 포함 되어 있는의 합니다 [.NET Core SDK](https://www.microsoft.com/net/core)합니다. 이러한 도구는.NET Core 프로젝트를 사용 하 여 작동합니다.

경우에 Visual Studio를 사용 하는, 권장 합니다 [패키지 관리자 콘솔 도구](powershell.md) 대신:
* 선택한 현재 프로젝트를 사용 하 여 자동으로 작동 합니다 **패키지 관리자 콘솔** 디렉터리를 수동으로 전환 하지 않고도 합니다.
* 이러한 명령이 완료 되 면 명령에 의해 생성 된 파일을 자동으로 열립니다.

## <a name="installing-the-tools"></a>도구 설치

설치 절차는 프로젝트 형식 및 버전에 따라 달라 집니다.

* ASP.NET Core 2.1 이상 버전
* EF Core 2.x
* EF Core 1.x

### <a name="aspnet-core-21"></a>ASP.NET Core 2.1 이상

* 현재 설치 [.NET Core SDK](https://www.microsoft.com/net/download/core)합니다. 최신 버전의 Visual Studio 2017이 있는 경우에도 이 SDK를 설치해야 합니다.

  때문에 ASP.NET Core 2.1 이상에 필요한 모든 이것이 합니다 `Microsoft.EntityFrameworkCore.Design` 패키지에 포함 됩니다 합니다 [Microsoft.AspNetCore.App 메타 패키지](/aspnet/core/fundamentals/metapackage-app)합니다.

### <a name="ef-core-2x-not-aspnet-core"></a>EF Core 2.x (ASP.NET Core 아님)

합니다 `dotnet ef` 명령을 사용할 수 있도록 설치 해야 하는.NET Core SDK에 포함 된를 `Microsoft.EntityFrameworkCore.Design` 패키지 있습니다.

* 현재 설치 [.NET Core SDK](https://www.microsoft.com/net/download/core)합니다. 최신 버전의 Visual Studio 2017이 있는 경우에도 이 SDK를 설치해야 합니다.

* 안정적인 최신 설치 `Microsoft.EntityFrameworkCore.Design` 패키지 있습니다.

  ``` Console
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

### <a name="ef-core-1x"></a>EF Core 1.x

* .NET Core SDK 2.1.200 버전을 설치 합니다. 이상 버전 EF Core 1.0 및 1.1에 대 한 CLI 도구를 사용 하 여 호환 되지 않습니다.

* 응용 프로그램 사용을 2.1.200 SDK 버전을 수정 하 여 구성할 해당 [global.json](/dotnet/core/tools/global-json) 파일입니다. 이 파일은 일반적으로 솔루션 디렉터리 (프로젝트 위에서 하나)에 포함 됩니다.

* 프로젝트 파일을 편집 하 고 추가 `Microsoft.EntityFrameworkCore.Tools.DotNet` 으로 `DotNetCliToolReference` 항목입니다. 예를 들어 최신 1.x 버전을 지정 합니다. 1.1.6 합니다. 이 섹션의 끝에 있는 프로젝트 파일 예제를 참조 하세요.

* 최신 1.x 버전의 설치는 `Microsoft.EntityFrameworkCore.Design` 예를 들어 패키지:

  ```console
  dotnet add package Microsoft.EntityFrameworkCore.Design -v 1.1.6
  ```

  두 패키지 참조 추가 사용 하 여 프로젝트 파일을 다음과 같습니다.

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

  패키지 참조를 사용 하 여 `PrivateAssets="All"` 이 프로젝트를 참조 하는 프로젝트에 노출 되지 않습니다. 이 제한은 일반적으로 개발 하는 동안에 사용 되는 패키지 특히 유용 합니다.

### <a name="verify-installation"></a>설치 확인

EF Core CLI 도구가 올바르게 설치 되어 있는지 확인 하려면 다음 명령을 실행 합니다.

  ``` Console
  dotnet restore
  dotnet ef
  ```

명령에서 출력 사용에서 도구 버전을 식별합니다.

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

## <a name="using-the-tools"></a>도구를 사용 하 여

도구를 사용 하기 전에 시작 프로젝트를 만들거나 환경을 설정 해야 합니다.

### <a name="target-project-and-startup-project"></a>대상 프로젝트와 시작 프로젝트

명령 참조는 *프로젝트* 와 *시작 프로젝트*합니다.

* 합니다 *프로젝트* 이 라고도 합니다 *대상 프로젝트* 명령을 추가 하거나 파일을 제거할 위치 이기 때문에 합니다. 기본적으로 현재 디렉터리에 있는 프로젝트에는 대상 프로젝트가 합니다. 사용 하 여 대상 프로젝트로 다른 프로젝트를 지정할 수 있습니다 합니다 <nobr> `--project` </nobr> 옵션입니다.

* 합니다 *시작 프로젝트* 도구 빌드 및 실행 하는 것입니다. 도구는 데이터베이스 연결 문자열 및 모델의 구성 등 프로젝트에 대 한 정보를 가져오려면 디자인 타임에 응용 프로그램 코드를 실행 해야 합니다. 기본적으로 현재 디렉터리에 있는 프로젝트에는 시작 프로젝트가 합니다. 사용 하 여 시작 프로젝트로 다른 프로젝트를 지정할 수 있습니다 합니다 <nobr> `--startup-project` </nobr> 옵션입니다.

시작 프로젝트 및 대상 프로젝트에서 동일한 프로젝트 많습니다. 일반적인 시나리오를 별도 프로젝트 되는 경우:

* EF Core 컨텍스트 및 엔터티 클래스는.NET Core 클래스 라이브러리에는.
* .NET Core 콘솔 앱 또는 웹 앱을 클래스 라이브러리를 참조 합니다.

수 이기도 [EF Core 컨텍스트를 별도 클래스 라이브러리에 마이그레이션 코드를 배치](xref:core/managing-schemas/migrations/projects)합니다.

### <a name="other-target-frameworks"></a>다른 대상 프레임 워크

CLI 도구는.NET Core 및.NET Framework 프로젝트를 사용 하 여 작동합니다. .NET Core 또는.NET Framework 프로젝트를.NET Standard 클래스 라이브러리는 EF Core 모델에 있는 앱 없을 수도 있습니다. 예를 들어, Xamarin 및 유니버설 Windows 플랫폼 앱의 마찬가지입니다. 이러한 경우를 도구용 시작 프로젝트 역할을 위해서만 사용 되는.NET Core 콘솔 앱 프로젝트를 만들 수 있습니다. 프로젝트는 실제 코드가 없는 임시 프로젝트 일 수 &mdash; 는 도구에 대 한 대상을 제공에 필요 합니다.

필요한 더미 프로젝트 이유는 무엇입니까? 앞에서 설명한 대로 도구는 디자인 타임에 응용 프로그램 코드를 실행 해야 합니다. 이렇게 하려면.NET Core 런타임을 사용 해야 합니다. EF Core 모델 프로젝트를.NET Core 또는.NET Framework를 대상으로 하는 경우 EF Core 도구는 프로젝트에서 런타임에 차용 합니다. EF Core 모델은.NET Standard 클래스 라이브러리에 해당 하는 수행할 수 없습니다. .NET Standard는 실제.NET 구현이 아닙니다. 이.NET 구현에서 지원 해야 하는 Api 집합의 사양. 따라서.NET Standard 부족 응용 프로그램 코드를 실행 하는 EF Core 도구에 대 한 합니다. 시작 프로젝트로 사용 하기 위해 만든 더미 프로젝트 도구는.NET Standard 클래스 라이브러리를 로드할 수 구체적인 대상 플랫폼을 제공 합니다.

### <a name="aspnet-core-environment"></a>ASP.NET Core 환경

ASP.NET Core 프로젝트에 대 한 환경에 지정 하려면 합니다 **ASPNETCORE_ENVIRONMENT** 명령을 실행 하기 전에 환경 변수입니다.

## <a name="common-options"></a>일반 옵션

|                   | 옵션                            | 설명                                                                                                                                                                                                                                                   |
|:------------------|:----------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                   | `--json`                          | JSON 출력을 표시 합니다.                                                                                                                                                                                                                                             |
| <nobr>`-c`</nobr> | `--context <DBCONTEXT>`           | `DbContext` 클래스를 사용 합니다. 유일한 또는 정규화 된 네임 스페이스의 클래스 이름입니다.  이 옵션을 생략할 경우 EF Core 컨텍스트 클래스를 찾을 수 있습니다. 여러 컨텍스트 클래스의 경우이 옵션이 필요 합니다.                                            |
| `-p`              | `--project <PROJECT>`             | 대상 프로젝트의 프로젝트 폴더에 상대 경로입니다.  기본값은 현재 폴더입니다.                                                                                                                                                              |
| `-s`              | `--startup-project <PROJECT>`     | 시작 프로젝트의 프로젝트 폴더에 상대 경로입니다. 기본값은 현재 폴더입니다.                                                                                                                                                              |
|                   | `--framework <FRAMEWORK>`         | 합니다 [대상 프레임 워크 모니커](/dotnet/standard/frameworks#supported-target-framework-versions) 에 대 한 합니다 [대상 프레임 워크](/dotnet/standard/frameworks)합니다.  프로젝트 파일을 여러 대상 프레임 워크를 지정 하 고 그 중 하나를 선택 하려는 경우 사용 합니다. |
|                   | `--configuration <CONFIGURATION>` | 예를 들어 빌드 구성을: `Debug` 또는 `Release`합니다.                                                                                                                                                                                                   |
|                   | `--runtime <IDENTIFIER>`          | 패키지를 복원할 대상 런타임의 식별자입니다. RID(런타임 식별자) 목록은 [RID 카탈로그](/dotnet/core/rid-catalog)를 참조하세요.                                                                                                      |
| `-h`              | `--help`                          | 도움말 정보를 표시 합니다.                                                                                                                                                                                                                                        |
| `-v`              | `--verbose`                       | 자세한 정보 출력을 표시 합니다.                                                                                                                                                                                                                                          |
|                   | `--no-color`                      | 출력 색을 지정 하지 마십시오.                                                                                                                                                                                                                                        |
|                   | `--prefix-output`                 | 수준으로 출력 하는 접두사입니다.                                                                                                                                                                                                                                     |

## <a name="dotnet-ef-database-drop"></a>dotnet ef 데이터베이스 삭제

데이터베이스를 삭제합니다.

옵션:

|                   | 옵션                   | 설명                                              |
|:------------------|:-------------------------|:---------------------------------------------------------|
| <nobr>`-f`</nobr> | <nobr>`--force`</nobr>   | 확인 하지 마십시오.                                           |
|                   | <nobr>`--dry-run`</nobr> | 데이터베이스는 삭제할 수 있지만 삭제 하지는 마세요 보여 줍니다. |

## <a name="dotnet-ef-database-update"></a>dotnet ef 데이터베이스 업데이트

지정 된 마이그레이션 또는 마지막 마이그레이션을 위해 데이터베이스를 업데이트합니다.

인수:

| 인수      | 설명                                                                                                                                                                                                                                                     |
|:--------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<MIGRATION>` | 대상 마이그레이션입니다. 마이그레이션은 식별 이름 또는 id입니다. 숫자 0는 특별 한 의미 *첫 번째 마이그레이션을 시작 하기 전에* 하 고 모든 마이그레이션에 되돌릴 수 있습니다. 마이그레이션 없음이 지정 된 경우 명령을 기본값은 마지막 마이그레이션입니다. |

다음 예제에서는 지정 된 마이그레이션에 데이터베이스를 업데이트 합니다. 첫 번째 마이그레이션 이름을 사용 하 고 마이그레이션 ID를 사용 하 여 두 번째 키를 누릅니다.

```console
dotnet ef database update InitialCreate
dotnet ef database update 20180904195021_InitialCreate
```

## <a name="dotnet-ef-dbcontext-info"></a>dotnet ef dbcontext 정보

에 대 한 정보를 가져옵니다는 `DbContext` 형식입니다.

## <a name="dotnet-ef-dbcontext-list"></a>dotnet ef dbcontext 목록

사용 가능한 목록 `DbContext` 형식입니다.

## <a name="dotnet-ef-dbcontext-scaffold"></a>dotnet ef dbcontext 스 캐 폴드

에 대 한 코드 생성을 `DbContext` 및 데이터베이스에 대 한 엔터티 형식입니다. 엔터티 형식을 생성 하려면이 명령에 대 한 순서 대로 기본 키를 데이터베이스 테이블에 있어야 합니다.

인수:

| 인수       | 설명                                                                                                                                                                                                             |
|:---------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<CONNECTION>` | 데이터베이스에 연결 문자열입니다. ASP.NET Core 2.x 프로젝트에 대 한 값이 될 수 있습니다 *이름 =\<연결 문자열의 이름 >* 합니다. 이 경우 이름을 프로젝트에 대해 설정 된 구성 소스에서 제공 됩니다. |
| `<PROVIDER>`   | 공급자에서 사용 합니다. 일반적으로이 NuGet 패키지의 이름 예: `Microsoft.EntityFrameworkCore.SqlServer`합니다.                                                                                           |

옵션:

|                 | 옵션                                   | 설명                                                                                                                                                                    |
|:----------------|:-----------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>-d</nobr> | `--data-annotations`                     | (가능한 한) 경우 모델을 구성 하려면 특성을 사용 합니다. 이 옵션을 생략할 경우 fluent API만 사용 됩니다.                                                                |
| `-c`            | `--context <NAME>`                       | 이름을 합니다 `DbContext` 클래스를 생성 합니다.                                                                                                                                 |
|                 | `--context-dir <PATH>`                   | 적용할 디렉터리는 `DbContext` 에서 클래스 파일입니다. 프로젝트 디렉터리를 기준으로 경로입니다. 네임 스페이스는 폴더 이름에서 파생 됩니다.                                 |
| `-f`            | `--force`                                | 기존 파일을 덮어씁니다.                                                                                                                                                      |
| `-o`            | `--output-dir <PATH>`                    | 에 엔터티 클래스 파일을 배치할 디렉터리입니다. 프로젝트 디렉터리를 기준으로 경로입니다.                                                                                       |
|                 | <nobr>`--schema <SCHEMA_NAME>...`</nobr> | 에 대 한 엔터티 형식을 생성 하는 테이블의 스키마입니다. 여러 스키마를 지정 하려면 반복 `--schema` 각각에 대 한 합니다. 이 옵션을 생략 하면 모든 스키마 포함 됩니다.          |
| `-t`            | `--table <TABLE_NAME>`...                | 에 대 한 엔터티 형식을 생성 하는 테이블입니다. 여러 테이블을 지정 하려면 반복 `-t` 또는 `--table` 각각에 대 한 합니다. 이 옵션을 생략 하면 테이블을 모두 포함 됩니다.                |
|                 | `--use-database-names`                   | 데이터베이스에 표시 된 대로 테이블 및 열 이름을 사용 합니다. 이 옵션을 생략 하면, 데이터베이스 이름은 더욱 긴밀 하 게 C# 이름 스타일 규칙에 맞게 변경 됩니다. |

다음 예제에서는 모든 스키마 및 테이블을 스 캐 폴딩 하 고 새 파일에 저장 합니다 *모델* 폴더입니다.

```console
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -o Models
```

다음 예제에서는 선택한 테이블만 스 캐 폴딩 하 고 지정한 이름 가진 별도 폴더에 컨텍스트를 만듭니다.

```console
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -o Models -t Blog -t Post --context-dir Context -c BlogContext
```

## <a name="dotnet-ef-migrations-add"></a>dotnet ef migrations 추가

새 마이그레이션을 추가합니다.

인수:

| 인수 | 설명                |
|:---------|:---------------------------|
| `<NAME>` | 마이그레이션 이름입니다. |

옵션:

|                   | 옵션                             | 설명                                                                                                      |
|:------------------|:-----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <nobr>`-o`</nobr> | <nobr>`--output-dir <PATH>`</nobr> | 사용 하 여 디렉터리 (및 하위 네임 스페이스). 프로젝트 디렉터리를 기준으로 경로입니다. 기본값은 "마이그레이션"입니다. |

## <a name="dotnet-ef-migrations-list"></a>dotnet ef migrations 목록

사용할 수 있는 마이그레이션을 나열합니다.

## <a name="dotnet-ef-migrations-remove"></a>dotnet ef migrations 제거

마지막으로 마이그레이션 (마이그레이션에 대해 수행 된 코드 변경 내용을 롤백)를 제거 합니다.

옵션:

|                   | 옵션    | 설명                                                                     |
|:------------------|:----------|:--------------------------------------------------------------------------------|
| <nobr>`-f`</nobr> | `--force` | 마이그레이션 되돌리기 (데이터베이스에 적용 된 변경 내용 롤백). |

## <a name="dotnet-ef-migrations-script"></a>dotnet ef migrations 스크립트

마이그레이션에서 SQL 스크립트를 생성합니다.

인수:

| 인수 | 설명                                                                                                                                                   |
|:---------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<FROM>` | 마이그레이션을 시작 합니다. 마이그레이션은 식별 이름 또는 id입니다. 숫자 0는 특별 한 의미 *첫 번째 마이그레이션 전에*입니다. 기본값은 0입니다. |
| `<TO>`   | 끝 마이그레이션입니다. 기본값 마지막 마이그레이션입니다.                                                                                                         |

옵션:

|                   | 옵션            | 설명                                                        |
|:------------------|:------------------|:-------------------------------------------------------------------|
| <nobr>`-o`</nobr> | `--output <FILE>` | 스크립트를 쓸 파일입니다.                                   |
| `-i`              | `--idempotent`    | 모든 마이그레이션 데이터베이스에서 사용할 수 있는 스크립트를 생성 합니다. |

다음 예제에서는 InitialCreate 마이그레이션에 대 한 스크립트를 만듭니다.

```console
dotnet ef migrations script 0 InitialCreate
```

다음 예제에서는 InitialCreate 마이그레이션 후 모든 마이그레이션에 대 한 스크립트를 만듭니다.

```console
dotnet ef migrations script 20180904195021_InitialCreate
```
