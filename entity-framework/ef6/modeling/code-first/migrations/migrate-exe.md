---
title: Migrate.exe-EF6을 사용 하 여
author: divega
ms.date: 2016-10-23
ms.assetid: 989ea862-e936-4c85-926a-8cfbef5df5b8
ms.openlocfilehash: 8f0ff6d472c39eaf000c31783fe7a769c8746fec
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251117"
---
# <a name="using-migrateexe"></a>Migrate.exe를 사용 하 여
Code First 마이그레이션을에서 데이터베이스를 업데이트할 수 visual studio 내에서 명령줄 도구 migrate.exe 통해 실행할 수 있습니다. 이 페이지 migrate.exe를 사용 하 여 데이터베이스에 대해 마이그레이션을 실행 하는 방법에 대 한 빠른 개요를 제공 합니다.

> [!NOTE]
> 이 문서에서는 기본 시나리오에서 Code First 마이그레이션을 사용 하는 방법을 알고 있다고 가정 합니다. 이렇게 하지 않으면 경우 읽기 해야 [Code First 마이그레이션을](~/ef6/modeling/code-first/migrations/index.md) 계속 하기 전에 합니다.

## <a name="copy-migrateexe"></a>Migrate.exe 복사

Entity Framework를 설치할 때 NuGet을 사용 하 여 migrate.exe가 됩니다 다운로드 한 패키지의 tools 폴더 내에서. &lt;프로젝트 폴더&gt;\\패키지\\EntityFramework.&lt; 버전&gt;\\도구

Migrate.exe를 만든 후 마이그레이션을 포함 하는 어셈블리의 위치에 복사 해야 합니다.

응용 프로그램에.NET 4를 대상으로 하지 4.5 다음 해야 복사할 경우 합니다 **Redirect.config** 위치로 및 바꾸거나 **migrate.exe.config**합니다. 이 migrate.exe 올바른 바인딩 리디렉션을 Entity Framework 어셈블리를 찾을 수 있도록 합니다.

| .NET 4.5                                   | .NET 4.0                                   |
|:-------------------------------------------|:-------------------------------------------|
| ![.NET 4.5 파일](~/ef6/media/net45files.png)  | ![.NET 4.0 파일](~/ef6/media/net40files.png)  |

> [!NOTE]
> migrate.exe x64를 지원 하지 않습니다 어셈블리입니다.

Migrate.exe 올바른 폴더에 이동 하면 마이그레이션을 데이터베이스에 대해 실행에 사용할 수 있습니다. 마이그레이션을 실행 유틸리티는 수행 하도록 디자인 됩니다. 마이그레이션을 생성 하거나 SQL 스크립트를 만들 수 없습니다.

## <a name="see-options"></a>옵션을 보려면

``` console
Migrate.exe /?
```

위의 참고를 EntityFramework.dll migrate.exe이 작업을 위해에서 실행 중인 동일한 위치에 해야 하는이 유틸리티를 사용 하 여 연결 된 도움말 페이지가 표시 됩니다.

## <a name="migrate-to-the-latest-migration"></a>위해 최신 마이그레이션으로 마이그레이션

``` console
Migrate.exe MyMvcApplication.dll /startupConfigurationFile=”..\\web.config”
```

구성 파일을 지정 하지 않으면 경우 migrate.exe 유일한 필수 매개 변수를 실행 중인 실행 하려고 하는 마이그레이션을 포함 하는 어셈블리는 어셈블리 이지만 모든 규칙을 사용할지 기반 설정 합니다.

## <a name="migrate-to-a-specific-migration"></a>마이그레이션할 특정 마이그레이션

``` console
Migrate.exe MyApp.exe /startupConfigurationFile=”MyApp.exe.config” /targetMigration=”AddTitle”
```

특정 마이그레이션까지 마이그레이션을 실행 하려는 경우 마이그레이션의 이름을 지정할 수 있습니다. 모든 이전 마이그레이션 필요에 따라 실행될지이 지정 된 마이그레이션 시작 될 때까지 합니다.

## <a name="specify-working-directory"></a>작업 디렉터리를 지정 합니다.

``` console
Migrate.exe MyApp.exe /startupConfigurationFile=”MyApp.exe.config” /startupDirectory=”c:\\MyApp”
```

어셈블리가 있습니다 종속성 또는 작업 디렉터리를 기준으로 파일을 읽고 경우 startupDirectory 설정 해야 합니다.

## <a name="specify-migration-configuration-to-use"></a>사용할 마이그레이션 구성 지정

``` console
Migrate.exe MyAssembly CustomConfig /startupConfigurationFile=”..\\web.config”
```

여러 마이그레이션 구성 클래스에 있는 경우 DbMigrationConfiguration에서 상속 클래스를 다음 지정 해야이 실행에 사용 하는 것입니다. 이 위에서 스위치 없이 선택적 두 번째 매개 변수를 제공 하 여 지정 됩니다.

## <a name="provide-connection-string"></a>연결 문자열 제공

``` console
Migrate.exe BlogDemo.dll /connectionString=”Data Source=localhost;Initial Catalog=BlogDemo;Integrated Security=SSPI” /connectionProviderName=”System.Data.SqlClient”
```

명령줄에서 연결 문자열을 지정 하려는 경우 공급자 이름을 제공 해야 합니다. 공급자 이름을 지정 하지는 예외가 발생 합니다.

## <a name="common-problems"></a>일반적인 문제

| 오류 메시지                                                                                                                                                                                                                                                                                                                      | 솔루션                                                                                                                                                                                                                                                                                             |
|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 처리 되지 않은 예외: System.IO.FileLoadException: 파일이 나 어셈블리를 로드할 수 없습니다 ' EntityFramework, 버전 5.0.0.0, Culture = neutral, PublicKeyToken = b77a5c561934e089 =' 또는 해당 종속성 중 하나입니다. 찾은 어셈블리의 매니페스트 정의가 어셈블리 참조를 일치 하지 않습니다. (HRESULT의 예외: 0x80131040)         | 일반적으로 Redirect.config 파일 없이.NET 4 응용 프로그램을 실행 하는 것을 의미 합니다. Redirect.config migrate.exe와 동일한 위치에 복사 하 고 이름을 migrate.exe.config로 해야 합니다.                                                                                       |
| 처리 되지 않은 예외: System.IO.FileLoadException: 파일이 나 어셈블리를 로드할 수 없습니다 ' EntityFramework, 버전 4.4.0.0, Culture = neutral, PublicKeyToken = b77a5c561934e089 =' 또는 해당 종속성 중 하나입니다. 찾은 어셈블리의 매니페스트 정의가 어셈블리 참조를 일치 하지 않습니다. (HRESULT의 예외: 0x80131040)          | 이 예외는 Redirect.config 사용 하 여 응용 프로그램 migrate.exe 위치로 복사 하는.NET 4.5를 실행 하는 것을 의미 합니다. 앱이.NET 4.5 경우 구성 파일 내에서 리디렉션 사용 하 여 필요가 없습니다. Migrate.exe.config 파일을 삭제 합니다.                                    |
| 오류: 보류 중인 변경 및 자동 마이그레이션을 사용할 수 없습니다 때문에 현재 모델과 일치 하도록 데이터베이스를 업데이트할 수 없습니다. 코드 기반 마이그레이션에 보류 중인 모델 변경 내용 쓰기 또는 자동 마이그레이션을 사용 하도록 설정 합니다. 자동 마이그레이션을 사용 하도록 설정 하려면 true로 설정 하는 DbMigrationsConfiguration.AutomaticMigrationsEnabled를 설정 합니다. | 실행 중인 마이그레이션 변경 내용을 모델에 대 한 대처로 마이그레이션을 만들지 않은 데이터베이스 모델과 일치 하지 않는 경우이 오류가 발생 합니다. 다음 데이터베이스를 업그레이드 하려면 마이그레이션을 만들지 않고 migrate.exe를 실행 하는 모델 클래스에 속성 추가이 예시입니다. |
| 오류: 형식 멤버에 대 한 해결 되지 않으면 ' System.Data.Entity.Migrations.Design.ToolingFacade+UpdateRunner,EntityFramework, 버전 5.0.0.0, Culture = neutral, PublicKeyToken = b77a5c561934e089 ='.                                                                                                                                       | 이 오류는 잘못 된 시작 디렉터리를 지정 하 여 발생할 수 있습니다. Migrate.exe의 위치 여야 합니다.                                                                                                                                                                                      |
| 처리 되지 않은 예외: System.NullReferenceException: 개체 참조가 개체의 인스턴스로 설정 되지 않았습니다. <br/>   System.Data.Entity.Migrations.Console.Program.Main (String args)에서                                                                                                                                             | 이 사용 하는 시나리오에 대 한 필수 매개 변수를 지정 하지 않으면에서 발생할 수 있습니다. 예를 들어 공급자 이름을 지정 하지 않고 연결 문자열을 지정 합니다.                                                                                                                        |
| 오류: 'ClassLibrary1' 어셈블리에서 마이그레이션을 구성 형식이 둘 이상 찾았습니다. 사용할 이름을 지정 합니다.                                                                                                                                                                                                  | 오류 상태는 구성 클래스가 둘 이상 지정된 된 어셈블리 에서입니다. 사용 하려는 지정 하려면 /configurationType 스위치를 사용 해야 합니다.                                                                                                                                           |
| 오류:에 파일 또는 어셈블리를 로드할 수 없습니다 '&lt;assemblyName&gt;' 또는 해당 종속성 중 하나입니다. 지정된 된 어셈블리 이름 또는 코드 베이스가 올바르지 않습니다. (HRESULT의 예외: 0x80131047)                                                                                                                                                    | 이 어셈블리 이름이 올바르게 지정 또는 있지 않은 경우 발생할 수 있습니다.                                                                                                                                                                                                                          |
| 오류:에 파일 또는 어셈블리를 로드할 수 없습니다 '&lt;assemblyName&gt;' 또는 해당 종속성 중 하나입니다. 프로그램을 잘못된 형식으로 로드하려고 했습니다.                                                                                                                                                                          | X64 migrate.exe 실행 하려는 경우이 응용 프로그램입니다. EF 5.0 이하의 x86에만 적용 됩니다.                                                                                                                                                                                |
