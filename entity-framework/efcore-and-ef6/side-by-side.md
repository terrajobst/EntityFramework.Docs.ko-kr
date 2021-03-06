---
title: EF6 및 EF Core - 동일한 애플리케이션에서 사용
author: ajcvickers
ms.date: 01/23/2019
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: bcf0a26535c4ec880a9ac25478c987fb683f6d26
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413866"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a>동일한 애플리케이션에서 EF Core 및 EF6 사용

NuGet 패키지를 둘 다 설치하여 동일한 애플리케이션 또는 라이브러리에서 EF Core 및 EF6를 사용할 수 있습니다.

일부 유형은 EF Core 및 EF6에서 같은 이름을 가지며 네임스페이스만 다르므로 동일한 코드 파일에서 EF Core 및 EF6를 둘 다 사용하는 것이 이해하기 어려울 수 있습니다. 네임스페이스 별칭 지시문을 사용하면 모호함을 쉽게 제거할 수 있습니다. 예들 들어 다음과 같습니다.

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

여러 EF 모델이 포함된 기존 애플리케이션을 포팅하는 경우 모델 중 일부를 선택적으로 포팅하도록 선택하고 다른 모델에는 EF6를 계속 사용할 수 있습니다.
