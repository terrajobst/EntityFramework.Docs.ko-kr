---
title: 리버스 엔지니어링-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/13/2018
ms.assetid: 6263EF7D-4989-42E6-BDEE-45DA770342FB
uid: core/managing-schemas/scaffolding
ms.openlocfilehash: ef729c0c26d5a1f57099f339eb51cda7e83289df
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688682"
---
# <a name="reverse-engineering"></a>리버스 엔지니어링

리버스 엔지니어링은 엔터티 형식 클래스 및 데이터베이스 스키마를 기반으로 하는 DbContext 클래스를 스 캐 폴딩의 프로세스입니다. 사용 하 여 수행할 수 있습니다 합니다 `Scaffold-DbContext` EF Core 패키지 관리자 콘솔 (PMC) 도구 명령 또는 `dotnet ef dbcontext scaffold` .NET CLI (명령줄 인터페이스) 도구 명령입니다.

## <a name="installing"></a>설치

리버스 엔지니어링, 하기 전에 설치 해야 합니다 [PMC 도구](xref:core/miscellaneous/cli/powershell) (Visual Studio에만 해당) 또는 [CLI 도구](xref:core/miscellaneous/cli/dotnet)합니다. 세부 정보에 대 한 링크를 참조 하세요.

적절 한 설치도 해야 [데이터베이스 공급자](xref:core/providers/index) 리버스 엔지니어링 하려는 데이터베이스 스키마에 대 한 합니다.

## <a name="connection-string"></a>연결 문자열

명령에 첫 번째 인수가 데이터베이스에 연결 문자열입니다. 도구는 데이터베이스 스키마를 읽어올이 연결 문자열을 사용 합니다.

인용 하 고 연결 문자열을 이스케이프 하는 방법에 셸 명령을 실행 하는 따라 달라 집니다. 자세한 내용은 셸의 문서를 참조 하십시오. 예를 들어 PowerShell에서는 이스케이프 해야 합니다 `$` 문자를 제외한 `\`합니다.

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

``` Console
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="configuration-and-user-secrets"></a>구성 및 사용자 암호

ASP.NET Core 프로젝트를 만든 경우 사용할 수 있습니다는 `Name=<connection-string>` 구문을 구성에서 연결 문자열을 읽습니다.

이 잘 작동 합니다 [암호 관리자 도구](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) 데이터베이스 암호를 코드 베이스에서 별도로 유지 하 합니다.

``` Console
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a>공급자 이름

두 번째 인수에 공급자 이름입니다. 공급자 이름은 일반적으로 공급자의 NuGet 패키지 이름과 동일 합니다.

## <a name="specifying-tables"></a>지정 된 테이블

데이터베이스 스키마의 모든 테이블은 기본적으로 엔터티 형식으로 엔지니어링 역방향입니다. 테이블은 스키마 및 테이블을 지정 하 여 엔지니어링 역방향 제한할 수 있습니다.

합니다 `-Schemas` PMC에서 매개 변수 및 `--schema` 스키마 내의 모든 테이블을 포함 하려면 CLI에서 옵션을 사용할 수 있습니다.

`-Tables` (PMC) 및 `--table` (CLI)를 사용 하 여 특정 테이블을 포함 하도록 있습니다.

PMC에서 여러 테이블에 포함 하려면 배열을 사용 합니다.

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

CLI에서 여러 테이블에 포함 하려면 옵션을 여러 번 지정 합니다.

``` Console
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

## <a name="preserving-names"></a>이름 유지

테이블 및 열 이름은 기본적으로 형식 및 속성에 대 한.NET 명명 규칙을 더 잘 맞게 고정 됩니다. 지정 된 `-UseDatabaseNames` PMC에서 전환 또는 `--use-database-names` 옵션 CLI에서 원래 데이터베이스 이름을 최대한 많이 보존 하는이 동작을 사용 하지 않도록 설정 됩니다. 잘못 된.NET 식별자 여전히 수정 될 예정 및 탐색 속성과 같이 합성 된 이름은.NET 명명 규칙을 따라야 계속 됩니다.

## <a name="fluent-api-or-data-annotations"></a>데이터 주석 또는 Fluent API

엔터티 형식은 기본적으로 Fluent API를 사용 하 여 구성 됩니다. 지정할 `-DataAnnotations` (PMC) 또는 `--data-annotations` (CLI) 대신 사용 하 여 가능한 경우 데이터 주석입니다.

예를 들어, Fluent API를 사용 하는 스 캐 폴딩이 있습니다.

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

데이터 주석을 사용 하는 동안는 스 캐 폴딩이 있습니다.

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a>DbContext 이름

스 캐 폴드 된 DbContext 클래스 이름 접미사를 사용 하는 데이터베이스의 이름이 됩니다 *상황에 맞는* 기본적으로 합니다. 다른 이름을 지정 하려면 사용 `-Context` PMC에서 및 `--context` CLI에서.

## <a name="directories-and-namespaces"></a>디렉터리 및 네임 스페이스

엔터티 클래스 및 DbContext 클래스는 프로젝트의 루트 디렉터리에 스 캐 폴드 된 프로젝트의 기본 네임 스페이스를 사용 합니다. 디렉터리를 지정할 수 있는 클래스를 사용 하 여 스 캐 폴드는 `-OutputDir` (PMC) 또는 `--output-dir` (CLI). 네임 스페이스는 루트 네임 스페이스와 프로젝트의 루트 디렉터리 아래의 하위 디렉터리의 이름이 됩니다.

사용할 수도 있습니다 `-ContextDir` (PMC) 및 `--context-dir` (CLI) 엔터티 형식 클래스에서 별도 디렉터리에 DbContext 클래스를 스 캐 폴딩 합니다.

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

``` Console
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

## <a name="how-it-works"></a>작동 방법

리버스 엔지니어링 데이터베이스 스키마를 참조 하 여 시작 합니다. 테이블, 열, 제약 조건 및 인덱스에 대 한 정보를 읽습니다.

다음으로, EF Core 모델을 만들 스키마 정보를 사용 합니다. 테이블은 엔터티 형식을 만드는 데 사용 됩니다. 열 속성을 만드는 데 사용 됩니다. 및 외래 키 관계를 만드는 데 사용 됩니다.

마지막으로 모델 코드 생성에 사용 됩니다. 앱에서 동일한 모델을 다시 만들려면 해당 엔터티 형식 클래스, Fluent API 및 데이터 주석을 스 캐 폴드 된 됩니다.

## <a name="what-doesnt-work"></a>작동 하지 않는

모델에 대 한 것은 아닙니다 데이터베이스 스키마를 사용 하 여 나타낼 수 있습니다. 예를 들어,에 대 한 정보 **상속 계층 구조**, **소유 된 형식이**, 및 **분할 테이블** 데이터베이스 스키마에 없는 합니다. 이 인해 이러한 구문은 됩니다 하지 수 리버스 엔지니어링 합니다.

또한 **몇 가지 열 형식을** EF Core 공급자가 지원 되지 않습니다. 이러한 열은 모델에 포함 되지 않습니다.

EF Core에는 키를 갖는 모든 엔터티 형식에 필요 합니다. 그러나 테이블 기본 키를 지정 하려면 필요한 되지 않습니다. **기본 키가 없는 테이블** 은 리버스 엔지니어링 된 현재 없습니다.

정의할 수 있습니다 **동시성 토큰** EF Core 모델 두 사용자가 동시에 동일한 엔터티를 업데이트 하지 못하도록 합니다. 일부 데이터베이스를 되돌릴 수 있는 경우가이 정보를 엔지니어링에서이 형식의 열 (예: SQL Server에서 rowversion)을 나타내는 특수 형식이 그러나 다른 동시성 토큰은 수 리버스 엔지니어링 되지 않습니다.

## <a name="customizing-the-model"></a>모델을 사용자 지정

EF Core에서 생성 된 코드는 코드입니다. 자유롭게 변경할 수 있습니다. 리버스 엔지니어링 하면 동일한 모델을 다시 하는 경우만 재생성 됩니다. 스 캐 폴드 된 코드를 나타내는 *하나* 데이터베이스에 있지만 액세스할 수 있는 모델은 분명 하지는 *만* 사용할 수 있는 모델입니다.

엔터티 형식 클래스 및 필요에 맞게 DbContext 클래스를 사용자 지정 합니다. 예를 들어, 형식 및 속성을 이름 바꾸기, 상속 계층 구조를 도입 또는 여러 엔터티를 테이블에 분할을 선택할 수 있습니다. 또한 비고유 인덱스, 사용 하지 않는 시퀀스 및 탐색 속성, 선택적 스칼라 속성 및 제약 조건 이름은 모델에서 제거할 수 있습니다.

또한 추가 생성자, 메서드, 속성 등을 추가할 수 있습니다. 별도 파일에서 다른 partial 클래스를 사용 합니다. 이 방법은 다시 리버스 엔지니어링 모델 하려는 경우에 작동 합니다.

## <a name="updating-the-model"></a>모델 업데이트

데이터베이스에 구성을 변경한 후 변경 내용을 반영 하도록 EF Core 모델을 업데이트 해야 합니다. 데이터베이스 변경 내용을 간단한 경우 EF Core 모델을 수동으로 변경할 가장 수도 있습니다. 예를 들어, 테이블 또는 열 이름 바꾸기, 열을 제거 하거나 열의 형식을 업데이트 하는 코드에서 변경 됩니다.

그러나 더 중요 한 변경 내용은 않습니다 쉽게 확인으로 수동으로. 다시 사용 하 여 데이터베이스에서 모델을 설계를 반대로 하려면 일반적인 워크플로 하나 `-Force` (PMC) 또는 `--force` (CLI) 기존 모델 업데이트 된 항목을 덮어쓸 수 있습니다.

다른 일반적으로 요청 된 기능은 이름 바꾸기, 형식 계층 구조 등 사용자 지정을 유지 하는 동안 데이터베이스에서 모델을 업데이트 하는 기능입니다. 문제를 사용 하 여 [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) 이 기능의 진행률을 추적할 수 있습니다.

> [!WARNING]
> 를 리버스 엔지니어링 하면 데이터베이스에서 모델 다시 파일을 변경한 후 모든 변경 내용이 손실 됩니다.
