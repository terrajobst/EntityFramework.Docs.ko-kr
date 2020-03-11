---
title: 디자인 타임 서비스-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
uid: core/miscellaneous/cli/services
ms.openlocfilehash: 57294ab41e7c251b1dafae9d573aa98676c5d939
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414208"
---
# <a name="design-time-services"></a>디자인 타임 서비스

도구에서 사용 하는 일부 서비스는 디자인 타임에만 사용 됩니다. 이러한 서비스는 앱과 함께 배포 되는 것을 방지 하기 위해 EF Core의 런타임 서비스와는 별도로 관리 됩니다. 이러한 서비스 중 하나를 재정의 하려면 (예: 마이그레이션 파일을 생성 하는 서비스) 시작 프로젝트에 `IDesignTimeServices`의 구현을 추가 합니다.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/DesignTimeServices.cs)]
