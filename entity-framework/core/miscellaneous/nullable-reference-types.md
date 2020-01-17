---
title: Nullable 참조 형식 사용-EF Core
author: roji
ms.date: 09/09/2019
ms.assetid: bde4e0ee-fba3-4813-a849-27049323d301
uid: core/miscellaneous/nullable-reference-types
ms.openlocfilehash: c16a475c363320cd18804a4efe78ccae1ae22f0d
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76124355"
---
# <a name="working-with-nullable-reference-types"></a>Nullable 참조 형식 사용

C#8은 null을 포함할 수 있는지 여부를 나타내는 [nullable 참조 형식](/dotnet/csharp/tutorials/nullable-reference-types)이라는 새로운 기능을 도입 하 여 참조 형식에 주석을 추가할 수 있도록 합니다. 이 기능을 처음 사용 하는 경우 C# 문서를 참조 하 여 자신에 게 친숙 하 게 만드는 것이 좋습니다.

이 페이지에서는 nullable 참조 형식에 대 한 EF Core를 소개 하 고이를 사용 하는 모범 사례에 대해 설명 합니다.

## <a name="required-and-optional-properties"></a>필수 및 선택적 속성

필수 및 선택적 속성에 대 한 기본 설명서와 nullable 참조 형식과의 상호 작용은 [필수 및 선택적 속성](xref:core/modeling/entity-properties#required-and-optional-properties) 페이지입니다. 먼저 해당 페이지를 읽어 시작 하는 것이 좋습니다.

> [!NOTE]
> 기존 프로젝트에서 nullable 참조 형식을 사용 하도록 설정 하는 경우 주의 해야 합니다. 이전에 선택적으로 구성 된 참조 형식 속성은 nullable로 명시적으로 주석 처리 되지 않는 한 이제 필수로 구성 됩니다. 관계형 데이터베이스 스키마를 관리할 때이로 인해 데이터베이스 열의 null 허용 여부를 변경 하는 마이그레이션이 생성 될 수 있습니다.

## <a name="dbcontext-and-dbset"></a>DbContext 및 DbSet

Nullable 참조 형식을 사용 하도록 설정 하면 C# 컴파일러가 null을 포함 하므로 초기화 되지 않은 null을 허용 하지 않는 속성에 대 한 경고를 내보냅니다. 결과적으로 컨텍스트에 대해 null을 허용 하지 않는 `DbSet`를 정의 하는 일반적인 방법은 이제 경고를 생성 하는 것입니다. 그러나 EF Core는 항상 DbContext 파생 형식에서 모든 `DbSet` 속성을 초기화 하므로 컴파일러에서이를 인식 하지 못하는 경우에도 null이 되지 않도록 보장 됩니다. 따라서 `DbSet` 속성을 nullable이 아닌 상태로 유지 하는 것이 좋습니다. null 검사 없이 해당 속성에 액세스할 수 있도록 하 고, null로 명시적으로 설정 합니다 (!).

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/NullableReferenceTypesContext.cs?name=Context&highlight=3-4)]

## <a name="non-nullable-properties-and-initialization"></a>Null을 허용 하지 않는 속성 및 초기화

초기화 되지 않은 null을 허용 하지 않는 참조 형식에 대 한 컴파일러 경고도 엔터티 형식에 대 한 일반 속성의 문제입니다. 위의 예제에서는 null을 허용 하지 않는 속성과 완벽 하 게 작동 하는 기능인 [생성자 바인딩을](xref:core/modeling/constructors)사용 하 여 이러한 경고를 사용 하지 않는 것을 방지 하 여 항상 초기화 되도록 합니다. 그러나 일부 시나리오에서는 생성자 바인딩이 옵션이 아닙니다. 예를 들어,를 이런 방식으로 초기화할 수 없습니다.

필요한 탐색 속성은 추가 어려움이 있습니다. 즉, 특정 보안 주체에 대 한 종속 항목이 항상 존재 하지만 프로그램의 해당 지점에 있는 요구 사항에 따라 특정 쿼리에서 로드 될 수도 있고 로드 되지 않을 수도 있습니다 ([데이터를 로드 하는 다양 한 패턴 참조](xref:core/querying/related-data)). 이와 동시에 이러한 속성을 nullable로 설정 하는 것은 바람직하지 않습니다. 이러한 속성은 필요한 경우에도 모든 액세스에서 null을 확인 하도록 합니다.

이러한 시나리오를 처리 하는 한 가지 방법은 nullable [지원 필드](xref:core/modeling/backing-field)를 사용 하는 null을 허용 하지 않는 속성을 사용 하는 것입니다.

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Order.cs?range=10-17)]

탐색 속성은 null을 허용 하지 않으므로 필요한 탐색이 구성 됩니다. 탐색이 적절히 로드 되는 동안에는 속성을 통해 종속에 액세스할 수 있습니다. 그러나 관련 엔터티를 먼저 제대로 로드 하지 않고 속성에 액세스 하는 경우 API 계약이 잘못 사용 되었으므로 InvalidOperationException이 throw 됩니다. EF는 설정 되지 않은 경우에도 값을 읽을 수 있도록 하므로 항상 지원 필드에 액세스 하도록 구성 해야 합니다. 이 작업을 수행 하는 방법에 대 한 [지원 필드](xref:core/modeling/backing-field) 에 대 한 설명서를 참조 하 고 구성이 올바른지 확인 하기 위해 `PropertyAccessMode.Field`를 지정 하는 것이 좋습니다.

Terser 대안은 null 인 경우에만 null로 속성을 초기화 하 여 null로 지정할 수 있습니다 (!).

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Order.cs?range=19)]

프로그래밍 버그가 발생 한 경우를 제외 하 고 실제 null 값은 관찰 되지 않습니다. 예를 들어 관련 엔터티를 미리 적절히 로드 하지 않고도 탐색 속성에 액세스할 수 있습니다.

> [!NOTE]
> 여러 관련 엔터티에 대 한 참조를 포함 하는 컬렉션 탐색은 항상 null을 허용 하지 않아야 합니다. 빈 컬렉션은 관련 된 엔터티가 없다는 것을 의미 하지만 목록 자체는 null 일 수 없습니다.

## <a name="navigating-and-including-nullable-relationships"></a>Nullable 관계 탐색 및 포함

선택적 관계를 처리할 때 실제 null 참조 예외가 불가능 한 컴파일러 경고가 발생할 수 있습니다. LINQ 쿼리를 변환 하 고 실행 하는 경우 선택적 관련 엔터티가 존재 하지 않을 경우이에 대 한 탐색은 throw 대신 무시 됩니다 EF Core 합니다. 그러나 컴파일러는이 EF Core 보장을 인식 하지 못하고 LINQ 쿼리가 메모리에서 실행 되는 것 처럼 LINQ to Objects를 사용 하 여 경고를 생성 합니다. 결과적으로 null-forgiving 연산자 (!)를 사용 하 여 실제 null 값이 가능 하지 않음을 컴파일러에 알려야 합니다.

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Program.cs?range=46)]

선택적인 탐색에서 여러 수준의 관계를 포함 하는 경우에도 유사한 문제가 발생 합니다.

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Program.cs?range=36-39&highlight=2)]

이 작업을 많이 수행 하 고 있는 엔터티 형식이 EF Core 쿼리에 주로 사용 되거나 단독으로 사용 되는 경우 탐색 속성을 null을 허용 하지 않는 것으로 설정 하 고 흐름 API 또는 데이터 주석을 통해 선택적으로 구성 하는 것이 좋습니다. 이렇게 하면 관계를 선택적으로 유지 하면서 모든 컴파일러 경고가 제거 됩니다. 그러나 엔터티가 EF Core 외부에서 이동 하는 경우에는 속성에 null을 허용 하지 않는 것으로 주석이 지정 되기는 하지만 null 값을 관찰할 수 있습니다.

## <a name="limitations"></a>제한 사항

* 리버스 엔지니어링은 현재 [ C# nrts (nullable reference types) 8](/dotnet/csharp/tutorials/nullable-reference-types)을 지원 하지 않습니다. EF Core C# 은 기능이 해제 된 것으로 가정 하는 코드를 항상 생성 합니다. 예를 들어 nullable 텍스트 열은 속성이 필수 인지 여부를 구성 하는 데 사용 되는 흐름 API 또는 데이터 주석을 사용 하 여 `string?`아닌 `string` 형식의 속성으로 스 캐 폴드 됩니다. 스 캐 폴드 코드를 편집 하 여 null 허용 여부 주석 C# 으로 바꿀 수 있습니다. Nullable 참조 형식에 대 한 스 캐 폴딩 지원은 [#15520](https://github.com/aspnet/EntityFrameworkCore/issues/15520)문제에 의해 추적 됩니다.
* EF Core의 공용 API 표면이 null 허용 여부에 대 한 주석 처리 되지 않았습니다 (공용 API가 "null 명확한"). NRT 기능이 설정 되어 있을 때 사용 하기 어려울 수 있습니다. 여기에는 EF Core에서 노출 하는 비동기 LINQ 연산자 (예: [FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_))가 포함 됩니다. 5\.0 릴리스를 위해이를 해결할 예정입니다.
