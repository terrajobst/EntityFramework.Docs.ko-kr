---
title: Entity Framework Core 도구 참조 - EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/19/2018
uid: core/miscellaneous/cli/index
ms.openlocfilehash: 9fcb452c2798a3d07e39cbcc3c34629dca4394ff
ms.sourcegitcommit: ad1bdea58ed35d0f19791044efe9f72f94189c18
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47447120"
---
# <a name="entity-framework-core-tools-reference"></a>Entity Framework Core 도구 참조

Entity Framework Core 도구는 디자인 타임 개발 작업에 도움이 됩니다. 주로 마이그레이션을 관리하고 데이터베이스의 스키마에 대한 리버스 엔지니어링을 통해 `DbContext`와 엔터티 형식을 스캐폴드하는 데 사용됩니다.

* [EF Core 패키지 관리자 콘솔 도구](powershell.md)는 Visual Studio의 [패키지 관리자 콘솔](https://docs.microsoft.com/nuget/tools/package-manager-console)에서 실행됩니다. 이러한 도구는 .NET Core와 .NET Framework 프로젝트 모두에서 작동합니다.

* [EF Core .NET CLI(명령줄 인터페이스) 도구](dotnet.md)는 플랫폼 간 [.NET Core CLI 도구](https://docs.microsoft.com/dotnet/core/tools/)의 확장입니다. 이러한 도구에는 .NET Core SDK 프로젝트(`Sdk="Microsoft.NET.Sdk"` 또는 프로젝트 파일에서 유사 항목)가 필요합니다.

두 도구 모두 동일한 기능을 제공합니다. Visual Studio에서 개발할 경우 더 통합적인 환경을 제공하는 **패키지 관리자 콘솔** 도구를 사용하는 것이 좋습니다.

## <a name="next-steps"></a>다음 단계

* [EF Core 패키지 관리자 콘솔 도구 참조](powershell.md)
* [EF Core .NET CLI 도구 참조](dotnet.md)