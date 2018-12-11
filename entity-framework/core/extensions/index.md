---
title: 도구 및 확장 - EF Core
author: ErikEJ
ms.date: 07/03/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: 67eae6cb943b974cc9cd581b8054836d2e37b1e9
ms.sourcegitcommit: a6082a2caee62029f101eb1000656966195cd6ee
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53181996"
---
# <a name="ef-core-tools--extensions"></a>EF Core 도구 및 확장

도구 및 확장은 Entity Framework Core에 대한 추가 기능을 제공합니다. 

> [!IMPORTANT]  
> 확장은 다양한 원본을 바탕으로 작성되며 Entity Framework Core 프로젝트의 일부로 유지되지 않습니다. 타사 확장을 고려할 때는 품질, 라이선싱, 호환성, 지원 등을 평가하여 요구 사항에 부합하는지 확인합니다.

## <a name="tools"></a>도구

### <a name="llblgen-pro"></a>LLBLGen Pro

LLBLGen Pro는 Entity Framework 및 Entity Framework Core 지원을 함께 제공하는 엔터티 모델링 솔루션입니다. 즉시 쿼리 작성을 시작할 수 있도록 데이터베이스를 우선 사용하거나 모델을 우선 사용하여 쉽게 엔터티 모델을 정의하고 데이터베이스에 매핑할 수 있습니다.

[웹 사이트](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a>Devart Entity Developer

Entity Developer는 ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access 및 LINQ to SQL을 위한 강력한 ORM 디자이너입니다. Model-First 및 Database-First 접근 방식을 사용하여 ORM 모델을 디자인하고, 그에 대한 C# 또는 Visual Basic .NET 코드를 생성할 수 있습니다. ORM 모델 디자인에 대한 새로운 접근 방식을 소개하며, 생산성을 높이고, 데이터베이스 응용 프로그램 개발을 용이하게 합니다.

[웹 사이트](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a>EF Core Power Tools

Visual Studio 2017+ 확장. 데이터베이스 또는 SQL Server 데이터베이스 프로젝트에서 DbContext 및 POCO 클래스를 리버스 엔지니어링하고 DbContext를 다양한 방법으로 시각화 및 검사할 수 있습니다.

[GitHub Wiki](https://github.com/ErikEJ/SqlCeToolbox/wiki/EF-Core-Power-Tools)

### <a name="entity-framework-visual-editor"></a>Entity Framework Visual Editor

Entity Framework 6, Core 2.0 및 Core 2.1 클래스의 시각적 디자인을 위한 ORM 디자이너가 추가된 Visual Studio 2017 확장입니다. 코드는 T4 템플릿을 사용하여 생성되므로 필요에 맞게 완전히 사용자 지정할 수 있습니다. 열거형 및 클래스를 색으로 구분하는 기능처럼 상속, 단방향 및 양방향 연결이 모두 지원되며 디자인의 잠재적으로 난해한 부분을 설명하기 위한 텍스트 블록이 추가됩니다.

[Marketplace](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a>CatFactory

CatFactory는 .NET Core 및 Entity Framework Core에 대한 스캐폴딩 엔진입니다. CatFactory 이면의 개념은 SQL Serve 인스턴스에서 기존 데이터베이스를 내보낸 다음, 스캐폴드 엔터티, 구성, 리포지토리 등의 데이터베이스 모델의 표현도 함께 내보냅니다.

[GitHub 리포지토리](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a>LoreSoft의 Entity Framework Core 생성기

Entity Framework Core 생성기(efg)는 `dotnet ef dbcontext scaffold`와 마찬가지로 기존 데이터베이스에서 EF Core 모델을 생성할 수 있는 .NET Core CLI 도구입니다. 그러나 안전 코드 [재생성](https://efg.loresoft.com/en/latest/regeneration/)도 지원한다는 점에서 다릅니다. 재생성은 지역 대체 또는 매핑 파일 구문 분석을 통해 수행됩니다. 또한 이 도구는 보기 모델, 유효성 검사 및 개체 매퍼 코드 생성을 지원합니다. 자세한 내용은 자습서 및 제품 설명서 링크를 참조하세요.

[자습서](http://www.loresoft.com/Generate-ASP-NET-Web-API)
[설명서](https://efg.loresoft.com/en/latest/)

## <a name="extensions"></a>확장

### <a name="microsoftentityframeworkcoreautohistory"></a>Microsoft.EntityFrameworkCore.AutoHistory

데이터 변경 기록의 자동 레코딩을 지원하기 위한 Microsoft.EntityFrameworkCore에 대한 플러그 인.

[GitHub 리포지토리](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a>Microsoft.EntityFrameworkCore.DynamicLinq

비동기 지원을 추가하기 위한 Microsoft.EntityFrameworkCore에 대한 동적 Linq 확장

 [GitHub 리포지토리](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efcorepractices"></a>EFCore.Practices

N+1 쿼리를 검사하는 작은 프레임워크를 포함한 테스트를 지원하는 API에서 몇 가지 좋은 사례나 모범 사례 캡처를 시도합니다.

[GitHub 리포지토리](https://github.com/riezebosch/efcore-practices/tree/master/src/EFCore.Practices/)

### <a name="efsecondlevelcachecore"></a>EFSecondLevelCache.Core

두 번째 수준 캐싱 라이브러리입니다. 두 번째 수준 캐싱은 쿼리 캐시입니다. EF 명령의 결과는 캐시에 저장되므로 동일한 EF 명령은 다시 데이터베이스에 대해 실행하는 대신 캐시에서 해당 데이터를 검색합니다.

[GitHub 리포지토리](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="detachedentityframework"></a>Detached.EntityFramework

전체 분리된 엔터티 그래프(해당 자식 엔터티 및 목록이 있는 엔터티)를 로드하고 저장합니다. [GraphDiff](https://github.com/refactorthis/GraphDiff/)에서 영감을 받았습니다. 또한 감사 및 페이지 매김과 같은 일부 반복 작업을 간소화하기 위해 일부 플러그 인을 추가하였습니다.

[GitHub 리포지토리](https://github.com/leonardoporro/Detached/)

### <a name="entityframeworkcoreprimarykey"></a>EntityFrameworkCore.PrimaryKey

사전과 같이 엔터티에서 기본 키(복합 키 포함)를 검색합니다.

[GitHub 리포지토리](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcorerx"></a>EntityFrameworkCore.Rx

Entity Framework 엔터티의 핫 관찰 가능 항목에 대한 반응성 확장 래퍼입니다.

[GitHub 리포지토리](https://github.com/NickStrupat/EntityFramework.Rx/)

### <a name="entityframeworkcoretriggers"></a>EntityFrameworkCore.Triggers

이벤트를 삽입, 업데이트 및 삭제하도록 엔터티에 트리거를 추가합니다. 실패 전, 실패 후 및 실패 시 각각에 대해 3개의 이벤트가 있습니다.

[GitHub 리포지토리](https://github.com/NickStrupat/EntityFramework.Triggers/)

### <a name="entityframeworkcoretypedoriginalvalues"></a>EntityFrameworkCore.TypedOriginalValues

엔터티 속성의 OriginalValue에 대한 형식화된 액세스를 가져옵니다. 단순 및 복합 속성은 지원되며, 탐색/컬렉션은 지원되지 않습니다.

[GitHub 리포지토리](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a>Geco

Geco는 복수형/단수형을 지원하는 리버스 모델 생성기 및 C# 6.0 보간된 문자열을 기반으로 하며 .Net Core에서 실행되는 편집 가능한 템플릿을 제공합니다. 또한 SQL 병합 스크립트 및 스크립트 실행기가 포함된 시드 스크립트 생성기를 제공합니다.

[GitHub 리포지토리](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a>LinqKit.Microsoft.EntityFrameworkCore

LinqKit.Microsoft.EntityFrameworkCore는 LINQ to SQL 및 EntityFrameworkCore 고급 사용자를 위한 무료 확장 집합입니다. Include(...) 및 IDbAsync를 지원합니다.

[GitHub 리포지토리](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a>NeinLinq.EntityFrameworkCore

NeinLinq.EntityFrameworkCore는 LINQ 공급자(예: .NET 함수의 부 하위 집합만 지원하는 Entity Framework) 사용, 함수 재사용, 쿼리 재작성, null-safe로 만들기, 변환 가능한 조건자 및 선택기를 사용한 동적 쿼리 빌드를 위한 유용한 확장을 제공합니다.

[GitHub 리포지토리](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a>Microsoft.EntityFrameworkCore.UnitOfWork

지원되는 분산된 트랜잭션을 사용한 리포지토리, 작업 패턴의 단위 및 여러 데이터베이스를 지원하기 위한 Microsoft.EntityFrameworkCore에 대한 플러그 인입니다.

[GitHub 리포지토리](https://github.com/Arch/UnitOfWork/)

### <a name="entityframeworklazyloading"></a>EntityFramework.LazyLoading

EF Core 1.1에 대한 지연 로드

[GitHub 리포지토리](https://github.com/darxis/EntityFramework.LazyLoading)

### <a name="efcorebulkextensions"></a>EFCore.BulkExtensions

대량 작업(삽입, 업데이트, 삭제)를 위한 EntityFrameworkCore 확장입니다.

[GitHub 리포지토리](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a>Bricelam.EntityFrameworkCore.Pluralizer

EF Core에 디자인 타임 복수화를 추가합니다.

[GitHub 리포지토리](https://github.com/bricelam/EFCore.Pluralizer)
