---
title: Entity Framework Designer-EF6에서에서 ObjectContext 되돌리기
author: divega
ms.date: 10/23/2016
ms.assetid: 36550569-a1de-47cb-ba6d-544794ffd500
ms.openlocfilehash: 3e436f0d9cf94720be9c424b327816438d571ae8
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45488949"
---
# <a name="reverting-to-objectcontext-in-entity-framework-designer"></a>Entity Framework Designer에서 ObjectContext 되돌리기
ObjectContext에서 파생 된 컨텍스트 및 엔터티 클래스는 EntityObject에서 파생 된 이전 버전의 Entity Framework의 EF 디자이너를 사용 하 여 만든 모델을 사용 하 여 생성 됩니다.

EF4.1부터 좋습니다 DbContext 및 POCO 엔터티 클래스에서 파생 되는 컨텍스트를 생성 하는 코드 생성 템플릿을으로 전환 합니다.

Visual Studio 2012의 EF 디자이너를 사용 하 여 만든 모든 새 모델에 대해 기본적으로 생성 하는 DbContext 코드를 가져옵니다. 기존 모델 계속 DbContext 기반 코드 생성기를 교환 하려는 경우가 아니면 ObjectContext 기반 코드를 생성 합니다.

## <a name="reverting-back-to-objectcontext-code-generation"></a>ObjectContext 코드 생성을 다시 되돌리기

### <a name="1-disable-dbcontext-code-generation"></a>1. DbContext 코드 생성을 사용 하지 않도록 설정

파생 된 DbContext 및 POCO 클래스를 생성 하면 프로젝트에서 두 개의.tt 파일에 의해 처리 됩니다, 그리고 솔루션 탐색기에서.edmx 파일을 확장 하는 경우 이러한 파일이 나타납니다. 프로젝트에서 이러한 파일은 모두를 삭제 합니다.

![코드 생성 파일](~/ef6/media/codegenfiles.png)

VB.NET를 사용 하는 경우 선택 해야 합니다는 **모든 파일 표시** 단추에 중첩 된 파일을 참조 하세요.

![모든 파일 표시](~/ef6/media/showallfiles.png)

### <a name="2-re-enable-objectcontext-code-generation"></a>2. 다시 ObjectContext 코드 생성을 사용 하도록 설정

EF 디자이너에서 모델 열기 화면 디자인의 빈 부분에서 마우스 오른쪽 단추로 **속성**합니다.

속성 창 변경에서 합니다 **Code Generation Strategy** 에서 **없음** 하 **기본**합니다.

![코드 생성 전략](~/ef6/media/codegenstrategy.png)
