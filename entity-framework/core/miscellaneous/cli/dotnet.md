---
title: .NET core CLI-EF 코어
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 721235b07e695efd8df43294e1f4e90c28ae83d7
ms.sourcegitcommit: 72e59e6af86b568653e1b29727529dfd7f65d312
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34754498"
---
<a name="ef-core-net-command-line-tools"></a>EF 핵심.NET 명령줄 도구
===============================
Entity Framework Core.NET 명령줄 도구는 플랫폼 간에 대 한 확장 **dotnet** 명령에 포함 된의 [.NET Core SDK][2]합니다.

> [!TIP]
> 권장 하는 경우에 Visual Studio를 사용 하는, [PMC 도구] [ 1] 대신 더 통합 된 환경을 제공 하므로 합니다.

<a name="installing-the-tools"></a>도구 설치
--------------------
> [!NOTE]
> .NET Core SDK 버전 2.1.300 최신 작성과 **dotnet ef** EF 코어 2.0 및 이상 버전을 호환 되는 명령입니다. 따라서 최신 버전의 EF 핵심 런타임 및.NET Core SDK를 사용 하는 경우 설치가 필요 하지 않습니다 및이 섹션의 나머지 부분을 무시할 수 있습니다.
>
> 반면에 **dotnet ef** 2.1.300.NET Core SDK 버전에 포함 되 고 최신 도구 EF 핵심 버전 1.0 및 1.1와 호환 되지 않습니다. .NET Core SDK 2.1.300 있는 컴퓨터에 이전 버전의 EF 코어를 사용 하는 프로젝트를 사용 하 여 작업할 수 또는 그 이상 버전이 설치 전에 2.1.200 버전 설치 해야는 SDK의 이전 또는 수정 하 여 이전 버전에 사용할 응용 프로그램을 구성 하 고 해당  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) 파일입니다. 이 파일은 일반적으로 솔루션 디렉터리 (프로젝트 위에서 하나)에 포함 됩니다. 그런 다음 아래 installlation 명령으로 진행할 수 있습니다.

.NET Core SDK의 이전 버전에서는 다음이 단계를 사용 하 여 EF 코어.NET 명령줄 도구를 설치할 수 있습니다.

1. 프로젝트 파일을 편집 하 고 Microsoft.EntityFrameworkCore.Tools.DotNet DotNetCliToolReference 항목 (아래 참조)로 추가
2. 다음 명령을 실행합니다.

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

결과 프로젝트 코드는 다음과 같아야 합니다.

``` xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Microsoft.EntityFrameworkCore.Design"
                      Version="2.0.0"
                      PrivateAssets="All" />
  </ItemGroup>
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
                            Version="2.0.0" />
  </ItemGroup>
</Project>
```

> [!NOTE]
> 사용 하 여 패키지 참조 `PrivateAssets="All"` 의미는 일반적으로 개발 하는 동안에 사용 되는 패키지에 대 한 특히 유용이 프로젝트를 참조 하는 프로젝트에 노출 되지 않습니다.

모든 것이 올바르게를 수행한 경우에 명령 프롬프트에서 다음 명령을 실행 할 수 있어야 합니다.

``` Console
dotnet ef
```

<a name="using-the-tools"></a>도구를 사용 하 여
---------------
명령을 실행할 때마다 두 개의 프로젝트가 포함 됩니다.

대상 프로젝트에는 모든 파일이 추가됩니다(또는 일부 경우 제거됨). 대상 프로젝트의 프로젝트는 현재 디렉터리에 기본적으로 사용 되지만 사용 하 여 변경할 수는 <nobr> **-프로젝트** </nobr> 옵션입니다.

시작 프로젝트는 프로젝트 코드를 실행할 때 도구가 에뮬레이트하는 프로젝트입니다. 사용 하 여 변경할 수 있지만 것 현재 디렉터리에 프로젝트에 기본적으로 **-시작 프로젝트** 옵션입니다.

> [!NOTE]
> 예를 들어, EF 코어 다른 프로젝트에 설치 되어 있는 웹 응용 프로그램 데이터베이스를 업데이트 하는 다음과 같습니다: `dotnet ef database update --project {project-path}` (에서 웹 응용 프로그램 디렉터리)

일반 옵션:

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | -json                           | JSON 출력을 표시 합니다.           |
| -c | -상황에 맞는 \<DBCONTEXT >           | 사용할 DbContext 합니다.       |
| -p | -프로젝트 \<프로젝트 >             | 사용 하는 프로젝트입니다.         |
| -s | -시작 프로젝트 \<프로젝트 >     | 시작 프로젝트 사용입니다. |
|    | -프레임 워크 \<프레임 워크 >         | 대상 프레임 워크입니다.       |
|    | -configuration \<구성 > | 사용할 구성입니다.   |
|    | -런타임 \<식별자 >          | 사용 하는 런타임입니다.         |
| -h | -도움말                           | 도움말 정보를 표시 합니다.      |
| -v | -verbose                        | 자세한 출력을 표시 합니다.        |
|    | -색 없음                       | 출력 색을 지정 하지 마십시오.      |
|    | --prefix-output                  | 수준으로 출력 하는 접두사입니다.   |


> [!TIP]
> ASP.NET Core 환경을 지정 하려면는 **ASPNETCORE_ENVIRONMENT** 실행 하기 전에 환경 변수입니다.

<a name="commands"></a>명령
--------

### <a name="dotnet-ef-database-drop"></a>dotnet ef 데이터베이스 삭제

데이터베이스를 삭제 합니다.

옵션:

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| -f | -force   | 확인 하지 마십시오.                                           |
|    | --dry-run | 표시는 데이터베이스는 삭제할 수 없지만 삭제 하지 않습니다. |

### <a name="dotnet-ef-database-update"></a>dotnet ef 데이터베이스 업데이트

지정 된 마이그레이션에는 데이터베이스를 업데이트합니다.

인수:

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| \<마이그레이션 &GT; | 대상 마이그레이션입니다. 0 인 경우, 모든 마이그레이션 되돌립니다. 마지막으로 마이그레이션을 기본값입니다. |

### <a name="dotnet-ef-dbcontext-info"></a>dotnet ef dbcontext 정보

DbContext 형식에 대 한 정보를 가져옵니다.

### <a name="dotnet-ef-dbcontext-list"></a>dotnet ef dbcontext 목록

사용 가능한 DbContext 유형을 나열합니다.

### <a name="dotnet-ef-dbcontext-scaffold"></a>dotnet ef dbcontext 스 캐 폴딩

스 캐 폴드 데이터베이스에 대 한 DbContext 및 엔터티 형식입니다.

인수:

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| \<연결 &GT; | 데이터베이스에 연결 문자열입니다.                                      |
| \<공급자 &GT;   | 사용 하는 공급자입니다. (예: Microsoft.EntityFrameworkCore.SqlServer) |

옵션:

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <nobr>-d</nobr> | -데이터 주석                      | (사용 가능한) 위치 모델을 구성 하려면 특성을 사용 합니다. 생략 하면 fluent API만 사용 됩니다. |
| -c              | -상황에 맞는 \<이름 >                       | DbContext의 이름입니다.                                                                       |
|                 | -dir 컨텍스트 \<경로 >                   | 디렉터리에 DbContext 파일을 넣습니다. 경로 프로젝트 디렉터리에 상대적입니다.             |
| -f              | -force                                 | 기존 파일을 덮어씁니다.                                                                        |
| -o              | -출력-dir \<경로 >                    | 에 파일을 넣을 디렉터리입니다. 경로 프로젝트 디렉터리에 상대적입니다.                      |
|                 | <nobr>--schema \<SCHEMA_NAME>...</nobr> | 에 대 한 엔터티 형식을 생성 하는 테이블의 스키마입니다.                                              |
| -t              | -테이블 \<TABLE_NAME >...                | 엔터티 형식에 대 한 생성을 위한 테이블입니다.                                                         |
|                 | -이름-데이터베이스-사용                    | 데이터베이스에서 직접 테이블 및 열 이름을 사용 합니다.                                           |

### <a name="dotnet-ef-migrations-add"></a>dotnet ef 마이그레이션 추가

새 마이그레이션을 추가합니다.

인수:

|         |                            |
|:--------|:---------------------------|
| \<이름 &GT; | 마이그레이션의 이름입니다. |

옵션:

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <nobr>-o</nobr> | <nobr>--output-dir \<PATH></nobr> | 사용할 디렉터리 (및 하위 네임 스페이스). 경로 프로젝트 디렉터리에 상대적입니다. 기본값은 "마이그레이션"입니다. |

### <a name="dotnet-ef-migrations-list"></a>dotnet ef 마이그레이션 목록

사용 가능한 마이그레이션을 나열합니다.

### <a name="dotnet-ef-migrations-remove"></a>dotnet ef 마이그레이션 제거

마지막 마이그레이션을 제거합니다.

옵션:

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| -f | -force | 데이터베이스에 적용 된 경우 마이그레이션을 되돌립니다. |

### <a name="dotnet-ef-migrations-script"></a>dotnet ef 마이그레이션 스크립트

마이그레이션의 SQL 스크립트를 생성합니다.

인수:

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| \<FROM> | 마이그레이션을 시작 합니다. 기본값은 0 (초기 데이터베이스)입니다. |
| \<TO>   | 끝의 마이그레이션입니다. 마지막으로 마이그레이션을 기본값입니다.         |

옵션:

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| -o | -출력 \<파일 > | 결과를 쓸 파일입니다.                                   |
| -i | -idempotent     | 모든 마이그레이션에는 데이터베이스에서 사용할 수 있는 스크립트를 생성 합니다. |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
