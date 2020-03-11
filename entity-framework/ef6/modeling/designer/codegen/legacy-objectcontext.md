---
title: Entity Framework Designer의 ObjectContext로 되돌리기-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 36550569-a1de-47cb-ba6d-544794ffd500
ms.openlocfilehash: 3e436f0d9cf94720be9c424b327816438d571ae8
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415432"
---
# <a name="reverting-to-objectcontext-in-entity-framework-designer"></a>Entity Framework Designer의 ObjectContext로 되돌리기
이전 버전의 Entity Framework EF Designer를 사용 하 여 만든 모델은 EntityObject에서 파생 된 ObjectContext 및 엔터티 클래스에서 파생 되는 컨텍스트를 생성 합니다.

EF 4.1부터 DbContext 및 POCO 엔터티 클래스에서 파생 되는 컨텍스트를 생성 하는 코드 생성 템플릿으로 교체 하는 것이 좋습니다.

Visual Studio 2012에서는 EF Designer를 사용 하 여 만든 모든 새 모델에 대해 기본적으로 생성 되는 DbContext 코드를 얻게 됩니다. DbContext 기반 코드 생성기로 전환 하기로 결정 하지 않으면 기존 모델은 ObjectContext 기반 코드를 계속 생성 합니다.

## <a name="reverting-back-to-objectcontext-code-generation"></a>ObjectContext 코드 생성으로 되돌리기

### <a name="1-disable-dbcontext-code-generation"></a>1. DbContext 코드 생성 사용 안 함

파생 된 DbContext 및 POCO 클래스 생성은 프로젝트의 두 .tt 파일에 의해 처리 되며, 솔루션 탐색기에서 .edmx 파일을 확장 하면 이러한 파일이 표시 됩니다. 프로젝트에서 이러한 파일을 모두 삭제 합니다.

![코드 생성 파일](~/ef6/media/codegenfiles.png)

VB.NET를 사용 하는 경우 중첩 된 파일을 보려면 **모든 파일 표시** 단추를 선택 해야 합니다.

![모든 파일 표시](~/ef6/media/showallfiles.png)

### <a name="2-re-enable-objectcontext-code-generation"></a>2. ObjectContext 코드 생성 다시 사용

EF 디자이너에서 모델을 열고 디자인 화면의 빈 섹션을 마우스 오른쪽 단추로 클릭 한 다음 **속성**을 선택 합니다.

속성 창에서 **코드 생성 전략** 을 **없음** 에서 **기본값으로**변경 합니다.

![코드 생성 전략](~/ef6/media/codegenstrategy.png)
