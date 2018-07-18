---
title: EF Core 및 EF6 - 내게 적합한 항목
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: 17c81e0d6c384c06e45f0cf4f338d4ba402788e1
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949142"
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a>EF Core 및 EF6: 내게 적합한 항목

다음 정보를 바탕으로 Entity Framework Core 및 Entity Framework 6 중에 선택할 수 있습니다.

## <a name="guidance-for-new-applications"></a>새 응용 프로그램에 대한 지침

EF Core의 모든 기능을 활용하려는 경우 새 응용 프로그램에 대한 EF Core를 사용하는 것이 좋으며 응용 프로그램은 EF Core에서 아직 구현되지 않은 기능이 필요하지 않습니다.

EF6은 .NET Framework 4.0(또는 이상 버전)이 필요하며 Windows에서만 지원되지만(즉, EF6은 현재 .NET Core에서 실행되지 않으며 다른 운영 체제에서 지원되지 않음) 해당 제약 조건이 허용되는 한 새 응용 프로그램에 여전히 유용한 선택이며 응용 프로그램은 EF6에 사용할 수 없는 EF Core의 새 기능이 필요하지 않습니다.

[기능 비교](features.md)를 검토하여 EF Core가 응용 프로그램에 적합한 선택이 될 수 있는지 확인합니다.

## <a name="guidance-for-existing-ef6-applications"></a>기존 EF6 응용 프로그램에 대한 지침

EF Core의 기본적인 변경 내용 때문에, 변경할 중요한 이유가 있는 경우에만 EF6 응용 프로그램을 EF Core로 이동하는 것이 좋습니다. 새로운 기능을 사용하기 위해 EF Core로 이동하려면 시작하기 전에 제한 사항을 알고 있어야 합니다. [기능 비교](features.md)를 검토하여 EF Core가 응용 프로그램에 적합한 선택이 될 수 있는지 확인합니다.

**EF6에서 EF Core로의 이동은 업그레이드가 아닌 포트로 이해해야 합니다.**  자세한 내용은 [EF6에서 EF Core로 포팅](porting/index.md)을 참조하세요.
