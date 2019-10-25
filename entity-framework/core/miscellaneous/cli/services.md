---
title: 디자인 타임 서비스-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
uid: core/miscellaneous/cli/services
ms.openlocfilehash: ff0243a588d5e957aed89fcf1ce7462b5b9a747a
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811943"
---
# <a name="design-time-services"></a>디자인 타임 서비스

도구에서 사용 하는 일부 서비스는 디자인 타임에만 사용 됩니다. 이러한 서비스는 앱과 함께 배포 되는 것을 방지 하기 위해 EF Core의 런타임 서비스와는 별도로 관리 됩니다. 이러한 서비스 중 하나를 재정의 하려면 (예: 마이그레이션 파일을 생성 하는 서비스) 시작 프로젝트에 `IDesignTimeServices`의 구현을 추가 합니다.

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
