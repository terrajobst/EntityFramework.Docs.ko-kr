---
title: "IBM Data Server(DB2) 데이터베이스 공급자 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 02/15/2017
ms.assetid: 825e5332-5aa3-4600-9efb-ab71aaff59ec
ms.technology: entity-framework-core
uid: core/providers/ibm/index
ms.openlocfilehash: a9caa8df63850d4f6b5f2164dad7ac5af7504076
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="ibm-data-server-db2-ef-core-database-providers"></a>IBM Data Server (DB2) EF Core 데이터베이스 공급자

이 데이터베이스 공급자를 설치하면 Entity Framework Core를 IBM Data Server에서 사용할 수 있습니다. 이 공급자는 IBM에서 유지 관리됩니다.

> [!NOTE]  
> 이 공급자는 Entity Framework Core 프로젝트의 일부로 유지 관리되지 않습니다. 타사 공급자를 고려할 때는 품질, 라이선싱, 지원 등을 평가하여 요구 사항에 부합하는지 확인합니다.

## <a name="install"></a>설치

Windows에서 IBM Data Server를 사용하려면 [IBM.EntityFrameworkCore NuGet 패키지](https://www.nuget.org/packages/IBM.EntityFrameworkCore)를 설치합니다.

``` powershell
Install-Package IBM.EntityFrameworkCore
```

Linux에서 IBM Data Server를 사용하려면 [IBM.EntityFrameworkCore-lnx NuGet 패키지](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx)를 설치합니다.

``` powershell
Install-Package IBM.EntityFrameworkCore-lnx
```

## <a name="get-started"></a>시작

[.NET Core용 IBM.NET 공급자 시작](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/DB2DotnetCore?lang=en)

## <a name="supported-database-engines"></a>지원되는 데이터베이스 엔진

* zOS
* IBM dashDB를 포함한 LUW
* IBM I
* Informix

## <a name="supported-platforms"></a>지원되는 플랫폼

* .NET Framework(4.6부터)
* .NET Core
