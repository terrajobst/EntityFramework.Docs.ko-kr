---
title: EF Core 1.0 RC2에서 RTM-EF Core로 업그레이드
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 779caad7883d13684b389dab7515be44bc42e1ef
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414040"
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a>EF Core 1.0 RC2에서 RTM으로 업그레이드

이 문서에서는 RC2 패키지를 사용 하 여 빌드된 응용 프로그램을 1.0.0 RTM으로 이동 하기 위한 지침을 제공 합니다.

## <a name="package-versions"></a>패키지 버전

응용 프로그램에 일반적으로 설치 하는 최상위 패키지의 이름은 RC2와 RTM 사이에서 변경 되지 않았습니다.

**설치 된 패키지를 RTM 버전으로 업그레이드 해야 합니다.**

* 런타임 패키지 (예: `Microsoft.EntityFrameworkCore.SqlServer`)가 `1.0.0-rc2-final`에서 `1.0.0`로 변경 되었습니다.

* `Microsoft.EntityFrameworkCore.Tools` 패키지가 `1.0.0-preview1-final`에서 `1.0.0-preview2-final`로 변경 되었습니다. 도구는 여전히 시험판입니다.

## <a name="existing-migrations-may-need-maxlength-added"></a>기존 마이그레이션은 maxLength가 추가 되어야 할 수 있습니다.

R c 2에서 마이그레이션의 열 정의는 `table.Column<string>(nullable: true)`와 같이 마이그레이션 후 코드에 저장 하는 일부 메타 데이터에서 조회 됩니다. RTM에서 길이는 이제 스 캐 폴드 코드 `table.Column<string>(maxLength: 450, nullable: true)`포함 됩니다.

RTM을 사용 하기 전에 스 캐 폴드 기존 마이그레이션에는 지정 된 `maxLength` 인수가 없습니다. 즉, 데이터베이스에서 지원 되는 최대 길이가 사용 됩니다 (SQL Server에`nvarchar(max)`). 이는 일부 열에는 문제가 없지만 키, 외래 키 또는 인덱스의 일부인 열은 최대 길이를 포함 하도록 업데이트 해야 합니다. 규칙에 따라 450는 키, 외래 키 및 인덱싱된 열에 사용 되는 최대 길이입니다. 모델에서 길이를 명시적으로 구성한 경우에는 해당 길이를 대신 사용 해야 합니다.

### <a name="aspnet-identity"></a>ASP.NET ID

이 변경 내용은 ASP.NET Identity를 사용 하 고 RTM 프로젝트 템플릿에서 만든 프로젝트에 영향을 줍니다. 프로젝트 템플릿에는 데이터베이스를 만드는 데 사용 되는 마이그레이션이 포함 되어 있습니다. 다음 열의 최대 `256` 길이를 지정 하려면이 마이그레이션을 편집 해야 합니다.

* **AspNetRoles**
  * 이름
  * NormalizedName
* **AspNetUsers**
  * Email
  * NormalizedEmail
  * NormalizedUserName
  * UserName

이러한 변경을 수행 하지 못하면 초기 마이그레이션이 데이터베이스에 적용 될 때 다음 예외가 발생 합니다.

``` Console
System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.
```

## <a name="net-core-remove-imports-in-projectjson"></a>.NET Core: project. json에서 "imports"를 제거 합니다.

R c 2를 사용 하 여 .NET Core를 대상으로 하는 경우 .NET Standard를 지원 하지 않는 EF Core 종속성 중 일부에 대 한 임시 해결 방법으로 project. json에 `imports`를 추가 해야 했습니다. 이제 제거할 수 있습니다.

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

> [!NOTE]  
> 버전 1.0 RTM을 사용 하 여 [.NET Core SDK](https://www.microsoft.com/net/download/core) 는 더 이상 Visual Studio 2015를 사용 하 여 .net Core 응용 프로그램 `project.json` 또는 개발을 지원 하지 않습니다. [project.json에서 csproj로 마이그레이션](https://docs.microsoft.com/dotnet/articles/core/migration/)하는 것이 좋습니다. Visual Studio를 사용 하는 경우 [Visual studio 2017](https://www.visualstudio.com/downloads/)로 업그레이드 하는 것이 좋습니다.

## <a name="uwp-add-binding-redirects"></a>UWP: 바인딩 리디렉션 추가

UWP (유니버설 Windows 플랫폼) 프로젝트에서 EF 명령을 실행 하려고 하면 다음 오류가 발생 합니다.

```output
System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.
```

UWP 프로젝트에 바인딩 리디렉션을 수동으로 추가 해야 합니다. 프로젝트 루트 폴더에 `App.config` 라는 파일을 만들고 올바른 어셈블리 버전에 리디렉션을 추가 합니다.

```xml
<configuration>
 <runtime>
   <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
     <dependentAssembly>
       <assemblyIdentity name="System.IO.FileSystem.Primitives"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
     <dependentAssembly>
       <assemblyIdentity name="System.Threading.Overlapped"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
   </assemblyBinding>
 </runtime>
</configuration>
```
