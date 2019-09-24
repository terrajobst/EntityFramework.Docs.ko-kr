---
title: 리버스 엔지니어링-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/13/2018
ms.assetid: 6263EF7D-4989-42E6-BDEE-45DA770342FB
uid: core/managing-schemas/scaffolding
ms.openlocfilehash: afe2c865305ade93dd10c8838b80c8b4177e7e8e
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197190"
---
# <a name="reverse-engineering"></a>리버스 엔지니어링

리버스 엔지니어링은 엔터티 형식 클래스 및 데이터베이스 스키마 기반의 DbContext 클래스를 스캐폴딩하는 프로세스입니다. EF Core 패키지 관리자 콘솔(PMC) 도구의 `Scaffold-DbContext` 명령이나 .NET 명령줄 인터페이스(CLI) 도구의 `dotnet ef dbcontext scaffold` 명령을 사용하여 수행할 수 있습니다.

## <a name="installing"></a>설치

리버스 엔지니어링을 하기 전에 [PMC 도구](xref:core/miscellaneous/cli/powershell)(Visual Studio에만 해당) 또는 [CLI 도구](xref:core/miscellaneous/cli/dotnet) 중 하나를 설치해야 합니다. 자세한 내용은 링크를 참조하세요.

또한 리버스 엔지니어링하려는 데이터베이스 스키마에 적절한 [데이터베이스 공급자](xref:core/providers/index)를 설치해야 합니다.

## <a name="connection-string"></a>연결 문자열

명령의 첫 번째 인수는 데이터베이스에 대한 연결 문자열입니다. 도구는 이 연결 문자열을 사용하여 데이터베이스 스키마를 읽습니다.

연결 문자열을 인용하고 이스케이프하는 방법은 명령을 실행하는 데 사용 중인 셸에 따라 다릅니다. 자세한 내용은 셸 설명서를 참조하십시오. 예를 들어 PowerShell에서는 `$` 문자를 이스케이프해야 하지만 `\`는 이스케이프하지 않아야 합니다.

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

``` Console
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="configuration-and-user-secrets"></a>구성 및 사용자 암호

ASP.NET Core 프로젝트를 만든 경우 `Name=<connection-string>` 구문을 사용하여 구성에서 연결 문자열을 읽을 수 있습니다.

이 기능은 [암호 관리자 도구](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager)와 잘 작동하여 데이터베이스 암호를 코드베이스와 별도로 유지합니다.

``` Console
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a>공급자 이름

두 번째 인수는 공급자 이름입니다. 공급자 이름은 일반적으로 공급자의 NuGet 패키지 이름과 동일합니다.

## <a name="specifying-tables"></a>지정 된 테이블

데이터베이스 스키마의 모든 테이블은 기본적으로 엔터티 형식으로 리버스 엔지니어링됩니다. 스키마와 테이블을 지정하여 리버스 엔지니어링되는 테이블을 제한할 수 있습니다.

PMC의 `-Schemas` 매개 변수와 CLI의 `--schema` 옵션을 사용하여 스키마의 모든 테이블을 포함할 수 있습니다.

`-Tables` (PMC) 및 `--table` (CLI)를 사용하여 특정 테이블을 포함할 수 있습니다.

PMC에서 여러 테이블을 포함하려면 배열을 사용합니다.

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

CLI에서 여러 테이블을 포함하려면 옵션을 여러 번 지정합니다.

``` Console
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

## <a name="preserving-names"></a>이름 유지

테이블 및 열 이름은 기본적으로 형식 및 속성에 대한 .NET 명명 규칙과 보다 잘 일치하도록 수정됩니다. PMC에서 지정된 `-UseDatabaseNames` 또는 CLI에서 지정된 `--use-database-names` 옵션을 지정하면 가능한 한 원본 데이터베이스 이름을 그대로 유지하면서 이 동작이 비활성화됩니다. 잘못된 .NET 식별자는 여전히 수정되며 탐색 속성과 같은 합성된 이름은 .NET 명명 규칙을 계속 준수합니다.

## <a name="fluent-api-or-data-annotations"></a>데이터 주석 또는 Fluent API

엔터티 형식은 기본적으로 Fluent API를 사용하여 구성됩니다. 가능한 경우 데이터 주석을 대신 사용하려면 `-DataAnnotations`(PMC) 또는 `--data-annotations`(CLI)를 지정합니다.

예를 들어, 흐름 API를 사용 하면 다음과 같이 스 캐 폴드 됩니다.

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

데이터 주석을 사용 하는 동안 다음과 같은 스 캐 폴드 됩니다.

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a>DbContext 이름

스캐폴딩된 DbContext 클래스 이름은 기본적으로 *Context* 접미사가 붙은 데이터베이스의 이름이 됩니다. 다른 이름을 지정하려면 PMC에서 `-Context`를 사용하고 CLI에서 `--context`를 사용합니다.

## <a name="directories-and-namespaces"></a>디렉터리 및 네임 스페이스

엔터티 클래스 및 DbContext 클래스는 프로젝트의 루트 디렉터리로 스캐폴딩되어 프로젝트의 기본 네임스페이스를 사용합니다. `-OutputDir`(PMC) 또는 `--output-dir`(CLI)를 사용하여 클래스가 스캐폴딩되는 디렉터리를 지정할 수 있습니다. 네임스페이스는 루트 네임스페이스와 프로젝트의 루트 디렉터리에 있는 하위 디렉터리의 이름이 됩니다.

`-ContextDir`(PMC) 및 `--context-dir`(CLI)를 사용하여 엔터티 형식 클래스와 별도의 디렉터리에 DbContext 클래스를 스캐폴딩할 수도 있습니다.

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

``` Console
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

## <a name="how-it-works"></a>작동 방법

리버스 엔지니어링은 데이터베이스 스키마를 읽음으로써 시작됩니다. 테이블, 열, 제약 조건 및 인덱스에 대한 정보를 읽습니다.

그런 다음 스키마 정보를 사용하여 EF Core 모델을 작성합니다. 테이블은 엔터티 형식을 만드는 데 사용됩니다. 열은 속성을 만드는 데 사용됩니다.

관계를 작성하기 위해 외래 키가 사용됩니다. 해당 엔터티 형식 클래스, Fluent API 및 데이터 주석은 앱에서 동일한 모델을 재생성하기 위해 스캐폴딩됩니다.

## <a name="limitations"></a>제한 사항

* 모델에 대한 모든 것이 데이터베이스 스키마를 사용하여 표현될 수 있는 것은 아닙니다. 예를 들어 [**상속 계층 구조**](../modeling/inheritance.md), [**소유 된 형식**](../modeling/owned-entities.md)및 [**테이블 분할**](../modeling/table-splitting.md) 에 대 한 정보는 데이터베이스 스키마에 없습니다. 이 때문에 이러한 구조는 결코 리버스 엔지니어링되지 않습니다.
* 또한 **몇 가지 열 형식**은 EF Core 공급자가 지원되지 않습니다. 이러한 열은 모델에 포함되지 않습니다.
* EF Core 모델에서 [**동시성 토큰**](../modeling/concurrency.md)을 정의 하 여 두 사용자가 동시에 동일한 엔터티를 업데이트 하지 못하게 할 수 있습니다. 일부 데이터베이스에는 이 형식의 열(예: SQL Server의 rowversion)을 나타내는 특수한 형식이 있습니다. 이 경우 이 정보를 리버스 엔지니어링할 수 있습니다. 그러나 다른 동시성 토큰은 리버스 엔지니어링되지 않습니다.
* [8 nullable 참조 형식 기능은 현재 리버스 엔지니어링에서 지원 되지 않습니다. C# ](/dotnet/csharp/tutorials/nullable-reference-types) EF Core는 기능 C# 을 사용할 수 없는 것으로 가정 하는 코드를 항상 생성 합니다. 예를 들어 nullable 텍스트 열은 속성이 필수 인지 여부를 구성 하 `string` 는 데 `string?`사용 되는 흐름 API 또는 데이터 주석을 사용 하지 않고 형식의 속성으로 스 캐 폴드 됩니다. 스 캐 폴드 코드를 편집 하 여 null 허용 여부 주석 C# 으로 바꿀 수 있습니다. Nullable 참조 형식에 대 한 스 캐 폴딩 지원은 [#15520](https://github.com/aspnet/EntityFrameworkCore/issues/15520)문제에 의해 추적 됩니다.

## <a name="customizing-the-model"></a>모델을 사용자 지정

EF Core에서 생성된 코드는 자유롭게 변경할 수 있습니다. 자유롭게 변경할 수 있습니다. 리버스 엔지니어링 하면 동일한 모델을 다시 하는 경우만 재생성 됩니다. 스캐폴딩된 코드는 데이터베이스에 액세스하는 데 사용할 수 있는 *하나*의 모델을 나타내지만 확실히 사용할 수 있는 *유일한* 모델은 아닙니다.

필요에 맞게 엔터티 형식 클래스 및 DbContext 클래스를 사용자 정의합니다. 예를 들어 형식 및 속성의 이름을 바꾸거나 상속 계층 구조를 도입하거나 테이블을 여러 엔터티로 분할하도록 선택할 수 있습니다. 또한 비고유 인덱스, 사용하지 않는 시퀀스 및 탐색 속성, 선택적 스칼라 속성 및 제약 조건 이름은 모델에서 제거할 수 있습니다.

또한 추가 생성자, 메서드, 속성 등을 추가할 수 있습니다. 별도 파일에서 다른 partial 클래스를 사용 합니다. 이 방법은 다시 리버스 엔지니어링 모델 하려는 경우에 작동 합니다.

## <a name="updating-the-model"></a>모델 업데이트

데이터베이스를 변경한 후에는 변경 사항을 반영하도록 EF Core 모델을 업데이트해야 할 수도 있습니다. 데이터베이스 변경이 간단하면 EF Core 모델을 수동으로 변경하는 것이 가장 쉽습니다. 예를 들어, 테이블 또는 열 이름 바꾸기, 열을 제거하거나 열의 형식을 업데이트 하는 코드에서 변경됩니다.

그러나 더 중요한 변경 사항은 수동으로 작성하기가 쉽지 않습니다. 일반적인 워크플로 중 하나는 `-Force`(PMC) 또는 `--force`(CLI)를 사용하여 데이터베이스에서 모델을 다시 리버스 엔지니어링하여 기존 모델을 업데이트된 모델로 덮어 쓰는 것입니다.

다른 일반적으로 요구되는 또 다른 기능은 이름 변경, 형식 계층 구조 등과 같은 사용자 정의를 유지하면서 데이터베이스에서 모델을 업데이트하는 기능입니다. 이 기능의 진행 상황을 추적하려면 이슈 [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831)을 사용합니다.

> [!WARNING]
> 모델을 데이터베이스에서 다시 리버스 엔지니어링하면 파일에 대한 모든 변경 사항이 손실됩니다.
