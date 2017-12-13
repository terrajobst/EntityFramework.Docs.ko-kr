---
title: "EF Core 및 EF6 - 내게 적합한 항목"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: 9a113e0965fa75a03510199fb75165f6e9be0bbd
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a>EF Core 및 EF6: 내게 적합한 항목

다음 정보를 바탕으로 Entity Framework Core 및 Entity Framework 6 중에 선택할 수 있습니다.

## <a name="guidance-for-new-applications"></a>새 응용 프로그램에 대한 지침

EF Core가 새로운 제품이지만 일부 중요한 O/RM 기능이 없기 때문에 EF6는 대부분의 응용 프로그램에 가장 적합한 선택입니다.

**다음 응용 프로그램 유형에는 EF Core를 사용하는 것이 좋습니다.**

* EF Core에 구현되지 않은 기능이 필요하지 않은 새로운 응용 프로그램입니다. [기능 비교](features.md)를 검토하여 EF Core가 응용 프로그램에 적합한 선택이 될 수 있는지 확인합니다.

* UWP(유니버설 Windows 플랫폼) 및 ASP.NET Core 응용 프로그램과 같은 .NET Core를 대상으로 하는 응용 프로그램입니다. 이러한 응용 프로그램에는 .NET Framework(.NET Framework 4.5)가 필요하므로 EF6를 사용할 수 없습니다.

## <a name="guidance-for-existing-ef6-applications"></a>기존 EF6 응용 프로그램에 대한 지침

EF Core의 기본적인 변경 내용 때문에, 변경할 중요한 이유가 있는 경우에만 EF6 응용 프로그램을 EF Core로 이동하는 것이 좋습니다. 새로운 기능을 사용하기 위해 EF Core로 이동하려면 시작하기 전에 제한 사항을 알고 있어야 합니다. [기능 비교](features.md)를 검토하여 EF Core가 응용 프로그램에 적합한 선택이 될 수 있는지 확인합니다.

**EF6에서 EF Core로의 이동은 업그레이드가 아닌 포트로 이해해야 합니다.**  자세한 내용은 [EF6에서 EF Core로 포팅](porting/index.md)을 참조하세요.
