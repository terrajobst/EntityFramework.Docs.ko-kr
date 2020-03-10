---
title: 새로운 기능 - EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
uid: ef6/what-is-new/index
ms.openlocfilehash: 9daae787d0cec0ca536413e6263bb363ba76ff2c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413418"
---
# <a name="whats-new-in-ef6"></a>EF6의 새로운 기능

최신 기능 및 높은 안정성을 얻을 수 있도록 Entity Framework 최신 릴리스 버전을 사용하는 것이 좋습니다.
하지만 이전 버전을 사용해야 하거나 최신 시험판 릴리스의 개선된 기능을 사용해 보려는 분들도 있다는 사실을 잘 알고 있습니다.
EF 특정 버전을 설치하는 방법은 [Entity Framework 받기](~/ef6/fundamentals/install.md)를 참조하세요.

## <a name="ef-630"></a>EF 6.3.0

EF 6.3.0 런타임은 2019년 9월에 NuGet에서 출시되었습니다. 이 릴리스의 주요 목표는 EF 6을 사용하는 기존 애플리케이션을 .NET Core 3.0으로 손쉽게 마이그레이션하는 것이었습니다. 커뮤니티 역시 여러 버그 수정과 기능 향상에 기여했습니다. 자세한 내용은 각 6.3.0 [마일스톤](https://github.com/aspnet/EntityFramework6/milestones?state=closed)에서 종결된 문제를 참조하세요. 다음은 몇 가지 주목할 만한 것입니다.

- .NET Core 3.0 지원
  - 이제 EntityFramework 패키지는 .NET Framework 4.x. 외에도 .NET Standard 2.1을 대상으로 합니다.
  - 즉, EF 6.3은 플랫폼 간이며 Windows 외에도 Linux 및 macOS와 같은 다른 운영 체제에서도 지원됨을 의미합니다.
  - 마이그레이션 명령이 프로세스에서 실행되고 SDK 스타일 프로젝트에서 사용할 수 있도록 다시 작성되었습니다.
- SQL Server HierarchyId 지원.
- Roslyn 및 NuGet PackageReference와의 호환성이 향상되었습니다.
- 어셈블리로부터 마이그레이션을 사용, 추가, 스크립팅 및 적용하기 위한 `ef6.exe` 유틸리티를 추가했습니다. 이는 `migrate.exe`를 대체합니다.

### <a name="ef-designer-support"></a>EF 디자이너 지원

현재 .NET Core 또는 .NET Standard 프로젝트에서 직접 EF 디자이너를 사용하는 것은 지원되지 않습니다. 

동일한 솔루션의 .NET Core 3.0 또는 .NET Standard 2.1 프로젝트에 EDMX 파일 및 엔터티와 DbContext에 대해 생성된 클래스를 연결 파일로 추가하여 이 제한을 해결할 수 있습니다.

연결 파일은 프로젝트 파일에서 다음과 같이 표시됩니다.

``` csproj 
<ItemGroup>
  <EntityDeploy Include="..\EdmxDesignHost\Entities.edmx" Link="Model\Entities.edmx" />
  <Compile Include="..\EdmxDesignHost\Entities.Context.cs" Link="Model\Entities.Context.cs" />
  <Compile Include="..\EdmxDesignHost\Thing.cs" Link="Model\Thing.cs" />
  <Compile Include="..\EdmxDesignHost\Person.cs" Link="Model\Person.cs" />
</ItemGroup>
```

EDMX 파일은 EntityDeploy 빌드 작업과 연결됩니다. 이 작업은 EF 모델을 대상 어셈블리에 포함 리소스로 추가하거나 EDMX의 메타데이터 아티팩트 처리 설정에 따라 출력 폴더의 파일로 복사하는 특수 MSBuild 작업(현재 EF 6.3 패키지에 포함됨)입니다. 설정하는 방법에 대한 자세한 내용은 [EDMX .NET Core 샘플](https://aka.ms/EdmxDotNetCoreSample)을 참조하세요.

## <a name="past-releases"></a>이전 릴리스

[이전 릴리스](past-releases.md) 페이지에는 모든 EF 이전 버전 및 각 릴리스에 도입된 주요 기능 아카이브가 포함되어 있습니다.
