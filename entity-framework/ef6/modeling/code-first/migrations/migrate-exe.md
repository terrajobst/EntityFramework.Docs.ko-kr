---
title: EF6 사용
author: divega
ms.date: 10/23/2016
ms.assetid: 989ea862-e936-4c85-926a-8cfbef5df5b8
ms.openlocfilehash: 34866ddbbf81f090a064af148a612dd354ae9401
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824818"
---
# <a name="using-migrateexe"></a>Debug.exe 사용
Code First 마이그레이션를 사용 하 여 visual studio 내에서 데이터베이스를 업데이트할 수 있지만 명령줄 도구인 debug.exe를 통해 실행할 수도 있습니다. 이 페이지에서는 마이그레이션을 사용 하 여 데이터베이스에 대해 마이그레이션을 실행 하는 방법에 대 한 간략 한 개요를 제공 합니다.

> [!NOTE]
> 이 문서에서는 기본 시나리오에서 Code First 마이그레이션를 사용 하는 방법을 알고 있다고 가정 합니다. 그렇지 않으면 계속 하기 전에 [Code First 마이그레이션](~/ef6/modeling/code-first/migrations/index.md) 읽어야 합니다.

## <a name="copy-migrateexe"></a>복사 마이그레이션 .exe

NuGet을 사용 하 여 Entity Framework를 설치 하는 경우 다운로드 한 패키지의 tools 폴더에 포함 됩니다. &lt;프로젝트 폴더에서 \\패키지\\EntityFramework로&gt;합니다.&lt;버전&gt;\\도구

Debug.exe를 만든 후에는 마이그레이션을 포함 하는 어셈블리의 위치에 복사 해야 합니다.

응용 프로그램이 4.5이 아닌 .NET 4를 대상으로 하는 경우에는 해당 위치에도 **Redirect** 를 복사 하 고 이름을 **debug.exe**로 바꾸어야 합니다. 이렇게 하면 debug.exe는 Entity Framework 어셈블리를 찾을 수 있도록 올바른 바인딩 리디렉션을 가져옵니다.

| .NET 4.5                                      | .NET 4.0                                      |
|:----------------------------------------------|:----------------------------------------------|
| ![.NET 4.5 파일](~/ef6/media/net45files.png) | ![.NET 4.0 파일](~/ef6/media/net40files.png) |

> [!NOTE]
> debug.exe는 x64 어셈블리를 지원 하지 않습니다.

Debug.exe를 올바른 폴더로 이동한 후에는이를 사용 하 여 데이터베이스에 대해 마이그레이션을 실행할 수 있습니다. 모든 유틸리티는 마이그레이션 실행을 위해 설계 되었습니다. 마이그레이션을 생성 하거나 SQL 스크립트를 만들 수 없습니다.

## <a name="see-options"></a>옵션 참조

``` console
Migrate.exe /?
```

위의 작업을 수행 하려면이 유틸리티와 연결 된 도움말 페이지가 표시 됩니다 .이 작업을 수행 하려면이를 실행 하는 위치와 동일한 위치에 EntityFramework .dll이 있어야 합니다.

## <a name="migrate-to-the-latest-migration"></a>최신 마이그레이션으로 마이그레이션

``` console
Migrate.exe MyMvcApplication.dll /startupConfigurationFile="..\\web.config"
```

마이그레이션을 실행 하는 경우 필수 매개 변수는 실행 하려는 마이그레이션이 포함 된 어셈블리인 어셈블리 이지만 구성 파일을 지정 하지 않을 경우 모든 규칙 기반 설정을 사용 합니다.

## <a name="migrate-to-a-specific-migration"></a>특정 마이그레이션으로 마이그레이션

``` console
Migrate.exe MyApp.exe /startupConfigurationFile="MyApp.exe.config" /targetMigration="AddTitle"
```

특정 마이그레이션에 대 한 마이그레이션을 실행 하려는 경우 마이그레이션 이름을 지정할 수 있습니다. 이렇게 하면 지정 된 마이그레이션에 도달할 때까지 필요한 모든 이전 마이그레이션을 실행 합니다.

## <a name="specify-working-directory"></a>작업 디렉터리 지정

``` console
Migrate.exe MyApp.exe /startupConfigurationFile="MyApp.exe.config" /startupDirectory="c:\\MyApp"
```

어셈블리에 종속성이 있거나 작업 디렉터리를 기준으로 파일을 읽는 경우 startupDirectory를 설정 해야 합니다.

## <a name="specify-migration-configuration-to-use"></a>사용할 마이그레이션 구성 지정

``` console
Migrate.exe MyAssembly CustomConfig /startupConfigurationFile="..\\web.config"
```

여러 마이그레이션 구성 클래스, DbMigrationConfiguration에서 상속 하는 클래스가 있는 경우이 실행에 사용할을 지정 해야 합니다. 위와 같이 스위치 없이 선택적인 두 번째 매개 변수를 제공 하 여이를 지정 합니다.

## <a name="provide-connection-string"></a>연결 문자열 제공

``` console
Migrate.exe BlogDemo.dll /connectionString="Data Source=localhost;Initial Catalog=BlogDemo;Integrated Security=SSPI" /connectionProviderName="System.Data.SqlClient"
```

명령줄에서 연결 문자열을 지정 하려는 경우 공급자 이름도 제공 해야 합니다. 공급자 이름을 지정 하지 않으면 예외가 발생 합니다.

## <a name="common-problems"></a>일반적인 문제

| 오류 메시지                                                                                                                                                                                                                                                                                                                      | 해결 방법                                                                                                                                                                                                                                                                                             |
|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 처리 되지 않은 예외: FileLoadException: 파일이 나 어셈블리 ' EntityFramework, Version = 5.0.0.0, Culture = 중립, PublicKeyToken = b77a5c561934e089 ' 또는 해당 종속성 중 하나를 로드할 수 없습니다. 찾은 어셈블리의 매니페스트 정의가 어셈블리 참조와 일치 하지 않습니다. (HRESULT의 예외: 0x80131040)         | 이는 일반적으로 사용자가 리디렉션 파일을 사용 하지 않고 .NET 4 응용 프로그램을 실행 하는 것을 의미 합니다. Dcpromo.exe와 동일한 위치에 리디렉션과 .config를 복사 하 고 이름을 변경 하 여 마이그레이션하려고 합니다.                                                                                       |
| 처리 되지 않은 예외: FileLoadException: 파일이 나 어셈블리 ' EntityFramework, Version = 4.4.0.0, Culture = 중립, PublicKeyToken = b77a5c561934e089 ' 또는 해당 종속성 중 하나를 로드할 수 없습니다. 찾은 어셈블리의 매니페스트 정의가 어셈블리 참조와 일치 하지 않습니다. (HRESULT의 예외: 0x80131040)          | 이 예외는 마이그레이션 .exe 위치에 복사 된 리디렉션과 함께 .NET 4.5 응용 프로그램을 실행 하는 것을 의미 합니다. 앱이 .NET 4.5 인 경우에는 내부 리디렉션으로 구성 파일을 가질 필요가 없습니다. Debug.exe .config 파일을 삭제 합니다.                                    |
| 오류: 보류 중인 변경 내용이 있으며 자동 마이그레이션을 사용 하지 않도록 설정 되어 있으므로 현재 모델과 일치 하도록 데이터베이스를 업데이트할 수 없습니다. 보류 중인 모델 변경 내용을 코드 기반 마이그레이션에 쓰거나 자동 마이그레이션을 사용 하도록 설정 합니다. DbMigrationsConfiguration를 true로 설정 하 여 자동 마이그레이션을 사용 하도록 설정 합니다. | 이 오류는 모델에 대 한 변경 내용을 적용 하기 위해 마이그레이션을 만들지 않았고 데이터베이스가 모델과 일치 하지 않을 때 마이그레이션을 실행 하는 경우에 발생 합니다. 모델 클래스에 속성을 추가한 다음 마이그레이션을 만들지 않고 마이그레이션을 실행 하 여 데이터베이스를 업그레이드 하는 것이이에 대 한 예입니다. |
| 오류: 멤버 ' UpdateRunner '에 대 한 형식이 확인 되지 않았습니다. ToolingFacade +, EntityFramework, Version = 5.0.0.0, Culture = 중립, PublicKeyToken = b77a5c561934e089 '.                                                                                                                                       | 이 오류는 잘못 된 시작 디렉터리를 지정 하 여 발생할 수 있습니다. 이 위치는 마이그레이션 .exe의 위치 여야 합니다.                                                                                                                                                                                      |
| 처리 되지 않은 예외: NullReferenceException: 개체 참조가 개체의 인스턴스로 설정 되지 않았습니다. <br/>   at system.xml. System.web. System.web (String [] args)                                                                                                                                             | 이는 사용 중인 시나리오에 대 한 필수 매개 변수를 지정 하지 않은 경우에 발생할 수 있습니다. 예를 들어 공급자 이름을 지정 하지 않고 연결 문자열을 지정 합니다.                                                                                                                        |
| 오류: 어셈블리 ' Classlibrary1.chainone '에서 둘 이상의 마이그레이션 구성 유형을 찾았습니다. 사용할 이름을 지정 합니다.                                                                                                                                                                                                  | 오류가 발생 하면 지정 된 어셈블리에 둘 이상의 구성 클래스가 있습니다. /ConfigurationType 스위치를 사용 하 여 사용할을 지정 해야 합니다.                                                                                                                                           |
| 오류: 파일 또는 어셈블리 '&lt;assemblyName&gt;' 또는 해당 종속성 중 하나를 로드할 수 없습니다. 지정 된 어셈블리 이름 또는 코드 베이스가 잘못 되었습니다. (HRESULT의 예외: 0x80131047)                                                                                                                                                    | 이는 어셈블리 이름을 잘못 지정 하거나를 포함 하지 않는 경우에 발생할 수 있습니다.                                                                                                                                                                                                                          |
| 오류: 파일 또는 어셈블리 '&lt;assemblyName&gt;' 또는 해당 종속성 중 하나를 로드할 수 없습니다. 프로그램을 잘못된 형식으로 로드하려고 했습니다.                                                                                                                                                                          | 이는 x64 응용 프로그램에 대해 .exe를 실행 하려고 할 때 발생 합니다. EF 5.0이 하는 x86 에서만 작동 합니다.                                                                                                                                                                                |
