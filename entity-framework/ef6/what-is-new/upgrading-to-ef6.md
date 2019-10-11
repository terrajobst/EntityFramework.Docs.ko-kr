---
title: Entity Framework 6-EF6로 업그레이드
author: divega
ms.date: 10/23/2016
ms.assetid: 29958ae5-85d3-4585-9ba6-550b8ec9393a
ms.openlocfilehash: 4395a9c117a6cf38e7fc08f11ee689d6fffa6fed
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182106"
---
# <a name="upgrading-to-entity-framework-6"></a>Entity Framework 6으로 업그레이드

이전 버전의 EF에서 코드는 NuGet 패키지에 제공 된 .NET Framework 및 대역 외 (주로 EntityFramework .dll)의 일부로 제공 되는 핵심 라이브러리 (주로 System.object) 간에 분할 되었습니다. EF6는 핵심 라이브러리의 코드를 가져와서 OOB 라이브러리에 통합 합니다. 이는 EF가 오픈 소스를 만들 수 있도록 하 고 .NET Framework에서 다른 속도로 발전 시킬 수 있도록 하기 위해 필요 했습니다. 이로 인해 이동한 형식에 대해 응용 프로그램을 다시 작성 해야 합니다.

EF 4.1 이상에서 제공 되는 DbContext을 사용 하는 응용 프로그램에 대해서는이 작업을 수행 하는 것이 간단 합니다. ObjectContext를 사용 하는 응용 프로그램의 경우에는 더 많은 작업이 필요 하지만 여전히 사용 하기 어렵습니다.

다음은 기존 응용 프로그램을 EF6로 업그레이드 하기 위해 수행 해야 하는 작업의 검사 목록입니다.

## <a name="1-install-the-ef6-nuget-package"></a>1. EF6 NuGet 패키지 설치

새 Entity Framework 6 런타임으로 업그레이드 해야 합니다.

1. 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **NuGet 패키지 관리 ...** 를 선택 합니다.  
2. **온라인** 탭에서 **entityframework** 를 선택 하 고 **설치** 를 클릭 합니다.  
   > [!NOTE]
   > 이전 버전의 EntityFramework NuGet 패키지가 설치 되 면 EF6로 업그레이드 됩니다.

또는 패키지 관리자 콘솔에서 다음 명령을 실행할 수 있습니다.

``` powershell
Install-Package EntityFramework
```

## <a name="2-ensure-that-assembly-references-to-systemdataentitydll-are-removed"></a>2. System.object에 대 한 어셈블리 참조가 제거 되었는지 확인 합니다.

EF6 NuGet 패키지를 설치 하면 프로젝트에서 System.object에 대 한 참조가 자동으로 제거 됩니다.

## <a name="3-swap-any-ef-designer-edmx-models-to-use-ef-6x-code-generation"></a>3. EF 2.x 코드 생성을 사용 하도록 EDMX (EF Designer) 모델 교환

EF Designer를 사용 하 여 모델을 만든 경우에는 EF6 호환 코드를 생성 하도록 코드 생성 템플릿을 업데이트 해야 합니다.

> [!NOTE]
> 현재는 Visual Studio 2012 및 2013에 대해 EF 6.x DbContext 생성기 템플릿만 사용할 수 있습니다.

1. 기존 코드 생성 템플릿을 삭제 합니다. 이러한 파일 이름은 일반적으로 **\<edmx_file_name\>.tt** 하 고 **\<edmx_file_name\>합니다. Context.tt** 및 솔루션 탐색기에서 edmx 파일을 중첩할 수 있습니다. 솔루션 탐색기에서 템플릿을 선택 하 고 **Del** 키를 눌러 항목을 삭제할 수 있습니다.  
   > [!NOTE]
   > 웹 사이트 프로젝트에서는 템플릿이 edmx 파일 아래에 중첩 되지 않지만 솔루션 탐색기에 함께 나열 됩니다.  

   > [!NOTE]
   > VB.NET 프로젝트에서는 중첩 된 템플릿 파일을 볼 수 있도록 ' 모든 파일 표시 '를 사용 하도록 설정 해야 합니다.
2. 적절 한 EF 6.x 코드 생성 템플릿을 추가 합니다. EF 디자이너에서 모델을 열고 디자인 화면을 마우스 오른쪽 단추로 클릭 한 다음 **코드 생성 항목 추가** ...를 선택 합니다.
    - DbContext API를 사용 하는 경우 (권장) **EF 6.X DbContext 생성기** 는 **데이터** 탭에서 사용할 수 있습니다.  
      > [!NOTE]
      > Visual Studio 2012을 사용 하는 경우이 템플릿을 사용 하려면 EF 6 도구를 설치 해야 합니다. 자세한 내용은 [Entity Framework 가져오기](~/ef6/fundamentals/install.md) 를 참조 하세요.  

    - ObjectContext API를 사용 하는 경우 **온라인** 탭을 선택 하 고 **EF 6.X. x entityobject 생성기**를 검색 해야 합니다.  
3. 코드 생성 템플릿에 사용자 지정을 적용 한 경우 업데이트 된 템플릿에 다시 적용 해야 합니다.

## <a name="4-update-namespaces-for-any-core-ef-types-being-used"></a>4. 사용 중인 모든 코어 EF 형식에 대 한 네임 스페이스 업데이트

DbContext 및 Code First 형식에 대 한 네임 스페이스는 변경 되지 않았습니다. 즉, EF 4.1 이상을 사용 하는 많은 응용 프로그램에서 아무것도 변경할 필요가 없습니다.

이전에 System.object에 있던 ObjectContext와 같은 형식이 새 네임 스페이스로 이동 되었습니다. 즉, EF6에 대해 빌드하기 위해 *using* 또는 *Import* 지시문을 업데이트 해야 할 수 있습니다.

네임 스페이스 변경 내용에 대 한 일반 규칙 System.Data.* 에서 모든 유형의 System.Data.Entity.Core.* 이동 됩니다. 즉, 단순히 **Entity. Core** 를 삽입 합니다. 시스템 데이터 이후. 예를 들어 다음과 같은 가치를 제공해야 합니다.

- System.object 예외 = >. 데이터입니다. **엔터티입니다**. EntityException  
- System.object = > 시스템 데이터입니다. **엔터티입니다**. Objects. ObjectContext  
- DataClasses. RelationshipManager = >. 데이터입니다. **엔터티입니다**. DataClasses. RelationshipManager  

이러한 형식은 대부분의 DbContext 기반 응용 프로그램에 직접 사용 되지 않으므로 *핵심* 네임 스페이스에 있습니다. System.object의 일부인 일부 형식은 여전히 DbContext 기반 응용 프로그램에 대해 자주 사용 되며 *핵심* 네임 스페이스로 이동 되지 않았습니다. 이러한 항목은 다음과 같습니다.

- EntityState = > 시스템 데이터입니다. **엔터티입니다**. EntityState  
- DataClasses는 >. m a m a. **엔터티인 DbFunctionAttribute**  
  > [!NOTE]
  > 이 클래스의 이름이 변경 되었습니다. 이전 이름을 가진 클래스가 여전히 존재 하 고 작동 하지만 이제는 사용 되지 않는 것으로 표시 되었습니다.  
- System.object는 >을 (를) 나타내는 데이터입니다. **엔터티인 함수**  
  > [!NOTE]
  > 이 클래스의 이름이 변경 되었습니다. 이전 이름을 가진 클래스가 여전히 존재 하 고 작동 하지만 이제 사용 되지 않는 것으로 표시 되었습니다.)  
- 공간 클래스 (예: DbGeography, DbGeometry)는 > System.object에서 이동 되었습니다. **엔터티입니다**. 음

> [!NOTE]
> System.object 네임 스페이스의 일부 형식은 EF 어셈블리가 아닌 System.object에 있습니다. 이러한 형식은 이동 되지 않으므로 네임 스페이스는 변경 되지 않은 상태로 유지 됩니다.
