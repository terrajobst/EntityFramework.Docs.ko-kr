---
title: 데이터 시드-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/02/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 5c056c600f696ad1443ddb7b8c95c4b0ead06d21
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414574"
---
# <a name="data-seeding"></a>데이터 시드

데이터 시드는 초기 데이터 집합을 사용 하 여 데이터베이스를 채우는 프로세스입니다.

EF Core에서이 작업을 수행할 수 있는 몇 가지 방법이 있습니다.

* 모델 초기값 데이터
* 수동 마이그레이션 사용자 지정
* 사용자 지정 초기화 논리

## <a name="model-seed-data"></a>모델 초기값 데이터

> [!NOTE]
> 이 기능은 EF Core 2.1의 새로운 기능입니다.

EF6와는 달리, EF Core에서 시드 데이터는 모델 구성의 일부로 엔터티 형식에 연결 될 수 있습니다. 그런 다음 EF Core [마이그레이션은](xref:core/managing-schemas/migrations/index) 데이터베이스를 새 버전의 모델로 업그레이드할 때 적용 해야 하는 삽입, 업데이트 또는 삭제 작업을 자동으로 계산할 수 있습니다.

> [!NOTE]
> 마이그레이션은 필요한 상태로 시드 데이터를 가져오기 위해 수행 해야 하는 작업을 결정할 때만 모델 변경 사항을 고려 합니다. 따라서 마이그레이션 외부에서 수행 되는 데이터에 대 한 모든 변경 내용이 손실 되거나 오류가 발생할 수 있습니다.

예를 들어 `OnModelCreating`에서 `Blog`에 대 한 초기값 데이터를 구성 합니다.

[!code-csharp[BlogSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

관계가 있는 엔터티를 추가 하려면 외래 키 값을 지정 해야 합니다.

[!code-csharp[PostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

엔터티 형식에 섀도 상태의 속성이 있는 경우 익명 클래스를 사용 하 여 값을 제공할 수 있습니다.

[!code-csharp[AnonymousPostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=AnonymousPostSeed)]

소유 된 엔터티 형식은 비슷한 방식으로 시드 될 수 있습니다.

[!code-csharp[OwnedTypeSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=OwnedTypeSeed)]

자세한 컨텍스트는 [전체 샘플 프로젝트](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) 를 참조 하세요.

모델에 데이터를 추가한 후에는 [마이그레이션을](xref:core/managing-schemas/migrations/index) 사용 하 여 변경 내용을 적용 해야 합니다.

> [!TIP]
> 자동 배포의 일부로 마이그레이션을 적용 해야 하는 경우 실행 전에 미리 볼 수 있는 [SQL 스크립트를 만들](xref:core/managing-schemas/migrations/index#generate-sql-scripts) 수 있습니다.

또는 `context.Database.EnsureCreated()`를 사용 하 여 테스트 데이터베이스에 대 한 데이터를 포함 하는 새 데이터베이스를 만들거나 메모리 내 공급자나 관계 없는 데이터베이스를 사용할 수 있습니다. 데이터베이스가 이미 있는 경우 `EnsureCreated()`는 데이터베이스의 스키마 및 초기값 데이터를 업데이트 하지 않습니다. 관계형 데이터베이스의 경우 마이그레이션을 사용 하려는 경우 `EnsureCreated()`를 호출 하면 안 됩니다.

### <a name="limitations-of-model-seed-data"></a>모델 초기값 데이터의 제한 사항

이러한 유형의 초기값 데이터는 마이그레이션에 의해 관리 되며 데이터베이스에 이미 있는 데이터를 업데이트 하는 스크립트는 데이터베이스에 연결 하지 않고 생성 해야 합니다. 몇 가지 제한 사항이 있습니다.

* 기본 키 값은 일반적으로 데이터베이스에서 생성 되는 경우에도 지정 해야 합니다. 마이그레이션 간의 데이터 변경 내용을 검색 하는 데 사용 됩니다.
* 기본 키가 변경 되 면 이전에 시드 된 데이터가 제거 됩니다.

따라서이 기능은 마이그레이션 외부에서 변경 될 것으로 예상 되지 않고 데이터베이스의 다른 항목 (예: 우편 번호)에 종속 되지 않는 정적 데이터에 가장 유용 합니다.

시나리오에 다음 중 하나가 포함 된 경우 마지막 섹션에 설명 된 사용자 지정 초기화 논리를 사용 하는 것이 좋습니다.

* 테스트용 임시 데이터
* 데이터베이스 상태에 따라 달라 지는 데이터
* 대체 키를 id로 사용 하는 엔터티를 포함 하 여 데이터베이스에서 생성 될 키 값이 필요한 데이터입니다.
* 일부 암호 해시와 같이 사용자 지정 변환이 필요한 데이터 ( [값 변환](xref:core/modeling/value-conversions)에서 처리 되지 않음)
* ASP.NET Core Id 역할 및 사용자 만들기와 같은 외부 API를 호출 해야 하는 데이터

## <a name="manual-migration-customization"></a>수동 마이그레이션 사용자 지정

마이그레이션을 추가 하면 `HasData`로 지정 된 데이터에 대 한 변경 내용이 `InsertData()`, `UpdateData()`및 `DeleteData()`에 대 한 호출로 변환 됩니다. `HasData`에 대 한 몇 가지 제한 사항을 해결 하는 한 가지 방법은 대신 이러한 호출 또는 [사용자 지정 작업](xref:core/managing-schemas/migrations/operations) 을 마이그레이션에 수동으로 추가 하는 것입니다.

[!code-csharp[CustomInsert](../../../samples/core/Modeling/DataSeeding/Migrations/20181102235626_Initial.cs?name=CustomInsert)]

## <a name="custom-initialization-logic"></a>사용자 지정 초기화 논리

간단 하 고 강력한 데이터 시드를 수행 하는 방법은 기본 응용 프로그램 논리 실행을 시작 하기 전에 [`DbContext.SaveChanges()`](xref:core/saving/index) 를 사용 하는 것입니다.

[!code-csharp[Main](../../../samples/core/Modeling/DataSeeding/Program.cs?name=CustomSeeding)]

> [!WARNING]
> 여러 인스턴스가 실행 중일 때 동시성 문제가 발생 하 고 응용 프로그램에 데이터베이스 스키마를 수정할 수 있는 권한이 있어야 하므로 시드 코드가 일반적인 앱 실행의 일부가 아니어야 합니다.

배포의 제약 조건에 따라 초기화 코드는 여러 가지 방법으로 실행할 수 있습니다.

* 로컬로 초기화 앱 실행
* 초기화 루틴을 호출 하 고 초기화 앱을 사용 하지 않도록 설정 하거나 제거 하는 기본 앱을 사용 하 여 초기화 앱을 배포 합니다.

이는 일반적으로 [게시 프로필](/aspnet/core/host-and-deploy/visual-studio-publish-profiles)을 사용 하 여 자동화할 수 있습니다.
