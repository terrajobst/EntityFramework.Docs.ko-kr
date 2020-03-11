---
title: Visual Studio 릴리스-EF6
author: divega
ms.date: 07/05/2018
ms.assetid: 028FF890-4EDB-4F03-AE53-72F9C33EC92F
ms.openlocfilehash: 16bcdc6d0e7c5632d4f4c06ba285a7a666f24204
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414358"
---
# <a name="visual-studio-releases"></a>Visual Studio 릴리스

최신 버전의 .NET, NuGet 및 Entity Framework에 대 한 최신 도구를 포함 하므로 항상 최신 버전의 Visual Studio를 사용 하는 것이 좋습니다.
실제로 Entity Framework 설명서에 있는 다양 한 샘플 및 연습은 최신 버전의 Visual Studio를 사용 하 고 있다고 가정 합니다.

그러나 몇 가지 차이점을 고려해 야 하는 경우 다른 버전의 Entity Framework와 함께 이전 버전의 Visual Studio를 사용할 수 있습니다.

## <a name="visual-studio-2017-157-and-newer"></a>Visual Studio 2017 15.7 이상

- 이 버전의 Visual Studio에는 Entity Framework 도구 및 EF 6.2 런타임의 최신 릴리스가 포함 되어 있으며, 추가 설치 단계가 필요 하지 않습니다.
이러한 릴리스에 대 한 자세한 내용은 [새로운 기능](~/ef6/what-is-new/index.md) 을 참조 하세요.
- EF 도구를 사용 하 여 새 프로젝트에 Entity Framework을 추가 하면 EF 6.2 NuGet 패키지가 자동으로 추가 됩니다.
온라인에서 사용 가능한 EF NuGet 패키지를 수동으로 설치 하거나 업그레이드할 수 있습니다.
- 기본적으로이 버전의 Visual Studio에서 사용할 수 있는 SQL Server 인스턴스는 MSSQLLocalDB 라는 LocalDB 인스턴스입니다.
사용 해야 하는 연결 문자열의 서버 섹션은 "(localdb)\\MSSQLLocalDB"입니다.
코드에서 C# 연결 문자열을 지정할 때 `@` 또는 이중 백슬래시 "\\\\" 접두사가 붙은 축 자 문자열을 사용 해야 합니다.  


## <a name="visual-studio-2015-to-visual-studio-2017-156"></a>Visual Studio 2015에서 Visual Studio 2017로 15.6

- 이러한 버전의 Visual Studio에는 Entity Framework 도구 및 런타임 6.1.3 포함 되어 있습니다.
이러한 릴리스에 대 한 자세한 내용은 [이전 릴리스](~/ef6/what-is-new/past-releases.md#ef-613) 를 참조 하세요.
- EF 도구를 사용 하 여 새 프로젝트에 Entity Framework을 추가 하면 EF 6.1.3 NuGet 패키지가 자동으로 추가 됩니다.
온라인에서 사용 가능한 EF NuGet 패키지를 수동으로 설치 하거나 업그레이드할 수 있습니다.
- 기본적으로이 버전의 Visual Studio에서 사용할 수 있는 SQL Server 인스턴스는 MSSQLLocalDB 라는 LocalDB 인스턴스입니다.
사용 해야 하는 연결 문자열의 서버 섹션은 "(localdb)\\MSSQLLocalDB"입니다.
코드에서 C# 연결 문자열을 지정할 때 `@` 또는 이중 백슬래시 "\\\\" 접두사가 붙은 축 자 문자열을 사용 해야 합니다.  


## <a name="visual-studio-2013"></a>Visual Studio 2013
- 이 버전의 Visual Studio에는 및 이전 버전의 Entity Framework 도구 및 런타임이 포함 되어 있습니다.
Microsoft 다운로드 센터에서 제공 하 [는 설치 관리자](https://www.microsoft.com/download/details.aspx?id=40762) 를 사용 하 여 Entity Framework Tools 6.1.3로 업그레이드 하는 것이 좋습니다.
이러한 릴리스에 대 한 자세한 내용은 [이전 릴리스](~/ef6/what-is-new/past-releases.md#ef-613) 를 참조 하세요.
- 업그레이드 된 EF 도구를 사용 하 여 새 프로젝트에 Entity Framework을 추가 하면 EF 6.1.3 NuGet 패키지가 자동으로 추가 됩니다.
온라인에서 사용 가능한 EF NuGet 패키지를 수동으로 설치 하거나 업그레이드할 수 있습니다.
- 기본적으로이 버전의 Visual Studio에서 사용할 수 있는 SQL Server 인스턴스는 MSSQLLocalDB 라는 LocalDB 인스턴스입니다.
사용 해야 하는 연결 문자열의 서버 섹션은 "(localdb)\\MSSQLLocalDB"입니다.
코드에서 C# 연결 문자열을 지정할 때 `@` 또는 이중 백슬래시 "\\\\" 접두사가 붙은 축 자 문자열을 사용 해야 합니다.  

## <a name="visual-studio-2012"></a>Visual Studio 2012

- 이 버전의 Visual Studio에는 및 이전 버전의 Entity Framework 도구 및 런타임이 포함 되어 있습니다.
Microsoft 다운로드 센터에서 제공 하 [는 설치 관리자](https://www.microsoft.com/download/details.aspx?id=40762) 를 사용 하 여 Entity Framework Tools 6.1.3로 업그레이드 하는 것이 좋습니다.
이러한 릴리스에 대 한 자세한 내용은 [이전 릴리스](~/ef6/what-is-new/past-releases.md#ef-613) 를 참조 하세요.
- 업그레이드 된 EF 도구를 사용 하 여 새 프로젝트에 Entity Framework을 추가 하면 EF 6.1.3 NuGet 패키지가 자동으로 추가 됩니다.
온라인에서 사용 가능한 EF NuGet 패키지를 수동으로 설치 하거나 업그레이드할 수 있습니다.
- 기본적으로이 버전의 Visual Studio에서 사용할 수 있는 SQL Server 인스턴스는 v 11.0 이라는 LocalDB 인스턴스입니다.
사용 해야 하는 연결 문자열의 서버 섹션은 "(localdb)\\v 11.0"입니다.
코드에서 C# 연결 문자열을 지정할 때 `@` 또는 이중 백슬래시 "\\\\" 접두사가 붙은 축 자 문자열을 사용 해야 합니다.  

## <a name="visual-studio-2010"></a>Visual Studio 2010

- 이 버전의 Visual Studio에서 사용할 수 있는 Entity Framework Tools 버전은 Entity Framework 6 런타임과 호환 되지 않으므로 업그레이드할 수 없습니다.
- 기본적으로 Entity Framework 도구는 프로젝트에 4.0 Entity Framework를 추가 합니다.
최신 버전의 EF를 사용 하 여 응용 프로그램을 만들려면 먼저 [NuGet 패키지 관리자 확장](https://marketplace.visualstudio.com/items?itemName=NuGetTeam.NuGetPackageManager)을 설치 해야 합니다.
- 기본적으로 EF tools 버전의 모든 코드 생성은 EntityObject 및 Entity Framework 4를 기반으로 합니다.
또는 [C#](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforC) [Visual Basic](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforVBNET)에 대 한 DbContext 코드 생성 템플릿을 설치 하 여 DbContext 및 Entity Framework 5를 기반으로 코드 생성을 전환 하는 것이 좋습니다.
- NuGet 패키지 관리자 확장을 설치한 후에는 온라인으로 제공 되는 EF NuGet 패키지를 수동으로 설치 또는 업그레이드 하 고 디자이너를 필요로 하지 않는 Code First와 함께 EF6를 사용할 수 있습니다.
- 기본적으로이 버전의 Visual Studio에서 사용할 수 있는 SQL Server 인스턴스는 SQLEXPRESS 라는 SQL Server Express 됩니다.
사용 해야 하는 연결 문자열의 서버 섹션은 "입니다.\\SQLEXPRESS ".
코드에서 C# 연결 문자열을 지정할 때 `@` 또는 이중 백슬래시 "\\\\" 접두사가 붙은 축 자 문자열을 사용 해야 합니다.
