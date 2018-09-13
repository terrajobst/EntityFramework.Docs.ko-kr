---
title: EF6에서 EF 디자이너 모델에 대 한 엔터티 프레임 워크 런타임 버전 선택
author: divega
ms.date: 10/23/2016
ms.assetid: 7ace90a6-46f8-4f55-a88c-7cad9620085c
ms.openlocfilehash: 40ad05c1f015e6a51150d04eee8d9aa581d72c33
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45488493"
---
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a>EF 디자이너 모델에 대 한 엔터티 프레임 워크 런타임 버전 선택
> [!NOTE]
> **EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.

EF6부터 다음 화면 모델을 만들 때 대상으로 하려는 런타임 버전을 선택할 수 있도록 EF 디자이너에 추가 되었습니다. 최신 버전의 Entity Framework 프로젝트에 이미 설치 되어 있지 않으면 화면 표시 됩니다. 최신 버전이 이미 설치 되어 있는 경우 기본적으로만 사용 됩니다.

![화면](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a>대상 EF6.x

EF6 EF6 런타임 프로젝트에 추가할 사용자 버전 선택 ' 화면에서 선택할 수 있습니다. EF6를 추가한 후 현재 프로젝트에이 화면이 표시 되 중지 됩니다.

(때문에 여러 버전의 동일한 프로젝트에서 런타임 대상 할 수 없습니다)를 설치 하는 EF의 이전 버전을 이미 있는 경우 EF6은 사용할 수 없습니다. EF6 옵션 여기을 사용 하지 않는 경우 EF6에 프로젝트를 업그레이드 하려면 다음이 단계를 수행 합니다.

1.  솔루션 탐색기에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **NuGet 패키지 관리...**
2.  선택 **업데이트**
3.  선택 **EntityFramework** (원하는 버전으로 업데이트 하려는 있는지 확인)
4.  클릭 **업데이트**

 

## <a name="targeting-ef5x"></a>대상 EF5.x

EF5 EF5 런타임 프로젝트에 추가할 사용자 버전 선택 ' 화면에서 선택할 수 있습니다. EF5를 추가한 후 사용 하지 않도록 설정 하는 EF6 옵션을 사용 하 여 화면을 계속 표시 됩니다.

이미 설치 된 런타임의 EF4.x 버전이 있는 경우 표시 됩니다 해당 버전의 EF EF5 보다는 화면에 나열 합니다. 다음 단계를 사용 하 여이 이런 EF5로 업그레이드할 수 있습니다.

1.  선택 **도구-&gt; 라이브러리 패키지 관리자-&gt; 패키지 관리자 콘솔**
2.  실행 **Install-package EntityFramework-버전 5.0.0**

 

## <a name="targeting-ef4x"></a>대상 EF4.x

다음 단계를 사용 하 여 프로젝트에 EF4.x runtime을 설치할 수 있습니다.

1.  선택 **도구-&gt; 라이브러리 패키지 관리자-&gt; 패키지 관리자 콘솔**
2.  실행 **Install-package EntityFramework-버전 4.3.0**
