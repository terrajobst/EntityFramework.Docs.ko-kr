---
title: 연결이 끊긴 엔터티 사용 - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 12138003-a373-4817-b1b7-724130202f5f
caps.latest.revision: 3
ms.openlocfilehash: 5419215a77b57ab3c92fb88a512510070ea23bd6
ms.sourcegitcommit: 45494121254ad4fdcec613d1dd22d850068d6f39
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37913446"
---
# <a name="working-with-disconnected-entities"></a>연결이 끊긴 엔터티 사용
Entity Framework 기반 응용 프로그램에서 컨텍스트 클래스는 추적된 엔터티에 적용된 변경 내용을 검색합니다. SaveChanges 메서드를 호출하면 컨텍스트에서 추적한 변경 내용이 데이터베이스에 유지됩니다. n 계층 응용 프로그램을 작업할 때 엔터티 개체는 일반적으로 컨텍스트와 연결이 끊어진 동안 수정되므로 변경 내용을 추적하고 변경 내용을 컨텍스트에 보고하는 방법을 결정해야 합니다. 이 토픽에서는 엔터티 연결이 끊어진 Entity Framework를 사용할 때 제공되는 여러 옵션을 알아보겠습니다.   

## <a name="web-service-frameworks"></a>웹 서비스 프레임워크

웹 서비스 기술은 일반적으로 연결이 끊어진 개별 개체의 변경 내용을 유지하는 데 사용할 수 있는 패턴을 지원합니다. 예를 들어 ASP.NET Web API는 데이터베이스 개체의 변경 내용을 유지하기 위한 EF 호출을 포함할 수 있는 컨트롤러 동작을 코딩할 수 있습니다. 사실, Visual Studio의 Web API 도구를 사용하면 Entity Framework 6 모델의 Web API 컨트롤러를 간단하게 스캐폴드할 수 있습니다. 자세한 내용은 [Entity Framework 6에 Web API 사용](https://docs.microsoft.com/en-us/aspnet/web-api/overview/data/using-web-api-with-entity-framework/)을 참조하세요.   

[WCF Data Services](https://docs.microsoft.com/dotnet/framework/data/wcf/create-a-data-service-using-an-adonet-ef-data-wcf) 및 [RIA Services](https://docs.microsoft.com/en-us/previous-versions/dotnet/wcf-ria/ee707344(v=vs.91))처럼 Entity Framework와의 통합을 제공하는 여러 가지 다른 웹 서비스 기술이 있습니다.

## <a name="low-level-ef-apis"></a>하위 수준 EF API

기존 n 계층 솔루션을 사용하지 않으려는 경우 또는 Web API 서비스의 컨트롤러 작업 내에서 발생하는 동작을 사용자 지정하려는 경우를 위해 Entity Framework는 연결이 끊긴 계층의 변경 내용을 적용할 수 있는 API를 제공합니다. 자세한 내용은 [추가, 연결 및 엔터티 상태](~/ef6/saving/change-tracking/entity-state.md)를 참조하세요.  

## <a name="self-tracking-entities"></a>자동 추적 엔터티  

EF 컨텍스트와의 연결이 끊어진 동안 엔터티의 임의 그래프 변경 내용을 추적하기란 매우 어려운 문제입니다. 이 문제를 해결하기 위한 시도 중 하나는 자동 추적 엔터티 코드 생성 템플릿이었습니다. 이 템플릿은 엔터티 자체의 상태에 따라 연결이 끊긴 계층의 변경 내용을 추적하는 논리가 포함된 엔터티 클래스를 생성합니다. 이러한 변경 내용을 컨텍스트에 적용하기 위한 확장 메서드 집합도 생성됩니다.

이 템플릿은 EF 디자이너를 사용하여 만든 모델에 사용할 수 있지만, Code First 모델에는 사용할 수 없습니다. 자세한 내용은 [자체 추적 엔터티](self-tracking-entities/index.md)를 참조하세요.  

> [!IMPORTANT]
> 자동 추적 엔터티 템플릿을 더 이상 권장하지 않습니다. 이 템플릿은 기존 응용 프로그램을 지원하는 용도로만 제공될 것입니다. 응용 프로그램에서 연결이 끊긴 엔터티 그래프를 사용해야 하는 경우 커뮤니티에서 적극적으로 개발한 자동 추적 엔터티와 비슷한 기술인 [추적 가능 엔터티](http://trackableentities.github.io/) 같은 다른 대안을 고려하거나 하위 수준 변경 내용 추적 API를 사용하여 사용자 지정 코드를 작성하는 방법을 고려해 보세요.
