---
title: 별도의 마이그레이션 프로젝트 사용-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/projects
ms.openlocfilehash: 89b7f50fe750c2953aa75efcdffcb1a5199ce90c
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824416"
---
# <a name="using-a-separate-migrations-project"></a>별도의 마이그레이션 프로젝트 사용

`DbContext`를 포함 하는 어셈블리와 다른 어셈블리에 마이그레이션을 저장할 수 있습니다. 또한이 전략을 사용 하 여 여러 마이그레이션 집합을 유지 관리할 수 있습니다. 예를 들어 개발 및 릴리스 간 업그레이드를 위한 여러 마이그레이션 집합을 유지할 수 있습니다.

수행 작업

1. 새 클래스 라이브러리를 만듭니다.

2. DbContext 어셈블리에 대 한 참조를 추가 합니다.

3. 마이그레이션 및 모델 스냅숏 파일을 클래스 라이브러리로 이동 합니다.
   > [!TIP]
   > 기존 마이그레이션이 없는 경우 DbContext를 포함 하는 프로젝트에서이를 생성 한 후 이동 합니다.
   > 마이그레이션 어셈블리에 기존 마이그레이션이 포함 되어 있지 않은 경우에는 마이그레이션 추가 명령이 DbContext을 찾을 수 없기 때문에이는 중요 합니다.

4. 마이그레이션 어셈블리를 구성 합니다.

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. 시작 어셈블리에서 마이그레이션 어셈블리에 대 한 참조를 추가 합니다.
   * 이로 인해 순환 종속성이 발생 하는 경우 클래스 라이브러리의 출력 경로를 업데이트 합니다.

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

모든 작업을 올바르게 수행한 경우 프로젝트에 새 마이그레이션을 추가할 수 있습니다.

## <a name="net-core-clitabdotnet-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add NewMigration --project MyApp.Migrations
```

## <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```

***
