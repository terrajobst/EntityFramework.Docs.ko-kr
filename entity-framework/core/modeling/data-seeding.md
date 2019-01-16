---
title: 데이터 시드-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/02/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 1c450b142573368d043430f55a3144b6696a8691
ms.sourcegitcommit: b4a5ed177b86bf7f81602106dab6b4acc18dfc18
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/15/2019
ms.locfileid: "54316636"
---
# <a name="data-seeding"></a>데이터 시드

데이터 시드는 초기 데이터 집합을 사용 하 여 데이터베이스를 채우는 프로세스입니다.

여러 가지 방법으로 EF Core에서이 작업을 수행할 수 있습니다.
* 모델 시드 데이터
* 수동 마이그레이션 사용자 지정
* 사용자 지정 초기화 논리

## <a name="model-seed-data"></a>모델 시드 데이터

> [!NOTE]
> 이 기능은 EF Core 2.1의 새로운 기능입니다.

달리 ef6에서 EF Core에서 데이터 시드 수 모델 구성의 일부로 엔터티 형식과 사용 하 여 연결 합니다. 그런 다음 EF Core [마이그레이션을](xref:core/managing-schemas/migrations/index) 삽입, 업데이트 또는 삭제할 모델의 새 버전으로 데이터베이스를 업그레이드 하는 경우 적용 해야 하는 작업 항목에 자동으로 계산할 수 있습니다.

> [!NOTE]
> 마이그레이션은 원하는 상태에 시드 데이터를 가져올 수 있는 작업을 수행 하도록 결정 하는 경우 모델 변경 내용을 고려 합니다. 따라서 마이그레이션 외부에서 수행 하는 데이터 변경 사항이 있으면 손실 하거나 오류가 발생할 수 있습니다.

예를 들어에 대 한 시드 데이터를 구성 합니다이 `Blog` 에서 `OnModelCreating`:

[!code-csharp[BlogSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

외래 키 값 관계가 있는 엔터티를 추가 하려면 지정 해야 합니다.

[!code-csharp[PostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

엔터티 형식에 있는 경우 모든 속성 섀도 상태 값을 제공 하는 익명 클래스를 사용할 수 있습니다.

[!code-csharp[AnonymousPostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=AnonymousPostSeed)]

소유 된 엔터티 형식이 비슷한 방식으로 시드할 수 있습니다.

[!code-csharp[OwnedTypeSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=OwnedTypeSeed)]

참조 된 [전체 샘플 프로젝트](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) 자세한 컨텍스트.

데이터 모델에 추가 되 면 [마이그레이션을](xref:core/managing-schemas/migrations/index) 변경 내용을 적용 하려면 사용 해야 합니다.

> [!TIP]
> 자동화 된 배포의 일환으로 마이그레이션을 적용 해야 할 수 있습니다 [SQL 스크립트를 만듭니다](xref:core/managing-schemas/migrations/index#generate-sql-scripts) 실행 하기 전에 미리 볼 수 있습니다.

사용할 수 있습니다 `context.Database.EnsureCreated()` 메모리 내 공급자 또는 비-관계 데이터베이스를 사용 하는 경우 또는 예를 들어 테스트 데이터베이스에 대 한 시드 데이터를 포함 하는 새 데이터베이스를 만들려고 합니다. 해당 경우 데이터베이스가 이미 `EnsureCreated()` 데이터베이스의 스키마 나 시드 데이터를 모두 업데이트 됩니다. 관계형 데이터베이스에 대 한 호출 하지 않아야 하면 `EnsureCreated()` 마이그레이션을 사용 하려는 경우.

### <a name="limitations-of-model-seed-data"></a>모델 데이터의 제한 사항

데이터베이스에 연결 하지 않고도 생성 해야 하는 데이터베이스에 이미 있는 데이터를 업데이트 하는 스크립트와 이러한 종류의 시드 데이터 마이그레이션을 하 여 관리 됩니다. 이 인해 몇 가지 제한 사항이 있습니다.
* 기본 키 값을 데이터베이스에서 일반적으로 생성 되는 경우에 지정 해야 합니다. 마이그레이션 간에 데이터 변경 내용을 검색 하려면 사용 됩니다.
* 이전에 시드 된 데이터를 어떤 방식으로 기본 키 변경 되 면 제거 됩니다.

따라서이 기능은 정적 데이터 마이그레이션을 외부 변경 되지는 않을 종속 되지 않는 데이터베이스, 예를 들어 우편 번호의에서 다른 부분에 대 한 가장 유용 합니다.

시나리오 중 하나를 포함 하는 경우 마지막 섹션에서 설명 하는 사용자 지정 초기화 논리를 사용 하는 것이 좋습니다.
* 테스트에 대 한 임시 데이터
* 데이터베이스 상태에 따라 데이터를
* 키 값을 id로 대체 키를 사용 하는 엔터티를 포함 하 여 데이터베이스에서 생성할 수는 데이터
* 사용자 지정 변환 해야 하는 데이터 (에서 처리 되지 않은 [변환 값](xref:core/modeling/value-conversions)), 같은 일부 암호 해시
* ASP.NET Core Id 역할 및 사용자 만들기와 같은 외부 API에 대 한 호출을 필요로 하는 데이터

## <a name="manual-migration-customization"></a>수동 마이그레이션 사용자 지정

마이그레이션을 사용 하 여 지정 된 데이터는 변경 내용이 추가 될 때 `HasData` 에 대 한 호출으로 변환 됩니다 `InsertData()`하십시오 `UpdateData()`, 및 `DeleteData()`합니다. 제한 중 일부를 해결 하기는 한 가지 방법은 `HasData` 수동으로 이러한 호출을 추가 하거나 [사용자 지정 작업](xref:core/managing-schemas/migrations/operations) 마이그레이션을 위해 대신 합니다.

[!code-csharp[CustomInsert](../../../samples/core/Modeling/DataSeeding/Migrations/20181102235626_Initial.cs?name=CustomInsert)]

## <a name="custom-initialization-logic"></a>사용자 지정 초기화 논리

데이터 시드 작업을 수행 하는 간단 하 고 강력한 방법을 사용 하는 것 [ `DbContext.SaveChanges()` ](xref:core/saving/index) 논리를 주 응용 프로그램 전에 실행을 시작 합니다.

[!code-csharp[Main](../../../samples/core/Modeling/DataSeeding/Program.cs?name=CustomSeeding)]

> [!WARNING]
> 시드 코드가 여러 인스턴스가 실행 되 고 데이터베이스 스키마를 수정할 수 있는 권한을 가진 앱도 해야 하는 경우 동시성 문제를 발생할 수 있습니다이 일반 앱 실행에 포함 되어야 합니다.

배포의 제약 조건에 따라 다른 방법으로 초기화 코드를 실행할 수 있습니다.
* 초기화 앱을 로컬로 실행
* 기본 앱 초기화 루틴을 호출 하 고 사용 하지 않도록 설정 또는 초기화 앱 제거를 사용 하 여 초기화 앱을 배포 합니다.

일반적으로 사용 하 여 자동화할 수 있습니다 [게시 프로필](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/visual-studio-publish-profiles)합니다.
