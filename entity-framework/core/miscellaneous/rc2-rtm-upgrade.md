---
title: 1.0 RC2에서 RTM-Core EF에서 업그레이드 EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 1b95b2ab1943dfb541b3a7c873cff3cb4c16d9c1
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998321"
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a>EF Core 1.0 RC2에서 RTM으로 업그레이드

이 문서에서는 1.0.0 RC2 패키지를 사용 하 여 빌드된 응용 프로그램을 이동 하는 것에 대 한 지침을 제공 RTM입니다.

## <a name="package-versions"></a>패키지 버전

RC2에서 RTM 사이 일반적으로 응용 프로그램으로 설치 하는 최상위 수준 패키지의 이름을 변경 되지 않았습니다.

**RTM 버전에 설치 된 패키지를 업그레이드 해야 합니다.**

* 런타임 패키지 (예를 들어 `Microsoft.EntityFrameworkCore.SqlServer`)에서 변경 `1.0.0-rc2-final` 에 `1.0.0`입니다.

* 합니다 `Microsoft.EntityFrameworkCore.Tools` 패키지에서 변경 `1.0.0-preview1-final` 에 `1.0.0-preview2-final`입니다. 도구는 여전히 시험판을 참고 합니다.

## <a name="existing-migrations-may-need-maxlength-added"></a>기존 마이그레이션 maxLength를 추가 해야 합니다.

RC2의 마이그레이션에서는 열 정의 다음과 같았습니다 `table.Column<string>(nullable: true)` 열의 길이 코드 숨김 마이그레이션에에서 저장 하는 일부 메타 데이터에서 조회 하 고 있습니다. RTM, 길이 이제 스 캐 폴드 된 코드에 포함 되어 `table.Column<string>(maxLength: 450, nullable: true)`입니다.

RTM을 사용 하기 전에 스 캐 폴드 된는 모든 기존 마이그레이션 수는 없습니다를 `maxLength` 인수를 지정 합니다. 즉, 데이터베이스에서 지 원하는 최대 길이가 사용 됩니다 (`nvarchar(max)` SQL Server에서). 이 키 및 외래 키의 일부인 열을 제외한 일부 열에 대 한 세밀 하 게 될 또는 인덱스의 최대 길이 포함 하도록 업데이트 해야 합니다. 규칙에 따라 450 이며 최대 길이 키, 외래 키에 사용 되는 인덱싱된 열입니다. 모델의 길이 명시적으로 구성한 경우 다음 대신 사용 해야 해당 길이입니다.

**ASP.NET Id**

이 변경은 ASP.NET Id를 사용 하는 이전 버전에서 만든 프로젝트에 영향을 줍니다-RTM 프로젝트 템플릿. 프로젝트 템플릿 데이터베이스를 만드는 데 마이그레이션을 포함 합니다. 이 마이그레이션은의 최대 길이 지정 하려면 편집 해야 `256` 다음 열에 대 한 합니다.

*  **AspNetRoles**

    * name

    * NormalizedName

*  **AspNetUsers**

   * 메일

   * NormalizedEmail

   * NormalizedUserName

   * UserName

이 변경을 수행 하지 않으면 초기 마이그레이션을 데이터베이스에 적용 될 때 예외가 발생 합니다.

    System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.

## <a name="net-core-remove-imports-in-projectjson"></a>.NET core: project.json에서 "가져오기"를 제거 합니다.

RC2 사용 하 여.NET Core를 대상으로 된 경우 추가 해야 할 `imports` 일부.NET Standard를 지원 하지 않는 EF Core의 종속성에 대 한 임시 방편으로 project.json에 있습니다. 이러한을 제거할 수 있습니다.

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
> 버전 1.0부터 RTM 합니다 [.NET Core SDK](https://www.microsoft.com/net/download/core) 더 이상 지원 되지 `project.json` Visual Studio 2015를 사용 하 여.NET Core 응용 프로그램을 개발 합니다. [project.json에서 csproj로 마이그레이션](https://docs.microsoft.com/dotnet/articles/core/migration/)하는 것이 좋습니다. Visual Studio를 사용 하는 경우 업그레이드 하는 것이 좋습니다 [Visual Studio 2017](https://www.visualstudio.com/downloads/)합니다.

## <a name="uwp-add-binding-redirects"></a>UWP: 바인딩 리디렉션을 추가합니다

다음 오류가 발생에서 하는 유니버설 Windows 플랫폼 (UWP) 프로젝트 결과에서 EF 명령은 실행 하려고 합니다.

    System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.

UWP 프로젝트에 바인딩 리디렉션을 수동으로 추가 해야 합니다. 이라는 파일을 만듭니다 `App.config` 프로젝트의 루트 폴더 및 올바른 어셈블리 버전 리디렉션을 추가 합니다.

``` xml
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
