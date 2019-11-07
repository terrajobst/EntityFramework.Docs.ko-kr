---
title: 데이터베이스 공급자 작성-EF Core
author: anmiller
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
uid: core/providers/writing-a-provider
ms.openlocfilehash: 9d52a8581772cc5405e94966fa7ebdff4128c252
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73654770"
---
# <a name="writing-a-database-provider"></a>데이터베이스 공급자 작성

Entity Framework Core 데이터베이스 공급자를 작성 하는 방법에 대 한 자세한 내용은를 참조 하십시오 .이를 통해 [Arthur Vickers](https://github.com/ajcvickers)에서 [EF Core 공급자를 작성](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) 합니다.

> [!NOTE]
> 이러한 게시물은 1.1 EF Core 이후에 업데이트 되지 않으며, 해당 시간 [681](https://github.com/aspnet/EntityFramework.Docs/issues/681) 이이 설명서에 대 한 업데이트를 추적 하 고 있으므로 중요 한 변경 사항이 있었습니다.

EF Core codebase는 오픈 소스 이며 참조로 사용할 수 있는 여러 데이터베이스 공급자를 포함 합니다. <https://github.com/aspnet/EntityFrameworkCore>에서 소스 코드를 찾을 수 있습니다. [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql), [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact)등 일반적으로 사용 되는 타사 공급자에 대 한 코드를 살펴보는 것도 도움이 될 수 있습니다. 특히, 이러한 프로젝트는 NuGet에 게시 하는 기능 테스트를 확장 하 고 실행 하도록 설정 되어 있습니다. 이러한 종류의 설정은 적극 권장 됩니다.

## <a name="keeping-up-to-date-with-provider-changes"></a>공급자 변경 내용에 대 한 최신 상태 유지

2\.1 릴리스 후의 작업부터 공급자 코드에서 해당 변경 내용이 필요할 수 있는 [변경 로그](provider-log.md) 를 만들었습니다. 이는 EF Core의 새 버전에서 작동 하도록 기존 공급자를 업데이트할 때 도움을 주기 위한 것입니다.

2\.1 이전에는 [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) 를 사용 하 고 GitHub 문제에 대 한 레이블 및 [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) 유사한 목적으로 요청을 끌어옵니다. 지정 된 릴리스의 작업 항목에는 공급자에서 작업을 수행 해야 할 수도 있음을 나타내는 데 이러한 레이블에 on 문제를 continiue 합니다. `providers-beware` 레이블은 일반적으로 작업 항목의 구현이 공급자를 중단할 수 있는 반면, `providers-fyi` 레이블은 일반적으로 공급자가 손상 되지 않는다는 것을 의미 하지만 새 기능을 사용 하도록 설정 하는 등의 방법으로 코드를 변경 해야 할 수도 있습니다.

## <a name="suggested-naming-of-third-party-providers"></a>타사 공급자의 제안 된 이름 지정

NuGet 패키지에 다음 이름을 사용 하는 것이 좋습니다. 이는 EF Core 팀에서 제공 하는 패키지의 이름과 일치 합니다.

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

예를 들면,

* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
