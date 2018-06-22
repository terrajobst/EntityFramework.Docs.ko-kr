---
title: EF6에서 EF 코어-EDMX 기반 모델을 사용 하는 이식로 이식
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: c999d2114c76ee3a7615ae897b42ee3376cff14e
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812692"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>EF6 EDMX 기반 모델 EF 코어로 이식

EF 코어 모델에 대 한 EDMX 파일 형식을 지원 하지 않습니다. 이러한 모델을 이식 하 응용 프로그램에 대 한 데이터베이스에서 새 코드 기반 모델을 생성 하는 것이 가장 좋습니다.

## <a name="install-ef-core-nuget-packages"></a>EF Core NuGet 패키지 설치

설치는 `Microsoft.EntityFrameworkCore.Tools` NuGet 패키지 합니다.

## <a name="regenerate-the-model"></a>모델을 다시 생성

이제 기존 데이터베이스를 기초로 모델을 만드는 리버스 엔지니어링 기능을 사용할 수 있습니다.

패키지 관리자 콘솔에서 다음 명령을 실행 (도구는 NuGet 패키지 관리자 –> 패키지 관리자 콘솔 –>). 참조 [패키지 관리자 콘솔 (Visual Studio)](../../core/miscellaneous/cli/powershell.md) 테이블 등의 하위 집합을 스 캐 폴드 명령 옵션에 대 한 합니다.

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

예를 들어 다음은 SQL Server LocalDB 인스턴스에서 블로깅 데이터베이스에서 모델을 스 캐 폴드 명령이입니다.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>EF6 모델 제거

이제 응용 프로그램에서 EF6 모델을 제거 합니다.

EF 코어 및 EF6 사용된-함께 동일한 응용 프로그램 수 그대로 EF6 NuGet 패키지 (EntityFramework)가 설치 하는 것은 합니다. 그러나 EF6 응용 프로그램의 모든 영역에서 사용 하려는 아닌, 패키지를 설치 제거 할 경우 주의 해야 하는 코드의 부분에 컴파일 오류가 발생 합니다.

## <a name="update-your-code"></a>코드 업데이트

이 시점에서 주소 지정 컴파일 오류 및 EF6 및 EF 코어 간의 동작 변경 내용이 영향을 줄 수를 참조 하는 코드를 검토 하는 것 이기 합니다.

## <a name="test-the-port"></a>포트를 테스트 합니다.

응용 프로그램 컴파일되기 때문에 EF 코어 성공적으로 이식 의미 하지 않습니다. 응용 프로그램 악영향을 동작 변경 되도록 응용 프로그램의 모든 영역을 테스트 해야 합니다.

> [!TIP]
> 참조 [EF 코어에 있는 기존 데이터베이스와 ASP.NET Core 시작](xref:core/get-started/aspnetcore/existing-db) 는 기존 데이터베이스와 함께 작동 하는 방법에 대 한 추가 참조에 대 한 
