---
title: "디자인 타임 서비스-EF 코어"
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
ms.technology: entity-framework-core
ms.openlocfilehash: f9c8208a59bfcefeaab01ea69e65fe809a0b3d89
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2017
---
<a name="design-time-services"></a>디자인 타임 서비스
====================
도구에서 사용 되는 일부 서비스는 디자인 타임에만 사용 됩니다. 이러한 서비스는 응용 프로그램과 함께 배포 되지 않도록 EF 핵심 런타임 서비스에서 별도로 관리 됩니다. 이러한 서비스 (예를 들어 마이그레이션 파일을 생성 하는 서비스) 중 하나를 재정의 하려면 구현 추가 `IDesignTimeServices` 시작 프로젝트에 있습니다.

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
