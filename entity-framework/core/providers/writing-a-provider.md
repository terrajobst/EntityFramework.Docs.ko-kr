---
title: 데이터베이스 공급자-EF 코어 작성
author: anmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
ms.technology: entity-framework-core
uid: core/providers/writing-a-provider
ms.openlocfilehash: 4bddf5858ab2c6b2fd22571a20edb3f7c85e2853
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="writing-a-database-provider"></a>데이터베이스 공급자 작성

Entity Framework Core 데이터베이스 공급자를 작성 하는 방법에 대 한 정보를 참조 하십시오. [EF 핵심 공급자를 작성 하려고](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) 여 [Arthur Vickers](https://github.com/ajcvickers)합니다.

EF 핵심 코드 베이스는 오픈 소스 이며 대 한 참조로 사용할 수 있는 몇 가지 데이터베이스 공급자를 포함 합니다. https://github.com/aspnet/EntityFrameworkCore에서 소스 코드를 찾을 수 있습니다.

## <a name="the-providers-beware-label"></a>공급자 조심 레이블

공급자에 대 한 작업을 시작한 경우에 대 한 감시는 [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) 우리의 GitHub 문제와 끌어오기 요청에는 레이블. 공급자 기록기에 영향을 줄 수 있는 변경 내용을 식별이 레이블을 사용 합니다.

## <a name="suggested-naming-of-third-party-providers"></a>타사 공급자의 이름을 지정 하는 제안 된

NuGet 패키지에 대 한 다음과 같은 명명을 사용 하는 것이 좋습니다. EF 핵심 팀에서 제공 하는 패키지의 이름으로 구성 됩니다.

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

예:
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
