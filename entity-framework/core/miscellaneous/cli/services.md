---
title: 디자인 타임 서비스-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
ms.openlocfilehash: e1cacdd4f40f9c395d8c88a91df4a92ef27001a8
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997533"
---
<a name="design-time-services"></a>디자인 타임 서비스
====================
도구에서 사용 되는 일부 서비스는 디자인 타임에만 사용 됩니다. 이러한 서비스는 앱과 함께 배포 되지 않도록 하기 위해 EF Core 런타임 서비스에서 별도로 관리 됩니다. 이러한 서비스 (예를 들어 마이그레이션 파일을 생성 하려면 서비스) 중 하나를 재정의 하려면 추가 구현의 `IDesignTimeServices` 시작 프로젝트에 있습니다.

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
