---
title: 도구 및 확장 - EF Core
author: ErikEJ
ms.date: 01/07/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: 414773284df7c208b9a2acf0536fda459bdf775b
ms.sourcegitcommit: 7bde8e6ad3c4565a4638646ce04bcf5e66f7b5fd
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/07/2019
ms.locfileid: "54069232"
---
# <a name="ef-core-tools--extensions"></a>EF Core 도구 및 확장

이러한 도구 및 확장은 Entity Framework Core 2.0 이상에 대한 추가 기능을 제공합니다.

> [!IMPORTANT]  
> 확장은 다양한 원본을 바탕으로 작성되며 Entity Framework Core 프로젝트의 일부로 유지되지 않습니다. 타사 확장을 고려할 때는 품질, 라이선싱, 호환성, 지원 등을 평가하여 요구 사항에 적합한지를 확인해야 합니다.

## <a name="tools"></a>도구

### <a name="llblgen-pro"></a>LLBLGen Pro

LLBLGen Pro는 Entity Framework 및 Entity Framework Core 지원을 함께 제공하는 엔터티 모델링 솔루션입니다. 즉시 쿼리 작성을 시작할 수 있도록 데이터베이스를 우선 사용하거나 모델을 우선 사용하여 쉽게 엔터티 모델을 정의하고 데이터베이스에 매핑할 수 있습니다.

[웹 사이트](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a>Devart Entity Developer

Entity Developer는 ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access 및 LINQ to SQL을 위한 강력한 ORM 디자이너입니다. 모델 우선 접근법 또는 데이터베이스 우선 접근법 및 C# 또는 Visual Basic Code 생성을 사용하여 EF Core 모델을 시각적으로 지원합니다. 

[웹 사이트](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a>EF Core Power Tools

EF Core Power Tools는 간단한 사용자 인터페이스에서 다양한 EF Core 디자인 타임 작업을 노출하는 Visual Studio 2017의 확장 기능입니다. 여기에는 기존 데이터베이스에서 DbContext와 엔터티 클래스의 리버스 엔지니어링 및 [SQL Server Dacpac](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), 데이터베이스 마이그레이션의 관리 및 모델 시각화가 포함됩니다.

[GitHub Wiki](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a>Entity Framework Visual Editor

Entity Framework Visual Editor는 EF 6 및 EF Core 클래스의 시각적 디자인을 위해 ORM 디자이너를 추가한 Visual Studio 2017의 확장 기능입니다. 코드는 T4 템플릿을 사용하여 생성되므로 필요에 맞게 사용자 지정할 수 있습니다. 상속, 단방향 및 양방향 연결, 열거형 및 클래스를 색으로 구분하는 기능을 지원하고, 디자인의 잠재적으로 난해한 부분을 설명하기 위한 텍스트 블록을 추가합니다.

[Marketplace](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a>CatFactory

CatFactory는 SQL Server 데이터베이스에서 DbContext 클래스, 엔터티, 매핑 구성 및 리포지토리 클래스의 생성을 자동화할 수 있는 .NET Core에 대한 스캐폴딩 엔진입니다.

[GitHub 리포지토리](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a>LoreSoft의 Entity Framework Core 생성기

Entity Framework Core 생성기(efg)는 `dotnet ef dbcontext scaffold`와 비슷하게 기존 데이터베이스에서 EF Core 모델을 생성할 수 있는 .NET Core CLI 도구이지만 지역 교체를 통하거나 매핑 파일을 구문 분석하여 안전한 코드 [재생성](https://efg.loresoft.com/en/latest/regeneration/)도 지원합니다. 이 도구는 보기 모델, 유효성 검사 및 개체 매퍼 코드를 생성하도록 지원합니다. 

[자습서](http://www.loresoft.com/Generate-ASP-NET-Web-API)
[설명서](https://efg.loresoft.com/en/latest/)

## <a name="extensions"></a>확장

### <a name="microsoftentityframeworkcoreautohistory"></a>Microsoft.EntityFrameworkCore.AutoHistory

기록 테이블에 EF Core에서 수행된 데이터 변경 내용을 자동으로 기록할 수 있는 플러그 인 라이브러리입니다.

[GitHub 리포지토리](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a>Microsoft.EntityFrameworkCore.DynamicLinq

EF Core와 비동기 지원 기능을 포함하는 System.Linq.Dynamic의 .NET Core/.NET 표준 포트입니다.
코드 대신 문자열 식에서 동적으로 LINQ 쿼리를 생성하는 방법을 보여주는 Microsoft 샘플로 시작된 System.Linq.Dynamic입니다.

[GitHub 리포지토리](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efsecondlevelcachecore"></a>EFSecondLevelCache.Core

이후에 동일한 쿼리를 실행할 경우 데이터베이스에 대한 액세스를 방지하고 캐시에서 직접 데이터를 검색할 수 있도록 두 번째 수준의 캐시에 EF Core 쿼리의 결과를 저장할 수 있는 확장 기능입니다.

[GitHub 리포지토리](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="entityframeworkcoreprimarykey"></a>EntityFrameworkCore.PrimaryKey

이 라이브러리를 사용하면 사전과 같이 엔터티에서 기본 키(복합 키 포함)의 값을 검색할 수 있습니다.

[GitHub 리포지토리](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcoretypedoriginalvalues"></a>EntityFrameworkCore.TypedOriginalValues

이 라이브러리를 사용하면 엔터티 속성의 원래 값에 대한 강력한 형식의 액세스를 허용할 수 있습니다. 

[GitHub 리포지토리](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a>Geco

Geco(생성기 콘솔)는 .NET Core에서 실행되고 콘솔 생성을 위해 C# 보간 문자열을 사용하는 콘솔 프로젝트에 기반한 간단한 코드 생성기입니다. Geco에는 복수형, 단수형 및 편집 가능한 템플릿에 대한 지원과 함께 EF Core용 리버스 모델 생성기가 포함됩니다. 시드 데이터 스크립트 생성기, 스크립트 실행기 및 데이터베이스 클리너도 제공합니다.

[GitHub 리포지토리](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a>LinqKit.Microsoft.EntityFrameworkCore

LinqKit.Microsoft.EntityFrameworkCore는 EF Core와 호환 가능한 LINQKit 라이브러리의 버전입니다. LINQKit는 LINQ to SQL 및 Entity Framework 전원 사용자를 위한 일련의 체험 확장 기능입니다. 조건자 식을 동적으로 빌드하고 하위 쿼리의 식 변수를 사용하는 등 고급 기능을 사용할 수 있습니다.  

[GitHub 리포지토리](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a>NeinLinq.EntityFrameworkCore

NeinLinq는 함수를 재사용할 수 있도록 설정하기 위해 Entity Framework와 같은 LINQ 공급 기업을 확장하여 쿼리를 다시 작성하고, 변환 가능한 조건자 및 선택기를 사용하는 동적 쿼리를 빌드합니다.

[GitHub 리포지토리](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a>Microsoft.EntityFrameworkCore.UnitOfWork

지원되는 분산된 트랜잭션을 사용한 리포지토리, 작업 패턴의 단위 및 여러 데이터베이스를 지원하기 위한 Microsoft.EntityFrameworkCore에 대한 플러그 인입니다.

[GitHub 리포지토리](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a>EFCore.BulkExtensions

대량 작업(삽입, 업데이트, 삭제)을 위한 EF Core 확장입니다.

[GitHub 리포지토리](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a>Bricelam.EntityFrameworkCore.Pluralizer

EF Core에 디자인 타임 복수화를 추가합니다.

[GitHub 리포지토리](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="pomelofoundationpomeloentityframeworkcoreextensionstosql"></a>PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql

EF Core가 간단한 시나리오에서 지정된 LINQ 쿼리에 대해 생성하는 SQL 문을 가져오는 간단한 확장 메서드입니다. EF Core가 단일 LINQ 쿼리에 대해 둘 이상의 SQL 문을 생성하고 매개 변수 값에 따라 다른 SQL 문을 생성할 수 있기 때문에 ToSql 메서드는 간단한 시나리오로 제한됩니다.

[GitHub 리포지토리](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql)

### <a name="toolbeltentityframeworkcoreindexattribute"></a>Toolbelt.EntityFrameworkCore.IndexAttribute

EF Core에 대한 [인덱스] 특성의 반복입니다(모델 빌드를 위한 확장 기능을 포함).

[GitHub 리포지토리](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a>EfCore.InMemoryHelpers

EF Core 메모리 내 데이터베이스 공급 기업에 대한 래퍼를 제공합니다. 관계형 공급 기업과 같이 잘 작동할 수 있도록 합니다.

[GitHub 리포지토리](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a>EFCore.TemporalSupport

EF Core에 대한 임시 지원을 구현합니다.

[GitHub 리포지토리](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="entityframeworkcorecacheable"></a>EntityFrameworkCore.Cacheable

EF Core에 대한 두 번째 수준 고성능 쿼리 캐시입니다.

[GitHub 리포지토리](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)
