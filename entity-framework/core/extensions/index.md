---
title: 도구 및 확장 - EF Core
author: ErikEJ
ms.date: 12/17/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: e3806f7161fecfe66450d3e08f97caf3d2c84cf3
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634234"
---
# <a name="ef-core-tools--extensions"></a>EF Core 도구 및 확장

이러한 도구 및 확장은 Entity Framework Core 2.1 이상에 대한 추가 기능을 제공합니다.

> [!IMPORTANT]  
> 확장은 다양한 원본을 바탕으로 작성되며 Entity Framework Core 프로젝트의 일부로 유지되지 않습니다. 타사 확장을 고려할 때는 품질, 라이선싱, 호환성, 지원 등을 평가하여 요구 사항에 적합한지를 확인해야 합니다. 특히 이전 버전의 EF Core용으로 빌드된 확장은 최신 버전에서 작동하기 전에 업데이트해야 할 수 있습니다.

## <a name="tools"></a>도구

### <a name="llblgen-pro"></a>LLBLGen Pro

LLBLGen Pro는 Entity Framework 및 Entity Framework Core 지원을 함께 제공하는 엔터티 모델링 솔루션입니다. 즉시 쿼리 작성을 시작할 수 있도록 데이터베이스를 우선 사용하거나 모델을 우선 사용하여 쉽게 엔터티 모델을 정의하고 데이터베이스에 매핑할 수 있습니다. EF Core용: 2.

[웹 사이트](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a>Devart Entity Developer

Entity Developer는 ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access 및 LINQ to SQL을 위한 강력한 ORM 디자이너입니다. 모델 우선 접근법 또는 데이터베이스 우선 접근법 및 C# 또는 Visual Basic Code 생성을 사용하여 EF Core 모델을 시각적으로 지원합니다. EF Core용: 2.

[웹 사이트](https://www.devart.com/entitydeveloper/)

### <a name="nhydrate-orm-for-entity-framework"></a>Entity Framework에 대한 ORM nHydrate

Entity Framework용 강력한 형식의 확장 가능한 클래스를 만드는 ORM입니다. 생성된 코드는 Entity Framework Core입니다. 차이가 없습니다. 이것은 EF 또는 사용자 지정 ORM을 대체하지 않습니다. 팀이 복잡한 데이터베이스 스키마를 관리할 수 있는 시각적 모델링 계층입니다. Git 같은 SCM 소프트웨어에서 잘 작동하므로 충돌을 최소화하면서 모델에 대한 다중 사용자 액세스를 허용합니다. 설치 프로그램이 모델 변경 내용을 추적하고 업그레이드 스크립트를 만듭니다. EF Core용: 3.

[Github 사이트](https://github.com/nHydrate/nHydrate)

### <a name="ef-core-power-tools"></a>EF Core Power Tools

EF Core Power Tools는 간단한 사용자 인터페이스에서 다양한 EF Core 디자인 타임 작업을 노출하는 Visual Studio의 확장 기능입니다. 여기에는 기존 데이터베이스에서 DbContext와 엔터티 클래스의 리버스 엔지니어링 및 [SQL Server Dacpac](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), 데이터베이스 마이그레이션의 관리 및 모델 시각화가 포함됩니다. EF Core용: 2, 3.

[GitHub Wiki](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a>Entity Framework Visual Editor

Entity Framework Visual Editor는 EF 6 및 EF Core 클래스의 시각적 개체 디자인을 위해 ORM 디자이너를 추가한 Visual Studio의 확장 기능입니다. 코드는 T4 템플릿을 사용하여 생성되므로 필요에 맞게 사용자 지정할 수 있습니다. 상속, 단방향 및 양방향 연결, 열거형 및 클래스를 색으로 구분하는 기능을 지원하고, 디자인의 잠재적으로 난해한 부분을 설명하기 위한 텍스트 블록을 추가합니다. EF Core용: 2.

[Marketplace](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a>CatFactory

CatFactory는 SQL Server 데이터베이스에서 DbContext 클래스, 엔터티, 매핑 구성 및 리포지토리 클래스의 생성을 자동화할 수 있는 .NET Core에 대한 스캐폴딩 엔진입니다. EF Core용: 2.

[GitHub 리포지토리](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a>LoreSoft의 Entity Framework Core 생성기

Entity Framework Core 생성기(efg)는 `dotnet ef dbcontext scaffold`와 비슷하게 기존 데이터베이스에서 EF Core 모델을 생성할 수 있는 .NET Core CLI 도구이지만 지역 교체를 통하거나 매핑 파일을 구문 분석하여 안전한 코드 [재생성](https://efg.loresoft.com/en/latest/regeneration/)도 지원합니다. 이 도구는 보기 모델, 유효성 검사 및 개체 매퍼 코드를 생성하도록 지원합니다. EF Core용: 2.

[자습서](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[설명서](https://efg.loresoft.com/en/latest/)

## <a name="extensions"></a>확장

### <a name="microsoftentityframeworkcoreautohistory"></a>Microsoft.EntityFrameworkCore.AutoHistory

기록 테이블에 EF Core에서 수행된 데이터 변경 내용을 자동으로 기록할 수 있는 플러그 인 라이브러리입니다. EF Core용: 2.

[GitHub 리포지토리](https://github.com/Arch/AutoHistory/)

### <a name="efsecondlevelcachecore"></a>EFSecondLevelCache.Core

이후에 동일한 쿼리를 실행할 경우 데이터베이스에 대한 액세스를 방지하고 캐시에서 직접 데이터를 검색할 수 있도록 두 번째 수준의 캐시에 EF Core 쿼리의 결과를 저장할 수 있는 확장 기능입니다. EF Core용: 2.

[GitHub 리포지토리](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="geco"></a>Geco

Geco(생성기 콘솔)는 .NET Core에서 실행되고 콘솔 생성을 위해 C# 보간 문자열을 사용하는 콘솔 프로젝트에 기반한 간단한 코드 생성기입니다. Geco에는 복수형, 단수형 및 편집 가능한 템플릿에 대한 지원과 함께 EF Core용 리버스 모델 생성기가 포함됩니다. 시드 데이터 스크립트 생성기, 스크립트 실행기 및 데이터베이스 클리너도 제공합니다. EF Core용: 2.

[GitHub 리포지토리](https://github.com/iQuarc/Geco)

### <a name="entityframeworkcorescaffoldinghandlebars"></a>EntityFrameworkCore.Scaffolding.Handlebars 

Handlebars 템플릿이 있는 Entity Framework Core 도구 체인을 사용하여 기존 데이터베이스에서 리버스 엔지니어링된 클래스를 사용자 지정할 수 있습니다. EF Core용: 2, 3.

[GitHub 리포지토리](https://github.com/TrackableEntities/EntityFrameworkCore.Scaffolding.Handlebars)

### <a name="neinlinqentityframeworkcore"></a>NeinLinq.EntityFrameworkCore 

NeinLinq는 함수를 재사용할 수 있도록 설정하기 위해 Entity Framework와 같은 LINQ 공급 기업을 확장하여 쿼리를 다시 작성하고, 변환 가능한 조건자 및 선택기를 사용하는 동적 쿼리를 빌드합니다. EF Core용: 2, 3.

[GitHub 리포지토리](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a>Microsoft.EntityFrameworkCore.UnitOfWork

지원되는 분산된 트랜잭션을 사용한 리포지토리, 작업 패턴의 단위 및 여러 데이터베이스를 지원하기 위한 Microsoft.EntityFrameworkCore에 대한 플러그 인입니다. EF Core용: 2.

[GitHub 리포지토리](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a>EFCore.BulkExtensions

대량 작업(삽입, 업데이트, 삭제)을 위한 EF Core 확장입니다. EF Core용: 2, 3.

[GitHub 리포지토리](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a>Bricelam.EntityFrameworkCore.Pluralizer

디자인 타임 복수화를 추가합니다. EF Core용: 2.

[GitHub 리포지토리](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="toolbeltentityframeworkcoreindexattribute"></a>Toolbelt.EntityFrameworkCore.IndexAttribute

[인덱스] 특성의 반복입니다(모델 빌드를 위한 확장 기능을 포함). EF Core용: 2, 3.

[GitHub 리포지토리](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a>EfCore.InMemoryHelpers

EF Core 메모리 내 데이터베이스 공급 기업에 대한 래퍼를 제공합니다. 관계형 공급 기업과 같이 잘 작동할 수 있도록 합니다. EF Core용: 2.

[GitHub 리포지토리](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a>EFCore.TemporalSupport

임시 지원을 구현합니다. EF Core용: 2.

[GitHub 리포지토리](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="efcoretemporaltable"></a>EfCoreTemporalTable

`AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`의 도입된 확장 메서드를 사용하여 즐겨 찾는 데이터베이스에서 임시 쿼리를 쉽게 수행합니다. EF Core용: 3.

[GitHub 리포지토리](https://github.com/glautrou/EfCoreTemporalTable)

### <a name="efcoretimetraveler"></a>EFCore.TimeTraveler

이미 정의된 EF Core 코드, 엔터티 및 매핑을 사용하여 [SQL Server 임시 기록](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel)에 대한 모든 기능을 갖춘 Entity Framework Core 쿼리를 허용합니다.  `using (TemporalQuery.AsOf(targetDateTime)) {...}`에서 코드를 래핑하여 시간을 통해 이동합니다. EF Core용: 3.

[GitHub 리포지토리](https://github.com/VantageSoftware/EFCore.TimeTraveler)


### <a name="entityframeworkcoretemporaltables"></a>EntityFrameworkCore.TemporalTables

SQL Server를 사용하여 임시 테이블을 쉽게 사용할 수 있도록 하는 Entity Framework Core에 대한 확장 라이브러리입니다. EF Core용: 2.

[GitHub 리포지토리](https://github.com/findulov/EntityFrameworkCore.TemporalTables)


### <a name="entityframeworkcorecacheable"></a>EntityFrameworkCore.Cacheable

두 번째 수준 고성능 쿼리 캐시입니다. EF Core용: 2.

[GitHub 리포지토리](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a>Entity Framework Plus

다음과 같은 기능으로 DbContext를 확장합니다. 필터, 감사, 캐싱, 향후 쿼리, 일괄 삭제, 일괄 업데이트 등을 포함합니다. EF Core용: 2, 3.

[웹 사이트](https://entityframework-plus.net/)
[GitHub 리포지토리](https://github.com/zzzprojects/EntityFramework-Plus)

### <a name="entity-framework-extensions"></a>Entity Framework Extensions

고성능 대량 작업을 통해 DbContext를 확장합니다. BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge 등이 있습니다. EF Core용: 2, 3.

[웹 사이트](https://entityframework-extensions.net/)

### <a name="expressionify"></a>Expressionify

LINQ 람다에서 확장 메서드를 호출하기 위한 지원을 추가합니다. EF Core용: 3.1

[GitHub 리포지토리](https://github.com/ClaveConsulting/Expressionify)

### <a name="xlinq"></a>XLinq

관계형 데이터에 대한 LINQ(Language-Integrated Query)입니다. C#을 사용하여 강력한 형식의 쿼리를 작성할 수 있도록 합니다. EF Core용: 3.1

- 쿼리 생성에 대한 완전한 C# 지원: 람다, 변수, 함수 등에 포함된 여러 문
- 의미상으로 SQL과 차이는 없습니다. XLinq는 SQL 문(예: `SELECT`, `FROM`, `WHERE`)을 최고 C# 메서드로 선언하고 친숙한 구문에 Intellisense, 형식 안전성 및 리팩터링을 결합합니다.

결과적으로 SQL은 API를 로컬로 노출하는 “또 다른” 단순한 클래스 라이브러리가 아니라 “언어 통합 SQL”입니다. 

[웹 사이트](http://xlinq.live/)
